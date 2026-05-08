# Laboratorio: Persistencia en Docker con Named Volumes

## Objetivo
Demostrar la diferencia entre contenedores efímeros y persistentes usando MariaDB.

## Comandos Ejecutados

### Paso 1 — Sin persistencia (datos volátiles)
```bash
docker run -d --name db-efimera -e MYSQL_ROOT_PASSWORD=123 mariadb:10.11
docker exec -it db-efimera mysql -uroot -p123 -e "CREATE DATABASE tienda;"
docker rm -f db-efimera
# Resultado: base de datos 'tienda' eliminada permanentemente
```

### Paso 2 — Crear Named Volume
```bash
docker volume create mi-data-db
docker volume inspect mi-data-db
```

### Paso 3 — Contenedor persistente
```bash
docker run -d --name db-persistente \
  -v mi-data-db:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=123 \
  mariadb:10.11
```

### Paso 4 — Prueba de resiliencia
```bash
# Insertar datos, destruir y recrear
docker rm -f db-persistente
docker run -d --name db-nueva -v mi-data-db:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=123 mariadb:10.11
docker exec -it db-nueva mysql -uroot -p123 \
  -e "USE tienda; SELECT * FROM productos;"
# Resultado: datos intactos ✓
```


## Conclusión
Los Named Volumes desacoplan los datos del ciclo de vida del contenedor.
Los datos en `/var/lib/docker/volumes/mi-data-db/` sobreviven cualquier `docker rm`.