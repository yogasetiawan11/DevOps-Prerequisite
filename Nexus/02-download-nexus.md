# Download on dev environment
To install Nexus on Linux, follow these steps:
Prerequisites:

- Ensure that Java Development Kit (JDK) is installed on your Linux system. Nexus requires Java to run. You can check if Java is installed by running the command java -version in the terminal. If Java is not installed, you can install it using the package manager of your Linux distribution.
Download Nexus:

- Visit the Sonatype website and go to the Nexus download [page:](https://www.sonatype.com/nexus/repository-oss-download)
- Download the latest version of Nexus Repository Manager OSS (Open Source Edition). Look for the "Unix/Linux" distribution 

Extract Nexus:

- Open a terminal and navigate to the directory where the downloaded Nexus package is located.
- Extract the package using the following command:
```sh 
tar -xf latest-unix.tar.gz
```
Configure Nexus:

- Open the extracted Nexus directory.
In the bin folder, you will find a script called nexus (or nexus.exe for Windows). This is the Nexus startup script.
Make the script executable by running the following command:
```sh
chmod +x nexus
```
- Start Nexus:

Start Nexus by running the following command in the terminal:
```sh
./nexus start
```
- Access Nexus Web UI:

Open a web browser and navigate to``` http://localhost:8081```. This is the default URL for accessing Nexus.
The first time you access Nexus, you will be prompted to set up the administrator credentials. to get the password run this command: 

```sh
cat /path/to/sonatype-work/nexus3/admin.password
```

That's it! Nexus is now installed and running on your Linux system. You can proceed to configure Nexus, create repositories, and manage artifacts using the Nexus Web UI.

# Download nexus via Docker
```sh
docker run -d --name nexus -p 8081:8081 sonatype/nexus3
```

# Download nexus for production environment
In production, you need to address reliability, performance, and security concerns.

- Here’s what to improve:

1. Run as a dedicated user

Never run Nexus as root.
Create a user:

```bash
sudo useradd --system --no-create-home --shell /bin/false nexus
```

Then change ownership:

```bash
sudo chown -R nexus:nexus /opt/nexus /opt/sonatype-work
```
2. Use a service manager (systemd)

Instead of manually starting ./nexus start, create a systemd service:
```sh
sudo vim /etc/systemd/system/nexus.service
```

Example:
```bash
[Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
User=nexus
Group=nexus
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

- Then enable and start:
```bash
sudo systemctl enable nexus
sudo systemctl start nexus
```

3. Use proper storage

By default, Nexus stores artifacts in ```/opt/sonatype-work/nexus3.```

### For production:

- Mount it to a dedicated volume or NFS mount.

- Use durable storage (EBS, network volume, or object store) for persistence and backups.

4. Enable HTTPS
Never expose Nexus on plain HTTP (:8081) to the internet.

Options:

- Use NGINX or Apache as a reverse proxy with SSL.

- Or configure TLS directly in Nexus (requires certificate setup).

5. Secure and monitor

- Change default admin password immediately.

- Integrate with LDAP or SSO for user access.

- Restrict firewall access — only CI/CD and internal developers should reach it.

- Enable system metrics and logging to a central log collector (e.g., Prometheus, Grafana, or ELK).

6. Automate backup

Backup both:

- /opt/sonatype-work/nexus3 (the data directory)

- /opt/nexus/etc (config)
Use cron jobs or snapshot-based backups.