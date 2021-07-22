# Cribl Pack for Syslog Input
----

This Pack enables a variety of functions when LogStream is used to receive data from Syslog senders.  

The pack provides the following benefits:
* Provides a pipeline for use as an Input Pre-Conditioning pipeline
* Volume reduction by removing redundant information, such as the human-readable timestamp.
* Timezone normalization, when senders do not include timezone information
* Lookup-based enrichment to set additional meta-information for a given sender.  Examples include index, sourcetype, and time zone.

The pack also includes a variety sample files for use when evaluating which features to enable/disable for a given customer deployment.

This Pack's pipeline is intended for use with general-purpose ingest of data from Syslog senders, by way of an Input Conditiong pipeline. It is used when the sender is using the _Syslog protocol_ to send to LogStream, as opposed to data in a syslog-compliant format being delivered via another delivery method. 

*Input conditioning pipelines* are particularly suited to general actions that one _always_ wants to take for a given input source.  They generally should _not_ be used for specific data sets; pipeline manipulation for a given data set should be acheived within a pipeline specifically tailored to that data set.

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


## Release Notes

### Version 1.0.0 - 2021-07-29
Initial release of the Cribl Syslog Pre-processing pack.

## Contributing to the Pack
To contribute to the Pack, please do the following connect with Michael Donnelly on the Cribl Community Slack.  You can suggest new features or offer to collaborate.

## License
This Pack uses the following license: [`Apache 2.0`]
(https://github.com/criblio/appscope/blob/master/LICENSE).
