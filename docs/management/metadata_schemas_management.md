---
layout: page
title: Metadata Schemas Management
parent: Management
nav_order: 0
---
# Metadata Schemas Management

{: .no_toc }
&nbsp;
{: .no_toc .text-delta }

1. TOC
{:toc}

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

XML Schemas are XSD (XML Schema Definition) files. Preservica allows these XSD Documents
to be registered with the system and they can be used to validate both submission and
uploaded documents for compliance with the schema.

### XML Transforms

XML Transforms are used for presenting metadata fragments on the Preservica User Interface.
There are two types of XML Transforms: editor and viewer. Both types of files are XSLT
(Extensible Stylesheet Language Transformations) files.

These files are used to convert the format of XML metadata held in Preservica to another XML schema.

### XML Documents

XML Documents are valid fragments of XML. There are two types of XML Documents in Preservica:
template and indexer. Both types of files are XML files. Template documents can be used within
Preservica User Interface to add blocks of additional metadata to an entity (folder or asset).
Indexer documents are used to define a custom search indexer for generic metadata. They should
fit the Custom Indexer schema.

## Schemas management instructions

[Preservica's Application Programming Interface](https://nypl.preservica.com/api/documentation.html)
(API)
