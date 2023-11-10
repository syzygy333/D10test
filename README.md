# Module Monster

## What is it?
Module Monster is a "recipe"-style solution to separating dependency packages into groups (admin experience, media, search, config management, etc.) using separate, highly-focused Composer files in order to streamline the process of spinning up a new site with all the contributed and custom modules that we use across projects while also providing a clean, easy way to also include project-specific packages without muddying any of the other dependency streams.

Said another way, instead of bootstrapping your project with a "skeleton" Composer file that tries to fit a broad set of circumstances and tends to bloat as the project grows (becoming something more akin to a zombie than a skeleton), Module Monster breaks up the typical dependency scaffold into discrete chunks (ew) that can be included or not included according to what's appropriate for a given project. Keeping with metaphor, if you don't like the heads or arms that are provided to you by default, you can easily create new body parts that fit your particular situation.

## How does it work?
The main composer.json in the root of the project operates mostly as you are used to. The major difference is that it contains only the bare minimum of modules (`core-recommended` etc) and configuration (`installer-paths` etc) needed for the site to function. Customization beyond that is relegated to the "child" Composer files located in the `/module-monster/*` directories. These are broadly separated into categories like "admin" or "media," but they can be whatever you like, as long as you reference them correctly.

### Adding dependencies to an existing Composer file
To add a new dependency, manually edit the appropriate `module-monster/foo/composer.json` and save the file (this is not normally recommended, but is necessary in this case). Then run `ddev composer update --lock` from the project root. The new item will be installed and the main `composer.lock` will be updated.

### Adding a new `composer.json` and subdirectory
To add a new directory to Module Monster, simply create the directory (`module-monster/bar`) and add a new `composer.json` containing the following:
```
{
    "name": "module-monster/bar",
    "require": {

    },
    "comments": {

    },
    "config": {
        "sort-packages": true
    }
}
```
Next, add it to the `"repositories"` list:
```
{
    ...
    "repositories": [
        {
            "type": "path",
            "url": "module-monster/bar"
        }
    ]
    ...
}
```
Then, require it as normal: `ddev composer require 'module-monster/bar`.

### Patching
Patches for all modules are kept in the `composer.patches.json` file. Manually add them to the list, then apply them as normal by running `ddev composer install`.

### Commenting
Commenting in each `composer.json` is possible by simply adding a `"comments"` key with key/value pairs as the identifiers/comments, like so:
```
"comments": {
    "_metadrop/drupal-updater_comment": "installing also installs davidrjonas/composer-lock-diff"
}
```
These are generally used to keep notes about the dependencies themselves; you might like to document which feature that a given dependency was installed to support or note what packages are installed as a dependency of a dependency etc. Both the note key and value content can be whatever you like, but we recommend "sticking to the facts" and keeping them consistent. ***Note: The underscore that precedes each key is not functional, it is simply a quick way to visually distinguish these items from a `require` line, which might look similar.***

Visit https://www.freecodecamp.org/news/comments-in-json/ for more information on commenting in JSON.

### Helpers
The [Drupal Updater](https://github.com/Metadrop/drupal-updater) helper is included as a Composer script. To run it, run `ddev composer easy-update`. This helper makes updating Drupal and its dependencies nearly foolproof and ensures that dependencies of dependencies aren't forgotten (since they typically don't appear in a project's `composer.json`).


#### ⚠️
***Note: It is not recommended to run composer commands while on a VPN; they will likely timeout or fail with other errors.***

## References
### Recipes
[Drupal recipes initiative](https://www.drupal.org/project/distributions_recipes)
[Drupal Slack recipes channel](https://drupal.slack.com/archives/C2THUBAVA)
[Drupal recipe unpack](https://gitlab.ewdev.ca/yonas.legesse/drupal-recipe-unpack)

### Composer
[Composer best practices and note about composer-merge-plugin deprecation](https://drupalize.me/tutorial/composer-configuration-drupal)
[Using a "path" type repository](https://getcomposer.org/doc/05-repositories.md#path)

### Other
[Drupal Updater](https://github.com/Metadrop/drupal-updater)
