# Cloudflare Claude Proxy

This project is a Cloudflare Worker that acts as a proxy for Claude API requests, forwarding them to Google AI Studio. It allows you to use Claude models through Google's infrastructure.

## Prerequisites

- A Cloudflare account
- Wrangler CLI installed (`npm install -g wrangler`)
- Google Cloud Platform account with AI Platform API enabled
- Google Cloud service account with necessary permissions

## Setup and Deployment

1. Clone this repository:
   ```
   git clone https://github.com/NuckyInSg/claude-proxy
   cd claude-proxy

   ```

2. Authenticate Wrangler with your Cloudflare account:
   ```
   wrangler login
   ```

3. Update the `wrangler.toml` file:
   - Ensure the `name` field is set to your desired worker name
   - Verify that the `main` field points to `src/worker.js`

4. Set up the required environment variables:
   ```
   wrangler secret put CLIENT_EMAIL
   wrangler secret put PRIVATE_KEY
   wrangler secret put PROJECT
   wrangler secret put API_KEY
   ```
   For each command, you'll be prompted to enter the corresponding value:
   - `CLIENT_EMAIL`: Your Google Cloud service account email
   - `PRIVATE_KEY`: Your Google Cloud service account private key (replace newlines with `\n`)
   - `PROJECT`: Your Google Cloud project ID
   - `API_KEY`: A custom API key for authenticating requests to your worker

5. Deploy the worker:
   ```
   wrangler deploy
   ```

6. After deployment, Wrangler will provide you with a URL for your worker. You can use this URL to make requests to Claude API through your proxy.

## Usage

To use the proxy, make POST requests to your worker's URL, setting the following headers:

- `Content-Type: application/json`
- `x-api-key: [Your API_KEY value]`

The request body should follow the Claude API format, with the addition of a `model` field to specify which Claude model to use.

Example:
```json
{
  "model": "claude-3-opus-20240229",
  "messages": [
    {
      "role": "user",
      "content": "Hello, Claude!"
    }
  ]
}
```

## Available Models

The proxy supports the following Claude models:
- claude-3-opus
- claude-3-sonnet
- claude-3-haiku
- claude-3-5-sonnet

You can also use versioned model names like `claude-3-opus-20240229`.

## Security Notes

- Keep your API_KEY secret and use HTTPS to make requests to your worker.
- The worker uses your Google Cloud credentials to authenticate with Google AI Studio. Ensure these credentials are kept secure.
- Consider implementing additional authentication mechanisms if needed for your use case.

## Troubleshooting

- If you encounter errors, check the Cloudflare Workers logs in the Cloudflare dashboard.
- Verify that all environment variables are correctly set.
- Ensure your Google Cloud service account has the necessary permissions to access the AI Platform API.

