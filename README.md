# TicketGenerator

Genera un ticket de compra en PDF codificado en base64.

**URL**
```
POST https://tszqoxfnvhb54zp32ytazlphgm0tnbcy.lambda-url.us-east-1.on.aws/
```

---

## Request

**Headers**

| Header | Valor |
|---|---|
| Content-Type | application/json |

**Body**

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `nombre_negocio` | string | ✅ | Nombre del negocio que aparece en el ticket |
| `atiende` | string | ✅ | Nombre del empleado que atiende |
| `cliente` | string | ✅ | Nombre del cliente |
| `ticket` | string | ✅ | Número o identificador del ticket |
| `total_compra` | number | ✅ | Total a cobrar (se muestra tal cual, sin recalcular) |
| `productos` | array | ✅ | Lista de productos (ver estructura abajo) |
| `moneda` | string | ❌ | Código de moneda. Default: `"MXN"` |
| `web_url` | string | ❌ | URL que aparece en el pie del ticket |
| `puntos_compra` | integer | ❌ | Puntos generados en esta compra |
| `puntos_totales` | integer | ❌ | Puntos acumulados del cliente |
| `logo_base64` | string | ❌ | Logo del negocio en base64 (PNG o JPG). Si se omite, se busca en `/media/logo.png` dentro del ZIP de la Lambda |
| `logo_path` | string | ❌ | Ruta alternativa al logo en disco |

**Estructura de `productos`**

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `cant` | integer | ✅ | Cantidad de unidades |
| `producto` | string | ✅ | Nombre del producto |
| `precio` | number | ❌ | Precio unitario. Si se incluye, se muestra el subtotal por línea |

---

## Response

```json
{
  "pdf_base64": "<string>",
  "phrase_used": "<string>"
}
```

| Campo | Descripción |
|---|---|
| `pdf_base64` | PDF del ticket codificado en base64 |
| `phrase_used` | Frase motivacional incluida en el ticket (obtenida de `positive-api.online`) |

---

## Ejemplo

**Request**
```bash
curl --location 'https://tszqoxfnvhb54zp32ytazlphgm0tnbcy.lambda-url.us-east-1.on.aws/' \
--header 'Content-Type: application/json' \
--data '{
  "nombre_negocio": "Tienda Don Pepe",
  "atiende": "Carlos",
  "cliente": "Juan Garcia",
  "ticket": "00142",
  "moneda": "MXN",
  "web_url": "www.donpepe.mx",
  "puntos_compra": 15,
  "puntos_totales": 340,
  "total_compra": 153.00,
  "productos": [
    { "cant": 2, "producto": "Coca-Cola 600ml",     "precio": 18.50 },
    { "cant": 1, "producto": "Pan Bimbo Integral",  "precio": 55.00 },
    { "cant": 3, "producto": "Agua Ciel 1L",        "precio": 12.00 },
    { "cant": 1, "producto": "Sabritas 170g",       "precio": 19.00 }
  ]
}'
```

**Response**
```json
{
  "pdf_base64": "JVBERi0xLjQKJ...",
  "phrase_used": "Cada acto de bondad tiene su recompensa."
}
```

Para decodificar el PDF desde el base64:
```bash
echo "JVBERi0xLjQKJ..." | base64 --decode > ticket.pdf
```

---

## Notas

- El `total_compra` se muestra tal como se recibe; la Lambda **no recalcula** el total a partir de los precios de los productos.
- Si `precio` se omite en todos los productos, no se renderiza la columna de precios por línea.
- La altura del ticket es dinámica: crece según el número de productos.
- La frase motivacional tiene fallback a `"Gracias por elegirnos hoy."` si la API externa no responde.
