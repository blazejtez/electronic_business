FROM prestashop/prestashop-git:latest

# To run files with the same group as your primary user
ARG GROUP_ID
ARG USER_ID

RUN groupmod -g $GROUP_ID www-data \
  && usermod -u $USER_ID -g $GROUP_ID www-data

COPY .docker/wait-for-it.sh /tmp/
COPY .docker/docker_run_git.sh /tmp/
COPY .docker/000-default.conf /tmp/
COPY .docker/apache2.conf /tmp/

RUN mkdir -p /var/www/.npm
RUN chown -R www-data:www-data /var/www/.npm
RUN mkdir -p /var/www/.composer
RUN chown -R www-data:www-data /var/www/.composer

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt install -y nodejs

# Install mailutils to make sendmail work
RUN apt install -y \
    apt-utils \
    mailutils

COPY ./ /var/www/html/

RUN chown -R www-data:www-data /var/www/html/


CMD ["/tmp/wait-for-it.sh", "--timeout=60", "--strict", "be_12455_mysql:3306", "--", "/tmp/docker_run_git.sh"]
