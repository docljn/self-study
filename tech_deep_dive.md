# Tech Deep Dive

## The Brief

> "The interviewer will want to hear about an interesting project you worked on (perhaps one that you're particularly proud of). You'll have the chance to take the interviewer through a deep dive into your project; highlighting the complexities, your role, the scope of decisions you made, trade-offs, and any other important impact the project had."

## The Project: replacing the data source for Id-Mapping

### What?

Replace the api-based retrieval of records from PubMed with a local data store.

### Why?

In late April 2022, we received an email informing us that the PubMed API was in a testing phase for their planned upgrade. We hadn't previously been aware of this, which meant a hurried investigation into the potential impact of the upgrade.

The critical change was that PubMed were moving to a setup which meant that all queries to the api would be restricted to the first 10,000 records returned, and our implementation relied on receiving all matching responses which could be in excess of 40,000 records depending on the query.

Incomplete record sets would result in an inability to match a "mention" of a research output with the relevant published research, meaning that our users would have an inaccurate picture of the reach of their output. This is because the PubMed data provides a reliable way to link the title, authors, source, and various reference ids of a research output.

PubMed suggested that we create a local copy of their data (in effect a mirror) as most of their power users do, making use of the ftp facility they provide.

### Risks

Doing nothing would mean that when the API upgrade was released we would no longer be able to provide a complete catalogue of mentions for any research output, resulting in a loss of trust from our key academic and institutional customers.

### Investigation

PubMed provide an annual baseline dataset in mid-December, followed by ~daily update files.
I confirmed that the result of the annual baseline plus update files is NOT the same as the new annual baseline dataset, meaning that the baseline must be imported each year to ensure accurate data.
Fortunately, the file structure in the compressed files available by ftp was identical to that returned from the api queries, meaning that a new XML parser/mapper was not required.
I also confirmed that it would be possible to retain the api-based query functionality in order to handle the situation where a reference id is not present in the local data store due to relative timings of updates.
I also confirmed, after checking the ftp data, that the timestamps referenced were not UTC, and the key information was the sequential file name: data must be imported in sequence for creation, update and (permanent) deletion purposes

### Implementation

- in order to reduce the risk, it was decided to retain the same local data structure (a subset of the full pubmed data per record) rather than build a complete local mirror
  - the disadvantage is that should additional fields become useful, the full data set would need to be reimported
  - the advantage is that none of the dependent code or processes would need to change, and the fewer changes, the lower the risk
- runtime modification of the `store_in` collection for the pubmed records and import history to allow for the current api-based implementation to continue to function as the source of data while the baseline dataset and subsequent update files were being imported (the baseline was 1114 gz compressed xml files and took a while to process)
  - this will also allow for the annual baseline dataset switch, as queries will need to run on the current year's data while the new set is imported
- pubmed client extended to handle ftp (Net::FTP) as well as http requests
- bulk import class created to handle sequential file import, unzip (Zlib::GzipReader), validation (md5 checksum), and cleanup as well as errors/retry logic in the file import process
- existing xml parsing class extended to handle record deletion (which had previously been ignored, and likely been a source of some of the data quality issues we were seeing)

### Outcomes

- soft launch with no customer impact a week before the api change was due to go live
- documentation outlining the process for the annual dataset switchover
- monitoring switched to new mongodb collections
- zombie code in two separate repositories identified (it was making direct queries to the mongodb collection rather than via the idmapping project, or would have been if it were ever actually used) and removed
- an entire repository identified as no-longer required (had been a proof of concept) and archived just in time to prevent a colleague spending time on upgrading the ruby and rails versions involved

### For next time

- remember that although mongoid has a way to specify indices in the model class this isn't applied until you run the rake task
- would not change the approach (using the existing xml mapping class) but would have liked to have time to investigate the possibility of moving to Ox (a SAX-based XML parser) to avoid building the entire file tree before parsing (memory intensive)

## The Documentation: Use FTP as the source of data for the Altmetric PubMed Integration

### Context

<https://digital-science.atlassian.net/browse/ICM-104>

#### Why do we need a PubMed Integration?

The Altmetric pipeline uses a number of different centrally-provided document identifiers as part of the process to link a potential mention to a research output and enrich the data held on that research output. The full list of identifiers which are cross-referenced in the process of matching mentions and research outputs is listed in the altmetric-id-mapping README.md. PubMed is one provider of these document identifiers.

