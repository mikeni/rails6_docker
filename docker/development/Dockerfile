FROM ruby:2.6.5

# RUN rm /bin/sh && ln -s /bin/bash /bin/sh
RUN pwd
RUN ruby -v

# Install apt based dependencies required to run Rails as 
# well as RubyGems. As the Ruby image itself is based on a 
# Debian image, we use apt-get to install those.
RUN apt-get update && apt-get install -y \ 
  build-essential \
  sudo


RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
RUN apt-get install -y nodejs

RUN node -v
RUN npm -v

RUN npm install -g yarn
RUN yarn install --check-files

RUN yarn -v

# Configure the main working directory. This is the base 
# directory used in any further RUN, COPY, and ENTRYPOINT 
# commands.
RUN mkdir -p /app 
WORKDIR /app
RUN pwd


# Copy the Gemfile as well as the Gemfile.lock and install 
# the RubyGems. This is a separate step so the dependencies 
# will be cached unless changes to one of those two files 
# are made.
# COPY Gemfile Gemfile.lock ./ 
COPY Gemfile Gemfile.lock package.json yarn.lock ./
RUN gem install bundler && gem install foreman && bundle install --verbose --jobs 20 --retry 5





# Copy the main application.
COPY . ./

# RUN bin/rake assets:precompile
RUN bundle exec rake assets:precompile

# Expose port 3000 to the Docker host, so we can access it 
# from the outside.
EXPOSE 3000

# The main command to run when the container starts. Also 
# tell the Rails dev server to bind to all interfaces by 
# default.
CMD ["bundle", "exec", "rails", "server", "-b", "0.0.0.0"]
# CMD ["bundle", "exec", "rails", "server"]