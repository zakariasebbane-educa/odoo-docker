# 🧩 Odoo 17 + PostgreSQL + pgAdmin con Docker Compose

Este entorno de desarrollo levanta una instancia de **Odoo 17** con su base de datos **PostgreSQL 15** y una interfaz de administración **pgAdmin4**, todo gestionado con **Docker Compose**.  
Incluye además un script de inicialización para poblar datos de demostración automáticamente tras la creación de la base.

---

## 🚀 Servicios Incluidos

### 🗄️ Base de Datos (PostgreSQL)
- **Imagen:** `postgres:15`
- **Contenedor:** `odoo-db`
- **Puerto:** `5432`
- **Variables de entorno:**
  ```yaml
  POSTGRES_DB: postgres
  POSTGRES_USER: odoo
  POSTGRES_PASSWORD: odoo
  ```
- **Volumen persistente:** `odoo-db-data:/var/lib/postgresql/data`
- **Healthcheck:** comprueba que la base esté lista antes de iniciar Odoo.

---

### 🧠 Odoo 17
- **Imagen:** `odoo:17`
- **Contenedor:** `odoo17`
- **Puerto:** `8069` (interfaz web)
- **Variables de entorno:**
  ```yaml
  HOST: db
  USER: odoo
  PASSWORD: odoo
  DATABASE: demo_consulting
  WITHOUT_DEMO: 1
  ```
- **Volúmenes:**
  - `./odoo-data:/var/lib/odoo` → datos persistentes de Odoo  
  - `./populate_odoo_demo.py:/usr/local/bin/populate_odoo_demo.py` → script de datos demo  
- **Inicialización automática:**
  1. Crea la base de datos `demo_consulting` (puedes cambiarla).
  2. Instala los módulos `base` y `sales`.
  3. Ejecuta el script `populate_odoo_demo.py` para agregar datos de ejemplo.
  4. Inicia Odoo en modo desarrollo (`--dev all`).

---

### 🧰 pgAdmin 4
- **Imagen:** `dpage/pgadmin4`
- **Contenedor:** `pgadmin4`
- **Puerto:** `5050` → accede en [http://localhost:5050](http://localhost:5050)
- **Variables de entorno:**
  ```yaml
  PGADMIN_DEFAULT_EMAIL: admin@pgadmin.com
  PGADMIN_DEFAULT_PASSWORD: admin
  ```
- **Volumen persistente:** `pgadmin-data:/var/lib/pgadmin`

---

## ⚙️ Cómo usar

### 1️⃣ Clonar el proyecto
```bash
git clone <URL_DEL_REPO>
cd <NOMBRE_DEL_DIRECTORIO>
```

### 2️⃣ Ejecutar el entorno
```bash
docker compose up -d
```

Esto descargará las imágenes necesarias y levantará los contenedores.

---

## 🌐 Accesos

| Servicio | URL | Usuario | Contraseña |
|-----------|-----|----------|-------------|
| **Odoo** | [http://localhost:8069](http://localhost:8069) | admin | admin |
| **pgAdmin** | [http://localhost:5050](http://localhost:5050) | admin@pgadmin.com | admin |
| **PostgreSQL** | `localhost:5432` | odoo | odoo |

---

## 🧱 Volúmenes persistentes

| Volumen | Descripción |
|----------|--------------|
| `odoo-db-data` | Datos de PostgreSQL |
| `pgadmin-data` | Configuración de pgAdmin |
| `odoo-data` | Archivos y datos de Odoo |

Puedes eliminar todos los datos con:
```bash
docker compose down -v
```

---

## 🧩 Personalización

Puedes modificar los valores marcados con comentarios `# SE PUEDE CAMBIAR` en el `docker-compose.yml`, por ejemplo:
- Nombre de la base (`POSTGRES_DB`)
- Usuario y contraseña (`POSTGRES_USER`, `POSTGRES_PASSWORD`)
- Nombre de la base de datos de Odoo (`DATABASE`)
- Credenciales de pgAdmin

> 💡 **Consejo:** Evita usar `odoo` como nombre de base de datos principal, ya que puede generar conflictos con bases internas del sistema.

---

## 🐞 Solución de problemas

- Si Odoo muestra errores de conexión, verifica que el contenedor `odoo-db` esté **healthy**:
  ```bash
  docker ps
  ```
- Para ver logs:
  ```bash
  docker compose logs -f odoo
  docker compose logs -f db
  ```
- Si necesitas reiniciar todo desde cero:
  ```bash
  docker compose down -v
  docker compose up -d --build
  ```

---

## 🧰 Estructura del Proyecto

```
.
├── docker-compose.yml
├── README.md
├── populate_odoo_demo.py
└── odoo-data/
```

---

## 📜 Licencia
Este proyecto está disponible bajo la licencia MIT.  
Puedes usarlo y modificarlo libremente.

---

✉️ **Autor:** [Tu Nombre o Empresa]  
📅 **Versión:** 1.0  
🐳 **Compatibilidad probada:** Docker Compose v3.9, Odoo 17, PostgreSQL 15, pgAdmin 4