The PubmedIdentifierMapping records stored in mongodb contain a subset of the information provided by PubMed about each research output. These records must be kept current in order to ensure that mentions are correctly matched with research outputs.

#### Why was a change necessary?

The automated daily process to keep the local pubmed records in sync with the original data relied on querying the PubMed E-utilities API for a list of identifiers which had been created or updated over the previous 24 hours. The number of PubMed identifiers processed by Altmetric regularly exceeded 10,000 in a 24 hour period.

PubMed announced an E-utilities API change to limit the response to the first 10,000 records.

### Solution Selected

Replace the current PubMed integration with one based on bulk download of files by ftp, maintaining a local copy of the attributes useful to Altmetric in exactly the same format and data store as is currently supported by the API-based integration

Retain the PubMed E-utilities API integration for ad-hoc queries.

#### Justification

This guarantees a complete and correct local collection of PubMed records, and partly complies with PubMed’s suggestion that Altmetric (as a power user of their data) maintain our own mirror.

#### Consequences

There will need to be an annual switchover from one baseline file set to the next, following the process outlined by PubMed, to ensure that the local data continues to be correct and complete.

### Annual Switchover Process

PubMed provide [online documentation](https://www.nlm.nih.gov/databases/download/pubmed_medline_documentation.html) as well as regular updates via the [NLM Technical Bulletin](https://www.nlm.nih.gov/pubs/techbull) and the [EUtilities listserv](https://www.ncbi.nlm.nih.gov/mailman/listinfo/utilities-announce/) on the anticipated dates and effects of the [PubMed end of year processing](https://www.nlm.nih.gov/bsd/policy/yep_background.html).

Once PubMed have confirmed that the new annual baseline dataset is available:
- Amend the daily import job to use the current (“pubmed22n”) sequence for the pubmed_id_mapping_collection and the pubmed_import_log_collection so that the updatesets do not affect the new baseline data
- Amend the rake task pubmed:import_baseline to set the correct Thread variables so that the new baseline filesets can be stored locally without affecting the data queried by the altmetric pipeline
  - a new mongodb collection for the pubmed_id_mapping_collection “pubmed23n”
  - a new mongodb collection for the pubmed_import_log_collection “pubmed23n_import_logs”
- Confirm that the data structure used in the PubMed XML has not changed by comparing the sample data file with the local test fixtures in “spec/fixtures/pubmed”
- Create indices on the two new mongo collections to match those on the currently-in-use collections (Jira ticket for what to do if you forget and need indices added to populated collections)
- Test the import on staging (!) (and remember the indices on staging AND production!)
- Import the full baseline file set: historically this has taken up to 10 minutes per compressed file
- Amend the daily import job to use the new (“pubmed23n”) sequence for the pubmed_id_mapping_collection and the pubmed_import_log_collection
- Amend the “store_in collection:” defaults for both PubmedIdentifierMapping and PubmedImportLog records to use the new mongodb collections (historic PR)
- Amend the PubmedRecord mongodb collection in altmetric-afi-connectors to use the new mongodb collection (historic PR)
- Check if any other applications are querying the mongodb collections directly and amend
- Confirm that monitoring (sensu?) is functioning on the new collections

### Options Considered

- Do nothing: this option was discounted as it increases the risk of missed and/or mismatched mentions, and loss of customer trust, as the local records will be incorrect.
- Request the ids of updated PubMed records from the entrez-tools api endpoint at shorter intervals than the current 24h: this option was discounted as the smallest interval that PubMed support is one day meaning we have no way to request smaller update sets
- Request the ids of new records and updated records from PubMed in two separate queries: this option was discounted as both sets often exceeded 10,000 records/day in May 2022
- Build & Maintain a full PubMed Mirror based on bulk download of files by ftp which would fulfil the suggestion by PubMed that power users maintain a mirror. It was discounted as it would require a completely new data store and would involve retaining irrelevant data
- Replace the current PubMed integration with one based on bulk download of files by ftp, maintaining a local copy of the attributes useful to Altmetric in exactly the same format and data store as is currently supported by the API-based integration. Retain the PubMed E-utilities API integration for ad-hoc queries when the PubMed identifier cannot be found in the local copy: selected as the best solution from those considered
