 string wwwrootPath = _webHostEnvironment.WebRootPath;
 string filePath = Path.Combine(wwwrootPath, "den.html");
 string htmlContent = string.Empty;

 using (FileStream fileStream = new FileStream(filePath, FileMode.Open, FileAccess.Read))
 {
     using (StreamReader reader = new StreamReader(fileStream))
     {
         htmlContent = await reader.ReadToEndAsync();
     }
 }

 // Convert HTML to PDF using Aspose.PDF
 var pdfDocument = new Aspose.Pdf.Document();
 var htmlLoadOptions = new HtmlLoadOptions
 {
     // Set additional options here if needed
     PageInfo = new PageInfo { Width = PageSize.A4.Width, Height = PageSize.A4.Height }
 };

 using (var htmlStream = new MemoryStream(System.Text.Encoding.UTF8.GetBytes(htmlContent)))
 {
     pdfDocument = new Aspose.Pdf.Document(htmlStream, htmlLoadOptions);
 }

 using (var memoryStream = new MemoryStream())
 {
     pdfDocument.Save(memoryStream);
     var pdfBytes = memoryStream.ToArray();
     string fileName = $"myfile_{101}.pdf";
     return File(pdfBytes, "application/pdf", fileName);
 }
