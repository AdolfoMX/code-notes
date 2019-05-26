# Audio / Soundcard
Unless otherwise specified, audio access is provided by "NAudio". This can be installed with NuGet.

## List Audio Devices

```cs
private void ScanSoundCards()
{
    cbDevice.Items.Clear();
    for (int i = 0; i < NAudio.Wave.WaveIn.DeviceCount; i++)
        cbDevice.Items.Add(NAudio.Wave.WaveIn.GetCapabilities(i).ProductName);
    if (cbDevice.Items.Count > 0)
        cbDevice.SelectedIndex = 0;
    else
        MessageBox.Show("ERROR: no recording devices available");
}
```

## Recording Audio data with NAudio
```cs
// do the setup only once
wvin = new WaveInEvent();
wvin.DeviceNumber = DeviceIndex;
wvin.WaveFormat = new NAudio.Wave.WaveFormat(SampleRate, BitRate, MicrophoneChannels);
wvin.DataAvailable += OnDataAvailable;
wvin.BufferMilliseconds = BufferMilliseconds;

// elsewhere
wvin.StartRecording();
```

```cs
private void OnDataAvailable(object sender, WaveInEventArgs args)
{
    // convert byte array to Int16 array
    int bytesPerSample = BitRate / 8;
    int samplesRecorded = args.BytesRecorded / bytesPerSample;
    Int16[] lastBuffer = new Int16[samplesRecorded];
    for (int i = 0; i < samplesRecorded; i++)
        lastBuffer[i] = BitConverter.ToInt16(args.Buffer, i * bytesPerSample);
    int lastBufferAmplitude = lastBuffer.Max() - lastBuffer.Min();
}
```

## Linearize a Circular Buffer
```cs
public double[] GetLinearPcmData()
{
    int indexSplit = nextBufferToFill * bufferSampleCount;
    double[] data = new double[pcmDataCircular.Length];
    for (int i = 0; i < indexSplit; i++)
        data[pcmDataCircular.Length - indexSplit + i] = pcmDataCircular[i];
    for (int i = 0; i < data.Length - indexSplit; i++)
        data[i] = pcmDataCircular[i + indexSplit];
    return data;
}
```
