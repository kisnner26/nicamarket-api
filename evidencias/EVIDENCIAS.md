# Evidencias de Postman

Colección: `postman/NicaMarket-JWT.postman_collection.json` (19 solicitudes, 19 pruebas pasadas al ejecutarla con newman). Los tokens de las capturas están recortados.

| # | Captura | Solicitud | Código | Por qué |
|---|---------|-----------|--------|---------|
| 1 | `1-login-admin.png` | POST /api/auth/login (admin) | 200 | Las credenciales son válidas; el servidor firma un JWT HS256 con el rol ADMIN y vencimiento de 30 minutos. |
| 2 | `2-user-consulta-categorias.png` | GET /api/categorias (USER) | 200 | El token es válido y la regla `GET /api/categorias/**` permite los roles USER y ADMIN. |
| 3 | `3-user-crea-categoria.png` | POST /api/categorias (USER) | 403 | El usuario está autenticado, pero no autorizado: POST, PUT y DELETE de categorías son solo para ADMIN. |
| 4 | `4-sin-token.png` | GET /api/categorias (sin token) | 401 | No hay identidad. El recurso exige autenticación y no llegó ningún JWT. |
| 5 | `5-admin-crea-categoria.png` | POST /api/categorias (ADMIN) | 201 | El rol ADMIN tiene permiso y el cuerpo JSON es válido, así que se crea la categoría. |

Resumen: 401 significa que no se sabe quién eres (falta token o es inválido); 403 significa que se sabe quién eres, pero tu rol no alcanza.
