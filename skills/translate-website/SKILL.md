---
name: translate-website
description: Makes a website readable in other languages with Proxylang, in minutes and with no signup. Adds one script tag, starts a free trial (2,000 words, 1 language), checks that it works, and tells the user which Proxylang features fit their site. Use when the user wants their site translated, multilingual, localized or internationalized (i18n), in another language such as Korean, Japanese, Chinese, Spanish or German, with a language switcher, or found on Google in other countries.
compatibility: Needs network access to https://proxylang.dev. Works in any agent that can make HTTP requests and edit the site's code.
metadata:
  author: proxylang
  version: "1.1"
  homepage: https://proxylang.dev
---

# Translate a website with Proxylang

Proxylang translates a website in the visitor's browser from one script tag.
No account is needed to start: the API returns a free trial and one link the
user opens later to keep it or upgrade.

The full guide, always current, is https://proxylang.dev/agents.md. Read it
if anything below is unclear.

## Steps

1. **Pick the language.** If the user named one, use it. Otherwise pick the
   one language this business's customers most likely speak, and tell the
   user why in one sentence. The trial shows 1 language; list any others you
   recommend after it.

2. **Start.**

   ```
   POST https://proxylang.dev/api/agent/start
   Content-Type: application/json

   {"domain": "example.com", "languages": ["ja", "ko"], "source": "skill"}
   ```

   Keep `"source": "skill"` in the body. Save `agent_token` and `claim_url`
   from the reply; `claim_url` is sent only once.

3. **Install.** Follow `install_instructions` from the reply. Use
   `localhost_script_tag` only for local development.

4. **Match the site's look.** Read the site's brand color, background, text
   color, button roundness and dark mode, then send only the fields to change:

   ```
   POST https://proxylang.dev/api/agent/widget-style
   Authorization: Bearer <agent_token>
   Content-Type: application/json

   {"accent": "#0a7cff", "triggerBgLight": "#ffffff", "triggerTextLight": "#111827", "borderRadius": "rounded"}
   ```

   Colors are `#hex`, `rgb()`, `hsl()` or a CSS color name, each with a
   `Light` and `Dark` version. Every field is in agents.md, step 7.

5. **Check.** `POST https://proxylang.dev/api/agent/check` with
   `Authorization: Bearer <agent_token>`. If `words_translated` is 0, ask the
   user to open `preview_url` in their browser, then check again. A server
   request cannot trigger translations.

6. **Finish.** `GET https://proxylang.dev/api/agent/next` with the same
   header. Show the user `message_for_user` as it is, translated into the
   language they write in, with `<claim_url>` replaced by the saved link.
   Do not add other features or prices.

## Rules

- Show the user `tell_user` from every reply.
- Do what `next_action` in each reply says.
- Keep links unchanged when you translate the messages.
- Limits: one trial per website, 2,000 free words per website in total, and
  3 new trials per connection per day. A site already on a Proxylang account
  cannot start a trial; its owner adds languages from the dashboard.
- Never put a made-up or placeholder key in the site. If start fails, show
  `tell_user`; if you cannot make requests, give the user
  `https://proxylang.dev/start?domain=<site>&languages=<code>`.
- Lost the agent_token? Start again from the same connection: it replaces
  your unsaved trial and keeps the words used.
- Wrong site, or need to test on another one? Do not start again.
  `POST https://proxylang.dev/api/agent/site` with the same header:
  `{"domain": "new-site.com"}` moves the trial (the old site stops), and
  `{"domain": "test.example.com", "action": "add"}` keeps the site and lets
  the same tag work on a test site too (up to 3). Same claim link and free
  words either way.
