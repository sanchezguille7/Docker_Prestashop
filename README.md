# Docker_Prestashop

Este archivo de Docker Compose configura un entorno de desarrollo que consta de los siguientes servicios:

## Servicio de MariaDB
Este servicio utiliza la imagen de MariaDB versión especificada. Configura un contenedor MariaDB inicializando la base de datos con las siguientes variables de entorno:
- `MYSQL_ROOT_PASSWORD`: Contraseña de root de MySQL.
- `MYSQL_DATABASE`: Nombre de la base de datos.
- `MYSQL_USER`: Usuario de MySQL.
- `MYSQL_PASSWORD`: Contraseña del usuario de MySQL.
Los datos de la base de datos se almacenan en un volumen llamado `mariadb_data`.

## Servicio de phpMyAdmin
Este servicio utiliza la imagen de phpMyAdmin, exponiéndola en el puerto 8080 y vinculándola al servicio MariaDB. Utiliza la siguiente variable de entorno:
- `PMA_HOST`: Especifica el host donde se ejecuta el servicio MariaDB.
Este servicio forma parte de las redes `frontend-network` y `backend-network`.

## Servicio de PrestaShop
Este servicio utiliza la imagen de Bitnami PrestaShop y configura varias variables de entorno para su configuración, como detalles del propietario de la tienda, host de la base de datos y credenciales de la misma. Monta un volumen llamado `prestashop_data` para persistir los datos. Al igual que phpMyAdmin, este servicio forma parte de las redes `frontend-network` y `backend-network`.

## Servicio HTTPS-Portal
Este servicio proporciona encriptación HTTPS utilizando la imagen `https-portal`, escuchando en los puertos 80 y 443 y redirigiendo el tráfico al servicio PrestaShop. Utiliza la variable de entorno `DOMAINS` para especificar la configuración de los dominios y la redirección HTTPS. Los certificados SSL se almacenan en el volumen `ssl_certs_data`.

## Volúmenes
- `mariadb_data`: Almacena los datos de MariaDB.
- `prestashop_data`: Almacena los datos de PrestaShop.
- `ssl_certs_data`: Almacena los certificados SSL para HTTPS.

## Redes
- `backend-network`: Utilizada para la comunicación entre servicios backend (MariaDB, PrestaShop).
- `frontend-network`: Utilizada para la comunicación entre servicios frontend (phpMyAdmin, PrestaShop, HTTPS-Portal).
