FROM ruby:2.4

# BASE BEGIN
RUN set -ex \
  && apt-get update \
  && apt-get install -y --no-install-recommends ca-certificates curl wget git-core htop \
   screen net-tools apt-transport-https cron supervisor locales openssh-server vim bzip2 \
   libfontconfig rsync monit net-tools less \
  && rm -rf /var/lib/apt/lists/*

RUN set -ex \
  && { \
      echo '[program:cron]'; \
      echo 'command=/usr/sbin/cron -f'; \
  } >> /etc/supervisor/conf.d/cron.conf \
  \
  && mkdir -p /var/log/supervisor \
  && { \
    echo '[supervisord]'; \
    echo 'nodaemon=true'; \
  } >> /etc/supervisor/conf.d/supervisord.conf \
  \
  && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
  && locale-gen \
  && update-locale LANG=en_US.UTF-8 LANGUAGE=en_US:en: \
  \
  && mkdir -p /var/run/sshd \
  && { \
    echo '[program:sshd]'; \
    echo 'command=/usr/sbin/sshd -D'; \
  } >> /etc/supervisor/conf.d/sshd.conf

ENV LC_ALL en_US.UTF-8
# BASE END

# NGINX_SERVER BEGIN
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 \
  && echo "deb http://nginx.org/packages/debian/ $(awk -F"[)(]+" '/VERSION=/ {print $2}' /etc/os-release) nginx" >> /etc/apt/sources.list \
  && apt-get update \
  && apt-get install --no-install-recommends --no-install-suggests -y \
            ca-certificates \
            nginx \
  && { \
    echo '[program:nginx]'; \
    echo 'command=nginx -g "daemon off;"'; \
  } >> /etc/supervisor/conf.d/nginx.conf \
  && rm -rf /var/lib/apt/lists/*

# NGINX_SERVER END

# NODE10_LTS BEGIN
RUN set -ex \
  && curl -sL https://deb.nodesource.com/setup_10.x | bash - \
  && apt-get update \
  && apt-get install -y --no-install-recommends nodejs \
  && rm -rf /var/lib/apt/lists/*


# NODE10_LTS END

# YARN BEGIN
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
  && apt-get update \
  && apt-get install --no-install-recommends yarn \
  && rm -rf /var/lib/apt/lists/*

# YARN END

# TIMEZONE BEGIN
RUN set -ex \
  && echo "Asia/Shanghai" > /etc/timezone \
  && dpkg-reconfigure -f noninteractive tzdata

# TIMEZONE END

# SSH_AGENT BEGIN
RUN set -ex \
  && mkdir -p /root/.ssh /root/.bash.d \
  && { \
    echo 'Host *'; \
    echo 'ServerAliveInterval=15'; \
    echo 'ServerAliveCountMax=6'; \
    echo 'ForwardAgent yes'; \
  } >> /root/.ssh/config \
  && { \
    echo 'source ~/.bash.d/10_ssh-agent.bash'; \
  } >> /root/.bashrc

RUN set -ex \
  && cd /root/.bash.d \
  && curl -SLO https://raw.githubusercontent.com/agideo/docker-baseimage/master/files/bash.d/10_ssh-agent.bash \
  && cd -

# SSH_AGENT END

# DOTBASHRC BEGIN
RUN set -ex \
  && { \
    echo "export TERM=xterm"; \
    echo "export PATH=$(echo $PATH)"; \
    echo "unset HISTFILE"; \
  } >> /root/.bashrc

# DOTBASHRC END

RUN mkdir /app
WORKDIR /app
VOLUME /app

EXPOSE 22 80 3000
CMD ["sh", "-c", "env >> /etc/environment ; /usr/bin/supervisord"]
