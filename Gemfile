source :rubygems
#source 'http://rubygems.torquebox.org'


gem 'bundler'
gem 'rake'
gem "sinatra", "1.1.2"
gem "sinatra-reloader", "0.5.0"
gem "rack-flash", "0.1.1"

# odd, but this is required by monkey-lib which is required by rack session
# cookies, but doesn't get installed unless we're explicit about it
gem "extlib" 

gem 'haml', '~>3.0'
gem 'json_pure'
gem 'state_machine'
gem 'git'

gem 'data_mapper', '~>1.1'
gem 'dm-core', '~>1.1'
gem 'dm-postgres-adapter', '~>1.1'
gem 'dm-migrations', '~>1.1'
gem 'dm-timestamps', '~>1.1'
gem 'dm-observer', '~>1.1'

gem 'torquebox', '1.0.0.CR1'

group :development do
  gem 'thor'
  gem 'jeweler'
end

group :test do
  gem 'rspec'
  gem 'rack-test'
end
