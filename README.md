# TicketGenerator
Generador de ticket pdf en base64

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
    { "cant": 2, "producto": "Coca-Cola 600ml",                "precio": 18.50 },
    { "cant": 1, "producto": "Pan Bimbo Integral 680g",        "precio": 55.00 },
    { "cant": 3, "producto": "Agua Ciel 1L",                   "precio": 12.00 },
    { "cant": 1, "producto": "Sabritas 170g", "precio": 19.00 }
  ]
}'
