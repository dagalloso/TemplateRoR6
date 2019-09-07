FROM ruby:2.6.3
RUN apt-get update &&\
    # add support to unicode chars from keyboard: ç,ã,ô:
    apt-get install -y locales &&\
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && /usr/sbin/locale-gen &&\
    rm -rf /var/lib/apt/lists/*
ENV LANG en_US.UTF-8
RUN apt-get update -qq && apt-get install -y postgresql-client
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs

RUN node -v
RUN npm -v

ENV APP_HOME /rubyapp

RUN mkdir $APP_HOME
WORKDIR $APP_HOME
COPY Gemfile $APP_HOME/Gemfile
COPY Gemfile.lock $APP_HOME/Gemfile.lock
RUN gem install bundler -v 1.17.2
RUN gem install foreman -v 0.85.0
RUN bundle install --verbose --jobs 20 --retry 5

RUN npm install -g yarn
RUN yarn install --check-files

COPY . $APP_HOME

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
