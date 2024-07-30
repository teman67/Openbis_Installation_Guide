# So installieren Sie Openbis über VirtualBox

## Schritt-für-Schritt-Anleitung:

1. **Download und Installation von VirtualBox**
   - Laden Sie VirtualBox herunter: [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)
   - Installieren Sie VirtualBox auf Ihrem System.

2. **Download des Ubuntu-Images**
   - Laden Sie das Ubuntu-Image herunter: [Ubuntu Downloads](https://ubuntu.com/download)

3. **Erstellen Sie eine neue virtuelle Maschine in VirtualBox**
   - Öffnen Sie VirtualBox.
   - Klicken Sie auf die Schaltfläche "Neu".
   - Geben Sie der virtuellen Maschine einen Namen (z.B. `ubuntu_2024`).
   - Definieren Sie den Installationspfad für die virtuelle Maschine.
   - Wählen Sie die heruntergeladene Ubuntu-Image-Datei aus.
   - Legen Sie einen Benutzernamen und ein Passwort für Ubuntu fest.
   - Weisen Sie Speicher und Prozessoren zu.
   - Definieren Sie die Größe der Festplatte (z.B. 100 GB), lassen Sie die anderen Optionen unverändert und klicken Sie auf "Fertigstellen".

4. **Starten und Installieren von Ubuntu**
   - Wählen Sie die neu erstellte virtuelle Maschine aus dem linken Panel aus.
   - Klicken Sie auf "Starten", um die Ubuntu-Installation zu beginnen.
   - Folgen Sie den Installationsanweisungen, um die Einrichtung abzuschließen.

5. **Ubuntu aktualisieren**
   - Führen Sie das installierte Ubuntu in VirtualBox aus.
   - Öffnen Sie das Terminal und geben Sie ein: `sudo apt upgrade`, um alle Bibliotheken zu aktualisieren.

6. **Openbis-Quelle herunterladen und vorbereiten**
   - Gehen Sie zu: [Openbis Production Releases](https://unlimited.ethz.ch/display/openbis/Production+Releases)
   - Laden Sie eine Version herunter (z.B. Version 20.10.7.2), indem Sie auf "Installation and Upgrade Wizard" klicken.
   - Erstellen Sie drei Ordner: `source_openbis`, `openbis` und `DSS` (z.B. auf dem Desktop).
   - Verschieben Sie den heruntergeladenen Quellcode in den Ordner `source_openbis`.
   - Extrahieren Sie den Quellcode: `tar xzvf openBIS-installation-…...tar.gz`

7. **PostgreSQL und Java installieren**
   - Führen Sie die folgenden Befehle im Terminal aus:
     ```bash
     sudo apt install postgresql
     sudo apt install openjdk-11-jdk
     ```
   - Legen Sie die Standard-Java-Version fest:
     ```bash
     sudo update-alternatives --config java
     ```
     - Wählen Sie Version 11 aus der Liste aus.
     - Überprüfen Sie die Java-Version: `java -version`

8. **PostgreSQL-Status überprüfen**
   - Führen Sie aus: `sudo systemctl status postgresql`, um zu überprüfen, ob PostgreSQL läuft.
   - Optional können Sie verwenden: `pg_lsclusters`, um den PostgreSQL-Serverstatus zu überprüfen.

9. **Erstellen eines Datenbankbenutzers**
   - Führen Sie die folgenden Befehle aus, um einen Datenbankbenutzer mit demselben Namen wie der OS-Benutzer zu erstellen:
     ```bash
     sudo -u postgres createuser openbis   # Ersetzen Sie 'openbis' durch Ihren OS-Benutzernamen
     su – openbis
     psql -U postgres -l
     psql -l
     ```

10. **PostgreSQL konfigurieren**
    - Bearbeiten Sie die PostgreSQL-Konfigurationsdatei:
      ```bash
      sudo nano /etc/postgresql/16/main/pg_hba.conf
      ```
    - Ändern Sie alle `peer` zu `trust` am Ende der Datei, speichern Sie und beenden Sie.
    - Starten Sie PostgreSQL neu:
      ```bash
      sudo systemctl restart postgresql
      ```

11. **Openbis konfigurieren und installieren**
    - Navigieren Sie zum Ordner `source_openbis` und betreten Sie den entpackten Ordner.
    - Bearbeiten Sie `console.properties`:
      ```bash
      nano console.properties
      ```
      - Setzen Sie `INSTALL_PATH` auf den Ordner `openbis`.
      - Setzen Sie `DSS_ROOT_DIR` auf den Ordner `DSS`.
      - Kommentieren Sie die folgenden Zeilen aus:
        ```
        ELN-LIMS, ELN-LIMS-LIFE-SCIENCES, ILLUMINA-NGS, MICROSCOPY, FLOW
        ```
      - Speichern und beenden Sie die Datei.
    - Installieren Sie Openbis:
      ```bash
      ./run-console.sh
      ```
    - Starten Sie Openbis:
      ```bash
      cd openbis/bin
      ./allup.sh
      ```

12. **Mögliche Fehler beheben**
    - Wenn Sie auf den Fehler stoßen: “ERROR [main] OPERATION.PostgreSQLAdminDAO - Failed to get the version of the database server,” folgen Sie diesen Schritten:
      ```bash
      ./alldown.sh
      export FORCE_OPENBIS_POSTGRES_VALID_VERSIONS=16.3  # Version entsprechend anpassen
      ./allup.sh
      ```
    - Bearbeiten Sie `allup.sh`, um Wiederholungen zu vermeiden:
      ```bash
      nano allup.sh
      ```
      - Fügen Sie hinzu: `export FORCE_OPENBIS_POSTGRES_VALID_VERSIONS=16.3` unter der Zeile `# Starts up openbis and DSS`.
      - Speichern und beenden.

13. **Auf Openbis zugreifen**
    - Öffnen Sie Ihren Browser und navigieren Sie zu:
      - [Openbis Admin Panel](https://localhost:8443/openbis/webapp/openbis-ng-ui/)
      - [Openbis ELN](https://localhost:8443/openbis/webapp/eln-lims/)
    - Melden Sie sich mit dem Admin-Benutzernamen (`admin`) und dem während der Installation festgelegten Passwort an.


---

## Genießen Sie die Nutzung von Openbis !

**Gut Gemacht!**
