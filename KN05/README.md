KN05 – Teil A: Netzwerk / Sicherheit
Erklärung der Grundbegriffe
VPC (Virtual Private Cloud)

Eine VPC ist ein logisch isoliertes, virtuelles Netzwerk innerhalb von AWS. Sie stellt den übergeordneten IP-Adressraum bereit und definiert den Rahmen, in dem Subnetze, Instanzen und weitere Netzwerkressourcen betrieben werden.

Subnetz (Subnet)

Ein Subnetz ist ein Teilbereich einer VPC mit einem eigenen IP-Adressbereich (CIDR). Es dient zur logischen Trennung von Ressourcen, z. B. nach Verfügbarkeit, Sicherheitsanforderungen oder Aufgaben (Webserver, Datenbank).

Public IP

Eine öffentliche IP-Adresse ist über das Internet erreichbar. Instanzen mit einer Public IP können direkt von externen Clients angesprochen werden (z. B. Webserver).

Private IP

Eine private IP-Adresse ist nur innerhalb der VPC erreichbar. Sie wird für die interne Kommunikation zwischen Instanzen verwendet, z. B. zwischen Webserver und Datenbank.

Static IP (Elastic IP)

Eine statische IP-Adresse bleibt auch nach einem Stop/Start der Instanz gleich. In AWS wird dies über sogenannte Elastic IPs realisiert und ist insbesondere für Webserver wichtig.

Subnetz-Übersicht

Das Subnetz der vorherigen Aufgabe (KN04) wurde korrekt identifiziert und in Sub_KN04 umbenannt.

Für diese Aufgabe wurde ein separates Subnetz Sub_KN05 verwendet.

Die Subnetze decken gemeinsam den IP-Adressbereich der VPC ab.

(Screenshot der Subnetz-Liste mit den angepassten Namen ist beigefügt.)

Gewähltes Subnetz (KN05)

Subnetzname: Sub_KN05

IPv4 CIDR: 172.31.48.0/20

Verfügbare IPv4-Adressen: 4096

Das Subnetz erlaubt IP-Adressen im Bereich von 172.31.48.0 bis 172.31.63.255, wobei von AWS reservierte Adressen ausgeschlossen sind.

Definierte private IP-Adressen

Gemäss den Vorgaben (letzte Zahl durch 10 teilbar, IPs im korrekten Subnetz):

Webserver private IP:

172.31.48.10


Datenbankserver private IP:

172.31.48.20


Diese IPs liegen innerhalb des Subnetz-IP-Ranges und sind nicht von AWS reserviert.