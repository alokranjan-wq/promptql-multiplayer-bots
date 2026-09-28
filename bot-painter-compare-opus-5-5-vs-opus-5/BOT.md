# Bot Painter: Opus 5 vs Opus 5.5

A side-by-side test of how two models paint. Two bots, one running Opus 5 and one running Opus 5.5, each paint a portrait of the person in the given X profile. You then compare their work and merge their painting videos into one.

If no X profile link is given, or the link is not a real profile URL, ask for one before doing anything else.

## Steps

### 1. Start the two painter bots

- Spin up two bots, one running Opus 5 and the other Opus 5.5.
- Send each bot the same painter prompt below, with the X profile link filled in.

Painter prompt:

```
I want to test you for aesthetics and visual perception.

Paint a portrait of the person in this X profile, live on your VM desktop:
<X profile link>

Read the full instructions in this file and follow them exactly:
https://raw.githubusercontent.com/hasura/promptql-multiplayer-bots/main/bot-painter/BOT.md
If you cannot open the file, tell me instead of guessing.
```

### 2. Check on both bots after 25 minutes

- After 25 minutes, check on both bots.

### 3. Compare the two bots

- How each bot tackled the task.
- How their approaches differed.
- Whether Opus 5.5 was better at painting, and why.

### 4. Merge the painting videos

Merge the two bots' painting videos into one portrait video at mobile resolution (1080x1920):

- Stack them one above the other. Fill the space above and below with black.
- If one video is shorter, freeze its last frame until the longer one ends. Show "Done!" on the frozen frame.
- Title each video section at its top left, in white text on a black background: "Bot Painter : Opus 5.5" and "Bot Painter : Opus 5".

### 5. Hand it over

- Post the comparison and the merged MP4 in this chat.
