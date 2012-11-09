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

(It would be really nice if DPLA had a OAI-PMH provider, then you could just use CCK + Feeds + [Feeds OAI-PMH](http://drupal.org/project/feeds_oai_pmh))

Requirements:

  * [CCK](http://drupal.org/project/cck)

    `drush pm-download cck`

  * [Feeds](http://drupal.org/project/feeds)

    `drush pm-download feeds`

  * [Feeds - JSON Parser](http://drupal.org/project/feeds_jsonpath_parser)

    `drush pm-download feeds_jsonpath_parser`
    `cd sites/all/modules/feeds_jsonpath_parser && wget http://jsonpath.googlecode.com/files/jsonpath-0.8.1.php`

Setup:

1. Create a Content Type for the DPLA content you would like to pull in (admin/content/types/add)
2. Create DPLA metadata fields for the Content Type (admin/content/node-type/YOURCONTENTYPE/fields)
  
  * title
  * description
  * creator
  * type
  * publisher
  * format
  * rights
  * contributor
  * created
  * spatial
  * temporal
  * source

4. Create a new feed importer (admin/build/feeds/create)
5. Configure the settings for you new feed importer
  
  * Basic settings:
    * Select the Content Type you would like to import into
    * Select a fequency you would like Feeds to ingest
  * Fetcher
    * HTTP Fetcher
  * Parser
    * JSONPath Parser
    * Settings for JSONPath parser
      * Context: `$.count.docs.*`
  * Processor
    * Node processor
    * Mappings
      * Source : jsonpath_parser:0
      * Target : Title
6. Construct a search you would like to ingest using the [DPLA API](https://github.com/dpla/platform/wiki)

  * ex:  `http://api.dp.la/v1/items?dplaContributor=%22Minnesota%20Digital%20Library%22`

7. Boop!
