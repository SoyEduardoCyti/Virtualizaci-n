# Laboratorio 19 — Orquestación de una Stack Web Completa con Docker Compose

## Objetivo

Desplegar una stack web completa (WordPress + MariaDB) usando Docker Compose como herramienta de orquestación de contenedores, definiendo la infraestructura como código en formato YAML.

---

## Tecnologías utilizadas

| Servicio     | Imagen    |  Versión |
|---           |---        |---       |
| Base de datos| mariadb   | latest   |
| Frontend/CMS | wordpress | latest   |

---

## Arquitectura

```
Navegador (host)
      │
   :8080
      │
┌─────▼──────────────────────────┐
│         red-academica (SDN)    │
│                                │
│  ┌─────────────┐   ┌────────┐  │
│  │  wordpress  │──▶│   db   │  │
│  │   :80       │   │  :3306 │  │
│  └─────────────┘   └───┬────┘  │
│                        │       │
└────────────────────────┼───────┘
                         │
                    db_data (volume)
```

---

## Estructura del proyecto

```
lab19/
├── docker-compose.yml
└── README.md
```

---

## Comandos utilizados

### 1. Crear el entorno de trabajo
```bash
mkdir ~/lab19
cd ~/lab19
nano docker-compose.yml
```

### 2. Desplegar la stack completa
```bash
docker-compose up -d
```

### 3. Verificar estado de los contenedores
```bash
docker-compose ps
```

### 4. Ver imágenes descargadas y sus tamaños
```bash
docker-compose images
```

### 5. Ver logs de la base de datos
```bash
docker-compose logs db
```

### 6. Ver logs de WordPress
```bash
docker-compose logs wordpress
```

### 7. Verificar la red SDN creada
```bash
docker network ls
docker network inspect lab19_red-academica
```

### 8. Apagar la stack (preserva datos)
```bash
docker-compose stop
```

### 9. Volver a encender
```bash
docker-compose start
```

### 10. Borrado completo de contenedores y red
```bash
docker-compose down
```

### 11. Borrado total incluyendo volúmenes
```bash
docker-compose down -v
```

---

## Variables de entorno configuradas

| Variable | Servicio | Descripción |
|---|---|---|
| `MYSQL_ROOT_PASSWORD` | db | Contraseña root de MariaDB |
| `MYSQL_DATABASE` | db | Base de datos creada al inicio |
| `MYSQL_USER` | db | Usuario de aplicación |
| `MYSQL_PASSWORD` | db | Contraseña del usuario |
| `WORDPRESS_DB_HOST` | wordpress | Hostname de la DB (nombre del servicio) |
| `WORDPRESS_DB_USER` | wordpress | Usuario para conectar a la DB |
| `WORDPRESS_DB_PASSWORD` | wordpress | Contraseña de conexión |
| `WORDPRESS_DB_NAME` | wordpress | Nombre de la base de datos |

---

## Conceptos clave aplicados

- **Infraestructura como Código (IaC):** La arquitectura completa está descrita en un solo archivo YAML versionable.
- **DNS interno SDN:** Docker Compose crea resolución de nombres automática dentro de `red-academica`. WordPress llega a MariaDB usando el nombre `db` sin necesitar su IP.
- **Persistencia con volúmenes:** El volumen `db_data` preserva los datos de MySQL aunque el contenedor sea destruido y recreado.
- **Política de reinicio:** `restart: always` garantiza que los servicios se levanten automáticamente tras un reinicio del sistema.

---

## Resultado

Stack accesible en: `http://[IP_VM]:8080`

WordPress conectado exitosamente a MariaDB a través de la red `red-academica`.
