---
layout: post
title:  "ePADD and Archivematica"
date:   2018-11-02 10:00:00 -0400
categories: sip AIP DIP OAIS Archivematica ePADD
author: Nick Krabbenhoeft
---

## ePADD and Archivematica: IPs, IPs, and IPs
I'm a very big fan of using OAIS to model workflows. Get some SIPs. Make some AIPs. Deliver some DIPs. It's all so easy, until you start trying to do it with actual tools. While working with ePADD and Archivematica recently, I ran into that theory->practice barrier difficulty again.

If you haven't heard of ePADD, it's an amazing tool built at Stanford to work with email archives. The workflow is basically

1. Load 1 or more email inboxes into the Appraisal module.
2. Appraise, redact, accession, describe, process correspondence using Appraisal and Processing modules.
3. Allow users to search, browse, and export processed emails using the Discovery and Delivery modules.

And making all of that possible for inboxes that may contain millions of messages are some nifty tools like entity extractors, PII identifiers, and attachment browsers.

If I were to OAISify ePADD, each email account loaded into Appraisal is a SIP. Sets of correspondence loaded into Discovery and emails exported through Delivery are DIPs. But where's the AIP?

The Processing module creates a set of files that could be treated as an AIP, but the ePADD system does not have storage functionality built in to do things like monitor fixity or manage location. It also isn't possible to create a policy for AIP structure, file formats, or accompanying PDI. And that's perfectly fine since there are other tools that do that. The challenge is how to integrate ePADD with those tools.

For example, if you use Archivematica to produce an AIP, how would handle a processed ePADD package? And how would ePADD load an Archivematica AIP into its Discovery and Delivery modules? The ePADD team was testing those questions out, and NYPL contributed.

## OAIS Time

Since everyone uses OAIS a little differently, I'll define how I use them here at NYPL.

1. Content Information - the target of preservation

2. PDI - Preservation Description Information, information the supports the identification, preservation, and understanding of Content Information.

3. IP - Information Package, a group that includes Content Information and/or PDI

4. SIP - Submission Information Package, the objects that we receive as part of an archival acquisition, digitization project, or another process. In this case, each inbox is a SIP.

5. AIP - Archival Information Package, the material that the Library has accessioned and commits to preserve for long-term access. In this case, the package poduced by Archivematica is an AIP.

6. DIP - Dissemination Information Package, the material that the Library makes available to its users. In this case, ePADD's Discovery and Delivery modules create DIPs.

7. Ingest - The set of processes that produces descriptions and AIPs from SIPs. It can include augmenting, combining, splitting, deleting, or transforming one-or-more SIPs. In this case, ePADD's Appraisal and Proceesing modules and Archivematica are all part of ingest, particularly the Receiving SIP, Generating AIP, and Generating Description functions.

I think it's important to define my usage of these terms because they can be at odds with how the tools use those definitions. For example, in Archivematica every submitted package is a transfer, every transfer is transformed into one-or-more SIPs, and every SIP is transformed into an AIP. But I think of the original submission to ePADD as a SIP, so going by my tools' terms a workflow would look like this: SIP -> Transfer -> SIP -> AIP. That's confusing and limits how I can conceptually link these tools together.

For me, in this case and most other worfklows, Archivematica is receiving an IP that is somewhere between a SIP and an AIP.

## ePADD IP and Archivematica Expectations

Back to the integration work. ePADD takes one or more inboxes and turns them into an IP with the following directory structure.
* user
	* blobs - directory for attachments
	* indexes - directory for indexes created to search email and attachments
	* lexicons - directory of terms used to classify emails
	* sessions - directory of added mappings like VIAF IDs and annotations

We would like to put that IP into Archivematica, but Archivematica has expectations about how any IP is structured.
* directory
	* objects - digital objects to be preserved
	* metadata - metadata about digital objects to be preserved
		* submissionDocumention - any documents about the acquisition of records
	* logs - metadata about processes performed on digital objects

In Archivematica, /objects contains Content Information and /metadata and /logs contains PDI. That's a key piece of information because Archivematica runs some microservices on everything it thinks is Content Information, and generally maintains PDI as is.

For example, if you rename ePADD's /user as /objects and send it to Archivematica, all of the indexes, lexicons, and sessions will be treated as Content Information. If you apply file naming or normalizing rules to those files, they would be rendered useless for the ePADD Discovery and Delivery modules.

On the other hand, if we treat those directories as PDI describing the context of the emails and attachments, they should be placed in /metadata and would remain untouched by Archivematica microservices. And then moving the /blobs to /objects allows us to apply preservation policies to any attachments.

To do this, all we need is a script that translates the ePADD IP to the Archivematica IP. That looks like [this](https://github.com/NYPL/amatica-epadd/blob/master/epadd-amatica-restructure.py). Note, we were experimenting with bags, so this script also restructures the bag so that Archivematica can read the checksums and validate the Content Information.

## Archivematica AIPs and ePADD DIP Expectations
After translating the directory structure, Archivematica processes the ePADD IP into an AIP, and sends it to the Storage Service. That's great, but only half of the journey. To go from the Storage Service back to ePADD, we need to translate the Archivematica AIP back into an IP that ePADD can understand. Since the Archivematica AIP is similar to its transfer requirements, doing translating back can be accomplished by doing the [same steps in reverse](https://github.com/NYPL/amatica-epadd/blob/master/amatica-epadd-restructure.py), for the most part.

There are several files and directories created by Archivematica that I left in their original position. That's fine, since ePADD is unaware of what most of those files are. You might even take the step of deleting those files since an end user may have no interest in them. It's not a permanent deletion since we're working with a copy of the AIP.

The most important file is the METS file, which records any changes made to the Content Objects by Archivematica. This may include file name change like replacing spaces with '\_' or file format changes like converting a PICT image to a BMP.

## So what?

So far, these scripts translate from ePADD IP to Archivematica IP and back, but this isn't just shuffling a deck of card for the fun of it. Archivematica can transform the Content Information in a number of ways, and all of those changes are logged in the Archivematica METS file.

By selectively applying Archivematica to the attachments stored in /blobs, we can address preservation risks in those attachments. The ePADD team has written code that parses out what preservation actions were taken and can then present both the original and normalized files. If you set a policy in Archivematica to migrate a Wordstar document to PDF, when you load the transformed AIP into ePADD, it will update its indexes to display the migrated version of the object. Very cool!

## Challenges Remaining

This work is not wrapped up. It only covers the use case where Archivematica is normalizing formats for preservation. I'd like to add the ability to use file normalized for access, but that requires documenting how the Archivematica AIP stores those files.

It also needs some more documentation. Several options in the Archivematica workflow must be configured in specific ways. For example, Archivematica should not unpack any compressed packages, since ePADD doesn't index the contents of compressed packages. I've included a MCP XML file with the correct configuration, but it would be more useful to include which of those choices are required.

There's also the matter of adding this to automation tools. That would require identifying some heuristics to identify ePADD bags and applying the right scripts.

If you have any questions about this work, please contact me through email or the Github repository. One of the most important things that this work helped me realize is how fixity information can be passed into and out of Archivematica, which is something that I'll write about in the future.