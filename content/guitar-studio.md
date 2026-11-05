+++
title = "Acoustic Guitar & Vocal Studio"
description = "Browser-based client-side guitar & vocal denoiser, snippet cutter, and 5-track audio mixer with WhatsApp-friendly MP3 export"
date = "2026-11-05"
aliases = ["guitar-cleaner", "guitar-studio", "vocal-cleaner", "mixer", "pedal"]
author = "Dhruvik Donga"
+++

<style>
.guitar-app {
  font-family: inherit;
  max-width: 960px;
  width: 95%;
  margin: 1.5rem auto;
  padding: 1.8rem;
  border: 1px solid var(--border-color, #2a2d36);
  border-radius: 12px;
  background-color: var(--bg-color, #16171b);
  color: var(--text-color, #e2e8f0);
  box-shadow: 0 12px 36px rgba(0, 0, 0, 0.45);
  transition: all 0.2s ease;
}
.guitar-app.maximized {
  position: fixed !important;
  top: 0 !important;
  left: 0 !important;
  width: 100vw !important;
  max-width: 100vw !important;
  height: 100vh !important;
  margin: 0 !important;
  border-radius: 0 !important;
  z-index: 999999 !important;
  overflow-y: auto !important;
  box-sizing: border-box !important;
  padding: 1.5rem 2.5rem !important;
  border: none !important;
}
.guitar-app.maximized .guitar-canvas {
  height: 130px !important;
}
.guitar-app.maximized .mixer-canvas {
  height: 95px !important;
}
.studio-tabs {
  display: flex;
  gap: 0.5rem;
  border-bottom: 1px solid #2a2d36;
  margin-bottom: 1.3rem;
  padding-bottom: 0.2rem;
}
.studio-tab-btn {
  background: transparent;
  border: none;
  color: #94a3b8;
  padding: 0.6rem 1rem;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all 0.2s ease;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
}
.studio-tab-btn:hover {
  color: #f1f5f9;
}
.studio-tab-btn.active {
  color: #10b981;
  border-bottom-color: #10b981;
}
.guitar-dropzone {
  border: 2px dashed #3a3f4d;
  border-radius: 8px;
  padding: 2.4rem 1.5rem;
  text-align: center;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.02);
  transition: border-color 0.2s, background-color 0.2s, transform 0.15s ease;
}
.guitar-dropzone:hover, .guitar-dropzone.dragover {
  border-color: #10b981;
  background-color: rgba(16, 185, 129, 0.05);
  transform: translateY(-1px);
}
.guitar-dropzone input {
  display: none;
}
.guitar-btn {
  background: linear-gradient(135deg, #10b981, #059669);
  color: #fff;
  border: none;
  border-radius: 6px;
  padding: 0.6rem 1.2rem;
  font-size: 0.92rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  box-shadow: 0 3px 12px rgba(16, 185, 129, 0.25);
  text-decoration: none;
}
.guitar-btn:hover {
  opacity: 0.95;
  box-shadow: 0 5px 16px rgba(16, 185, 129, 0.38);
  transform: translateY(-1px);
}
.guitar-btn:disabled {
  background: #333842;
  cursor: not-allowed;
  opacity: 0.55;
  box-shadow: none;
  transform: none;
}
.guitar-btn-secondary {
  background: rgba(255, 255, 255, 0.05);
  color: #cbd5e1;
  border: 1px solid #333844;
  border-radius: 6px;
  padding: 0.6rem 1.1rem;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  text-decoration: none;
}
.guitar-btn-secondary:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #ffffff;
  border-color: #4b5563;
}
.guitar-btn-download-mp3 {
  background: linear-gradient(135deg, #10b981, #059669);
  color: #ffffff;
  border: none;
  border-radius: 6px;
  padding: 0.65rem 1.2rem;
  font-size: 0.92rem;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  cursor: pointer;
  box-shadow: 0 4px 14px rgba(16, 185, 129, 0.35);
  transition: all 0.2s ease;
}
.guitar-btn-download-mp3:hover {
  opacity: 0.95;
  transform: translateY(-1px);
}
.guitar-btn-download-wav {
  background: rgba(16, 185, 129, 0.12);
  color: #34d399;
  border: 1px solid rgba(52, 211, 153, 0.4);
  border-radius: 6px;
  padding: 0.65rem 1.1rem;
  font-size: 0.92rem;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  cursor: pointer;
  transition: all 0.2s ease;
}
.guitar-btn-download-wav:hover {
  background: #10b981;
  color: #fff;
  border-color: #10b981;
}
.guitar-section {
  margin-top: 1.4rem;
  padding-top: 1.2rem;
  border-top: 1px solid var(--border-color, #262933);
}
.guitar-input-field {
  width: 100%;
  padding: 0.65rem 0.8rem;
  background: #0f1013;
  border: 1px solid #333844;
  border-radius: 6px;
  color: #f1f5f9;
  font-family: inherit;
  font-size: 0.92rem;
  box-sizing: border-box;
  margin-top: 0.3rem;
  transition: border-color 0.2s, box-shadow 0.2s;
}
.guitar-input-field:focus {
  border-color: #10b981;
  outline: none;
  box-shadow: 0 0 0 2px rgba(16, 185, 129, 0.25);
}
.guitar-status {
  margin-top: 0.8rem;
  font-size: 0.92rem;
  color: #34d399;
  font-weight: 500;
}
.guitar-player {
  width: 100%;
  margin-top: 0.8rem;
  accent-color: #10b981;
  border-radius: 8px;
}
.guitar-mode-toggle {
  display: inline-flex;
  background: #0f1013;
  border: 1px solid #2e323d;
  border-radius: 6px;
  padding: 3px;
}
.guitar-toggle-btn {
  background: transparent;
  border: none;
  color: #94a3b8;
  padding: 0.35rem 0.85rem;
  font-size: 0.85rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s ease;
}
.guitar-toggle-btn.active {
  background: #10b981;
  color: #fff;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.4);
}
.guitar-toggle-btn.active.orig {
  background: #f59e0b;
  color: #fff;
}
.guitar-time-badge {
  font-family: monospace;
  font-size: 0.85rem;
  font-weight: 700;
  color: #34d399;
  background: #0f1013;
  padding: 0.3rem 0.65rem;
  border-radius: 4px;
  border: 1px solid #282c37;
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  white-space: nowrap;
}
.guitar-viz-container {
  margin-top: 1rem;
}
.guitar-viz-card {
  background: #0d0e11;
  border: 1px solid #23262f;
  border-radius: 6px;
  padding: 0.75rem 0.9rem 0.6rem 0.9rem;
}
.guitar-viz-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.4rem;
}
.guitar-badge {
  display: inline-block;
  padding: 1px 6px;
  font-size: 0.7rem;
  font-weight: 700;
  border-radius: 3px;
  margin-right: 5px;
  text-transform: uppercase;
  vertical-align: middle;
}
.badge-orig {
  background: rgba(245, 158, 11, 0.18);
  color: #fbbf24;
  border: 1px solid rgba(245, 158, 11, 0.35);
}
.badge-clean {
  background: rgba(16, 185, 129, 0.18);
  color: #34d399;
  border: 1px solid rgba(16, 185, 129, 0.35);
}
.guitar-canvas {
  width: 100%;
  height: 85px;
  display: block;
  cursor: pointer;
  border-radius: 4px;
  background: #090a0c;
}
.guitar-demo-accordion {
  margin-top: 1.4rem;
  border: 1px solid #2a2d36;
  border-radius: 8px;
  background: #1a1c22;
  overflow: hidden;
  transition: border-color 0.2s;
}
.guitar-demo-accordion[open] {
  border-color: #3f4454;
}
.guitar-demo-summary {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.85rem 1.1rem;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.92rem;
  color: #e2e8f0;
  user-select: none;
  background: rgba(255, 255, 255, 0.02);
  transition: background 0.2s;
}
.guitar-demo-summary:hover {
  background: rgba(255, 255, 255, 0.05);
}
.guitar-demo-summary::-webkit-details-marker {
  display: none;
}
.guitar-summary-arrow {
  font-size: 0.75rem;
  color: #94a3b8;
  transition: transform 0.2s ease;
}
.guitar-demo-accordion[open] .guitar-summary-arrow {
  transform: rotate(90deg);
}
.guitar-demo-body {
  padding: 1rem 1.1rem 1.2rem 1.1rem;
  border-top: 1px solid #2a2d36;
}

/* Mixer Styles */
.mixer-master-bar {
  background: #181a20;
  border: 1px solid #282c37;
  border-radius: 8px;
  padding: 1rem 1.2rem;
  margin-bottom: 1.2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.8rem;
}
.mixer-timecode {
  font-family: monospace;
  font-size: 1.05rem;
  font-weight: 700;
  color: #34d399;
  background: #0f1013;
  padding: 0.3rem 0.6rem;
  border-radius: 4px;
  border: 1px solid #282c37;
}
.mixer-track-card {
  background: #1a1c22;
  border: 1px solid #2a2e3a;
  border-radius: 8px;
  padding: 1.1rem;
  margin-bottom: 1rem;
  transition: border-color 0.2s;
}
.mixer-track-card:hover {
  border-color: #3f4556;
}
.mixer-track-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 0.6rem;
}
.mixer-track-name-input {
  background: transparent;
  border: 1px solid transparent;
  color: #f1f5f9;
  font-weight: 600;
  font-size: 0.95rem;
  padding: 0.2rem 0.4rem;
  border-radius: 4px;
  width: 240px;
}
.mixer-track-name-input:focus {
  background: #0f1013;
  border-color: #10b981;
  outline: none;
}
.mixer-btn-tag {
  background: #252833;
  border: 1px solid #363b4b;
  color: #94a3b8;
  font-size: 0.78rem;
  font-weight: 700;
  padding: 0.25rem 0.55rem;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s;
}
.mixer-btn-tag:hover {
  background: #323746;
  color: #fff;
}
.mixer-btn-tag.active-mute {
  background: #dc2626;
  color: #fff;
  border-color: #dc2626;
}
.mixer-btn-tag.active-solo {
  background: #f59e0b;
  color: #fff;
  border-color: #f59e0b;
}
.mixer-canvas {
  width: 100%;
  height: 68px;
  display: block;
  background: #090a0c;
  border-radius: 4px;
  cursor: pointer;
}
.mixer-controls-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 0.7rem;
  margin-top: 0.8rem;
  background: #14151a;
  padding: 0.75rem;
  border-radius: 6px;
  border: 1px solid #232630;
}
.mixer-control-group {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}
.mixer-control-group label {
  font-size: 0.75rem;
  font-weight: 600;
  color: #94a3b8;
  display: flex;
  justify-content: space-between;
}
.mixer-control-group input[type="number"] {
  background: #090a0c;
  border: 1px solid #2e323f;
  color: #fff;
  padding: 0.3rem 0.4rem;
  border-radius: 4px;
  font-size: 0.85rem;
}
.mixer-control-group input[type="range"] {
  accent-color: #10b981;
  cursor: pointer;
}
</style>

<div class="guitar-app" id="guitarApp">
<div class="studio-tabs" style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 0.5rem;">
<div style="display: flex; gap: 0.5rem; flex-wrap: wrap;">
<button type="button" id="tabBtnCleaner" class="studio-tab-btn active">🧹 Audio Cleaner (Guitar & Vocal)</button>
<button type="button" id="tabBtnMixer" class="studio-tab-btn">🎛️ 5-Track Mixer & Cutter (<span id="trackCountBadge">0/5</span>)</button>
</div>
<button type="button" id="btnToggleFullscreen" class="guitar-btn-secondary" style="padding: 0.35rem 0.8rem; font-size: 0.82rem; margin-bottom: 0.2rem;" title="Expand studio to occupy 100% width and height of page">
<span id="fullscreenIcon">⛶</span> <span id="fullscreenText">Maximize Studio</span>
</button>
</div>

<!-- ================= TAB 1: AUDIO CLEANER ================= -->
<div id="tabContentCleaner">
<p style="margin-top: 0; color: #94a3b8; font-size: 0.92rem; line-height: 1.5;">
100% private, client-side cleaning. Removes background air, mic hiss, and room noise with custom DSP profiles for instruments and singing.
</p>

<!-- Mode Selector (Guitar vs Vocal) -->
<div style="display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 0.6rem; margin-bottom: 1rem; background: #131418; padding: 0.7rem 0.9rem; border-radius: 8px; border: 1px solid #262933;">
<span style="font-size: 0.88rem; font-weight: 600; color: #cbd5e1;">Target Profile:</span>
<div class="guitar-mode-toggle" id="targetModeToggle">
<button type="button" id="btnModeGuitar" class="guitar-toggle-btn active">🎸 Acoustic Guitar</button>
<button type="button" id="btnModeVocal" class="guitar-toggle-btn">🎙️ Vocal / Voice</button>
</div>
<span id="targetModeDesc" style="font-size: 0.78rem; color: #8492a6; width: 100%;">
🎸 Guitar Mode: Preserves warm wooden resonance and pick brightness while eliminating background static.
</span>
</div>

<div class="guitar-dropzone" id="dropZone">
<span style="font-size: 2.4rem;">📁</span>
<p style="margin: 0.5rem 0 0 0; font-weight: 600; font-size: 1rem;">Click to upload or drag & drop audio</p>
<p style="margin: 0.25rem 0 0 0; font-size: 0.82rem; color: #8492a6;">MP3, WAV, M4A, AAC, FLAC supported</p>
<input type="file" id="audioInput" accept="audio/*" />
</div>

<details class="guitar-demo-accordion" id="demoAccordion">
<summary class="guitar-demo-summary">
<span>🎧 Experience the Difference (Guitar Melody Demo)</span>
<span class="guitar-summary-arrow">▶</span>
</summary>
<div class="guitar-demo-body">
<p style="margin: 0 0 0.8rem 0; font-size: 0.84rem; color: #94a3b8; line-height: 1.45;">
Recorded with an acoustic guitar in a standard home room. Notice how the fuzzy room hiss in the pauses is completely silenced while the warm wooden resonance and string chime stay 100% natural.
</p>
<div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 0.6rem;">
<span style="font-size: 0.85rem; font-weight: 600; color: #cbd5e1;">A/B Demo Comparison:</span>
<div style="display: flex; align-items: center; gap: 0.6rem;">
<span id="demoTimeBadge" class="guitar-time-badge">⏱️ 0:00.0 / 1:00.7</span>
<div class="guitar-mode-toggle" id="demoModeToggle">
<button type="button" id="btnDemoClean" class="guitar-toggle-btn active">✨ Cleaned Melody</button>
<button type="button" id="btnDemoOrig" class="guitar-toggle-btn">📻 Raw (With Hiss)</button>
</div>
</div>
</div>
<audio id="demoAudioPlayer" class="guitar-player" controls preload="none"></audio>
<div class="guitar-viz-container" style="margin-top: 0.8rem;">
<div class="guitar-viz-card">
<div class="guitar-viz-header">
<span style="color: #f59e0b; font-weight: 600; font-size: 0.85rem;">
<span class="guitar-badge badge-orig">BEFORE</span> Raw Melody Recording (Hiss & Room Static)
</span>
<span style="font-size: 0.78rem; color: #94a3b8;">Floor: ~ -51 dBFS</span>
</div>
<canvas id="demoOrigCanvas" class="guitar-canvas"></canvas>
</div>
<div class="guitar-viz-card" style="margin-top: 0.8rem;">
<div class="guitar-viz-header">
<span style="color: #10b981; font-weight: 600; font-size: 0.85rem;">
<span class="guitar-badge badge-clean">AFTER</span> Cleaned Natural Guitar (Silent Floor & Enhanced Chime)
</span>
<span style="font-size: 0.78rem; color: #34d399;">Floor: ~ -98 dBFS (Silent -47 dB)</span>
</div>
<canvas id="demoCleanCanvas" class="guitar-canvas"></canvas>
</div>
<p style="margin: 0.4rem 0 0 0; font-size: 0.78rem; color: #64748b; text-align: right;">💡 Move mouse across waveform to preview exact time. Click to jump playback.</p>
</div>
</div>
</details>

<div id="actionArea" class="guitar-section" style="display: none;">
<p style="margin: 0 0 0.5rem 0; font-size: 0.9rem;" id="fileInfo"></p>
<label for="renameInput" style="font-size: 0.88rem; font-weight: 600; display: block;">Custom Output Filename:</label>
<input type="text" id="renameInput" class="guitar-input-field" value="cleaned-track.mp3" placeholder="cleaned-track.mp3" />
<button id="processBtn" class="guitar-btn" style="margin-top: 1rem;">Clean Audio Now</button>
<div id="statusText" class="guitar-status"></div>
</div>

<div id="resultArea" class="guitar-section" style="display: none;">
<div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 0.6rem;">
<h4 style="margin: 0; font-size: 1.05rem;">🎧 Audio Preview & Comparison</h4>
<div style="display: flex; align-items: center; gap: 0.6rem;">
<span id="cleanerTimeBadge" class="guitar-time-badge">⏱️ 0:00.0 / 0:00.0</span>
<div class="guitar-mode-toggle" id="modeToggle">
<button type="button" id="btnPlayClean" class="guitar-toggle-btn active">✨ Cleaned</button>
<button type="button" id="btnPlayOrig" class="guitar-toggle-btn">📻 Original</button>
</div>
</div>
</div>
<audio id="audioPlayer" class="guitar-player" controls></audio>
<div class="guitar-viz-container">
<div class="guitar-viz-card">
<div class="guitar-viz-header">
<span style="color: #f59e0b; font-weight: 600; font-size: 0.85rem;">
<span class="guitar-badge badge-orig">BEFORE</span> Original (Raw Input with Hiss)
</span>
<span id="origDbStat" style="font-size: 0.78rem; color: #94a3b8;"></span>
</div>
<canvas id="origCanvas" class="guitar-canvas"></canvas>
</div>
<div class="guitar-viz-card" style="margin-top: 0.8rem;">
<div class="guitar-viz-header">
<span style="color: #10b981; font-weight: 600; font-size: 0.85rem;">
<span class="guitar-badge badge-clean">AFTER</span> Cleaned (Silent Floor & Enhanced Clarity)
</span>
<span id="cleanDbStat" style="font-size: 0.78rem; color: #34d399;"></span>
</div>
<canvas id="cleanCanvas" class="guitar-canvas"></canvas>
</div>
<p style="margin: 0.4rem 0 0 0; font-size: 0.78rem; color: #64748b; text-align: right;">💡 Move mouse across waveform to see time. Click to jump playback.</p>
</div>

<!-- Download and Send to Mixer -->
<div style="margin-top: 1.2rem; display: flex; gap: 0.8rem; align-items: center; flex-wrap: wrap;">
<button type="button" id="btnDownloadMp3" class="guitar-btn-download-mp3">⬇️ Download MP3 (~1.5MB WhatsApp)</button>
<button type="button" id="btnDownloadWav" class="guitar-btn-download-wav">⬇️ Lossless WAV</button>
<button type="button" id="btnSendToMixer" class="guitar-btn-secondary" style="border-color: #10b981; color: #34d399;">➕ Send to 5-Track Mixer</button>
</div>
<div id="exportStatus" style="font-size: 0.84rem; color: #94a3b8; margin-top: 0.4rem;"></div>
</div>
</div>

<!-- ================= TAB 2: MULTI-TRACK MIXER ================= -->
<div id="tabContentMixer" style="display: none;">
<p style="margin-top: 0; color: #94a3b8; font-size: 0.92rem; line-height: 1.5;">
Combine your cleaned guitar and friend's vocal tracks. Cut snippets, duplicate/copy riffs, set starting delays, and export a ready-to-share MP3.
</p>

<!-- Mixer Master Bar -->
<div class="mixer-master-bar">
<div style="display: flex; align-items: center; gap: 0.6rem;">
<button type="button" id="btnMixerPlay" class="guitar-btn" style="margin: 0; padding: 0.5rem 1rem;">▶ Play Mix</button>
<button type="button" id="btnMixerStop" class="guitar-btn-secondary" style="margin: 0; padding: 0.5rem 0.8rem;">⏹ Stop</button>
<span id="mixerTimecode" class="mixer-timecode">00:00.0 / 00:00.0</span>
</div>
<div style="display: flex; align-items: center; gap: 0.8rem;">
<label style="font-size: 0.82rem; font-weight: 600; color: #94a3b8;">Master Vol:</label>
<input type="range" id="mixerMasterVolume" min="0" max="1.5" step="0.05" value="1.0" style="width: 100px; accent-color: #10b981;" />
<span id="mixerMasterVolVal" style="font-size: 0.82rem; color: #34d399;">100%</span>
</div>
<div style="display: flex; gap: 0.5rem; flex-wrap: wrap;">
<button type="button" id="btnMixerAddTrack" class="guitar-btn-secondary" style="border-color: #3b82f6; color: #60a5fa;">➕ Add Audio File</button>
<input type="file" id="mixerFileInput" accept="audio/*" style="display: none;" />
<button type="button" id="btnExportMixMp3" class="guitar-btn-download-mp3" style="padding: 0.5rem 1rem;">⬇️ Export Mix MP3</button>
<button type="button" id="btnExportMixWav" class="guitar-btn-download-wav" style="padding: 0.5rem 0.9rem;">WAV</button>
</div>
</div>
<div id="mixerExportStatus" style="font-size: 0.85rem; color: #34d399; margin-bottom: 0.8rem;"></div>

<!-- Track List Container -->
<div id="mixerTracksList">
<div id="mixerEmptyNotice" style="text-align: center; padding: 3rem 1.5rem; border: 2px dashed #2a2e3a; border-radius: 8px; color: #8492a6;">
<span style="font-size: 2.2rem;">🎛️</span>
<p style="margin: 0.5rem 0 0 0; font-weight: 600;">No tracks in mixer yet.</p>
<p style="margin: 0.2rem 0 1rem 0; font-size: 0.85rem;">Clean an audio above and click "Send to 5-Track Mixer", or click "Add Audio File" to load tracks directly.</p>
<button type="button" class="guitar-btn" onclick="document.getElementById('mixerFileInput').click();">➕ Add First Track</button>
</div>
</div>
</div>

</div>

<script>
(function() {
'use strict';

function formatTimeExact(sec) {
  if (isNaN(sec) || sec < 0) sec = 0;
  const m = Math.floor(sec / 60);
  const s = Math.floor(sec % 60);
  const ms = Math.floor((sec % 1) * 10);
  return `${m}:${s < 10 ? '0' : ''}${s}.${ms}`;
}

// --- Dynamic LAME.js MP3 Encoder Loader ---
let lameJsPromise = null;
function loadLameJs() {
  if (window.lamejs) return Promise.resolve(window.lamejs);
  if (lameJsPromise) return lameJsPromise;
  lameJsPromise = new Promise((resolve, reject) => {
    const s = document.createElement('script');
    s.src = 'https://cdnjs.cloudflare.com/ajax/libs/lamejs/1.2.1/lame.all.min.js';
    s.onload = () => resolve(window.lamejs);
    s.onerror = () => {
      const s2 = document.createElement('script');
      s2.src = 'https://cdn.jsdelivr.net/npm/lamejs@1.2.1/lame.min.js';
      s2.onload = () => resolve(window.lamejs);
      s2.onerror = reject;
      document.head.appendChild(s2);
    };
    document.head.appendChild(s);
  });
  return lameJsPromise;
}
setTimeout(loadLameJs, 1500);

async function encodeMp3(leftChannel, rightChannel, sampleRate, kbps = 192) {
  const lame = await loadLameJs();
  const channels = rightChannel ? 2 : 1;
  const mp3encoder = new lame.Mp3Encoder(channels, sampleRate, kbps);

  function floatToInt16(floatArr) {
    const len = floatArr.length;
    const int16 = new Int16Array(len);
    for (let i = 0; i < len; i++) {
      const s = Math.max(-1, Math.min(1, floatArr[i]));
      int16[i] = s < 0 ? s * 0x8000 : s * 0x7FFF;
    }
    return int16;
  }

  const leftInt16 = floatToInt16(leftChannel);
  const rightInt16 = rightChannel ? floatToInt16(rightChannel) : leftInt16;

  const mp3Data = [];
  const sampleBlockSize = 1152;
  const numSamples = leftInt16.length;

  for (let i = 0; i < numSamples; i += sampleBlockSize) {
    const leftChunk = leftInt16.subarray(i, i + sampleBlockSize);
    const rightChunk = rightInt16.subarray(i, i + sampleBlockSize);
    const mp3buf = channels === 2
      ? mp3encoder.encodeBuffer(leftChunk, rightChunk)
      : mp3encoder.encodeBuffer(leftChunk);
    if (mp3buf.length > 0) mp3Data.push(mp3buf);
  }
  const flushBuf = mp3encoder.flush();
  if (flushBuf.length > 0) mp3Data.push(flushBuf);

  return new Blob(mp3Data, { type: 'audio/mp3' });
}

// Precomputed waveform bars for the demo melody
const DEMO_RAW_BARS = [0.015, 0.028, 0.336, 0.279, 0.325, 0.33, 0.243, 0.14, 0.128, 0.092, 0.178, 0.1, 0.212, 0.118, 0.071, 0.033, 0.12, 0.15, 0.091, 0.064, 0.131, 0.1, 0.108, 0.1, 0.106, 0.163, 0.097, 0.224, 0.155, 0.094, 0.4, 0.297, 0.144, 0.079, 0.106, 0.076, 0.039, 0.155, 0.15, 0.198, 0.176, 0.118, 0.027, 0.082, 0.046, 0.074, 0.074, 0.114, 0.159, 0.163, 0.151, 0.104, 0.079, 0.175, 0.125, 0.143, 0.321, 0.164, 0.252, 0.214, 0.273, 0.118, 0.091, 0.24, 0.115, 0.08, 0.088, 0.036, 0.102, 0.06, 0.148, 0.062, 0.062, 0.07, 0.126, 0.024, 0.029, 0.108, 0.05, 0.04, 0.175, 0.138, 0.124, 0.128, 0.071, 0.087, 0.116, 0.153, 0.19, 0.294, 0.302, 0.126, 0.155, 0.364, 0.23, 0.231, 0.041, 0.166, 0.1, 0.077, 0.135, 0.163, 0.068, 0.141, 0.103, 0.077, 0.071, 0.049, 0.034, 0.104, 0.09, 0.075, 0.083, 0.056, 0.04, 0.023, 0.203, 0.128, 0.034, 0.129, 0.14, 0.245, 0.169, 0.117, 0.236, 0.278, 0.154, 0.369, 0.212, 0.088, 0.162, 0.126, 0.091, 0.127, 0.076, 0.052, 0.142, 0.096, 0.045, 0.155, 0.093, 0.05, 0.038, 0.089, 0.075, 0.107, 0.252, 0.126, 0.036, 0.075, 0.137, 0.097, 0.232, 0.159, 0.188, 0.34, 0.27, 0.195, 0.194, 0.254, 0.124, 0.241, 0.154, 0.249, 0.06, 0.099, 0.116, 0.063, 0.108, 0.072, 0.072, 0.054, 0.031, 0.111, 0.113, 0.017, 0.087, 0.081, 0.165, 0.098, 0.037, 0.044, 0.144, 0.185, 0.178, 0.057, 0.098, 0.231, 0.289, 0.1, 0.218, 0.179, 0.233, 0.204, 0.162, 0.118, 0.064, 0.159, 0.039, 0.033];
const DEMO_CLEAN_BARS = [0.0, 0.133, 0.089, 0.74, 0.664, 0.72, 0.464, 0.72, 0.574, 0.648, 0.355, 0.435, 0.585, 0.487, 0.449, 0.414, 0.758, 0.478, 0.387, 0.706, 0.552, 0.675, 0.577, 0.642, 0.383, 0.314, 0.635, 0.393, 0.319, 0.331, 0.335, 0.352, 0.474, 0.474, 0.771, 0.364, 0.395, 0.649, 0.577, 0.534, 0.354, 0.33, 0.382, 0.503, 0.58, 0.538, 0.492, 0.616, 0.556, 0.719, 0.669, 0.367, 0.452, 0.509, 0.43, 0.476, 0.308, 0.313, 0.623, 0.713, 0.667, 0.623, 0.725, 0.586, 0.48, 0.538, 0.433, 0.767, 0.712, 0.706, 0.506, 0.653, 0.517, 0.7, 0.453, 0.763, 0.689, 0.665, 0.415, 0.498, 0.734, 0.581, 0.737, 0.609, 0.786, 0.743, 0.618, 0.734, 0.649, 0.731, 0.54, 0.627, 0.299, 0.629, 0.68, 0.311, 0.46, 0.411, 0.387, 0.459, 0.61, 0.815, 0.357, 0.719, 0.448, 0.445, 0.527, 0.601, 0.615, 0.273, 0.675, 0.406, 0.517, 0.701, 0.512, 0.64, 0.679, 0.344, 0.451, 0.685, 0.732, 0.548, 0.559, 0.363, 0.355, 0.648, 0.551, 0.768, 0.609, 0.735, 0.457, 0.421, 0.391, 0.793, 0.497, 0.404, 0.312, 0.818, 0.607, 0.512, 0.707, 0.658, 0.519, 0.753, 0.593, 0.592, 0.487, 0.212, 0.707, 0.6, 0.761, 0.508, 0.459, 0.763, 0.536, 0.629, 0.697, 0.519, 0.437, 0.547, 0.589, 0.652, 0.585, 0.537, 0.543, 0.467, 0.779, 0.503, 0.704, 0.723, 0.566, 0.738, 0.577, 0.766, 0.424, 0.322, 0.398, 0.642, 0.394, 0.517, 0.342, 0.598, 0.77, 0.66, 0.718, 0.592, 0.52, 0.594, 0.599, 0.611, 0.555, 0.582, 0.431, 0.502, 0.402, 0.434, 0.264, 0.351, 0.183, 0.445];

// --- In-Browser DSP Engine (Guitar & Vocal) ---
class RealFFT {
  constructor(size) {
    this.size = size;
    this.cosTable = new Float32Array(size / 2);
    this.sinTable = new Float32Array(size / 2);
    for (let i = 0; i < size / 2; i++) {
      this.cosTable[i] = Math.cos((2 * Math.PI * i) / size);
      this.sinTable[i] = Math.sin((2 * Math.PI * i) / size);
    }
  }

  forward(real, imag) {
    const n = this.size;
    let j = 0;
    for (let i = 0; i < n - 1; i++) {
      if (i < j) {
        let tr = real[i]; real[i] = real[j]; real[j] = tr;
        let ti = imag[i]; imag[i] = imag[j]; imag[j] = ti;
      }
      let k = n >> 1;
      while (k <= j) {
        j -= k;
        k >>= 1;
      }
      j += k;
    }

    for (let len = 2; len <= n; len <<= 1) {
      const half = len >> 1;
      const step = n / len;
      for (let i = 0; i < n; i += len) {
        for (let k = 0; k < half; k++) {
          const idx = k * step;
          const c = this.cosTable[idx];
          const s = this.sinTable[idx];
          const tr = real[i + k + half] * c + imag[i + k + half] * s;
          const ti = imag[i + k + half] * c - real[i + k + half] * s;
          real[i + k + half] = real[i + k] - tr;
          imag[i + k + half] = imag[i + k] - ti;
          real[i + k] += tr;
          imag[i + k] += ti;
        }
      }
    }
  }

  inverse(real, imag) {
    const n = this.size;
    for (let i = 0; i < n; i++) imag[i] = -imag[i];
    this.forward(real, imag);
    for (let i = 0; i < n; i++) {
      real[i] /= n;
      imag[i] = -imag[i] / n;
    }
  }
}

function processAudioDSP(rawInputSamples, sampleRate, mode = 'guitar') {
  const isVocal = mode === 'vocal';
  const outLen = rawInputSamples.length;
  const samples = new Float32Array(outLen);
  samples.set(rawInputSamples);

  // 80Hz Rumble Highpass for Vocal
  if (isVocal) {
    const cutoff = 80;
    const rc = 1.0 / (2.0 * Math.PI * cutoff);
    const dt = 1.0 / sampleRate;
    const alphaHp = rc / (rc + dt);
    let prevIn = samples[0];
    let prevOut = samples[0];
    for (let i = 0; i < outLen; i++) {
      const curIn = samples[i];
      const curOut = alphaHp * (prevOut + curIn - prevIn);
      prevIn = curIn;
      prevOut = curOut;
      samples[i] = curOut;
    }
  }

  const fftLen = 2048;
  const hopLen = 512;
  const numBins = fftLen / 2 + 1;
  const binHz = sampleRate / fftLen;

  const fft = new RealFFT(fftLen);
  const hann = new Float32Array(fftLen);
  for (let i = 0; i < fftLen; i++) {
    hann[i] = 0.5 * (1.0 - Math.cos((2.0 * Math.PI * i) / fftLen));
  }

  const numFrames = Math.floor((samples.length - fftLen) / hopLen);
  const frameEnergies = [];
  const real = new Float32Array(fftLen);
  const imag = new Float32Array(fftLen);

  for (let f = 0; f < numFrames; f++) {
    const start = f * hopLen;
    let sumSq = 0;
    for (let i = 0; i < fftLen; i++) {
      const s = samples[start + i];
      sumSq += s * s;
    }
    frameEnergies.push({ f, energy: sumSq / fftLen });
  }

  frameEnergies.sort((a, b) => a.energy - b.energy);
  const noiseFramesCount = Math.min(30, frameEnergies.length);
  const rawNoiseProfile = new Float32Array(numBins);

  for (let k = 0; k < noiseFramesCount; k++) {
    const fIdx = frameEnergies[k].f;
    const start = fIdx * hopLen;
    for (let i = 0; i < fftLen; i++) {
      real[i] = samples[start + i] * hann[i];
      imag[i] = 0;
    }
    fft.forward(real, imag);
    for (let b = 0; b < numBins; b++) {
      rawNoiseProfile[b] += Math.hypot(real[b], imag[b]) / noiseFramesCount;
    }
  }

  const noiseProfile = new Float32Array(numBins);
  for (let b = 0; b < numBins; b++) {
    const left = b > 0 ? rawNoiseProfile[b - 1] : rawNoiseProfile[b];
    const center = rawNoiseProfile[b];
    const right = b + 1 < numBins ? rawNoiseProfile[b + 1] : rawNoiseProfile[b];
    noiseProfile[b] = 0.25 * left + 0.5 * center + 0.25 * right;
  }

  // Alpha Over-subtraction Curve
  const alphaCurve = new Float32Array(numBins);
  for (let b = 0; b < numBins; b++) {
    const freq = b * binHz;
    if (isVocal) {
      if (freq < 100) alphaCurve[b] = 4.5;
      else if (freq < 300) alphaCurve[b] = 2.0;
      else if (freq < 3500) alphaCurve[b] = 2.2;
      else if (freq < 7500) alphaCurve[b] = 2.8;
      else alphaCurve[b] = 4.0;
    } else {
      if (freq < 800) alphaCurve[b] = 1.8;
      else if (freq < 2000) alphaCurve[b] = 2.6;
      else if (freq < 8000) alphaCurve[b] = 3.6;
      else if (freq < 11000) alphaCurve[b] = 4.2;
      else alphaCurve[b] = 5.5;
    }
  }

  const output = new Float32Array(samples.length + fftLen);
  const normSum = new Float32Array(samples.length + fftLen);
  const prevGains = new Float32Array(numBins).fill(1.0);
  const gammaSmooth = isVocal ? 0.75 : 0.70;
  const minFloor = 0.001;

  for (let f = 0; f < numFrames; f++) {
    const start = f * hopLen;
    for (let i = 0; i < fftLen; i++) {
      real[i] = samples[start + i] * hann[i];
      imag[i] = 0;
    }
    fft.forward(real, imag);

    for (let b = 0; b < numBins; b++) {
      const mag = Math.hypot(real[b], imag[b]);
      const effectiveNoise = noiseProfile[b] * alphaCurve[b];

      let instantGain;
      if (mag > effectiveNoise * 1.05) {
        const snr = (mag - effectiveNoise) / Math.max(effectiveNoise, 1e-6);
        instantGain = snr / (1.0 + snr);
        instantGain = Math.pow(instantGain, 0.85);
      } else {
        const ratio = mag / Math.max(effectiveNoise, 1e-6);
        instantGain = minFloor * Math.pow(Math.max(0, ratio), 2.5);
      }

      const smoothedGain = gammaSmooth * prevGains[b] + (1.0 - gammaSmooth) * instantGain;
      prevGains[b] = smoothedGain;

      real[b] *= smoothedGain;
      imag[b] *= smoothedGain;
      if (b > 0 && b < fftLen / 2) {
        const mirror = fftLen - b;
        real[mirror] = real[b];
        imag[mirror] = -imag[b];
      }
    }

    fft.inverse(real, imag);

    for (let i = 0; i < fftLen; i++) {
      const outIdx = start + i;
      output[outIdx] += real[i] * hann[i];
      normSum[outIdx] += hann[i] * hann[i];
    }
  }

  const cleaned = new Float32Array(outLen);
  for (let i = 0; i < outLen; i++) {
    cleaned[i] = normSum[i] > 1e-4 ? output[i] / normSum[i] : output[i];
  }

  // Downward Expander (Smooth release for vocal breath/tail preservation)
  let env = 0;
  const attackMs = isVocal ? 0.012 : 0.008;
  const releaseMs = isVocal ? 0.120 : 0.040;
  const threshold = isVocal ? 0.0045 : 0.006;
  const attack = Math.exp(-1.0 / (sampleRate * attackMs));
  const release = Math.exp(-1.0 / (sampleRate * releaseMs));

  for (let i = 0; i < outLen; i++) {
    const s = Math.abs(cleaned[i]);
    env = s > env ? attack * env + (1.0 - attack) * s : release * env + (1.0 - release) * s;

    let expanderGain = 1.0;
    if (env < threshold) {
      const ratio = env / threshold;
      expanderGain = Math.pow(ratio, 2.0);
    }
    cleaned[i] *= expanderGain;
  }

  // Lookahead Limiter (+4.5 dB for guitar, +3.5 dB for vocal)
  const lookaheadMs = 4.0;
  const lookaheadSamples = Math.floor(sampleRate * (lookaheadMs / 1000.0));
  const targetGain = isVocal ? 1.50 : 1.68;
  const ceiling = 0.95;
  const releaseCoeff = Math.exp(-1.0 / (sampleRate * 0.060));

  let limiterGain = 1.0;
  const finalSamples = new Float32Array(outLen);

  for (let i = 0; i < outLen; i++) {
    let localMax = 0;
    const windowEnd = Math.min(outLen, i + lookaheadSamples);
    for (let j = i; j < windowEnd; j++) {
      const a = Math.abs(cleaned[j]) * targetGain;
      if (a > localMax) localMax = a;
    }

    let desiredGain = 1.0;
    if (localMax > ceiling) {
      desiredGain = ceiling / localMax;
    }

    if (desiredGain < limiterGain) {
      limiterGain = desiredGain;
    } else {
      limiterGain = releaseCoeff * limiterGain + (1.0 - releaseCoeff) * desiredGain;
    }

    finalSamples[i] = Math.max(-0.98, Math.min(0.98, cleaned[i] * targetGain * limiterGain));
  }

  return finalSamples;
}

function encodeWav(monoSamples, sampleRate) {
  const numChannels = 2;
  const bitsPerSample = 16;
  const bytesPerSample = bitsPerSample / 8;
  const blockAlign = numChannels * bytesPerSample;
  const dataSize = monoSamples.length * blockAlign;
  const buffer = new ArrayBuffer(44 + dataSize);
  const view = new DataView(buffer);

  function writeString(offset, str) {
    for (let i = 0; i < str.length; i++) {
      view.setUint8(offset + i, str.charCodeAt(i));
    }
  }

  writeString(0, 'RIFF');
  view.setUint32(4, 36 + dataSize, true);
  writeString(8, 'WAVE');
  writeString(12, 'fmt ');
  view.setUint32(16, 16, true);
  view.setUint16(20, 1, true);
  view.setUint16(22, numChannels, true);
  view.setUint32(24, sampleRate, true);
  view.setUint32(28, sampleRate * blockAlign, true);
  view.setUint16(32, blockAlign, true);
  view.setUint16(34, 16, true);
  writeString(36, 'data');
  view.setUint32(40, dataSize, true);

  let offset = 44;
  for (let i = 0; i < monoSamples.length; i++) {
    const s = Math.max(-1, Math.min(1, monoSamples[i]));
    const val = s < 0 ? s * 0x8000 : s * 0x7FFF;
    const sampleInt16 = Math.round(val);
    view.setInt16(offset, sampleInt16, true);
    view.setInt16(offset + 2, sampleInt16, true);
    offset += 4;
  }

  return new Blob([buffer], { type: 'audio/wav' });
}

function computeWaveformBars(samples, barCount) {
  const bars = new Float32Array(barCount);
  const step = samples.length / barCount;
  for (let i = 0; i < barCount; i++) {
    const start = Math.floor(i * step);
    const end = Math.floor((i + 1) * step);
    let peak = 0;
    for (let j = start; j < end; j++) {
      const v = Math.abs(samples[j]);
      if (v > peak) peak = v;
    }
    bars[i] = Math.min(1.0, peak);
  }
  return bars;
}

function drawWaveform(canvas, bars, mainColor, playedColor, progressRatio, trimRatioStart = 0, trimRatioEnd = 1, hoverRatio = -1, totalDuration = 0) {
  if (!canvas || !bars) return;
  const dpr = window.devicePixelRatio || 1;
  const width = canvas.clientWidth || 800;
  const height = canvas.clientHeight || 85;

  if (canvas.width !== Math.round(width * dpr) || canvas.height !== Math.round(height * dpr)) {
    canvas.width = Math.round(width * dpr);
    canvas.height = Math.round(height * dpr);
  }

  const ctx = canvas.getContext('2d');
  ctx.save();
  ctx.scale(dpr, dpr);
  ctx.clearRect(0, 0, width, height);

  const midY = height / 2;

  // Zero-axis reference
  ctx.strokeStyle = 'rgba(255, 255, 255, 0.08)';
  ctx.lineWidth = 1;
  ctx.beginPath();
  ctx.moveTo(0, midY);
  ctx.lineTo(width, midY);
  ctx.stroke();

  const barCount = bars.length;
  const barWidth = width / barCount;
  const playheadX = progressRatio * width;
  const bw = Math.max(1, barWidth - 1);

  const trimStartX = trimRatioStart * width;
  const trimEndX = trimRatioEnd * width;

  for (let i = 0; i < barCount; i++) {
    const x = i * barWidth;
    const peak = bars[i];
    const h = Math.max(1.5, peak * (height * 0.44));

    const isInsideTrim = x >= trimStartX && x <= trimEndX;
    if (isInsideTrim) {
      ctx.fillStyle = (x <= playheadX) ? playedColor : mainColor;
    } else {
      ctx.fillStyle = 'rgba(255, 255, 255, 0.12)';
    }

    ctx.fillRect(x, midY - h, bw, h * 2);
  }

  // Playhead line & time tag
  if (progressRatio > 0 && progressRatio < 1) {
    ctx.strokeStyle = '#ffffff';
    ctx.lineWidth = 1.5;
    ctx.beginPath();
    ctx.moveTo(playheadX, 0);
    ctx.lineTo(playheadX, height);
    ctx.stroke();

    ctx.fillStyle = '#ffffff';
    ctx.beginPath();
    ctx.arc(playheadX, midY, 2.5, 0, Math.PI * 2);
    ctx.fill();

    if (totalDuration > 0) {
      const curSec = progressRatio * totalDuration;
      const timeStr = formatTimeExact(curSec);
      ctx.font = 'bold 10px monospace';
      const tw = ctx.measureText(timeStr).width;
      const pillX = Math.max(tw / 2 + 4, Math.min(width - tw / 2 - 4, playheadX));
      ctx.fillStyle = 'rgba(15, 23, 42, 0.9)';
      ctx.fillRect(pillX - tw / 2 - 4, 3, tw + 8, 14);
      ctx.fillStyle = '#34d399';
      ctx.fillText(timeStr, pillX - tw / 2, 14);
    }
  }

  // Hover cursor line & time tooltip
  if (hoverRatio >= 0 && hoverRatio <= 1 && totalDuration > 0) {
    const hx = hoverRatio * width;
    ctx.strokeStyle = 'rgba(56, 189, 248, 0.8)';
    ctx.setLineDash([3, 3]);
    ctx.lineWidth = 1;
    ctx.beginPath();
    ctx.moveTo(hx, 0);
    ctx.lineTo(hx, height);
    ctx.stroke();
    ctx.setLineDash([]);

    const hoverSec = hoverRatio * totalDuration;
    const hoverStr = formatTimeExact(hoverSec);
    ctx.font = 'bold 10px monospace';
    const tw = ctx.measureText(hoverStr).width;
    const pillX = Math.max(tw / 2 + 4, Math.min(width - tw / 2 - 4, hx));
    ctx.fillStyle = '#0284c7';
    ctx.fillRect(pillX - tw / 2 - 4, height - 16, tw + 8, 14);
    ctx.fillStyle = '#ffffff';
    ctx.fillText(hoverStr, pillX - tw / 2, height - 5);
  }

  ctx.restore();
}

function estimateNoiseFloorDb(samples, sampleRate) {
  const chunkSize = Math.floor(sampleRate * 0.1);
  if (samples.length < chunkSize) return -90;
  let minRms = 1.0;
  for (let i = 0; i < samples.length - chunkSize; i += chunkSize) {
    let sumSq = 0;
    for (let j = 0; j < chunkSize; j++) {
      const s = samples[i + j];
      sumSq += s * s;
    }
    const rms = Math.sqrt(sumSq / chunkSize);
    if (rms < minRms) minRms = rms;
  }
  return 20 * Math.log10(Math.max(minRms, 1e-6));
}

// --- Safe Initializer for DOM Elements ---
function initGuitarStudio() {
  const el = id => document.getElementById(id);

  // Tabs
  const tabBtnCleaner = el('tabBtnCleaner');
  const tabBtnMixer = el('tabBtnMixer');
  const tabContentCleaner = el('tabContentCleaner');
  const tabContentMixer = el('tabContentMixer');
  const trackCountBadge = el('trackCountBadge');

  function switchTab(target) {
    if (target === 'cleaner') {
      tabBtnCleaner.classList.add('active');
      tabBtnMixer.classList.remove('active');
      tabContentCleaner.style.display = 'block';
      tabContentMixer.style.display = 'none';
    } else {
      tabBtnMixer.classList.add('active');
      tabBtnCleaner.classList.remove('active');
      tabContentMixer.style.display = 'block';
      tabContentCleaner.style.display = 'none';
      renderAllMixerTracks();
    }
  }

  if (tabBtnCleaner) tabBtnCleaner.addEventListener('click', () => switchTab('cleaner'));
  if (tabBtnMixer) tabBtnMixer.addEventListener('click', () => switchTab('mixer'));

  // Fullscreen / Maximize Studio Logic
  const guitarApp = el('guitarApp');
  const btnToggleFullscreen = el('btnToggleFullscreen');
  const fullscreenIcon = el('fullscreenIcon');
  const fullscreenText = el('fullscreenText');

  function toggleFullscreen() {
    if (!guitarApp) return;
    const isMax = guitarApp.classList.toggle('maximized');
    if (fullscreenIcon) fullscreenIcon.textContent = isMax ? '⤢' : '⛶';
    if (fullscreenText) fullscreenText.textContent = isMax ? 'Restore Size' : 'Maximize Studio';
    setTimeout(() => {
      renderWaveforms();
      if (demoAccordion && demoAccordion.open) renderDemoWaveforms();
      if (mixerTracks.length > 0) {
        mixerTracks.forEach(t => renderTrackWaveform(t));
      }
    }, 60);
  }

  if (btnToggleFullscreen) {
    btnToggleFullscreen.addEventListener('click', toggleFullscreen);
  }

  window.addEventListener('keydown', (e) => {
    if (e.key === 'Escape' && guitarApp && guitarApp.classList.contains('maximized')) {
      toggleFullscreen();
    }
  });

  // Cleaner Elements
  const dropZone = el('dropZone');
  const audioInput = el('audioInput');
  const actionArea = el('actionArea');
  const resultArea = el('resultArea');
  const processBtn = el('processBtn');
  const statusText = el('statusText');
  const renameInput = el('renameInput');
  const audioPlayer = el('audioPlayer');
  const origCanvas = el('origCanvas');
  const cleanCanvas = el('cleanCanvas');
  const btnPlayClean = el('btnPlayClean');
  const btnPlayOrig = el('btnPlayOrig');
  const origDbStat = el('origDbStat');
  const cleanDbStat = el('cleanDbStat');
  const cleanerTimeBadge = el('cleanerTimeBadge');
  const btnModeGuitar = el('btnModeGuitar');
  const btnModeVocal = el('btnModeVocal');
  const targetModeDesc = el('targetModeDesc');
  const btnDownloadMp3 = el('btnDownloadMp3');
  const btnDownloadWav = el('btnDownloadWav');
  const btnSendToMixer = el('btnSendToMixer');
  const exportStatus = el('exportStatus');

  let currentTargetMode = 'guitar';
  if (btnModeGuitar) {
    btnModeGuitar.addEventListener('click', () => {
      currentTargetMode = 'guitar';
      btnModeGuitar.classList.add('active');
      btnModeVocal.classList.remove('active');
      if (targetModeDesc) targetModeDesc.textContent = '🎸 Guitar Mode: Preserves warm wooden resonance and pick brightness while eliminating background static.';
    });
  }
  if (btnModeVocal) {
    btnModeVocal.addEventListener('click', () => {
      currentTargetMode = 'vocal';
      btnModeVocal.classList.add('active');
      btnModeGuitar.classList.remove('active');
      if (targetModeDesc) targetModeDesc.textContent = '🎙️ Vocal Mode: Applies 80Hz rumble cut, protects vocal vowel formants, smooth release to keep natural breath decays.';
    });
  }

  // Demo Section
  const demoAccordion = el('demoAccordion');
  const demoAudioPlayer = el('demoAudioPlayer');
  const demoOrigCanvas = el('demoOrigCanvas');
  const demoCleanCanvas = el('demoCleanCanvas');
  const btnDemoClean = el('btnDemoClean');
  const btnDemoOrig = el('btnDemoOrig');
  const demoTimeBadge = el('demoTimeBadge');

  let loadedAudioBuffer = null;
  let originalFileName = "cleaned-track";
  let origBars = null;
  let cleanBars = null;
  let origWavUrl = null;
  let cleanWavUrl = null;
  let cleanSamplesCache = null;
  let cleanSampleRate = 44100;
  let currentMode = 'clean';
  let animFrameId = null;

  let cleanerHoverRatio = -1;
  let demoHoverRatio = -1;

  // Demo state
  const DEMO_CLEAN_URL = "/audio/guitar_natural_pure.mp3";
  const DEMO_ORIG_URL = "/audio/guitar_raw.mp3";
  let demoMode = 'clean';
  let demoAnimFrameId = null;
  const demoDuration = 60.7;

  function getDemoProgress() {
    if (!demoAudioPlayer || !demoAudioPlayer.duration) return 0;
    return Math.max(0, Math.min(1, demoAudioPlayer.currentTime / demoAudioPlayer.duration));
  }

  function renderDemoWaveforms() {
    const prog = getDemoProgress();
    const curTime = demoAudioPlayer ? demoAudioPlayer.currentTime : 0;
    const dur = demoAudioPlayer && demoAudioPlayer.duration ? demoAudioPlayer.duration : demoDuration;
    if (demoTimeBadge) {
      demoTimeBadge.textContent = `⏱️ ${formatTimeExact(curTime)} / ${formatTimeExact(dur)}`;
    }
    if (demoOrigCanvas) {
      drawWaveform(demoOrigCanvas, DEMO_RAW_BARS, 'rgba(245, 158, 11, 0.45)', '#f59e0b', prog, 0, 1, demoHoverRatio, dur);
    }
    if (demoCleanCanvas) {
      drawWaveform(demoCleanCanvas, DEMO_CLEAN_BARS, 'rgba(16, 185, 129, 0.45)', '#10b981', prog, 0, 1, demoHoverRatio, dur);
    }
  }

  function startDemoPlaybackLoop() {
    if (demoAnimFrameId) cancelAnimationFrame(demoAnimFrameId);
    function loop() {
      renderDemoWaveforms();
      if (!demoAudioPlayer.paused && !demoAudioPlayer.ended) {
        demoAnimFrameId = requestAnimationFrame(loop);
      }
    }
    demoAnimFrameId = requestAnimationFrame(loop);
  }

  function attachDemoListeners(canvas) {
    if (!canvas || !demoAudioPlayer) return;
    canvas.addEventListener('click', (e) => {
      const dur = demoAudioPlayer.duration || demoDuration;
      const rect = canvas.getBoundingClientRect();
      const ratio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
      demoAudioPlayer.currentTime = ratio * dur;
      renderDemoWaveforms();
    });
    canvas.addEventListener('mousemove', (e) => {
      const rect = canvas.getBoundingClientRect();
      demoHoverRatio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
      renderDemoWaveforms();
    });
    canvas.addEventListener('mouseleave', () => {
      demoHoverRatio = -1;
      renderDemoWaveforms();
    });
  }

  attachDemoListeners(demoOrigCanvas);
  attachDemoListeners(demoCleanCanvas);

  if (demoAudioPlayer) {
    demoAudioPlayer.src = DEMO_CLEAN_URL;
    demoAudioPlayer.addEventListener('play', startDemoPlaybackLoop);
    demoAudioPlayer.addEventListener('pause', () => {
      if (demoAnimFrameId) cancelAnimationFrame(demoAnimFrameId);
      renderDemoWaveforms();
    });
    demoAudioPlayer.addEventListener('ended', () => {
      if (demoAnimFrameId) cancelAnimationFrame(demoAnimFrameId);
      renderDemoWaveforms();
    });
    demoAudioPlayer.addEventListener('seeked', renderDemoWaveforms);
    demoAudioPlayer.addEventListener('timeupdate', renderDemoWaveforms);
  }

  function setDemoMode(mode) {
    if (demoMode === mode || !demoAudioPlayer) return;
    demoMode = mode;
    const isPlaying = !demoAudioPlayer.paused;
    const curTime = demoAudioPlayer.currentTime;

    if (mode === 'clean') {
      demoAudioPlayer.src = DEMO_CLEAN_URL;
      if (btnDemoClean) btnDemoClean.classList.add('active');
      if (btnDemoOrig) btnDemoOrig.classList.remove('active', 'orig');
    } else {
      demoAudioPlayer.src = DEMO_ORIG_URL;
      if (btnDemoOrig) btnDemoOrig.classList.add('active', 'orig');
      if (btnDemoClean) btnDemoClean.classList.remove('active');
    }

    demoAudioPlayer.currentTime = curTime;
    if (isPlaying) demoAudioPlayer.play().catch(() => {});
    renderDemoWaveforms();
  }

  if (btnDemoClean) btnDemoClean.addEventListener('click', () => setDemoMode('clean'));
  if (btnDemoOrig) btnDemoOrig.addEventListener('click', () => setDemoMode('orig'));

  if (demoAccordion) {
    demoAccordion.addEventListener('toggle', () => {
      if (demoAccordion.open) {
        requestAnimationFrame(renderDemoWaveforms);
      }
    });
  }

  // File Upload Handlers
  if (dropZone && audioInput) {
    dropZone.addEventListener('click', () => audioInput.click());

    ['dragenter', 'dragover'].forEach(name => {
      dropZone.addEventListener(name, (e) => { e.preventDefault(); dropZone.classList.add('dragover'); });
    });
    ['dragleave', 'drop'].forEach(name => {
      dropZone.addEventListener(name, (e) => { e.preventDefault(); dropZone.classList.remove('dragover'); });
    });
    dropZone.addEventListener('drop', (e) => {
      if (e.dataTransfer && e.dataTransfer.files.length) handleFile(e.dataTransfer.files[0]);
    });
    audioInput.addEventListener('change', (e) => {
      if (e.target.files && e.target.files.length) handleFile(e.target.files[0]);
    });
  }

  async function handleFile(file) {
    originalFileName = file.name.replace(/\.[^/.]+$/, "");
    if (origWavUrl) URL.revokeObjectURL(origWavUrl);
    origWavUrl = URL.createObjectURL(file);

    if (renameInput) {
      renameInput.value = `${originalFileName}-clean.mp3`;
    }
    if (statusText) statusText.textContent = "Decoding audio...";
    if (actionArea) actionArea.style.display = "block";
    if (resultArea) resultArea.style.display = "none";

    try {
      const arrayBuffer = await file.arrayBuffer();
      const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      loadedAudioBuffer = await audioCtx.decodeAudioData(arrayBuffer);

      const durationSec = loadedAudioBuffer.duration.toFixed(1);
      const infoEl = el('fileInfo');
      if (infoEl) infoEl.textContent = `📁 ${file.name} (${durationSec}s, ${loadedAudioBuffer.sampleRate} Hz)`;
      if (statusText) statusText.textContent = "Ready to clean.";
      if (processBtn) processBtn.disabled = false;
    } catch (err) {
      if (statusText) statusText.textContent = "Error decoding audio file: " + err.message;
    }
  }

  function getProgress() {
    if (!audioPlayer || !audioPlayer.duration) return 0;
    return Math.max(0, Math.min(1, audioPlayer.currentTime / audioPlayer.duration));
  }

  function renderWaveforms() {
    const prog = getProgress();
    const curTime = audioPlayer ? audioPlayer.currentTime : 0;
    const dur = audioPlayer && audioPlayer.duration ? audioPlayer.duration : (loadedAudioBuffer ? loadedAudioBuffer.duration : 0);

    if (cleanerTimeBadge) {
      cleanerTimeBadge.textContent = `⏱️ ${formatTimeExact(curTime)} / ${formatTimeExact(dur)}`;
    }

    if (origCanvas && origBars) {
      drawWaveform(origCanvas, origBars, 'rgba(245, 158, 11, 0.45)', '#f59e0b', prog, 0, 1, cleanerHoverRatio, dur);
    }
    if (cleanCanvas && cleanBars) {
      drawWaveform(cleanCanvas, cleanBars, 'rgba(16, 185, 129, 0.45)', '#10b981', prog, 0, 1, cleanerHoverRatio, dur);
    }
  }

  function startPlaybackLoop() {
    if (animFrameId) cancelAnimationFrame(animFrameId);
    function loop() {
      renderWaveforms();
      if (!audioPlayer.paused && !audioPlayer.ended) {
        animFrameId = requestAnimationFrame(loop);
      }
    }
    animFrameId = requestAnimationFrame(loop);
  }

  function attachCleanerSeek(canvas) {
    if (!canvas) return;
    canvas.addEventListener('click', (e) => {
      if (!audioPlayer || !audioPlayer.duration) return;
      const rect = canvas.getBoundingClientRect();
      const ratio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
      audioPlayer.currentTime = ratio * audioPlayer.duration;
      renderWaveforms();
    });
    canvas.addEventListener('mousemove', (e) => {
      const rect = canvas.getBoundingClientRect();
      cleanerHoverRatio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
      renderWaveforms();
    });
    canvas.addEventListener('mouseleave', () => {
      cleanerHoverRatio = -1;
      renderWaveforms();
    });
  }

  attachCleanerSeek(origCanvas);
  attachCleanerSeek(cleanCanvas);

  if (audioPlayer) {
    audioPlayer.addEventListener('play', startPlaybackLoop);
    audioPlayer.addEventListener('pause', () => {
      if (animFrameId) cancelAnimationFrame(animFrameId);
      renderWaveforms();
    });
    audioPlayer.addEventListener('ended', () => {
      if (animFrameId) cancelAnimationFrame(animFrameId);
      renderWaveforms();
    });
    audioPlayer.addEventListener('seeked', renderWaveforms);
    audioPlayer.addEventListener('timeupdate', renderWaveforms);
  }

  function setMode(mode) {
    if (currentMode === mode || !audioPlayer) return;
    currentMode = mode;
    const isPlaying = !audioPlayer.paused;
    const curTime = audioPlayer.currentTime;

    if (mode === 'clean') {
      audioPlayer.src = cleanWavUrl;
      if (btnPlayClean) btnPlayClean.classList.add('active');
      if (btnPlayOrig) btnPlayOrig.classList.remove('active', 'orig');
    } else {
      audioPlayer.src = origWavUrl;
      if (btnPlayOrig) btnPlayOrig.classList.add('active', 'orig');
      if (btnPlayClean) btnPlayClean.classList.remove('active');
    }

    audioPlayer.currentTime = curTime;
    if (isPlaying) audioPlayer.play().catch(() => {});
    renderWaveforms();
  }

  if (btnPlayClean) btnPlayClean.addEventListener('click', () => setMode('clean'));
  if (btnPlayOrig) btnPlayOrig.addEventListener('click', () => setMode('orig'));

  if (processBtn) {
    processBtn.addEventListener('click', () => {
      if (!loadedAudioBuffer) return;
      processBtn.disabled = true;
      if (statusText) statusText.textContent = `Cleaning background noise in ${currentTargetMode.toUpperCase()} mode...`;

      setTimeout(() => {
        try {
          const numChannels = loadedAudioBuffer.numberOfChannels;
          const length = loadedAudioBuffer.length;
          const mono = new Float32Array(length);
          if (numChannels >= 2) {
            const ch0 = loadedAudioBuffer.getChannelData(0);
            const ch1 = loadedAudioBuffer.getChannelData(1);
            for (let i = 0; i < length; i++) mono[i] = (ch0[i] + ch1[i]) * 0.5;
          } else {
            mono.set(loadedAudioBuffer.getChannelData(0));
          }

          cleanSampleRate = loadedAudioBuffer.sampleRate;
          cleanSamplesCache = processAudioDSP(mono, cleanSampleRate, currentTargetMode);
          const wavBlob = encodeWav(cleanSamplesCache, cleanSampleRate);

          if (cleanWavUrl) URL.revokeObjectURL(cleanWavUrl);
          cleanWavUrl = URL.createObjectURL(wavBlob);

          origBars = computeWaveformBars(mono, 200);
          cleanBars = computeWaveformBars(cleanSamplesCache, 200);

          const origDb = estimateNoiseFloorDb(mono, cleanSampleRate);
          const cleanDb = estimateNoiseFloorDb(cleanSamplesCache, cleanSampleRate);

          if (origDbStat) origDbStat.textContent = `Floor: ~${Math.round(origDb)} dBFS`;
          if (cleanDbStat) cleanDbStat.textContent = `Floor: ~${Math.round(cleanDb)} dBFS (Silent -${Math.max(1, Math.round(origDb - cleanDb))} dB)`;

          currentMode = 'clean';
          if (btnPlayClean) btnPlayClean.classList.add('active');
          if (btnPlayOrig) btnPlayOrig.classList.remove('active', 'orig');

          if (audioPlayer) audioPlayer.src = cleanWavUrl;
          if (statusText) statusText.textContent = `Audio cleaned in ${currentTargetMode === 'vocal' ? 'Vocal' : 'Guitar'} mode! 🎉`;
          if (resultArea) resultArea.style.display = "block";

          requestAnimationFrame(renderWaveforms);
        } catch (err) {
          if (statusText) statusText.textContent = "Processing error: " + err.message;
        } finally {
          processBtn.disabled = false;
        }
      }, 50);
    });
  }

  // Cleaner Export Functions (MP3 & WAV)
  function downloadBlob(blob, filename) {
    const a = document.createElement('a');
    a.href = URL.createObjectURL(blob);
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    setTimeout(() => {
      document.body.removeChild(a);
      URL.revokeObjectURL(a.href);
    }, 1000);
  }

  if (btnDownloadWav) {
    btnDownloadWav.addEventListener('click', () => {
      if (!cleanSamplesCache) return;
      const wavBlob = encodeWav(cleanSamplesCache, cleanSampleRate);
      const baseName = (renameInput && renameInput.value.trim().replace(/\.[^/.]+$/, "")) || originalFileName;
      downloadBlob(wavBlob, `${baseName}.wav`);
    });
  }

  if (btnDownloadMp3) {
    btnDownloadMp3.addEventListener('click', async () => {
      if (!cleanSamplesCache) return;
      if (exportStatus) exportStatus.textContent = "Encoding compressed MP3 for WhatsApp... (~1s)";
      btnDownloadMp3.disabled = true;
      try {
        const mp3Blob = await encodeMp3(cleanSamplesCache, cleanSamplesCache, cleanSampleRate, 192);
        const baseName = (renameInput && renameInput.value.trim().replace(/\.[^/.]+$/, "")) || originalFileName;
        downloadBlob(mp3Blob, `${baseName}.mp3`);
        const mbSize = (mp3Blob.size / (1024 * 1024)).toFixed(1);
        if (exportStatus) exportStatus.textContent = `MP3 Ready! (${mbSize} MB - Perfect for WhatsApp) 🎉`;
      } catch (err) {
        if (exportStatus) exportStatus.textContent = "MP3 encoding failed: " + err.message;
      } finally {
        btnDownloadMp3.disabled = false;
      }
    });
  }

  // Send Cleaned Audio Directly to Mixer
  if (btnSendToMixer) {
    btnSendToMixer.addEventListener('click', () => {
      if (!cleanSamplesCache) return;
      const baseName = (renameInput && renameInput.value.trim().replace(/\.[^/.]+$/, "")) || originalFileName;
      const trackName = `${currentTargetMode === 'vocal' ? '🎙️ Vocal' : '🎸 Guitar'}: ${baseName}`;

      addTrackToMixerFromData(cleanSamplesCache, cleanSampleRate, trackName);
      switchTab('mixer');
    });
  }

  // ================= 5-TRACK MIXER ENGINE =================
  const MAX_TRACKS = 5;
  const mixerTracks = []; // { id, name, buffer, volume, pan, muted, solo, trimStart, trimEnd, offset, bars, hoverRatio }
  let nextTrackId = 1;
  let mixerAudioCtx = null;
  let activeSources = [];
  let isMixerPlaying = false;
  let mixerStartTime = 0;
  let mixerPauseOffset = 0;
  let mixerAnimId = null;

  const mixerTracksList = el('mixerTracksList');
  const mixerEmptyNotice = el('mixerEmptyNotice');
  const btnMixerAddTrack = el('btnMixerAddTrack');
  const mixerFileInput = el('mixerFileInput');
  const btnMixerPlay = el('btnMixerPlay');
  const btnMixerStop = el('btnMixerStop');
  const mixerTimecode = el('mixerTimecode');
  const mixerMasterVolume = el('mixerMasterVolume');
  const mixerMasterVolVal = el('mixerMasterVolVal');
  const btnExportMixMp3 = el('btnExportMixMp3');
  const btnExportMixWav = el('btnExportMixWav');
  const mixerExportStatus = el('mixerExportStatus');

  function updateTrackBadge() {
    if (trackCountBadge) trackCountBadge.textContent = `${mixerTracks.length}/${MAX_TRACKS}`;
    if (btnMixerAddTrack) btnMixerAddTrack.style.display = mixerTracks.length >= MAX_TRACKS ? 'none' : 'inline-flex';
    if (mixerEmptyNotice) mixerEmptyNotice.style.display = mixerTracks.length === 0 ? 'block' : 'none';
  }

  function getMixerTotalDuration() {
    let maxEnd = 0;
    mixerTracks.forEach(t => {
      const dur = Math.max(0, t.trimEnd - t.trimStart);
      const trackEnd = t.offset + dur;
      if (trackEnd > maxEnd) maxEnd = trackEnd;
    });
    return maxEnd;
  }

  function addTrackToMixerFromData(monoSamples, sampleRate, name) {
    if (mixerTracks.length >= MAX_TRACKS) {
      alert("Mixer limit reached: Maximum 5 tracks supported.");
      return;
    }
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const buffer = audioCtx.createBuffer(1, monoSamples.length, sampleRate);
    buffer.getChannelData(0).set(monoSamples);

    const dur = buffer.duration;
    const bars = computeWaveformBars(monoSamples, 180);

    const track = {
      id: nextTrackId++,
      name: name || `Track ${mixerTracks.length + 1}`,
      buffer,
      volume: 1.0,
      pan: 0.0,
      muted: false,
      solo: false,
      trimStart: 0,
      trimEnd: dur,
      offset: 0,
      bars,
      hoverRatio: -1
    };
    mixerTracks.push(track);
    updateTrackBadge();
    renderAllMixerTracks();
  }

  if (btnMixerAddTrack && mixerFileInput) {
    btnMixerAddTrack.addEventListener('click', () => mixerFileInput.click());
    mixerFileInput.addEventListener('change', async (e) => {
      if (e.target.files && e.target.files.length) {
        const file = e.target.files[0];
        try {
          const arrayBuffer = await file.arrayBuffer();
          const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
          const decoded = await audioCtx.decodeAudioData(arrayBuffer);
          const mono = new Float32Array(decoded.length);
          if (decoded.numberOfChannels >= 2) {
            const ch0 = decoded.getChannelData(0);
            const ch1 = decoded.getChannelData(1);
            for (let i = 0; i < decoded.length; i++) mono[i] = (ch0[i] + ch1[i]) * 0.5;
          } else {
            mono.set(decoded.getChannelData(0));
          }
          const base = file.name.replace(/\.[^/.]+$/, "");
          addTrackToMixerFromData(mono, decoded.sampleRate, base);
        } catch (err) {
          alert("Error loading track audio: " + err.message);
        }
      }
      mixerFileInput.value = "";
    });
  }

  // Live Playback Engine
  function stopMixerPlayback() {
    activeSources.forEach(s => {
      try { s.stop(); s.disconnect(); } catch (e) {}
    });
    activeSources = [];
    isMixerPlaying = false;
    mixerPauseOffset = 0;
    if (mixerAnimId) cancelAnimationFrame(mixerAnimId);
    if (btnMixerPlay) btnMixerPlay.textContent = "▶ Play Mix";
    updateMixerPlayhead(0);
  }

  function pauseMixerPlayback() {
    if (!isMixerPlaying) return;
    const elapsed = mixerAudioCtx ? (mixerAudioCtx.currentTime - mixerStartTime) : 0;
    mixerPauseOffset += elapsed;
    activeSources.forEach(s => {
      try { s.stop(); s.disconnect(); } catch (e) {}
    });
    activeSources = [];
    isMixerPlaying = false;
    if (mixerAnimId) cancelAnimationFrame(mixerAnimId);
    if (btnMixerPlay) btnMixerPlay.textContent = "▶ Resume Mix";
  }

  function startMixerPlayback() {
    const totalDur = getMixerTotalDuration();
    if (totalDur <= 0 || mixerTracks.length === 0) return;

    if (mixerPauseOffset >= totalDur) {
      mixerPauseOffset = 0;
    }

    mixerAudioCtx = new (window.AudioContext || window.webkitAudioContext)();
    mixerStartTime = mixerAudioCtx.currentTime;
    isMixerPlaying = true;
    if (btnMixerPlay) btnMixerPlay.textContent = "⏸ Pause";

    const hasSolo = mixerTracks.some(t => t.solo);
    const masterVol = parseFloat(mixerMasterVolume ? mixerMasterVolume.value : 1.0);

    const masterGainNode = mixerAudioCtx.createGain();
    masterGainNode.gain.value = masterVol;
    masterGainNode.connect(mixerAudioCtx.destination);

    activeSources = [];

    mixerTracks.forEach(track => {
      const isAudible = hasSolo ? track.solo : !track.muted;
      if (!isAudible) return;

      const dur = track.trimEnd - track.trimStart;
      if (dur <= 0) return;

      const trackStart = track.offset;
      const trackEnd = track.offset + dur;

      if (trackEnd <= mixerPauseOffset) return;

      let startDelayInCtx = 0;
      let offsetInSnippet = 0;

      if (mixerPauseOffset < trackStart) {
        startDelayInCtx = trackStart - mixerPauseOffset;
        offsetInSnippet = 0;
      } else {
        startDelayInCtx = 0;
        offsetInSnippet = mixerPauseOffset - trackStart;
      }

      const durationToPlay = dur - offsetInSnippet;
      if (durationToPlay <= 0) return;

      const src = mixerAudioCtx.createBufferSource();
      src.buffer = track.buffer;

      const gain = mixerAudioCtx.createGain();
      gain.gain.value = track.volume;

      const panner = mixerAudioCtx.createStereoPanner ? mixerAudioCtx.createStereoPanner() : null;
      if (panner) panner.pan.value = track.pan;

      if (panner) {
        src.connect(gain).connect(panner).connect(masterGainNode);
      } else {
        src.connect(gain).connect(masterGainNode);
      }

      const when = mixerStartTime + startDelayInCtx;
      const grainOffset = track.trimStart + offsetInSnippet;
      src.start(when, grainOffset, durationToPlay);
      activeSources.push(src);
    });

    function loop() {
      if (!isMixerPlaying) return;
      const elapsed = mixerAudioCtx ? (mixerAudioCtx.currentTime - mixerStartTime) : 0;
      const curTime = mixerPauseOffset + elapsed;
      updateMixerPlayhead(curTime);

      if (curTime >= totalDur) {
        stopMixerPlayback();
      } else {
        mixerAnimId = requestAnimationFrame(loop);
      }
    }
    mixerAnimId = requestAnimationFrame(loop);
  }

  if (btnMixerPlay) {
    btnMixerPlay.addEventListener('click', () => {
      if (isMixerPlaying) pauseMixerPlayback();
      else startMixerPlayback();
    });
  }
  if (btnMixerStop) btnMixerStop.addEventListener('click', stopMixerPlayback);

  if (mixerMasterVolume) {
    mixerMasterVolume.addEventListener('input', () => {
      if (mixerMasterVolVal) mixerMasterVolVal.textContent = `${Math.round(mixerMasterVolume.value * 100)}%`;
    });
  }

  function updateMixerPlayhead(curTime) {
    const totalDur = getMixerTotalDuration();
    if (mixerTimecode) mixerTimecode.textContent = `${formatTimeExact(curTime)} / ${formatTimeExact(totalDur)}`;

    mixerTracks.forEach(track => {
      const canvas = document.getElementById(`trackCanvas_${track.id}`);
      const timeBadge = document.getElementById(`trackTimeBadge_${track.id}`);
      if (!canvas) return;

      const dur = track.buffer.duration;
      const trimStartRatio = track.trimStart / dur;
      const trimEndRatio = track.trimEnd / dur;

      let trackLocalTime = curTime - track.offset;
      let prog = (track.trimStart + trackLocalTime) / dur;
      let displayTime = 0;

      if (curTime < track.offset) {
        prog = -1;
        displayTime = 0;
      } else if (trackLocalTime > (track.trimEnd - track.trimStart)) {
        prog = -1;
        displayTime = track.trimEnd - track.trimStart;
      } else {
        displayTime = trackLocalTime;
      }

      if (timeBadge) {
        const snippetDur = track.trimEnd - track.trimStart;
        timeBadge.textContent = `⏱️ ${formatTimeExact(displayTime)} / ${formatTimeExact(snippetDur)}`;
      }

      drawWaveform(canvas, track.bars, 'rgba(16, 185, 129, 0.45)', '#10b981', prog, trimStartRatio, trimEndRatio, track.hoverRatio, dur);
    });
  }

  // Render Track List UI
  function renderAllMixerTracks() {
    if (!mixerTracksList) return;
    const cards = mixerTracksList.querySelectorAll('.mixer-track-card');
    cards.forEach(c => c.remove());

    mixerTracks.forEach((track) => {
      const card = document.createElement('div');
      card.className = 'mixer-track-card';
      card.id = `trackCard_${track.id}`;

      const totalDur = track.buffer.duration;
      const snippetDur = track.trimEnd - track.trimStart;
      const isMuted = track.muted;
      const isSolo = track.solo;

      card.innerHTML = `
        <div class="mixer-track-header">
          <div style="display: flex; align-items: center; gap: 0.5rem; flex-wrap: wrap;">
            <span style="font-size: 1.1rem;">🎵</span>
            <input type="text" class="mixer-track-name-input" id="nameInput_${track.id}" value="${track.name}" title="Click to rename" />
            <span class="guitar-time-badge" style="font-size: 0.75rem; padding: 0.2rem 0.5rem;" id="trackTimeBadge_${track.id}">⏱️ 0:00.0 / ${formatTimeExact(snippetDur)}</span>
          </div>
          <div style="display: flex; align-items: center; gap: 0.4rem;">
            <button type="button" class="mixer-btn-tag ${isMuted ? 'active-mute' : ''}" id="muteBtn_${track.id}">M</button>
            <button type="button" class="mixer-btn-tag ${isSolo ? 'active-solo' : ''}" id="soloBtn_${track.id}">S</button>
            <button type="button" class="mixer-btn-tag" id="dupBtn_${track.id}" title="Duplicate / Copy snippet to another track slot">📋 Duplicate</button>
            <button type="button" class="mixer-btn-tag" id="delBtn_${track.id}" style="color: #ef4444;" title="Remove track">🗑️</button>
          </div>
        </div>

        <canvas class="mixer-canvas" id="trackCanvas_${track.id}"></canvas>

        <div class="mixer-controls-grid">
          <div class="mixer-control-group">
            <label>Start Delay (s): <span id="offsetTime_${track.id}" style="color: #38bdf8; font-family: monospace;">(${formatTimeExact(track.offset)})</span></label>
            <input type="number" id="offsetInput_${track.id}" min="0" max="600" step="0.1" value="${track.offset.toFixed(1)}" />
          </div>
          <div class="mixer-control-group">
            <label>Trim In (s): <span id="trimInTime_${track.id}" style="color: #34d399; font-family: monospace;">(${formatTimeExact(track.trimStart)})</span></label>
            <input type="number" id="trimStartInput_${track.id}" min="0" max="${totalDur.toFixed(1)}" step="0.1" value="${track.trimStart.toFixed(1)}" />
          </div>
          <div class="mixer-control-group">
            <label>Trim Out (s): <span id="trimOutTime_${track.id}" style="color: #34d399; font-family: monospace;">(${formatTimeExact(track.trimEnd)})</span></label>
            <input type="number" id="trimEndInput_${track.id}" min="0" max="${totalDur.toFixed(1)}" step="0.1" value="${track.trimEnd.toFixed(1)}" />
          </div>
          <div class="mixer-control-group">
            <label>Volume: <span id="volVal_${track.id}">${Math.round(track.volume * 100)}%</span></label>
            <input type="range" id="volSlider_${track.id}" min="0" max="1.5" step="0.05" value="${track.volume}" />
          </div>
          <div class="mixer-control-group">
            <label>Pan: <span id="panVal_${track.id}">${track.pan === 0 ? 'C' : (track.pan < 0 ? 'L' + Math.abs(track.pan).toFixed(1) : 'R' + track.pan.toFixed(1))}</span></label>
            <input type="range" id="panSlider_${track.id}" min="-1" max="1" step="0.1" value="${track.pan}" />
          </div>
          <div class="mixer-control-group" style="justify-content: flex-end;">
            <button type="button" class="mixer-btn-tag" id="cropBtn_${track.id}" style="background: #1e3a8a; color: #93c5fd; border-color: #3b82f6;">✂️ Crop Snippet</button>
          </div>
        </div>
      `;

      mixerTracksList.appendChild(card);

      // Event Listeners for Track Controls
      const nameIn = card.querySelector(`#nameInput_${track.id}`);
      nameIn.addEventListener('input', (e) => { track.name = e.target.value; });

      const muteBtn = card.querySelector(`#muteBtn_${track.id}`);
      muteBtn.addEventListener('click', () => {
        track.muted = !track.muted;
        muteBtn.classList.toggle('active-mute', track.muted);
      });

      const soloBtn = card.querySelector(`#soloBtn_${track.id}`);
      soloBtn.addEventListener('click', () => {
        track.solo = !track.solo;
        soloBtn.classList.toggle('active-solo', track.solo);
      });

      const delBtn = card.querySelector(`#delBtn_${track.id}`);
      delBtn.addEventListener('click', () => {
        const idx = mixerTracks.findIndex(t => t.id === track.id);
        if (idx !== -1) {
          mixerTracks.splice(idx, 1);
          updateTrackBadge();
          renderAllMixerTracks();
        }
      });

      const dupBtn = card.querySelector(`#dupBtn_${track.id}`);
      dupBtn.addEventListener('click', () => {
        if (mixerTracks.length >= MAX_TRACKS) {
          alert("Maximum 5 tracks reached. Delete a track to duplicate.");
          return;
        }
        const clone = {
          id: nextTrackId++,
          name: `${track.name} (Copy)`,
          buffer: track.buffer,
          volume: track.volume,
          pan: track.pan,
          muted: false,
          solo: false,
          trimStart: track.trimStart,
          trimEnd: track.trimEnd,
          offset: track.offset + (track.trimEnd - track.trimStart),
          bars: track.bars,
          hoverRatio: -1
        };
        mixerTracks.push(clone);
        updateTrackBadge();
        renderAllMixerTracks();
      });

      const offsetIn = card.querySelector(`#offsetInput_${track.id}`);
      const offsetTime = card.querySelector(`#offsetTime_${track.id}`);
      offsetIn.addEventListener('input', (e) => {
        track.offset = Math.max(0, parseFloat(e.target.value) || 0);
        if (offsetTime) offsetTime.textContent = `(${formatTimeExact(track.offset)})`;
        updateMixerPlayhead(0);
      });

      const trimStartIn = card.querySelector(`#trimStartInput_${track.id}`);
      const trimEndIn = card.querySelector(`#trimEndInput_${track.id}`);
      const trimInTime = card.querySelector(`#trimInTime_${track.id}`);
      const trimOutTime = card.querySelector(`#trimOutTime_${track.id}`);
      const timeBadge = card.querySelector(`#trackTimeBadge_${track.id}`);

      trimStartIn.addEventListener('input', (e) => {
        track.trimStart = Math.max(0, Math.min(track.trimEnd - 0.1, parseFloat(e.target.value) || 0));
        if (trimInTime) trimInTime.textContent = `(${formatTimeExact(track.trimStart)})`;
        if (timeBadge) timeBadge.textContent = `⏱️ 0:00.0 / ${formatTimeExact(track.trimEnd - track.trimStart)}`;
        renderTrackWaveform(track);
      });
      trimEndIn.addEventListener('input', (e) => {
        track.trimEnd = Math.max(track.trimStart + 0.1, Math.min(track.buffer.duration, parseFloat(e.target.value) || track.buffer.duration));
        if (trimOutTime) trimOutTime.textContent = `(${formatTimeExact(track.trimEnd)})`;
        if (timeBadge) timeBadge.textContent = `⏱️ 0:00.0 / ${formatTimeExact(track.trimEnd - track.trimStart)}`;
        renderTrackWaveform(track);
      });

      const volSlider = card.querySelector(`#volSlider_${track.id}`);
      const volVal = card.querySelector(`#volVal_${track.id}`);
      volSlider.addEventListener('input', (e) => {
        track.volume = parseFloat(e.target.value);
        volVal.textContent = `${Math.round(track.volume * 100)}%`;
      });

      const panSlider = card.querySelector(`#panSlider_${track.id}`);
      const panVal = card.querySelector(`#panVal_${track.id}`);
      panSlider.addEventListener('input', (e) => {
        track.pan = parseFloat(e.target.value);
        panVal.textContent = track.pan === 0 ? 'C' : (track.pan < 0 ? 'L' + Math.abs(track.pan).toFixed(1) : 'R' + track.pan.toFixed(1));
      });

      const trackCanvas = card.querySelector(`#trackCanvas_${track.id}`);
      trackCanvas.addEventListener('mousemove', (e) => {
        const rect = trackCanvas.getBoundingClientRect();
        track.hoverRatio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
        renderTrackWaveform(track);
      });
      trackCanvas.addEventListener('mouseleave', () => {
        track.hoverRatio = -1;
        renderTrackWaveform(track);
      });
      trackCanvas.addEventListener('click', (e) => {
        const rect = trackCanvas.getBoundingClientRect();
        const ratio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
        const clickedTimeInTrack = ratio * track.buffer.duration;
        const targetMixerTime = track.offset + Math.max(0, clickedTimeInTrack - track.trimStart);

        if (isMixerPlaying) {
          pauseMixerPlayback();
          mixerPauseOffset = targetMixerTime;
          startMixerPlayback();
        } else {
          mixerPauseOffset = targetMixerTime;
          updateMixerPlayhead(targetMixerTime);
        }
      });

      // Crop snippet permanently
      const cropBtn = card.querySelector(`#cropBtn_${track.id}`);
      cropBtn.addEventListener('click', () => {
        const startSec = track.trimStart;
        const endSec = track.trimEnd;
        const dur = endSec - startSec;
        if (dur <= 0.1) return;

        const sr = track.buffer.sampleRate;
        const startSample = Math.floor(startSec * sr);
        const endSample = Math.min(track.buffer.length, Math.floor(endSec * sr));
        const numSamples = endSample - startSample;

        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        const croppedBuffer = audioCtx.createBuffer(1, numSamples, sr);
        const monoData = track.buffer.getChannelData(0).subarray(startSample, endSample);
        croppedBuffer.getChannelData(0).set(monoData);

        track.buffer = croppedBuffer;
        track.trimStart = 0;
        track.trimEnd = dur;
        track.bars = computeWaveformBars(monoData, 180);
        renderAllMixerTracks();
      });

      renderTrackWaveform(track);
    });

    updateTrackBadge();
    updateMixerPlayhead(0);
  }

  function renderTrackWaveform(track) {
    const canvas = document.getElementById(`trackCanvas_${track.id}`);
    if (!canvas) return;
    const dur = track.buffer.duration;
    const trimStartRatio = track.trimStart / dur;
    const trimEndRatio = track.trimEnd / dur;
    drawWaveform(canvas, track.bars, 'rgba(16, 185, 129, 0.45)', '#10b981', -1, trimStartRatio, trimEndRatio, track.hoverRatio, dur);
  }

  // Offline Audio Context Multi-Track Rendering
  async function renderMixToBuffer() {
    const totalDur = getMixerTotalDuration();
    if (totalDur <= 0.1 || mixerTracks.length === 0) {
      throw new Error("No tracks to export. Add at least one track to the mixer.");
    }

    const sampleRate = 44100;
    const totalSamples = Math.ceil(totalDur * sampleRate);
    const offlineCtx = new OfflineAudioContext(2, totalSamples, sampleRate);

    const hasSolo = mixerTracks.some(t => t.solo);
    const masterVol = parseFloat(mixerMasterVolume ? mixerMasterVolume.value : 1.0);

    const masterGainNode = offlineCtx.createGain();
    masterGainNode.gain.value = masterVol;
    masterGainNode.connect(offlineCtx.destination);

    mixerTracks.forEach(track => {
      const isAudible = hasSolo ? track.solo : !track.muted;
      if (!isAudible) return;

      const dur = track.trimEnd - track.trimStart;
      if (dur <= 0) return;

      const src = offlineCtx.createBufferSource();
      src.buffer = track.buffer;

      const gain = offlineCtx.createGain();
      gain.gain.value = track.volume;

      const panner = offlineCtx.createStereoPanner ? offlineCtx.createStereoPanner() : null;
      if (panner) panner.pan.value = track.pan;

      if (panner) {
        src.connect(gain).connect(panner).connect(masterGainNode);
      } else {
        src.connect(gain).connect(masterGainNode);
      }

      src.start(track.offset, track.trimStart, dur);
    });

    return await offlineCtx.startRendering();
  }

  // Master Export: MP3 & WAV
  if (btnExportMixMp3) {
    btnExportMixMp3.addEventListener('click', async () => {
      try {
        if (mixerExportStatus) mixerExportStatus.textContent = "Rendering multi-track mix and encoding MP3 for WhatsApp... (~2s)";
        btnExportMixMp3.disabled = true;

        const renderedBuffer = await renderMixToBuffer();
        const leftChannel = renderedBuffer.getChannelData(0);
        const rightChannel = renderedBuffer.getChannelData(1);

        const mp3Blob = await encodeMp3(leftChannel, rightChannel, renderedBuffer.sampleRate, 192);
        downloadBlob(mp3Blob, "mixed-song.mp3");

        const mbSize = (mp3Blob.size / (1024 * 1024)).toFixed(1);
        if (mixerExportStatus) mixerExportStatus.textContent = `Mix Exported! (${mbSize} MB MP3 - Ready to send on WhatsApp) 🎉`;
      } catch (err) {
        if (mixerExportStatus) mixerExportStatus.textContent = "Mix export error: " + err.message;
      } finally {
        btnExportMixMp3.disabled = false;
      }
    });
  }

  if (btnExportMixWav) {
    btnExportMixWav.addEventListener('click', async () => {
      try {
        if (mixerExportStatus) mixerExportStatus.textContent = "Rendering master mix WAV...";
        btnExportMixWav.disabled = true;

        const renderedBuffer = await renderMixToBuffer();
        const left = renderedBuffer.getChannelData(0);
        const right = renderedBuffer.getChannelData(1);
        const interleaved = new Float32Array(left.length);
        for (let i = 0; i < left.length; i++) interleaved[i] = (left[i] + right[i]) * 0.5;

        const wavBlob = encodeWav(interleaved, renderedBuffer.sampleRate);
        downloadBlob(wavBlob, "mixed-song.wav");
        if (mixerExportStatus) mixerExportStatus.textContent = "Mix exported as Lossless WAV! 🎉";
      } catch (err) {
        if (mixerExportStatus) mixerExportStatus.textContent = "WAV export error: " + err.message;
      } finally {
        btnExportMixWav.disabled = false;
      }
    });
  }

  window.addEventListener('resize', () => {
    renderWaveforms();
    if (demoAccordion && demoAccordion.open) renderDemoWaveforms();
    if (mixerTracks.length > 0) {
      mixerTracks.forEach(t => renderTrackWaveform(t));
    }
  });
}

if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', initGuitarStudio);
} else {
  initGuitarStudio();
}
})();
</script>
