## jpc-catalogs-admin

### Architecture
##### object-types:
- _edition_ : content-item stored at level 1, ex: "u2013_fr" {company/publisher}
- _volume-cat_ : content-item representing a catalog : {yp, lang, edition}
- _section-pdf_ : content-item - a pdf file.
- _pdf-page_ : txt record - a page within a pdf file => {pageno, lang, fti, raw_text}
##### hierarchies:
- _package_id_ : all items belonging to an app-instance.
- _context_id_ : used for permission system.
- _parent_id_ : customized for each object-type.


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
- convert xlsx input into a _sequential-yaml_.
- _sequential-yaml_: only 1 level with {edition, h1, h2} see ex below.

### xp104-list-app-instances
- find all app-instances and associated app-folder : package_key = 'cms'
- and at root level : (parent_id = -100)
- note: a package can be deployed at any node in openacs.

### xp106-clean-app-instance
- remove all content-items (cr_items, cr_revisions, and txt)

### xp107-catalog-directory
- list all _volume-cat_ found for a given _edition_.

### xp108-pages-directory
- list all pdf pages found in a given app-folder
- apply a filter to get only what is related to a language.
- apply a filter on path.

### xp109-mk-pdf-pages
- given an app-instance and a path
- split pdf-file into pages using pdflib => raw-text.
- create a txt record for each pdf-page
- postgres has a trigger to create FTI on each raw-text

```
- for each section-pdf found:
  - locate pdf file in specified folder.
  - check fsize and mtime
  - if unchanged and no 'force' => continue next pdf.
  - remove existing txt (pages)
  - split pdf into pages, extract raw text.
  - insert a txt record for each page, a trigger creates the FTI.
  - commit data : a new revision for section-pdf with updated {fsize, timestamp}
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

> If it says something like the following, it worked:

  - see https://kbroman.org/github_tutorial/pages/first_time.html

## _sequential_ YAML input


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
