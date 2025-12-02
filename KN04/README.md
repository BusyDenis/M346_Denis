## KN04 – Cloud-init & SSH-Key

### A) Cloud-init Datei verstehen (10 %)

- **Ziel**: Verständnis von Cloud-init und dem YAML-Format.
- **Aufgabe**:
  - Theorie zu **Cloud-init** und **YAML** lesen.
  - Vorgegebene `cloud-init.yaml` herunterladen.
  - **Alle Zeilen** der Cloud-init Datei im Schema `Konfiguration # Erklärung` dokumentieren, z.B.:
    - `#cloud-config  # Deklariert die Datei als Cloud-Init-Konfiguration`
    - `users:  # Definition der Benutzer, die bei der Initialisierung erstellt/konfiguriert werden`
    - `- name: ubuntu  # Benutzername`
    - `sudo: ALL=(ALL) NOPASSWD:ALL  # sudo-Regeln für diesen Benutzer`

- **Umsetzung in diesem Projekt**:
  - Die Datei `cloud-init.yaml` wurde vollständig kommentiert, sodass jede Zeile bzw. jeder Block mit einer kurzen Erklärung versehen ist.

- **Abgabe A**:
  - **Dokumentierte YAML-Datei**: `cloud-init.yaml` (mit Kommentaren) im Git-Repository.

---

### B) SSH-Key und Cloud-init (15 %)

- **Ziel**: Verständnis von SSH mit Private/Public Key und Verwendung von SSH-Keys über Cloud-init und AWS-GUI.

- **Schritte**:
  1. **Public Keys aus privaten Schlüsseln extrahieren** (aus KN02):
     - Erster Key (für Cloud-init):
       - Per Befehl z.B.: `ssh-keygen -y -f key1 > key1.pub`
     - Zweiter Key (für AWS-GUI):
       - Per Befehl z.B.: `ssh-keygen -y -f key2 > key2.pub`
  2. **Neue Instanz in AWS erstellen**:
     - AMI: **Ubuntu 24.04**
     - **Key pair**: zweiten Schlüssel (`key2`) auswählen.
     - Sonstige Einstellungen: Standard, ausser Cloud-init/User Data.
  3. **Cloud-init mit erstem Public Key anpassen**:
     - In `cloud-init.yaml` unter `ssh_authorized_keys` den Eintrag ersetzen durch den Inhalt von `key1.pub` im Format:
       - `ssh-rsa <ihr-Schlüssel-ohne-zeilenumbrüche> aws-key`
     - Die komplette `cloud-init.yaml` in AWS im Bereich **Advanced details → User data** einfügen.
  4. **SSH-Login testen**:
     - Mit **erstem privaten Key** (zu `key1.pub` / Cloud-init):
       - `ssh -i key1 ubuntu@<public-ip>`
     - Mit **zweitem privaten Key** (zu `key2.pub` / AWS-GUI):
       - `ssh -i key2 ubuntu@<public-ip>`
     - Nachweis, dass der Login mit dem ersten Key aus der Cloud-init-Konfiguration funktioniert.
  5. **Cloud-init-Log einsehen**:
     - Befehl auf der Instanz:
       - `sudo cat /var/log/cloud-init-output.log`
     - Pfad für spätere Fehleranalysen merken: `/var/log/cloud-init-output.log`

- **Abgaben B**:
  - **Angepasste Cloud-init Konfiguration**:
    - Datei `cloud-init.yaml` im Git-Repository mit eingetragenem ersten Public Key.
  - **Screenshot der Instanz-Details** (Key-Pair):
    - Datei: `keypairassignedto.png`
    - Bereich mit **"Key pair assigned at launch"** sichtbar.

    ![Key pair assigned at launch](./keypairassignedto.png)

  - **Screenshot SSH mit erstem Key** (Cloud-init-Key):
    - Datei: `SSHConnection.png`
    - Zeigt den `ssh`-Befehl mit dem **ersten privaten Key** und die erfolgreiche Verbindung zur Instanz.

    ![SSH Verbindung mit erstem Key](./SSHConnection.png)

  - **Screenshot Cloud-init-Log**:
    - Datei: `cloud-init-log_access.png`
    - Sichtbar: der verwendete Befehl (z.B. `sudo cat /var/log/cloud-init-output.log`) und der obere Teil des Logs.

    ![Cloud-init Log Ausgabe](./cloud-init-log_access.png)

---

### Übersicht der wichtigen Dateien in `KN04`

- `cloud-init.yaml` – kommentierte Cloud-init-Konfiguration mit SSH-Key.
- `README.md` – diese Aufgabenbeschreibung und Dokumentation.
- `keypairassignedto.png` – Screenshot Instanzdetails mit Key-Pair-Anzeige.
- `SSHConnection.png` – Screenshot SSH-Verbindung mit dem ersten Key.
- `cloud-init-log_access.png` – Screenshot Zugriff auf `/var/log/cloud-init-output.log`.

