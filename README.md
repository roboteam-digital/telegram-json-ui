# telegram-json-ui

Web UI with search and navigation for Telegram chat / channel dump (export) from JSON

## Demo

- https://telegram-json-ui.netlify.app


## Stack

- [SvelteKit](https://kit.svelte.dev/)


## Svelte components

- `<VirtualList>` for fast rendering large message dumps


### Run for local dev or deploy for production

- [PM2](https://pm2.keymetrics.io/docs/usage/quick-start/)
- [Netlify](https://docs.netlify.com/configure-builds/common-configurations/sveltekit/)


## Requriments

1. PM2 to run SvelteKit app in background:

`pnpm install pm2@latest -g` (or use npm)


## Quick start

1. `git clone <this repo>`

2. `cd telegram-json-ui/frontend/sveltekit`

3. `pnpm i` (or use `npm`)

4. `./start`

If you see port error, change it in `start` script


## Telegram messages JSON-dump source

- While this is a proof of concept, we put exported directory with dump in SvelteKit ./static dir
- TODO: JSON-dump upload feature

## How to export Telegram chat

- [Telegram desktop clients for your OS](https://desktop.telegram.org/)
- [About Telegram Chat Export Tool](https://telegram.org/blog/export-and-more)


## Original idea

- https://t.me/ruarxivechat/838


## More about digital archiving

- https://github.com/ruarxive


