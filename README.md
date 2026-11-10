# EDI Comparator — GitHub + Cloudflare Workers

## Correct repository structure

```text
edi-comparator/
├── public/
│   └── index.html
├── src/
│   └── index.js
├── package.json
├── wrangler.toml
└── README.md
```

Keep any other existing project files (for example `EDI_Amount_Adjuster.html`) if you still use them.

## Upload to GitHub

1. Download and extract this ZIP.
2. Open your `edi-comparator` GitHub repository.
3. Choose **Add file → Upload files**.
4. Upload the `public` folder, `src` folder, `package.json`, `wrangler.toml`, and `README.md` from the extracted project. GitHub should show the folders and files, not just the ZIP.
5. Commit the changes to your deployment branch (normally `main`).

Do not upload the ZIP itself as the only repository file.

## Connect Cloudflare Workers to GitHub

1. In Cloudflare Dashboard, open **Workers & Pages**.
2. Choose **Create application → Workers** and select the GitHub deployment option (wording may vary).
3. Authorize/reconnect the GitHub account if prompted and grant access to the `edi-comparator` repository.
4. Select the repository and branch (`main`).
5. Use the repository root as the root directory.
6. The project is configured with `wrangler.toml`; the Worker entry point is `src/index.js`, and static assets are in `public`.
7. Deploy. Cloudflare should publish the Worker and provide a `workers.dev` URL.

If Cloudflare shows the Git account as disconnected, fix the GitHub app connection under GitHub **Settings → Applications → Installed GitHub Apps** (or the installation configuration page) and grant access to this repository. Missing `index.html` does not cause the Git-account-disconnected warning; it causes the app content to be missing after a deployment.

## Test

Open the deployed URL. `/health` should display `ok`. The root URL should show the EDI Comparator.

## Privacy

The browser reads the selected files using the File API and performs the comparison in JavaScript. This project has no file-upload endpoint, database, or file storage. The Worker serves the web app; it does not receive EDI file contents from the comparator. Standard website request metadata is still handled by Cloudflare. Follow your organization's approval and security requirements before using real healthcare data.

## Local preview (optional)

Install Node.js, then in the project folder run:

```bash
npm install
npx wrangler dev
```

Open the local URL printed by Wrangler.

## Notes

This is a line-based text comparison tool, not an EDI validator or claims adjudication system. Very large files with extensive differences can still use significant browser memory and CPU.
