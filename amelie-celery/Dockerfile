FROM debian:bullseye

# Install required debian packages
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y apt-utils git net-tools python3 python3-pip mariadb-client libmariadb-dev xmlsec1 libssl-dev libldap-dev libsasl2-dev libjpeg-dev zlib1g-dev gettext

# Make directories for amelie
RUN mkdir -p /amelie /amelie/static /media /photo_upload /config /var/log /var/run

# Copy amelie sources
COPY amelie-backend /amelie

# Set /amelie as startup working directory
WORKDIR /amelie

# Switch to root user
USER root

# Install python requirements
RUN pip3 install -r requirements.txt

# Install default configuration file
RUN cp "/amelie/amelie/settings/local.py.default" "/amelie/amelie/settings/local.py"

# Correct permissions on directories
RUN chown -R 1000:1000 /amelie /media /photo_upload /config /var/log /var/run

# Switch back to a local user
USER 1000:1000

# Check if Django can run
RUN python3 manage.py check

# Expose volumes
VOLUME ["/amelie/static", "/media", "/photo_upload", "/config"]

# Expose the web port
EXPOSE 80

# Start the website
CMD ["/amelie/scripts/start_celery.sh"]
