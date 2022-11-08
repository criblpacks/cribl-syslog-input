# Cribl Pack for Syslog Input
----

This Pack enables a variety of Functions when using Cribl Stream to receive data from syslog senders.  The pack is specifically to be used in combination with a Cribl Syslog source; syslog-formatted data arriving via other means are not supported by the pack.

Pack benefits:
* Provides a pre-processing Pipeline for handling Cribl Stream's Syslog Source.
* Reduces volume by removing redundant information, such as the human-readable timestamp (typical reduction is 20-30% overall).
* Normalizes timezones when senders do not include timezone information.
* Adds lookup-based enrichment to set additional metadata for a given sender (e.g., index, sourcetype, and timezone).

This Pack also includes sample logs for evaluating which features to enable or disable for specific customer deployments.

Use this Pack's Pipeline to ingest general-purpose syslog senders' data. The Pack's pre-processing Pipeline is for a sender using the **Syslog protocol** when sending to Cribl Stream. It is not for data in a syslog-compliant format delivered via another method - Splunk forwarder, Elastic Beats agent, etc. 

Pre-processing Pipelines (including this one) are particularly suited to general actions that you **always** want to take for an input Source. Generally, do **not** use this pre-processing Pipeline for specialized datasets. For specialized datasets, you should specifically tailor a Pipeline for that dataset.  

For example, to process all syslog data from all syslog senders on port 514, use a pre-processing Pipeline to normalize the processing of severity and facility information along with basic volume reduction.

When processing a specific dataset, use the pre-processing Pipeline **in combination** with the other Packs, Routes, or Pipelines **specific to that dataset**.  For example, when receiving Palo Alto firewall data sent via syslog, this pack does the pre-processing and the Palo Alto Pack provides additional processing.

## Deploying the Pack into Production

Most Cribl Stream Packs provide a collection of Routes and Pipelines used after the pre-processing phase. This Pack differs in that it also provides a pre-processing Pipeline. Deploying this Pack occurs across three main phases.
 

### 1. Configure the Pack

The Pack modifies the data being delivered by Cribl Stream, and it supports multiple options for doing so. Prior to putting it into production, it is important to review those options and selectively enable desired features or disable undesired features.  Each Function and option is explained within the Pipeline's comments and so those details are omitted here.

Familiarize yourself with the Pack's processing details:

1. Open the Pack's `CriblSyslogPreProcessing` Pipeline.
2. Select a saved sample and review the **IN** and **OUT** versions.
3. Read the Pipeline's comments and enable or disable functions as necessary for the deployment environment.
4. Optionally, use **Sample Data** > **Capture New** with a filter of `__inputId.startsWith('syslog:')` to capture data from a deployed production environment with Syslog Sources.

### 2. Edit the Pack's Lookup File

When enabled, the Pack uses a Lookup file to map hostnames or host IPs to metadata including sourcetype, source, index, or timezone.  The procedures below provide two ways of editing the Lookup file.

#### Edit Using the UI
(Recommended for small number of edits or additions, as with testing.)

1. Click **Knowledge** in the Pack's submenu and click the included Lookup `SyslogLookup.csv`. (This Lookup file's name is pre-configured in the Lookup Functions within this Pack.)
2. Change **Edit Mode** to `Text` to edit the metadata column names.  Append any additional metadata fields to the first row.
3. To understand how the rows of the Lookup file work, review the comments in the first lines of the Lookup file.  
4. Resume editing by changing **Edit Mode** to `Table`. Any added metadata columns will display.
5. The saved samples included in the Pack reference hosts on lines 3-7. You can remove these lines for deployments where external information is not allowed. Note that the Pipeline's preview mode will no longer show the Lookup results when looking at those samples.
6. Click **Add Row** and provide information for a host.  Repeat as needed.
7. Click **Save**.
8. Return to the Pipeline to test Lookup Functions against the edited file.
9. Click **Commit & Deploy** when testing is complete.

#### Edit Using an External Spreadsheet
(Recommended for bulk updates.)

