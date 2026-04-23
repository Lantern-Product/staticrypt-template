# @lantern-product/staticrypt-template

Lantern-branded [staticrypt v3](https://github.com/robinmoisson/staticrypt) password page template.

Matches the Lantern lander: Poppins font, cream/sage gradient background, inlined Lantern logo, warm amber-orange button.

## Usage

Install in any repo that uses staticrypt:

```bash
npm install @lantern-product/staticrypt-template
```

Then pass the template path to staticrypt in your build command:

```bash
npx staticrypt@3 pages/your-file.html \
  --password "$PAGE_PASSWORD" \
  --directory dist \
  --remember false \
  --short \
  --template node_modules/@lantern-product/staticrypt-template/template.html
```

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

Add `PAGE_PASSWORD` in Vercel → Settings → Environment Variables.

Vercel automatically runs `npm install` when a `package.json` is present, so the template is available before the build command runs.

## Template variables

All standard staticrypt v3 CLI flags work as expected:

| Flag | Default |
|---|---|
| `--template-title` | `"Protected Page"` |
| `--template-instructions` | `""` |
| `--template-placeholder` | `"Password"` |
| `--template-button` | `"DECRYPT"` |
| `--template-error` | `"Bad password!"` |
