# vite-php-setup-ddev-test

This repo adds DDEV support for  https://github.com/andrefelipe/vite-php-setup. Vite 3 is used, thanks very much to @andrefelipe for providing.

## Setup & run it

```bash
ddev exec "cd vite && npm install"
# Add symlink for assets, 
# see https://github.com/andrefelipe/vite-php-setup#known-issue-2-during-dev-only
ddev exec "ln -s /var/www/html/vite/src/assets /var/www/html/public/assets"

ddev exec "cd vite && npm run dev" 
# open/reload browser now, vite should be active
ddev launch
# change something in js/css to check if hot module reloading works
```

- If you run into "502 broken DDEV backend", a `ddev restart` sometimes helps

## What was changed/added after forking?

- Init DDEV: 

```bash
ddev config --project-type=php --docroot="public"
ddev start
```

- Added `.ddev/docker-compose.vite.yaml` for ports (run `ddev restart` afterwards)
- Set `const VITE_HOST = 'https://vite-php-setup-ddev-test.ddev.site:5133';` in `public/helpers.php`
- Set `isDev` to true when `.ddev.site` is in URL
- Modified `vite/vite.config.js` server settings:

```javascript
 server: {
    // respond to all network requests
    host: '0.0.0.0',
    // we need a strict port to match on PHP side
    // change freely, but update on PHP to match the same port
    // tip: choose a different port per project to run them at the same time
    strictPort: true,
    port: 5133
  },
```

See commit history for full documentation of changes:

https://github.com/mandrasch/vite-php-setup-ddev-test/commit/805cdb57d823894ebe24a0f37d708f350847357b

## More resources / other approaches
 
- https://github.com/torenware/ddev-viteserve/issues/2
- https://discord.com/channels/664580571770388500/993996599506259978 
- https://github.com/iammati/vite-ddev
- https://github.com/nystudio107/craft-vite/ (uses `ports` which is not recommended) via https://craftquest.io/courses/ddev-and-craft-cms-quick-start-guide/43674
