---
layout: post
title:  A really, really simple demo ticket system with lightswitch
date:   2010-08-26 14:00:00
tags:   .NET, LightSwitch, VisualStudio
assets: 2010-08-26-a-really-really-simple-demo-ticket-system-with-lightswitch
header: 3
comments: true
---

Microsoft’s neuster Streich Visual Studio LightSwitch steht als Beta 1 zum Download bereit. Das neueste Mitglied der Familie soll die Erstellung von professionellen, datengetriebenen Applikationen für Desktop, Web und Cloud vereinfachen. Ich habe mal ein wenig rumgespielt und ein simples Ticket System zur Demo erstellt.

1. Herunterladen und Installieren (link: http://www.microsoft.com/visualstudio/en-us/lightswitch text:[Download])
2. Los geht's

**File –> New –> Project** | LightSwitch – ein schon fast leerer Startbildschirm erscheint und man kann direkt mit der Datenmodellierung beginnen. 

**Create new table**

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-1.png)

**Customer (Customers)**

<table class="table table-striped">
	<thead>
		<tr>
			<th width="20%">Name</th>
			<th>Type</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Id</td>
			<td>Int32</td>
		</tr>
		<tr>
			<td>Name</td>
			<td>String</td>
		</tr>
		<tr>
			<td>Address</td>
			<td>String</td>
		</tr>
		<tr>
			<td>Phone</td>
			<td>PhoneNumber</td>
		</tr>
		<tr>
			<td>Email</td>
			<td>EmailAddress</td>
		</tr>		
		<tr>
			<td><em>Tickets</em></td>
			<td><em>TicketCollection</em></td>
		</tr>
	</tbody>
</table>

Die Id ist bereits fix vorgegeben und kann nicht geändert werden. Das Feld Tickets wird beim hinzufügen von Relationships automatisch generiert.

**Category (Categories)**

<table class="table table-striped">
	<thead>
		<tr>
			<th width="20%">Name</th>
			<th>Type</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Id</td>
			<td>Int32</td>
		</tr>
		<tr>
			<td>Name</td>
			<td>String</td>
		</tr>		
		<tr>
			<td><em>Tickets</em></td>
			<td><em>TicketCollection</em></td>
		</tr>
	</tbody>
</table>

Dasselbe hier, das Feld Tickets wird automatisch generiert.

**Ticket (Tickets)**

<table class="table table-striped">
	<thead>
		<tr>
			<th width="20%">Name</th>
			<th>Type</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Id</td>
			<td>Int32</td>
		</tr>
		<tr>
			<td>Title</td>
			<td>String</td>
		</tr>	
		<tr>
			<td>ErrorMessage</td>
			<td>String</td>
		</tr>	
		<tr>
			<td>FailureDateTime</td>
			<td>DateTime</td>
		</tr>
		<tr>
			<td>Priority</td>
			<td>Int32</td>
		</tr>
		<tr>
			<td><em>Category</em></td>
			<td>Category</td>
		</tr>
		<tr>
			<td><em>Customer</em></td>
			<td>Customer</td>
		</tr>
	</tbody>
</table>

Und auch bei dieser Table, Category und Customer werden automatisch generiert.
Dem Feld Priority habe ich über die Properties eine Choice List hinzugefügt, somit kann der Ersteller eines Tickets aus drei vordefinierten Ticket-Prioritäten auswählen.

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-2.png)

Danach können die einzelnen Relationships definiert werden. **Add: Relationship**

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-3.png)

Auch hier kann das Ganze “zusammengeklickt” werden, dabei wird man vom Wizard unterstützt, in dem die Relationship Grafisch und in Textform augezeigt werden.

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-4.png)

Für das Ticket System habe ich folgende zweiRelationships verwendet.

<table class="table table-striped">
	<thead>
		<tr>
			<th width="20%">From</th>
			<th width="20%">To</th>
			<th>Multiplicity</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Ticket</td>
			<td>Category</td>
			<td>Many/One</td>
		</tr>
		<tr>
			<td>Ticket</td>
			<td>Customer</td>
			<td>Many/One</td>
		</tr>
	</tbody>
</table>

Somit sieht das Datenmodell aus der Sicht der Table Ticket zum Schluss folgendermassen aus.

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-5.png)

Nun, da die Datenmodellierung abgeschlossen ist, können die einzelnen Screens erstellt werden. **Add: Screen**

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-6.png)

Hierbei kann zwischen New Data Screen, Search Data Screen, Dtails Screen, Editable Grid Screen und List and Details Screen gewählt werden. Unter Screen Data die gewünschte Tabelle auswählen, fertig.

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-7.png)

Zum Ticket System, für die Datenerfassung habe ich drei Screens erstellt. CreateNewCustomer, CreateNewTicket und CreateNewCategory. Dabei wurde alles beim Standard belassen, ausser im Screen CreateNewTicket, da wäre beim Feld ErrorMessage etwas mehr Platz für die Eingabe wünschenswert, nicht? Hierfür kann die TextBox | ErrorMessage ausgewählt werden und in den Properties die Rows (Default 1) erweitert werden.

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-8.png)

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-9.png)

Soweit so gut, let’s fire up the app. **F5**

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-10.png)

Cool? Nun zum Schluss wäre eine Übersicht der Tickets noch hilfreich… Add: New Screen – List and Details Screen – Screen Data: Tickets. Fertig.

![]({{ site.url }}/assets/{{page.assets}}/a-really-really-simple-demo-ticket-system-with-lightswitch-11.png)

#Fazit 
Das Ganze sieht nicht schlecht aus und ist einfach zu bedienen. Mehr kann ich noch nicht sagen, lassen wir uns mal überraschen :)