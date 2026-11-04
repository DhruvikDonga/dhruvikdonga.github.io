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
  padding: 1.5rem;
  border: 1px solid var(--border-color, #444);
  border-radius: 8px;
  background-color: var(--bg-color, #1e1e1e);
  color: var(--text-color, #ddd);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
.guitar-dropzone {
  border: 2px dashed var(--border-color, #666);
  border-radius: 6px;
  padding: 2.5rem 1.5rem;
  text-align: center;
  cursor: pointer;
  transition: border-color 0.2s, background-color 0.2s;
}
.guitar-dropzone:hover, .guitar-dropzone.dragover {
  border-color: #42b983;
  background-color: rgba(66, 185, 131, 0.05);
}
.guitar-dropzone input {
  display: none;
}
.guitar-btn {
  background-color: #42b983;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 0.6rem 1.2rem;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s;
  display: inline-block;
  margin-top: 0.5rem;
}
.guitar-btn:hover {
  opacity: 0.88;
}
.guitar-btn:disabled {
  background-color: #555;
  cursor: not-allowed;
  opacity: 0.6;
}
.guitar-section {
  margin-top: 1.5rem;
  padding-top: 1.2rem;
  border-top: 1px solid var(--border-color, #333);
}
.guitar-input-field {
  width: 100%;
  padding: 0.6rem;
  background: rgba(0, 0, 0, 0.25);
  border: 1px solid var(--border-color, #555);
  border-radius: 4px;
  color: inherit;
  font-family: inherit;
  font-size: 1rem;
  box-sizing: border-box;
  margin-top: 0.3rem;
}
.guitar-status {
  margin-top: 0.8rem;
  font-size: 0.95rem;
  color: #42b983;
  font-weight: 500;
}
.guitar-player {
  width: 100%;
  margin-top: 0.8rem;
}
.guitar-mode-toggle {
  display: inline-flex;
  background: rgba(0, 0, 0, 0.35);
  border: 1px solid var(--border-color, #444);
  border-radius: 6px;
  padding: 2px;
}
.guitar-toggle-btn {
  background: transparent;
  border: none;
  color: #aaa;
  padding: 0.35rem 0.85rem;
  font-size: 0.85rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s ease;
}
.guitar-toggle-btn.active {
  background: #42b983;
  color: #fff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}
.guitar-toggle-btn.active.orig {
  background: #e67e22;
  color: #fff;
}
.guitar-viz-container {
  margin-top: 1.2rem;
}
.guitar-viz-card {
  background: rgba(0, 0, 0, 0.28);
  border: 1px solid var(--border-color, #333);
  border-radius: 6px;
  padding: 0.6rem 0.8rem 0.5rem 0.8rem;
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
  background: rgba(230, 126, 34, 0.2);
  color: #e67e22;
  border: 1px solid rgba(230, 126, 34, 0.4);
}
.badge-clean {
  background: rgba(66, 185, 131, 0.2);
  color: #42b983;
  border: 1px solid rgba(66, 185, 131, 0.4);
}
.guitar-canvas {
  width: 100%;
  height: 75px;
  display: block;
  cursor: pointer;
  border-radius: 4px;
  background: rgba(0, 0, 0, 0.25);
}
</style>

<div class="guitar-app">
<p style="margin-top: 0; color: #888; font-size: 0.95rem;">
Zero server upload. Runs 100% locally in your browser. Eliminates room air and mic static while keeping your natural acoustic guitar tone.
</p>
<div class="guitar-dropzone" id="dropZone">
<span style="font-size: 2.5rem;">🎸</span>
<p style="margin: 0.5rem 0 0 0; font-weight: 600;">Click to upload or drag & drop guitar audio</p>
<p style="margin: 0.2rem 0 0 0; font-size: 0.85rem; color: #888;">MP3, WAV, M4A, AAC, FLAC supported</p>
<input type="file" id="audioInput" accept="audio/*" />
</div>
<div id="actionArea" class="guitar-section" style="display: none;">
<p style="margin: 0 0 0.5rem 0; font-size: 0.9rem;" id="fileInfo"></p>
<label for="renameInput" style="font-size: 0.88rem; font-weight: 600; display: block;">Custom Output Filename:</label>
<input type="text" id="renameInput" class="guitar-input-field" value="cleaned-guitar.wav" placeholder="cleaned-guitar.wav" />
<button id="processBtn" class="guitar-btn" style="margin-top: 1rem;">Clean Audio Now</button>
<div id="statusText" class="guitar-status"></div>
</div>
<div id="resultArea" class="guitar-section" style="display: none;">
<div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 0.6rem;">
<h4 style="margin: 0;">🎧 Audio Preview & Comparison</h4>
<div class="guitar-mode-toggle" id="modeToggle">
<button type="button" id="btnPlayClean" class="guitar-toggle-btn active">✨ Cleaned</button>
<button type="button" id="btnPlayOrig" class="guitar-toggle-btn">📻 Original</button>
</div>
</div>
<audio id="audioPlayer" class="guitar-player" controls></audio>
<div class="guitar-viz-container">
<div class="guitar-viz-card">
<div class="guitar-viz-header">
<span style="color: #e67e22; font-weight: 600; font-size: 0.85rem;">
<span class="guitar-badge badge-orig">BEFORE</span> Original (Raw Input with Hiss)
</span>
<span id="origDbStat" style="font-size: 0.78rem; color: #888;"></span>
</div>
<canvas id="origCanvas" class="guitar-canvas"></canvas>
</div>
<div class="guitar-viz-card" style="margin-top: 0.8rem;">
<div class="guitar-viz-header">
<span style="color: #42b983; font-weight: 600; font-size: 0.85rem;">
<span class="guitar-badge badge-clean">AFTER</span> Cleaned (Silent Floor & Pure Tone)
</span>
<span id="cleanDbStat" style="font-size: 0.78rem; color: #42b983;"></span>
</div>
<canvas id="cleanCanvas" class="guitar-canvas"></canvas>
</div>
<p style="margin: 0.4rem 0 0 0; font-size: 0.78rem; color: #777; text-align: right;">💡 Click either waveform to jump playback</p>
</div>
<div style="margin-top: 1rem; display: flex; gap: 1rem; align-items: center; flex-wrap: wrap;">
<a id="downloadLink" class="guitar-btn" style="text-decoration: none;" download="cleaned-guitar.wav">⬇️ Download Clean WAV</a>
<span id="outputStats" style="font-size: 0.85rem; color: #888;"></span>
</div>
</div>
</div>

<script>
(function() {
'use strict';
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
  const windowSize = Math.floor(sampleRate * 0.02);
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
function computeWaveformBars(samples, numBars) {
  const bars = new Float32Array(numBars);
  const step = samples.length / numBars;
  for (let i = 0; i < numBars; i++) {
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

  const numBars = bars.length;
  const barWidth = width / numBars;
  const playheadX = progressRatio * width;
  const bw = Math.max(1, barWidth - 1);

  for (let i = 0; i < numBars; i++) {
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
      drawWaveform(origCanvas, origBars, 'rgba(230, 126, 34, 0.45)', '#f39c12', prog);
    }
    if (cleanCanvas && cleanBars) {
      drawWaveform(cleanCanvas, cleanBars, 'rgba(66, 185, 131, 0.45)', '#42b983', prog);
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

  window.addEventListener('resize', renderWaveforms);

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
          const numBars = 220;
          origBars = computeWaveformBars(mono, numBars);
          cleanBars = computeWaveformBars(cleanSamples, numBars);

          const origDb = estimateNoiseFloorDb(mono, loadedAudioBuffer.sampleRate);
          const cleanDb = estimateNoiseFloorDb(cleanSamples, loadedAudioBuffer.sampleRate);

          if (origDbStat) origDbStat.textContent = `Hiss Floor: ~${Math.round(origDb)} dBFS (Detected)`;
          if (cleanDbStat) cleanDbStat.textContent = `Hiss Floor: ~${Math.round(cleanDb)} dBFS (Silent -${Math.max(1, Math.round(origDb - cleanDb))} dB)`;

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
