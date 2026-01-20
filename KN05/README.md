## KN05 – Netzwerk & Sicherheit

### Teil A: Netzwerk – Grundbegriffe

**VPC (Virtual Private Cloud)**

Eine VPC ist ein logisch isoliertes, virtuelles Netzwerk innerhalb von AWS. Sie stellt den übergeordneten IP-Adressraum bereit und definiert den Rahmen, in dem Subnetze, Instanzen und weitere Netzwerkressourcen betrieben werden.

**Subnetz (Subnet)**

Ein Subnetz ist ein Teilbereich einer VPC mit einem eigenen IP-Adressbereich (CIDR). Es dient zur logischen Trennung von Ressourcen, z. B. nach Verfügbarkeit, Sicherheitsanforderungen oder Aufgaben (Webserver, Datenbank).

**Public IP**

Eine öffentliche IP-Adresse ist über das Internet erreichbar. Instanzen mit einer Public IP können direkt von externen Clients angesprochen werden (z. B. Webserver).

**Private IP**

Eine private IP-Adresse ist nur innerhalb der VPC erreichbar. Sie wird für die interne Kommunikation zwischen Instanzen verwendet, z. B. zwischen Webserver und Datenbank.

**Static IP (Elastic IP)**

Eine statische IP-Adresse bleibt auch nach einem Stop/Start der Instanz gleich. In AWS wird dies über sogenannte Elastic IPs realisiert und ist insbesondere für Webserver wichtig.

### Subnetz-Übersicht

Das Subnetz der vorherigen Aufgabe (KN04) wurde korrekt identifiziert und in `Sub_KN04` umbenannt.

Für diese Aufgabe wurde ein separates Subnetz `Sub_KN05` verwendet.

Die Subnetze decken gemeinsam den IP-Adressbereich der VPC ab.

**Screenshots Subnetze:**

- `SubnetzKN04.png` – Subnetz-Liste mit umbenanntem Subnetz `Sub_KN04`.
- `SubnetzKN05.png` – Details des Subnetzes `Sub_KN05` mit CIDR `172.31.48.0/20`.

### Gewähltes Subnetz (KN05)

**Subnetzname**: `Sub_KN05`

**IPv4 CIDR**: `172.31.48.0/20`

**Verfügbare IPv4-Adressen**: `4096`

Das Subnetz erlaubt IP-Adressen im Bereich von `172.31.48.0` bis `172.31.63.255`, wobei von AWS reservierte Adressen ausgeschlossen sind.

### Definierte private IP-Adressen

Gemäss den Vorgaben (letzte Zahl durch 10 teilbar, IPs im korrekten Subnetz):

**Webserver private IP**:

`172.31.48.10`


**Datenbankserver private IP**:

`172.31.48.20`


Diese IPs liegen innerhalb des Subnetz-IP-Ranges und sind nicht von AWS reserviert.

---

### Teil B: Objekte und Instanzen erstellen

#### Sicherheitsgruppen

Für die Trennung von Webserver und Datenbank wurden zwei separate Sicherheitsgruppen erstellt, um den Zugriff gezielt zu steuern und die Angriffsfläche zu minimieren.

##### SG-Webserver

Die Sicherheitsgruppe `SG-Webserver` wird für die Webserver-Instanz verwendet und erlaubt ausschliesslich die notwendigen Zugriffe aus dem Internet.

**Inbound-Regeln:**

- SSH (TCP 22) – Quelle: **My IP**
- HTTP (TCP 80) – Quelle: `0.0.0.0/0`
- HTTPS (TCP 443) – Quelle: `0.0.0.0/0`

**Outbound-Regeln:**

- Alle ausgehenden Verbindungen erlaubt

**Screenshots SG-Webserver:**

- `Screenshot 2026-01-20 151821.png` – Inbound-Regeln der Sicherheitsgruppe `SG-Webserver` (HTTP/HTTPS/SSH).
- `Screenshot 2026-01-20 152138.png` – Sicherheitsgruppen-Übersicht mit `SG-Webserver` und `SG-Database`.

##### SG-Database

Die Sicherheitsgruppe `SG-Database` wird für die Datenbank-Instanz verwendet und erlaubt den Zugriff ausschliesslich vom Webserver aus.

**Inbound-Regeln:**

- MySQL / Aurora (TCP 3306) – Quelle: `SG-Webserver`

**Outbound-Regeln:**

- Alle ausgehenden Verbindungen erlaubt

