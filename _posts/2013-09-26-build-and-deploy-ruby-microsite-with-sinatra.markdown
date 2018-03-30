---
layout: post
title: "Build and Deploy Ruby Microsite with Sinatra on a Linux VPS"
date: 2013-09-26 17:33
comments: true
categories: 
- ruby
- sinatra
- code
- simplehacks
- tutorial
- linux
---

At some point in your life developing Ruby you're going to have to build a microsite, whether static or dynamic. It may be for a client, a campaign, or your own business. Maybe you just want to learn. The point is, you'll have to do it. Sinatra is the perfect framework for it. I recommend learning Sinatra before Rails anyways because Sinatra forces you to write more Ruby and rely on less magic. Not to mention RoR is overkill for the majority of the sites out there. 

We're going to use the following technologies:

* **[Sinatra](http://www.sinatrarb.com/)** -- Light, lightening fast, and well documented Ruby app framework
* **[Sinatra Assetpack](http://ricostacruz.com/sinatra-assetpack/)** -- Convenient asset handling with awesome features.
* **PostgreSQL** -- Ya. You won't even use it now, but we're going to get you set up in case you decide to add more features than this tutorial.
* **Rspec, Capybara** -- For the test suite this is pretty standard and makes life easy. 
* **slim** -- The most simple and Ruby like HTML markdown language out there. Makes HTML much more readable. 
* **Twitter Bootstrap** -- I can't recommend this enough. These guys rock and save me so much time finding buttons, writing CSS, and JS.
* **Capistrano** -- Capistrano is how we deploy at [codelitt](http://www.codelitt.com). You can write the perfect recipe to make every subsequent deployment a breeze, even over multiple servers. 
* **Thin in development and Passenger in production** Thin is a great and light app server for development and production, but I use Passenger I have Nginx running the version compiled with Passenger already. DRY. 
* **Nginx** -- The most amazing piece of Russion engineering. It blows Apache out of the water in handling requests, speed, and static assets. 
* **Ubuntu Server 12.04** -- All this technology is running on Ubuntu 12.04 Server Edition LTS. I know I'm usually a huge [Arch Linux advocate](http://codylittlewood.com/blog/categories/arch-linux/), but frankly Ubuntu what I use on a server because of stability. Currently looking at Debian for my next server though. 

Alright so Sinatra has no generators so you'll need to create the files and directories. You'll want an app directory for assets, db directory for migrations, and a views directory for the views. I'm calling my app codelitt so before anything get your app directory made and inside of it the other mentioned directories. 

`mkdir codelitt`

`cd codelitt`

`mkdir app db views`

You'll also need your basic files made. Make a file with the same name as your main app directory. This will be the name of your app. The majority of the model and controller logic will go in this file. It's going to be a large piece of your application. 

`touch codelitt.rb`

We're using Bundler to handle our gems and dependencies so:

`touch Gemfile`

Rake file for Rake tasks:

`touch Rakefile`

In our Gemfile we're going to put the basic gems which we'll need to get things fired up. 

And a settings file where you'll enter information for your database connections. 

`touch settings.rb`

You may have noticed that this is a bit more built out than a very simple, single-file Sinatra app. You could do the same things in a single file app, but this gets really messy. I prefer to build it in a more modulular approach.

For your environment you'll want to setup a rvmrc file in the application directory. This ensures that RVM pairs your app with the right versions of your gems and Ruby. 

'touch .rvmrc'

We're also going to want to create the gemset.

`rvm gemset create codelitt`

Now edit the .rvmrc file when you cd into your application directory it always uses the same version of gems and Ruby as are installed by bundle in your app. 

`vim .rvmrc`

Inside this file you're going to enter this with the name of your gemset/ruby version instead of mine:

`rvm use 2.0.0-p247@codelitt` 

Now we'll initiate a git repo to handle the version control of our development process.

`git init`

Create a new repo on Github. If you are lost, check out these directions for [creating a new repo](https://help.github.com/articles/create-a-repo).

`git add --all`

`git commit -am "First commit with basic files"`

`git remote add origin git@github.com:codelitt/codelitt.git`

`git push origin master`

Now we're going to get our basic gems setup so our app fires up for the first time.

`vim Gemfile`

{% codeblock lang:rb Gemfile %}
source 'https://rubygems.org'

gem 'sinatra', '~>1.4.3', :require => 'sinatra/base'
gem 'sinatra-activerecord', '~> 1.2.3'
gem 'pg', '~> 0.16.0'
gem 'sinatra-assetpack', '~> 0.3.1', :require => 'sinatra/assetpack'
gem 'slim', '~> 2.0.1'

group :development do
  gem 'thin', '~> 1.5.1'
end

group :test do
  gem 'rspec', '~> 2.14.1'
  gem 'capybara', '~> 2.1.0'
end
{% endcodeblock %}

We'll create and setup a config.ru file to work with bundler so we can install and keep these gems updated easily. 

`vim config.ru`

{% codeblock lang:rb config.ru %}

require 'rubygems'
require 'bundler/setup'
Bundler.require

root_dir = File.dirname(__FILE__)
app_file = File.join(root_dir, 'codelitt.rb')
require app_file

run Codelitt

{% endcodeblock %}

Finally we'll require the basic files needed and write our first route to our main app file so we can fire it up!

`vim codelitt.rb`

{% codeblock codelitt.rb %}

require 'rubygems'
require 'sinatra'

class Codelitt < Sinatra::Base
  
  get '/' do
    "Hello world!"
  end
 
run! if app_file == $0 
end

{% endcodeblock %}

Now let's install all the gems before trying it out locally.

`bundle install'

Start your local server (which in development we have used Thin), by running

`ruby codelitt.rb` 

And then pointing your browser to http://localhost4567 

Everytime you change your app file you'll have to restart your server with ctrl + c and then running that command again, but for css and view file changes you can just reload the page in the browser. 

For styling, I really can't recommend Twitter Bootstrap enough. It makes life much easier, especially for those of us who like to focus on the backend. 

Download [Twitter Bootstrap here](https://github.com/twbs/bootstrap/archive/v3.0.0.zip)

Now create the css, image, and js directories in your app directory. 

`mkdir -p app/css`

`mkdir -p app/js`

`mkdir -p app/image`

Unzip the bootstrap archive you just downloaded and copy the bootstrap files into these directories. 

` cp ~/bootstrap/css/bootstrap.min.css ~/code/codelitt/app/css/bootstrap.min.css`

` cp ~/bootstrap/css/bootstrap-theme.min.css ~/code/codelitt/app/css/bootstrap-theme.min.css`

` cp ~/bootstrap/js/bootstrap.min.js ~/code/codelitt/app/js/bootstrap.min.js`

` cp ~/bootstrap/fonts/glyphicons-halflings-regular.svg  ~/code/codelitt/app/image/glyphicons-halflings-regular.svg`

We'll also create the theme.css file which we can use to edit parts of our site that aren't taken care of by Bootstrap. 

` touch app/css/theme.csss `


Now we'll setup sinatra-asset pack to pick these up. You already have it in your Gemfile. You'll just need to alter a few things in your main app file to make sure it works. You'll add this to your main app file. Note that we also are now requiring the assetpack and slim gems in our main app file. 

{% codeblock codelitt.rb %} 
require 'rubygems'
require 'sinatra'
require 'sinatra/base'
require 'sinatra/assetpack'
require 'slim'

class UberUrlShortener < Sinatra::Base
  set :root, File.dirname(__FILE__)

  register Sinatra::AssetPack

  enable :inline_templates

  assets do
    serve '/js',     from: 'app/js'        # Default
    serve '/css',    from: 'app/css'       # Default
    serve '/image', from: 'app/image'    # Default

    css :cssapp, [
      # '/css/bootstrap.css', # bootstrap.less
      '/css/*.css'
    ]

    js :jsapp, [
     '/js/*.js'
    ]

    css_compression :simple
    js_compression :jsmin
  end

........

{% endcodeblock %}

Now we are going to create our home page. We're also  going to create our basic layout file which will be consistent across the site.

`touch views/layout.slim`

`touch views/index.slim`

The slim extension is what you'll use if you went ahead and added slim to your Gemfile. It's the templating engine we'll use in this tutorial. 

In the layout.slim we're going to include all of the basic information of a normal page and then you'll notice in the main diff that we pull in the specific page with yield. 

```
doctype html
html
  head
    title codelitt technology innovation
    meta name="keywords" content="codelitt, technology innovation, technology, technologist, consulting, web, mobile, apps, custom operating systems"
    meta name="author" content="Cody Littlewood"
    meta name="description" content="codelitt helps companies innovate like startups and works with them to prototype and build new technology. Our specialty is in emerging technologies and media."
    meta name="viewport" content="width=device-width, initial-scale=1.0"
    link rel="icon" type="image/ico" href="favicon.ico"
    /This next line adds the routes for css from assetpack
    == css :cssapp
  header
    nav class="navbar navbar-default navbar-fixed-top" role="navigation"
      div class="container"
        div class="nav-header"
          div class="navbar-collapse collapse" 
            button type="button" class="navbar-toggle" data-toggle="collapse" data-target="navbar-ex1-collapse"
              span class="sr-only"
              span class="icon-bar"
              span class="icon-bar"
              span class="icon-bar"
            a class="navbar-brand" href="/" codelitt
            ul class="nav navbar-nav navbar-right"
              li
                a href="/" home
              li
                a href="/technologies" technologies
              li
                a href="/contact" contact
  
  body
    div class="main"
      == yield
      ==js :jsapp
  
  footer
```
Now we'll add basic content to our index.slim file as a placeholder. 

{% codeblock lang:rb index.slim %}
  h1 Hello world!

{% endcodeblock %}

In your main app file you're going to change your route to recognise this change. Going forward you'll use this basic format to add any new pages/routes that you'd like. 

{% codeblock codelitt.rb %}

.....

class Codelitt < Sinatra::Base

....
# goes at the end of the class before you close it with "end"
  get '/' do
    slim :index
  end
end
......
{% endcodeblock %}

Now I'm going to assume that you already have Ubuntu, Nginx, and Passenger already set up. If you don't then follow this [guide](https://www.digitalocean.com/community/articles/how-to-install-rails-and-nginx-with-passenger-on-ubuntu) except don't worry about installing the rails gem if you don't want. 

We're going to deploy with Capistrano which is a sweet little diddy that I use for all of my deployments. It automates things. You just need the right recipe. First we'll add the two gem's we'll need to our Gemfile

{% codeblock Gemfile %}

gem 'capistrano', '~> 2.15.5'
gem 'rvm-capistrano', '~> 1.3.3'


{% endcodeblock %} 

Run `bundle install`

And then `capify .`

This will build the main files you need. 

You'll want to edit your config.ru file so it looks like this for my capistrano recipe.

{% codeblock lang:ru config.ru %}

rure 'rubygems'
require 'bundler/setup'
Bundler.require

root_dir = File.dirname(__FILE__)
app_file = File.join(root_dir, 'codelitt.rb')
require app_file

set :environment, ENV['RACK_ENV'].to_sym
set :root,        root_dir
set :app_file,    app_file
disable :run

run Codelitt


{% endcodeblock %}

Here is the Capistrano recipe for a near zero downtime deploy. Change the variables for your own app. 

{% codeblock config/deploy.rb %}

require 'rvm/capistrano'
require 'bundler/capistrano'

#RVM and bundler settings
set :bundle_cmd, "/home/deploy/.rvm/gems/ruby-2.0.0-p247@global/bin/bundle"
set :bundle_dir, "/home/deploy/.rvm/gems/ruby-2.0.0-p247/gems"
set :rvm_ruby_string, :local
set :rack_env, :production

#general info
set :user, 'deploy'
set :domain, 'www.codelitt.com'
set :applicationdir, "/var/www/codelitt"
set :scm, 'git'
set :application, "codelitt"
set :repository,  "git@github.com:codelitt/codelitt"
set :branch, 'master'
set :git_shallow_clone, 1
set :scm_verbose, true
set :deploy_via, :remote_cache

role :web, domain                          # Your HTTP server, Apache/etc
role :app, domain                          # This may be the same as your `Web` server
role :db,  domain, :primary => true # This is where Rails migrations will run
#role :db,  "your slave db-server here"
#deploy config
set :deploy_to, applicationdir
set :deploy_via, :export

#addition settings. mostly ssh
ssh_options[:forward_agent] = true
ssh_options[:keys] = [File.join(ENV["HOME"], ".ssh", "id_rsa")]
ssh_options[:paranoid] = false
default_run_options[:pty] = true
# if you want to clean up old releases on each deploy uncomment this:
# after "deploy:restart", "deploy:cleanup"

# if you're still using the script/reaper helper you will need
# these http://github.com/rails/irs_process_scripts

# After an initial (cold) deploy, symlink the app and restart nginx
after "deploy:cold" do
  admin.nginx_restart
end

# As this isn't a rails app, we don't start and stop the app invidually
namespace :deploy do
  desc "Not starting as we're running passenger."
  task :start do
  end

{% endcodeblock %}

Now ssh into your deploy server and make a directory where you keep your websites called your app name. 

`mkdir codelitt`

Next you'll need to add the site to your sites-available and then make a symlink to sites-enabled.

`touch /etc/nginx/sites-available/codelitt`

{% codeblock codelitt %}
server {
        listen 80;
        server_name codelitt.com www.codelitt.com;
        root /var/www/codelitt/current/public;
        passenger_enabled on;
}

{% endcodeblock %}

`sudo ln -nfs /etc/nginx/sites-available/codelitt /etc/nginx/sites-enabled/codelitt`

We're pulling from the git repository so commit and push your final changes. 

`git add --all `

`git commit -am "final changes before deploy"`

`git push origin master`

Now you can deploy your site. The script should automatically restart Nginx for you. 

` cap deploy `

That's it! You're all done. 
