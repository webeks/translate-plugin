# Translation plugin

Enables multi-lingual sites.

## Selecting a language

Different languages can be set up in the back-end area, with a single default language selected. This activates the use of the language on the front-end and in the back-end UI.

A visitor can select a language by prefixing the language code to the URL, for example:

* **http://localhost/ru/** will display the site in Russian
* **http://localhost/fr/** will display the site in French
* **http://localhost/** will display the site in the default language

## Message translation

Message or string translation is the conversion of adhoc strings used throughout the site. A message can be translated with parameters.

    {{ 'Welcome to our website!'|_ }}

    {{ 'Hello :name!'|_({ name: 'Friend' }) }}

A message can also be translated for a choice usage.

    {{ 'There are no apples|There are :number applies!'|__(2, { number: 'two' }) }}

Themes can provide default values for these messages by including a `lang.yaml` file in the theme directory.

## Model translation

Models can have their attributes translated by using the `RainLab\Translate\Traits\Translatable` trait and specifying which attributes to translate in the class.

    class User
    {
        use RainLab\Translate\Traits\Translatable;

        public $translatable = ['name'];
    }

The attribute will then contain the default language value and other language code values can be created by using the `translateContext()` method.

    $user = User::first();

    // Outputs the name in the default language
    echo $user->name;

    $user->translateContext('fr');

    // Sets the name in French
    $user->name = 'Giselle';

    // Outputs the name in French
    echo $user->name;

The `lang()` method is a shorthand version of `translateContext()` and is also chainable.

    // Outputs the name in French
    echo $user->lang('fr')->name;

This can be useful inside a Twig template.

    {{ user.lang('fr').name }}

There are ways to get and set attributes without changing the context.

    // Gets a single translated attribute for a language
    $user->getTranslateAttribute('name', 'fr');

    // Sets a single translated attribute for a language
    $user->setTranslateAttribute('name', 'Jean-Claude', 'fr');

## Content translation

This plugin activates a feature in the CMS that allows content files to use language suffixes, for example:

* **welcome.htm** will contain the content in the default language.
* **welcome.ru.htm** will contain the content in Russian.
* **welcome.fr.htm** will contain the content in French.