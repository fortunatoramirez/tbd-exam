# Examen U3 — Mini Hackatón

**Sistema Web para Préstamo de Instrumentos de Laboratorio**
(autenticación, roles, búsqueda en vivo, Excel, `.env`, `nodemon`, Bootstrap, Live Share y GitHub)

**Duración:** 1 h 40 min
**Equipos:** 5–7 personas
**Recursos permitidos:** prácticas previas, internet y **IA** (documentar qué usaron), apoyo dentro del equipo.

---

## 1) Objetivo (claro y directo)

Construir un sistema web **sencillo** que permita **gestionar el catálogo** de instrumentos y **registrar préstamos** básicos. Debe incluir **login**, **roles**, **CRUD de instrumentos**, **búsqueda en vivo** y **carga/descarga** de datos en **Excel**.

**Nota:** Aunque el tema del mini hackatón es *Préstamo de Instrumentos y Kits de Laboratorio* PUEDEN PROPONER UN TEMA DIFERENTE y deberá ser aprobado.

---

## 2) Organización (obligatoria desde el minuto 0)

1. **Live Share** en VS Code con permisos de edición (captura de evidencia).
2. **GitHub**: crear repositorio, subir código y un `README` breve.
3. Asignar **roles** y anotar responsables:

   * **Líder técnico** (coordina, timeboxing).
   * **Backend** (rutas, lógica, RBAC, Excel).
   * **Base de datos** (tablas, usuario no‑root, conexión).
   * **Frontend** (Bootstrap, vistas y fetch).
   * **DevOps/Git** (repo, `.env.example`, estructura).
   * **QA/Docs** (pruebas, capturas, PDF, Anexo‑IA).

> Pegar esta tabla en su documento y llenarla:
>
> | Bloque                          | Responsable | Estado |
> | ------------------------------- | ----------- | ------ |
> | Live Share + GitHub             |             |        |
> | BD + usuario no‑root            |             |        |
> | Login/Logout (bcrypt)           |             |        |
> | Roles (Admin/Asistente/Auditor) |             |        |
> | CRUD instrumentos               |             |        |
> | Búsqueda en vivo                |             |        |
> | Excel (subir/bajar)             |             |        |
> | UI con Bootstrap                |             |        |

---

## 3) Requisitos técnicos (mínimos)

### Dependencias

```
npm i express mysql2 dotenv bcrypt multer xlsx
npm i -D nodemon
```

### Estructura sugerida

```
/mini-hackaton-lab
├── server.js
├── package.json
├── nodemon.json
├── .env            # no subir a Git
├── .env.example    # sí subir a Git
├── /db
│   └── schema.sql
├── /uploads        # temporales xlsx
└── /public
    ├── index.html
    ├── login.html
    ├── instrumentos.html
    ├── prestamos.html (opcional)
    ├── busqueda.html
    ├── navbar.html
    └── styles.css
```

### `nodemon.json` (simple)

```json
{ "watch": ["server.js", "public"], "exec": "node server.js" }
```

### `.env.example`

```env
PORT=3000
DB_HOST=localhost
DB_USER=lab_user
DB_PASS=lab_pass
DB_NAME=laboratorio
```

---

## 4) Base de datos (simple y suficiente)

Ejecutar `db/schema.sql`:

```sql
CREATE DATABASE IF NOT EXISTS laboratorio CHARACTER SET utf8mb4;
USE laboratorio;

CREATE TABLE IF NOT EXISTS usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  correo VARCHAR(120) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  rol ENUM('ADMIN','ASISTENTE','AUDITOR') NOT NULL DEFAULT 'ASISTENTE'
);

CREATE TABLE IF NOT EXISTS instrumentos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(120) NOT NULL,
  categoria VARCHAR(80) NOT NULL,
  estado ENUM('DISPONIBLE','PRESTADO','MANTENIMIENTO') DEFAULT 'DISPONIBLE',
  ubicacion VARCHAR(120)
);

-- (Opcional) préstamos mínimos
CREATE TABLE IF NOT EXISTS prestamos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  instrumento_id INT NOT NULL,
  usuario_correo VARCHAR(120) NOT NULL,
  fecha_salida DATETIME DEFAULT CURRENT_TIMESTAMP,
  fecha_regreso DATETIME NULL,
  FOREIGN KEY (instrumento_id) REFERENCES instrumentos(id)
);
```

