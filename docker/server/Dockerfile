ARG PHP_VERSION
FROM php:${PHP_VERSION}-apache

RUN a2enmod rewrite

# Install packages
RUN apt-get update && apt-get install -y git unzip zip nano 

# Add user
ARG PROJECT_NAME_LOWERCASE
ARG USERNAME=user${PROJECT_NAME_LOWERCASE}
ARG USER_UID
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME 

WORKDIR /var/www/html

# Install PHP extensions
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/
RUN install-php-extensions gd pdo_mysql bcmath zip intl opcache

# Composer
COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer

WORKDIR /var/www/html/${PROJECT_NAME_LOWERCASE}

#ENV TERM xterm-256color
RUN echo PS1="'"'\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\ \033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '"'" >> ~/.bashrc
RUN echo alias ls="'"'ls --color=auto'"'" >> ~/.bashrc

RUN composer self-update

# Get symfony console
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' |bash
RUN apt install symfony-cli

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME

