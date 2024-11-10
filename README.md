Here's the updated `README.md` with the inclusion of **Nautobot Celery Beat** and **Nautobot Celery Worker**:

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

### 2. Configure Secrets for Postgres

This setup requires a PostgreSQL password, which should be handled securely. You need to create a Postgres secret that will be referenced within the pod YAML file.

1. Open the `nautobot.yaml` file (or the equivalent pod YAML file you're using).
2. Replace the placeholder password in the `POSTGRES_PASSWORD` environment variable under the `nautobot-db` container definition with a secure value. You should ideally use a more secure secret management strategy rather than hardcoding passwords in the YAML file.

   **Example**:
   ```yaml
   env:
     - name: POSTGRES_PASSWORD
       value: "your_secure_password_here"
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

## Security Considerations

The current configuration includes sensitive information (e.g., database passwords) directly within the YAML file. **For a production environment**, you should:
- Use Podman’s secret management or environment files to handle sensitive data.
- Configure more robust security settings, including network restrictions and SSL configurations, for both the database and Nautobot.

## Components Included

- **Nautobot app**: The core Nautobot application.
- **Postgres**: Database for Nautobot.
- **Redis**: Caching and queuing backend for Nautobot.
- **Nautobot Celery Beat**: Manages scheduled tasks for Nautobot.
- **Nautobot Celery Worker**: Processes background tasks asynchronously for Nautobot.

## Notes

This configuration provides a quick setup for local development and testing with Nautobot on Podman. It’s intended to help you get started but may require additional configuration for a production-ready deployment.

---

This updated `README.md` now includes descriptions for **Nautobot Celery Beat** and **Nautobot Celery Worker** to reflect the full functionality included in the setup.