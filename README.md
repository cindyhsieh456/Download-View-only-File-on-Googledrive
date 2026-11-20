# Use this script to download specific pages on view only pdf file from Google Drive. 

Work with AI tools (chatGPT)

## Big-Note: Use this script wisely!
### Instruction
1. Open the PDF from Google Drive,
2. Click Preview,
3. Then on top right page click on three vertical dots menu -> Open in new window,
4. Scroll down pdf to end of content and make sure all of content is loaded,
5. Inspect element your browser and go to console tab,
6. Type allow pasting and press ENTER, if you are using Chrome and cannot paste any code directly in the console tab,
7. Copy and paste script below on console tab,

```javascript
let startPage = 1;     // start page
let endPage = 100;     // end page

let trustedURL;

if (window.trustedTypes && trustedTypes.createPolicy) {
    const policy = trustedTypes.createPolicy('myPolicy', {
        createScriptURL: (input) => input
    });

    trustedURL = policy.createScriptURL(
        'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js'
    );
} else {
    console.warn(
        "Trusted Types are not supported in this browser."
    );

    trustedURL =
        'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js';
}

let jspdf = document.createElement("script");

jspdf.onload = function () {

    let pdf = new jsPDF();

    // find every blob images
    let images = Array.from(
        document.getElementsByTagName("img")
    ).filter(img => /^blob:/.test(img.src));

    // only focus on specific pages
    let selectedImages =
        images.slice(startPage - 1, endPage);

    for (let img of selectedImages) {

        let canvasElement = document.createElement('canvas');
        let con = canvasElement.getContext("2d");

        canvasElement.width = img.width;
        canvasElement.height = img.height;

        con.drawImage(
            img,
            0,
            0,
            img.width,
            img.height
        );

        let imgData =
            canvasElement.toDataURL("image/jpeg", 1.0);

        pdf.addImage(
            imgData,
            'JPEG',
            0,
            0
        );

        pdf.addPage();
    }

    // 避免最後多一個空白頁
    if (selectedImages.length > 0) {
        // 這裡的邏輯可再依 jsPDF 版本調整
    }

    pdf.save(
        `download-${startPage}-${endPage}.pdf`
    );
};

jspdf.src = trustedURL;

document.body.appendChild(jspdf);
```
8. Select file save place
9. Finish 😻

## Source Reference
This script is modified with source from :
[Google-Drive-View-Only-PDF-Script-Downloader](https://github.com/zavierferodova/Google-Drive-View-Only-PDF-Script-Downloader)
