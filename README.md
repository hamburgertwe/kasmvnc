
# GeForce NOW in GitHub Codespaces

## Start the Codespace

1. Push this repository to GitHub, then open its GitHub page.
2. Select **Code** > **Codespaces** > **Create codespace on main** (or choose
   the branch you want to use).
3. Wait for the Codespace to finish creating the dev container. It starts
   Chromium and should open the forwarded browser on port 3000 automatically.
4. If the browser does not open, select the **Ports** tab in the Codespace,
   find port 3000 (**GeForce NOW Stream**), and open it in your browser.

Sign in to GeForce NOW and start a game. To start the Codespace again later,
open the repository's **Code** > **Codespaces** menu and select your existing
Codespace. It resumes and starts Chromium automatically.

The browser profile is saved in `.chrome-data` so your sign-in survives Codespace
restarts. The stream is tuned for gaming: fixed 1280x720, 60 FPS, full-frame
video mode, CBR with congestion control, and a two-frame backpressure queue.
This favors fresh input over preserving every frame, so congestion may cause
dropped frames. Reduce the stream frame rate to 30 FPS if it still stutters.

Codespaces does not provide a GPU, and its forwarded browser is not optimized
for low-latency game streaming. GeForce NOW may perform better or may not work
as expected compared with a supported local device and browser.

## Deploy on Render

1. Push this repository to GitHub and create a **Blueprint** on Render using
   this repository. Render reads `render.yaml` and builds the root `Dockerfile`.
2. Set the required `PASSWORD` when Render prompts for it. The browser UI uses
   this password for HTTP basic authentication; use a unique, strong password.
3. Open the deployed service's `onrender.com` URL.

Render runs Chromium directly rather than running the Codespaces Docker-in-Docker
startup command. The Blueprint selects Render's free plan (0.1 CPU, 512 MB RAM).
The Render image defaults to 1280x720 at 30 FPS and 4 Mbps to reduce
software-encoding load, but Chromium may still run slowly or run out of memory
with the free plan. For more headroom, change the plan to `1c-2g` (1 CPU, 2 GB
RAM) in `render.yaml`; Render has no GPU on either plan. The filesystem is
ephemeral, so browser profile changes do not survive redeploys or instance
restarts.

The browser UI includes a terminal with passwordless `sudo`. LinuxServer
recommends a robust authentication proxy for internet-facing deployments;
HTTP basic authentication alone is not suitable for sensitive use. Do not use
this public service for sensitive accounts or data.



