### Install ruby to your machine
```
sudo apt-get update && sudo apt-get install ruby
```

### Install curl
```
sudo apt-get install curl
```

### Install RVM
```
gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

```
\curl -sSL https://get.rvm.io | bash -s stable
```

*Note:- Please restart your terminal to get things ready*


### Install Third party libraries for making things correctly install
 
```
sudo nano /etc/apt/sources.list
add deb http://security.ubuntu.com/ubuntu bionic-security main
sudo apt update && apt-cache policy libssl1.0-dev
sudo apt-get install libssl1.0-dev

```

### Install ruby using RVM

```
rvm install ruby-{ruby_version}
```

### Install Postgres

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql

sudo apt-get install libpq-dev # External postgres library
```

:tada: That's it 
*Welcome to ruby world*

