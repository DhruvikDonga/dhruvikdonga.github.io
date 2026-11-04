+++
title = "Guitar Denoise Studio"
description = "Browser-based client-side acoustic guitar noise remover and audio enhancer"
date = "2026-11-04"
aliases = ["guitar-cleaner", "guitar-studio", "denoise", "pedal"]
author = "Dhruvik Donga"
+++

<style>
.guitar-app {
  font-family: inherit;
  max-width: 680px;
  margin: 1.5rem auto;
  padding: 1.6rem;
  border: 1px solid var(--border-color, #2a2d36);
  border-radius: 12px;
  background-color: var(--bg-color, #16171b);
  color: var(--text-color, #e2e8f0);
  box-shadow: 0 12px 36px rgba(0, 0, 0, 0.45);
}
.guitar-dropzone {
  border: 2px dashed #3a3f4d;
  border-radius: 8px;
  padding: 2.2rem 1.5rem;
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
  padding: 0.65rem 1.3rem;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  display: inline-block;
  margin-top: 0.5rem;
  box-shadow: 0 3px 12px rgba(16, 185, 129, 0.25);
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
.guitar-btn-download {
  background: rgba(16, 185, 129, 0.12);
  color: #34d399;
  border: 1px solid rgba(52, 211, 153, 0.4);
  border-radius: 6px;
  padding: 0.65rem 1.25rem;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.2s ease;
  cursor: pointer;
}
.guitar-btn-download:hover {
  background: #10b981;
  color: #ffffff;
  border-color: #10b981;
  box-shadow: 0 4px 14px rgba(16, 185, 129, 0.35);
  transform: translateY(-1px);
}
.guitar-section {
  margin-top: 1.5rem;
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
  font-size: 0.95rem;
  box-sizing: border-box;
  margin-top: 0.4rem;
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
.guitar-viz-container {
  margin-top: 1rem;
}
.guitar-viz-card {
  background: #0d0e11;
  border: 1px solid #23262f;
  border-radius: 6px;
  padding: 0.65rem 0.85rem 0.5rem 0.85rem;
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
  height: 75px;
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
</style>

<div class="guitar-app">
<p style="margin-top: 0; color: #94a3b8; font-size: 0.92rem; line-height: 1.5;">
Zero server upload. Runs 100% locally in your browser. Eliminates room air and mic static while keeping your natural acoustic guitar tone.
</p>

<div class="guitar-dropzone" id="dropZone">
<span style="font-size: 2.4rem;">🎸</span>
<p style="margin: 0.5rem 0 0 0; font-weight: 600; font-size: 1rem;">Click to upload or drag & drop guitar audio</p>
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
<div class="guitar-mode-toggle" id="demoModeToggle">
<button type="button" id="btnDemoClean" class="guitar-toggle-btn active">✨ Cleaned Melody</button>
<button type="button" id="btnDemoOrig" class="guitar-toggle-btn">📻 Raw (With Hiss)</button>
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
<p style="margin: 0.4rem 0 0 0; font-size: 0.78rem; color: #64748b; text-align: right;">💡 Click either waveform to seek demo playback</p>
</div>
</div>
</details>

<div id="actionArea" class="guitar-section" style="display: none;">
<p style="margin: 0 0 0.5rem 0; font-size: 0.9rem;" id="fileInfo"></p>
<label for="renameInput" style="font-size: 0.88rem; font-weight: 600; display: block;">Custom Output Filename:</label>
<input type="text" id="renameInput" class="guitar-input-field" value="cleaned-guitar.wav" placeholder="cleaned-guitar.wav" />
<button id="processBtn" class="guitar-btn" style="margin-top: 1rem;">Clean Audio Now</button>
<div id="statusText" class="guitar-status"></div>
</div>

<div id="resultArea" class="guitar-section" style="display: none;">
<div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 0.6rem;">
<h4 style="margin: 0; font-size: 1.05rem;">🎧 Audio Preview & Comparison</h4>
<div class="guitar-mode-toggle" id="modeToggle">
<button type="button" id="btnPlayClean" class="guitar-toggle-btn active">✨ Cleaned</button>
<button type="button" id="btnPlayOrig" class="guitar-toggle-btn">📻 Original</button>
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
<span class="guitar-badge badge-clean">AFTER</span> Cleaned (Silent Floor & Pure Tone)
</span>
<span id="cleanDbStat" style="font-size: 0.78rem; color: #34d399;"></span>
</div>
<canvas id="cleanCanvas" class="guitar-canvas"></canvas>
</div>
<p style="margin: 0.4rem 0 0 0; font-size: 0.78rem; color: #64748b; text-align: right;">💡 Click either waveform to jump playback</p>
</div>
<div style="margin-top: 1.2rem; display: flex; gap: 1rem; align-items: center; flex-wrap: wrap;">
<a id="downloadLink" class="guitar-btn-download" download="cleaned-guitar.wav">⬇️ Download Clean WAV</a>
<span id="outputStats" style="font-size: 0.85rem; color: #94a3b8;"></span>
</div>
</div>
</div>

<script>
(function() {
'use strict';

// Precomputed waveform bars for the demo melody
const DEMO_RAW_BARS = [0.015, 0.028, 0.336, 0.279, 0.325, 0.33, 0.243, 0.14, 0.128, 0.092, 0.178, 0.1, 0.212, 0.118, 0.071, 0.033, 0.12, 0.15, 0.091, 0.064, 0.131, 0.1, 0.108, 0.1, 0.106, 0.163, 0.097, 0.224, 0.155, 0.094, 0.4, 0.297, 0.144, 0.079, 0.106, 0.076, 0.039, 0.155, 0.15, 0.198, 0.176, 0.118, 0.027, 0.082, 0.046, 0.074, 0.074, 0.114, 0.159, 0.163, 0.151, 0.104, 0.079, 0.175, 0.125, 0.143, 0.321, 0.164, 0.252, 0.214, 0.273, 0.118, 0.091, 0.24, 0.115, 0.08, 0.088, 0.036, 0.102, 0.06, 0.148, 0.062, 0.062, 0.07, 0.126, 0.024, 0.029, 0.108, 0.05, 0.04, 0.175, 0.138, 0.124, 0.128, 0.071, 0.087, 0.116, 0.153, 0.19, 0.294, 0.302, 0.126, 0.155, 0.364, 0.23, 0.231, 0.041, 0.166, 0.1, 0.077, 0.135, 0.163, 0.068, 0.141, 0.103, 0.077, 0.071, 0.049, 0.034, 0.104, 0.09, 0.075, 0.083, 0.056, 0.04, 0.023, 0.203, 0.128, 0.034, 0.129, 0.14, 0.245, 0.169, 0.117, 0.236, 0.278, 0.154, 0.369, 0.212, 0.088, 0.162, 0.126, 0.091, 0.127, 0.076, 0.052, 0.142, 0.096, 0.045, 0.155, 0.093, 0.05, 0.038, 0.089, 0.075, 0.107, 0.252, 0.126, 0.036, 0.075, 0.137, 0.097, 0.232, 0.159, 0.188, 0.34, 0.27, 0.195, 0.194, 0.254, 0.124, 0.241, 0.154, 0.249, 0.06, 0.099, 0.116, 0.063, 0.108, 0.072, 0.072, 0.054, 0.031, 0.111, 0.113, 0.017, 0.087, 0.081, 0.165, 0.098, 0.037, 0.044, 0.144, 0.185, 0.178, 0.057, 0.098, 0.231, 0.289, 0.1, 0.218, 0.179, 0.233, 0.204, 0.162, 0.118, 0.064, 0.159, 0.039, 0.033];
const DEMO_CLEAN_BARS = [0.0, 0.133, 0.089, 0.74, 0.664, 0.72, 0.464, 0.72, 0.574, 0.648, 0.355, 0.435, 0.585, 0.487, 0.449, 0.414, 0.758, 0.478, 0.387, 0.706, 0.552, 0.675, 0.577, 0.642, 0.383, 0.314, 0.635, 0.393, 0.319, 0.331, 0.335, 0.352, 0.474, 0.474, 0.771, 0.364, 0.395, 0.649, 0.577, 0.534, 0.354, 0.33, 0.382, 0.503, 0.58, 0.538, 0.492, 0.616, 0.556, 0.719, 0.669, 0.367, 0.452, 0.509, 0.43, 0.476, 0.308, 0.313, 0.623, 0.713, 0.667, 0.623, 0.725, 0.586, 0.48, 0.538, 0.433, 0.767, 0.712, 0.706, 0.506, 0.653, 0.517, 0.7, 0.453, 0.763, 0.689, 0.665, 0.415, 0.498, 0.734, 0.581, 0.737, 0.609, 0.786, 0.743, 0.618, 0.734, 0.649, 0.731, 0.54, 0.627, 0.299, 0.629, 0.68, 0.311, 0.46, 0.411, 0.387, 0.459, 0.61, 0.815, 0.357, 0.719, 0.448, 0.445, 0.527, 0.601, 0.615, 0.273, 0.675, 0.406, 0.517, 0.701, 0.512, 0.64, 0.679, 0.344, 0.451, 0.685, 0.732, 0.548, 0.559, 0.363, 0.355, 0.648, 0.551, 0.768, 0.609, 0.735, 0.457, 0.421, 0.391, 0.793, 0.497, 0.404, 0.312, 0.818, 0.607, 0.512, 0.707, 0.658, 0.519, 0.753, 0.593, 0.592, 0.487, 0.212, 0.707, 0.6, 0.761, 0.508, 0.459, 0.763, 0.536, 0.629, 0.697, 0.519, 0.437, 0.547, 0.589, 0.652, 0.585, 0.537, 0.543, 0.467, 0.779, 0.503, 0.704, 0.723, 0.566, 0.738, 0.577, 0.766, 0.424, 0.322, 0.398, 0.642, 0.394, 0.517, 0.342, 0.598, 0.77, 0.66, 0.718, 0.592, 0.52, 0.594, 0.599, 0.611, 0.555, 0.582, 0.431, 0.502, 0.402, 0.434, 0.264, 0.351, 0.183, 0.445];

// --- Professional In-Browser DSP Engine ---
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

function processGuitarDSP(samples, sampleRate) {
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

  const alphaCurve = new Float32Array(numBins);
  for (let b = 0; b < numBins; b++) {
    const freq = b * binHz;
    if (freq < 800) alphaCurve[b] = 1.8;
    else if (freq < 2000) alphaCurve[b] = 2.6;
    else if (freq < 8000) alphaCurve[b] = 3.6;
    else if (freq < 11000) alphaCurve[b] = 4.2;
    else alphaCurve[b] = 5.5;
  }

  const output = new Float32Array(samples.length + fftLen);
  const normSum = new Float32Array(samples.length + fftLen);
  const prevGains = new Float32Array(numBins).fill(1.0);
  const gammaSmooth = 0.70;
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

  const outLen = samples.length;
  const cleaned = new Float32Array(outLen);
  for (let i = 0; i < outLen; i++) {
    cleaned[i] = normSum[i] > 1e-4 ? output[i] / normSum[i] : output[i];
  }

  // Downward Expander for absolute silence in rests
  let env = 0;
  const attack = Math.exp(-1.0 / (sampleRate * 0.008));
  const release = Math.exp(-1.0 / (sampleRate * 0.040));
  const threshold = 0.006;

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

  // Lookahead Limiter (+4.5 dB boost)
  const lookaheadMs = 4.0;
  const lookaheadSamples = Math.floor(sampleRate * (lookaheadMs / 1000.0));
  const targetGain = 1.68;
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

// Waveform downsampler and drawer
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

function drawWaveform(canvas, bars, mainColor, playedColor, progressRatio) {
  if (!canvas || !bars) return;
  const dpr = window.devicePixelRatio || 1;
  const width = canvas.clientWidth || 600;
  const height = canvas.clientHeight || 75;

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

  for (let i = 0; i < barCount; i++) {
    const x = i * barWidth;
    const peak = bars[i];
    const h = Math.max(1.5, peak * (height * 0.44));

    ctx.fillStyle = (x <= playheadX) ? playedColor : mainColor;
    ctx.fillRect(x, midY - h, bw, h * 2);
  }

  // Playhead line
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
  const dropZone = el('dropZone');
  const audioInput = el('audioInput');
  const actionArea = el('actionArea');
  const resultArea = el('resultArea');
  const processBtn = el('processBtn');
  const statusText = el('statusText');
  const renameInput = el('renameInput');
  const audioPlayer = el('audioPlayer');
  const downloadLink = el('downloadLink');
  const origCanvas = el('origCanvas');
  const cleanCanvas = el('cleanCanvas');
  const btnPlayClean = el('btnPlayClean');
  const btnPlayOrig = el('btnPlayOrig');
  const origDbStat = el('origDbStat');
  const cleanDbStat = el('cleanDbStat');

  // Demo Section Elements
  const demoAccordion = el('demoAccordion');
  const demoAudioPlayer = el('demoAudioPlayer');
  const demoOrigCanvas = el('demoOrigCanvas');
  const demoCleanCanvas = el('demoCleanCanvas');
  const btnDemoClean = el('btnDemoClean');
  const btnDemoOrig = el('btnDemoOrig');

  if (!dropZone || !audioInput) return;

  let loadedAudioBuffer = null;
  let originalFile = null;
  let originalFileName = "guitar-cleaned";
  let origBars = null;
  let cleanBars = null;
  let origWavUrl = null;
  let cleanWavUrl = null;
  let currentMode = 'clean';
  let animFrameId = null;

  // Demo state
  const DEMO_CLEAN_URL = "/audio/guitar_natural_pure.mp3";
  const DEMO_ORIG_URL = "/audio/guitar_raw.mp3";
  let demoMode = 'clean';
  let demoAnimFrameId = null;

  // --- Demo Waveform & Audio Logic ---
  function getDemoProgress() {
    if (!demoAudioPlayer || !demoAudioPlayer.duration) return 0;
    return Math.max(0, Math.min(1, demoAudioPlayer.currentTime / demoAudioPlayer.duration));
  }

  function renderDemoWaveforms() {
    const prog = getDemoProgress();
    if (demoOrigCanvas) {
      drawWaveform(demoOrigCanvas, DEMO_RAW_BARS, 'rgba(245, 158, 11, 0.45)', '#f59e0b', prog);
    }
    if (demoCleanCanvas) {
      drawWaveform(demoCleanCanvas, DEMO_CLEAN_BARS, 'rgba(16, 185, 129, 0.45)', '#10b981', prog);
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

  function attachDemoSeek(canvas) {
    if (!canvas || !demoAudioPlayer) return;
    canvas.addEventListener('click', (e) => {
      if (!demoAudioPlayer.duration) return;
      const rect = canvas.getBoundingClientRect();
      const ratio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
      demoAudioPlayer.currentTime = ratio * demoAudioPlayer.duration;
      renderDemoWaveforms();
    });
  }

  attachDemoSeek(demoOrigCanvas);
  attachDemoSeek(demoCleanCanvas);

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

  // --- Main User File Upload & Processing Logic ---
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

  async function handleFile(file) {
    originalFile = file;
    originalFileName = file.name.replace(/\.[^/.]+$/, "");
    if (origWavUrl) URL.revokeObjectURL(origWavUrl);
    origWavUrl = URL.createObjectURL(file);

    if (renameInput) {
      renameInput.value = `${originalFileName}-clean.wav`;
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
    if (origCanvas && origBars) {
      drawWaveform(origCanvas, origBars, 'rgba(245, 158, 11, 0.45)', '#f59e0b', prog);
    }
    if (cleanCanvas && cleanBars) {
      drawWaveform(cleanCanvas, cleanBars, 'rgba(16, 185, 129, 0.45)', '#10b981', prog);
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

  function attachSeek(canvas) {
    if (!canvas) return;
    canvas.addEventListener('click', (e) => {
      if (!audioPlayer || !audioPlayer.duration) return;
      const rect = canvas.getBoundingClientRect();
      const ratio = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width));
      audioPlayer.currentTime = ratio * audioPlayer.duration;
      renderWaveforms();
    });
  }

  attachSeek(origCanvas);
  attachSeek(cleanCanvas);

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

  window.addEventListener('resize', () => {
    renderWaveforms();
    if (demoAccordion && demoAccordion.open) renderDemoWaveforms();
  });

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
      if (statusText) statusText.textContent = "Cleaning air & static... (~1-2s)";

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

          const cleanSamples = processGuitarDSP(mono, loadedAudioBuffer.sampleRate);
          const wavBlob = encodeWav(cleanSamples, loadedAudioBuffer.sampleRate);

          if (cleanWavUrl) URL.revokeObjectURL(cleanWavUrl);
          cleanWavUrl = URL.createObjectURL(wavBlob);

          // Compute waveform bars & noise stats
          origBars = computeWaveformBars(mono, 200);
          cleanBars = computeWaveformBars(cleanSamples, 200);

          const origDb = estimateNoiseFloorDb(mono, loadedAudioBuffer.sampleRate);
          const cleanDb = estimateNoiseFloorDb(cleanSamples, loadedAudioBuffer.sampleRate);

          if (origDbStat) origDbStat.textContent = `Floor: ~${Math.round(origDb)} dBFS`;
          if (cleanDbStat) cleanDbStat.textContent = `Floor: ~${Math.round(cleanDb)} dBFS (Silent -${Math.max(1, Math.round(origDb - cleanDb))} dB)`;

          currentMode = 'clean';
          if (btnPlayClean) btnPlayClean.classList.add('active');
          if (btnPlayOrig) btnPlayOrig.classList.remove('active', 'orig');

          if (audioPlayer) audioPlayer.src = cleanWavUrl;
          if (downloadLink) {
            downloadLink.href = cleanWavUrl;
            const renameVal = (renameInput && renameInput.value.trim()) || `${originalFileName}-clean.wav`;
            const finalName = renameVal.endsWith('.wav') ? renameVal : `${renameVal}.wav`;
            downloadLink.download = finalName;
          }

          if (statusText) statusText.textContent = "Audio cleaned successfully! 🎉";
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

  if (renameInput) {
    renameInput.addEventListener('input', () => {
      if (downloadLink) {
        const renameVal = renameInput.value.trim() || `${originalFileName}-clean.wav`;
        downloadLink.download = renameVal.endsWith('.wav') ? renameVal : `${renameVal}.wav`;
      }
    });
  }
}

if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', initGuitarStudio);
} else {
  initGuitarStudio();
}
})();
</script>
