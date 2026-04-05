# biblioteca-api

**Unidad 5 — Post-Contenido 1**  
Curso: Patrones de Diseño de Software  
Universidad de Santander (UDES) — Ingeniería de Sistemas  
Estudiante: Cristian Alonso Peñaranda Parra  
Código: 02230131010  

---

## Descripción

API REST desarrollada con **Spring Boot 3.2** que implementa el patrón **Repository** con **JPA/Hibernate** para gestionar un catálogo de libros. La aplicación demuestra la separación de responsabilidades en cuatro capas: `Entity`, `Repository`, `Service` y `Controller`, con persistencia en base de datos **H2 embebida**.

---

## Arquitectura por capas

```
Controller  →  recibe peticiones HTTP y devuelve ResponseEntity
    ↓
Service     →  lógica de negocio (validaciones, unicidad de ISBN)
    ↓
Repository  →  patrón Repository con JpaRepository (Spring Data JPA)
    ↓
Entity      →  clase Libro mapeada con JPA a la tabla "libros" en H2
```

**Paquetes:**

| Capa | Paquete | Clase/Interfaz |
|---|---|---|
| Entity | `com.universidad.patrones.model` | `Libro` |
| Repository | `com.universidad.patrones.repository` | `LibroRepository` |
| Service | `com.universidad.patrones.service` | `LibroService` |
| Controller | `com.universidad.patrones.controller` | `LibroController` |

---

## Tecnologías y dependencias

| Dependencia | Versión | Uso |
|---|---|---|
| Spring Boot | 3.2.x | Framework principal |
| Spring Web | — | Controladores REST |
| Spring Data JPA | — | Patrón Repository con Hibernate |
| H2 Database | — | Base de datos embebida en memoria |
| Lombok | — | Reducción de boilerplate (getters, setters) |
| Validation | — | Validación con `@Valid`, `@NotBlank`, `@Min` |
| Java | 17 | Lenguaje |
| Maven | 3.8+ | Gestión de dependencias y build |

---

## Requisitos previos

- Java 17 o superior instalado y en el PATH
- Maven 3.8+ (o usar el Maven Wrapper incluido: `mvnw`)
- IntelliJ IDEA Community o VS Code con extensión Spring Boot Tools

---

## Cómo ejecutar el proyecto

### Opción 1 — Desde IntelliJ IDEA

1. Abrir el proyecto como proyecto Maven (`File > Open` y seleccionar la carpeta raíz)
2. Esperar a que Maven descargue las dependencias
3. Ejecutar la clase `BibliotecaApiApplication` con el botón **Run**

### Opción 2 — Desde terminal

```bash
# Clonar el repositorio
git clone https://github.com/CristianPrnda/penaranda-post1-u5.git
cd penaranda-post1-u5

# Compilar y ejecutar
mvn spring-boot:run
```

La aplicación inicia en `http://localhost:8080`.

---

## Endpoints disponibles

Base URL: `http://localhost:8080/api/libros`

| Método | Endpoint | Descripción | Código de respuesta |
|---|---|---|---|
| GET | `/api/libros` | Listar todos los libros | 200 OK |
| GET | `/api/libros/{id}` | Obtener libro por ID | 200 OK / 404 Not Found |
| POST | `/api/libros` | Crear un nuevo libro | 201 Created |
| PUT | `/api/libros/{id}` | Actualizar un libro existente | 200 OK |
| DELETE | `/api/libros/{id}` | Eliminar un libro | 204 No Content / 404 Not Found |
| GET | `/api/libros/buscar?q={palabra}` | Buscar libros por palabra en el título | 200 OK |

---

## Ejemplos de uso (curl)

**Crear un libro:**
```bash
curl -X POST http://localhost:8080/api/libros \
  -H "Content-Type: application/json" \
  -d '{"titulo":"Clean Code","autor":"Robert C. Martin","isbn":"978-0132350884","anioPublicacion":2008,"categoria":"Ingenieria de Software"}'
```

**Listar todos los libros:**
```bash
curl http://localhost:8080/api/libros
```

**Obtener un libro por ID:**
```bash
curl http://localhost:8080/api/libros/1
```

**Buscar por palabra en el título:**
```bash
curl "http://localhost:8080/api/libros/buscar?q=Clean"
```

**Actualizar un libro:**
```bash
curl -X PUT http://localhost:8080/api/libros/1 \
  -H "Content-Type: application/json" \
  -d '{"titulo":"Clean Code","autor":"Robert C. Martin","isbn":"978-0132350884","anioPublicacion":2008,"categoria":"Programacion"}'
```

**Eliminar un libro:**
```bash
curl -X DELETE http://localhost:8080/api/libros/1
```

---

## Consola H2

Con la aplicación en ejecución, accede a la consola web de H2 en:

```
http://localhost:8080/h2-console
```

Configuración de conexión:

| Campo | Valor |
|---|---|
| JDBC URL | `jdbc:h2:mem:biblioteca_db` |
| User Name | `sa` |
| Password | *(vacío)* |

---

## Evidencia de funcionamiento

Las capturas de pantalla de los endpoints funcionando se encuentran en la carpeta [`/docs`](./docs).

| Evidencia | Descripción |
|---|---|
| `docs/post-create.png` | POST /api/libros — respuesta 201 Created |
| `docs/get-all.png` | GET /api/libros — lista de libros en JSON |
| `docs/get-by-id.png` | GET /api/libros/{id} — libro individual |
| `docs/delete.png` | DELETE /api/libros/{id} — respuesta 204 |
| `docs/h2-console.png` | Consola H2 con la tabla libros |

---

## Estructura del proyecto

```
biblioteca-api/
├── src/
│   └── main/
│       ├── java/com/universidad/patrones/
│       │   ├── BibliotecaApiApplication.java
│       │   ├── controller/
│       │   │   └── LibroController.java
│       │   ├── service/
│       │   │   └── LibroService.java
│       │   ├── repository/
│       │   │   └── LibroRepository.java
│       │   └── model/
│       │       └── Libro.java
│       └── resources/
│           └── application.properties
├── pom.xml
└── README.md
```

---

## Patrones de diseño aplicados

- **Repository Pattern** — `LibroRepository` extiende `JpaRepository`, desacoplando la lógica de acceso a datos del resto de la aplicación.
- **MVC (Model-View-Controller)** — separación clara entre la capa de datos (`model`), la lógica de negocio (`service`) y la exposición HTTP (`controller`).
- **Dependency Injection** — Spring inyecta las dependencias vía constructor en `LibroService` y `LibroController`, favoreciendo bajo acoplamiento y testabilidad.
