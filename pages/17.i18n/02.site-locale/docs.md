---
title: Setting up the site language
metadata:
    description: Configuration options are available to control the overall language presented by UserFrosting.
taxonomy:
    category: docs
---

UserFrosting is translated in a variaty of languages provided by our community. While a default locale will be used for new visitors, each user can  choose their prefered language.

## The default locale

The site default languages can be set in the [config](/configuration/config-files) parameters. The `site.locales.default` contains the locale to use for global, guest users.

For example, to use _French_ as the default locale :

```php
'default' => 'fr_FR',
```

[notice=note]When returned by the browser, the browser prefered locale will be used as the default locale for guest user.[/notice]

## The available user locales

A user can also use its own language, which he can chose in is profile settings. All available locales the users can choose from are defined in the `site.locales.available` config.

To remove one locale from the available ones, simply set the unwanted locale to `false` in your sprinkle config. For example, the following config will only present the English, Spanish and French locale to the user :

```php
    'available' => [
        'en_US' => true,
        'zh_CN' => false,
        'es_ES' => true,
        'ar'    => false,
        'pt_PT' => false,
        'ru_RU' => false,
        'de_DE' => false,
        'fr_FR' => true,
        'tr'    => false,
        'it_IT' => false,
        'th_TH' => false,
    ],
```
[notice=tip]Want to add a new locale to UserFrosting? Feel free to [contribute](/contributing/supporting-userfrosting#contributing-code-and-content) on GitHub ![/notice]