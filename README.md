# NicaMarket API (proyecto base)

API REST de **productos** y **categorías** para la Práctica guiada n.º 9 (Servicios Web, UAM).
Es el punto de partida **sin seguridad** y **sin base de datos**: los datos viven en memoria
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
