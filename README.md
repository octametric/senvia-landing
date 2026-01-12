## Deploy

Upload the **contents of `dist/`** so your host serves:

- `/index.html`
- `/assets/...` (hashed JS/CSS bundles)
- any root assets copied from `public/` that are referenced at runtime (e.g. `/senvia-full-logo.png`, `/googleplay.svg`)

Tip: If your platform expects a single artifact, zip the **contents** of `dist/` (not the parent folder).

## Quick verification (recommended)

1. Open `dist/index.html` and confirm it references `/assets/...`.
2. Ensure any root references like `/senvia-artwork-only.png` exist in `dist/`.

## If hosting under a sub-path

If you will host the site at a sub-path like `https://example.com/landing/`, you must build with Vite `base` set to `/landing/` (otherwise `/assets/...` will 404).

---

## Option A: AWS S3 static website hosting (dist-only)

1. Create an S3 bucket (optionally named after your domain).
2. Enable **Static website hosting** for the bucket.
3. Set:
   - **Index document:** `index.html`
   - **Error document:** `index.html` (safe default; useful if you later add client-side routes)
4. Upload the **contents of `dist/`** to the bucket root:
   - `index.html`
   - `assets/`
   - any root images (e.g. `senvia-full-logo.png`)
5. Make the bucket content publicly readable (website hosting requires it) or use CloudFront (recommended for production).

### Optional: CloudFront (recommended)

- Create a CloudFront distribution in front of the S3 bucket.
- Set **Default root object** to `index.html`.
- If you need SPA fallback routing later, configure custom error responses for 403/404 to return `/index.html` with status 200.

---

## Option B: Standard static HTTP server

### Python

Run from inside `dist/`:

```bash
python -m http.server 8080
```

Open `http://localhost:8080`.

### Node

Run from inside `dist/`:

```bash
npx serve .
```

---

## Common pitfalls

- **Wrong upload level:** Don’t upload a `dist` folder inside another folder; your host should serve `index.html` at the root URL.
- **Sub-path hosting without `base`:** If you deploy under `/landing/`, rebuild with Vite `base` set accordingly.
- **Missing root assets:** Any `/foo.png` reference must exist at the deployed site root (it should be present in `dist/` if it came from `public/` and was referenced).
