# dmit-nginx-proxy-manager

Este repositorio configura y despliega un Nginx Proxy Manager para gestionar el dominio `www.dmit.ar`.

## Descripción

Nginx Proxy Manager es una herramienta que facilita la gestión de proxies inversos con Nginx, incluyendo la configuración de hosts, certificados SSL, y más, a través de una interfaz gráfica de usuario.

## Estructura del Proyecto

- `config.json`: Archivo de configuración para la base de datos.
- `docker-compose.yml`: Archivo de configuración de Docker Compose para desplegar los servicios necesarios.
- `README.md`: Este archivo.

## Servicios

El despliegue incluye los siguientes servicios:

- **app**: Servicio principal que corre Nginx Proxy Manager.
  - Imagen: `jc21/nginx-proxy-manager:latest`
  - Puertos: `80`, `81`, `443`
  - Volúmenes:
    - `./config.json:/app/config/production.json`
    - `./data:/data`
    - `./letsencrypt:/etc/letsencrypt`

- **db**: Servicio de base de datos MariaDB.
  - Imagen: `jc21/mariadb-aria:10.4.15`
  - Variables de entorno:
    - `MYSQL_ROOT_PASSWORD: 'npm'`
    - `MYSQL_DATABASE: 'npm'`
    - `MYSQL_USER: 'npm'`
    - `MYSQL_PASSWORD: 'npm'`
  - Volúmenes:
    - `./data/mysql:/var/lib/mysql`

## Nota: Comandos útilies
Para limpiar todos los contenedores, redes y volúmenes que no se usan en Docker, puedes usar los siguientes comandos:

1. Para eliminar todos los contenedores detenidos, redes no utilizadas, imágenes colgantes y volúmenes no utilizados:

```sh
docker system prune -a --volumes
```

Este comando eliminará:

- Todos los contenedores detenidos
- Todas las redes no utilizadas
- Todas las imágenes colgantes (imágenes sin etiquetas)
- Todos los volúmenes no utilizados

2. Si solo quieres eliminar los volúmenes no utilizados:

```sh
docker volume prune
```

3. Si solo quieres eliminar las redes no utilizadas:

```sh
docker network prune
```

4. Si solo quieres eliminar los contenedores detenidos:

```sh
docker container prune
```

Estos comandos te ayudarán a limpiar tu entorno Docker de recursos no utilizados.

## Uso

Para desplegar los servicios, ejecuta el siguiente comando:

```sh
docker-compose up -d

