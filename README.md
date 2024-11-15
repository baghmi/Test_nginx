# Proyecto NGIX con Vagrant

Este Vagrantfile configura una máquina virtual utilizando Vagrant con una base Debian (bookworm64). Configura una red privada, instala Nginx, Git y VSFTPD, y establece dos sitios web: uno basado en Git y otro basado en FTP. La configuración también incluye la creación de un certificado SSL y la gestión de servicios de Nginx y VSFTPD.

## Estructura de Archivos

### Vagrantfile
Este archivo es la configuración principal para Vagrant:

  - Nombre de la VM: ismael
  - IP de red privada: `192.168.57.102`
  - Distribucion: `debian/bookworm64`

  **Primer Sitio Web**
  - Crea un directorio para el contenido web en /var/www/ismael/html.
  - Clona un ejemplo de sitio web estático desde GitHub en este directorio.
  - Establece la propiedad para www-data y permisos.
  - Copia el archivo de configuración del sitio Nginx desde /vagrant/ismael-sites a /etc/nginx/sites-available/.
  - Habilita el sitio mediante un enlace simbólico en /etc/nginx/sites-enabled/.

  **Segundo Sitio Web**
  - Crea un directorio para un segundo servicio web en /var/www/segunda/html.
  - Crea un usuario ftpuser con un directorio principal en /var/www/segunda/html y establece la contraseña en vagrant.
  - Ajusta los permisos para otorgar la propiedad al usuario.
  - Copia la configuración del sitio Nginx desde /vagrant/segunda-sites a /etc/nginx/sites-available/ y lo habilita.

  **Configuracion para el ftp**
  - Genera un certificado SSL autofirmado para FTP.
  - Copia el archivo de configuración personalizado de VSFTPD desde /vagrant/vsftpd.conf a /etc/vsftpd.conf.

### ismael-sites
```
server {
    listen 80;
    listen [::]:80;
    root /var/www/ismael/html;
    index index.html index.htm index.nginx-debian.html;
    server_name ismael;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

- Raíz del documento: El contenido del servidor está ubicado en el directorio /var/www/ismael/html.
- Nombre del servidor: El nombre del servidor es ismael.

### segunda-sites
```
server {
    listen 80;
    listen [::]:80;
    root /var/www/segunda/html;
    index index.html index.htm index.nginx-debian.html;
    server_name segunda;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

- Raíz del documento: El contenido del servidor está ubicado en el directorio /var/www/segunda/html.
- Nombre del servidor: El nombre del servidor es segunda.

### vsftpd.conf
Aqui hemos añadido el codigo que viene en la practica
```
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH
local_root=/var/www/segunda/html
```
y ademas de esto tambien hemos añadido:
- Para que me deje mandar los ficheros que quiera `write_enable=YES`
- Para que me ponga los permisos que quiero para que el nginx acceda a este `local_umask=022`

## Filezilla
Para la realización de esta practica tendremos que pasar la segunda web por filezilla en el sitio indicado
