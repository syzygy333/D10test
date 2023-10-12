how is this going to be used
while Drupal's recipes initiative is still in development, this is a "recipe"-style solution to separating dependency packages into groups (admin experience, media, search, config management, etc.) using separate, highly-focused Composer files in order to streamline the process of spinning up a new site with all the contrib and custom modules that we are most familiar with.

add requires to sub-composers manually, then run `composer update` from root composer.json to install

note:
do not run composer commands while on VPN; they will likely timeout or fail with other errors.

Resources
  Recipes
    https://www.drupal.org/project/distributions_recipes
    https://drupal.slack.com/archives/C2THUBAVA Drupal Slack recipes channel
    https://gitlab.ewdev.ca/yonas.legesse/drupal-recipe-unpack

  composer
    https://drupalize.me/tutorial/composer-configuration-drupal (Custom Packages H2, especially "Note: In versions prior to 8.8.0, the Wikimedia Composer Merge plugin was utilized to merge in additional composer.json files. This method is now deprecated.")
    https://getcomposer.org/doc/05-repositories.md#path

  other
    https://github.com/Metadrop/drupal-updater (`ddev exec ./vendor/bin/drupal-updater`)
