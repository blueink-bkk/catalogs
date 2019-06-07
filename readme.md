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

### xp102-drop-app-instance
- uses pg-function `cms_instance__drop(`package_id`)`;
- a simple wrapper around `apm_package__delete(`package_id`);`

### xp103-import-xlsx
- convert yaml input into a _sequential-json_.
- _sequential-json_: only 1 level with {edition, h1, h2} see ex below.


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

> If it says something like the following, it worked:

  - see https://kbroman.org/github_tutorial/pages/first_time.html

## YAML input


```
#xp103-import 1 Ultimheat commercial test 2019.xlsx
---
- edition: u2013_fr
  path: u2013_fr
  lang: fr
  yp: 2013
  publisher: Ultimheat

- h1: Thermostats électromécaniques et électroniques  pour intégration
  path: u2013_fr.1
  lang: fr
  pic: Cat1 C1 Ultimheat FR 20190513

- h2: Sommaire
  path: u2013_fr.1.1
  lang: fr
  pic: Cat1 C1 Ultimheat FR 20190513
  url: Cat1 Ultimheat FR C1-P2 20190513

- h2: Introduction à la technologie des thermostats
  path: u2013_fr.1.2
  lang: fr
  pic: Cat1 C1 Ultimheat FR 20190513
  url: Cat1 Ultimheat FR P3-P29 20190513

```
