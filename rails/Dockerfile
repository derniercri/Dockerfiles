FROM ubuntu:16.10

SHELL ["/bin/bash", "-c"]

RUN useradd -ms /bin/bash user

# Install all dependencies
RUN  apt-get update \
  && apt-get install -y \
  git-core \
  curl \
  zlib1g-dev \
  build-essential \
  libssl-dev \
  libreadline-dev \
  libyaml-dev \
  libsqlite3-dev \
  sqlite3 \
  libxml2-dev \
  libxslt1-dev \
  libcurl4-openssl-dev \
  python-software-properties \
  libffi-dev \
  nodejs \
  libgdbm-dev \
  libncurses5-dev \
  automake \
  libtool \
  bison \
  libffi-dev \
  && rm -rf /var/lib/apt/lists/*

# Install RVM
USER user
WORKDIR /home/user

ENV PATH $PATH:/home/user/.rvm/bin

RUN curl -sSL https://get.rvm.io | bash

# On build, install the needed Ruby and Rails versions
ONBUILD ARG RUBY_VERSION=2.4.0
ONBUILD ARG RAILS_VERSION=5.0.1

ONBUILD ENV PATH $PATH:/home/user/.rvm/rubies/ruby-$RUBY_VERSION/bin
ONBUILD ENV PATH $PATH:/home/user/.rvm/gems/ruby-$RUBY_VERSION/bin

ONBUILD RUN rvm autolibs disable && rvm install $RUBY_VERSION && rvm use $RUBY_VERSION
ONBUILD RUN gem install bundler
ONBUILD RUN gem install rails -v $RAILS_VERSION

# On build, install app dependencies
ONBUILD COPY Gemfile /home/user/Gemfile
ONBUILD COPY Gemfile.lock /home/user/Gemfile.lock

ONBUILD RUN bundle install
ONBUILD COPY . /home/user

# Add correct rights
ONBUILD USER root
ONBUILD RUN chown -R user:user /home/user
ONBUILD USER user

# Default start command
ONBUILD CMD bundle exec puma -C config/puma.rb
