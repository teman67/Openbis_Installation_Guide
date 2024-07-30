# How to Install Openbis via VirtualBox

## Step-by-Step Instructions:

1. **Download and Install VirtualBox**
   - Download VirtualBox from: [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)
   - Install VirtualBox on your system.

2. **Download Ubuntu Image**
   - Download the Ubuntu image from: [Ubuntu Downloads](https://ubuntu.com/download)

3. **Set Up a New Virtual Machine in VirtualBox**
   - Open VirtualBox.
   - Click the "New" button.
   - Name the virtual machine (e.g., `ubuntu_2024`).
   - Define the installation path for the virtual machine.
   - Select the downloaded Ubuntu image file.
   - Set a username and password for Ubuntu.
   - Allocate memory and processors.
   - Define the hard disk size (e.g., 100 GB), leave other options as default, and click "Finish."

4. **Start and Install Ubuntu**
   - Select the newly created virtual machine from the left panel.
   - Click "Start" to begin the Ubuntu installation.
   - Follow the installation prompts to complete the setup.

5. **Update Ubuntu**
   - Run the installed Ubuntu in VirtualBox.
   - Open the terminal and type: `sudo apt upgrade` to update all libraries.

6. **Download and Prepare Openbis Source**
   - Go to: [Openbis Production Releases](https://unlimited.ethz.ch/display/openbis/Production+Releases)
   - Download a version (e.g., Version 20.10.7.2) by clicking on “Installation and Upgrade Wizard.”
   - Create three folders: `source_openbis`, `openbis`, and `DSS` (e.g., on Desktop).
   - Move the downloaded source code to the `source_openbis` folder.
   - Extract the source code: `tar xzvf openBIS-installation-…...tar.gz`

7. **Install PostgreSQL and Java**
   - Run the following commands in the terminal:
     ```bash
     sudo apt install postgresql
     sudo apt install openjdk-11-jdk
     ```
   - Set the default Java version:
     ```bash
     sudo update-alternatives --config java
     ```
     - Select version 11 from the list.
     - Verify the Java version: `java -version`

8. **Check PostgreSQL Status**
   - Run: `sudo systemctl status postgresql` to check if PostgreSQL is running.
   - Optionally, use: `pg_lsclusters` to verify the PostgreSQL server status.

9. **Create a Database User**
   - Run the following commands to create a database user with the same name as the OS user:
     ```bash
     sudo -u postgres createuser openbis   # Replace 'openbis' with your OS username
     su – openbis
     psql -U postgres -l
     psql -l
     ```

10. **Configure PostgreSQL**
    - Edit the PostgreSQL configuration file:
      ```bash
      sudo nano /etc/postgresql/16/main/pg_hba.conf
      ```
    - Change all `peer` to `trust` at the end of the file, then save and exit.
    - Restart PostgreSQL:
      ```bash
      sudo systemctl restart postgresql
      ```

11. **Configure and Install Openbis**
    - Navigate to the `source_openbis` folder and enter the unzipped folder.
    - Edit `console.properties`:
      ```bash
      nano console.properties
      ```
      - Set `INSTALL_PATH` to the `openbis` folder.
      - Set `DSS_ROOT_DIR` to the `DSS` folder.
      - Uncomment the following lines:
        ```
        ELN-LIMS, ELN-LIMS-LIFE-SCIENCES, ILLUMINA-NGS, MICROSCOPY, FLOW
        ```
      - Save and exit the file.
    - Install Openbis:
      ```bash
      ./run-console.sh
      ```
    - Start Openbis:
      ```bash
      cd openbis/bin
      ./allup.sh
      ```

12. **Handle Potential Errors**
    - If you encounter the error: “ERROR [main] OPERATION.PostgreSQLAdminDAO - Failed to get the version of the database server,” follow these steps:
      ```bash
      ./alldown.sh
      export FORCE_OPENBIS_POSTGRES_VALID_VERSIONS=16.3  # Adjust version as necessary
      ./allup.sh
      ```
    - Edit `allup.sh` to avoid repetition:
      ```bash
      nano allup.sh
      ```
      - Add: `export FORCE_OPENBIS_POSTGRES_VALID_VERSIONS=16.3` below the `# Starts up openbis and DSS` line.
      - Save and exit.

13. **Access Openbis**
    - Open your browser and navigate to:
      - [Openbis Admin Panel](https://localhost:8443/openbis/webapp/openbis-ng-ui/)
      - [Openbis ELN](https://localhost:8443/openbis/webapp/eln-lims/)
    - Log in with the admin username (`admin`) and the password you set during installation.

---

## Enjoy Using Openbis on Your Host System!

**Well Done!**

**Gut Gemacht!**

**Bra Gjort!**

**Výborně!**
