# StompBox : A Deployer for TorqueBox

StompBox is a simple Sinatra app that can be used to manage deployments on
[TorqueBox][torquebox] by accepting commit notifications from [GitHub][github].
It provides a user interface for managing one-click deployment and undeployment
of your github repositories to TorqueBox for specific branches and commit
points.

## Why?

StompBox is useful for testing and development environments where your code is
changing frequently and you want to quickly deploy working branches and staging
instances.  It integrates with your [GitHub][github] repositories to enable
extremely quick and simple app deployment of any Rack-based application.  And
it does all this on top of the industrial-strength TorqueBox platform,
automatically providing your application with enterprise-level functionality
such as messaging, scheduling, tasks, clustering and more.

## Installation

If you don't already have a TorqueBox instance installed, you should do that
first.  Head on over to the [TorqueBox][torquebox] web site and follow the
detailed instructions for downloading, installing, configuring and running
TorqueBox.  Don't worry - it's easy. 

StompBox hasn't been tested on Windows and may not work in that environment.
However, if you are running a Unix-y system such as Fedora or Mac OSX, then you
should be fine.  

Once you've installed TorqueBox, clone the StompBox repository, install the
gems, and deploy it to your running TorqueBox instance.

    $ git clone git@github.com:lance/stompbox.git
    Cloning into stompbox...
    remote: Counting objects: 187, done.
    remote: Compressing objects: 100% (168/168), done.
    remote: Total 187 (delta 108), reused 0 (delta 0)
    Receiving objects: 100% (187/187), 22.96 KiB, done.
    Resolving deltas: 100% (108/108), done.

    $ cd stompbox && bundle install
    <output clipped for brevity - you should see this at the end>
    Your bundle is complete! It was installed into /path/to/gem/home
    
    $ rake torquebox:deploy
    
## Configuration

Configuration options are found in config/torquebox.yml.  Here are some things
you'll want to pay attention to.

* **DATABASE_URL** Provide a URI for connecting to your database.  By convention,
  StompBox uses a database called "stompbox", but you can change this to
  whatever you want as long as it is supported by DataMapper. Be sure the user
  specified in the connection URI has appropriate privileges for executing DDL
  on your database. When the app starts for the first time, the tables will be
  created and it will fail if your user doesn't have the proper permissions.

* **AUTO_MIGRATE** The first time you run StompBox, you'll want to have this on
  so that your database is automagically created.  After that, it's most efficient
  to just unset this option by commenting it out.

* **DEPLOYMENTS** When you deploy an application using StompBox, it clones the
  git repository into this directory. Make sure that your TorqueBox instance is
  running as a user with enough privileges to write to this directory.  

* **REQUIRE_AUTHENTICATION** To turn authentication on, set this to any non-nil
  value.  StompBox uses the built-in JAAS authentication provided by TorqueBox and
  JBossAS.  See the **Authentication** section below for more information.

* **API_KEY** This should be set to something unguessable like an SHA1 hash.
  You can generate these any number of ways. 
  
Here's one one way to generate a key.

    $ echo "Now is the winter of our discontent made glorious summer by this son of York" | openssl sha1
    
    (stdin)= e4ba3556d1d059e2eadca9488b093d6685657e00

## Authentication

StompBox uses the built-in JAAS authentication provided by JBoss and exposed
via TorqueBox.  If this is a development system (it probably should be), the
simplest way to set usernames and passwords is to use the builtin rake task.

    $ rake torquebox:auth:adduser username:password

Be sure to set REQUIRE_AUTHENTICATION in config/torquebox.yml as well.
Voilá, authentication is enabled.

TorqueBox (and subsequently StompBox) can also authenticate with any other
realm you have configured in your JBossAS. If you have configured JAAS settings
in your TorqueBox instance, you may authenticate against them by supplying
the authentication realm in config/torquebox.yml as shown.

    auth:
      default:
        domain: my-configured-jaas-realm
    
## Usage

Start by telling StompBox what repositories and branches you want to track.
Once you've got everything configured and deployed, you can start by telling
StompBox what repositories and branches you want to track. Then, from
[GitHub][github], browse to the repository admin screen for one of the
repositories you specified in `config/stompbox.yml`. Select "Service Hooks" ->
"Post Receive URLs" and enter your StompBox URL plus your api_key (you
configured it didn't you?).  It should look something like this.

    http://mydomain.com/stompbox/push/3821A95D456134214FAD6FA91A2BAFE574D47151
    
After you save your settings, testing the service hook should send a POST
request to your StompBox.  Play around. 

## Caveats

This project is very immature and there are many features which are outstanding (such as any real authentication).  You should **not** use it in a production environment.  It is currently used for research, development and testing only.  

## License

This software is distributed under an [MIT software license][license].

[torquebox]: http://torquebox.org "TorqueBox"
[github]: https://github.com "GitHub"
[license]: 'LICENSE.txt' "MIT License"
