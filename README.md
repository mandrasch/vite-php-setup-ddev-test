# vite-php-setup-ddev-test

This repo adds DDEV support for https://github.com/andrefelipe/vite-php-setup. 

The new default vite 3 port `5173` is used in this example. It is assumed that you visit your DDEV site via `https://`. 

Thanks very much to @andrefelipe for providing!

## Set up & run it

```bash
ddev exec "cd vite && npm install"
# Add symlink for assets, 
# see https://github.com/andrefelipe/vite-php-setup#known-issue-2-during-dev-only
ddev exec "ln -s /var/www/html/vite/src/assets /var/www/html/public/assets"

# open your site in browser, vite is not active yet
ddev launch

# run vite & reload your browser window
ddev exec "cd vite && npm run dev" 

# vite should now be active, change something in src/main.js 
# or src/styles/examples.css to check if hot module reloading 
# works, e.g. add alert('hello world'). 
```

- If you run into "502 broken DDEV backend", a `ddev restart` sometimes helps

## What was changed/added after forking?

- Init DDEV: 

```bash
ddev config --project-type=php --docroot="public"
ddev start
```

- Added `.ddev/docker-compose.vite.yaml` for DDEV router port handling (run `ddev restart` afterwards):

```yaml
version: '3.6'
services:
  web:
    expose:
      - '5173'
    environment:
      - HTTP_EXPOSE=${DDEV_ROUTER_HTTP_PORT}:80,${DDEV_MAILHOG_PORT}:8025,5174:5173
      - HTTPS_EXPOSE=${DDEV_ROUTER_HTTPS_PORT}:80,${DDEV_MAILHOG_HTTPS_PORT}:8025,5173:5173
```

- Set `const VITE_HOST = 'https://vite-php-setup-ddev-test.ddev.site:5173';` in `public/helpers.php`
- Set `isDev` to true when `.ddev.site` is in URL  in `public/helpers.php`
- Modified `vite/vite.config.js` server settings:

```javascript
 server: {
    // respond to all network requests
    host: '0.0.0.0',
    // we need a strict port to match on PHP side
    strictPort: true,
    port: 5173
  },
```

See commit history for full documentation of changes:

https://github.com/mandrasch/vite-php-setup-ddev-test/commit/805cdb57d823894ebe24a0f37d708f350847357b

## More resources / other approaches
 
- [Discussion with fellow DDEV users](https://github.com/torenware/ddev-viteserve/issues/2#issuecomment-1184472413)
- Other approaches
  - https://github.com/iammati/vite-ddev
  - https://github.com/torenware/ddev-viteserve
- Laravel + Vite + DDEV (WIP): https://github.com/mandrasch/ddev-laravel-breeze-vite
- Vite + DDEV in Craft CMS: https://nystudio107.com/docs/vite/#using-ddev (uses `ports` which is not recommended, see [comment by @rfay](https://github.com/torenware/ddev-viteserve/issues/2#issuecomment-1184472413)) via [craftquest](https://craftquest.io/courses/ddev-and-craft-cms-quick-start-guide/43674)
