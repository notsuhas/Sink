## This deployment

This fork keeps Sink's application code unchanged except for one callback type annotation needed by typecheck. Cloudflare bindings live in `wrangler.jsonc` and production runtime settings are passed by the deployment Action; site and analytics tokens are runtime secrets. Sink runs at `link.theclau.de`, as a standalone service. The private referral assistant is one API client and stores its own recipient mappings.

Pushes to `master` run lint, typecheck, build, and upstream tests before deploying through `.github/workflows/deploy.yml`. Repository variables supply `DEPLOY_D1_DATABASE_ID` and `DEPLOY_KV_NAMESPACE_ID`. Secrets supply `CLOUDFLARE_ACCOUNT_ID`, `CLOUDFLARE_API_TOKEN`, `NUXT_CF_API_TOKEN`, and `NUXT_SITE_TOKEN`. Production builds have no site or analytics token; deployment installs them as runtime secrets. Deployment replaces plain Worker variables with the declared hosting settings so removed overrides do not persist. D1, KV, and the Analytics Engine dataset are named `sink`. Sink retains its standard AI binding and runtime defaults.

Only deployment and release sync are enabled; unrelated upstream Actions are disabled. Neither runs on pull requests. Deployment credentials are scoped to deployment steps, and dependency setup is pinned to a commit.

`.github/workflows/sync-upstream.yml` checks stable upstream releases hourly, tests the merge, then updates the fork and starts deployment. Conflicts, failed checks, and workflow changes require manual review. If deployment dispatch fails after the push, re-run the failed publish job or start Deploy Sink manually. GitHub may disable schedules after 60 inactive days.

Vue is declared explicitly for upstream unit tests, and Vitest disables remote Cloudflare bindings.

Use Node 24 and pnpm 11.11.0. Run lint, typecheck, build, and tests with a synthetic site token and no production environment file.

---

# ⚡ Sink

**A Simple, Speedy, Secure, and Serverless Link Shortener with Analytics, Running Entirely on Cloudflare.**

