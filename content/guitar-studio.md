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
<h4 style="margin: 0 0 0.5rem 0;">✨ Cleaned Audio Preview</h4>
<audio id="audioPlayer" class="guitar-player" controls></audio>
<div style="margin-top: 1rem; display: flex; gap: 1rem; align-items: center; flex-wrap: wrap;">
<a id="downloadLink" class="guitar-btn" style="text-decoration: none;" download="cleaned-guitar.wav">⬇️ Download Clean WAV</a>
<span id="outputStats" style="font-size: 0.85rem; color: #888;"></span>
</div>
</div>
</div>

<script>
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
      const snr = mag / Math.max(1e-7, effectiveNoise);
      const snrDb = 20.0 * Math.log10(Math.max(1e-4, snr));

      const rawGain = 1.0 / (1.0 + Math.exp(-0.75 * (snrDb - 4.0)));
      const targetGain = Math.max(minFloor, rawGain);

      let smoothedGain;
      if (targetGain > prevGains[b]) {
        smoothedGain = 0.20 * prevGains[b] + 0.80 * targetGain;
      } else {
        smoothedGain = gammaSmooth * prevGains[b] + (1.0 - gammaSmooth) * targetGain;
      }
      prevGains[b] = smoothedGain;

      real[b] *= smoothedGain;
      imag[b] *= smoothedGain;
      if (b > 0 && b < numBins - 1) {
        real[fftLen - b] = real[b];
        imag[fftLen - b] = -imag[b];
      }
    }

    fft.inverse(real, imag);

    for (let i = 0; i < fftLen; i++) {
      const outIdx = start + i;
      output[outIdx] += real[i] * hann[i];
      normSum[outIdx] += hann[i] * hann[i];
    }
  }

  const clean = new Float32Array(samples.length);
  for (let i = 0; i < samples.length; i++) {
    clean[i] = normSum[i] > 1e-4 ? output[i] / normSum[i] : output[i];
  }

  const attackCoeff = Math.exp(-1.0 / (sampleRate * 0.005));
  const releaseCoeff = Math.exp(-1.0 / (sampleRate * 0.400));
  let envelope = 0;
  let expGain = 1.0;

  for (let i = 0; i < clean.length; i++) {
    const absIn = Math.abs(clean[i]);
    if (absIn > envelope) {
      envelope = attackCoeff * envelope + (1 - attackCoeff) * absIn;
    } else {
      envelope = releaseCoeff * envelope + (1 - releaseCoeff) * absIn;
    }
    const targetGain = envelope >= 0.022 ? 1.0 : Math.max(0.002, Math.exp((Math.log(Math.max(0.0001, envelope / 0.022)) / 2.302585 * 20.0) * 0.8));
    expGain += (targetGain - expGain) * 0.06;
    clean[i] *= expGain;
  }

  const boostGain = Math.pow(10, 4.5 / 20.0);
  const ceiling = 0.95;
  const lookahead = Math.round(sampleRate * 0.005);
  const limAttack = Math.exp(-1.0 / (sampleRate * 0.002));
  const limRelease = Math.exp(-1.0 / (sampleRate * 0.080));

  const boosted = new Float32Array(clean.length);
  const reqGains = new Float32Array(clean.length);
  for (let i = 0; i < clean.length; i++) {
    const s = clean[i] * boostGain;
    boosted[i] = s;
    const absS = Math.abs(s);
    reqGains[i] = (absS > ceiling && absS > 0.0001) ? ceiling / absS : 1.0;
  }

  const windowMin = new Float32Array(clean.length);
  const deq = [];
  for (let i = clean.length - 1; i >= 0; i--) {
    while (deq.length > 0 && deq[deq.length - 1].g >= reqGains[i]) {
      deq.pop();
    }
    deq.push({ idx: i, g: reqGains[i] });
    if (deq[0].idx >= i + lookahead) {
      deq.shift();
    }
    windowMin[i] = deq[0].g;
  }

  let curGain = 1.0;
  const finalized = new Float32Array(clean.length);
  for (let i = 0; i < clean.length; i++) {
    const target = windowMin[i];
    if (target < curGain) {
      curGain = limAttack * curGain + (1 - limAttack) * target;
    } else {
      curGain = limRelease * curGain + (1 - limRelease) * target;
    }
    curGain = Math.min(curGain, reqGains[i]);
    finalized[i] = boosted[i] * curGain;
  }

  return finalized;
}

function encodeWav(monoSamples, sampleRate) {
  const numChannels = 2;
  const bytesPerSample = 2;
  const blockAlign = numChannels * bytesPerSample;
  const dataSize = monoSamples.length * blockAlign;
  const buffer = new ArrayBuffer(44 + dataSize);
  const view = new DataView(buffer);

  function writeString(offset, string) {
    for (let i = 0; i < string.length; i++) {
      view.setUint8(offset + i, string.charCodeAt(i));
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

  if (!dropZone || !audioInput) return;

  let loadedAudioBuffer = null;
  let originalFileName = "guitar-cleaned";

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
    originalFileName = file.name.replace(/\.[^/.]+$/, "");
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
          const wavUrl = URL.createObjectURL(wavBlob);

          if (audioPlayer) audioPlayer.src = wavUrl;
          if (downloadLink) {
            downloadLink.href = wavUrl;
            const renameVal = (renameInput && renameInput.value.trim()) || `${originalFileName}-clean.wav`;
            const finalName = renameVal.endsWith('.wav') ? renameVal : `${renameVal}.wav`;
            downloadLink.download = finalName;
          }

          if (statusText) statusText.textContent = "Audio cleaned successfully! 🎉";
          if (resultArea) resultArea.style.display = "block";
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
</script>
