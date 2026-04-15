# Lab 16 - Docker Networking y DNS

## Comandos utilizados

# Instalar VMware Tools
sudo apt install -y open-vm-tools open-vm-tools-desktop
# Mejora la integración entre la VM y Windows: copiar/pegar,
# pantalla completa y mejor rendimiento general.

# Actualizar el sistema
sudo apt update && sudo apt upgrade -y
# Actualiza la lista de paquetes y aplica todas las
# actualizaciones disponibles antes de instalar Docker.

# Instalar dependencias previas
sudo apt install -y ca-certificates curl gnupg lsb-release
# Instala herramientas necesarias para descargar y verificar
# paquetes de forma segura desde repositorios externos.

# Crear directorio para claves GPG
sudo install -m 0755 -d /etc/apt/keyrings
# Crea el directorio estándar donde se guardan las claves
# de confianza de repositorios externos en Ubuntu moderno.

# Descargar y convertir la clave GPG de Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
# Descarga la clave oficial de Docker y la convierte al
# formato binario que requiere apt para verificar paquetes.

# Asignar permisos a la clave GPG
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# Da permisos de lectura a todos los usuarios para que
# apt pueda autenticar los paquetes de Docker.

# Registrar el repositorio oficial de Docker
echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Agrega el repositorio de Docker al sistema, detectando
# automáticamente la arquitectura y versión de Ubuntu.

# Actualizar repositorios con la nueva fuente
sudo apt update
# Descarga el índice de paquetes del repositorio de Docker
# recién agregado. Confirma que download.docker.com aparece.

# Instalar Docker y sus componentes
sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin
# Instala el motor principal, CLI, runtime de contenedores,
# plugin para imágenes multi-plataforma y Docker Compose.

# Verificar la instalación
sudo docker run hello-world
# Prueba que todos los componentes funcionan correctamente.
# Debe mostrar el mensaje "Hello from Docker!".

# Usar Docker sin sudo
sudo usermod -aG docker $USER
# Agrega el usuario al grupo docker para no necesitar sudo
# en cada comando. Requiere cerrar y abrir sesión.

# Activar Docker al inicio del sistema
sudo systemctl enable docker
sudo systemctl start docker
# Registra Docker para arranque automático e inicia
# el servicio inmediatamente sin reiniciar.

# ── LABORATORIO DE NETWORKING ──────────────────────────

# Crear la red personalizada
docker network create red-negocio
# Crea una red bridge definida por el usuario. A diferencia
# de la bridge por defecto, habilita el DNS interno entre
# contenedores.

# Listar las redes disponibles
docker network ls
# Muestra todas las redes. Confirma que red-negocio aparece
# con driver bridge y scope local.

# Desplegar la base de datos MariaDB
docker run -d --name mi-db \
  --network red-negocio \
  -e MYSQL_ROOT_PASSWORD=secreto \
  mariadb:latest
# Lanza MariaDB en segundo plano, conectada a red-negocio
# y con la contraseña de root definida por variable de entorno.

# Verificar que el contenedor está corriendo
docker ps
# Muestra los contenedores activos. Confirma que mi-db
# aparece con status Up y puerto 3306 expuesto.

# Inspeccionar la red y sus contenedores conectados
docker network inspect red-negocio
# Muestra la configuración completa en JSON: subred
# 172.18.0.0/16, gateway 172.18.0.1 y el contenedor
# mi-db con su IP asignada en la sección "Containers".

# Desplegar el frontend Alpine y entrar al contenedor
docker run -it --rm --network red-negocio alpine ash
# Lanza Alpine en modo interactivo, conectado a red-negocio.
# Con --rm se elimina automáticamente al salir.

# Dentro del contenedor Alpine — probar DNS interno
ping mi-db
# Docker resuelve el nombre mi-db a 172.18.0.2 de forma
# automática gracias al DNS embebido. En el lab se enviaron
# 16 paquetes con 0% de pérdida y ~0.137 ms de promedio.

# Salir del contenedor Alpine
exit
# Sale del shell. Al tener --rm el contenedor se elimina
# automáticamente dejando el entorno limpio.