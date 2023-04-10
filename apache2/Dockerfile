FROM httpd:2.4.55-alpine

# Configuration
# Install Apache modules
RUN apk --update --no-cache add apache2-proxy apache2-ssl

ADD ./default.conf /usr/local/apache2/conf/vhosts/default.conf

# Configure Apache
RUN sed -i 's/^#LoadModule proxy_module/LoadModule proxy_module/' /usr/local/apache2/conf/httpd.conf
RUN sed -i 's/^#LoadModule proxy_fcgi_module/LoadModule proxy_fcgi_module/' /usr/local/apache2/conf/httpd.conf
RUN sed -i 's/^#LoadModule rewrite_module/LoadModule rewrite_module/' /usr/local/apache2/conf/httpd.conf

RUN echo "ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://php:9000/var/www/html/\$1" >> /usr/local/apache2/conf/httpd.conf
RUN echo "DirectoryIndex /index.php index.php" >> /usr/local/apache2/conf/httpd.conf

RUN echo "LoadModule rewrite_module modules/mod_rewrite.so" >> /usr/local/apache2/conf/httpd.conf
RUN echo "Include /usr/local/apache2/conf/vhosts/*.conf" >> /usr/local/apache2/conf/httpd.conf

#
# SSL
# RUN sed -i \
# 		-e 's/^#\(Include .*httpd-ssl.conf\)/\1/' \
# 		-e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
# 		-e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
# 		conf/httpd.conf

# Set the working directory to the document root
WORKDIR /usr/local/apache2/htdocs

# Expose port 80
EXPOSE 80 443

# Start Apache
CMD ["httpd-foreground"]
