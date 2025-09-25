# InfraDeploy

This repository allows you to launch the entire InfraGest platform using Docker Compose.

---

## Getting Started

### 1. Clone this repository:

```sh
git clone https://github.com/bunnystring/infra-deploy.git
cd infra-deploy
```

### 2. Clona cada microservicio en la carpeta correspondiente:

```sh
git clone https://github.com/bunnystring/infra-config-server.git infra-config-server
git clone https://github.com/bunnystring/infra-devices-service.git infra-devices-service
git clone https://github.com/bunnystring/infra-orders-service.git infra-orders-service
git clone https://github.com/bunnystring/infra-groups-service.git infra-groups-service
git clone https://github.com/bunnystring/infra-notifications-service.git infra-notifications-service
git clone https://github.com/bunnystring/infra-api-gateway.git infra-api-gateway
git clone https://github.com/bunnystring/infra-frontend.git infra-frontend
```

---

## Database Initialization Scripts

- Coloca todos tus scripts de inicialización de base de datos en la carpeta `initdb/`.
- Todos los archivos `.sql` dentro de esta carpeta serán ejecutados automáticamente la **primera vez** que se inicie el contenedor de base de datos (`infra-db`).

**Ejemplo de script recomendado para usuarios y permisos (colócalo en `initdb/init-users.sql`):**

```sql
CREATE DATABASE IF NOT EXISTS devices_db;
CREATE DATABASE IF NOT EXISTS orders_db;
CREATE DATABASE IF NOT EXISTS groups_db;
CREATE DATABASE IF NOT EXISTS notifications_db;

-- Usuarios para cualquier host
CREATE USER IF NOT EXISTS 'devices_user'@'%' IDENTIFIED BY 'devices_pass';
CREATE USER IF NOT EXISTS 'orders_user'@'%' IDENTIFIED BY 'orders_pass';
CREATE USER IF NOT EXISTS 'groups_user'@'%' IDENTIFIED BY 'groups_pass';
CREATE USER IF NOT EXISTS 'notifications_user'@'%' IDENTIFIED BY 'notifications_pass';

-- Usuarios para localhost
CREATE USER IF NOT EXISTS 'devices_user'@'localhost' IDENTIFIED BY 'devices_pass';
CREATE USER IF NOT EXISTS 'orders_user'@'localhost' IDENTIFIED BY 'orders_pass';
CREATE USER IF NOT EXISTS 'groups_user'@'localhost' IDENTIFIED BY 'groups_pass';
CREATE USER IF NOT EXISTS 'notifications_user'@'localhost' IDENTIFIED BY 'notifications_pass';

-- Permisos para cualquier host
GRANT ALL PRIVILEGES ON devices_db.* TO 'devices_user'@'%';
GRANT ALL PRIVILEGES ON orders_db.* TO 'orders_user'@'%';
GRANT ALL PRIVILEGES ON groups_db.* TO 'groups_user'@'%';
GRANT ALL PRIVILEGES ON notifications_db.* TO 'notifications_user'@'%';

-- Permisos para localhost
GRANT ALL PRIVILEGES ON devices_db.* TO 'devices_user'@'localhost';
GRANT ALL PRIVILEGES ON orders_db.* TO 'orders_user'@'localhost';
GRANT ALL PRIVILEGES ON groups_db.* TO 'groups_user'@'localhost';
GRANT ALL PRIVILEGES ON notifications_db.* TO 'notifications_user'@'localhost';

FLUSH PRIVILEGES;
```

---

## Start all services

```sh
docker-compose up --build
```

Esto construirá e iniciará todos los servicios y bases de datos de la plataforma.

---

## Architecture Diagrams

Los siguientes diagramas están incluidos en este repositorio para referencia:

- **Entity Relationship Diagram:**  
  `diagrama-er.drawio.png`

- **Microservices Architecture Diagram:**  
  `arquitectura-microservicios.drawio.png`

---

## Expected Structure

```
infra-deploy/
  ├── docker-compose.yml
  ├── README.md
  ├── initdb/
  ├── diagrama-er.drawio.png
  ├── arquitectura-microservicios.drawio.png
  ├── infra-config-server/
  ├── infra-devices-service/
  ├── infra-orders-service/
  ├── infra-groups-service/
  ├── infra-notifications-service/
  ├── infra-api-gateway/
  └── infra-frontend/
```

**IMPORTANTE:**  
Los nombres de las carpetas deben coincidir exactamente para que Docker Compose pueda construir correctamente las imágenes y levantar los servicios.

---

## Updating Services

Si actualizas algún microservicio, solo corre `git pull` dentro de la carpeta correspondiente y reinicia el entorno:

```sh
cd infra-devices-service
git pull
cd ..
docker-compose up --build
```

---

## Troubleshooting

- Asegúrate de que todos los repositorios estén clonados en las carpetas correctas.
- Coloca tus scripts de base de datos en la carpeta `initdb/`.
- Si quieres un inicio limpio, usa:

```sh
docker-compose down -v
```

Esto detendrá y eliminará todos los contenedores y volúmenes.

---

## Notas adicionales de configuración y solución de problemas

- Si ves errores de acceso denegado para usuarios de base de datos, asegúrate de que tu script de inicialización crea los usuarios tanto para `'%'` como para `'localhost'`.
- Para conectarte desde DBeaver o desde la terminal a MariaDB, usa los usuarios y contraseñas configurados, y prueba tanto con `localhost` como con `127.0.0.1` como host.

---
