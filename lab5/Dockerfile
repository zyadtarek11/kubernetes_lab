# Use an official Apache image as a base
FROM httpd:alpine

# Install OpenSSL to generate certificates
RUN apk add --no-cache openssl

# Copy your custom Apache config
COPY custom-apache.conf /usr/local/apache2/conf/custom-apache.conf

# Generate SSL certificate and key
RUN openssl req -x509 -nodes -days 365 \
    -newkey rsa:4096 -keyout /usr/local/apache2/conf/mycert.key \
    -out /usr/local/apache2/conf/mycert.crt \
    -subj "/C=US/ST=State/L=City/O=Organization/OU=Department/CN=localhost"
   
RUN chmod 600 /usr/local/apache2/conf/mycert.key
RUN chmod 644 /usr/local/apache2/conf/mycert.crt

# Update the main Apache config to include custom configurations
RUN echo "Include /usr/local/apache2/conf/custom-apache.conf" >> /usr/local/apache2/conf/httpd.conf

# Expose both ports
EXPOSE 80 
EXPOSE 443