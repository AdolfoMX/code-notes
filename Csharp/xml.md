# XML Parsing

## XmlDocument

### Reading XML with `XmlDocument`
```cs
using System.Xml;
```

```cs
XmlDocument xmlDoc = new XmlDocument();
xmlDoc.Load(pathXML);

// top level XML properties
Version = xmlDoc.DocumentElement.SelectSingleNode("/PVScan").Attributes["version"].Value;
Date = xmlDoc.DocumentElement.SelectSingleNode("/PVScan").Attributes["date"].Value;
SequenceType = xmlDoc.DocumentElement.SelectSingleNode("/PVScan/Sequence").Attributes["type"].Value;

// load all laser settings
XmlNodeList nodesLaserPower = xmlDoc.DocumentElement.SelectNodes("/PVScan/PVStateShard/PVStateValue[@key='laserPower']");
foreach (XmlElement xmlLaser in nodesLaserPower[0].ChildNodes)
{
	int laserIndex = int.Parse(xmlLaser.Attributes["index"].Value);
	double laserPower = double.Parse(xmlLaser.Attributes["value"].Value);
	string laserName = xmlLaser.Attributes["description"].Value;
}
```

### Creating XML with `XmlDocument`
```cs

XmlDocument doc = new XmlDocument();

XmlElement notes = doc.CreateElement("notes");
notes.InnerText = "experiment worked so awesomely";

XmlElement tags = doc.CreateElement("tags");
for (int i = 0; i < 5; i++)
{
XmlElement tag = doc.CreateElement("tag");
tag.SetAttribute("timeSec", $"{i * 60}");
tag.SetAttribute("frame", $"{i * 5}");
tag.InnerText = $"fancy drug {i + 1}";
tags.AppendChild(tag);
}

XmlElement xmlExperiment = doc.CreateElement("Experiment");
xmlExperiment.SetAttribute("created", $"2018-01-01");
xmlExperiment.SetAttribute("modified", $"2019-09-29");
xmlExperiment.AppendChild(notes);
xmlExperiment.AppendChild(tags);
doc.AppendChild(xmlExperiment);

string xmlText = doc.OuterXml;
System.IO.File.WriteAllText(pathFile, xmlText);
System.Console.WriteLine($"Saved: {pathFile}");
```
