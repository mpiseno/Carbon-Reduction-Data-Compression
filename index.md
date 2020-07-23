---
layout: default
---

# Dropbox Internship and Hack Week

<p align="justify">
This summer I had the humbling opportunity to intern at Dropbox as a software engineer intern. I worked on a chrome extension that helps users manage their workflow more efficiently, in which I built the ML infrastructure from the ground up. Having worked at several tech companies in Silicon Valley, I can say that Dropbox has been one of the most rewarding experiences I have had, even as a remote internship due to the Covid-19 pandemic.
</p>

<p align="justify">
Each summer, Dropbox hosts <a href="https://blog.dropbox.com/topics/inside-dbx/be-a-force-for-change--hack-week-2019" target="_blank">Hack Week</a>, an opportunity for Dropboxers to put aside their normal work for an entire week and hack together any project their hearts desire. For my Hack Week project, I put my intern project to the side and took on the challenge of changing how data is compressed from the Dropbox desktop client when users upload files to the server. Doing so has implications for the amount of CPU the client uses during an upload and involved learning Rust, a systems programming language quite different from my pythonic comfort zone. With the massive amounts of data Dropbox users upload on a daily basis, the savings in CPU power from smarter compression produces a non-trivial savings in energy, and consequently carbon emissions.
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
When a user uploads data to Dropbox, the compression algorithm used takes a parameter called compression quality, which we'll call $q$. $q$ is a positive integer that can takes values between 0 and 5 inclusive, where 0 performs the least amount of compression (but not no compression) and 5 performs the highest amount. Ideally, if we detect incompressible data being uploaded, we will tell the algorithm to bypass compression completely, otherwise we want to set $q$ to something reasonable based on how compressible we think the data is. To predict compressibility, we use a common measurement called shannon entropy, $H$, which is a measurement of how much information is in a byte stream (or string). The formula for $H$ is

$$H(s) = -\sum_{s_{i}} p_{i}\log_{2}(p_{i})$$

where $s$ is the input string, $s_{i}$ is character $i$ of the string, and $p_{i}$ is the probability of character $i$ appearing in the string.
</p>

<p align="justify">
For example, given the string $s_{1}=$"Hiiii", the shannon entropy would be

$$H(s_{1}) = -\frac{1}{5}\log_{2}(\frac{1}{5}) - 4 * \frac{4}{5}\log_{2}(\frac{4}{5}) = 1.49$$

For $s_{2}=$"hello", the shannon entropy would be

$$H(s_{2}) = -\frac{1}{5}\log_{2}(\frac{1}{5}) - \frac{1}{5}\log_{2}(\frac{1}{5}) - 2 * \frac{2}{5}\log_{2}(\frac{2}{5}) - \frac{1}{5}\log_{2}(\frac{1}{5}) = 1.90$$

So $s_{2}$ has higher entropy than $s_{1}$, meaning it has more information. This make sense, because there are more unique characters; it tells us more about the alphabet. It is more complex.
</p>

<p align="justify">
Similar logic allows us to predict that lower entropy data are likely to be more compressible. For example, if my (primitive) compression algorithm is a character followed by the number of times it appears in a row, $s_{1}$ when compressed becomes "1H4i", whereas $s_{2}$ becomes "1H1e2l1o". With this simple compression rule, $s_{1}$ is more compressible. Without getting into the details of more advanced compression algorithms, it suffices to say a similar trend holds.
</p>

<p align="justify">
With that in mind, we define a metric called the entropy ratio $R$ which is a function of the input string defined as

$$R(s) = \frac{H(s)}{log_{b}(|s|)}$$

which simply normalizes the entropy by the logarithm of the length of $s$. $b$ is chosen so that the maximum entropy results in a $R$ value of 1. This will allow us to use the entropy ratio as an estimator for the compression ratio of data. Based on the below chart of common user uploads and their known compression ratios, we define a simple heuristic to choose the $q$ value used for compresison based on the entropy ratio of the data to be uploaded.
</p>

<center>
  <div class="col-lg-8 col-md-8 col-sm-12 col-xs-12">
    <img class="rounded mx-auto d-block" src="{{ site.baseurl }}/assets/img/compression-ratio.png">
  </div>
</center>

<p align="justify">
When a user uploads data, we simply calculate $R$ on that data and use a larger value for $q$ if $R$ is low (say, below 0.5), while using a value that indicates no compression if $R$ is too high (say, above 0.8). This rule gives us a simple but promising heuristic for detecting incompressible data.
</p>

## Carbon Reduction

<p align="justify">
So how does this translate into carbon reduction? Well we have to quantify how much electricity we are saving in terms of the amount of compute power we are saving, which will involve some assumptions. We can take the number of GB of incompressible data that users upload each day, which we call $N_{GB}$ and multply it by the energy used from compressing each GB. [1] estimates that each GB of data we compress and decompress takes 2000 J of energy. If we can find out the distribution of Dropbox users (per country) and the correspodning carbon emmisions per KwH of energy on average for each country, we can directly quantify the carbon reduction of not compressing incompressible data.
</p>

## Results

<p align="justify">
Placeholder
</p>

## Future Work

<p align="justify">
Placeholder
</p>

## References

[1]

