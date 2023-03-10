# Kirby Headless Starter

> âšī¸ Send a request with a `Authorization: Bearer test` header to the [live playground](https://kirby-headless-starter.jhnn.dev) for an example response.

This starter kit converts any Kirby site a truly headless-first CMS. No visual data shall be presented. You will only be able to fetch JSON-encoded data â either by using Kirby's default template system or use KQL to fetch data in your consuming application.

Routing and JSON-encoded responses are handled by the internal [kirby-headless](https://github.com/johannschopplich/kirby-headless) plugin, specifically its [global routes](https://github.com/johannschopplich/kirby-headless/blob/main/src/extensions/routes.php) and [API routes](https://github.com/johannschopplich/kirby-headless/blob/main/src/extensions/api.php) for KQL.

This project works well with [nuxt-kql](https://nuxt-kql.jhnn.dev).

## Example Projects

- [kirby-nuxt-starterkit](https://github.com/johannschopplich/kirby-nuxt-starterkit): đ Kirby's sample site â ported to Nuxt 3 and KirbyQL

## Key Features

- đĻ­ Optional bearer token for authentication
- đ **public** or **private** API
- đ§Š [KQL](https://github.com/getkirby/kql) with bearer token support via new `/api/kql` route
- âĄī¸ Cached KQL queries
- đ Multilang support for KQL queries
- đ [Templates](./site/templates/) present JSON instead of HTML
- đĩâđĢ No CORS issues!
- đĸ Build your own [API chain](https://github.com/johannschopplich/kirby-headless/blob/main/src/extensions/routes.php)
- đĻž Express-esque [API builder](https://github.com/johannschopplich/kirby-headless#api-builder) with middleware support

## Use Cases

If you intend to fetch data from a headless Kirby instance, you have two options with this plugin:

- 1ī¸âŖ use [Kirby's default template system](#templates)
- 2ī¸âŖ use [KQL](#kirbyql)

Head over to the [usage](#usage) section for instructions.

## Prerequisites

- PHP 8.0+

> Kirby is not a free software. You can try it for free on your local machine but in order to run Kirby on a public server you must purchase a [valid license](https://getkirby.com/buy).

## Setup

### Composer

Kirby-related dependencies are managed via [Composer](https://getcomposer.org) and located in the `vendor` directory. To install them, run:

```bash
composer install
```

### Environment Variables

Duplicate the [`.env.development.example`](.env.development.example) as `.env`:

```bash
cp .env.development.example .env
```

Optionally, adapt it's values.

> âšī¸ Make sure to set the correct requesting origin instead of the wildcard `KIRBY_HEADLESS_ALLOW_ORIGIN=*` for your deployment.

## Usage

### Private vs. Public API

It's recommended to secure your API with a token. To do so, set the environment variable `KIRBY_HEADLESS_API_TOKEN` to a token string of your choice.

You will then have to provide the HTTP header `Authentication: Bearer ${token}` with each request.

> â ī¸ Without a token your page content will be publicly accessible to everyone.

> âšī¸ The internal `/api/kql` route will always enforce bearer authentication, unless you explicitly disable it in your config (see below).

### Templates

> đ [See documentation in `kirby-headless` plugin](https://github.com/johannschopplich/kirby-headless#templates)

### KirbyQL

> đ [See documentation in `kirby-headless` plugin](https://github.com/johannschopplich/kirby-headless#kirbyql)

### Panel Settings

#### Preview URL to the Frontend

With the headless approach, the default preview link from the Kirby Panel won't make much sense, since it will point to the backend API itself. Thus, we have to overwrite it utilizing a custom page method in your site/page blueprints:

```yaml
options:
  # Or `site.frontendUrl` for the `site.yml`
  preview: "{{ page.frontendUrl }}"
```

Set your frontend URL in your `.env`:

```
KIRBY_HEADLESS_FRONTEND_URL=https://example.com
```

If left empty, the preview button will be disabled.

#### Redirect to the Panel

Editors visiting the headless Kirby site may not want to see any API response, but use the Panel solely. To let them automatically be redirected to the Panel, set the following option in your Kirby configuration:

```php
# /site/config/config.php
return [
    'headless' => [
        'panel' => [
            'redirect' => false
        ]
    ]
];
```

### Deployment

> âšī¸ See [ploi-deploy.sh](./scripts/ploi-deploy.sh) for exemplary deployment instructions.

> âšī¸ Some hosting environments require to uncomment `RewriteBase /` in [`.htaccess`](./public/.htaccess) to make site links work.

## License

[MIT](./LICENSE) License ÂŠ 2022-2023 [Johann Schopplich](https://github.com/johannschopplich)
