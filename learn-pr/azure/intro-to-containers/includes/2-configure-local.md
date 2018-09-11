Before you run a container or container-integrated application in Azure, you'll most likely work in a local development environment like your laptop. This unit helps you prepare your system for container development with Docker.

## Docker for Windows and Mac

Docker, Inc. has published two applications to install and configure local container development environments. Essentially, each application prepares your system with Docker tooling, such as the necessary CLI and automation tools. A virtual machine is also created that hosts the Docker platform. The environment is configured such that Docker commands are passed through to the virtual machine. Each of these applications is similar in functionality and includes the following features:

- **Docker platform:** The core components necessary to create and run containers.
- **Docker CLI:** The command-line interface for interacting with Docker containers.
- **Docker Compose:** Automation tooling for defining and running multi-container applications.

Open the appropriate link below in a new tab to install Docker on your operating system. 

- [Docker for Windows](https://www.docker.com/docker-windows)
- [Docker for Mac](https://www.docker.com/docker-mac)

## Docker for Windows environments

When you use Docker for Windows, two environments are available: Linux and Windows. Using the Linux environment allows you to run Linux containers on your Windows system. You can select an environment by right-clicking on the Docker task bar icon, selecting **Switch to Linux containers**, and following the on-screen prompts.

![Docker for Windows, switch to Linux containers](../media-draft/2-docker-linux.png)

> [!NOTE]
> The steps in this tutorial assume that your system is configured to work with Linux containers.

## Docker on Linux

If you're working on a Linux-based system, the Docker server components and CLI tools can be manually installed. Follow the instructions found on [About Docker CE](https://docs.docker.com/install/#server) for your specific Linux distribution.

## Validate configuration

To validate that Docker has been successfully installed and configured, open a terminal and run the following command:

```bash
docker search nginx
```

If you see output similar to the following, your environment is ready for the next unit.

```output
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                                                  Official build of Nginx.                        9034                [OK]
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1362                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   589                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   390                                     [OK]
kong                                                   Open-source Microservice & API Management la…   204                 [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              106                                     [OK]
kitematic/hello-world-nginx                            A light-weight nginx container that demonstr…   102
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   59                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      54                                      [OK]
linuxserver/nginx                                      An Nginx container, brought to you by LinuxS…   37
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          36                                      [OK]
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
nginxdemos/nginx-ingress                               NGINX Ingress Controller for Kubernetes . Th…   11
wodby/drupal-nginx                                     Nginx for Drupal container image                9                                       [OK]
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          9                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   7                                       [OK]
centos/nginx-18-centos7                                Platform for running nginx 1.8 or building n…   6
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
centos/nginx-112-centos7                               Platform for running nginx 1.12 or building …   3
pebbletech/nginx-proxy                                 nginx-proxy sets up a container running ngin…   2                                       [OK]
toccoag/openshift-nginx                                Nginx reverse proxy for Nice running on same…   1                                       [OK]
travix/nginx                                           NGinx reverse proxy                             1                                       [OK]
ansibleplaybookbundle/nginx-apb                        An APB to deploy NGINX                          0                                       [OK]
mailu/nginx                                            Mailu nginx frontend                            0                                       [OK]
```

## Summary

In this unit, you prepared a local container development environment. In the next unit, you will learn about some basic Docker operations.