# Speaksheet on Render

The debate score tracker, with **Prep** and the theme **Suggest** button working through a small Node server.

**Who pays for Claude:** each person pastes their own Anthropic API key on the Prep tab, and their own account is billed. You pay nothing. Hosting on Render's free plan is free too.

```
public/index.html   the page (also still works on claude.ai)
server.js           serves the page and passes Prep requests to Claude
render.yaml         tells Render how to run it
```

Design and motion by **Disas Herath**.

## About the design

The page uses the "Chamber" design system, all inside `public/index.html`:

- **Type:** Archivo (condensed, for headings and structure), Instrument Serif italic (for voice) and JetBrains Mono (for every score). All three load from Google Fonts.
- **Colour:** one signal colour, vermilion. Light and dark themes follow the device setting.
- **Motion:** each tab opens with a kinetic title and the debate sequence *Claim → Argument → Clash → Rebuttal → Analysis → Speech → Decision*. Leaderboard averages count into place, the score chart draws itself, and blocks reveal as you scroll. Motion turns off for anyone who has *reduce motion* set on their device.
- **Extras:**
  - Click a speaker on the leaderboard for a full-screen **spotlight** card.
  - **Stage mode** on the Timer fills the screen and turns green when POIs are open and red in overtime.
  - Hover the score chart to compare every speaker in that round.
  - Tabs slide like pages, sums tick as you type scores, and leaderboard rows slide when ranks change.
  - The **light/dark switch** in the header remembers your choice.
  - On a computer with a mouse there's a custom cursor and a grid that glows around it.
  - A short loading screen plays once per visit.
- **Code:** the app's own script is unchanged. The motion is a separate script at the end of the page that only adds animation, so the scores, prep, timer and converter work exactly as before.

## 1. Put this folder on GitHub

1. Create a new repository on github.com, for example `speaksheet`.
2. Upload everything in this folder: `server.js`, `package.json`, `render.yaml`, `.gitignore`, `README.md` and the `public` folder. You can drag and drop them on GitHub's "uploading an existing file" page.

## 2. Deploy on Render

1. On https://dashboard.render.com choose **New → Blueprint** and pick your repository. Render reads `render.yaml`.
2. When Render asks for `ANTHROPIC_API_KEY` and `ACCESS_CODE`, **leave both blank**.
3. Click **Apply**. After a minute or two your site is live at `https://speaksheet-xxxx.onrender.com`.

To check that it's running, open `/api/health` on your site. It should show `"byok":true`.

## 3. What each team member does (once)

1. Go to https://console.anthropic.com, sign up and add credit under **Billing**. A Claude.ai Pro subscription does not cover this. The API is billed separately.
2. Set a monthly spend limit under **Limits**, for example $5.
3. Under **API keys**, create a key and copy it. It starts with `sk-ant-`.
4. On the site, open **Prep** and paste the key into **Your Anthropic API key**. The browser remembers it. **Forget key** removes it.

One prep brief usually costs a few cents.

## How the key is handled

- The key is kept in that person's browser only.
- It goes to your Render server with each Prep request, is used for that one call to Claude, and is then thrown away. It is never saved or logged.
- Anyone who can use that browser could see the key, so people shouldn't paste it on shared computers.

## Settings you can change (Render → your service → Environment)

| Variable | What it does | Default |
|---|---|---|
| `ANTHROPIC_API_KEY` | Leave blank so everyone uses their own key. If you set it, **you** pay for everyone instead. | blank |
| `ACCESS_CODE` | Only used if you set `ANTHROPIC_API_KEY`. It's a code people need to use your key. | blank |
| `HOURLY_LIMIT` | Claude requests allowed per visitor per hour. | 30 |
| `CLAUDE_MODEL` | Which Claude model to use. `claude-sonnet-5-5` is about half the price. | `claude-opus-5-5` |

## Good to know

- **Scores are saved in each person's browser** on the Render site. Use **Copy folder code** to move a folder between devices or people.
- **The free Render plan sleeps** after 15 minutes without visitors, so the first visit afterwards can take about 30 seconds to load.
- **To update the page**, replace `public/index.html` on GitHub. Render redeploys on its own.

## Run it on your own computer (optional)

Install Node.js 20 or newer from https://nodejs.org, then in this folder:

```powershell
npm install
npm start
```

Open http://localhost:3000.
