# Cribl Pack for Syslog Input
----

This Pack enables a variety of functions when LogStream is used to receive data from Syslog senders.  

The pack provides the following benefits:
* Provides a pipeline for use as an Input Pre-Conditioning pipeline
* Volume reduction by removing redundant information, such as the human-readable timestamp. Typical reduction is 20-30% overall.
* Timezone normalization, when senders do not include timezone information
* Lookup-based enrichment to set additional meta-information for a given sender.  Examples include index, sourcetype, and time zone.

The pack also includes sample logs for use when evaluating which features to enable/disable for a given customer deployment.

This Pack's pipeline is intended for use with general-purpose ingest of data from Syslog senders, by way of an Input Conditioning pipeline. It is used when the sender is using the _Syslog protocol_ to send to LogStream, as opposed to data in a syslog-compliant format being delivered via another delivery method. 

*Input conditioning pipelines* are particularly suited to general actions that one _always_ wants to take for a given input source.  They generally should _not_ be used for specific data sets; pipeline manipulation for a given data set should be achieved within a pipeline specifically tailored to that data set.

For example, to process all Syslog data from all Syslog senders on ports 514, and XYZ, use an input conditioning pipeline to normalize processing of Severity and Facility information, and for basic volume reduction.

When processing a specific data set, such as Palo Alto firewall data sent via Syslog, the input conditioning pipeline is used _in combination_ with the normal routes+packs (or routes+pipelines) approaches.  The Palo Alto pack would do any additional processing _specific to that dataset_.  

## Using The Pack

Most Cribl LogStream Packs provide a collection of routes and pipelines that are used after the Pre-Processing phase.  This pack is different, in that it provides a Pre-Processing pipeline.  

To use this Pack happens in these main phases: 

_Understanding the pack_
1. Open the Pack's CriblSyslogPreProcessing pipeline
2. Select a saved sample, and review _IN_ and _OUT_ versions
3. Read the pipeline's comments and enable/disable functions as necessary for the deployment environment.
4. Optional: To capture data from a deployed production environment with Syslog sources, use Sample Data -> Capture New with a filter of `__inputId.startsWith('syslog:')`
5. Proceed to the next phase when the steps above are complete.

_Editing the Knowledge Object_
1. Within the pack, select Knowledge and edit `SyslogLookup.csv`
2. Add host information from the deployment environment.
3. Optionally, remove lines 4,5,6
4. Click Save

_Tying the pipeline to sources_
1. In the Worker Group's Sources page, select a Syslog source where PreProcessing is desired.  (Probably, most of them.)
2. In Pre-Processing, select "[Pack] cribl-syslog-input
3. Repeat steps 1 and 2 for additional syslog sources as needed.


## Upgrading Packs
When upgrading this or any pack, it is recommended to
* Import the updated pack under a new name that includes the version. Example: `cribl-syslog-input-120`.  This allows you to review and adjust new functionality against currently-deployed configurations.
* Copy any modified lookup files from the previous version of the pack over to the newly installed version.  (Skip this step if lookups were not modified for your environment.)
* Review all comments in the new pack, and enable/disable functions as necessary.  You may find it useful to reference previous and new versions of the pack side-by-side.
* Update routes / pipelines / sources / destinations that use the previous pack to reference the new pack instead
* Test, test test
* Commit / Deploy 

## Release Notes

### Version 1.2.1 - 2022-07-12
1. Changed catch-all route (used when Source is not syslog) to use passthru pipeline and default destination.

### Version 1.2.0 - 2022-07-11
1. Resolved an issue where facility or severity were preserved unintentionally when the value is 0
2. Added an option to perform lookup using Eval function instead of Lookup function
3. Minor improvements to the order of processing for missing meta fields
4. Improved comments to indicate which settings are disabled by default

### Version 1.1.4 - 2022-03-30
1. Added metadata for packs.cribl.io suite
2. Added sample files for Ubiquiti routers
3. Updated minimum version of Stream to 3.4.0

### Version 1.1.0 - 2021-11-18
1. Increased volume reduction when event contains multiple timestamps, by removing second timestamp
2. Improved commenting througout
3. Added log samples from networking gear in an older syslog format
4. Improved metadata lookup attempts, now including hardcoded values if all other approaches fail.

### Version 1.0.0 - 2021-07-29
Initial release of the Cribl Syslog Pre-processing pack.

## Contributing to the Pack
To contribute to the Pack, please connect with Michael Donnelly on the Cribl Community Slack.  You can suggest new features or offer to collaborate.

## License
---
This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
