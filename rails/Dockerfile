FROM ubuntu:16.10

SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND noninteractive

RUN useradd -ms /bin/bash user
RUN echo 'gem: --no-ri --no-rdoc' > /etc/gemrc
WORKDIR /home/user

RUN  apt-get update \
  && apt-get install -y --no-install-recommends \
  git \
  software-properties-common \
  build-essential \
  patch \
  nodejs \
  npm \
  zlib1g-dev \
  liblzma-dev \
  libpq-dev \
  && apt-add-repository ppa:brightbox/ruby-ng \
  && apt-get update \
  && rm -rf /var/lib/apt/lists/*

CMD bundle exec puma -C config/puma.rb

ONBUILD ARG RUBY_VERSION=2.4
ONBUILD ARG RAILS_VERSION=5.0.1

ONBUILD RUN  apt-get update \
  && apt-get install -y \
  ruby$RUBY_VERSION ruby$RUBY_VERSION-dev \
  && rm -rf /var/lib/apt/lists

ONBUILD RUN gem install bundler
ONBUILD RUN gem install rails -v $RAILS_VERSION

ONBUILD COPY Gemfile /home/user/Gemfile
ONBUILD COPY Gemfile.lock /home/user/Gemfile.lock

ONBUILD RUN bundle install

ONBUILD COPY . /home/user

# Add correct rights
ONBUILD RUN chown -R user:user /home/user
ONBUILD USER user
