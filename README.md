# Compose `docker-compose.yml`

Este archivo define tres servicios principales: **Odoo**, **PostgreSQL** y **PgAdmin**. A continuación se describe cada uno de estos servicios:

---
## Comandos utilizados

- **Para modificar el archivo**: - nano docker-compose.yml
- **Para levantar el servicio**: - sudo docker compose up -d

![imagen](https://github.com/user-attachments/assets/e2ff07cd-95b8-4245-b6fc-c91c2eef4fb5)


## 1. Servicio `odoo`

- **Imagen**: `odoo:17`
  - Se usa la versión 17 de Odoo comunity.
  
- **Reinicio**: `always`
  - El contenedor se reinicia automáticamente si se detiene o falla.

- **Puertos**:
  - `"8069:8069"`: El puerto 8069 del contenedor se mapea al puerto 8069 en la máquina host. Puedes acceder a Odoo en `http://localhost:8069`.

- **Variables de entorno**:
  - `USER=daniel`: Define el usuario para el sistema Odoo.
  - `PASSWORD=daniel`: Define la contraseña para el usuario.

- **Dependencias**:
  - `depends_on: db`: Odoo depende del servicio `db`. El contenedor de Odoo se iniciará después de que el contenedor de la base de datos esté listo.

- **Redes**:
  - `networks: odoo_network`: El contenedor se conecta a la red `odoo_network` para comunicarse con otros contenedores.

---

## 2. Servicio `db`

- **Imagen**: `postgres:latest`
  - Usa la última versión de la imagen de PostgreSQL.

- **Reinicio**: `always`
  - Este contenedor también se reinicia automáticamente si se detiene o falla.

- **Variables de entorno**:
  - `POSTGRES_USER=daniel`: Define el nombre de usuario para la base de datos PostgreSQL.
  - `POSTGRES_PASSWORD=daniel`: Define la contraseña del usuario en la base de datos.
  - `POSTGRES_DB=postgres`: Define el nombre de la base de datos que se creará por defecto.
  - `PGDATA=/var/lib/post/postgresql/data/PGDATA`: Personaliza el directorio donde PostgreSQL almacenará los datos.

- **Redes**:
  - `networks: odoo_network`: El contenedor de la base de datos está conectado a la red `odoo_network`.

---

## 3. Servicio `pgadmin` (PgAdmin 4)

- **Imagen**: `dpage/pgadmin4`
  - Usa la imagen de PgAdmin 4.

- **Reinicio**: `always`
  - Este contenedor también se reinicia automáticamente si se detiene o falla.

- **Puertos**:
  - `"5433:80"`: El puerto 80 del contenedor se mapea al puerto 5433 en la máquina host. Puedes acceder a PgAdmin en `http://localhost:5433`.

- **Variables de entorno**:
  - `PGADMIN_DEFAULT_EMAIL=admin@admin.com`: Establece el correo electrónico por defecto para acceder a PgAdmin.
  - `PGADMIN_DEFAULT_PASSWORD=daniel`: Establece la contraseña por defecto para la cuenta de PgAdmin.

- **Dependencias**:
  - `depends_on: db`: PgAdmin depende del servicio `db` (PostgreSQL). PgAdmin se iniciará solo después de que el contenedor de la base de datos esté listo.

- **Redes**:
  - `networks: odoo_network`: El contenedor de PgAdmin también está conectado a la red `odoo_network`.

---

## 4. Redes

- **odoo_network**:
  - `driver: bridge`: La red está configurada como `bridge`, lo que permite la comunicación entre los contenedores dentro de la misma red sin exponerlos a la red de la máquina host.

---

## Resumen

- **Odoo**: El servicio principal de la aplicación.
- **PostgreSQL**: La base de datos que Odoo utiliza para almacenar los datos.
- **PgAdmin**: La herramienta web para gestionar la base de datos PostgreSQL.
  

# Comprobaciones  
- **Conexión con Odoo**

**Creación de la base de datos**
![imagen](https://github.com/user-attachments/assets/4541a724-0884-4ae1-b4f7-dbfdcaa6fbbc)

**Comprobación de la conexión**
![imagen](https://github.com/user-attachments/assets/cd8ab5c3-aa70-44ce-830d-5b6f512f5e6f)

- **Conexión con pgadmin**
![imagen](https://github.com/user-attachments/assets/0f736bfd-437f-4ae8-8799-d088f93016ef)

- **Datos para gestionar la conexión**
![imagen](https://github.com/user-attachments/assets/de7a8398-3e52-4eb4-b0e5-55f36ea2514f)

![imagen](https://github.com/user-attachments/assets/29d3db58-016d-493e-ae67-1b6ba81d8dd8)

- **Comprobación de que esta correctamente conectada**
![imagen](https://github.com/user-attachments/assets/4e5bb281-b7c1-4fe6-a140-da838e2b48ef)

---

# Preguntas

- **¿Que ocurre si en el ordenador local el puerto 5432 está ocupado?** 

El docker compose nos avisara de que el puerto esta ocupado y no levantara el servicio.

- **¿Y si lo estuviese el 8069?** 

Lo mismo que en el anterior saltara un error y no se levantara el servicio.

- **¿Como puedes solucionarlo?**

1 - Cambiando los puertos en el compose.

2 - Parando los servicios que estan utilizando esos puertos.
