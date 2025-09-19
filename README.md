# 🐳 Guía de Comandos Docker - FRI 2025


---

## 📊 Tabla de Flags más Importantes de Docker

| Flag | Comando | Descripción | Ejemplo |
|------|---------|-------------|---------|
| `-d, --detach` | `run` | Ejecuta contenedor en segundo plano | `docker run -d nginx` |
| `-it` | `run, exec` | Modo interactivo con terminal | `docker run -it ubuntu bash` |
| `-p, --publish` | `run` | Mapea puerto host:contenedor | `docker run -p 8080:80 nginx` |
| `-P` | `run` | Mapea todos los puertos expuestos | `docker run -P nginx` |
| `-v, --volume` | `run` | Monta volumen o carpeta | `docker run -v /host:/container nginx` |
| `--name` | `run` | Asigna nombre al contenedor | `docker run --name web nginx` |
| `-e, --env` | `run` | Define variable de entorno | `docker run -e MYSQL_ROOT_PASSWORD=123 mysql` |
| `--env-file` | `run` | Lee variables desde archivo | `docker run --env-file .env nginx` |
| `--network` | `run` | Conecta a red específica | `docker run --network mi-red nginx` |
| `--rm` | `run` | Auto-elimina al detenerse | `docker run --rm alpine echo "hola"` |
| `-f, --force` | `rm, rmi` | Fuerza eliminación | `docker rm -f contenedor` |
| `--restart` | `run` | Política de reinicio | `docker run --restart always nginx` |
| `-a, --all` | `ps, images` | Muestra todos (incluso inactivos) | `docker ps -a` |
| `-q, --quiet` | `ps, images` | Solo muestra IDs | `docker ps -q` |
| `-t, --tag` | `build` | Etiqueta imagen al construir | `docker build -t mi-app .` |
| `--build-arg` | `build` | Pasa argumentos al build | `docker build --build-arg VERSION=1.0 .` |
| `-f, --file` | `build, compose` | Especifica archivo | `docker build -f Dockerfile.prod .` |
| `--no-cache` | `build` | Ignora cache al construir | `docker build --no-cache .` |
| `-f, --follow` | `logs` | Sigue logs en tiempo real | `docker logs -f contenedor` |
| `--tail` | `logs` | Muestra últimas N líneas | `docker logs --tail 50 contenedor` |
| `--since` | `logs` | Logs desde tiempo específico | `docker logs --since 30m contenedor` |
| `-w, --workdir` | `run` | Directorio de trabajo | `docker run -w /app node npm start` |
| `--user` | `run` | Usuario para ejecutar | `docker run --user 1000:1000 nginx` |
| `--memory` | `run` | Límite de memoria | `docker run --memory 512m nginx` |
| `--cpus` | `run` | Límite de CPUs | `docker run --cpus 0.5 nginx` |
| `--hostname` | `run` | Define hostname | `docker run --hostname web01 nginx` |
| `--mount` | `run` | Sintaxis nueva para volúmenes | `docker run --mount source=vol,target=/data nginx` |
| `--format` | `ps, images, inspect` | Formato de salida personalizado | `docker ps --format "table {{.Names}}\t{{.Status}}"` |
| `-s, --size` | `ps` | Muestra tamaño de contenedores | `docker ps -s` |
| `--scale` | `compose up` | Escala servicio | `docker-compose up --scale web=3` |

---

## 🔧 Comandos Básicos de Docker

### Gestión de Contenedores

