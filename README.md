# Docker Compose for SonarQube, PostgreSQL, and Nginx

This repository contains a `docker-compose.yml` file for setting up a development environment with SonarQube, PostgreSQL, and Nginx. The configuration ensures that the containers restart automatically unless manually stopped.

## Services

### SonarQube

- **Image**: `sonarqube:latest`
- **Container Name**: `sonarqube`
- **Ports**: `9000:9000`
- **Environment Variables**:
  - `SONAR_JDBC_URL`: `jdbc:postgresql://db:5432/sonarqube`
  - `SONAR_JDBC_USERNAME`: `sonarqube`
  - `SONAR_JDBC_PASSWORD`: `sonarqube_b1d88f9419ef949ae56`
- **Volumes**:
  - `sonarqube_data`: `/opt/sonarqube/data`
  - `sonarqube_extensions`: `/opt/sonarqube/extensions`
  - `sonarqube_logs`: `/opt/sonarqube/logs`
- **Restart Policy**: `unless-stopped`
- **Depends On**: `db`

### PostgreSQL

- **Image**: `postgres:15.2-alpine`
- **Container Name**: `sonarqube_db`
- **Ports**: `5432:5432`
- **Environment Variables**:
  - `POSTGRES_USER`: `sonarqube`
  - `POSTGRES_PASSWORD`: `sonarqube_b1d88f9419ef949ae56`
  - `POSTGRES_DB`: `sonarqube`
- **Volumes**:
  - `postgres_data`: `/var/lib/postgresql/data`
- **Restart Policy**: `unless-stopped`

### Nginx

- **Image**: `nginx:latest`
- **Container Name**: `nginx`
- **Ports**: `80:80`
- **Volumes**:
  - `./nginx.conf`: `/etc/nginx/nginx.conf`
- **Restart Policy**: `unless-stopped`
- **Depends On**: `sonarqube`

## Volumes

- `sonarqube_data`
- `sonarqube_extensions`
- `sonarqube_logs`
- `postgres_data`

## Network

- **Name**: `sonarnet`
- **Driver**: `bridge`

## How to Use

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/krtvelaz/SonarQube-DockerCompose.git
   cd SonarQube-DockerCompose
   ```

2. **Start the Services**:

   ```bash
   docker-compose up -d
   ```

3. **Verify the Containers**:

   You can check the status of the containers using:

   ```bash
   docker ps
   ```

4. **Access SonarQube**:

   Open your web browser and navigate to `http://localhost:9000`.

## Notes

- Ensure that Docker and Docker Compose are installed on your system.
- The restart policy `unless-stopped` ensures that the containers restart automatically after a host reboot unless they are stopped manually.
