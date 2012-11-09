DPLA Appfest Drupal integration
===

Idea
---

Drupal module or distribution

Your Name: Nate Hill

Type of app: Drupal CMS

Description of App: Many. many libraries choose to use Drupal as their content management system or as their application development framework. A contrib Drupal module that creates a simple interface for admin users to curate collections of DPLA content for display on a library website would be useful.

Workflow
---

Preamble:

I don't like recreating the wheel. So, let's see what contrib modules already exist, and see if we can just create a workflow to do this to start with. It would be really nice if DPLA had a OAI-PMH provider, then you could just use CCK + Feeds + [Feeds OAI-PMH](http://drupal.org/project/feeds_oai_pmh).

Requirements:

  * [CCK](http://drupal.org/project/cck)

    `drush pm-download cck`

  * [Feeds](http://drupal.org/project/feeds)

    `drush pm-download feeds`

  * [Feeds - JSON Parser](http://drupal.org/project/feeds_jsonpath_parser)

    `drush pm-download feeds_jsonpath_parser`
    `cd sites/all/modules/feeds_jsonpath_parser && wget http://jsonpath.googlecode.com/files/jsonpath-0.8.1.php`

Setup:

* Create a Content Type for the DPLA content you would like to pull in (admin/content/types/add) ![CCK Content Type](http://i.imgur.com/Eh1ae.png) 
* Create DPLA metadata fields for the Content Type (admin/content/node-type/YOURCONTENTYPE/fields) ![CCK DPLA Field Types](http://i.imgur.com/TK2Pu.png) 
* Create a new feed importer (admin/build/feeds/create)
* Configure the settings for you new feed importer
  * Basic settings:
    * Select the Content Type you would like to import into
    * Select a fequency you would like Feeds to ingest ![Feeds Import - Basic Settings](http://i.imgur.com/wfC3l.png)
  * Fetcher
    * HTTP Fetcher
  * Processor
    * Node processor
    * Select the Content Type you created
    * Mappings (create a mapping for each metadata field you created)
      * Source : jsonpath_parser:0
      * Target : Title ![Feeds Import - Processor(http://i.imgur.com/PZTDP.png)
  * Parser
    * JSONPath Parser
    * Settings for JSONPath parser
      * Context: `$.docs.*` ![Feeds Import - JSONPath](http://i.imgur.com/xsfHJ.png)
* Construct a search you would like to ingest using the [DPLA API](https://github.com/dpla/platform/wiki)
  * ex:  `http://api.dp.la/v1/items?dplaContributor=%22Minnesota%20Digital%20Library%22`
* Start the import! (node/add/YOURCONTENTTYPE)
* Give the import a title... whatever your heart desires.
* Add a feed url
* Click on JSONPath Parser settings, and start adding all of the JSONPaths ![Node/Feed add](http://i.imgur.com/WCzzd.png)
* Click save, and watch the import go. ![Import success](http://i.imgur.com/mjNEe.png)
* Check out your results 
![Record](http://i.imgur.com/6AuEH.png)
