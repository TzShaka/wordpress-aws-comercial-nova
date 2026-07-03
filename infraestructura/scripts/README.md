# Scripts de infraestructura

Este proyecto se desplegó usando AWS CLI directamente desde AWS CloudShell,
ejecutando los comandos documentados en `aws/inventario_recursos_aws.md`.

- `user-data-wordpress.sh`: script de arranque (user-data) ejecutado automáticamente
  por cada instancia EC2 al lanzarse. Instala Apache, PHP y WordPress.
