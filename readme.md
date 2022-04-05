## Docker con WP + PHP-FPM + Nginx + WP-CLI + MariaDB + PMA + Redis + SSL


### Instalación / configuración

- La plantilla está configurada para crear certificados SSL a través de mkcert, para poder tener nuestro dominio bajo SSL, intrucciones para instalarlo: https://github.com/FiloSottile/mkcert
- Se puede elegir la versión de WordPress y de PHP cambiando la primera sentencia del `Dockerfile` eligiendo cualquiera de las imágenes oficiales disponibles en https://hub.docker.com/_/wordpress/
- El resto de contenedores/servicios están indicados desde el archivo `docker-compose.yml`
- Xdebug queda instalado en el servicio `wp` que es el que contiene WordPress y PHP-FPM.
- Modificar las variables de entorno del archivo `.env` por las de nuestro proyecto particular.
- Modificar archivo `nginx.conf`, indicando el nombre correcto de los certificados que hayamos creado previamente y el nombre de nuestro dominio.



#### Archivos personalizados

- `.htaccess`
- `nginx.conf`
- `php.conf.ini`
- `xdebug.ini`


__*Nota__: En la plantilla se utiliza el dominio `local.wp-nginx.com` como ejemplo.


### Puesta en marcha

1. ```git clone git@github.com:josandreu/docker-wp-php-fpm-nginx.git```
2. Modificamos las variables de entorno del archivo `.env`, especialmente `PROJECT_NAME`
3. ```cd docker/config/nginx-conf/ssl```
4. `mkcert -install nombre-dominio-proyecto`
5. Modificamos el archivo `nginx.conf`, indicando el nombre correcto de SSLCertificateFile y SSLCertificateKeyFile, que corresponderán a los nombres de los certificados que hayamos creado previamente con mkcert. Como ejemplo está `local.wp-nginx.com`
6. Cambiar `server_name` en el archivo `nginx.conf` para que coincida con nuestro nombre de dominio, como ejemplo está `local.wp-nginx.com`
7. `cd ../../../..`
8. `docker-compose up -d`
9. Esperaremos unos instantes a que se copien los archivos de WordPress para que el servidor pueda responder.
10. Realizamos la configuración inicial de WordPress 
11. Si queremos tener Redis activado, instalamos el plugin para Redis con wp-cli: `docker-compose run --rm wpcli plugin install redis-cache`
12. Activamos el plugin de Redis y comprobamos la conexión


### WP-CLI

Para ejecutar wp-cli: `docker-compose run --rm wpcli {comando}`

Nos muestra la info del contenedor de wp-cli: `docker-compose run --rm wpcli`

Para mostrar los usuarios de WP: `docker-compose run --rm wpcli user list`



