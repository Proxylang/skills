# Publishing checklist (for Titus)

Run these after https://proxylang.dev/mcp is live on production.

## 1. GitHub repo

Create the public repo `proxylang/skills` and push this folder to it. The
skill is then installable with `npx skills add proxylang/skills`, and it shows
up on skills.sh once people install it (skills.sh counts installs from the
`skills` CLI).

## 2. Official MCP Registry (domain name `dev.proxylang/translate`)

1. Install the publisher: `brew install mcp-publisher`
2. Make a key pair. macOS `openssl` can't do Ed25519, so use OpenSSL 3:

   ```bash
   brew install openssl@3
   OSSL=/opt/homebrew/opt/openssl@3/bin/openssl
   $OSSL genpkey -algorithm Ed25519 -out key.pem
   PUBLIC_KEY="$($OSSL pkey -in key.pem -pubout -outform DER | tail -c 32 | base64)"
   echo "proxylang.dev. IN TXT \"v=MCPv1; k=ed25519; p=${PUBLIC_KEY}\""
   ```

3. Add that TXT record on the apex `proxylang.dev` (not a subdomain) in
   Cloudflare DNS.
4. Log in and publish from this folder:

   ```bash
   PRIVATE_KEY="$($OSSL pkey -in key.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')"
   mcp-publisher login dns --domain proxylang.dev --private-key "$PRIVATE_KEY"
   mcp-publisher publish
   ```

5. Keep `key.pem` in the keys store and delete the local copy. Never commit it.

## 3. Smithery

Add the server at https://smithery.ai with the URL `https://proxylang.dev/mcp`
(no auth needed).
