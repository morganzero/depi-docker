# NOT WORKING ATM
# Dockerized Debian-Nginx-PHP-IonCube Stack for Traefik Reverse Proxy

This Docker container provides a pre-configured Debian (bullseye), Nginx, PHP (8.1), and Ioncube (DEPI) stack, allowing you to easily deploy web applications such as WHMCS or other similar platforms behind a Traefik reverse proxy. With Traefik, you can efficiently manage incoming requests and route them to the appropriate container.

## Prerequisites

Before you begin, ensure that you have the following components installed:

- Docker: [Installation Guide](https://docs.docker.com/get-docker/)
- Traefik: [Installation Guide](https://doc.traefik.io/traefik/)

## Deployment - docker-compose

To deploy the DEPI stack container with Traefik, follow these steps:

1. Create a docker-compose.yml file in your desired directory and copy the following content into it. For example, let's use the name "WHMCS" for the container:

```yaml
version: "3.9"
services:
  whmcs:
    image: docker.io/sushibox/depi:latest
    restart: always
    ports:
      - 80:80
    volumes:
      - /opt/depi:/var/www/html
    labels:
      - "traefik.enable=true"
      
  mariadb:
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: whmcs
      MYSQL_USER: whmcs
      MYSQL_PASSWORD: whmcs
      MYSQL_RANDOM_ROOT_PASSWORD: whmcs
    volumes:
      - /opt/depi/db:/var/lib/mysql

volumes:
  mysql:
```

2. Save the `docker-compose.yml` file.

3. Open a terminal or command prompt, navigate to the directory where you saved the `docker-compose.yml` file, and run the following command:

```bash
docker-compose up -d
```

4. Wait for the container to start and verify that it's running without any errors by running:

```bash
docker-compose ps
```

## Deployment - Docker run

1. Run the "whmcs" service:

```bash
docker run -d \
  --name whmcs \
  -p 80:80 \
  -v /opt/depi:/var/www/html \
  docker.io/sushibox/depi:latest
```

2. Run the "mariadb" service:

```bash
docker run -d \
  --name mariadb \
  -e MYSQL_DATABASE=whmcs \
  -e MYSQL_USER=whmcs \
  -e MYSQL_PASSWORD=whmcs \
  -e MYSQL_RANDOM_ROOT_PASSWORD=whmcs \
  -v /opt/depi/db:/var/lib/mysql \
  mariadb
```

## Configuration

By default, this container exposes port 80, allowing HTTP traffic to reach your web application. Ensure that you have set up Traefik correctly to handle incoming requests and route them to this container.

If you wish to customize the configuration or add additional services, you can modify the `docker-compose.yml` file accordingly.

## Volumes

This container uses the following volumes:

- `/var/www/html`: Mount this volume to persistently store your web application files.
- `/var/lib/mysql`: Mount this volume to persistently store the MariaDB database files.

You can adjust the volume paths in the `docker-compose.yml` file to match your desired locations on the host machine.

## Troubleshooting

If you encounter any issues or need further assistance, consider checking the following:

- Make sure you have the necessary permissions to bind ports and access the specified volumes.
- Verify that Traefik is properly configured and routes traffic to the correct container.

## License

This project is licensed under the [MIT License](LICENSE).

---

We hope you find this Dockerized DAPI stack container helpful for deploying your web applications with ease. If you have any questions or feedback, please don't hesitate to reach out. Happy coding!
