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

## NOTE
> [!WARNING]
> Invoking `document.AddChart<LineChart>();` will not render the line chart in `Xceed.Document.NET4` (version 4).
>
> It will only render the line chart in `Xceed.Document.NET4` (version 5), see the [explanation] here.
## Full code

