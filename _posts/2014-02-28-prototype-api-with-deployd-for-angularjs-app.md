---
layout: post
title:  Prototype an API with Deployd for an AngularJS APP  
date:   2014-02-28 17:30:00
tags:	WebDev, Development, Vagrant, Protobox
assets: 2014-02-28-prototype-api-with-deployd-for-angularjs-app
header: 1
comments: true
---

An der [ng-conf 2014](http://ng-conf.org) habe ich mir die Präsentation von Jeff Cross [Rapid Prototyping with Angular & Deployd](http://www.youtube.com/watch?v=0V8fQoqQLLA) angeschaut und bereits nach ein paar Minuten wusste ich, dass ich mir [Deployd](http://www.deployd.com) einmal genauer anschauen muss.

#Ausgangslage

Seit einiger Zeit arbeite ich im Backend Bereich mit ASP.NET MVC/WebAPI oder NancyFX. Der Data Access Layer ist dabei mittels Entity Framework Code First zu einer Relationalen Datenbank (MSSQL) realisiert und das durch ASP.NET MVC/WebAPI/NancyFX realisierte RESTful API wird im Frontend von einer AngularJS App konsumiert. Das Zusammenspiel dieser Technologien funktioniert wunderbar und es gibt auch keinen grossen Bedarf dies zu ändern. Nun gibt es jedoch oft die Situation, in der man für einen Kunden oder einfach zum Test ein simples API hochfahren möchte, ohne dabei die Ganze DB/Entity Framework Konfiguration durchführen zu müssen. An dieser Stelle kommt Deployd ins Spiel. 

#Deployd

Deployd ist eine Open Source Plattform zum Erstellen von APIs für Web und Mobile Apps.

> **THE SIMPLEST WAY TO BUILD AN API.**
> Design, build and scale APIs for web and mobile apps in minutes instead of days.
> 
> –deployd.com

Die Lösung basiert auf Node.js und MongoDB und es stehen Installers für OSX und Windows, sowie ein node Module zur Verfügung.

##Deployd API erstellen

Nach der Installation kann mit folgenden Commands ein neues Deployd API erstellt und gestartet werden.

{% highlight bash %}
dpd create deploydTestApi
cd deploydTestApi
dpd -d
{% endhighlight %}

Nach dem Starten öffnet sich automatisch das Dashboard in einem Browserfenster.

![]({{ site.url }}/assets/{{page.assets}}/dashboard.png)

##Collections erstellen

Nun können Collections erstellt und Daten eingefügt werden. Für die Test App habe ich die Collections **CONTACTS** und **ADDRESSES** erstellt mit den folgenen Properties:

**CONTACTS**

- id (auto generated)
- firstName (string)
- lastName (string)
- phone (string)
- email (string)
- addresses (array)

**ADDRESSES**

- id (auto generated)
- street (string)
- zip (number)
- city (string)

##Daten erfassen

Die Daten können ebenfalls einfach über das Dashboard erfasst werden. Für die Test App wurden drei Adressen und zwei Kontakte erfasst. Die Kontakte halten im `addresses`-Array jeweils eine bzw. zwei Addresses-Object-IDs.

![]({{ site.url }}/assets/{{page.assets}}/addresses-array.png)

##API Testen

Nach dem Design des APIs und dem Einfügen der Daten kann das API nun getestet werden. Beim Aufruf der URL `http://localhost:<port>` erscheint der Welcome Screen.

![]({{ site.url }}/assets/{{page.assets}}/welcome-screen.png)

Nun kann über `/contacts` resp. `/addresses` auf die Collections zugegriffen werden. 

![]({{ site.url }}/assets/{{page.assets}}/collection-access.png)

**Hinweis:** Für eine etwas schönere Darstellung der Daten (JSON) gibt es vielzählige Tools, beispielsweise die [JSON Formatter](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa?utm_source=chrome-ntp-icon) Google Chrome App.

#Angular

Nun wird von einer AngularJS App auf die Daten zugegriffen und diese in einer simplen Tabelle darzustellen.

##View 

{% highlight html %}
<!DOCTYPE html>
<html data-ng-app="deploydTestApi">
<head>
    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
	<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.12/angular.min.js"></script>
</head>
<body>
    <div class="container" data-ng-controller="contactController">
        <div class="page-header">
            <h1>{{title}}</h1>
        </div>
        <table class="table">
            <thead>
                <tr>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>Phone</th>
                    <th>Email</th>
                    <th>Addresses</th>
                </tr>
            </thead>
            <tbody>
                <tr data-ng-repeat="contact in contacts">
                    <td>{{contact.firstName}}</td>
                    <td>{{contact.lastName}}</td>
                    <td>{{contact.phone}}</td>
                    <td>{{contact.email}}</td>
                    <td>
                        <p data-ng-repeat="address in contact.addressesPlain">
                            {{address.street}}, {{address.zip}} {{address.city}}
                        </p>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
	<script src="app.js"></script>
</body>
</html>
{% endhighlight %}

##Angular

Der Controller hält ein Array von `contacts` welches beim Initialisieren der Seite in `getContacts()` abgefüllt wird. Dabei werden jeweils die Adressen der jeweiligen Kontakte in einem separaten Request (Roundtrips mal aussen vor gelassen) geholt und dem Array angehängt. 

{% highlight js %}
var app = angular.module('deploydTestApi', []);

app.controller('contactController', function ($scope, $http) {

    $scope.title = 'Welcome to deploydTestApi!';
    $scope.contacts = [];

    getContacts();

    function getContacts() {
        $http.get('http://localhost:2403/contacts').then(function (response) {
            $scope.contacts = response.data;

            angular.forEach($scope.contacts, function (value, key) {
                var addresses = [];
                angular.forEach(value.addresses, function (value, key) {
                    getAddress(value).then(function (data) {
                        addresses.push(data);
                    });
                });
                value.addressesPlain = addresses;
            });

        });
    }

    function getAddress(objectId) {
        return $http.get('http://localhost:2403/addresses/' + objectId).then(function (response) {
            return response.data;
        });
    }

});
{% endhighlight %}

Das Resultat sieht dann folgendermassen aus:

![]({{ site.url }}/assets/{{page.assets}}/result.png)

#Ausblick

Dieser Post deckt nur ein kleiner Teil der Funktionalität ab. Es stehen noch weitere Interessante Konzepte und Funktionen bereit wie:

- Easy Graph Data
- User Management
- Validation
- dpd.js Library
- ..

#Fazit

Deployd ist tatsächlich eine einfache Lösung um schnell ein API hochzuziehen. Das Handling gefällt mir und gerade für schnelle Tests ist es perfekt. Für den produktiven Betrieb ist es jedoch sicherlich noch nicht geeignet (v0.6.10 preview). Ich bin mal gespannt wie es da weitergeht, zumal die Entwicklung etwas stillzustehen scheint. Auch die Website war Mitte Februar für knapp eine Woche down, ohne Statement oder Reaktion auf Twitter.

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/jentulman">@jentulman</a> <a href="https://twitter.com/deploydapp">@deploydapp</a> any news on that?</p>&mdash; Lu Fehr (@lufehr) <a href="https://twitter.com/lufehr/statuses/435099048492924928">February 16, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Ein gutes Zeichen, aktuell ist die Website wieder verfügbar.

#Quellen & Links

- [Deployd](http://www.deployd.com)
- [AngularJS](http://www.angularjs.org)
- [MongoDB](http://www.mongodb.com)