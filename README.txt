INTRODUCTION
------------
The drupalMigrate module demonstrates how to implement custom migrations
for Drupal 8.

STRUCTURE
---------
1. Migration configuration, in the migrations and config/install directories.
   These YAML files describe the migration process and provide the mappings from
   the source data to Drupal's destination entities. The difference between the
   two possible directories:

   a. Files in the migrations directory provide configuration directly for the
   migration plugins. The filenames are of the form <migration ID>.yml. This
   approach is recommended when your migration configuration is fully hardcoded
   and does not need to be overridden (e.g., you don't need to change the URL to
   a source web service through an admin UI). While developing migrations,
   changes to these files require at most a 'drush cr' to load your changes.

   b. Files in the config/install directory provide migration configuration as
   configuration entities, and have names of the form
   migrate_plus.migration.<migration ID>.yml ("migration" because they define
   entities of the "migration" type, and "migrate_plus" because that is the
   module which implements the "migration" type). Migrations defined in this way
   may have their configuration modified (in particular, through a web UI) by
   loading the configuration entity, modifying its configuration, and saving the
   entity. When developing, to get edits to the .yml files in config/install to
   take effect in active configuration, use the config_devel module.

   Configuration in either type of file is identical - the only differences are
   the directories and filenames.

RUNNING THE MIGRATIONS
----------------------
The migrate_tools module (https://www.drupal.org/project/migrate_tools) provides
the tools you need to perform migration processes. At this time, the web UI only
provides status information - to perform migration operations, you need to use
the drush commands.

# Enable the tools and the example module if you haven't already.
drush en -y drupalMigrate

# Look at the migrations. Just look at them. Notice that they are displayed in
# the order they will be run, which reflects their dependencies. For example,
# because the node migration references the imported terms and users, it must
# run after those migrations have been run.
drush ms               # Abbreviation for migrate-status

# Run the import operation for nodes.
drush mi d7Article  # Abbreviation for migrate-import

# Look at what you've done! Also, visit the site and see the imported content
# etc.
drush ms

# Look at the message.
drush mmsg d7Article   # Abbreviation for migrate-messages

# Run the rollback operation for all the migrations (removing all the imported
# contents etc.). Note that it will rollback the migrations in the opposite
# order as they were imported.
drush mr d7Article  # Abbreviation for migrate-rollback

# Added new line.