[Website](https://sink.cool) · [Documentation](https://docs.sink.cool) · [API Reference](https://sink.cool/_docs/scalar)

<a href="https://trendshift.io/repositories/20331" target="_blank">
  <img
    src="https://trendshift.io/api/badge/repositories/20331"
    alt="miantiao-me/Sink | Trendshift"
    width="250"
    height="55"
  />
</a>
<a href="https://news.ycombinator.com/item?id=40843683" target="_blank">
  <img
    src="https://hackernews-badge.vercel.app/api?id=40843683"
    alt="Featured on Hacker News"
    width="250"
    height="55"
  />
</a>
<a href="https://hellogithub.com/repository/57771fd91d1542c7a470959b677a9944" target="_blank">
  <img
    src="https://abroad.hellogithub.com/v1/widgets/recommend.svg?rid=57771fd91d1542c7a470959b677a9944&claim_uid=qi74Zp23wYKeAVB&theme=neutral"
    alt="Featured｜HelloGitHub"
    width="250"
    height="55"
  />
</a>
<a href="https://www.uneed.best/tool/sink" target="_blank">
  <img
    src="https://www.uneed.best/POTW1.png"
    alt="Uneed Badge"
    width="250"
    height="55"
  />
</a>

[<img src="https://devin.ai/assets/deepwiki-badge.png" alt="DeepWiki" height="20"/>](https://deepwiki.com/miantiao-me/Sink)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F69652?style=flat&logo=cloudflare&logoColor=white)
![Nuxt](https://img.shields.io/badge/Nuxt-00DC82?style=flat&logo=nuxtdotjs&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![shadcn/ui](https://img.shields.io/badge/shadcn/ui-000000?style=flat&logo=shadcnui&logoColor=white)
![License](https://img.shields.io/badge/License-AGPL--3.0-blue?style=flat)

![Hero](./public/image.png)

---

## ✨ Features

- **🔗 URL Shortening:** Compress your URLs to their minimal length.
- **📈 Analytics:** Monitor link analytics and gather insightful statistics.
- **☁️ Serverless:** Deploy without the need for traditional servers.
- **🎨 Customizable Slug:** Support personalized slugs, UTM parameters, and optional case-sensitive slug matching through configuration.
- **🪄 AI Assistance:** Optionally use Cloudflare Workers AI to generate slugs and OpenGraph metadata from page content.
- **⏰ Link Control:** Set expirations, passwords, and unsafe-link warning pages.
- **📱 Smart Routing:** Redirect visitors by device or country.
- **🖼️ Social Preview:** Customize social previews with titles, descriptions, and images.
- **📊 Near-real-time Analytics:** Display a live 3D globe and event logs using 10-second analytics polling and client-side replay, not SSE or WebSocket.
- **🔲 QR Code:** Generate QR codes for your short links.
- **📦 Import/Export:** Transfer links via JSON and export access analytics via CSV.
- **🌍 Multi-language:** Full i18n support for dashboard and redirect pages.

> [!TIP]
> **Who is Sink for?**
>
> Sink focuses on **individuals and small teams** who want a simple, self-hosted shortener on Cloudflare.
>
> For professional / business needs (managed service, multi-user, SLA, and more), use **[S.EE](https://sink.cool/see)**.

## 🪧 Demo

Experience the demo at [Sink.Cool](https://sink.cool/dashboard). Log in using the Site Token below:

```txt
Site Token: SinkCool
```

<details>
  <summary><b>Screenshots</b></summary>
  <img alt="Analytics" src="./docs/images/sink.cool_dashboard.png"/>
  <img alt="Links" src="./docs/images/sink.cool_dashboard_links.png"/>
  <img alt="Link Analytics" src="./docs/images/sink.cool_dashboard_link_slug.png"/>
</details>

## 🔀 Sibling versions

Sink and [Slite](https://github.com/miantiao-me/Slite) are sibling versions of the same link-management and analytics project. Sink runs on Cloudflare's serverless platform, while Slite runs as a local Node.js 24+/Docker process. They keep features, API contracts, and file organization compatible with each other wherever practical. Neither version is a legacy branch, and Slite is not a fork replacement for Sink.

## 🧱 Technologies Used

- **Framework**: [Nuxt 4](https://nuxt.com/)
- **Database**: [Cloudflare D1](https://developers.cloudflare.com/d1/) is the authoritative link store; [Workers KV](https://developers.cloudflare.com/kv/) is a write-through read cache
- **ORM**: [Drizzle ORM](https://orm.drizzle.team/)
- **Analytics Engine**: [Cloudflare Workers Analytics Engine](https://developers.cloudflare.com/analytics/)
- **Object Storage**: [Cloudflare R2](https://developers.cloudflare.com/r2/) for optional logical JSON snapshots
- **AI**: Optional [Cloudflare Workers AI](https://developers.cloudflare.com/workers-ai/)
- **UI Components**: [shadcn-vue](https://www.shadcn-vue.com/)
- **Styling:** [Tailwind CSS](https://tailwindcss.com/)
- **Deployment**: [Cloudflare](https://www.cloudflare.com/)

## 🚗 Roadmap [WIP]

We welcome your contributions and PRs.

- [x] Browser Extension - [Sink Tool](https://github.com/zhuzhuyule/sink-extension)
- [x] Chrome Extension - [Sink Quick Shorten](https://chromewebstore.google.com/detail/sink-quick-shorten/emlojomjpenjgkaphajcokijobpkejih)
- [x] Raycast Extension - [Raycast-Sink](https://github.com/foru17/raycast-sink)
- [x] Apple Shortcuts - [Sink Shortcuts](https://s.search1api.com/sink001)
- [x] iOS App - [Sink](https://apps.apple.com/app/id6745417598)
- [x] Enhanced Link Management (with Cloudflare D1)
- [x] Analytics Enhancements (Multi-link filtering)
- [x] Dashboard Performance Optimization (Infinite loading)
- [x] API, migration, backup, and redirect tests

## 🏗️ Deployment

> Video tutorial: [Watch here](https://www.youtube.com/watch?v=MkU23U2VE9E)

We currently support deployment to [Cloudflare Workers](https://docs.sink.cool/deployment/workers) (recommended) and [Cloudflare Pages](https://docs.sink.cool/deployment/pages) (deprecated).

## ⚒️ Configuration

[Configuration Docs](https://docs.sink.cool/configuration/)

## 🔌 API

[API Docs](https://docs.sink.cool/api/) · [Live Scalar Reference for the public demo instance](https://sink.cool/_docs/scalar)

## 🤖 AI Skills

Install Sink AI Skills for enhanced coding assistance:

```bash
npx skills add miantiao-me/sink
```

## 🧰 MCP

Sink serves a built-in MCP endpoint at `POST /api/mcp`, using the official `@modelcontextprotocol/server` SDK v2, serving modern clients over the per-request transport and 2025-era clients over a stateless fallback with JSON responses.

> Replace the domain below with your own instance, and use the `NUXT_SITE_TOKEN` from your instance's environment variables as the bearer token.

```sh
claude mcp add --transport http sink https://sink.cool/api/mcp --header "Authorization: Bearer SinkCool"
```

Any client that supports an HTTP transport with custom headers can connect the same way:

```json
{
  "mcpServers": {
    "sink": {
      "type": "http",
      "url": "https://sink.cool/api/mcp",
      "headers": {
        "Authorization": "Bearer SinkCool"
      }
    }
  }
}
```

It exposes tools for managing links (list, search, read, count, tag, create, update, upsert, delete) and for reading analytics (counters, views over time, and top values per dimension). See the [integrations documentation](https://docs.sink.cool/integrations/) for the full list.

## 🙋🏻 FAQs

[FAQs](https://docs.sink.cool/faqs)

## 💖 Credits

1. [**Cloudflare**](https://www.cloudflare.com/)
2. [**NuxtHub**](https://hub.nuxt.com/)
3. [**Astroship**](https://astroship.web3templates.com/)
4. [**Tailark**](https://tailark.com/)

## 📄 License

[AGPL-3.0-only](LICENSE) © [miantiao-me](https://github.com/miantiao-me)

## ☕ Sponsor

1. [Follow Me on X (Twitter)](https://404.li/x).
2. [Become a sponsor on GitHub](https://github.com/sponsors/miantiao-me).
