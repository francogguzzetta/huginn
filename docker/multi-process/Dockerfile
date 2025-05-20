FROM ruby:2.7

RUN apt-get update -qq && apt-get install -y \
  build-essential \
  libpq-dev \
  nodejs \
  git \
  libffi-dev \
  libxml2-dev \
  libxslt1-dev \
  imagemagick \
  curl \
  libsqlite3-dev

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
  echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
  apt-get update && apt-get install yarn -y

ENV RAILS_ENV=production
ENV RACK_ENV=production

RUN git clone https://github.com/huginn/huginn.git /huginn
WORKDIR /huginn

RUN gem install bundler -v 2.2.33
RUN bundle config set without 'development test'
RUN bundle install

RUN yarn install --pure-lockfile
RUN bundle exec rake assets:precompile

EXPOSE 3000

CMD ["bundle", "exec", "foreman", "start"]
