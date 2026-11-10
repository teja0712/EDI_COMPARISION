# EDI Comparator — Cloudflare Workers

A static browser application hosted by a Cloudflare Worker. The Worker serves the HTML app; the browser reads and compares the selected EDI files locally. The comparator does not POST file contents to the Worker or store them.

## Project structure

```text
edi-comparator-cloudflare/
├── public/
│   └── index.html
├── src/
│   └── index.js
├── package.json
├── wrangler.toml
└── README.md
```

## Requirements

- A Cloudflare account
- Node.js (current LTS recommended)
- npm
- A terminal (PowerShell, Windows Terminal, macOS Terminal, or Linux shell)

## Deploy to Cloudflare Workers

1. Extract this project ZIP to a folder.
2. Open a terminal in the extracted `edi-comparator-cloudflare` folder.
3. Install dependencies:

   ```bash
   npm install
   ```

4. Sign in to Cloudflare:

   ```bash
   npx wrangler login
   ```

   A browser window opens. Sign in and authorize Wrangler.

5. Test locally:

   ```bash
   npm run dev
   ```

   Open the local URL printed by Wrangler (usually `http://localhost:8787`).
   Test with sample files before publishing.

6. Deploy:

   ```bash
   npm run deploy
   ```

7. Wrangler prints your live `workers.dev` URL. Open it to use the comparator.

## Optional custom domain

In the Cloudflare dashboard, open **Workers & Pages**, select `edi-comparator`, then configure a route or custom domain. Your domain must be in the Cloudflare account.

## Privacy and data handling

- The browser uses the File API to read the two selected files.
- The diff algorithm runs in the browser.
- This app contains no upload endpoint, database, analytics, or file-storage code.
- The Worker serves static application assets and a `/health` endpoint only.
- Cloudflare necessarily receives normal website requests and may process standard request metadata. The EDI file contents are not included in those requests by this app.
- Use only files you are authorized to process and follow your organization's healthcare-data/security policies. Do not treat this app as an approved HIPAA-compliant service without your organization's review.

## Notes

- This project uses Cloudflare Workers Static Assets (`[assets]` in `wrangler.toml`).
- `npm run dev` runs the same Worker locally.
- `npm run deploy` publishes the Worker and its static assets.
- The comparison is line-based. It does not validate EDI compliance or adjudicate claims.
- For extremely large files with many thousands of unrelated differences, any exact diff can require substantial browser memory and CPU. Test using representative files.

## Troubleshooting

**`npm` is not recognized**: install Node.js LTS, close and reopen the terminal, then run `node -v` and `npm -v`.

**Wrangler asks to authenticate**: run `npx wrangler login` again.

**Deployment name already exists**: change `name` in `wrangler.toml` to a unique Worker name, then deploy.

**Worker URL opens but app does not**: verify `public/index.html` exists and the `[assets] directory` is `./public`.