**Usuario no‑root:**

```sql
CREATE USER IF NOT EXISTS 'lab_user'@'localhost' IDENTIFIED BY 'lab_pass';
GRANT ALL PRIVILEGES ON laboratorio.* TO 'lab_user'@'localhost';
FLUSH PRIVILEGES;
```

> **Zona horaria (simple):** si pueden, ajusten MySQL/Node a `America/Tijuana`.

---

## 5) Funcionalidad requerida (qué debe existir)

1. **Login/Logout** con `bcrypt` (hash de contraseña).
2. **Roles**:

   * **ADMIN**: ver/crear/editar/eliminar usuarios e instrumentos.
   * **ASISTENTE**: ver y editar **instrumentos** (no borrar usuarios).
   * **AUDITOR**: **solo lectura**.
3. **CRUD de instrumentos** (nombre, categoría, estado, ubicación).
4. **Búsqueda en vivo**:

   * Campo de texto que consulta `/api/instrumentos?q=...` y muestra lista/tabla al teclear.
5. **Excel**:

   * **Subir** `.xlsx` con cabeceras: `nombre, categoria, estado, ubicacion` → inserta/actualiza.
   * **Descargar**: genera `instrumentos.xlsx` con el catálogo actual.
6. **UI con Bootstrap** (CDN): navbar simple y tablas/formularios agradables.

**Rutas mínimas:**

* `POST /api/auth/register` 
* `POST /api/auth/login` / `POST /api/auth/logout`
* `GET /api/instrumentos` 
* `POST /api/instrumentos` 
* `PUT /api/instrumentos/:id` 
* `DELETE /api/instrumentos/:id` 
* `GET /api/instrumentos/buscar?q=` 
* `POST /api/instrumentos/upload` 
* `GET /api/instrumentos/download`

> **Seguridad mínima:** consultas **parametrizadas** (con `?`), validar campos requeridos, manejar errores con mensajes simples.

---

## 6) Pasos sugeridos

* **0–10 min:** Live Share + repo GitHub + `.env.example` + `README` (cómo correr).
* **10–25 min:** BD (`schema.sql`) + usuario no‑root + conexión desde `server.js`.
* **25–45 min:** Login/Logout con `bcrypt`.
* **45–70 min:** Roles (middleware) + CRUD de instrumentos.
* **70–90 min:** Búsqueda en vivo + Excel (subir/bajar).
* **90–100 min:** Pruebas rápidas + capturas + push final.

---

## 7) Entregables

1. **PDF** con portada (título y nombres), capturas de: Live Share, login, CRUD, búsqueda, Excel, y link al repo.
2. **Repositorio GitHub** con código, `db/schema.sql`, `README`, y `.env.example` (sin credenciales).
3. **Anexo‑IA** en el PDF: prompts/herramientas usadas y cómo integraron los resultados.

---

## 8) Rúbrica (100 pts)

| Criterio                                                 |     Pts |
| -------------------------------------------------------- | ------: |
| Live Share funcional + GitHub (evidencias)               |      15 |
| Configuración básica: `.env`, `nodemon`, usuario no‑root |      10 |
| Login/Logout con `bcrypt`                                |      15 |
| Roles (ADMIN/ASISTENTE/AUDITOR) aplicados en rutas       |      15 |
| CRUD de instrumentos                                     |      20 |
| Búsqueda en vivo                                         |      10 |
| Excel (subir y descargar)                                |      10 |
| UI con Bootstrap (navbar + tablas/formularios)           |       5 |
| Documentación (PDF + Anexo‑IA)                           |      10 |
| **Total**                                                | **100** |

---

## 9) Posible plantilla de Excel (cabeceras)

```
nombre | categoria | estado | ubicacion
Multímetro | Electricidad | DISPONIBLE | Laboratorio A
Kit ECG | Biomédica | MANTENIMIENTO | Gabinete 3
Pipetas P1000 | Química | PRESTADO | —
```

---

## 10) Tips rápidos

* Conviertan primero un **insert** manual en instrumentos y verifiquen la lista.
* En frontend, usen **fetch** y actualicen la lista sin recargar página.
* Guarden el token de sesión (simple) en `localStorage` y envíenlo en headers.
* Si falta tiempo, prioricen **Login + Roles + CRUD + Búsqueda**; Excel puede quedar básico.