```bash
# Ejecutar contenedor
docker run <imagen>
docker run -d nginx                           # En segundo plano
docker run -it ubuntu bash                    # Interactivo
docker run --rm alpine echo "hola"            # Auto-eliminar
docker run -d -p 8080:80 --name web nginx    # Con nombre y puerto

# Listar contenedores
docker ps                                      # Solo activos
docker ps -a                                   # Todos
docker ps -q                                   # Solo IDs
docker ps -s                                   # Con tamaño

# Control de contenedores
docker start <contenedor>
docker stop <contenedor>
docker restart <contenedor>
docker pause <contenedor>
docker unpause <contenedor>
docker kill <contenedor>

# Eliminar contenedores
docker rm <contenedor>
docker rm -f <contenedor>                     # Forzar
docker container prune                        # Eliminar detenidos

# Ejecutar comandos en contenedor
docker exec <contenedor> ls
docker exec -it <contenedor> bash
docker exec -it <contenedor> sh              # Si no hay bash

# Ver logs
docker logs <contenedor>
docker logs -f <contenedor>                   # Tiempo real
docker logs --tail 50 <contenedor>           # Últimas 50 líneas
docker logs --since 30m <contenedor>         # Últimos 30 minutos
docker logs -t <contenedor>                   # Con timestamps

# Copiar archivos
docker cp archivo.txt <contenedor>:/tmp/
docker cp <contenedor>:/app/config.json ./

# Inspeccionar
docker inspect <contenedor>
docker port <contenedor>
docker top <contenedor>
docker diff <contenedor>
docker stats                                  # Estadísticas en vivo
docker stats --no-stream                      # Una sola lectura
```

---

## 📦 Gestión de Imágenes

```bash
# Descargar imágenes
docker pull <imagen>
docker pull nginx
docker pull ubuntu:20.04
docker pull alpine:latest

# Listar imágenes
docker images
docker image ls
docker images -a                              # Incluye intermedias
docker images -q                              # Solo IDs

# Buscar imágenes
docker search ubuntu
docker search nginx

# Eliminar imágenes
docker rmi <imagen>
docker rmi -f <imagen>                        # Forzar
docker image prune                            # Eliminar sin usar
docker image prune -a                         # Eliminar todas sin usar

# Etiquetar imágenes
docker tag <origen> <destino>
docker tag nginx:latest mi-nginx:v1
docker tag abc123 mi-app:produccion

# Construir imágenes
docker build -t <nombre> .
docker build -t mi-app:v1 .
docker build -f Dockerfile.prod -t mi-app:prod .
docker build --no-cache -t mi-app .
docker build --build-arg VERSION=1.0 -t mi-app .

# Ver historial
docker history <imagen>

# Guardar y cargar imágenes
docker save <imagen> > archivo.tar
docker save mi-app:v1 -o mi-app.tar
docker load < archivo.tar
docker load -i mi-app.tar

# Publicar imagen
docker login
docker push <usuario>/<imagen>:<tag>
docker logout
```

---

## 💾 Gestión de Volúmenes

```bash
# Crear volúmenes
docker volume create <nombre>
docker volume create mis-datos

# Listar volúmenes
docker volume ls
docker volume ls -q                           # Solo nombres

# Inspeccionar volumen
docker volume inspect <volumen>

# Eliminar volúmenes
docker volume rm <volumen>
docker volume prune                           # Eliminar sin usar

# Usar volúmenes en contenedores
docker run -v <volumen>:/data <imagen>
docker run -v mis-datos:/app/data nginx

# Bind mounts (carpeta del host)
docker run -v $(pwd):/app nginx
docker run -v /home/user/web:/usr/share/nginx/html nginx

# Sintaxis mount (nueva)
docker run --mount type=volume,source=mis-datos,target=/data nginx
docker run --mount type=bind,source=/home/user,target=/app nginx
docker run --mount type=tmpfs,destination=/tmp nginx

# Volumen de solo lectura
docker run -v /host:/container:ro nginx
docker run --mount type=bind,source=/src,target=/app,readonly nginx
```

---

## 🌐 Gestión de Redes

```bash
# Listar redes
docker network ls
docker network ls -q                          # Solo IDs

# Crear red
docker network create <nombre>
docker network create mi-red
docker network create --driver bridge app-net
docker network create --subnet=172.20.0.0/16 custom-net

# Inspeccionar red
docker network inspect <red>
docker network inspect bridge

# Conectar/desconectar contenedor
docker network connect <red> <contenedor>
docker network disconnect <red> <contenedor>

# Eliminar red
docker network rm <red>
docker network prune                          # Eliminar sin usar

# Ejecutar contenedor en red específica
docker run --network <red> <imagen>
docker run --network mi-red --name web nginx
docker run --network host nginx               # Usar red del host
docker run --network none nginx               # Sin red
```

