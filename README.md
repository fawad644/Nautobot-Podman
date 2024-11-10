Here's the updated `README.md` reflecting the need to update the volume mounts for the Nautobot configuration file and data storage:

---

# Nautobot-Podman

This project provides a setup to help you get started with running Nautobot using Podman. **Note:** This configuration is intended for development and testing purposes and is **not production ready**.

## Overview

This setup uses Podman’s pod and Kubernetes YAML file formats to easily spin up a local environment for Nautobot. The environment includes:
- **Nautobot application**
- **Postgres database** for Nautobot's data storage
- **Redis** for caching and background task queuing
- **Nautobot Celery Beat** for scheduled task management
- **Nautobot Celery Worker** for asynchronous background task processing

## Prerequisites

Ensure you have the following installed:
- **Podman**: Installation instructions can be found at [Podman’s official website](https://podman.io/).

## Setup Instructions

### 1. Clone the Repository

Clone this repository to your local machine:

```bash
git clone <repository-url>
cd Nautobot-Podman
```

### 2. Update Volume Mounts

You’ll need to specify locations on your host machine to store Nautobot’s configuration file and data.

1. **Open the `nautobot.yaml` file** (or the equivalent pod YAML file you're using).
2. Update the `hostPath` for the Nautobot configuration file and data directories under the `volumes` section to point to desired locations on your system. This will ensure persistent storage for configuration and data across restarts.

   For example:
   ```yaml
   volumes:
     - name: nautobot-config
       hostPath:
         path: /absolute/path/to/your/nautobot_config.py # Replace with the actual path to your Nautobot config file
         type: File
     - name: nautobot-media
       hostPath:
         path: /absolute/path/to/your/media/directory # Replace with the desired path for Nautobot media files
         type: Directory
     - name: nautobot-static
       hostPath:
         path: /absolute/path/to/your/static/directory # Replace with the desired path for Nautobot static files
         type: Directory
     - name: nautobot-db-data
       hostPath:
         path: /absolute/path/to/your/database/data # Replace with the desired path for Postgres data
         type: Directory
   ```

### 3. Start the Nautobot Pod

To start the Podman pod defined in the YAML configuration file:

```bash
podman play kube nautobot.yaml
```

This command will start up the necessary containers for Nautobot, Postgres, Redis, Nautobot Celery Beat, and Celery Workers, all configured within a single Pod.

### 4. Create a Superuser

Once the containers are running, you’ll need to create a superuser to access the Nautobot web interface. Run the following command:

```bash
podman exec -it nautobot-app nautobot-server createsuperuser
```

This command will prompt you to enter a username, email, and password for the superuser account.

### 5. Accessing Nautobot

Once the setup is complete, you should be able to access Nautobot via `http://localhost:8080` (or the appropriate port specified in your YAML file). Log in with the superuser credentials you created.

## Components Included

- **Nautobot app**: The core Nautobot application.
- **Postgres**: Database for Nautobot.
- **Redis**: Caching and queuing backend for Nautobot.
- **Nautobot Celery Beat**: Manages scheduled tasks for Nautobot.
- **Nautobot Celery Worker**: Processes background tasks asynchronously for Nautobot.

## Notes

This configuration provides a quick setup for local development and testing with Nautobot on Podman. It’s intended to help you get started but may require additional configuration for a production-ready deployment.

---

This `README.md` now reflects instructions for updating volume mounts to specify custom paths for configuration and data persistence.