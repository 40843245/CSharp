# How to draw a line chart in Xceed.Document.NET4 in C#?
## Answer from Microsoft Copilot.

<img width="443" alt="image" src="https://github.com/user-attachments/assets/d6f953e6-5b10-4c8a-96df-bbff9c43a02a" />

Creating a line chart in a Word document using Xceed Words for .NET in C# can be a straightforward process. Below is a concise example to help you get started:

```
using System; using Xceed.Words.NET;

namespace LineChartExample
{

   class Program
   {
      static void Main(string[] args)
      {
          // Create a new document
          var doc = DocX.Create("LineChartExample.docx");
          // Add a title
          doc.InsertParagraph("Line Chart Example").FontSize(20).Bold().Alignment = Alignment.center;
          // Create a chart
          var chart = new Chart();
          chart.AddSeries(
                          name: "Series 1",
                          xValues: new[] { "Jan", "Feb", "Mar", "Apr", "May" },
                          yValues: new[] { 10, 20, 30, 40, 50 }
          );
          // Add the chart to the document
             doc.InsertChart(chart);
          // Save the document
             doc.Save();
          Console.WriteLine("Document created successfully!");
       }
   }
}
```

### Explanation:

1.  **Create a new document**: `DocX.Create("LineChartExample.docx")` initializes a new Word document.
2.  **Add a title**: `doc.InsertParagraph("Line Chart Example")` adds a title to the document.
3.  **Create a chart**: `var chart = new Chart()` initializes a new chart object.
4.  **Add series to the chart**: `chart.AddSeries` adds data points to the chart.
5.  **Insert the chart into the document**: `doc.InsertChart(chart)` embeds the chart into the Word document.
6.  **Save the document**: `doc.Save()` saves the document to the specified file path.

This example demonstrates how to create a simple line chart and insert it into a Word document using Xceed Words for .NET. You can customize the chart further by adding more series, adjusting styles, and modifying data points as needed.
