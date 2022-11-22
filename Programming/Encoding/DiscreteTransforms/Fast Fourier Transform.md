Fast Fourier Transform
---

For sound;
It let's us show information in the frequency domain for a given time period, given amplitude information.

Information encoded in such a way contains low amplitude, high frequency information, and a high amplitude low frequency information(the frequency/period of the information) which shows us the frequency of the amplitude change.

Whether by then applying DCT or filtering out any information below a given threshold, we are left out with highly compressable data for the given time period.


Some information in the frequency domain "decays".
For example square waves, in the frequency domain it's peak echoes out onto the frequencies past it's original sample rate. 

The frequency domain describes the sine wave's amplitude change frequency.
With amplitude being the strength with which the magnet pushes the speaker.

---

Reverse Fast Fourier Transform
---
Is a fast way to reverse the frequency domain information, onto an PCM signal.
An approximation of layering repeating sine wave pattern of a given frequency(multiplied by max amplitude) is applied. 

---