# Imagen base MySQL 8 oficial
FROM mysql:8

# Copiar el script de inicialización dentro de la imagen
COPY init.sql /docker-entrypoint-initdb.d/

# Exponer el puerto de MySQL
EXPOSE 3306
