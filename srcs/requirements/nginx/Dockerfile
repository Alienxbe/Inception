FROM debian:bullseye

RUN apt-get update && apt-get upgrade -y && \
	apt-get install -y nginx openssl && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

# Remove default nginx configuration files
RUN rm -f /etc/nginx/sites-enabled/default
RUN rm -f /etc/nginx/sites-available/default

COPY ./conf/nginx.conf /etc/nginx/nginx.conf

COPY tools/setup_ssl.sh /setup.sh
RUN chmod +x /setup.sh

# Donne les droits en lecture/écriture/exécution pour le propriétaire, et lecture/exécution pour les autres
# sur tous les fichiers/dossiers du répertoire /var/www/html
RUN chmod -R 755 /var/www/html

# Change le propriétaire des fichiers du dossier /var/www/html en 'www-data'
# (l'utilisateur utilisé par NGINX pour exécuter les processus web)
RUN chown -R www-data:www-data /var/www/html

# Déclare le port 443 comme exposé (HTTPS)
EXPOSE 443

# Utilise le script bash comme point d’entrée (entrypoint)
ENTRYPOINT ["bash", "/setup.sh"]