# Imagen base actualizada para que sea compatible con VS Code
FROM php:8.2-apache

# Actualizar e instalar Vim automáticamente
RUN apt-get update && apt-get install -y vim

# Copiar tus archivos de la carpeta src al contenedor
COPY src/ /var/www/html

# Exponer el puerto 80
EXPOSE 80

# Tu información
LABEL maintainer="Fabricio Coledani <fabri.uriel.coledani@gmail.com>"