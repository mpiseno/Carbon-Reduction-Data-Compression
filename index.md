---
layout: default
---

# Dropbox Internship and Hack Week

<p align="justify">
This summer I had the humbling opportunity to intern at Dropbox as a software engineer intern. I worked on <a href="https://www.blinkapp.io/" target="_blank">Blink</a>, a chrome extension that helps users manage their workflow more efficiently, where I built the ML infrastructure from the ground up. Having worked at several tech companies in Silicon Valley, I can say that Dropbox has been one of the most rewarding experiences I have had, even as a remote internship due to the Covid-19 pandemic.
</p>

<p align="justify">
Each summer, Dropbox hosts <a href="https://blog.dropbox.com/topics/inside-dbx/be-a-force-for-change--hack-week-2019" target="_blank">Hack Week</a>, an opportunity for Dropboxers to put aside their normal work for an entire week and hack together any project their heart desires. For my Hack Week project, I put Blink to the side and took on the challenge of changing how data is compressed from the Dropbox desktop client when users upload files to the server. Doing so has implications for the amount of CPU the client uses during an upload and involved learning Rust, a systems programming language quite different from my pythonic comfort zone. With the massive amounts of data Dropbox users upload on a daily basis, the savings in CPU power from smarter compression produces a non-trivial savings in energy, and consequently carbon emissions.
</p>


## Carbon Reduction Challenge

<p align="justify">
The <a href="https://www.carbonreductionchallenge.org/" target="_blank">Carbon Reduction Challenge</a> is a competition hosted annually at my school, Georgia Tech, which challenges students to identify sources of carbon emissions at their internships, and allows them to propose changes within the company to reduce these emissions and many times even save the company money in the process. I saw this as a perfect opportunity to combine my interest in sustainability with Hack Week, and thus pursued my data compression project from a climate action perspective.
</p>


# Nucleus

<p align="justify">
<a href="https://dropbox.tech/infrastructure/rewriting-the-heart-of-our-sync-engine" target="_blank">Nucleus</a> is the name of Dropbox's new client sync engine, the technology behind how users' files on their local machines get synced with Dropbox servers. When a user creates or edits a file in their synced folder (the one called "Dropbox" on their local machine), Nucleus kicks in to action, making sure the local machine and server reach the same state. This process includes compressing the data so that it can be sent faster on the wire (through a TCP connection).
</p>

<p align="justify">
Unfortunately, some data are incompressible, and so any compression and subsquent decompression of that data is wasted CPU. Identifying incompressible data gives us an opportunity to save on this wasted CPU, however identifying it is a non-trivial task. It turns out a lot of the data users upload to Dropbox is incompressible, which means we have a great opportunity for saving on a ton of CPU usage.
</p>

## Smarter Data Compression

<p align="justify">
Placeholder
</p>

## Approach

<p align="justify">
Placeholder
</p>

## Results

<p align="justify">
Placeholder
</p>

