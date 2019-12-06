## Code

As an IT major, I've written a lot of code. I'm primarily interested in the entertainment side of IT, but here's something I'm relatively proud of


[here's](https://www.youtube.com/watch?v=cMg6qAI1IPs&feature=youtu.be) a video of some code I wrote for a basic synthesizer last year (If you watch this you may want to turn your volume down).


Since homework videos are boring and your time is valuable, though, you probably just want to look at this snippet of code to get an idea of what the program is like:

```C#
private void playSquare(float freq)
        {
            short[] wave = new short[SAMPLERATE];
            byte[] binaryWave = new byte[SAMPLERATE * sizeof(short)];

            for (int i = 0; i < SAMPLERATE; i++)
            {
                wave[i] = Convert.ToInt16(short.MaxValue * Math.Sign(Math.Sin(((Math.PI * 2 * freq) / SAMPLERATE) * i)));
            }
            Buffer.BlockCopy(wave, 0, binaryWave, 0, wave.Length * sizeof(short));

            using (MemoryStream memoryStream = new MemoryStream())
            using (BinaryWriter binaryWriter = new BinaryWriter(memoryStream))
            {
                short blockAlign = SAMPLEBITS / 8;
                int subChunkTwoSize = SAMPLERATE * blockAlign;
                binaryWriter.Write(new[] { 'R', 'I', 'F', 'F' });
                binaryWriter.Write(36 + subChunkTwoSize);
                binaryWriter.Write(new[] { 'W', 'A', 'V', 'E', 'f', 'm', 't', ' ' });
                binaryWriter.Write(16);
                binaryWriter.Write((short)1);
                binaryWriter.Write((short)1);
                binaryWriter.Write(SAMPLERATE);
                binaryWriter.Write(SAMPLERATE * blockAlign);
                binaryWriter.Write(blockAlign);
                binaryWriter.Write(SAMPLEBITS);
                binaryWriter.Write(new[] { 'd', 'a', 't', 'a' });
                binaryWriter.Write(subChunkTwoSize);
                binaryWriter.Write(binaryWave);
                memoryStream.Position = 0;
                new SoundPlayer(memoryStream).Play();
            }
        }
```


---
[HOME](https://github.com/danieljclubb/IT1000-Final/blob/master/README.md)