1. Click **Knowledge** in the Pack's submenu and click the included Lookup `SyslogLookup.csv`. (This Lookup file's name is pre-configured in the Lookup Functions within this Pack.)
2. Change **Edit Mode** to `Text` to view the file in `.csv` format.
3. Copy the entire contents and paste to a new file named `SyslogLookup.csv`.
4. Edit the file in your favorite spreadsheet tool, adding or editing columns as desired.
5. Upload your edited `.csv` file. Click **Knowledge** in the Pack's submenu, click `SyslogLookup.csv`, click **Reupload**, navigate to your `.csv` file, and then click **Open**.
6. Return to the Pipeline to test Lookup Functions against the edited file.
7. Click **Commit & Deploy** when testing is complete.
For future bulk edits, repeat steps 4, 5, and 7.

### 3. Tie the Pack to Syslog Sources

To this point, you have reviewed and prepared the Pack for use. In this step, we enable it by tying it to one or more Syslog Sources.

To set the Pack's Pipeline to use your Sources:

1. In the Worker Group's **Manage Sources** page, select a Syslog Source where you desire pre-processing - probably, most of them.
2. In the **Pre-Processing** tab, select the `[Pack] cribl-syslog-input (syslog pre-processing)` Pipeline.
3. Repeat steps 1 and 2 for additional Syslog Sources as needed.
4. Click **Commit & Deploy** when testing is complete.

## Upgrading this Pack

Upgrading certian Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.

Because this Pack has user-modified items (Pipelines, Functions, and Lookups), Cribl recommends that you install future versions with a unique Pack ID by appending the version number to the Pack ID during import. This allows side-by-side comparisons as you configure the updated version.

Once the update is imported, configure the newly installed Pack:

1. Review the Pipeline's Functions, enabling and disabling functions as needed.
2. Edit the Lookup's `.csv` by clicking **Reupload** or by copying the `.csv` from the previous version and pasting in the new version.
3. Update the Syslog Sources **Pre-Proccessing** tab's setting to use the newer Pack version.
4. Click **Commit & Deploy** when testing is complete.


## Release Notes

### Version 1.3.0 - 2022-10-27

* Re-grouped timezone detection from Message field for easier on/off management
* Auto detection of ISO 8601 time stamps, to avoid unnecessary timezone calculations
* Handling of timestamps from within the message field now support timezone lookups.
* Updates in comments to replace "LogStream" with "Stream"
* Verified support for Cribl Stream 4.0

### Version 1.2.3 - 2022-08-10

* Updated README for added clarity during installation and upgrades.

### Version 1.2.2 - 2022-07-21

* Added a `C.Lookup` option for returning timezones.

### Version 1.2.1 - 2022-07-12

* Changed catch-all Route (used when Source is not Syslog) to use a passthru Pipeline and default Destination.

### Version 1.2.0 - 2022-07-11

* Resolved an issue where severity or facility were preserved unintentionally when the value is 0.
* Added an option to perform a lookup using an Eval Function instead of a Lookup Function.
* Minor improvements to the order of processing for missing metadata fields.
* Improved comments to indicate which settings are disabled by default.

### Version 1.1.4 - 2022-03-30

* Added metadata for the `packs.cribl.io` suite.
* Added sample files for Ubiquiti routers.
* Updated minimum version of Cribl Stream to 3.4.0.

### Version 1.1.0 - 2021-11-18

* Increased volume reduction when an event contains multiple timestamps by removing the second timestamp.
* Improved commenting throughout.
* Added log samples from networking gear in an older syslog format.
* Improved metadata lookup attempts - including hardcoded values if all other approaches fail.

### Version 1.0.0 - 2021-07-29

The initial release of the Cribl Pack for Syslog Input.

## Contributing to the Pack

To contribute to the Pack, please connect with Michael Donnelly on [Cribl Community Slack](https://cribl-community.slack.com/). You can suggest new features or offer to collaborate.

## License
---
This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
