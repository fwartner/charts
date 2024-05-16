- [Laravel Helm Chart](#laravel-helm-chart)
  - [🛑 Requirements](#-requirements)
  - [🚀 Installation](#-installation)
    - [📜 Environment variables](#-environment-variables)
    - [🤖 Run workers (non-HTTP workload)](#-run-workers-non-http-workload)
  - [📡 Monitoring](#-monitoring)
    - [🔥 Scraping PHP-FPM and NGINX Metrics](#-scraping-php-fpm-and-nginx-metrics)
    - [❤ Healthchecks](#-healthchecks)
  - [🐛 Testing](#-testing)
  - [🤝 Contributing](#-contributing)
  - [🔒  Security](#--security)
  - [🎉 Credits](#-credits)

Laravel Helm Chart
==================

Containerize & Orchestrate your Laravel application with this simple Helm chart.

## 🛑 Requirements

- Kubernetes v1.22+

## 🚀 Installation

To create a Laravel project image for Docker, head over to [fwartner/laravel-helm-demo](https://github.com/fwartner/laravel-helm-demo) to get started. The Dockerfile differs based on the project you want to deploy, and there you will find a demo application to run to understand the basics.

Install the Helm chart repository:

```bash
$ helm repo add fwartner https://helm.wartner.io
$ helm repo update
```

Install Laravel chart:

```bash
$ helm upgrade laravel-app \
    --install \
    --version=1.0.0 \
    wartnerio/laravel
```

Check `values.yaml` for additional available customizations.

### 📜 Environment variables

Laravel needs an `.env` file to keep secrets within, so you will need a secret from which they will be pulled. To do this, simply create a `laravel-app-env` secret with the `.env` key if your helm release is called `laravel-app`.

To replace the name, check for `app.envSecretName` in `values.yaml`. By default, the name is `<release name>-env`.

```yaml
kind: Secret
apiVersion: v1
metadata:
  name: laravel-app-env
stringData:
  .env: |
    APP_NAME=Laravel
    APP_ENV=production
    APP_KEY=base64:kLHmdtqS0YnTACWSpkV4w1GVOQMEXQ68Usk8WR+yauA=
    APP_DEBUG=true
    APP_URL=http://test.laravel.com

    LOG_CHANNEL=null
    LOG_LEVEL=debug

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD=

    BROADCAST_DRIVER=log
    CACHE_DRIVER=file
    QUEUE_CONNECTION=sync
    SESSION_DRIVER=file
    SESSION_LIFETIME=120

    MEMCACHED_HOST=127.0.0.1

    REDIS_HOST=127.0.0.1
    REDIS_PASSWORD=null
    REDIS_PORT=6379

    MAIL_MAILER=smtp
    MAIL_HOST=mailhog
    MAIL_PORT=1025
    MAIL_USERNAME=null
    MAIL_PASSWORD=null
    MAIL_ENCRYPTION=null
    MAIL_FROM_ADDRESS=null
    MAIL_FROM_NAME="${APP_NAME}"

    AWS_ACCESS_KEY_ID=
    AWS_SECRET_ACCESS_KEY=
    AWS_DEFAULT_REGION=us-east-1
    AWS_BUCKET=

    PUSHER_APP_ID=
    PUSHER_APP_KEY=
    PUSHER_APP_SECRET=
    PUSHER_APP_CLUSTER=mt1

    MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
    MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
```

### 🤖 Run workers (non-HTTP workload)

Workers can be for example long-lived commands, like `php artisan queue:work` commands or `php artisan horizon` that run in separate processes, other the web workers that serve HTTP content.

To deploy such workload, check the [Worker Chart](https://github.com/fwartner/charts/tree/master/charts/laravel-worker) that will ease the job for you.

## 📡 Monitoring

### 🔥 Scraping PHP-FPM and NGINX Metrics

PHP-FPM and NGINX containers within the Laravel app pod can expose metrics for Prometheus to scrape. When enabling the exporters, the following endpoints return Prometheus-readable metrics:

- `localhost:9253/metrics` - PHP-FPM metrics
- `localhost:9113/metrics` - NGINX metrics

### ❤ Healthchecks

Healthchecks are set up and enabled by default for both `/health` on the NGINX container (the web application that will serve the Laravel app) and the TCP :9000 port on PHP-FPM.

For convenience, you may use [renoki-co/laravel-healthcheck](https://github.com/renoki-co/laravel-healthchecks) to easily set up healthchecks in your app, just like in `app/Http/Controllers/HealthController`.

## 🐛 Testing

Coming soon.

## 🤝 Contributing

Please see [CONTRIBUTING](../../CONTRIBUTING.md) for details.

## 🔒  Security

If you discover any security related issues, please email florian@wartner.io instead of using the issue tracker.

## 🎉 Credits

- [All Contributors](../../../../contributors)
