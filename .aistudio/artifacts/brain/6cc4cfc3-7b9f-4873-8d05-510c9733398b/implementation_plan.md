# Nebula Craft: Bedrock Dedicated Server (BDS) Console — Implementation Plan

A comprehensive, diegetic Minecraft Bedrock Edition web management application reproducing the full visual fidelity and mechanics of the mockup screens with a unified view switcher, interactive BDS terminal simulation, realistic server controls, and authentic Minecraft HUD styling.

---

## 1. Architecture & Design Direction

### Visual & Thematic Identity
- **Diegetic Skeuomorphism**: Authentic Minecraft GUI 3D borders (`mc-bevel`, `mc-inset`, `mc-stone-btn`, `mc-gui-window`), zero border-radius, hard-edged pixel bevels, CRT scanlines, and pixelated font hierarchy (`Press Start 2P`, `Silkscreen`, `Space Mono`, `VT323`, `JetBrains Mono`).
- **Color Codes**:
  - Emerald XP Green (`#7CBB43`, `#97D85D`): Online status, healthy TPS, XP level badges, active conduits.
  - Golden Redstone Lamp (`#DFEC40`, `#FFE55C`): Motd headers, level badges, warnings, active tabs.
  - Redstone Torch (`#FF5555`, `#93000A`): Critical alerts, stop/kill switches, checksum fails.
  - Obsidian & Slate (`#131314`, `#1B1C1C`, `#0E0E0E`): Deep recessed panels and terminal screens.
- **Embedded Assets**: Use the real uploaded assets:
  - Nebula Beacon Crest Logo
  - AlexMiner custom skin head avatar
  - Dusk panoramic mountain landscape hero image

---

## 2. Multi-View Screen Switcher (Unified Console)

The application provides a top-level view selector allowing seamless navigation across all 5 showcased screens, with global server state preserved:

1. **View 1: Classic Server Dashboard (`server-dashboard`)**
   - Server status HUD (Public host, copy IP button, 20.0 TPS, RAM 3.8/8.0 GB, CPU load 18%).
   - BDS Engine Version dropdown & 6-step ingestion pipeline with simulated checksum crash toggle.
   - Bedrock Properties crafting table form (MOTD, active level world, gamemode buttons, difficulty, max players stepper, collapsible advanced world settings with redstone lever for cheats).
   - Operator registry (Xbox 16-digit XUIDs validation, add/remove operator).
   - Live stdout log console with quick macro chips (`/list`, `/time set day`, `/weather clear`, `/save-all`, `/tps`) and interactive command input.

2. **View 2: Operator Access / Login Gateway (`operator-auth`)**
   - Panoramic landscape hero with glowing beacon and floating online badge.
   - Angled Minecraft splash text ("Now with Bedrock 1.21.30 protocol support!").
   - Direct connection card with BDS signal ping bars (24ms, 60 TPS).
   - Recessed panel token input with clipboard paste button and remember toggle.
   - Authentication flow with realistic BDS handshake and validation feedback.
   - World and allocated BDS RAM gauge.

3. **View 3: Master Container 54-Slot Chest Matrix (`chest-matrix`)**
   - Full 6x9 (54 slots) diegetic double chest inventory grid.
   - Diegetic server items: TPS Clock (20.0), Enchanted BDS Engine Core (v1.21), Vessel of Souls (12/20), Glowstone RAM Shard (3.8G), Blaze Rod CPU (18%), Active World Compass, Dimension Portals (Nether, End), Totem of Hot Reboot, Whitelist Ledger, Cheats Redstone Torch, Difficulty Skull, Render Distance Eye, Tick Distance Piston, Packs Emerald/Diamond, and World Backup Bundle.
   - Interactive hover tooltips with custom Minecraft lore and rarity styling.
   - Anvil Repair & Forge station for renaming server MOTD and combining JARs.
   - Book & Quill operator roster with AlexMiner player doll.
   - Toggleable multiplayer chat & log drawer (pressable with 'T' shortcut).
   - Persistent bottom 9-slot Hotbar with XP gauge (Level 30) and quick action keys 1-9.

