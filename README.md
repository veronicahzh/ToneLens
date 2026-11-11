# ToneLens

**Figure out what a text message actually means before you overthink it.**

ToneLens uses Gemini to read the emotional tone of text messages. Type a draft to see how it might come across before you send it, or upload a screenshot of a conversation to get a tone breakdown for each message.

Built at **HackCamp 2025** (hosted by [nwPlus](https://nwplus.io)).

---

## Why we built it

Text loses a lot: tone of voice, facial expression, timing. Most of us guess at what's missing. Is this person annoyed? Are they being sarcastic? Did my message come across wrong? The guessing is especially hard for people with social anxiety, for neurodivergent people, and for anyone who overthinks messages. We wanted to see if AI could make that guess a better one.

## Features

- **Nuanced tone detection.** Instead of a simple positive/negative label, ToneLens picks out cues like *sarcastic*, *weary*, *casual*, *formal* or *passive-aggressive*. Each message gets 2–3 tone words with a confidence score.
- **Live draft preview.** Type a message and the tone analysis updates as you write, so you can see how it might land before you hit send.
- **Screenshot analysis.** Upload a screenshot of a chat. Gemini pulls out the message bubbles in order and skips timestamps, names and reactions. Then each message is analyzed separately.
- **Visual results.** Confidence scores appear as progress bars, so you can read the results at a glance.

## Tech stack

| Layer | Tools |
| --- | --- |
| Framework | [Next.js 15](https://nextjs.org) (App Router, Turbopack), React 18, TypeScript |
| AI | [Genkit](https://genkit.dev) + `@genkit-ai/google-genai` using **Gemini 2.5 Pro** |
| Schemas | [Zod](https://zod.dev), for structured, typed model output |
| UI | Tailwind CSS, [shadcn/ui](https://ui.shadcn.com) (Radix UI), lucide-react |

## Getting started

### Prerequisites

- **Node.js 20+** and npm
- A **Gemini API key** from [Google AI Studio](https://aistudio.google.com/app/apikey)

### 1. Clone and install

```bash
git clone https://github.com/veronicahzh/ToneLens.git
cd ToneLens
npm install
```

### 2. Add your API key

Create a file named `.env` in the project root with this line:

```bash
GEMINI_API_KEY=your-api-key-here
```

This file is in `.gitignore`, so your key won't be committed.

### 3. Run the app

```bash
npm run dev
```

Open **<http://localhost:9002>**. Each analysis takes a few seconds.

## How it works

1. **Text:** your message goes to Gemini, which returns 2–3 tone words, each with a confidence score from 0 to 1.
2. **Screenshot:** Gemini first reads the image and pulls out the text of each message bubble in order. Each message is then sent for tone analysis, all at once.
3. The app shows each confidence score as a progress bar.

The AI logic lives in [`src/ai/flows/`](src/ai/flows/), and the server code that calls it is in [`src/app/actions.ts`](src/app/actions.ts).

### The prompt engineering problem

The hardest part was getting Gemini to return output in the same parseable format every time. Language models like to be expressive, but the UI needed structured data it could map straight to progress bars. We got there with many rounds of prompt changes plus a strict output schema enforced through Genkit and Zod. Sending data to the API turned out to be the easy part; most of the work went into shaping what came back.

## Project structure

```text
src/
├── ai/
│   ├── genkit.ts      # Gemini setup
│   └── flows/         # AI prompts: tone detection, screenshot text extraction
├── app/
│   ├── page.tsx       # Main page
│   └── actions.ts     # Server code that calls the AI
├── components/        # UI components
├── hooks/
└── lib/               # Shared types and helpers
```

## Possible next steps

- Analyze each screenshot message in the context of the conversation around it, not on its own
- Add an overall sentiment summary for the whole conversation
- Label each message by sender (you vs. them)
- Save past analyses

## Team

Built by:

- **Veronica Zhang Harvey** ([@veronicahzh](https://github.com/veronicahzh))
- **Avi Guha** ([@avi-guha](https://github.com/avi-guha))

Made for **HackCamp 2025** by nwPlus.

## License

[MIT](LICENSE)
