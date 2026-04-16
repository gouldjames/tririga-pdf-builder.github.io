# TRIRIGA PDF Builder User Guide

## TRIRIGA Fields
### Normal field, directly from the source business object
* `${triNameTX}`
* `${triIdTX}`
* `${triCustomerOrgTX}`

### Normal field from a named section in the source business object (including smart sections)
* `${RecordInformation::triIdTX}`
* `${triPeopleResponsible::triIdTX}`

*Note - when there are multiple sections containing the same field name, you **must** specify the section name. This is common for fields like triIdTX, triNameTX and triPathTX.*

### Truncating fields
The following syntax can be used to truncate the value of the field (in this case triNameTX) to the specified number of characters.
This can be useful to provide more control over the document layout.

* `${triNameTX(50)}`
* `${cstExternalNumberTX(20)}`

### Locator Fields
Locator fields are linked to another record in TRIRIGA. The following mapping can be used to display data from the record linked in the locator field
* `${triCustomerOrgTX[triNameTX]}`

In this example, value of `triNameTX` is mapped from the Organization record that has been mapped into `triCustomerOrgTX`

### Associated Fields
The following JSON definition can be used to map data from associated records.
When possible, Locator Field mappings are preferred for the following reasons:
1. It gives control over which record is mapped (when there are more than one associated records, 
the association mapping will just get the first associated record that meets the definition)
2. Locator fields are able to get the data from the database without using `ibs_spec_assignments` (which is more efficient)

```json
${
	"mod":"triAsset",
	"bo":"",
	"ass":"Requested Assets",
	"field":"triIdTX"
}
```
#### Notes
1. You must prefix the JSON with `$`
1. If no value is provided for `bo`, any associated record from the specified module will be used. Module must be populated.
1. `ass` is the Association String used to fetch the desired record

### Limitations
Not all combinations of these mappings have been tested. Combining some of this syntax to create complex mappings may not work.

## Queries / Tables
TRIRIGA Queries are added to the document output using tables in the Word document template.
1.	Insert a table into the document with the appropriate number of columns and two rows
1.	The first row will be the header row containing the column names and the second row is a placeholder for the data / TRIRIGA output
	1.	**The column names must be the same as the column Labels in the TRIRIGA Query Builder**
	1. The placeholder cells **must** contain text – this is used to style the output from TRIRIGA. 

	![Example Table](images/example_table.png)
 
1.	Make sure your table is selected, then, in the Ribbon, select Table Layout
	1.	It can be useful to turn on View Gridlines if your table does not have visible borders – this is especially useful if you're using nested tables
1.	Click Properties

	![Word Ribbon - Table Layout Tab](images/ribbon_table_layout.png)
 
1.	On the Alt Text tab, put the JSON query definition in the Table Description

```json
{
  	"module": "triItem",	
  	"bo": "cstQuoteLineItem",
	"query": "cstQuoteLineItem - Query - cstQuote - Has Line Item cstQuoteLineItem"
}
```

![Table Properties](images/table_properties.png)

### Notes
Tables can be used to control the document layout *without* mapping a TRIRIGA Query. When the Table Description is *not* set,
the columns and rows are fixed and the normal field mappings can be used to populate the table with data

