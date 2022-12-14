---
title: FastReport to PDF Demo
date: 2022-10-16 12:00:00 +0800
category: Proof Of Concept
tags: open-source csharp
image:
  path: /images/FastReportPDFDemo.png
  width: 1280   # in pixels
  height: 720   # in pixels
---

# FastReport to PDF Demo.
Minimal C# code to load a FastReport report template, populate a dataset, then export to PDF file.
> Project page: [https://github.com/arvinboggs/FastReportPdf](https://github.com/arvinboggs/FastReportPdf)

> Warning: FastReport is a commercial product with price starting at $499. 
> They also offer a [free open source][FROSgit] edition but the PDF export feature is actually an image export. That is, the text of the rendered PDF can neither be selected nor copied.
> For the purpose of creating a proof-of-concept project, I opted to use their [trial edition][FRT] instead of their open source edition.


1. Open Visual Studio 2022 and create a console application.
2. Optional. Set the target OS to Windows. This will minimize the file dependencies of FastReport nuget.
3. Go to Package Manager Console and run the following:
`Install-Package FastReport.Net.Demo`
4. On the startup method, write this section of code:
```c#
var pDataSet = CreateDataSet();
// optional. not needed for report rendering but needed for report designing
var pFilename = System.IO.Path.Combine(Util.AppDirectory, "dataset.xml");
pDataSet.WriteXml(pFilename, XmlWriteMode.WriteSchema); 
```
- The `CreateDataSet` function can anything that output a `DataSet`, ideally from a database query.
5. Download [FastReport Designer Community Edition]
6. Extract the zip and run `Designer.exe`.
7. Create a new blank report.
8. Add a new data source.
9. Click "New Connection".
10. Choose "XML Database" as the connection type.
11. On "Data file or URL" field, browse for "dataset.xml" (created by the code in step 2).  
12. Click Next.
13. Select the table.
14. Click Finish.
15. Save the report template as "report.frx".
16. Go back to Visual Studio and write these section:
```c#
pFilename = System.IO.Path.Combine(Util.AppDirectory, "report.frx");
var pReport = new Report();
pReport.Load(pFilename);            
pReport.RegisterData(pDataSet);
pReport.Prepare();
```
17. At this point, we are ready to produce a report file that an end user can use.
```c#
pFilename = System.IO.Path.Combine(Util.AppDirectory, "rendered_report.pdf");
FastReport.Export.Pdf.PDFExport pExport = new();
pExport.Producer = "My Application"; // optional
pExport.Title = "Products Report"; // optional
pReport.Export(pExport, pFilename);
```
18. Done.

## Resources

- [FastReport Open Source project page][FROSgit]
 
[FROSgit]:https://github.com/FastReports/FastReport
[FRT]:https://www.fast-report.com/en/download/fast-report-net
[FastReport Designer Community Edition]:https://github.com/FastReports/FastReport/releases/latest
