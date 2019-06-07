```
const json = XLSX.utils.sheet_to_json(workbook.Sheets[sheet1],{
    header:[
      "icat",             // A
      "isec",             // B
      "publisherName",    // C
      "pic",              // D - jpeg
      "lang",             // E
      "h1",               // F
      'h2',               // G
      "npages",           // H
      'yp',               // I - date d'edition
      'pdf',              // J - fileName
      'npages'            // K
    ], range:1
}); // THIS IS THE HEAVY LOAD.

```
