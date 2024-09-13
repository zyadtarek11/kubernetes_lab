FROM httpd:alpine

# Copy the custom configuration file into the container
COPY ./custom-apache.conf /usr/local/apache2/conf/httpd.conf

# If you have SSL certificates, you can copy them here as well
# COPY your-certificates /path/to/certificates
EXPOSE 80
EXPOSE 443