---

## 🐙 Docker Compose

```bash
# Comandos básicos
docker-compose up                             # Iniciar servicios
docker-compose up -d                          # En segundo plano
docker-compose down                           # Detener y eliminar
docker-compose stop                           # Solo detener
docker-compose start                          # Iniciar detenidos
docker-compose restart                        # Reiniciar

# Ver estado
docker-compose ps
docker-compose top

# Logs
docker-compose logs
docker-compose logs -f                        # Tiempo real
docker-compose logs <servicio>
docker-compose logs --tail 50 web

# Ejecutar comandos
docker-compose exec <servicio> <comando>
docker-compose exec web bash
docker-compose run web npm install

# Construcción
docker-compose build
docker-compose build --no-cache
docker-compose up --build                     # Construir e iniciar

# Escalar servicios
docker-compose up -d --scale web=3

# Otros comandos
docker-compose pull                           # Actualizar imágenes
docker-compose push                           # Subir imágenes
docker-compose config                         # Validar archivo
docker-compose down -v                        # Eliminar con volúmenes

# Usar archivo específico
docker-compose -f docker-compose.prod.yml up
docker-compose -f custom.yml up -d
```

---

## 🧹 Limpieza del Sistema

```bash
# Eliminar contenedores detenidos
docker container prune
docker container prune -f                     # Sin confirmación

# Eliminar imágenes sin usar
docker image prune
docker image prune -a                         # Todas sin usar
docker image prune -f                         # Sin confirmación

# Eliminar volúmenes sin usar
docker volume prune
docker volume prune -f

# Eliminar redes sin usar
docker network prune
docker network prune -f

# Limpieza completa del sistema
docker system prune                           # Básica
docker system prune -a                        # Incluye imágenes
docker system prune --volumes                 # Incluye volúmenes
docker system prune -a --volumes -f          # Todo sin confirmar

# Ver uso de espacio
docker system df
docker ps -s                                  # Tamaño de contenedores
```

---

## 🔍 Comandos de Información y Debug

```bash
# Información del sistema
docker version
docker info
docker system df
docker system events                          # Eventos en tiempo real

# Inspeccionar recursos
docker inspect <recurso>
docker inspect <contenedor>
docker inspect <imagen>
docker inspect <red>
docker inspect <volumen>

# Formato personalizado de salida
docker ps --format "table {{.Names}}\t{{.Status}}"
docker images --format "{{.Repository}}:{{.Tag}}"
docker inspect -f '{{.NetworkSettings.IPAddress}}' <contenedor>

# Ver procesos
docker top <contenedor>

# Ver cambios en filesystem
docker diff <contenedor>

# Ver puertos
docker port <contenedor>

# Estadísticas
docker stats
docker stats <contenedor>
docker stats --no-stream

# Eventos del sistema
docker events
docker events --since '30m'
docker events --filter container=web
```

---

## ⚡ Comandos Rápidos y Útiles

```bash
# Detener todos los contenedores
docker stop $(docker ps -q)

# Eliminar todos los contenedores
docker rm $(docker ps -aq)

# Eliminar todos los contenedores activos
docker rm -f $(docker ps -q)

# Eliminar todas las imágenes
docker rmi $(docker images -q)

# Ver IPs de todos los contenedores
docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q)

# Logs de múltiples contenedores
docker ps -q | xargs -I {} docker logs --tail 5 {}

# Backup de volumen
docker run --rm -v <volumen>:/source -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz -C /source .

# Restaurar volumen
docker run --rm -v <volumen>:/target -v $(pwd):/backup alpine tar xzf /backup/backup.tar.gz -C /target

# Ejecutar comando temporal
docker run --rm alpine ping -c 3 google.com
docker run --rm busybox echo "Hola Docker"

# Ver tamaño de imágenes ordenado
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}" | sort -k3 -h
```

---





---

*Documento preparado para el curso FRI 2025 - Docker y Contenedorización*
