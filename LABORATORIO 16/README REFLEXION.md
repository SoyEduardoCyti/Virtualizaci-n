# Reflexión
## ¿Qué pasa si borro red-negocio?

Docker no te deja borrarla si hay contenedores activos conectados a ella. Verías este error:

## Error response from daemon: network red-negocio has active endpoints

Si detiengo los contenedores primero y luego la borro, esto es lo que sucede:

Los contenedores siguen existiendo pero quedan sin red, completamente aislados entre sí y sin internet.
El DNS embebido desaparece y ya no se puede hacer ping mi-db porque ese nombre vivía dentro de la red, no en el contenedor.
Si los contenedores tenían más de una red asignada, seguirían funcionando por la otra red.