4. **View 4: Command Block Architecture & Visual Node Pipeline (`node-pipeline`)**
   - Top command bus with redstone bus indicators.
   - Holographic beacon scanner matrix with scanline animation and ASCII bar graphs.
   - Visual 3-node topology diagram (BDS Engine Core -> Craft Recipe Matrix -> World Container) connected by pulsing redstone wire SVG.
   - 6-stage XP ingestion capacitor progress tracker.
   - Live death logs & combat feed (Creeper explosions, player joins, advancements).
   - Compact command block terminal with quick execute buttons.

5. **View 5: F3 Telemetry Tower & CRT Mission Control (`f3-telemetry`)**
   - Full-scale F3 debug screen overlay with live fluctuating hardware metrics (AMD EPYC CPU threads, Snappy compression, LevelDB I/O).
   - 24-tick frame-time millisecond bar chart tracking tick spikes and GC sweeps.
   - Beacon Emitter Harness with 4-tier netherite pyramid status and Haste II frequency.
   - Hex-encoded deployment sequencer (0x01 SIGTERM through 0x06 LAUNCH).
   - XUID authorization matrix with badges, Demote, and Ban Hammer actions.
   - Full-height CRT scanline terminal with green phosphor glow, baud speed toggle, and broadcast macro pad.

---

## 3. Interactive State & Simulation Engine

A shared reactive state store ensures changes in one screen synchronize across all views:
- **Server Lifecycle**: `RUNNING` (Stable), `RESTARTING...`, `STOPPED`, `DEPLOYING`.
- **Live Terminal & Command Processor**:
  - Supports standard Bedrock server commands: `/list`, `/say`, `/time set day/night`, `/weather clear/rain/thunder`, `/gamemode`, `/op`, `/deop`, `/kick`, `/ban`, `/save-all`, `/tps`, `/stop`, `/reload`, and custom broadcast messages.
  - Realistic console output with timestamps, color-coded prefixes (`[BDS]`, `[INFO]`, `[WARN]`, `[EXEC]`), and simulated responses.
- **Config Synchronization**:
  - Live editing of MOTD name, active world, max players, gamemode, cheats lever, and difficulty.
  - Dynamic operator management with XUID validation (16 numeric digits verification).
- **Simulated Checksum Crash & Recovery**:
  - Toggle checksum error to trigger "YOU DIED / Checksum Mismatch" state and test server recovery flows.
- **Audio & Haptic Feedback**:
  - Optional subtle 8-bit web audio synth sound effects for button clicks, anvil clinks, and terminal keystrokes (with mute toggle in header).

---

## 4. Implementation Steps

1. **Assets & Fonts Setup**:
   - Ensure Google Fonts (`Press Start 2P`, `Silkscreen`, `Space Mono`, `VT323`, `JetBrains Mono`) and Material Symbols are linked in `index.html`.
   - Update `metadata.json` with appropriate name and description.
2. **State & Types Foundation**:
   - Create `src/types/server.ts` defining server status, telemetry, config, operators, log entries, and inventory items.
   - Create `src/context/ServerContext.tsx` managing reactive server state, terminal command history, logs, and sound triggers.
3. **Sound FX Engine**:
   - Create `src/utils/audio.ts` using Web Audio API for lightweight diegetic 8-bit click, anvil, and alert sounds without external audio dependencies.
4. **Core UI Views**:
   - `src/components/Header.tsx`: Shared top navigation bar with realm status, ping, quick reboot/stop actions, and view switcher tabs.
   - `src/views/DashboardView.tsx`: Classic Bedrock Server Console (Mockup 3 & 4).
   - `src/views/OperatorAuthView.tsx`: Login & Operator Access modal with panorama hero (Mockup 5 & 6).
   - `src/views/ChestMatrixView.tsx`: 54-Slot diegetic chest, anvil forge, book roster, and hotbar (Mockup 7 & 8).
   - `src/views/NodePipelineView.tsx`: Command block visual node topology, redstone SVG wires, and combat log (Mockup 9 & 10).
   - `src/views/F3TelemetryView.tsx`: F3 debug screen, 20 TPS timeline, hex sequencer, and CRT console (Mockup 11 & 12).
5. **Shared Components**:
   - `src/components/TerminalLogs.tsx`: Reusable Bedrock terminal with auto-scroll, macros, and command input.
   - `src/components/HotbarDock.tsx`: Persistent or toggleable 9-slot hotbar with XP bar.
6. **Verification & Testing**:
   - Run compilation and ensure all responsive breakpoints, tooltips, clipboard interactions, and command simulations work smoothly.
