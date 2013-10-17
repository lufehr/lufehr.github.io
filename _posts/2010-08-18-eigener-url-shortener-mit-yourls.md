---
layout: post
title:  Eigener URL Shortener mit YOURLS
date:   2010-08-18 14:00:00
tags:   URL-Shortener, YOURLS
assets: 2010-08-18-eigener-url-shortener-mit-yourls
header: 3
comments: true
---

Sie sind cool und aus den social Networks (Twitter, etc.) nicht mehr wegzudenken. Aber was sind URL Shortener? Ein URL Shortener macht seinem Namen alle Ehre und “verkürzt” URLs wie zum Beispiel <kbd>http://www.mydomain.com/eStore/Products?Category=Baseball&..</kbd> zu <kbd>http://url.mydomain.com/a12b3</kbd> o. ä. Mal abgesehen von den esthetischen Gründen kann somit viel Platz eingespart werden (Twitter – 140 Zeichen) und die URLs sind einfacher zu lesen. In diesem kurzen HowTo wird beschrieben, wie man seinen eigenen URL Shortener mit YOURLS aufsetzt.

#Vorbereitung

Domain: Subdomain (url.mydomain.com) oder Subfolder(mydomain.com/url)
Download: Download YOURLS von http://yourls.org
Entpacken: ZIP-Archiv in wwwroot entpacken

#Installation

Als Erstes muss im <kbd>includes/</kbd> Verzeichnis das File config-sample.php zu config.php umbenannt werden. Danach müssen die folgenden Zeilen angepasst werden.

{% highlight php %}
/* MySQL database username */
define('YOURLS_DB_USER', 'myDBUser');
 
/* MySQL database password */
define('YOURLS_DB_PASS', 'myDBPassword');
 
/* The name of the database for YOURLS */
define('YOURLS_DB_NAME', 'myDBName');
 
/* MySQL hostname */
define('YOURLS_DB_HOST', 'localhost');
 
/* MySQL tables prefix */
define('YOURLS_DB_PREFIX', 'yourls_');
 
/* YOURLS installation URL, no trailing slash */
define('YOURLS_SITE', 'http://url.mydomain.com');
 
/*  Username(s) and password(s) allowed to access the site */
$yourls_user_passwords = array(
    'myUsername' => 'myPassword',
    'username2' => 'password2'    // You can have one or more 'login'=>'password' lines
);
{% endhighlight %}

Als nächstes kann die definierte URL mit <kbd>/admin/install.php</kbd> erweitert aufgerufen werden (z.B.http://url.mydomain.com/admin/install.php). Das Install-Script kann nun mit Install YOURLS gestartet werden.

![]({{ site.url }}/assets/{{page.assets}}/eigener-url-shortener-mit-yourls-1.png)

Nach Abschluss erscheint eine Bestätigung und man kann sich an der Administration Page mit den zuvor in config.php definierten Zugangsdaten anmelden.

![]({{ site.url }}/assets/{{page.assets}}/eigener-url-shortener-mit-yourls-2.png)

![]({{ site.url }}/assets/{{page.assets}}/eigener-url-shortener-mit-yourls-3.png)

Die Administration Page ist auch über die URL http://url.mydomain.com/admin erreichbar. Nun hat man die Möglichkeit einzelne URL “zu shorten” in dem man die URL ins Textfeld Enter the URL einträgt und mit Shorten The URL bestätigt. Dabei kann das System aufgefordert werden, automatisch eine Short URL zu generieren, oder man trägt unter Custom short URL die gewünschte Zeichenfolge ein.

![]({{ site.url }}/assets/{{page.assets}}/eigener-url-shortener-mit-yourls-4.png)

Der Eintrag erscheint danach in der untenstehenden Liste und kann jederzeit wieder editiert oder gelöscht werden.

![]({{ site.url }}/assets/{{page.assets}}/eigener-url-shortener-mit-yourls-5.png)

Nun möchte man natürlich nicht jede einzelne URL von Hand in die Liste eintragen, die Menschheit ist ja faul :), hierfür sollte man einfach mal kurz einen Blick ins API werfen.

Ein Folge-Post zum YOURLS WordPress Plugin und zur Verwendung von YOURLS mit Twitter for iPhone folgen.

Have Fun shorting some URLs!