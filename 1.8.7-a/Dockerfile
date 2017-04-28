FROM agideo/ruby:ree-1.8.7-2012.02

RUN set -ex \
  && sed -i 's/security.debian.org/mirrors.ustc.edu.cn\/debian-security/g' /etc/apt/sources.list \
  && sed -i 's/httpredir.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

RUN set -ex \
  && apt-get update \
  && apt-get install -y --no-install-recommends supervisor \
    openssh-server \
    git-core \
    vim \
    locales \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN set -ex \
  && mkdir -p /var/run/sshd /var/log/supervisor /root/.ssh \
  && { \
    echo '[supervisord]'; \
    echo 'nodaemon=true'; \
    echo '[program:sshd]'; \
    echo 'command=/usr/sbin/sshd -D'; \
  } >> /etc/supervisor/conf.d/supervisord.conf

# begin setting locale
RUN set -ex \
  && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
  && locale-gen \
  && update-locale LANG=en_US.UTF-8 LANGUAGE=en_US:en:

ENV LC_ALL en_US.UTF-8
# end

# begin setting timezone
RUN set -ex \
  && echo "Asia/Shanghai" > /etc/timezone \
  && dpkg-reconfigure -f noninteractive tzdata
#end

RUN mkdir /app
WORKDIR /app
VOLUME /app

EXPOSE 22
CMD ["/usr/bin/supervisord"]
