FROM buildpack-deps:wheezy

RUN mkdir -p /usr/local/etc \
  && { \
    echo 'install: --no-document'; \
    echo 'update: --no-document'; \
  } >> /usr/local/etc/gemrc

ENV RUBY_DOWNLOAD_SHA256 ecf4a6d4c96b547b3bf4b6be14e082ddaa781e83ad7f69437cd3169fb7576e42
ENV RUBYGEMS_VERSION 2.6.6

RUN set -ex \
  && buildDeps=' \
    bison \
    libgdbm-dev \
    ruby \
  ' \
  && echo 'deb http://archive.debian.org/debian wheezy main' > /etc/apt/sources.list \
  && apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y --no-install-recommends $buildDeps \
     gcc-4.6 \
  && curl -fSl -o ruby.tar.gz "https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/rubyenterpriseedition/ruby-enterprise-1.8.7-2012.02.tar.gz" \
  && echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.gz" | sha256sum -c - \
  && mkdir -p /usr/src/ruby \
  && tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1 \
  && rm ruby.tar.gz \
  && cd /usr/src/ruby/source \
  && curl -fSl -o tcmalloc.patch "https://raw.githubusercontent.com/rvm/rvm/master/patches/ree/1.8.7/tcmalloc.patch" \
  && patch -p1 < tcmalloc.patch \
  && curl -fSl -o stdout-rouge-fix.patch "https://raw.githubusercontent.com/rvm/rvm/master/patches/ree/1.8.7/stdout-rouge-fix.patch" \
  && patch -p1 < stdout-rouge-fix.patch \
  && cd /usr/src/ruby \
  && CC=/usr/bin/gcc-4.6 ./installer --auto /usr/local --dont-install-useful-gems \
  && apt-get purge -y --auto-remove $buildDeps \
  && rm -rf /var/lib/apt/lists/* \
  && rm -r /usr/src/ruby

ENV BUNDLER_VERSION 1.12.5

RUN gem install bundler --version "$BUNDLER_VERSION"

ENV GEM_HOME /usr/local/bundle
ENV BUNDLE_PATH="$GEM_HOME" \
  BUNDLE_BIN="$GEM_HOME/bin" \
  BUNDLE_SILENCE_ROOT_WARNING=1 \
  BUNDLE_APP_CONFIG="$GEM_HOME"
ENV PATH $BUNDLE_BIN:$PATH
RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
  && chmod 777 "$GEM_HOME" "$BUNDLE_BIN"

CMD [ "irb" ]
