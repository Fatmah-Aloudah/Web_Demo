# Team Quiz Arena — Local / GitHub-ready

A static bilingual quiz game for up to 3 teams. The supplied category-screen reference is used as the visual direction for the scoreboard + category grid.

## What is included

- Landing page
- Team setup page: 1–3 teams, Arabic/English names, avatar selection, language
- Category page with all team scores in the header
- Random question from the chosen category
- Start Timer button with configurable per-question seconds
- Answer / reveal page
- Point-claim page with one button per team + “No one” button
- Winner podium: 1st taller than 2nd, 2nd taller than 3rd
- Replay: clears points and used-question history
- English and Arabic JSON databases
- Question/answer support for text plus media types: `image`, `audio`, `video`
- Optional GitHub Raw JSON endpoints in `js/config.js`
- No framework and no build step

## Run locally

Because browsers restrict loading JSON with `file://`, start a small local web server.

### Windows
Double-click `scripts/start-windows.bat`, or run:

```text
python -m http.server 8000
```

Then open `http://localhost:8000`.

### macOS / Linux
Run:

```bash
chmod +x scripts/start.sh
./scripts/start.sh
```

Then open `http://localhost:8000`.

## GitHub database setup

You can keep `db/en.json` and `db/ar.json` in a GitHub repository and have the game fetch them directly.

1. Upload the `db` folder to your GitHub repository.
2. In `js/config.js`, set:

```js
window.QUIZ_CONFIG = {
  githubDb: {
    en: "https://raw.githubusercontent.com/YOUR-USER/YOUR-REPO/main/db/en.json",
    ar: "https://raw.githubusercontent.com/YOUR-USER/YOUR-REPO/main/db/ar.json"
  },
  localDb: {
    en: "db/en.json",
    ar: "db/ar.json"
  },
  defaultTimerSeconds: 30
};
```

The app will try the GitHub URL first and fall back to the local JSON files if the GitHub URL is unavailable.

## Question schema

Each question looks like:

```json
{
  "id": "en-example-1",
  "type": "text",
  "question": "What is 2 + 2?",
  "answer": "4",
  "points": 100,
  "timer": 30
}
```

For media questions, use a media URL or local asset path:

```json
{
  "id": "en-photo-1",
  "type": "image",
  "question": "Identify this landmark.",
  "media": "assets/example.jpg",
  "answer": "Example landmark",
  "answerMedia": "assets/answer.jpg",
  "points": 200,
  "timer": 40
}
```

Supported `type` values: `text`, `image`, `audio`, `video`.

For media answers, use `answerMedia`. For a text answer plus media, keep `answer` and add `answerMedia`.

## Notes about media on GitHub

For large audio/video files, it is better to use a CDN or another static asset host. GitHub is fine for smaller quiz assets, but Git LFS or a dedicated media host may be preferable for a large question library.

## Changing questions

Edit the appropriate JSON file, commit/push the change, and refresh the game. The game does not require a database server.

## Ready-to-test demo data

The bundled English and Arabic databases now include a complete local demo library with:

- Text questions
- Image questions with local SVG images
- Audio questions with local WAV audio
- Video questions with local MP4 clips
- Answer media, including image, audio, and video examples
- Different point values and timer lengths

The landing page has a **Quick Demo** button. It loads three sample teams (`Lions`, `Falcons`, `Stars`) and takes you straight to the category screen so you can test the full game quickly.

### Suggested full test

1. Click **Quick Demo**.
2. Open **Picture Challenge**, start the timer, reveal the answer, then award the points to a team.
3. Open **Sound Challenge** and test the audio player.
4. Open **Video Challenge** and test the video player.
5. Use **No one** on one question to verify that no points are added.
6. Finish the game to see the three-level winner podium.
7. Click **Replay from zero** and confirm that all scores return to 0 while the teams remain.
8. Go back through the normal **Teams** setup and switch to Arabic to test the second database and RTL layout.

The demo assets are under `assets/demo/`, so the entire example works without needing an internet connection once the local server is running.
