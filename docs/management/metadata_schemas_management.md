---
layout: page
title: Metadata Schemas Management
parent: Management
nav_order: 0
---
# Metadata Schemas Management

Digital Preservation (DP) Program utilizes customized metadata schemas in
the digital repository, Preservica, in order to connect ingested assets with
other systems used at the Library. We are storing minimal metadata, essentially
the system identifiers, so as to reduce repetitive work, because the existing
systems are used to provide descriptive metadata for our collections.

The system identifiers we store in Preservica metadata fragments will assist
us to create relationships and connect ingested assets with other databases
such as ArchivesSpace, SPEC (NYPL's collection management system), Sierra,
and other identifiers like barcode, division code, etc.

As the metadata schemas are customized, and are updated with time, DP Program
keeps track of the schema documents we use in a private GitHub repository,
[NYPL/prsv-schemas](https://github.com/NYPL/prsv-schemas).

## Preservica metadata schema documents

There are three types of Preservica metadata schema documents: XML Schemas, XML
Transforms and XML Documents. They are used in different ways in the software.
Below information presents their different uses and purposes.

### XML Schemas
xsd
"Preservica allows XML schemas (XSD documents) to be registered with the system. Registered schemas can then be used to validate both submissions and uploaded documents for compliance with the schema."

### XML Transforms
editor: xslt
viwer: xslt

"Preservica allows XML Transforms (XSLT documents) to be registered with the system. Registered transforms can then be used to convert the format of XML metadata held within Preservica to another XML schema."

### XML Documents
indexer: xml
template: xml

"Preservica allows XML documents to be registered with the system; these documents are valid fragments of XML."
"Metadata Template XML documents can be used within Explorer to provide a standard template to add blocks of additional metadata to an entity (folder or asset)"

"Custom Index Definition documents define a custom search indexer for generic metadata. These documents should fit the Custom Indexer schema."



## Preservica metadata schemas management instructions

[Preservica's Application Programming Interface](https://nypl.preservica.com/api/documentation.html)
(API)
