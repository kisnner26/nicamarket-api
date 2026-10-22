# NicaMarket API (proyecto base)

API REST de **productos** y **categorías** para la Práctica guiada n.º 9 (Servicios Web, UAM).
Ahora protegida con **Spring Security + JWT** (HS256). Sin base de datos: los datos viven en memoria
y se reinician al detener la aplicación.

- Java 21 · Spring Boot 4.1.1 (Spring Framework 7, Jakarta EE 11) · Maven
- Paquete base: `com.example.factura`

## Ejecutar

```bash
mvn spring-boot:run
```

o abrir el proyecto en IntelliJ IDEA y ejecutar `FacturaApplication`. Servidor en `http://localhost:8080`.

## Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | `/api/publico/saludo` | Mensaje de estado |
| GET | `/api/productos` | Listar productos |
| GET | `/api/productos/{id}` | Obtener un producto |
| POST | `/api/productos` | Crear producto |
| PUT | `/api/productos/{id}` | Actualizar producto |
| DELETE | `/api/productos/{id}` | Eliminar producto |
| GET | `/api/categorias` | Listar categorías |
| GET | `/api/categorias/{id}` | Obtener una categoría |
| POST | `/api/categorias` | Crear categoría |
| PUT | `/api/categorias/{id}` | Actualizar categoría |
| DELETE | `/api/categorias/{id}` | Eliminar categoría |

Datos iniciales: 3 categorías y 4 productos.

### Ejemplos de cuerpo JSON

Producto:

```json
{ "nombre": "Aceite 1 L", "precio": 62.50, "stock": 40, "categoriaId": 1 }
```

Categoría:

```json
{ "nombre": "Lácteos", "descripcion": "Leche, queso y derivados" }
```

### Códigos de respuesta

- `200` consulta/actualización correcta · `201` creado · `204` eliminado
- `400` datos inválidos (con detalle por campo)
- `404` recurso o categoría inexistente

## Estructura

```
com/example/factura/
├── FacturaApplication.java
├── controllers/   ProductoController, CategoriaController, PublicoController
├── services/      ProductoService, CategoriaService (almacén en memoria)
├── model/         Producto, Categoria
└── exception/     RecursoNoEncontradoException, ApiExceptionHandler
```

## Seguridad (rama `seguridad-jwt`)

1. Generar una clave: `openssl rand -base64 32`
2. Exportarla antes de iniciar (no se guarda en el repo):

```bash
export JWT_SECRET_BASE64=<clave>
mvn spring-boot:run
```

3. `POST /api/auth/login` con `{"username":"admin","password":"Admin123!"}` devuelve el token; enviarlo como `Authorization: Bearer <token>`.

Usuarios de laboratorio: `admin` (ADMIN) y `operador` (USER). Solo para desarrollo.

| Ruta | Anónimo | USER | ADMIN |
|------|---------|------|-------|
| `/api/publico/**`, `POST /api/auth/login` | 200 | 200 | 200 |
| `GET /api/productos`, `GET /api/categorias` (y `/{id}`) | 401 | 200 | 200 |
| `POST/PUT/DELETE` productos y categorías | 401 | 403 | permitido |
| `GET /api/auth/perfil` | 401 | usuario y roles | usuario y roles |
