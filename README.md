# @lantern-product/staticrypt-template

Lantern-branded [staticrypt v3](https://github.com/robinmoisson/staticrypt) password page template.

Matches the Lantern lander: Poppins font, cream/sage gradient background, inlined Lantern logo, warm amber-orange button.

## How it works

staticrypt takes any `.html` file, encrypts its contents with a password using AES-256, and wraps the result in a template — this one. When a visitor loads the page, they see the Lantern-branded password prompt. On correct entry, staticrypt decrypts and renders the original page entirely in the browser. No server required.

## Installation

```bash
npm install @lantern-product/staticrypt-template
```

## Usage

Pass the template path to staticrypt in your build command:

```bash
npx staticrypt@3 pages/your-file.html \
  --password "$PAGE_PASSWORD" \
  --directory dist \
  --remember false \
  --short \
  --template node_modules/@lantern-product/staticrypt-template/template.html
```

## Project type examples

### Single HTML file

Encrypt any `.html` file directly:

```bash
npx staticrypt@3 index.html \
  --password "$PAGE_PASSWORD" \
  --directory dist \
  --remember false \
  --short \
  --template node_modules/@lantern-product/staticrypt-template/template.html
```

### Single Page App (Vite, Create React App, Vue CLI)

Run staticrypt after your framework's build step, targeting the output `index.html`:

```bash
npm run build && npx staticrypt@3 dist/index.html \
  --password "$PAGE_PASSWORD" \
  --directory dist \
  --remember false \
  --short \
  --template node_modules/@lantern-product/staticrypt-template/template.html
```

### Static site generator (Next.js static export, Astro, Hugo, Eleventy)

Same pattern — run staticrypt after the build, pointing at the generated HTML:

```bash
# Next.js static export
next build && npx staticrypt@3 out/index.html \
  --password "$PAGE_PASSWORD" \
  --directory out \
  --remember false \
  --short \
  --template node_modules/@lantern-product/staticrypt-template/template.html
```

### Multi-page sites

staticrypt encrypts one file at a time. To protect every page, loop over the output files:

```bash
npm run build
for f in dist/**/*.html; do
  npx staticrypt@3 "$f" \
    --password "$PAGE_PASSWORD" \
    --directory "$(dirname "$f")" \
    --remember false \
    --short \
    --template node_modules/@lantern-product/staticrypt-template/template.html
done
```

## Deploying

### Vercel (`vercel.json`)

```json
{
  "buildCommand": "npx staticrypt@3 pages/your-file.html --password \"$PAGE_PASSWORD\" --directory dist --remember false --short --template node_modules/@lantern-product/staticrypt-template/template.html",
  "outputDirectory": "dist",
  "redirects": [
    { "source": "/", "destination": "/your-file.html", "permanent": false }
  ]
}
```

Vercel automatically runs `npm install` when a `package.json` is present, so the template is available before the build command runs.

### Netlify (`netlify.toml`)

```toml
[build]
  command = "npx staticrypt@3 index.html --password \"$PAGE_PASSWORD\" --directory dist --remember false --short --template node_modules/@lantern-product/staticrypt-template/template.html"
  publish = "dist"
```

### GitHub Pages (GitHub Actions)

```yaml
- name: Install dependencies
  run: npm install

- name: Encrypt page
  run: |
    npx staticrypt@3 index.html \
      --password "${{ secrets.PAGE_PASSWORD }}" \
      --directory dist \
      --remember false \
      --short \
      --template node_modules/@lantern-product/staticrypt-template/template.html

- name: Deploy to GitHub Pages
  uses: actions/upload-pages-artifact@v3
  with:
    path: dist
```

### Cloudflare Pages

Set the build command in the Cloudflare Pages dashboard:

```
npm install && npx staticrypt@3 index.html --password "$PAGE_PASSWORD" --directory dist --remember false --short --template node_modules/@lantern-product/staticrypt-template/template.html
```

Set the output directory to `dist`.

## Environment variables

All platforms expect `PAGE_PASSWORD` to be set as a secret environment variable. Never commit the password to the repository.

| Platform | Where to add it |
|---|---|
| Vercel | Project → Settings → Environment Variables |
| Netlify | Site → Site configuration → Environment variables |
| GitHub | Repository → Settings → Secrets and variables → Actions |
| Cloudflare Pages | Project → Settings → Environment variables |

> **Need to get set up?** Reach out to Courtny to get deployment access and the `PAGE_PASSWORD` value for your project.

## Template variables

All standard staticrypt v3 CLI flags work as expected:

| Flag | Default |
|---|---|
| `--template-title` | `"Protected Page"` |
| `--template-instructions` | `""` |
| `--template-placeholder` | `"Password"` |
| `--template-button` | `"DECRYPT"` |
| `--template-error` | `"Bad password!"` |
