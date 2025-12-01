## Video app — deployment & upload troubleshooting

When uploads fail on a deployed instance but work locally, this is commonly caused by missing environment variables or the runtime not allowing temporary files to be read by the server.

Quick checks:

- Ensure the server environment has the API key available under `VITE_GEMINI_API_KEY`.
  - Locally the dev server reads `.env` (see `package.json` dev script) or you can set the environment variable before running the server.
- If your hosting provider prevents the runtime from writing to `/tmp`, change `multer`'s destination in `server/index.mjs` to a writable location.
- If the upload fails the server will now return a helpful JSON message in the response body (e.g. `{ "error": "VITE_GEMINI_API_KEY is not set on the server" }`). These messages will be visible in the UI.

How to run locally for quick debugging:

1. Add a `.env` file in `/video` with a valid key (or set `VITE_GEMINI_API_KEY` in your shell):

   VITE_GEMINI_API_KEY=your_api_key_here

2. Start the app:

   npm run dev

3. Use the included curl helper (`test-upload.sh`) to simulate uploading a file to the server:

   ./test-upload.sh /path/to/sample.mp4

This will show the raw API response and help identify cases like missing API keys, invalid mime types, or other upload errors.
