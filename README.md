# Magento2 Docker Environment

Docker containers:
* Web container (nginx + php-fpm + cron)
* Database (MariaDB)
* Elastic search
* RabbitMQ
* Redis (cache & sessions)
* Load balancer and ssl offloader (traefik)

to run environment simply execute: `docker-compose up`

Environment variables can be adjusted in file `.env`

to change magento main domain we can adjust `.env` file our run:

```
export MAGENTO_BASE_URL=magento2.local
docker-compose up magento2
```
__NOTICE: value without protocol__