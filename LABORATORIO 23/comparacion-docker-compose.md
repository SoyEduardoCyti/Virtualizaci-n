

## Tabla comparativa

| Elemento | Profesor | Mi versión |
|---|---|---|
| `version` | `'3.8'` | Omitida |
| `restart` | `always` | `unless-stopped` |
| `image` MariaDB | `mariadb:10.6` | `mariadb:10.11` |
| `container_name` | No definido | Definido |
| `ports` Node.js | No expuesto | `"3000:3000"` |
| `depends_on` | No incluido | Incluido |
| Contraseña | `uagro_segura` | `secretpassword` |
| Nombre del volumen | `datos_seguros_db` | `datos-mariadb` |
| `internal: true` en red-privada | ❌ | ✅ |