Durch diese Konfiguration ist die Datenbank nicht direkt aus dem Internet erreichbar.

**Screenshots SG-Database:**

- `Screenshot 2026-01-20 152045.png` – Konfiguration der Inbound- und Outbound-Regeln von `SG-Database`.

#### Öffentliche, statische IP (Elastic IP)

Für den Webserver wurde eine Elastic IP erstellt, damit die öffentliche IP-Adresse auch nach einem Stop/Start der Instanz unverändert bleibt.

- Die Elastic IP wurde dem Webserver zugewiesen.
- Die automatisch zugewiesene öffentliche IP wurde dadurch ersetzt.

**Screenshots Elastic IP:**

- `Screenshot 2026-01-20 152511.png` – Elastic-IP-Liste mit der zugewiesenen Adresse.
- `Screenshot 2026-01-20 154027.png` – Zuweisung der Elastic IP zur Instanz mit privater IP `172.31.48.10`.

#### Instanzen

Es wurden zwei separate EC2-Instanzen erstellt, eine für den Webserver und eine für die Datenbank.

**Webserver (`KN05_Web`)**

- Subnetz: `Sub_KN05`
- Private IP-Adresse: `172.31.48.10`
- Öffentliche IP-Adresse: Elastic IP (`100.49.9.177`)
- Sicherheitsgruppe: `SG-Webserver`

**Datenbank (`KN05_DB`)**

- Subnetz: `Sub_KN05`
- Private IP-Adresse: `172.31.48.20`
- Öffentliche IP-Adresse: keine
- Sicherheitsgruppe: `SG-Database`

**Screenshots Instanzen:**

- `Screenshot 2026-01-20 152801.png` – Netzwerkkonfiguration Webserver beim Erstellen (Subnetz `Sub_KN05`, SG-Webserver, öffentliche IP aktiv).
- `Screenshot 2026-01-20 153516.png` – Netzwerkkonfiguration Datenbank beim Erstellen (Subnetz `Sub_KN05`, SG-Database, **ohne** öffentliche IP).
- `Screenshot 2026-01-20 153740.png` – Instanzdetails Webserver mit privater IP `172.31.48.10` und zugewiesener Elastic IP.
- `Screenshot 2026-01-20 154152.png` – Instanzdetails Datenbank mit privater IP `172.31.48.20` ohne öffentliche IP.

#### Stop-Test

Um zu überprüfen, dass die IP-Adressen korrekt und statisch konfiguriert sind, wurden beide Instanzen gestoppt.

- Die privaten IP-Adressen blieben unverändert.
- Die Elastic IP des Webservers blieb ebenfalls bestehen.

Dies bestätigt die korrekte Netzwerkkonfiguration.

**Screenshot Stop-Test:**

- `Screenshot 2026-01-20 154202.png` – Instanzen im Status „stopped“ mit unveränderten privaten IPs und Elastic IP.

#### Funktionstest

Nach dem erneuten Start der Instanzen wurden alle Seiten erfolgreich aufgerufen:

- `index.html`
- `info.php`
- `db.php`

Der Zugriff auf die Datenbank funktioniert ausschliesslich über den Webserver.

**Screenshot Funktionstest:**  

- `Screenshot 2026-01-20 155728.png` – Aufruf von `index.html` (Apache-Default-Page erreichbar über die Elastic IP).  
- `Screenshot 2026-01-20 160638.png` – Aufruf von `info.php` (PHP-Info sichtbar, PHP 8.3 läuft).  
- `Screenshot 2026-01-20 163237.png` – Aufruf von `db.php` mit Meldung `DB CONNECT OK`, erfolgreiche DB-Verbindung vom Webserver zur Datenbank.  
- `image.png` – Alternativer Nachweis `db.php` mit ausgegebener Benutzertabelle (DB-Verbindung und Query funktionieren).

#### Weitere Screenshots (Dokumentation)

- `Screenshot 2026-01-20 162711.png` – Erstellungs-Dialog der DB-Instanz mit gesetzter privater IP `172.31.48.20` (Zwischenschritt während der Einrichtung).

### Fazit

Durch die Trennung von Webserver und Datenbank in einem eigenen Subnetz, die gezielte Verwendung von Sicherheitsgruppen sowie den Einsatz einer Elastic IP für den Webserver wurde eine saubere und sichere Netzwerkarchitektur umgesetzt.

Die Datenbank ist nicht öffentlich erreichbar und kommuniziert ausschliesslich intern über private IP-Adressen mit dem Webserver.