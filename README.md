# Know Your Numbers — Vital Signs Dashboard

A live, browser-based version of the Success Metrics Vital Signs Tracker. Instead of filling out a spreadsheet, you fill in the same numbers on a web page and watch your dashboard update as you type: days cash on hand, quick ratio, gross margin, break-even revenue, customer acquisition cost, and retention, each with a plain-English read on where you stand.

## What's in here

One file, `index.html`. No build step, no dependencies to install, nothing to compile. Open it in a browser and it works.

## Running it locally

Double-click `index.html`, or open it from your browser's File menu. That's it.

## What it does

- Six vital signs, calculated live from the numbers you enter, matching the logic in the original Vital Signs Tracker spreadsheet.
- A dashboard that flags anything in caution or critical range and tells you what to look at first.
- A backwards pricing calculator, a break-even scenario planner, and a fixed vs. variable cost categorizer.
- A monthly history log so you can save a snapshot each month and watch your numbers move over time.
- Everything is saved in the browser's local storage on your device only. Nothing is sent to a server, because there isn't one yet — this is the standalone version.

## Deploying it

This repo is set up to deploy as-is on [Vercel](https://vercel.com): connect this GitHub repo to a Vercel project, and it publishes a live URL automatically. Any future push to this repo redeploys it.

The intended use is to embed that live URL in an iframe on a members-only page inside the Know Your Numbers course (hosted on Kajabi), so it's available to anyone who has purchased the course. Kajabi handles who's allowed to see the page; this app doesn't need to know anything about billing or accounts.

## What this isn't, yet

This version doesn't save anyone's data anywhere Jeremy can see it. It's a standalone calculator each client runs in their own browser. A future version with client accounts and a real coach-facing dashboard is planned as a separate phase.

## About Success Metrics

Financial coaching and education for nurses and healthcare entrepreneurs building something of their own. [mysuccessmetrics.com](https://mysuccessmetrics.com)
