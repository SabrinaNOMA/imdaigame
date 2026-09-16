# 🎨 IMD AI Challenge: Real vs AI

[![Netlify Status](https://api.netlify.com/api/v1/badges/5cb81358-132d-4bfd-a342-63b784a0d9b4/deploy-status)](https://imdaigame.netlify.app)
[![License: CC0 / Public Domain](https://img.shields.io/badge/License-CC0%20%2F%20Public%20Domain-blue.svg)](images/README.md)
[![Live App](https://img.shields.io/badge/Live%20Demo-imdaigame.netlify.app-003366?style=flat&logo=netlify)](https://imdaigame.netlify.app)

An interactive, responsive AI literacy web application designed for executive workshops, conferences, and group events (supporting 250+ simultaneous participants). Players test their critical perception against 20 high-stakes visual challenges, learning to identify the subtle tells between human-created masterpieces and generative AI synthetic imagery.

---

## 🚀 Live Demo

👉 **Play the game**: **[https://imdaigame.netlify.app](https://imdaigame.netlify.app)**

---

## ✨ Features

- **20 Curated Visual Challenges**:
  - **10 Human Masterpieces**: Authentic museum artworks with real physical brushwork, impasto texture, and classical perspective (from *The Metropolitan Museum of Art*).
  - **10 AI-Generated Artworks**: Created using state-of-the-art diffusion models (*Google Imagen*, *Midjourney v3/v4*) exhibiting classic generative anomalies (anatomical glitches, contradictory shadows, impossible perspective, non-Euclidean geometry).
- **Executive Scoring Engine**:
  - **Base Score**: +100 points per correct answer (up to 2,000 base points).
  - **Speed Bonus**: Additional points for high-velocity discernment (completed under 30 seconds).
  - **Performance Tiers**: Dynamically categorizes players as **Master Critic 🎓** ($\ge 90\%$), **Sharp Eye 👁️** ($\ge 70\%$), or **Apprentice 🎨**.
- **Contextual In-Game Hints**:
  - Optional `💡 Need a hint?` button revealing architectural and anatomical details to inspect without giving away the answer.
- **Answer Review & Sourcing**:
  - Post-challenge breakdown showing players exactly which artworks they guessed correctly (🟩) or missed (🟥), with direct links to the museum collection or generator notes.
- **Dual Leaderboard Architecture**:
  - **Local Persistence**: Browser `localStorage` maintains recent scores on the device with a one-click reset button.
  - **Cloud Sync**: Automatically streams scores, accuracy, and timestamps to a centralized **Google Sheet** in real time.
- **Zero-Build, High-Performance**:
  - Single-bundle vanilla HTML5, Tailwind CSS, and lightweight JS. Instant page loads on mobile phones and laptops alike.

---

## 📁 Repository Structure

```text
imdaigame/
├── index.html              # Main application entrypoint served by Netlify
├── front.html              # Workspace backup/sync template
├── images/                 # Self-hosted high-resolution image assets
│   ├── 01_real.jpg         # Wheat Field with Cypresses (Vincent van Gogh)
│   ├── 02_ai.jpg           # Futuristic Organic Sculpture (Imagen AI)
│   ├── ...                 # (10 Real and 10 AI images)
│   ├── 20_ai.png           # Abstract Organic Architecture (Midjourney)
│   ├── sources.json        # Structured programmatic dataset with hints and sources
│   └── README.md           # Dataset credits, museum accession numbers, and licenses
├── .vscode/
│   └── settings.json       # Scoped Git workspace configuration
└── README.md               # Project documentation (this file)
```

---

## 📊 Centralized Google Sheets Leaderboard

The app connects asynchronously to a private Google Sheet via a Google Apps Script Web App.

### Data Logged per Participant:
1. **Timestamp** (ISO 8601)
2. **Player Name**
3. **Total Score** (Base + Speed Bonus)
4. **Correct Artworks** (e.g., `18`)
5. **Total Artworks** (`20`)
6. **Elapsed Time** (Seconds to 1 decimal place)

### Apps Script Code (`Code.gs`):

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);

    sheet.appendRow([
      data.timestamp || new Date(),
      data.nickname,
      data.score,
      data.correct,
      data.total,
      data.time
    ]);

    return ContentService
      .createTextOutput(JSON.stringify({ status: "success" }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService
      .createTextOutput(JSON.stringify({ status: "error", message: error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

---

## 🛠️ How to Host an Event (Up to 250 Participants)

1. **Display on Stage**: Open your linked **Google Sheet** on the event projector or presentation screen.
2. **Share the Link / QR Code**: Direct participants to **`https://imdaigame.netlify.app`**.
3. **Run the Sprint**: Give participants 2 minutes to complete the 20 questions.
4. **Announce Winners**: Sort Column C (*Total Score*) in your Google Sheet descending to reveal the top 3 finishers live!
5. **Reset for Next Cohort**: Delete rows 2+ in your Google Sheet to wipe the board clean for another round.

---

## 🖼️ Image Dataset & Credits Summary

For detailed source links and accession numbers, see [images/README.md](images/README.md).

| Range | Origin | Creator / Model | License |
|---|---|---|---|
| Odd numbers (`01`, `03`, ..., `19`) | Human Masterpieces | Van Gogh, Seurat, Vermeer, Monet, David, Hokusai, Poussin | Public Domain / CC0 (The Met) |
| Even numbers (`02`, `04`, ..., `20`) | Generative AI | Google Imagen / Gemini AI, Midjourney v3/v4 | CC0 / CC-BY-SA 4.0 |

---

## 📜 License

Code is licensed under the [MIT License](LICENSE). Artworks and media are subject to public domain and respective Creative Commons licenses as detailed in [images/README.md](images/README.md).
