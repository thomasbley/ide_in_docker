# docker-compose build --build-arg UID=$(id -u)

FROM debian:bullseye-slim

RUN apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install apt-transport-https curl ca-certificates \
    && echo "deb https://packages.sury.org/php/ bullseye main" >/etc/apt/sources.list.d/ondrej-debian-php.list \
    && curl -s https://packages.sury.org/php/apt.gpg >/etc/apt/trusted.gpg.d/php.gpg \
    && apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install git php8.0-cli \
        php8.0-mbstring php8.0-curl php8.0-xml php8.0-zip \
    && curl -#L -o /tmp/code-server.deb https://github.com/cdr/code-server/releases/download/v3.11.1/code-server_3.11.1_amd64.deb \
    && dpkg -i /tmp/code-server.deb \
    && apt-get upgrade -y \
    && apt-get clean \
    && rm -rf /tmp/* /var/lib/apt/lists/* /var/cache/apt/archives/*

ARG UID
RUN useradd -m docker -u ${UID} -d /config -s /bin/bash

USER docker

ENV SERVICE_URL=https://open-vsx.org/vscode/gallery
ENV ITEM_URL=https://open-vsx.org/vscode/item

ADD LouisWT.regexp-preview-0.1.5.vsix /tmp/

RUN code-server --install-extension felixfbecker.php-debug \
                --install-extension fterrag.vscode-php-cs-fixer \
                --install-extension alphabotsec.vscode-eclipse-keybindings \
                --install-extension hediet.vscode-drawio \
                --install-extension getpsalm.psalm-vscode-plugin \
                --install-extension /tmp/LouisWT.regexp-preview-0.1.5.vsix \
    && chmod -R a+w /config

WORKDIR /var/www

EXPOSE 8000

CMD [ "code-server", "--auth", "none", "--disable-telemetry", "--disable-update-check", "--bind-addr", "0.0.0.0:8000" ]
