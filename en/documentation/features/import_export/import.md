---
layout: home
title: Feature - Import
sidebar: features
lang: en
subnav: do_import
---
---

<div class="alert alert-warning">
<p>This functionality is no longer available since version 2.3</p>
</div>

# Thelia imports

## How to create an import ?

For an import, you have to create a class that extends Thelia\Importimport\Import\ImportHandler  and implement the methods:

```php
protected function getMandatoryColumns();
public function retrieveFromFormatterData(FormatterData $data);
public function getHandledTypes();
```

```getMandatoryColumns``` must return an array with the name of mandatory columns. If you're doing a couple import/import, it should have the same values as your mandatory import aliases.

```retrieveFromFormatterData(FormatterData $data)``` is the method were you must put your import logic.

```Thelia\Core\FileFormat\Formatting\FormatterData``` is an array wrapper but is not Iterable.
A simple way to treat your data is to do:

```php
public function retrieveFromFormatterData(FormatterData $data) {
    while(null !== $row = $data->popRow()) {
         $this->checkMandatoryColumns($row);

         // Your treatement here
    }
}
```

```getHandledTypes()``` must return an array with handled formatters types.
Example :
```php
return array(
    FormatType::TABLE, // For tabled formats (CSV, ODS, ...)
    FormatType::UNBOUNDED, // For unbounded formats (XML, json, ..)
);
```

## Register an Import

To register an import in a module, you have to edit your Config/config.xml

Your have to add in "imports" a tag with that skeleton:

```xml
<imports>
    <import id="your.import.id" class="Your\ImportHandler" category_id="the.category_id">
        <import_descriptive locale="en_US">
            <title>Your import title </title>
             <!-- you may add an optionnal description -->
             <description> ... </description>
        </import_descriptive>
        <import_descriptive locale="fr_FR">
            <!-- Here's for another locale -->
        </import_descriptive>
    </import>
    <import>
        <!-- here's another import -->
    </import>
</imports>
```

Thelia import categories ids are:

- thelia.import.products : imports concerning Products
- thelia.import.modules : module specific imports

If you want to create a new category, you have to put in your Config/config.xml:

```xml
<import_categories>
        <import_category id="your.category.id">
            <title locale="en_US">A title</title>
            <title locale="fr_FR">Un titre</title>
        </import_category>
        <import_category id="your.other.category.id">
            <!-- here's another import category -->
        </import_category>
</import_categories>
```
