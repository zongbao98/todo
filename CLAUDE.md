# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A deliberately tiny to-do app: plain HTML, CSS and JavaScript, no frameworks, no build step, no tests, no package manager. Keep it that way; don't add dependencies or tooling.

## Running

Open `index.html` in a browser (double-click). There is nothing to build, lint or test. Verify changes by loading the page and exercising add, edit, delete, theme toggle.

## Architecture

- `index.html` is the whole app: inline `<style>` and `<script>`.
- State is one array `todos` of `{text, done, cat, due}` (`cat` is Work/Personal/Others, `due` is `YYYY-MM-DD` or `''`). Every mutation goes through `update()`, which calls `render()` and then saves to `localStorage` key `todos`.
- `render()` rebuilds the `<ul>` from scratch with DOM APIs and `textContent` (never `innerHTML`), so typed text can't inject HTML. Keep it that way.
- Theme: `applyTheme()` sets `document.documentElement.style.colorScheme`, which flips background, text and form controls without a custom palette. Saved under `localStorage` key `theme`; default follows `prefers-color-scheme`.
- All `localStorage` access is wrapped in try/catch because some previews and private modes block it. Items still work there but don't persist. Preserve this: `render()` must run before the save.
- Overdue styling compares `t.due` against `new Date().toLocaleDateString('sv')` (local `YYYY-MM-DD`) as strings.

## mockup.html

A static phone mock-up (iPhone 15 and Galaxy S25 frames) that embeds a copy of `index.html` in two iframes via escaped `srcdoc`, with three seeded items. It is a snapshot: it does not track changes to `index.html`, so regenerate it after UI changes if you want it current.
