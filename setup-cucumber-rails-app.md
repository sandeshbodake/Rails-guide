#### Create rails app with postgreSql

```
rails new sample-rails-app --database=postgresql
```

#### Scoffolding 

```
rails generate scaffold recipe title:string  name:string
```

#### Add Gems

```
group :test do
  gem 'cucumber-rails', require: false
  gem 'database_cleaner'
  gem 'rspec-rails'
end
````

#### Setup test enviroment

```
rails generate cucumber:install — help
```

```
rails generate rspec:install
```

```
rails generate cucumber:install
```

