# Deploy Rails app to AWS EC2

### Step 1

Add Gems to Gemfile.rb

```

gem 'figaro'
gem 'puma'
group :development do
  gem 'capistrano'
  gem 'capistrano3-puma'
  gem 'capistrano-rails', require: false
  gem 'capistrano-bundler', require: false
  gem 'capistrano-rvm'
end

```

Then run 
```
bundle install
```

### Step 2

Run command: 
```
cap install STAGES=production
```
This will install capistrino configuration to rails app

### Step 3

Update your Capfile like below

```
# Load DSL and set up stages
require "capistrano/setup"

# Include default deployment tasks
require "capistrano/deploy"

# Include tasks from other gems included in your Gemfile
#
# For documentation on these, see for example:
#
#   https://github.com/capistrano/rvm
#   https://github.com/capistrano/rbenv
#   https://github.com/capistrano/chruby
#   https://github.com/capistrano/bundler
#   https://github.com/capistrano/rails
#   https://github.com/capistrano/passenger
#
require 'capistrano/rvm'
# require 'capistrano/rbenv'
# require 'capistrano/chruby'
require 'capistrano/bundler'
require 'capistrano/rails/assets'
require 'capistrano/rails/migrations'
require 'capistrano/puma'
# require 'capistrano/passenger'

# Load custom tasks from `lib/capistrano/tasks` if you have any defined
Dir.glob("lib/capistrano/tasks/*.rake").each { |r| import r }
```


### Step 4

Create Account on AWS
Launch EC2 ubuntu instance


1] Update 
```
sudo apt-get update && sudo apt-get -y upgrade
```
2] Add new user
```
sudo useradd -d /home/deploy -m deploy


sudo passwd deploy


deploy ALL=(ALL:ALL) ALL
```

3] Setup ssh access server to local
```
su - deploy
ssh-keygen

cat .ssh/id_rsa.pub

vi .ssh/authorized_keys # add your id_rsa.pub from local machine to server

```

4] setup git
```
sudo apt-get install git

```

5] Add nginx rules
```
sudo vi /etc/nginx/sites-available/default

```
Open above file and add below code

```
upstream app {
  # Path to Puma SOCK file, as defined previously
  server unix:/home/deploy/your_app_name/shared/tmp/sockets/puma.sock fail_timeout=0;
}

server {
  listen 80;
  server_name localhost;

  root /home/deploy/your_app_name/current/public;

  try_files $uri/index.html $uri @app;

  location / {
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $host;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Connection '';
    proxy_pass http://app;
  }

  location ~ ^/(assets|fonts|system)/|favicon.ico|robots.txt {
    gzip_static on;
    expires max;
    add_header Cache-Control public;
  }

  error_page 500 502 503 504 /500.html;
  client_max_body_size 4G;
  keepalive_timeout 10;
}  
```

6] Setup postgres
```

sudo apt-get install postgresql postgresql-contrib libpq-dev


sudo -u postgres createuser -s urlshortner #create postgres user

sudo -u postgres psql

postgres=# \password urlshortner


sudo -u postgres createdb -O urlshortner urlshortner_production #create postgres database

```

7] Setup Ruby
```
su - deploy
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable



rvm install 2.2.3


gem install bundler --no-ri --no-rdoc

sudo apt-get install build-essential libcurl4-openssl-dev
sudo apt-get install libxml2-dev

mkdir urlshortner
mkdir -p urlshortner/shared/config
nano urlshortner/shared/config/database.yml


nano urlshortner/shared/config/application.yml
```
