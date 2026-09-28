# Live Portrait Painting Test

A test of aesthetics and visual perception. Paint a portrait of the person in the given X profile, live on your VM desktop, using only Python and a browser.

If no X profile link is given, or the link is not a real profile URL, ask for one before doing anything else.

## Rules

- Do not use image-generation or style-transfer models.
- Use only Python and a browser.
- Get the profile picture from the profile link, through your VM browser.
- Show the live VM desktop to the user before any other work.
- Paint live on your VM desktop, record the process, and deliver the final video.

## Steps

### 1. Start the desktop and show it first

Do this before anything else, including opening the profile or writing code. Do not skip it or do it later.

1. If you do not have a VM yet, provision one. Only v2 VMs have a desktop. If your VM is not v2, tell the user and stop.
2. Start the desktop service on the VM, and confirm it is running.
3. Create a live desktop app artifact. Starting the service alone does not make the desktop visible to the user. The artifact does.
4. Show the desktop artifact to the user in a chat message, and tell them they can watch the whole process live there. Then continue without waiting for a reply.
5. Only then go to step 2. Do all later work (photo download, painting, recording) on this desktop, so the user sees everything from the photo download to the last stroke.

Keep the screen clean the whole time: no mouse cursor, no popups.

### 2. Get the photo

- Open the profile in the VM browser, on the shared desktop, and download the largest version of the profile picture. Do not use a third-party avatar service.
- If X shows a login wall, the link is not a valid profile, or the picture is the default one, ask for a photo upload instead.

### 3. Study the photo once and write a short brief

- What makes the face recognizable.
- The palette, and where the light comes from.
- Where detail matters (eyes, mouth, hairline) and where it can stay loose.
- One painting style that suits the photo (Van Gogh, Monet, Rembrandt, Seurat...) and why.
- Painter settings for that look.

### 4. Build a painter in Python that works like a real painter

- Paint coarse to fine, in layers, with 4 to 5 brush sizes. X pictures are small, so upscale the photo first.
- In each layer, repaint only where the canvas still differs from the photo, blurred to that layer's brush size.
- Use curved strokes that follow edges and do not cross them.
- Take stroke colors from the photo, with a small jitter.
- Place strokes in random order, with jitter in size and angle, and visible bristle texture.
- Expose a few style knobs: stroke length, brush sizes, color variation, texture.
- Finish with a fine detail pass, only where the brief said detail matters.
- Compare canvas and photo numerically, in memory. Never use screenshots for this.
- Plan all strokes in one code run. Do not stop between layers to check by eye.

### 5. Paint live and record

- On screen: a large, centered canvas, the reference photo beside it, and a small terminal that narrates each layer.
- One screenshot is allowed, only to confirm the screen is clean.
- Say in chat that painting is about to start, then draw every stroke on the canvas as it happens. The painting should take 1 to 2 minutes.
- Record the screen. Keep the video under 2 minutes, with no dead time and no taskbar.

### 6. Hand it over

- Post the final PNG, the MP4, and the brief in the chat.
