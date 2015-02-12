# Drupal VM for BBC Store

It will install the following on an Ubuntu 14.04 linux VM:

  - Apache 2.4.x
  - PHP 5.5.x (configurable)
  - MySQL 5.5.x
  - Drush latest release (configurable)
  - Drupal 7.34
  - Optional (installed by default):
    - Memcached
    - XHProf, for profiling your code
    - XDebug, for debugging your code
    - PHPMyAdmin, for accessing databases directly
    - Solr

## Customizing the VM

There are a couple places where you can customize the VM for your needs:

  - `config.yml`: Contains variables like the VM domain name and IP address, PHP and MySQL configuration, etc.
  - `drupal.make.yml`: Contains configuration for the Drupal core version, modules, and patches that will be downloaded on Drupal's initial installation (more about [Drush make files](https://www.drupal.org/node/1432374)).

## Quickstart

### 1 - Install dependencies (VirtualBox, Vagrant, Ansible)

  1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html).
  3. [Mac/Linux only] Install [Ansible](http://docs.ansible.com/intro_installation.html).

Note for Windows users: *Ansible will be installed inside the VM, and everything will be configured internally (unlike on Mac/Linux hosts). See [JJG-Ansible-Windows](https://github.com/geerlingguy/JJG-Ansible-Windows) for more information.*

### 2 - Build the Virtual Machine

  1. Download this project and put it wherever you want.
  2. Clone the BBC Store codebase
  3. Open Terminal, cd to this directory (containing the `Vagrantfile` and this README file).
  4. Install Ansible Galaxy roles required for this VM: `$ sudo ansible-galaxy install -r requirements.txt`
  5. Get hold of a database dump and place it in the provisioning folder with a filename `dump.sql.gz`
  6. Edit `local.config.yml` and:
     *  set the `vagrant_ip` to a unique IP address for your network
     *  set the `local_path` to the location of the BBC Store docroot directory
  6. Run `vagrant up`, and let Vagrant do its magic.

Note: *If there are any errors during the course of running `vagrant up`, and it drops you back to your command prompt, just run `vagrant provision` to continue building the VM from where you left off. If there are still errors after doing this a few times, post an issue to this project's issue queue on GitHub with the error.*

### 3 - Configure your host machine to access the VM.

  1. [Edit your hosts file](http://www.rackspace.com/knowledge_center/article/how-do-i-modify-my-hosts-file), and point the static ip you assigned to vagrant_ip to local.store.bbc.com.
  2. Open your browser and access [http://local.store.bbc.com](http://local.store.bbc.com).

### 4 - Configure solr

  1. Login to the site with an admin account
  2. Navigate to [http://local.store.bbc.com/admin/config/search/search_api](http://local.store.bbc.com/admin/config/search/search_api) and hit the "Add server" button
  3. Server name can be anything
  4. Service class should be "Solr service"
  5. Defaults are fine but set Solr Path to "/solr/bbcstore"
  6. Save.
  7. Navigate back to [http://local.store.bbc.com/admin/config/search/search_api](http://local.store.bbc.com/admin/config/search/search_api)
  8. Click edit next to "BBC Store Index"
  9. Set "Server" to the server name you defined in step 3.
  10. Save.

### 5 - Keep it in sync

Files are kept in sync with a vagrant rsync file share (NFS is supported too but its really slow on Drupal projects). To start sharing run `vagrant rysnc-auto` afer starting the VM.
