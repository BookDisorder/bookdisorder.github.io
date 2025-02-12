---
date: '2025-02-11T19:44:35-05:00'
draft: false
title: 'Automating Automation'
tags: ['Gemini', 'AI']
---
I *finally* found my first real world use case for a large language model chatbot.

I mentioned in my first [post](/posts/hello-world) that I would use Google's chatbot Gemini to help set up the hugo.yaml config file for this site. *But,* that didn't really work out. Gemini was able to help define a few key parameters. It could not, however, create a .yaml file through prompting alone. I ended up having to do it the old fashioned way -- by referencing the Hugo docs.

## My First Use Case

### The problem

I am enrolled in school, going for my BA in English. For one of my recent papers I had to reference a documentary available on Youtube. In my initial viewing I wrote down a couple of paraphrased quotes and foolishly neglected to record the timestamps.

For some reason, not all movies and TV shows on YouTube have transcripts available. This documentary did have closed captions though, so I knew that there must be a transcript somewhere. I had to find that transcript. I really didn't want to have to scrub through the whole 90 minute documentary again looking for the quotes.

### Finding the right words

I pulled up the video, clicked the closed caption button, opened up the network tab of Chrome DevTools. A few seconds after pressing play, I saw a URL for "timedtext."

![Screenshot of timedtext in DevTools](/images/timedtext.png)

I clicked open in new tab and downloaded the text file. Sure enough, all the words were there...

```json
 {
    "tStartMs": 5202696,
    "dDurationMs": 3037,
    "segs": [ {
      "utf8": "♪ And it's all\ngonna roll your way ♪"
    } ],
    "wpWinPosId": 35,
    "wsWinStyleId": 1
  }, {
    "tStartMs": 5205766,
    "dDurationMs": 2036,
    "segs": [ {
      "utf8": "♪ Working nine to five ♪"
    } ],
    "wpWinPosId": 32,
    "wsWinStyleId": 1
  }, {
    "tStartMs": 5207835,
    "dDurationMs": 2503,
    "segs": [ {
      "utf8": "♪ What a way\nto make a living ♪"
    } ],
    "wpWinPosId": 25,
    "wsWinStyleId": 1
  },
```

... in JSON format, timestamped in milliseconds.

### All in good time

I knew that I could search the file to find the quotes I needed. Could I make the file easier to read? Specifically, I wanted the timestamps in an easier to read format for referencing the video.

I prompted Gemini 2.0 Pro Experimental:

>I have a closed caption timed text file from a YouTube video. I want to turn this file into a transcript for the video with timestamps formatted in hours, seconds, minutes. Write a python script to do this.

I also included a sample of the JSON formatted text into the chat for reference.

Gemini returned this python script:

```python
import json

def convert_to_srt(data):
    """Converts YouTube timed text data to SRT format."""
    srt_output = ""
    counter = 1
    for item in data:
        start_ms = item['tStartMs']
        duration_ms = item['dDurationMs']
        end_ms = start_ms + duration_ms
        text = "".join([seg['utf8'] for seg in item['segs']]).strip()

        start_time = format_time(start_ms)
        end_time = format_time(end_ms)

        srt_output += f"{counter}\n"
        srt_output += f"{start_time} --> {end_time}\n"
        srt_output += f"{text}\n\n"
        counter += 1
    return srt_output

def convert_to_vtt(data):
    """Converts YouTube timed text data to VTT format."""
    vtt_output = "WEBVTT\n\n"
    for item in data:
        start_ms = item['tStartMs']
        duration_ms = item['dDurationMs']
        end_ms = start_ms + duration_ms
        text = "".join([seg['utf8'] for seg in item['segs']]).strip()

        start_time = format_time(start_ms, vtt=True)
        end_time = format_time(end_ms, vtt=True)

        vtt_output += f"{start_time} --> {end_time}\n"
        vtt_output += f"{text}\n\n"
    return vtt_output

def format_time(ms, vtt=False):
    """Formats milliseconds into HH:MM:SS,mmm (SRT) or HH:MM:SS.mmm (VTT) format."""
    seconds = ms // 1000
    ms = ms % 1000
    minutes = seconds // 60
    seconds = seconds % 60
    hours = minutes // 60
    minutes = minutes % 60

    if vtt:
        return f"{hours:02d}:{minutes:02d}:{seconds:02d}.{ms:03d}"
    else:
        return f"{hours:02d}:{minutes:02d}:{seconds:02d},{ms:03d}"

# --- Main part of the script ---

input_file = "your_timed_text_file.txt"  # Replace with your input file name
output_file_srt = "output.srt"
output_file_vtt = "output.vtt"

try:
    with open(input_file, 'r', encoding='utf-8') as f:
        # Check if the file starts with '[' (likely an array)
        first_char = f.read(1)
        f.seek(0)  # Reset file pointer
        
        if first_char == '[':
            timed_text_data = json.load(f)
        else:
            # Handle it as a wrapped JSON object
            json_str = f.read()
            # Assuming it's wrapped in {"events": ...}
            json_obj = json.loads(json_str)
            timed_text_data = json_obj.get("events", [])

    srt_captions = convert_to_srt(timed_text_data)
    vtt_captions = convert_to_vtt(timed_text_data)

    with open(output_file_srt, 'w', encoding='utf-8') as f:
        f.write(srt_captions)

    with open(output_file_vtt, 'w', encoding='utf-8') as f:
        f.write(vtt_captions)

    print(f"SRT captions saved to {output_file_srt}")
    print(f"VTT captions saved to {output_file_vtt}")

except FileNotFoundError:
    print(f"Error: Input file '{input_file}' not found.")
except json.JSONDecodeError:
    print(f"Error: Invalid JSON format in '{input_file}'.")
except Exception as e:
    print(f"An error occurred: {e}")
```

It is a little overkill, I guess. I just wanted a text file output, not srt or vtt subtitle files.

Will it get me the results I want?

### output.srt

I tweaked the python script to take my input.txt file.

```python
input_file = "input.txt"  # Replace with your input file name
```

I saved the script with the name recommended by Gemini, and included my input.txt file in the same directory.

I ran the script:

```bash
$ python3 convert_yt_transcript.py
SRT captions saved to output.srt
VTT captions saved to output.vtt
```

The output was exactly what I needed. 

```
1698
01:26:42,696 --> 01:26:45,733
♪ And it's all
gonna roll your way ♪

1699
01:26:45,766 --> 01:26:47,802
♪ Working nine to five ♪

1700
01:26:47,835 --> 01:26:50,338
♪ What a way
to make a living ♪
```

### Automagic

Voila, just like that!

I was able to implement a time saving solution for a simple, potentially tedious task within 5 minutes using some casual instructions and little bit of know-how. It's far less time than it would have taken me to write it myself, and Gemini gave me a functional script that reformatted an entire text file.

Here's the catch though: I haven't been able to repeat it again on the first try. While writing this post I've tried to recreate the results multiple times in separate Gemini chats. Only the first script that Gemini created (the one above) worked on the first try.

Right now, this seems to be the hitch with all of the major LLMs. You can't be quite sure *which* rabbit a chatbot is going to pull out of its hat... or if there will even be a rabbit at all.