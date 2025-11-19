## KN02 – AWS EC2 Instanz & SSH-Zugriff

### B) Instanz erstellen (40 %)

- **Ziel**  
  Eine EC2-Instanz mit Ubuntu 24.04 im Learner Lab erstellen, um die Grundlagen der AWS-Konsole, des Budget-Monitorings und der Modulstruktur zu üben.

- **Vorgehen**  
  1. Im Learner Lab die AWS-Konsole starten, Region aus Vorgabe übernehmen.  
  2. `EC2 → Instances → Launch instance`.  
  3. Parameter setzen:  
     - Name: `KN02_Denis` (Instance-ID `i-02ea7cd86acfbf4ee`)  
     - OS Image: `Ubuntu Server 24.04 LTS (HVM)` / AMI `ami-0ecb62995f68bb549`  
     - Instance Type: `t2.micro`  
     - Key pair: `Create new key pair` → einmal `Denis1`, einmal `Denis2` (RSA, .pem) und anschließend `Denis1` auswählen  
     - Networking, Monitoring, Shutdown behavior unverändert lassen  
  4. Launch, nach Status `running` Elastic-IP (= Public IPv4) notieren.

- **Angeforderte Eckdaten**  
  - **Diskgrösse:** 8 GiB gp3 (Default beim Launch)  
  - **Betriebssystem:** Ubuntu Server 24.04 LTS  
  - **RAM:** 1 GiB (t2.micro)  
  - **vCPU:** 1 vCPU (Intel Xeon, burstable)  
  - **Public IP:** `52.201.60.41` (DNS `ec2-52-201-60-41.compute-1.amazonaws.com`)  
  - **Private IP:** `172.31.24.39`

- **Nachweise**  
  - Detailansicht mit Instanz-Infos:  
    ![Instanzdetails 1](image.png)  
  - Detailansicht mit Netzwerk / IP:  
    ![Instanzdetails 2](ssh-key2-error.png)  

### C) Zugriff mit SSH-Key (40 %)

- **Grundlagen**  
  - AWS speichert nur den *öffentlichen* Schlüssel unter `/home/ubuntu/.ssh/authorized_keys`.  
  - Der *private* Schlüssel (`.pem`) bleibt ausschließlich lokal und muss mit restriktiven Rechten (`chmod 700`) abgelegt werden.  
  - Public Keys können jederzeit aus dem Private Key erzeugt werden:  
    `ssh-keygen -y -f Denis1.pem > Denis1.pub`

- **WSL/Windows Setup**  
  - Wegen der restriktiven NTFS/WSL-Rechte wurde der Benutzer `awsProject` unter WSL erstellt.  
  - Schlüssel liegen im WSL-Home (`/home/awsProject/.ssh/`) statt unter `/mnt/c/Users/denis/.ssh`, damit `chmod 700` funktioniert.  
  - Beispiel-Permissions:  
    ```sh
    sudo chown awsProject:awsProject ~/.ssh
    chmod 700 ~/.ssh
    chmod 400 ~/.ssh/Denis1.pem ~/.ssh/Denis2.pem
    ```

- **SSH-Befehle**  
  - **Key 1 (funktioniert):**  
    `ssh -i "Denis1.pem" ubuntu@ec2-52-201-60-41.compute-1.amazonaws.com`  
    ![Key im Detail](instanzdetails-ami.png)
  - **Key 2 (aktuell fehlgeschlagen):**  
    `ssh -i "Denis2.pem" ubuntu@ec2-52-201-60-41.compute-1.amazonaws.com`  
    ![SSH Key 2 Fehler](instanzdetails-network.png)
  - **Key 2 überprüfen:**  
    `cat .ssh/authorized_keys`  
    ![SSH Key 2 Fehler](ssh-key1.png)

- **Analyse des zweiten Keys**  
  - In `/home/ubuntu/.ssh/authorized_keys` befindet sich nur der Eintrag für `Denis1` (Screenshot bestätigt).  

### Leitfragen / Checkpoints

- **AWS-Umgebung:** Learner Lab starten/stoppen, Budget-Panel prüfen, Zeitfenster im Blick behalten.  
- **EC2-Verständnis:** Kenntnis der Ressourcen (Instances, Volumes, Key-Pairs, Security Groups) und ihrer Konfiguration.  
- **Instanztuning:** Speicher-/RAM-/CPU-Parameter wurden bewusst gewählt (t2.micro, 8 GiB gp3).  
- **SSH & Keys:**  
  - Unterschied Private/Public Key und Speicherort bekannt.  
  - Public Key aus Private Key extrahieren (`ssh-keygen -y`).  
  - SSH-Befehl sowie Option `-i` zur Schlüsselwahl beherrscht.  
  - Problembehebung: fehlenden Public Key manuell in `authorized_keys` eintragen.

---

**Hinweis:** Ich arbeite primär unter Windows, nutze aber WSL für CLI-Aufgaben. Wegen der unterschiedlichen Berechtigungsmodelle verwalte ich AWS-Schlüssel ausschließlich im WSL-Home des Users `awsProject`, damit die `.ssh`-Rechte den Linux-Anforderungen entsprechen.

  - **Neuer Benutzer erstellt ^für Kommunikation zwischen WSL und Windows:**  
    ![SSH Key 1](instanzliste.png)

