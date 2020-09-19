# Deploy Rails app to AWS EC2 🚀
#### Using capistrano + nginx + postgres

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
This will install capistrino configuration in rails app


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

# Load puma configurations
install_plugin Capistrano::Puma
```

So upto this step we created all local configuration for capistrano, Now let's move towards AWS

### Step 4

Create Account on AWS
Launch EC2 instance

For creating AWS instance you can follow https://www.cloudbooklet.com/create-an-ec2-instance-on-aws-with-ubuntu-18-04/

#### 1] Update 

```
sudo apt-get update && sudo apt-get -y upgrade
```
#### 2] Add new user
```
sudo useradd -d /home/deploy -m deploy
sudo passwd deploy
```

Now open, 
```sudo visudo```
And make following changes,

```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL
deploy    ALL=(ALL:ALL) ALL # make this changes to get all privileges
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:
```


#### 3] Setup ssh-keygen
```
su - deploy # go to deploy user group
ssh-keygen

cat .ssh/id_rsa.pub
```
Now, add your ssh key to server's authorized_keys

```
vi .ssh/authorized_keys # add your id_rsa.pub from local machine to server

```

#### 4] setup git
```
sudo apt-get install git

```

#### 5] Add nginx rules
```
sudo vi /etc/nginx/sites-available/default

```
Open above file and add below code
Make sure that your app path is correct else your appliction won't start

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

#### 6] Setup postgres

```
sudo apt-get install postgresql postgresql-contrib libpq-dev

sudo -u postgres createuser -s your_postgres_user #create postgres user

sudo -u postgres psql

postgres=# \password your_postgres_user #setup password

sudo -u postgres createdb -O your_postgres_user database_name #create postgres database

```

#### 7] Setup Ruby and RVM
```
su - deploy # Make sure that you are in deploy user

gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

\curl -sSL https://get.rvm.io | bash -s stable

rvm install # This will install latest rvm

gem install bundler # install bundler

sudo apt-get install build-essential libcurl4-openssl-dev

sudo apt-get install libxml2-dev
```

#### 8] Create project structure

```
su - deploy # Make sure that you are in deploy user

mkdir your_app_name

mkdir -p your_app_name/shared/config # Create config folder
```
Now create database.yml file and add your configuration

```
vi your_app_name/shared/config/database.yml
```

And add below configuration

```
production:
  adapter: postgresql
  encoding: unicode
  database: your_database_nname
  username: username
  password: password
  host: localhost
  port: 5432
```

Also create application.yml

```
vi your_app_name/shared/config/application.yml
```

And add SECRET_KEY_BASE, if you don't know how to create just fire below command

```
RAILS_ENV=production bundle exec rake secret
```

And finally add that secret key to application.yml 

```
SECRET_KEY_BASE: your_secret_base
```


### Step 5

Install nodejs

```
sudo apt-get install nodejs
```


### Step 6 

Now All Set 🚀

Just run 

```
cap production deploy
```

And this commad will automatically deploy your rails app to AWS server

After that, make sure that your puma is working properly

If it's not just restart it

```
cap production puma:restart
``` 

