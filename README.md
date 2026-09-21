The Public Health Soundtrack

An interactive, browser-based game where familiar songs double as doorways into real public health concepts. Pick a topic, press play on a track, read the two-paragraph case study behind it, then answer a question to check what stuck. Finish every section and earn a DJ title and a shot at the leaderboard.

Built as a single self-contained HTML file, so it runs anywhere with no build step, no server, and no dependencies beyond a browser.

Live demo: open public-health-playlist.html directly in any browser, or host it on GitHub Pages (see below).

What's inside
8 sections, 21 tracks: Epidemiology, Health Informatics, Social Determinants, Health Behavior, Occupational Health, Health Policy, Environmental Health, and Cybersecurity
Every case study cites a real, verifiable source (CDC, EPA, USDA, peer-reviewed journals, Supreme Court rulings, etc.) — linked at the bottom of each track's panel
Optional real audio: any track can be wired to an actual Spotify embed
A shared leaderboard (via Firebase) so a group can compare scores
DJ nicknames: finishing a section unlocks a title like "DJ Outbreak" or "DJ Zip Code"
A "Thoughts?" feedback box that appears once someone finishes, so players can send suggestions or bug reports
A live invite to The Public Health Soundtrack, a real collaborative Spotify playlist, right on the landing page
Quick start
Download public-health-playlist.html
Open it in any browser — that's it, the game works immediately with plain title cards for every track

No installation, no npm, no build tools. It's one file.

Adding real songs

Each track has a spotifyLink field, currently blank:

js
{
  title: "Bad Blood",
  artist: "concept: viral spread",
  spotifyLink: "",   // ← paste a Spotify track link here
  ...
}

To add a song:

Find the song in Spotify
Tap ⋯ → Share → Copy Song Link
Paste the full link (e.g. https://open.spotify.com/track/273dCMFseLcVsoSWx59IoE) between the quotes for spotifyLink

Once a link is added, that track shows a pulsing "Press play to hear it" cue and a real embedded Spotify player when opened. Tracks left blank still work fine — they just show the case study with no player.

Setting up the shared leaderboard (optional)

The leaderboard and feedback box both save to a small Firebase Realtime Database. Without it, the game still works — scores and feedback just won't be saved anywhere.

Go to console.firebase.google.com, create a free project
Build → Realtime Database → Create Database → start in test mode
Project settings (gear icon) → Your apps → register a web app → copy the config object
In the HTML file, find this block near the top of the <script> section and paste in your values:
js
const FIREBASE_CONFIG = {
  apiKey: "PASTE_YOUR_API_KEY",
  databaseURL: "https://PASTE-YOUR-PROJECT-default-rtdb.firebaseio.com",
  projectId: "PASTE_YOUR_PROJECT_ID"
};

Note: Firebase's test-mode rules expire after 30 days by default. Firebase will email a reminder before that happens, and you can extend or tighten the rules from the console.

Hosting it online (GitHub Pages)
Create a GitHub repository (public)
Upload the HTML file, renaming it to index.html
Settings → Pages → Source: Deploy from a branch, Branch: main, folder: / (root) → Save
Wait about a minute, then visit https://yourusername.github.io/your-repo-name/
Editing content

All game content lives in the SECTIONS array near the top of the <script> tag. Each section looks like:

js
{
  id: "epi",
  name: "Epidemiology",
  nickname: "DJ Outbreak",
  tracks: [ /* ... */ ]
}

Each track needs: title, artist (a short concept subtitle), spotifyLink, caseStudy (an array of 1–2 paragraph strings), question, choices (array of { text, correct }), feedbackCorrect, feedbackIncorrect, and source ({ label, url }).

To add a new track, copy an existing track object and edit the fields. To add a new section, copy an existing section object, give it a unique id, and add it to the SECTIONS array.

The real Spotify playlist

Separately from the game, there's an actual collaborative Spotify playlist called The Public Health Soundtrack, linked on the landing page. Anyone with the link can add a song that carries a public health message. The game and the playlist are related but independent - songs in the game don't have to come from the playlist, and vice versa.

Tech notes
Single HTML file - all CSS and JavaScript are inline, no build step
Uses localStorage to remember a player's name between visits (falls back gracefully if unavailable)
Firebase is loaded dynamically only if a config is provided, so the file works standalone without it
No frameworks, no dependencies beyond the optional Firebase SDK (loaded from Google's CDN)
