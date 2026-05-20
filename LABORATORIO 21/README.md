# ?? Mi Web - Laboratorio 21: Distribución Global y Control de Versiones

## ?? Imagen en Docker Hub
?? https://hub.docker.com/r/jesuseduardoangel/bci-web

## ??? Etiquetas disponibles
| Tag      | Descripción                        |
|----------|------------------------------------|
| `1.0.0`  | Versión semántica estable inicial  |
| `stable` | Alias de producción recomendado    |

## ?? Comandos utilizados

### Login
```bash
docker login -u jesuseduardoangel
```

### Versionado semántico
```bash
docker tag jesuseduardoangel/mi-web:v1.0 jesuseduardoangel/bci-web:1.0.0
docker tag jesuseduardoangel/mi-web:v1.0 jesuseduardoangel/bci-web:stable
```

### Distribución global
```bash
docker push jesuseduardoangel/bci-web:1.0.0
docker push jesuseduardoangel/bci-web:stable
```

### Redespliegue desde entorno limpio
```bash
docker rmi -f $(docker images -q)
docker run -d -p 8080:80 jesuseduardoangel/bci-web:stable
```

### Inspección de capas
```bash
docker history jesuseduardoangel/bci-web:1.0.0
```