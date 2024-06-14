# Utilisez une image de base
FROM ubuntu:latest

# Mettez à jour le système
RUN apt-get update && apt-get upgrade -y

# Installez les dépendances nécessaires
RUN apt-get install -y nginx

# Copiez les fichiers de votre application dans le conteneur
COPY . /var/www/html

# Exposez le port 80 pour le trafic HTTP
EXPOSE 80

# Installez les dépendances nécessaires
# RUN mkdir /var/www/html/data

# Démarrez le serveur web
CMD ["nginx", "-g", "daemon off;"]

