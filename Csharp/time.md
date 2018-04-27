# Time Operations

## Get System Time (milliseconds)
```C#
double t = DateTime.Now.Ticks / TimeSpan.TicksPerMillisecond;
```

## Timing things with high resolution
```C#
System.Diagnostics.Stopwatch stopwatch = System.Diagnostics.Stopwatch.StartNew();

stopwatch.Reset();
stopwatch.Start();
// do something
stopwatch.Stop();

double timeMS = stopwatch.ElapsedTicks * 1000.0 / System.Diagnostics.Stopwatch.Frequency;

System.Console.WriteLine(string.Format("loaded in {1:0.00} ms", timeMS));
```

## Pause or Sleep
```cs
System.Threading.Thread.Sleep(5000); // sleep for 5 sec
```
