# Vagrant WordPress Environment
Vagrant allows you to completely encapsulate your development environment inside a virtual machine that you can build and tear down on-demand. Vagrant is better than a normal Virtual Machine because you can programmatically build out your requirements so everything is setup exactly how you want it every time.

##  Getting Started
Before starting your development, it's important that you read through the following documentation so that you can familiarize yourself with Vagrant and the installation process.

### Requirements
[VirtualBox](https://www.virtualbox.org/wiki/Downloads) - a general-purpose full virtualizer for x86 hardware, targeted at server, desktop and embedded use.
[Vagrant](https://www.vagrantup.com/downloads.html) - allows you to create and configure lightweight, reproducible, and portable development environments.

### Step-By-Step Installation
1. In order to connect to your virtual host from your browser, you'll need to add an entry to your hosts file. On Windows you can locate it at: `C:\Windows\System32\drivers\etc\hosts`. If you're on a Mac you can locate it at: `/etc/hosts`. Once you've found your hosts file, open it up and add the following to the bottom of the document: `192.168.56.101 wordpress.dev www.wordpress.dev`. This tells your computer to route "wordpress.dev" to the IP Address where vagrant is installed.
2. If you're on a Mac, you'll want to `sudo vim /etc/hosts` and if you're on Windows, you'll want to open `C:\Windows\System32\drivers\hosts` in your editor of choice. Once you've opened the file, add the following to the end of your file: `192.168.56.101 wordpress.dev www.wordpress.dev`. This tells your computer to point `wordpress.dev` to `192.168.56.101` which is the IP Address that Vagrant is listening to.
3. Open your project directory in the console and run `git clone <repository> vagrant-wordpress`.
4. Once the download is complete, run `cd vagrant-wordpress` to enter into your vagrant project.
5. Run the `vagrant up` command and wait for the installation process to complete. The very first time you install Vagrant puppet will download the virtual machine and save it on your hard drive, so it may take a while. You'll know the process is complete when your prompt re-appears.
6. If you haven't gotten a bunch of red text right above your prompt stating that something went terribly wrong, you've probably installed Vagrant successfully. During the install process it's normal to see some error messages - you can just ignore those (unless something actually broke).
7. Now that you've installed Vagrant, you'll want to test that it actually worked. To do so, run `vagrant ssh` - this will log you into the Virtual Box environment. If your prompt changes to "vagrant@packer-virtualbox-iso", you've done everything correctly!
8. The next step is to setup your computer so that [wordpress.dev](http://wordpress.dev) points you to our WordPress environment. To do so, you'll need to exit the Vagrant environment by running `exit`. You should then see your prompt you're accustomed to seeing.
9. After saving your hosts file config, open a new browser window and go to: [wordpress.dev](http://wordpress.dev). If everything was configured correctly you should see our WordPress site!

###  Help!
If you're unable to install Vagrant for any reason, the first step is to make sure you've followed the directions exactly. If you've done everything above and still can't get Vagrant working, try searching for a solution via [Google](http://google.com), [StackOverflow](http://stackoverflow.com/), or the [Puphpet GitHub account](https://github.com/puphpet/puphpet/issues).

## Environment Setup
This project was generated using [Puphpet](https://puphpet.com/), an online GUI for building reusable and maintainable vagrant environments. A major benefit of using Puphpet is that all our configuration settings are located in a single file: `puphpet/config.yaml`. Unless you know what you're doing, you shouldn't touch anything outside the www directory.

### Vagrant Directory
The vagrant directory is synced to our Vagrant environment so any files or folders within this folder are automatically created and updated in our environment. To access these files inside of vagrant, simply run `cd /var/www`.

### vagrant/wordpress
This directory is where all our WordPress files live. Apache is setup to use this directory as the webroot for wordpress.dev so anything you place in this directory is accessible via the browser.

### vagrant/wordpress/wp-content
As you probably know already, WordPress stores all its themes, plugins, and uploads inside the wp-content directory so you're probably not expecting these folders to be empty, but they are. This project explicitly tells Git to ignore the contents of the themes, plugins, and uploads directories because we want to separate themes and plugins into their own repositories (where applicable) and we don't need to version-control uploads.

Feel free to populate these directories as necessary, just don't expect them to be saved into this repository. Additionally, because we're using separate repositories for our plugins and themes, you'll need to make sure that you double-check your commits to make sure you're in the right project. This really shouldn't be an issue though if you've setup [SourceTree](http://www.sourcetreeapp.com/) properly.

### vagrant/mysql
The mysql directory contains a file call wordpress.sql by default. Vagrant uses this file to create and populate our initial WordPress database.

This folder is also generally a good place to keep MySQL bootstrap files for various projects. Bootstrap files should be created for each project and kept up-to-date as development progresses. These files are meant to help developers test out their environment, not keep your database in sync with other environments so they should remain small and contain only the essentials.

## WordPress
This environment is configured to automatically install and configure WordPress Multisite using sub-directories. This allows you the developer to work on multiple WordPress projects inside a single environment. It also allows you to replicate the same functionality that you would find in our Amazon WordPress environments.

###  Wordpress Credentials
You can access the WordPress Admin by opening up http://wordpress.dev/wp-admin/ in your browser and using the username and password below.
- **Username**: admin
- **Password**: password

### MySQL Credentials
This environment does not have phpMyAdmin installed by default so you'll need to connect to the database using a GUI. If you're on a Mac you can use [Sequel Pro](http://www.sequelpro.com/) or [MySQL WorkBench](http://dev.mysql.com/downloads/workbench/). If you're on Windows you should use [MySQL Workbench](http://dev.mysql.com/downloads/workbench/).

#### SSH Connection Info
- **Host**: wordpress.dev
- **Username**: vagrant
- **Key (OS X)**: ~/.vagrant.d/insecure_private_key
- **Key (Windows)**: C:\Users\<username>\.vagrant.d\insecure_private_key
- **Port:** 22

#### MySQL Connection Info
- **Host:** 127.0.0.1
- **Username:** wordpress
- **Password:** password
- **Database:** wpmultisite_dev

### Domain Mapping
Our WordPress Multisite environment relies heavily on the [WordPress MU Domain Mapping Plugin](https://wordpress.org/plugins/wordpress-mu-domain-mapping/) (as do most multisite environments). The plugin allows us to "map" top-level domains (TLDs) such as "<domain>.dev" and "<domain2>.dev" to our WordPress sites. This is incredibly important because by default uses a single TLD for the entire multisite installation.
Because we rely so heavily on the domain mapping plugin it is installed as part of our core environment.

#### Configuration
The Domain Mapping plugin settings are only available from the Network Administration Dashboard which means only Network Admins (i.e. super users) can update/modify the settings. In this development environment there is only one user named "admin" and it is the Network Admin by default.

##### Updating your hosts file
Before you can use the domain mapping plugin you have to add an entry to your "hosts" file. Add a new line below `192.168.56.101 wordpress.dev www.wordpress.dev` with the same IP Address but instead of "wordpress.dev" type-in the URL you want to use as an alias.

##### Activating an existing alias
To enable a domain alias or to create a new alias you'll need to go to `My Sites > Network Admin > Dashboard > Settings > Domains`. On this screen you'll see a list of pre-registered domains. Click the "edit" button next to the domain you want to activate and then click the checkbox that has "Primary" next to it, then click the save button.

##### Creating a new alias
Before you begin you'll need to know the Site ID of the site you want to alias. To find a Site ID got to: `My Sites > Network Admin > Sites` and click the link for the site you want to alias. Once you're redirected to the edit screen you will notice that the URL has a parameter aptly named "id"; this is the Site ID we need. Next go to `My Sites > Network Admin > Dashboard > Settings > Domains`, type in the Site ID, the alias you want to create, click on the "Primary" checkbox and click save.
