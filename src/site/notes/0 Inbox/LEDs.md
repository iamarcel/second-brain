---
{"dg-publish":true,"permalink":"/0-inbox/le-ds/","dgPassFrontmatter":true}
---

tags:: [[Electronics\|Electronics]]

Fun fact: LEDs are special diodes. Normal diodes also emit light, but infrared lights. Shining light on LEDs create a current through them. Solar panels are special LEDs.

LEDs are quite sensitive in how they can receive power. When heating up, they tend to let more current run through them. Which heats them up even more, and they get stuck in a loop burning up.

To correctly drive LEDs, you need to **limit the current**. LED strips do this with resistance in series with the lights, which is a simple, cheap way of doing it for small lights. High power LEDs need separate **current limiting power supplies**. There are:

-   Linear power supply (reduces voltage)
-   Buck converter (reduces voltage)
-   Buck boost / converter (can both boost and reduce)

For LEDs you need to find one that has current limiting option. Either get a power supply rated for the voltage and current of the light, or find one with an adjustable voltage and maximum current level.