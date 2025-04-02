# How to draw line chart in Xceed Word?
## Answer
### In Xceed.Document.NET4 (version 4)
Step 1:

Instantiate the `LineChart` instance.

```
var lineChart = new LineChart();
```

Step 2:

Add series to line chart.

Step 2.1:

To do that, first prepare the data.

```
List<int> yValueList = new List<int>();
List<string> xCategoryList = new List<string>();
for (int i = 0; i < 5; i++)
{
  xCategoryList.Add($"X{i}");
  yValueList.Add(i);
}
```

Step 2.2:

Then instantiate `Series` instance.

```
Series series = new Series("series1");
```

Step 2.3:

Bind the data to Series instance via `bind` method call.

```
series.Bind(xCategoryList, yValueList);
```

Step 2.4:

Add series to LineChart instance via `AddSeries` method call.

```
lineChart.AddSeries(series);
```

Step 3:

Configure the setting of line chart.

For example, you can set the line chart as 3D by

```
lineChart.View3D = true;
```

Step 4:

Insert the line chart to document.

```
document.InsertChart(lineChart);
```

#### NOTE
> [!WARNING]
> Invoking `document.AddChart<LineChart>();` will not render the line chart in `Xceed.Document.NET4` (version 4).
>
> It will only render the line chart in `Xceed.Document.NET4` (version 5).
>
> For more information, see the [explanation of Microsoft Copilot](https://github.com/40843245/CSharp/blob/main/third%20package/API/Xceed/Words/Q%26A/AI%20model/Microsoft%20Copilot/How%20to%20draw%20a%20line%20chart%20in%20Xceed.Document.NET4%20in%20C%23%3F.md) here.

#### Full code

```
        [Obsolete]
        public static void AddLineChartView2DExample()
        {
            string fullDirectory = FilePathConstants.DirectoryConstants.DOCUMENT_DIRECTORY;
            string fileName = @"AddLineChartView2DExample.docx";
            string fullPath = Path.Combine(fullDirectory, fileName);

            // Create a document.
            using (var document = DocX.Create(fullPath))
            {
                //var lineChart = document.AddChart<LineChart>();
                var lineChart = new LineChart();
                lineChart.View3D = false;
                List<int> yValueList = new List<int>();
                List<string> xCategoryList = new List<string>();
                for (int i = 0; i < 5; i++)
                {
                    xCategoryList.Add($"X{i}");
                    yValueList.Add(i);
                }

                Series series = new Series("series1");

                series.Bind(xCategoryList, yValueList);
                lineChart.AddSeries(series);
                document.InsertChart(lineChart);
                document.Save();
            }
        }
```

### In Xceed.Document.NET5 (version 5)
Concept of drawing a line chart in similar to that in `Xceed.Document.NET4`.

Step 1:

Insert the `LineChart` instance via `AddChart<LineChart>` method call.

```
var lineChart = document.AddChart<LineChart>();
```

Step 2:

Add series to line chart.

Step 2.1:

To do that, first prepare the data.

```
List<int> yValueList = new List<int>();
List<string> xCategoryList = new List<string>();
for (int i = 0; i < 5; i++)
{
  xCategoryList.Add($"X{i}");
  yValueList.Add(i);
}
```

Step 2.2:

Then instantiate `Series` instance.

```
Series series = new Series("series1");
```

Step 2.3:

Bind the data to Series instance via `bind` method call.

```
series.Bind(xCategoryList, yValueList);
```

Step 2.4:

Add series to LineChart instance via `AddSeries` method call.

```
lineChart.AddSeries(series);
```

Step 3:

Configure the setting of line chart.

For example, you can set the line chart as 3D by

```
lineChart.View3D = true;
```

#### NOTE

> [!WARNING]
> This approach is only apply for `Xceed.Document.NET5` (version 5).
#### Full Code
```
        public static void AddLineChartView2DExample()
        {
            string fullDirectory = FilePathConstants.DirectoryConstants.DOCUMENT_DIRECTORY;
            string fileName = @"AddLineChartView2DExample.docx";
            string fullPath = Path.Combine(fullDirectory, fileName);

            // Create a document.
            using (var document = DocX.Create(fullPath))
            {
                var lineChart = document.AddChart<LineChart>();
                lineChart.View3D = false;
                List<int> yValueList = new List<int>();
                List<string> xCategoryList = new List<string>();
                for (int i = 0; i < 5; i++)
                {
                    xCategoryList.Add($"X{i}");
                    yValueList.Add(i);
                }

                Series series = new Series("series1");

                series.Bind(xCategoryList, yValueList);
                lineChart.AddSeries(series);
                document.Save();
            }
        }
```
