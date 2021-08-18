<html>

<head>
    <meta charset="utf-8" />
    <script src="https://unpkg.com/pdf-lib@1.4.0"></script>
    <script src="https://unpkg.com/downloadjs@1.4.7"></script>
</head>

<body>
    <p>Click the button to modify an existing PDF document with <code>pdf-lib</code></p>
    <button onclick="modifyPdf()">Modify PDF</button>
    <p class="small">(Your browser will download the resulting file)</p>
</body>

<script>
    const { degrees, PDFDocument, rgb, StandardFonts } = PDFLib

    async function modifyPdf() {
        // Fetch an existing PDF document
        const url = 'https://drive.google.com/file/d/1D-VS34kzHg7uI0XtPBQ5Ffta4wIR1mLW/view?usp=sharing'
        const existingPdfBytes = await fetch(url, { mode: 'no-cors' }).then(res => res.arrayBuffer())

        // Load a PDFDocument from the existing PDF bytes
        const pdfDoc = await PDFDocument.load(existingPdfBytes)

        // Embed the Helvetica font
        const helveticaFont = await pdfDoc.embedFont(StandardFonts.Helvetica)

        // Get the first page of the document
        const pages = pdfDoc.getPages()
        const firstPage = pages[0]

        // Get the width and height of the first page
        const { width, height } = firstPage.getSize()

        // Draw a string of text diagonally across the first page
        firstPage.drawText('This text was added with JavaScript!', {
            x: 5,
            y: height / 2 + 300,
            size: 50,
            font: helveticaFont,
            color: rgb(0.95, 0.1, 0.1),
            rotate: degrees(-45),
        })

        // Serialize the PDFDocument to bytes (a Uint8Array)
        const pdfBytes = await pdfDoc.save()

        // Trigger the browser to download the PDF document
        download(pdfBytes, "pdf-lib_modification_example.pdf", "application/pdf");
    }
</script>

</html>
