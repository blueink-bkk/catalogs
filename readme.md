## jpc-catalogs-admin

### xp101-create-new-instance
- postgres password must be set using `$ export PGPASSWORD='xxxxxx'`
- uses postgres function `cms_instance__new(instance-name)`
```
    - locate all packages:package_type = 'cms'
    - exit if instance-name already exist.
    - create instance using pg-function `apm_package__new()`
    - create application folder using pg-function `content_folder__new(`root-folder:-100`)`
```


### XLSX columns description.

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

## NOTES:

about `Hi username! You've successfully authenticated, but Github does
not provide shell access.`

  - see https://kbroman.org/github_tutorial/pages/first_time.html
