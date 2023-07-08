## Instalacion de Traefik Dashboard

# 1.- Crear el directorio traefik-dashboard
$ mkdir traefik-dashboard

# 2.- Instala las herramientas httpd-tools
$ sudo dnf install httpd-tools -y

# 3.- Creacion de secretos
cree un usuario y una contraseña codificados en base64 que se usarán dentro del secreto de Kubernetes:
$ htpasswd -nb superTraefikUser unbelievableSafePassword | openssl base64
c3VwZXJUcmFlZmlrVXNlcjokYXByMSQyOXhpMGxkeSRJWEJoRS9LdXJhcmJjM0pBT1BQL2UxCgo=

$ vim 001-auth-secret.yaml
Agrega el siguiente contenido:
---
apiVersion: v1
kind: Secret
metadata:
  name: traefik-dashboard-auth
  namespace: traefik

data:
  users: c3VwZXJUcmFlZmlrVXNlcjokYXByMSQyOXhpMGxkeSRJWEJoRS9LdXJhcmJjM0pBT1BQL2UxCgo=

apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: traefik-dashboard-basicauth
  namespace: traefik

$ vim 002-middleware.yaml
Agrega el siguiente contenido:
spec:
  basicAuth:
    secret: traefik-dashboard-auth

