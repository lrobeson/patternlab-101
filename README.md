# Pattern Lab 101
Learn what this Pattern Lab thing is all about.


![Alt text](https://i.imgur.com/TvxJnHR.png "Motha flippin Pattern Lab")

## About Pattern Lab
The PHP version of Pattern Lab is, at its core, a static site generator. It combines platform-agnostic assets, like the [Mustache](http://mustache.github.io/)-based patterns and the JavaScript-based viewer, with a PHP-based "builder" that transforms and dynamically builds the Pattern Lab site. By making it a static site generator, Pattern Lab strongly separates patterns, data, and presentation from build logic.

This version of [Pattern Lab](http://patternlab.io/) modifies the markup and styles to match those in the [Gesso](http://drupal.org/project/gesso/) Drupal theme.

[Official documentation](http://patternlab.io/docs/index.html) has been tweaked and combined into this single README.md file for [ReadTheDocs](https://readthedocs.org/) digestion. 

# Getting Started

<a name="requirements"></a>
## Requirements
The requirements for the PHP version of Pattern Lab vary depending on what features you want to use.

### Minimum Requirements for Building Pattern Lab
It's expected that you'll use the PHP version of Pattern Lab locally on your computer to develop your atoms, molecules, organisms, templates and pages. To use the basic features of Pattern Lab, you must have **PHP 5.3+** installed. If you're using Mac OS X you shouldn't have to install anything and Pattern Lab should work "out of the box." If you're on Windows you can [download PHP from PHP.net](http://windows.php.net/download/#php-5.5).

You should _not_ need to set-up Apache or another web server to use Pattern Lab unless you want to use the [Page Follow feature](#page-follow) for multi-device testing.

**Note:** While the site will work in Safari 6+ the back button is broken in the "Apache-less" version of Pattern Lab.

### Minimum Requirements for Hosting Pattern Lab
Once you want to show off your edition of Pattern Lab to a client you might want to put it on your web host. There are **no** requirements for hosting your Pattern Lab site. The Pattern Lab site consists of HTML, CSS, and JavaScript. Simply upload the `public/` directory to your host and you should be good to go.

## Installation
In order to install the PHP version of Pattern Lab, first make sure you have the [required version of PHP](#requirements) and then:

1. Visit the [Pattern Lab project on Github](https://github.com/pattern-lab/patternlab-php).
2. Download the Pattern Lab files either by [getting the a ZIP file of the project](https://github.com/pattern-lab/patternlab-php/archive/master.zip) or by cloning the project from [the Github repository](https://github.com/pattern-lab/patternlab-php).
3. [Generate Pattern Lab for the first time](#first-time).

## Upgrading Pattern Lab
To upgrade the PHP version of Pattern Lab do the following:

1. **Important:** Make a back-up of `source/`. Also see the note below about upgrading pre-v0.7.0 installs.
2. [Download the latest release](https://github.com/pattern-lab/patternlab-php/releases) of the PHP version of Pattern Lab.
3. Copy the `core/` directory from your downloaded edition and copy it into your project.
4. If you're using Mac OS X open `core/scripts/` and double-click on `generateSite.command`. If you're using Windows or Linux you will need to use [the command line options](#command-line).
5. Double-check that your files are intact in `source/`.

During the upgrade process the PHP version of Pattern Lab will run migrations to move or add any files that are required for the new edition to work as well as copy any of your configuration options.

### A Note About Upgrades for pre-v0.7.0 Editions of Pattern Lab

By default the v0.7.0+ editions of the PHP version of Pattern Lab will clean out `public/` and replace it with the files from `source/`. This is the expected behavior. Some users may have manually disabled this feature in pre-v0.7.0 editions. To make sure your files in `public/` are saved do the following:

1. Make a back-up of `public/`.
2. Upgrade using the instructions above.
3. [turn off `cleanPublic` via the new configuration option](#advanced-clean).
4. replace `public/` with your back-up
5. manually copy `core/styleguide/` to `public/styleguide/`.

Future upgrades will respect your new `cleanPublic` setting. You can also drop your public files in `source/` and not worry about steps #3-5 above.

If you edited the pattern header and footer found in `source/_patternlab-files/pattern-header-footer` you'll want to copy those changes to `source/_patterns/00-atoms/00-meta` once you've completed the upgrade. `00-meta` is [the new location for the pattern header and footer](#header-footer).

### Cleaning Up Folders from pre-v0.7.0 Editions
If you want to you can clean-up some of the directories that are left-over after upgrading pre-v0.7.0 editions of the PHP version of Pattern Lab. You can delete the following as they reference old paths or are no longer used:

* `builder/`
* `listeners/`
* `scripts/`
* `source/_patternlab-files/`

<a name="first-time"></a>
## Generating Pattern Lab for the First Time
By default, a number of important pages, including the main page, **aren't** built when you first download the PHP version of Pattern Lab. Before you visit your install of Pattern Lab you'll need to make sure all of the necessary pages have been built.

### How to Generate the Pattern Lab Website
To generate the Pattern Lab website do the following:

1. Open `core/scripts/`
2. Double-click `generateSite.command`

For Linux and Windows users, you can also generate the Pattern Lab website from the command line. To do so open a command prompt and navigate to the root of the patternlab-php directory. Type:

The site should now be generated and available for browsing.

## Editing Pattern Lab Source Files
Because the PHP version of Pattern Lab is a static site generator you _**should not edit the files in the `public/` directory**_. Instead, you should edit the files under the `source/` directory. In addition to editing patterns under the `source/` directory you'll want to [edit your JavaScript, CSS, and images](#managing-assets) as well. Each time your site is generated your files will be automatically moved to the `public/` directory and the patterns will be automatically compiled.

Generating the Pattern Lab website after each change can be cumbersome. The PHP version of Pattern Lab comes with the ability to [watch files in the `source/` directory for changes and re-generate the site automatically](#advanced-auto-regenerate). The Pattern Lab website can also be [automatically reloaded](#auto-reload).

<a name="command-line"></a>
## Using The Command Line Options
If you're using Mac OS X all of the options for using the PHP version of Pattern Lab are available under the `core/scripts/` directory. Simply double-click on the appropriate `.command` file and the service should run.

If you'd rather use the command line or are using Linux or Windows you can use the following commands and options.

### The Generate Command and Options
The generate command generates an entire site a single time. By default it removes old content in `public/`, compiles the patterns and moves content from `source/` into `public/`. Options can be mixed and matched.

  Usage:
    php core/builder.php --generate|-g [--patternsonly|-p] [--nocache|-n] [--enablecss|-c]

  Available options:
    --patternsonly (-p)    Generate only the patterns. Does NOT clean public/.
    --nocache      (-n)    Set the cacheBuster value to 0.
    --enablecss    (-c)    Generate CSS for each pattern. Resource intensive.

  Samples:

   To generate only the patterns:
     php core/builder.php --generate --patternsonly
     php core/builder.php -g -p

   To turn off the cacheBuster:
     php core/builder.php --generate --nocache
     php core/builder.php -g -n

   To run and generate the CSS for each pattern:
     php core/builder.php --generate --enablecss
     php core/builder.php -g -c

### The Watch Command and Options

The watch command builds Pattern Lab, watches for changes in `source/` and regenerates Pattern Lab when there are any. Options can be mixed and matched.

  Usage:
    php core/builder.php --watch|-w [--patternsonly|-p] [--autoreload|-r]

  Available options:
    --patternsonly (-p)    Watches only the patterns. Does NOT clean public/.
    --autoreload   (-r)    Turn on the auto-reload service.

  Samples:

   To watch and generate only the patterns:
     php core/builder.php --watch --patternsonly
     php core/builder.php -w -p

   To turn on auto-reload:
     php core/builder.php --watch --autoreload
     php core/builder.php -w -r

### The Version Command
The version command prints out the current version of Pattern Lab.

  Usage:
    php core/builder.php --version|-v

### Starting the Auto-Reload and Page Follow Services
  php core/autoReloadServer.php
    Starts the WebSocket-based server to monitor for and notify browsers of changes to content. Browser
    windows will re-load.

  php core/pageFollowServer.php
    Starts the WebSocket-based server to monitor for and notify browsers of changes to navigation. Browser
    windows should update to match the browser window that changed.

# Working with Patterns
Patterns are the core element of Pattern Lab. Understanding how they work is the key to getting the most out of the system. Patterns use [Mustache](http://mustache.github.io/) so please read [Mustache's docs](http://mustache.github.io/mustache.5.html) as well.

## How Patterns Are Organized
Patterns are organized in a nested folder structure that helps the PHP version of Pattern Lab automatically find and build assets like the "view all" pages and the drop down navigation. The pattern directories are set-up using the following naming convention:

    source/_patterns/[patternType]/[patternSubType]/[patternName].mustache

**Important:** In order for the PHP version of Pattern Lab to work you _must_ follow this directory structure.

More information on each part:

* `patternType` denotes the overall type of pattern. This will be one of: atoms, molecules, organisms, templates, or pages.
* `patternSubType` denotes the sub-type of pattern. This helps to organize patterns under an overall pattern type in the drop downs in Pattern Lab.
* `patternName` is the name of the pattern. This is used when the pattern is displayed in the drop downs in Pattern Lab.

If you want to have spaces in your pattern sub-type or pattern names use a `-` (dash). For example, if you want a pattern to be displayed in the drop-down as "Hamburger Navigation" name it `hamburger-navigation.mustache`.

By default, the patterns are organized alphabetically. If you want more control over organizing your patterns and pattern sub-types please refer to "[Reorganizing Patterns](#reorganizing)."

## Adding New Patterns
Adding new patterns to the PHP version of Pattern Lab is simply a matter of adding Mustache templates in the appropriate pattern type and pattern sub-type directories under `source/_patterns`. For example, let's add a new pattern under the pattern type "molecules" and the pattern sub-type "blocks". The `source/_patterns/01-molecules/02-blocks/` directory looks like:

    00-media-block.mustache
    01-headline-byline.mustache
    02-block-hero.mustache

If we want to add a new pattern we simply tack it onto the end:

    00-media-block.mustache
    01-headline-byline.mustache
    02-block-hero.mustache
    03-new-pattern.mustache

The numbering controls where the pattern will show up in the navigation so if you need it to show up higher in the navigation [you can reorganize it](#reorganizing).

## Reorganizing Patterns
By default, the PHP version of Pattern Lab organizes patterns alphabetically when displaying them in the drop-down navigation, pattern sub-type "view all" pages, and the style guide. This may not meet your needs. You can re-order pattern sub-types and patterns by prefixing them with two digit numbers. We'll look at how we can re-organize patterns as an example. Using alphabetical ordering the `lists` pattern sub-type in `atoms` looks like:

    definition.mustache
    ordered.mustache
    unordered.mustache

This is also the order they'll show up in the drop-down navigation. Because you rarely need to see the definition list pattern maybe you want to have it show up last in the navigation. To re-order the patterns just add numbers to the beginning:

    01-ordered.mustache
    02-unordered.mustache
    03-definition.mustache

You may want to put some space between the numbers just in case you want to further re-order and not touch the other patterns. For example, a better default ordering might be:

    01-ordered.mustache
    05-unordered.mustache
    10-definition.mustache

The numbers will not show up when Pattern Lab displays the name of the pattern in the drop-down navigation. They're simply a re-ordering mechanism.

### Can Pattern Types & Pattern Sub-Types Be Reorganized in the Same Way?
Yes.

### Do Pattern Partials Need to be Updated to Reflect the Numbering?
Assuming you use the partials shorthand then, no, you don't have to worry about including the numbering in the pattern partial. The PHP version of Pattern Lab will find the pattern regardless of its numbering. For example, the partial call would look like this for `definition.mustache`, `03-definition.mustache` or `10-definition.mustache`:

<a name="pattern-including"></a>
## Including Patterns
To use one pattern within another, for example to create a molecule from several atoms, you can either use:

* a shorthand partials syntax or
* the default [Mustache](http://mustache.github.io/mustache.5.html) partials syntax.

### The Shorthand Partials Syntax
The shorthand partials syntax mimics the format that was found in the `inc()` function from the original version of Pattern Lab. It allows for partials to be less verbose than the default Mustache partials syntax. The shorthand syntax uses the following format:

    {{> [patternType]-[patternName] }}

For example, let's say we wanted to include the following pattern in a molecule:

    00-atoms/03-images/02-landscape-16x9.mustache

The pattern type is `atoms` and the pattern name is `landscape-16x9`. Pattern sub-types are _never_ used in this format and any digits for re-ordering are _dropped_. The shorthand partial syntax would be:

    {{> atoms-landscape-16x9 }}

Patterns can be re-ordered via the digits without requiring you to update any patterns.

The shorthand syntax also allows for fuzzy matching on pattern names. This means that, if you feel your pattern name is going to be unique within a given pattern type, you can supply just the unique part of the pattern name and the partial will be included correctly. For example, using the shorthand syntax this partial could be written as:

**Important:** If you include `16x9` in another pattern the PHP version of Pattern Lab may find that first depending on how your patterns are organized.

### The Default Mustache Partials Syntax
The PHP version of Pattern Lab also supports the default Mustache partials syntax if you need more specificity when adding partials to your templates. The syntax is simply the path to the partial minus the `.mustache` extension. For example, let's say we wanted to include the following pattern in a molecule:

    00-atoms/03-images/02-landscape-16x9.mustache

The default Mustache partial syntax would be:

    {{> 00-atoms/03-images/02-landscape-16x9 }}

**Important:** Unlike the shorthand partials syntax the default Mustache partials syntax **must** include any digits for reordering. If pattern names are updated then the partial paths also need to be updated.

### Examples
Here are some examples of how to include patterns as well as some gotchas.

  // partials to match
  00-atoms/00-global/04-test-with-picture.mustache
  00-atoms/00-global/05-test.mustache
  00-atoms/00-global/06-test.mustache
  00-atoms/00-global/test.mustache

  // using the shorthand partials syntax
  {{> atoms-test }}                 // will match 00-atoms/00-global/05-test.mustache
                                    // using the shorthand syntax you'll never be able to match 06-test nor test in this scenario
  {{> atoms-test-with-picture }}    // will match 00-atoms/00-global/04-test-with-picture.mustache
  {{> atoms-test-wit }}             // will match 00-atoms/00-global/04-test-with-picture.mustache

  // using the default mustache partials syntax
  {{> atoms/global/05-test }}       // won't match anything because atoms & global are missing their digits
  {{> 00-atoms/00-global/06-test }} // will match 00-atoms/00-global/06-test.mustache

<a name="managing-assets"></a>
## Managing Pattern Assets
Assets for patterns, including JavaScript, CSS, and images, should be stored in the `source/` directory. The PHP version of Pattern Lab will move these assets to the `public/` directory for you when you generate your site or when you watch the `source/` directory for changes. You can name and nest your assets however you like. The structure will be maintained when they're moved to the `public/` directory.

### Ignoring Assets Based on File Extension
By default, the PHP version of Pattern Lab will ignore assets with the following file extensions:

To ignore more file extensions just edit the `ie` variable in `config/config.ini`. For example, to ignore `.png` files your `ie` variable would look like:

    ie = "scss,DS_Store,less,png"

### Ignoring All Assets in a Directory
By default, the PHP version of Pattern Lab will ignore assets in directories with the following name:

To ignore more directories just edit the `id` variable in `config/config.ini`. For example, to ignore directories named `test/` your `id` variable would look like:

**Important:** The PHP version of Pattern Lab will only ignore exact matches of ignored directories. For example, if you had a directory named `cool_scss` it, and the assets underneath it, _would_ be moved to `public/` even though `scss` was in the name of the directory.

### Adding Assets to the Pattern Header & Footer
Static assets like Javascript and CSS **are not** added automagically to your patterns. You need to add them manually to the [shared pattern header and footer](#header-footer).

## Modifying the Pattern Header & Footer
**Note:** _The _meta patterns were introduced in v0.7.0 of the PHP version of Pattern Lab._

To add your own assets like JavaScript and CSS to your patterns' header and footer you need to modify two files:

* `source/_patterns/00-atoms/00-meta/_00-head.mustache`
* `source/_patterns/00-atoms/00-meta/_01-foot.mustache`

These files are added to every rendered pattern, "view all" page and style guide. To see your changes simply re-generate your site.

### Important: Don't Remove Two Things...
**Do not remove the following two lines in these patterns:**

* `{% pattern-lab-head %}` in `_00-head.mustache`
* `{% pattern-lab-foot %}` in `_00-foot.mustache`

Pattern Lab will stop working if you do.

## Using Pseudo-Patterns
**Note:** _Pseudo-patterns were introduced in v0.7.0 of the PHP version of Pattern Lab._

Pseudo-patterns are meant to give developers the ability to build multiple and unique **rendered** patterns off of one base pattern and its mark-up while giving them control over the data that is injected into the base pattern. This feature is especially useful when developing template- and page-style patterns.

### The Pseudo-Pattern Syntax
Pseudo-patterns are, essentially, the pattern-specific JSON files that would accompany a pattern. Rather than require a Mustache pattern, though, pseudo-patterns are hinted so a developer can reference a shared pattern. The basic syntax:

    patternName~pseudoPatternName.json

The tilde, `~`, and JSON extension denotes that this is a pseudo-pattern. `patternName` is the parent pattern that will be used when rendering the pseudo-pattern. `patternName` and `pseudoPatternName` are combined when adding the pseudo-pattern to the navigation.

The JSON file itself works exactly like the [pattern-specific JSON file](#pattern-json). It has the added benefit that the pseudo-pattern will also import any values from the parent pattern's pattern-specific JSON file.

### Adding Pseudo-Patterns to Your Project
Adding a pseudo-pattern is as simple as naming it correctly and following the [pattern-specific JSON file](#pattern-json) instructions for organizing its content. Let's look at a simple example where we want to show an emergency notification on our homepage template. Our `03-templates/` directory might look like this:

    00-homepage.mustache
    01-blog.mustache
    02-article.mustache

Our `00-homepage.mustache` template might look like this:

    <div id="main-container">
        {{# emergency }}
            <div class="emergency">Oh Noes! Emergency!</div>
        {{/ emergency }}
        { ...a bunch of other content... }
    </div>

If our `_data.json` file doesn't give a value for `emergency` that section will never show up when `00-homepage.mustache` is rendered.

We want to show both the regular and emergency states of the homepage but we don't want to duplicate the entire `00-homepage.mustache` template. That would be a maintenance nightmare. So let's add our pseudo-pattern:

    00-homepage.mustache
    00-homepage~emergency.json
    01-blog.mustache
    02-article.mustache

In our pseudo-pattern, `00-homepage~emergency.json`, we add our `emergency` attribute:

Now when we generate our site we'll have our homepage template rendered twice. Once as the regular template and once as a pseudo-pattern showing the emergency section. Note that the pseudo-pattern will show up in our navigation as `Homepage Emergency`.

### Using Pseudo-Patterns as Pattern Partials
Pseudo-patterns **cannot** be used as pattern partials. The data included in the pseudo-pattern, the bit that actually controls the magic, cannot be accessed when rendering the pattern partial.

<a name="pattern-parameters"></a>
## Using Pattern Parameters
**Note:** _Pattern parameters were introduced in v0.7.0 of the PHP version of Pattern Lab._

Pattern parameters are a simple mechanism for replacing Mustache variables via attributes on a pattern partial tag rather than having to use a [pattern-specific JSON file](#pattern-json). They are especially useful when you want to supply distinct values for Mustache variables in a specific pattern partial instance that may be included multiple times in a molecule, template, or page. Pattern parameters **do not** currently support the following:
* sub-lists (e.g. iteration of a section),
* and the use of long strings of text can be unwieldy

Pattern parameters are Pattern Lab-specific and do not take advantage of the traditional Mustache syntax. Learn more about pattern parameters:

### The Pattern Parameter Syntax
The attributes listed in the pattern parameters should match Mustache variable names in your pattern. The values listed for each attribute will replace the Mustache variables. For example:

    {{> patternType-pattern(attribute1: value, attribute2: "value string") }}

Again, pattern parameters are a simple find and replace of Mustache variables with the supplied values.

### Adding Pattern Parameters to Your Pattern Partial
Let's look at a simple example for how we might use pattern parameters. First we'll set-up a pattern that might be included multiple times. We'll make it a simple "message" pattern with a single Mustache variable of `message`.

    <div class="message">{{ message }}</div>

We'll organize it under Atoms > Global and call it "message" so it'll have the pattern partial of `atoms-message`.

Now let's create a pattern that includes our message pattern partial multiple times. It might look like this.

    <div class="main-container">
        {{> atoms-message }}
        <div>
           A bunch of extra information
        </div>
        {{> atoms-message }}
    </div>

Using `data.json` or a pattern-specific JSON file we wouldn't be able to supply separate messages to each pattern partial. For example, if we defined `message` in our `data.json` as "this is an alert" then "this is an alert" would show up twice when our parent pattern was rendered.

Instead we can use pattern parameters to supply unique messages for each instance. So let's show what that would look like:

    <div class="main-container">
        {{> atoms-message(message: "this is an alert message") }}
        <div>
            A bunch of extra information
        </div>
        {{> atoms-message(message: "this is an informational message") }}
    </div>

Now each pattern would have its very own message.

### Toggling Sections with Pattern Parameters

While pattern parameters are not a one-to-one match for Mustache they do offer the ability to toggle sections of content. For example we might have the following in a generic header pattern called `organisms-header`:

    <div class="main-container">
        {{# emergency }}
            <div class="alert">Emergency!!!</div>
        {{/ emergency }}
        <div class="header">
            ... stuff ...
        </div>
    </div>

We call the header pattern in a template like so:

    {{> organisms-header }}
    ... stuff ...

By default, if we don't have an `emergency` attribute in our `data.json` or the pattern-specific JSON file for the template the emergency alert will never be rendered. Instead of modifying either of those two files we can use a boolean pattern param to show it instead like so:

    {{> organisms-header(emergency: true) }}
    ... stuff ...

Again, because pattern parameters aren't leveraging Mustache this may not fit the normal Mustache workflow. We still wanted to offer a way to quickly turn on and off sections of an included pattern.

## Using Pattern States

**Note:** _Pattern states were introduced in v0.7.5 of the PHP version of Pattern Lab._

Pattern states provide your team and your client a simple visual of the current state of patterns in Pattern Lab. Pattern states can track progress of a pattern from development, through client review, to completion or they can be used to give certain patterns specific classes. It's important to note that the state of a pattern can be influenced by its pattern partials.

### The Default Pattern States

The PHP version of Pattern Lab comes with the following default pattern states:

* **inprogress**: pattern is in development or being worked upon. a red dot.
* **inreview**: pattern is ready for a client to look at and comment upon. a yellow dot.
* **complete**: pattern is ready to be moved to production. a green dot.

Any pattern that includes a pattern partial that has a lower pattern state will inherit that state. For example, a pattern with the state of `inreview` that includes a pattern partial with the state of `inprogress` will have its state overridden and set to `inprogress`. It will not change to `inreview` until the pattern partial has a state of `inreview` or `complete`.

### Giving Patterns a State

Giving patterns a state is simply a matter of modifying the file name. If we wanted to give our `molecules-media-block` pattern a state of `inprogress` we'd change the file name from:

    source/_patterns/01-molecules/02-blocks/00-media-block.mustache

to:

    source/_patterns/01-molecules/02-blocks/00-media-block@inprogress.mustache

### Adding Customized States

The three default states included with the PHP version of Pattern Lab might not be enough for everyone. To add customized states you should modify your own CSS files. **DO NOT** modify `states.css` in `public/styleguide/css/`. This is because `states.css` will be overwritten in future upgrades.

You can use the following as your CSS template for new pattern states:

    .newpatternstate:before {
        color: #B10DC9 !important;
    }

Then add `@newpatternstate` to your patterns to have the new look show up. If you want to add it to the cascade of the default patterns you can modify your `config.ini`. Simply add your new pattern state to the `patternStates` list.

## Using styleModifiers

**Note:** _styleModifiers were introduced in v0.7.0 of the PHP version of Pattern Lab._

styleModifiers allow you to create a base pattern that you can easily modify by adding a class name to the pattern partial.

### Syntax

The basic syntax:

    {{> patternType-pattern:styleModifier }}

### Example

Let's look at a simple example where we add a `styleModifier` Mustache variable to a pattern called `atoms-message`. For this feature to work the Mustache variable _has_ to be called `styleModifier`.

    <div class="message {{ styleModifier }}">{{ message }}</div>

Now let's include the pattern partial:

    <div>
        {{> atoms-message:error }}
    </div>

This would render as:

    <div>
        <div class="message error"></div>
    </div>

We forgot to provide a message so we can do that too with [pattern parameters](#pattern-parameters):

    <div>
        {{> atoms-message:error(message: "some error message") }}
    </div>

Which would render as:

    <div>
        <div class="message error">some error message</div>
    </div>

This feature is in anticipation of a future release of the PHP version of Pattern Lab that will include support for [KSS](http://warpspire.com/kss/).

## Hiding Patterns in the Navigation

There may come a time when you want to remove a pattern from Pattern Lab's drop-down navigation and style guide but still keep it in your patterns directory for later use. You can "comment out" a pattern by adding an `_` (underscore) to the beginning of the pattern name. For example, we may have a Google Map-based pattern that we don't need for a particular project. The path might look like:

    molecules/media/map.mustache

To "hide" the pattern we add the underscore:

    molecules/media/_map.mustache

Once the site has been re-generated the map pattern will no longer show up in the drop-down navigation, the media pattern sub-type "view all" page, nor the style guide.

## Adding Annotations

Annotations provide an easy way to add notes to elements within a pattern and they can be found in `source/_data/annotations.js`. They're _not_ pattern-specific. Rather, they're added to patterns using the same selector syntax you'd use for jQuery or CSS.

### Explaining Annotations

To explain how annotations are structured here is the annotation that's added to the logo:

    {
        "el": ".logo",
        "title" : "Logo",
        "comment": "The logo image is an SVG file, which ensures that the logo displays crisply even on high resolution displays. A PNG fallback is provided for browsers that don't support SVG images.</p><p>Further reading: <a href="http://bradfrostweb.com/blog/mobile/hi-res-optimization/">Optimizing Web Experiences for High Resolution Screens</a></p>"
    }

Breaking down the JSON:

* **el** \- the selector to be used to attach the annotation to a pattern
* **title** \- the title for a given annotation
* **comment** \- the description for a given annotation

### Making Changes to Annotations

In order to make changes or additions to annotations simply edit the `annotations.js` file. Once your changes have been made just make sure your site has been re-generated.

### Viewing Annotations

In order to view annotations you'll need to click "Annotations" under the "eye" icon in the Pattern Lab toolbar. Depending on which view you're on the annotations will look slightly different.

#### Pattern-only View

After clicking the "annotations" menu item you can mouse over elements that have annotations and your cursor will turn to a question mark. If you click the element then the annotation should pop up.

#### Style Guide and List View

After clicking the "annotations" menu item the annotations should show up listed under the appropriate patterns.

<a name="pattern-mobile"></a>
## Viewing Patterns on a Mobile Device

**Note:** _The QR code generator and xipHostname configuration option were introduced in v0.7.0 of the PHP version of Pattern Lab. As of v0.7.9 it is off by default. Turn it on in config.ini._

While the resizing toolbar is nice it's always good to review your patterns on real mobile devices. Depending on where your patterns are located the directions are slightly different.

### For Patterns Hosted Locally on Your Computer

For an instance of the PHP version of Pattern Lab hosted locally you can do the following. These directions assume that you're using Apache to host your Pattern Lab website locally and that you've set-up a `ServerAlias` for your site using `xip.io`. The `ServerAlias` should look like `patternlab.*.xip.io`

1. Install a QR code reader on your mobile device
2. Make sure `xipHostname` in `config/config.ini` matches your `xip.io` hostname in Apache
3. Make sure your mobile device and computer are on the same WiFi network
4. Generate your website
5. Navigate to the pattern you want to view on your mobile device
6. Scan the auto-generated QR code found under the gear icon in Pattern Lab's toolbar

If you don't like QR codes you can also do the following:

1. Note the IP address for your computer. On Mac OS X this is found under System Preferences > Sharing
2. Replace the star with your IP address in the following address: `patternlab.*.xip.io`
3. Enter that into the browser on your mobile device

### For Patterns Hosted on a Server

For an instance of the PHP version of Pattern Lab hosted on a server you can do the following:

1. Install a QR code reader on your mobile device
2. Make sure `xipHostname` in `config/config.ini` is **blank**
3. Generate your website
4. Upload it to your server
5. Navigate to the pattern you want to view on your mobile device
6. Scan the auto-generated QR code found under the gear icon in Pattern Lab's toolbar

If you don't like QR codes you can simply visit the website in your browser.

# Working With Data

The PHP version of Pattern Lab utilizes Mustache as the template language for patterns. In addition to allowing for the [inclusion of one pattern within another](http://patternlab.io/docs/pattern-including.html) it also gives pattern developers the ability to include variables. This means that attributes like image sources can be centralized in one file for easy modification across one or more patterns. The PHP version of Pattern Lab uses a JSON file, `source/_data/data.json`, to centralize many of these attributes.

## Introduction to JSON & Mustache Variables

The best reference for this topic is the [Mustache documentation](http://mustache.github.io/mustache.5.html) but this should provide a good beginner's primer.

### Simple Variables

At its core JSON is a simple key-value store. This means that any piece of data in JSON has a key and a value. The key is the name of an attribute and the value is what should be shown when that attribute is referenced. Here's a simple example:

    "src": "../../images/fpo-avatar.png"

In this case the key is `src` and the value is `../../images/fpo-avatar.png`. Let's look at how we might reference this data in a pattern template. Mustache variables are denoted by the double-curly braces (or mustaches).

    <img src="" alt="Avatar">

The Mustache variable is `{{ src }}`. Note that `src` matches the name of the key in our JSON example. When the PHP version of Pattern Lab compiles this template the end result will be:

    <img src="http://patternlab.io/images/fpo-avatar.png" alt="Avatar">

Note that `{{ src }}` was replaced by the value for `src` found in our JSON example.

### Nested Variables

We may want our JSON file to be a little more organized and our Mustache variable names to be a little more descriptive. For example, maybe we have multiple image sizes that we want to provide image sources for. We might organize our JSON key-values this way:

    "square": {
        "src": "../../images/fpo-square.png",
        "alt": "Square"
    },
    "avatar": {
        "src": "../../images/fpo-avatar.png",
        "alt": "Avatar"
    }

Note how their are attributes ( `src`, `alt` ) nested within a larger container ( `square` ). Also note how the attributes are separated by commas. If we wanted to use the attributes for the square image in our pattern we'd write:

    <img src="" alt="{{ square.alt }}">

This would compile to:

    <img src="http://patternlab.io/images/fpo-square.png" alt="Square">

This nesting makes it easier to read how the attributes are organized in our patterns. The default `data.json` file has several examples of this type of nesting of attributes.

### Rendering HTML in Variables

You may want to include HTML in your variables. By default, Mustache will convert HTML mark-up to their HTML entity equivalents. For example, our JSON may look like:

    "lyrics": "Just <em>good ol' boys</em>, wouldn't change if they could, <strong>fightin'</strong> the system like a true modern day Robin Hood."

Based on our previous Mustache examples you would probably write out your template like so:

    <h2>TV Show Lyrics</h2>
    <p>{{ lyrics }}</p>

Unfortunately, that would compile as:

    <h2>TV Show Lyrics</h2>
    <p>Just &lt;em&gt;good ol' boys&lt;/em&gt;, wouldn't change if they could, &lt;strong&gt;fightin'&lt;/strong&gt; the system like a true modern day Robin Hood.</p>

In order to make sure the mark-up doesn't get converted you must use _triple_ curly brackets like so:

    <h2>TV Show Lyrics</h2>
    <p>{{{ lyrics }}}</p>

Now it would compile correctly:

    <h2>TV Show Lyrics</h2>
    <p>Just <em>good ol' boys</em>, wouldn't change if they could, <strong>fightin'</strong> the system like a true modern day Robin Hood.</p>

<a name="pattern-json"></a>
## Creating Pattern-specific Values

Setting up data variables and values for your atoms, molecules, and organisms using the central `data.json` may work just fine. When fleshing out templates & pages, where data may need to be unique to each page but each will still use the same molecules and organisms, the central `data.json` file can become cumbersome. In order to work around this the PHP version of Pattern Lab allows you to define pattern-specific JSON files that allow you to override the default values found in `source/_data/data.json` or `source/_data/listitems.json`.

### Setting Up Pattern-specific Data

In order to tell the PHP version of Pattern Lab to use a pattern-specific JSON file to override the default variables just give the JSON file the same name as the pattern and put it in the same directory as the pattern. For example, if you wanted to provide pattern-specific data for the `article` pattern under the pattern type `pages` your `pages` directory would look like this:

    pages/article.mustache
    pages/article.json

### Overriding the Default Variables

Overriding the default variables using the pattern-specific data is a just a matter of giving the variables you want to override the same names and structure in your pattern-specific data as they appear in the central data file. For example, the 4x3 landscape image source may look like this in `data.json`:

    "landscape-4x3": {
        "src": "../../images/fpo-landscape-4x3.jpg",
        "alt": "Landscape 4x3 Image"
    }

In our pattern-specific data file, `article.json`, we'd simply copy that structure and provide our own information:

    "landscape-4x3": {
        "src": "../../images/a-team-hero.jpg"
    }

Now the article pattern will display an image of the A-Team when using `{{ landscape-4x3.src }}`. All other patterns using `{{ landscape-4x3.src }}` will display the default 4x3 image. Also, note that we **didn't** override the `landscape-4x3.alt` attribute. If we were to use that attribute in our pattern the default value, "Landscape 4x3 Image", would be displayed.

**Important note:** You don't have to override every attribute. You can limit the data in your pattern-specific data file to just those variables you want. The PHP version of Pattern Lab will fallback to using the default attributes from `data.json` if the attributes aren't defined in the pattern-specific data file.

### Working With Partials
Pattern-specific data is only loaded for the overall pattern and not for any related partials. For example, the pages template, `article`, might include the molecule, `block-hero`. `block-hero` may have its own pattern-specific data file, `block-hero.json`. The PHP version of Pattern Lab **will not** use the `block-hero` data when rendering `article`. It will only use `article.json` (if available) and `data.json`.

<a name="link-variable"></a>
## Linking to Patterns with Pattern Lab's Default `link` Variable
You can build patterns that link to one another to help simulate using a real website. This is especially useful when working with the Pages and Templates pattern types. Rather than having to remember the actual path to a pattern you can use the same shorthand syntax you'd use to include one pattern within another. **Important:** Pattern links _do not_ support the same fuzzy matching of names as the shorthand partials syntax does. The basic format is:

For example, if you wanted to add a link to the `article` template from your `blog` template you could write the following:

    <a href="{{ link.templates-article }}">Article Headline</a>

This would compile to:

    <a href="/patterns/templates-layouts-article/templates-layouts-article.html">Article Headline</a>

As you can see, it's a much easier way of linking patterns to one another.

## Creating Lists with Pattern Lab's Default `listItems` Variable
Many more complicated patterns may include lists of objects. For example, comments or headlines. The PHP version of Pattern Lab comes with a simple way to fill out these lists with dynamic data so that you can quickly stub them out. The list data can be found in `source/_data/listitems.json` and is accessed in Mustache with the `listItems` key. Lists are randomized each time the Pattern Lab website is generated.

### Using listItems Attributes to Create a Simple List
Let's look at a simple example of iterating over a list. In your template you might have:

    <ul>
    {{# listItems.four }}
        <li> {{ title }} </li>
    {{/ listItems.four }}
    </ul>

Let's break this down before showing the results. The `#` denotes that Mustache needs to loop over the given key that contains multiple values, in this case `listItems.four`, and write-out the corresponding value `{{ title }}`. A full list of attributes can be found below. The `/` denotes the end of the block that's being rendered. The PHP version of Pattern Lab supports the keys `one` through `twelve`. If you need more than twelve items for a given list you'll need to add your own data. **Important**: the keys `one` through `twelve` are Pattern Lab-specific and not a general feature of Mustache.

The above would compile to:

    <ul>
        <li> Rebel Mission to Ord Mantell</li>
        <li> Help, help, I'm being repressed!</li>
        <li> Bacon ipsum dolor sit amet turducken strip steak beef ribs shank</li>
        <li> Nullizzle shizznit velizzle, hizzle, suscipit own yo', gravida vizzle, arcu.</li>
    </ul>

If you wanted six items in your list you'd write:

    <ul>
    {{# listItems.six }}
        <li> {{ title }} </li>
    {{/ listItems.six }}
    </ul>

### Combining listItems with Partials

Let's look at how we might build a comment thread using `listItems` and partials. First we'll start with an organism called `comment-thread` that looks like:

    <section class="comments section">
        <div class="comments-container" id="comments-container">
            <h2 class="section-title">Comment List</h2>
            <div class="comment-list">
                {{# listItems.five }}
                    {{> molecules-single-comment }}
                {{/ listItems.five }}
            </div>
        </div>
    </section>

This organism is including the `single-comment` molecule ( `{{> molecules-single-comment}}` ) _within_ our block where we're iterating over five items from the `listItems` variable ( `{{# listItems.five }}` ). What this is doing is rendering the `single-comment` molecule five times with different data each time. Our `single-comment` molecule might look like this:

    <div class="comment-container">
        <div class="comment-meta">
            {{> atoms-avatar }}
            <h4 class="comment-name"><a href="{{ url }}">{{ name.first }} {{ name.last }}</a></h4>
        </div>
        <div class="comment-text">
            <p>{{ description }} </p>
        </div>
    </div>

Note how the Mustache variable names match up to the attributes available in our `listItems` variable. Again, each time the `single-comment` pattern is rendered those variables will have different data. Using a partial allows us to DRY up our code. We can even nest partials within partials as shown by `{{> atoms-avatar }}` in our example.

**Important**: You don't have to use the default `listItems` variable to take advantage of this feature. You can also use this method with pattern-specific data files or the default `data.json` file.

### Overriding the Defaults with Pattern-specific listItems

In much the [same way that one can override values](#pattern-json) in `_data.json` with pattern-specific data you can also override `_listitems.json`. The same principles apply but it's a slightly different naming convention. For example, if you wanted to provide pattern-specific `listItem` data for the `article` pattern under the pattern type `pages` your `pages` directory would look like this:

    pages/article.mustache
    pages/article.listitems.json

Otherwise [the same rules for overriding defaults](#pattern-json) for `_data.json` apply to `_listitems.json`.

### The listItems Attributes

The list text attributes were built using several lorem ipsum generators. The image attributes utilize [placeimg.com](http://placeimg.com). The names were generated with [Behind the Name](http://www.behindthename.com/). The available attributes are:

    title
    description
    url
    headline.short         (~37 characters long)
    headline.medium        (~72 characters long)
    excerpt.short          (~150 characters long)
    excerpt.medium         (~233 characters long)
    excerpt.long           (~450 characters long)
    img.avatar.src
    img.avatar.alt
    img.square.src
    img.square.alt
    img.rectangle.src      (4x3 aspect ratio)
    img.rectangle.alt
    img.landscape-4x3.src
    img.landscape-4x3.src
    img.landscape-16x9.src
    img.landscape-16x9.alt
    name.first             (e.g. Junius)
    name.firsti            (e.g. J)
    name.middle            (e.g. Marius)
    name.middlei           (e.g. M)
    name.last              (e.g. Koolen)
    name.lasti             (e.g. K)
    year.long              (e.g. 2013)
    year.short             (e.g. 13)
    month.long             (e.g. January)
    month.short            (e.g. Jan)
    month.digit            (e.g. 01)
    dayofweek.long         (e.g. Monday)
    dayofweek.short        (e.g. Mon)
    day.long               (e.g. 05)
    day.short              (e.g. 5)
    day.ordinal            (e.g. th)
    hour.long              (e.g. 11)
    hour.short             (e.g. 11)
    hour.military          (e.g. 23)
    hour.ampm              (e.g. pm)
    minute.long            (e.g. 09)
    minute.short           (e.g. 9)
    seconds                (e.g. 52)

The aspect ratio for `img.rectangle` is 4x3. Hopefully this gives pattern developers an easy way to build out dynamic lists for testing.

# Advanced Features

By default, the Pattern Lab assets can be manually generated and the Pattern Lab site manually refreshed but who wants to waste time doing that? Here are some ways that Pattern Lab can make your development workflow a little smoother:

<a name="advanced-auto-regenerate"></a>
## Watching for Changes and Auto Regenerating Patterns

The PHP version of Pattern Lab has the ability to watch for changes to patterns and select files. When these files change, it will automatically rebuild the entire Pattern Lab website. You simply make your changes, save the file, and the PHP version of Pattern Lab will take care of the rest.

### How to Start the Watch

To start the watch service on Mac OS X you can do the following:

1. Open `core/scripts/`
2. Double-click `startWatcher.command`

For Linux and Windows users you can start the watch from the command line. To do so open your command prompt and navigate to the root of the `patternlab-php` directory. Type:

### How to Start the Watch & Auto-Reload Server at the Same Time

Rather than manually refreshing your browser when your patterns or CSS change you can have the PHP version of Pattern Lab auto-reload your browser window for you when it's in watch mode. To start the watch and auto-reload services together on Mac OS X you can do the following:

1. Open `core/scripts/`
2. Double-click `startWatcherWithAutoReload.command`
3. Refresh the Pattern Lab site

For Linux and Windows users you can start the watch from the command line. To do so open your command prompt and navigate to the root of the `patternlab-php` directory. Type:

### How to Stop the Watch

To stop watching files on Mac OS X you can press`CTRL+C` in the Terminal window where the process is running. If you've used the method above to start the auto-reload server it will also shut down when using `CTRL+C`.

### The Default Files That Are Watched

By default, the PHP version of Pattern Lab monitors the following files:

* all of the patterns under `source/_patterns`
* all of the JSON files under `source/_data/`
* any directory in `source/` without an `_` (underscore) or that doesn't match a directory name found in the `id` variable in `config/config.ini`
* any file in `source/` with a file extension that doesn't match one found in the `ie` variable in `config/config.ini`

### Ignoring Other Directories & File Extensions

Instructions on how to ignore assets in other directories or with other file extensions can be found in "[Managing Assets for a Pattern](#managing-assets)".

<a name="auto-reload"></a>
## Auto Reloading the Browser Window When Changes Are Made

Rather than manually refreshing your browser when your patterns or CSS change you can have the PHP version of Pattern Lab auto-reload your browser window for you.

### How to Start the Auto-Reload Server

To start the service on Mac OS X you can do the following:

1. Open `core/scripts/`
2. Double-click `startAutoReloadServer.command`
3. Refresh the Pattern Lab site

For Linux and Windows users you can also start the service from the command line. To do so open your command prompt and navigate to the root of the patternlab-php directory. Type:

    php core/autoReloadServer.php

Your browser should now be listening for auto-reload events and the Pattern Lab website toolbar should note that "Auto-Reload" is now "On." For this feature to work you **must** have the PHP version of Pattern Lab watching for changes. [Learn how to set this up](#advanced-auto-regenerate).

**Important:** If you find that content sync is not working properly please make sure your browser [supports WebSockets](http://caniuse.com/websockets).

### How to Start the Watcher & Auto-Reload Server at the Same Time

To start the watch and auto-reload services together on Mac OS X you can do the following:

1. Open `core/scripts/`
2. Double-click `startWatcherWithAutoReload.command`
3. Refresh the Pattern Lab site

For Linux and Windows users you can also start the service from the command line. To do so open your command prompt and navigate to the root of the `patternlab-php` directory. Type:

### How to Stop the Service

To stop the service on Mac OS X you can press `CTRL+C` in the Terminal window where the process is running. If you've used the method above to start the watch and the auto-reload server they will both shut down when using `CTRL+C`.

<a name="page-follow"></a>
## Multi browser & Multi device Testing with Page Follow

The PHP version of Pattern Lab's Page Follow feature gives developers the ability to have one browser control other browsers that connect to the Pattern Lab website. When a browser first connects to the Pattern Lab website they'll be redirected to the last visited pattern. Navigating to a new pattern will update all connected browsers. This should be especially useful when testing patterns across multiple devices.

### How to Start the Page Follow Server

To start the service on Mac OS X you can do the following:

1. Open `core/scripts/`
2. Double-click `startPageFollowServer.command`
3. Refresh the Pattern Lab site

For Linux and Windows users you can also start the service from the command line. To do so open your command prompt and navigate to the root of the patternlab-php directory. Type:

    php core/pageFollowServer.php

Your browser should now be listening for Page Follow events and the Pattern Lab toolbar should note that "Page Follow" is now "On." Any other browser that visits the Pattern Lab site should now be redirected to the last visited pattern. When one browser views another pattern they should all be updated.

### How to Stop the Service

To stop the service on Mac OS X you can press CTRL+C in the Terminal window where the process is running.

### Viewing the Pattern Lab Website on Your Mobile Device

There is a [separate article on this topic](#pattern-mobile).

**Important:** If you find that page follow is not working properly please make sure your browser [supports WebSockets](http://caniuse.com/websockets).

## Keyboard Shortcuts

Pattern Lab comes with support for a number of special keyboard shortcuts to make using Pattern Lab easier. These are broken up by where they work or are most useful.

Modifying the viewport:

* **ctrl+shift+0**: set the viewport to 320px
* **ctrl+shift+[1-9]**: set the viewport to one of the pre-determined breakpoints in the MQs list. `1` is at the top.
* **ctrl+shift+s**: set the viewport to "small"
* **ctrl+shift+m**: set the viewport to "medium"
* **ctrl+shift+l**: set the viewport to "large"
* **ctrl+shift+h**: toggle Hay mode
* **ctrl+shift+d**: toggle disco mode

Modifying the views:

* **ctrl+shift+a**: open/close annotations view
* **ctrl+shift+c**: open/close code view
* **cmd+a/ctrl+a**: select the content of the current open tab in code view
* **ctrl+shift+u**: make the Mustache tab active
* **ctrl+shift+y**: make the HTML tab active
* **esc**: close the open view

Other:

* **ctrl+shift+f**: open/close the pattern search

## Pattern Lab's Special Query String Variables

Pattern Lab comes with support for a number of special query string variables to help you share patterns with clients. These query string variables include ways to link to patterns, set the Pattern Lab viewport to a specific width, open various views as well as start Hay and disco modes on page load. There are lots of options:

### Linking to Specific Patterns

You can link directly to any pattern listed on the Pattern Lab website. This might be useful when asking clients for feedback on a particular template or page pattern. If you want to [link from one pattern to another use the `link` variable](#link-variable).

#### Copy & Paste

The simplest method is to copy the address found in the address bar.

#### Manually Creating the Link

It's also very easy to create a link manually. Simply append `?p=pattern-name` to the end of the address for your Pattern Lab website. For example, if we wanted to link to the `templates-article` pattern we'd add the following to the address for our Pattern Lab website:

The direct link feature supports the [shorthand partials syntax](#pattern-including) found in the PHP version of Pattern Lab. Just provide part of a pattern name and Pattern Lab will attempt to resolve it.

### Setting the Default Width for the Viewport

You can load a specific viewport size by using the `w` query string variable.

    http://patternlab.localhost/?w=320   (sets the viewport to 320px)
    http://patternlab.localhost/?w=400px (sets the viewport to 400px)
    http://patternlab.localhost/?w=40em  (sets the viewport to 40em or 640px)

And it works with the `p` query string variable so you can also do:

    http://patternlab.localhost/?p=atoms-landscape-4x3&w=400px

### Opening Annotations View on Page Load

When sending a particular pattern to a client for review you may not want to include directions for how to open annotations. You can force the annotations view to open on page load by using the `view` query string variables with the values `annotations` or `a`:

    http://patternlab.localhost/?p=templates-homepage&view=annotations
    http://patternlab.localhost/?p=templates-homepage&view=a

You can also force the annotations panel to scroll to a particular item by including the query string variable `number`. To scroll to the fifth element in the list of annotations you'd do the following:

    http://patternlab.localhost/?p=templates-homepage&view=annotations&number=5

### Opening Code View on Page Load

When sending a particular pattern to a client for review you may not want to include directions for how to open the code view. You can force the code view to open on page load by using the `view` query string variables with the values `code` or `c`:

    http://patternlab.localhost/?p=templates-homepage&view=code
    http://patternlab.localhost/?p=templates-homepage&view=c

You can also force the HTML tab mark-up to be highlighted for copying by including the query string variable `copy`.

    http://patternlab.localhost/?p=templates-homepage&view=code&copy=true

### Starting Hay Mode on Page Load

You can start Hay mode automatically when the page is loaded by using the `h` or `hay` query string variables.

    http://patternlab.localhost/?h=true
    http://patternlab.localhost/?hay=true

And it works with the `p` query string variable so you can also do:

    http://patternlab.localhost/?p=atoms-landscape-4x3&h=true

### Starting Disco Mode on Page Load

You can start disco mode automatically when the page is loaded by using the `d` or `disco` query string variables.

    http://patternlab.localhost/?d=true
    http://patternlab.localhost/?disco=true

And it works with the `p` query string variable so you can also do:

    http://patternlab.localhost/?p=atoms-landscape-4x3&d=true

<a name="advanced-clean"></a>
## Stopping public/ from Being "Cleaned"

**Note:** _The cleanPublic configuration option was introduced in v0.7.0 of the PHP version of Pattern Lab._

By default the PHP version of Pattern Lab deletes most of the files and directories found in `public/` when generating your site or starting the watch. Developers are supposed to use `source/` to store their files. This includes static assets like images, JavaScript and CSS. When generating your site the PHP version of Pattern Lab moves all of the static assets found in `source/` to `public/` (_after cleaning it_) so there shouldn't be a reason not to use `source/`.

That said, developers might be more comfortable storing their static assets in `public/`. In order to turn-off the automatic `cleanPublic()` feature you can edit your configuration file. To do so do the following:

1. Open `config/config.ini`.
2. Change the `cleanPublic` from `"true"` to `"false"`

When you next generate your site or start the watch `public/` will no longer be cleaned.

## Generating CSS

**Note:** _The [CSS Rule Saver](https://github.com/dmolsen/css-rule-saver) library and CSS generation feature was added in v0.6.0 of the PHP version of Pattern Lab._

When using this feature, Pattern Lab can display only those CSS rules that affect a given pattern on the pattern detail view. This might be useful if you have a large Sass-generated CSS file or framework but only need a sub-set of styles that may affect a small piece of mark-up or pattern.

### How to Generate the CSS

To generate your Pattern Lab site with CSS support on Mac OS X you can do the following:

1. Open `core/scripts/`
2. Double-click `generateSiteWithCSS.command`
3. Refresh the Pattern Lab site

For Linux and Windows users you can also start the service from the command line. To do so open your command prompt and navigate to the root of the patternlab-php directory. Type:

### Important Note About Performance

It may take several seconds for the PHP version of Pattern Lab to generate a site when it's also calculating the CSS rules. Be patient.

## Modifying the Pattern Lab Nav

**Note:** _This option was introduced in v0.7.5 of the PHP version of Pattern Lab._

When sharing Pattern Lab with a client it may be beneficial to turn-off certain elements in the default navigation. To turn-off navigation elements simply add their keys to the `ishControlsHide` option in `config/config.ini` and then re-generate the site. The following keys are supported and will hide their respective elements:

    s
    m
    l
    full
    random
    disco
    hay
    mqs
    find
    views-all
    views-annotations
    views-code
    views-new
    tools-all
    tools-follow
    tools-reload
    tools-shortcuts
    tools-docs

## Editing the config.ini Options

Pattern Lab comes with a simple configuration file that allows you to modify certain aspects of the system. The following example configuration is from `v0.7.9` of Pattern Lab.

    /*
     * Configuration Options for Pattern Lab
     * If config.ini doesn't exist Pattern Lab will try to create a new version
     */

    v  = "0.7.9"

    // file extensions to ignore when building or watching the source dir, separate with a comma
    ie = "scss,DS_Store,less"

    // directories and files to ignore when building or watching the source dir, separate with a comma
    id = "scss,.svn,.sass-cache"

    // choose if these services should be loaded in the nav and their ports
    autoReloadNav  = "true"
    autoReloadPort = "8002"
    pageFollowNav  = "true"
    pageFollowPort = "8003"

    // whether the qr code generator should be loaded automatically in the nav
    qrCodeGeneratorOn = "false"

    // pattern lab's xip host if you have it configured, to be used with the QR code generator
    xipHostname = "http://patternlab.*.xip.io"

    // whether the public directory should be cleaned when generating your site
    cleanPublic = "true"

    // the minimum and maximum for the viewport resizer
    ishMinimum = "240"
    ishMaximum = "2600"

    // which, if any, controls to hide in the nav, separate with a comma
    ishControlsHide = "hay"

    // the order of pattern states, css class names
    patternStates = "inprogress,inreview,complete"

    // the pattern types that shouldn't be included in the style guide, useful if you nest pages/templates
    styleGuideExcludes = ""

    // should the cache buster be on, set to false to set the cacheBuster value to 0
    cacheBusterOn = "true"

## Integration with Compass

**Note:** _These directions incomplete. They are not meant to imply that Compass is officially supported with Pattern Lab. They should be modified to fit your instance of the PHP version of Pattern Lab._

Setting up Compass to work with the PHP version of Pattern Lab should be really straightforward. To set-up a Compass config that uses SCSS and _doesn't_ install any starter stylesheets do the following:

1. Open Terminal on a Mac
2. `gem install compass` (if you don't have it)
3. `cd <patternlab-project-folder>/source`
4. `compass create --bare --sass-dir "css" --css-dir "css" --javascripts-dir "js" --images-dir "images"`

The directories provided in step #4 are based on the default install of the PHP version of Pattern Lab and should be updated to reflect your directory structure. Also, if you need Compass to watch other directories or implement features modify step #4 as appropriate.

### Workflow with Pattern Lab

Compass will only recompile your SCSS. To get Pattern Lab to rebuild your entire site as well as reload the browser when your SCSS files have been updated do the following:

1. Open Terminal on a Mac
2. `cd <patternlab-project-folder>`
3. `compass watch source`
4. Open a new tab in Terminal
5. `php core/builder.php -wr`
6. Reload your browser

As you make changes to the SCSS files Compass will recompile them and, seeing the changes to `styles.css`, the PHP version of Pattern Lab will rebuild the entire site. It should also reload the Pattern Lab website.

## Integration with Grunt

**Note:** _These directions may be incomplete. They also require **v0.7.9** of the PHP version of Pattern Lab._

Setting up Grunt to work with the PHP version of Pattern Lab should be straightforward. To do so please do the following:

1. Open a terminal window
2. Type `npm install --save-dev grunt-shell` to install [grunt-shell](https://github.com/sindresorhus/grunt-shell)
3. Add the following to your `grunt.initConfig`. The `-p` flag ensures that Pattern Lab only generates patterns.

    shell: {
      patternlab: {
        command: "php core/builder.php -gp"
      }
    }

1. Add `grunt.loadNpmTasks('grunt-shell');` to your list of plugins.
2. Add `'shell:patternlab'` to your list of tasks in `grunt.registerTask`.

You should also be using `grunt-contrib-watch` to monitor changes to Pattern Lab's patterns and data. The Pattern Lab section for your `watch` might look like:

    html: {
      files: ['source/_patterns/**/*.mustache', 'source/_patterns/**/*.json', 'source/_data/*.json'],
      tasks: ['shell:patternlab'],
      options: {
        spawn: false
      }
    }

You might be able to use `livereload` as well but that hasn't been tested by us yet.
