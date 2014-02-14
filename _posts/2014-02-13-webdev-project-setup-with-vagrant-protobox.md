---
layout: post
title:  Webdev project setup with Vagrant and Protobox 
date:   2014-02-13 23:30:00
tags:	WebDev, Development, Vagrant, Protobox
assets:
header: 3
comments: true
---

#Damals

Früher habe ich für meine Web-Projekte [XAMPP](http://www.apachefriends.org) (Windows/Linux/OSX) oder [MAMPP](http://www.mamp.info) (OSX) eingesetzt. Dabei handelt es sich um einen Technologiestack (**X** Windows/Linux/OSX **A** Apache **M** MySQL **P** PHP **P** Perl) für die Entwicklung und den Betrieb von Webanwendungen, mit etwas *Infrastructure Sugar* für ein benutzerfreundliches Setup der Entwicklungsumgebung. Der Nachteil dabei ist jedoch, dass das Ganze direkt auf dem eigenen Host läuft, dessen Konfiguration sich mit Sicherheit von der Produktivumgebung unterscheidet. Zudem wäre es schön, für jedes Projekt eine eigene, isolierte Umgebung bereitstellen zu können, um bei allfälligen Konfigurationsänderungen nicht die anderen Projekte abzuschiessen.

#Etwas später

Vor einiger Zeit bin ich dann auf [Vagrant](http://www.vagrantup.com) gestossen, ein Command Line Interface für die Virtualisierungslösung [VirtualBox](https://www.virtualbox.org), ausgestattet mit zahlreichen Features für das automatische Setup und Provisioning von isolierten Entwicklungsumgebungen (Virtual Machines). Vagrant setzt [VirtualBox](https://www.virtualbox.org) voraus, die Installation ist simpel und ein erster Test mit `vagrant init` und `vagrant up` ist schnell vollzogen. Allerdings empfand ich das provisioning mittels Puppet etwas umständlich, sodass ich nicht allzu viel Zeit dafür investierte und mein Basis-Setup (Apache, MySQL, Git, Shared Folders, etc.) so lange als möglich verwendete. 

#Heute

Wie so oft kommt aber irgendwann der Zeitpunkt, an dem das Alte nicht mehr den Anforderungen genügt. In diesem Fall waren für ein anstehendes Projekt diverse Konfigurationsanpassungen nötig und ich hatte keine Lust, mich nochmals mit der Puppet-Thematik zu beschäftigen, was mich zum Schluss brachte, mich nach einer Alternative umzusehen. 

##Protobox

Zugegeben, zuerst wusste ich nicht so recht, wo ich [Protobox](http://getprotobox.com) einordnen sollte. Nach ein paar Minuten auf der Website jedoch war klar, dass ich mir das genauer anschauen sollte. Bei Protobox handelt es sich eigentlich nur um einen weiteren Layer über Vagrant, welcher einem einen Grossteil des Basis-Setups bereitstellt. Zudem lässt sich über ein Webinterface die nötige provisioning Konfiguration zusammenstellen.

###Installation

Die Installation ist ziemlich straightforward, unter Windows genügt es, ein clone des Github Repositories zu ziehen und das Konfigurationsfile anzupassen.

{% highlight bash %}
git clone git@github.com:protobox/protobox.git protobox
cd protobox && cp data/config/common.yml-dist data/config/common.yml
{% endhighlight %}

anschliessend kann mit `vagrant up` die VirtualBox gestartet werden und das Dashboard über `http://192.168.5.10` aufgerufen werden.

***Hinweis:*** Falls unter Windows der Fehler "/vagrant/ansible/shell/os-detect.sh: line 2: $'\r': command not found" auftritt, hängt dies mit den *line endings* zusammen, der Fehler mit Lösung ist hier beschrieben: [http://getprotobox.com/docs/issues/windows_shell](http://getprotobox.com/docs/issues/windows_shell).

###Konfiguration

Nun möchte man ja aber eine auf das Projekt zugeschnittene Konfiguration erstellen. Mit Protonet ist dies ein Leichtes. Über das elegante [Webinterface](http://getprotobox.com) können die nötigen Anpassungen vorgenommen, und zum Schluss das Konfigurationsfile heruntergeladen werden. Dies wird anschliessend unter `protobox/data/config/common.yml` gespeichert.

{% highlight bash %}
#
# Default configuration (common.yml)
#
---
protobox:
  version: 0.0.1
  document: abc123
  dashboard:
    install: 1
    path: /srv/www/web/protobox

vagrant:
  vm:
    box: ubuntu-precise12042-x64-vbox43
    box_url: 'http://files.vagrantup.com/precise64.box'
    hostname: protobox
    network:
      private_network: 192.168.5.10
      forwarded_port:
        web:
          host: ''
          guest: ''
    provider:
      virtualbox:
        modifyvm:
          name: protobox
          natdnshostresolver1: 'on'
          memory: '1024'
        setextradata:
          VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root: 1
      #parallels:
      #  name: protobox
      #  set:
      #    memsize: '1024'
    provision:
      ansible:
        playbook: default
        #hosts: "webservers"
        #inventory: "ansible/inventory"
        #verbose: "yes"
        #extra_vars: 
        #  ntp_server: "pool.ntp.org"
        #  nginx_workers: 4'
    synced_folder:
      root:
        id: vagrant-root
        source: ./
        target: /srv/www
        nfs: false
        owner: vagrant
        group: www-data
        mount_options: 
          - 'dmode=775'
          - 'fmode=775'
    usable_port_range: 2200..2250
  ssh:
    host: null
    port: null
    private_key_path: null
    public_key_path: null
    username: vagrant
    guest_port: null
    keep_alive: true
    forward_agent: false
    forward_x11: false
    shell: 'bash -l'
  vagrant:
    host: ':detect'

server:
  packages:
    - vim
  ssh:
    #authorized_keys are public keys for remote machines
    #authorized_keys:
    #  - name: patrick
    #    #key: '1234'
    #    file: /srv/www/data/ssh/github_id_rsa
    #  - name: test2
    #    #key: '1234'
    #    file: /srv/www/data/ssh/github_id_rsa
    #keys are private keys to add to the machine
    #private_keys:
    #  - name: patrick
    #    file: /srv/www/data/ssh/github_id_rsa
    #  - name: patrick
    #    entry: 'test\n\ttest2'
    #ssh config entries
    #config:
    #  - name: github
    #    file: /srv/www/data/ssh/git_config
    #  - name: github
    #    entry: 'host *\n\tIdentityFile ~/.ssh/github_id_rsa'
  dotfiles:
    install: 0
    repo: git@github.com:patrickheeney/dotfiles.git
    #files:
    #  - /srv/www/data/dot/file
    #bash_aliases: null

apache:
  install: 1
  modules:
    - rewrite
  user: vagrant
  group: www-data
  default_vhost: false
  mpm_module: prefork
  vhosts:
    - name: app
      servername: app.dev
      serveraliases:
        - 'www.app.dev'
      docroot: /srv/www/web/protobox
      port: '80'
      setenv:
        - 'APP_ENV dev'
      override:
        - All

nginx:
  install: 0
  mpm_module: fpm
  vhosts:
    - name: app
      server_name: app.dev
      server_aliases:
        - www.app.dev
      www_root: /srv/www/web/protobox
      listen_port: '80'
      index_files:
        - index.html
        - index.htm
        - index.php
      envvars:
        - 'APP_ENV dev'

varnish:
  install: 0

hhvm:
  install: 0
  nightly: 0
  composer:
    install: 1
    use_hhvm: 1

php:
  install: 1
  version: '54'
  modules:
    - php5-cli
    - php5-intl
    - php5-mcrypt
    - php5-curl
    - php5-gd
  pear:
    install: 1
    modules: [ ]
  pecl:
    install: 1
    modules: [ ]
  apc:
    install: 1
  composer: 
    install: 1
  mailcatcher: 
    install: 0
  phpmyadmin:
    install: 0
  xdebug:
    install: 1
    webgrind: 1
    settings:
      default_enable: '1'
      remote_autostart: '0'
      remote_connect_back: '1'
      remote_enable: '1'
      remote_handler: dbgp
      remote_port: '9000'
  xhprof:
    install: 0
    xhgui: 1
  ini:
    display_errors: 'On'
    display_startup_errors: 'On'
    error_reporting: '-1'
    short_open_tag: 'On'
  timezone: America/Chicago

phalcon:
  install: 0

mysql:
  install: 1
  root_password: 'root'
  databases:
    - name: app
      host: localhost
      user: user
      password: user
      grant:
        - ALL
      #sql_file: '/srv/www/data/sql/app.sql'
      sql_file: ''

postgresql:
  install: 0
  root_password: 'root'
  user: postgres
  group: postgres
  databases:
    - name: app
      user: root
      password: root
      grant: 
        - CREATEDB
        - SUPERUSER
      sql_file: ''

mongodb:
  install: 0
  root_password: 'root'

redis:
  install: 0
  conf_port: 6379
  conf_bind: 127.0.0.1

riak:
  install: 0

mariadb:
  install: 0
  version: '5.5'
  #repository: ''
  root_password: 'root'
  databases:
    - name: app
      host: localhost
      user: user
      password: user
      grant:
        - ALL
      sql_file: ''

solr:
  install: 0

elasticsearch:
  install: 0

beanstalkd:
  install: 0
  listen_addr: '127.0.0.1'
  listen_port: '11300'

rabbitmq:
  install: 0

node:
  install: 0
  npm:
    - grunt
    - grunt-cli
    - bower

ruby:
  install: 0
  gems:
    - sass
    - compass

newrelic:
  install: 0
  license: ''
  php: 1
  node: 1

ngrok:
  install: 0
  port: 80
  #subdomain: protoboxapp
  #httpauth: 'user:password'
  #proto: 'tcp 22'
  #client: 'client1 client2 client3'
  #hostname: 'your.domain.com'
  #tunnels:
  #  client:
  #    httpauth: 'user:password'
  #    proto: 'https 8080'
  #  ssh:
  #    proto: 'tcp: 22'

applications:
  install: 1
  # ...
{% endhighlight %}

###Run

Nun kann die Box mittels `vagrant up` gestartet werden. Der erste Start dauert dabei etwas länger, da das ganze Provisioning durchgeführt wird.

###Zugriff

Je nach Konfiguration des Apache VHOSTS muss nun lediglich noch ein Host-Eintrag erfasst werden

{% highlight bash %}
192.168.5.10	app.dev	www.app.dev
{% endhighlight %}

damit der Zugriff über `http://app.dev` resp. `www.app.dev` erfolgen kann.

#Fazit

Protobox gefällt mir sehr gut. Das Setup ist einfach und schnell, die Konfiguration über das Webinterface sehr benutzerfreundlich sodass ich das Ganze sicherlich für die nächsten Projekte einsetzen werde.

#Quellen & Links

- [VirtualBox](https://www.virtualbox.org)
- [Vagrant](http://www.vagrantup.com)
- [Protobox](http://getprotobox.com)