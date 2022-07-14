WIP, just demo repo

Fork of https://github.com/andrefelipe/vite-php-setup, this setup uses a custom port (5133).

## Run it

```bash
ddev exec "cd vite && npm install"
ddev launch
ddev exec "cd vite && npm run dev"
# reload browser now, vite should be active
```

## What was changed/added?

```bash
ddev config --project-type=php --docroot="public"
ddev start
```

- Added `.ddev/docker-compose.vite.yaml` for ports
- `ddev restart`
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

See commit history for full documentation of changes. 