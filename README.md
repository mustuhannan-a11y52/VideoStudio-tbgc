# Reel Studio (prototype)

Internal tool for TBGC's ops team to assemble the brief for a post-program highlight reel: name activities, tag their clips, pick a vibe, choose music, set the title-slide styling, and produce the JSON handed off to a render engine.

## Live on this deployment
- Program details (company, session, pax, title-slide color/font/alignment/format) — fully functional, all client-side
- Activities — add named activity blocks, upload real photos/video, live thumbnail previews
- Vibe — drives music filtering and (when re-enabled) caption tone
- Music — upload tracks, tag by vibe, select one per reel, real in-browser playback
- Reel brief — live running summary + a "Generate reel" button that outputs the exact JSON payload a render engine would need

## Disabled on this deployment
- **AI caption generator** — the working version calls the Anthropic API directly from the browser, which only works inside Claude's own artifact preview (it handles auth invisibly there). On a static GitHub Pages deploy there's no safe place to hold an API key client-side, so the feature is shown as "coming soon" and does nothing when clicked. Nothing is sent or logged anywhere in this state.

## To re-enable captions
Stand up a small backend (a single serverless function is enough) that:
1. Holds the real Anthropic API key server-side
2. Accepts `{theme, tone}` from the frontend
3. Calls the Anthropic API and returns the caption JSON
4. Update the frontend to call that endpoint instead of `api.anthropic.com` directly

## Not yet built at all
- **Actual video rendering.** "Generate reel" produces the brief JSON only. Turning that into an MP4 needs a connected render engine (e.g. Creatomate, Shotstack, or a custom ffmpeg pipeline) fed by a per-vibe template spec (cut timing, transitions, text position per moment) that doesn't exist yet — that's the single biggest remaining piece of work.
- BPM/beat detection for uploaded music
- Portrait-format-specific template adjustments beyond the aspect-ratio toggle
