+++
authors = ["Dhruvik Donga"]
title = "Guitar Paddle"
date = "2026-04-22"
description = "Guitar Paddle is a WASM based web tool to provide audio pedal with customised input and output device setup and Rust based audio realtime processing"
projectLink = "https://github.com/DhruvikDonga/web_paddle"
projectName = "guitarpaddle"
+++

## About
Guitar Paddle is a WASM based web tool to provide audio pedal with customised input and output device setup and Rust based audio realtime processing .

Visit the application live at [webpaddle.dhruvik.cc](https://webpaddle.dhruvik.cc)

--- 

## DSP logic summarized 

### Normal audio graph

{{< vega "audio-graph" >}}
{
  
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "width": 600,
  "height": 300,
  "data": {
    "sequence": {
      "start": 0,
      "stop": 100,
      "step": 1,
      "as": "x"
    }
  },
  "transform": [
    {
      "calculate": "sin(datum.x / 5)",
      "as": "y"
    }
  ],
  "mark": "line",
  "encoding": {
    "x": {
      "field": "x",
      "type": "quantitative",
      "title": "Time"
    },
    "y": {
      "field": "y",
      "type": "quantitative",
      "title": "Amplitude"
    }
  }
}
{{< /vega >}}