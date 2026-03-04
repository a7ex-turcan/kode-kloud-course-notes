# Service Management with SYSTEMD

## Service Unit File

* needs to be created under `/etc/systemd/system/project-mercury.service

```bash
[Unit]
Description="Any description"
Documentation=""
# Allows to start this service only after the postgresql.service is up and running
After=postgresql.service

[Service]
# Derivative
ExecStart= /bin/bash /usr/bin/project-mercury.sh
# Associates a Service Account to the Service. Default is root
User=project_mercury
# Defines when the service is restarted
Restart=on-failure
# The time in seconds before the system attempts a restart
RestartSec=10

# Allows running the service when booting into Graphical Mode
[Install]
WantedBy graphical.target
```

* to start the background service:

```bash
systemctl start project-mercury.service
```

* to check the status:

```bash
systemctl status project-mercury.service
```

* to stop:

```bash
systemctl stop projec-mercury.service
```

* after any changes to the service:

```bash
systemctl daemon-reload
```

```bash
systemctl start project-mercury.service
```

## SYSTEMD Tools

### `SYSTEMCTL`

* The main tool to manage services

* To see the status of a service:

```bash
systemctl status docker
```

* To Start a service

```bash
systemctl start docker
```

* To Stop a service

```bash
systemctl stop docker
```

* To Restart a service

```bash
systemctl restart docker
```

* To Reload a service

```bash
systemctl reload docker
```

* To enable/disable and make it persistent accross reboots:

```bash
systemctl enable/disable docker
```

* To see all the units the SYSTEMD loaded:

```bash
systemctl list-units --all
```

#### Possible Service States

1. Active - Service Running
2. Inactive - Service Stopped
3. Failed - Crashed/Error/Timeout

#### Editing a service file

```bash
systemctl edit <service file> --full
```

### `JOURNALCTL`

* Query the systemd journal
* Troubleshooting tool

```bash
journalctl -u docker.service
```

