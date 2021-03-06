# Coding

## Project structure
* `base/` Where vanilla Bootstrap is located. Contains the original Bootstrap SASS partials (`scss/vendor/bootstrap/`) but also custom partials (`scss/overrides`) and mixins (`scss/mixins`) that allow using Bootstrap alongside the original CiviCRM style
* `css/` Contains the two post-processed css files, `bootstrap.css` and `custom-civicrm.css`
* `fonts/` Font files used by the theme
* `js/` Any custom javascript code that the theme relies on
* `scss/` Contains the SASS partials that make up Shoreditch. It's split into two sub-folders
  * `scss/civicrm` Contains the style that the theme applies to the existing core screens, in order to make them look like as if they were also Bootstrap-based pages.
  * `scss/bootstrap` Contains the custom Bootstrap theme. In here not only the base Bootstrap theme is customized, but also Bootstrap elements are augmented with new modifiers, and completely custom component are added.

## Build
If you are doing development on this extension, then you may need to build
new CSS files. This requires the toolchain for SCSS=>CSS compilation.

First of all you need [NodeJS](https://nodejs.org/). Please be aware that currently Shoreditch *does not* support NodeJS v9 or higher! It is recommended that you install NodeJS LTS version **v8.9.4** (you can use [nvm](https://github.com/creationix/nvm) for managing multiple node versions on the same machine.

Once you have NodeJS installed, run

```sh
npm install
```

Once you have the tools, you can run `npx gulp watch`. This will monitor the SCSS files and automatically recompile whenever they are changed.

```sh
# npx command ensures you run a local repository Gulp and not the global one
npx gulp watch
```

If you would like to just compile files without watching, simply run `npx gulp`.

```sh
npx gulp
```

## Guidelines for `custom-civicrm.css`
Any style changes that are aimed at making the core screens look like they are part of the Bootstrap theme, should go here.

### Disable stylelint rules
Any .scss file under the `civicrm/` folder must have this annotation at the very top

```scss
/* stylelint-disable max-nesting-depth, selector-max-compound-selectors, selector-no-qualifying-type, selector-max-id */
```

in order to tell [stylelint](https://stylelint.io/) to be more a bit more lax when enforcing style conventions


*TBD*

## Guidelines for `bootstrap.css`
While `custom-civicrm.css` can be thought of as the "legacy mode" of Shoreditch, this component can be considered as the actual theme.

There are three main types of changes you might want to introduce to the theme
1. **Tweak the overall look-and-feel.** *(i.e., color palette, border radius, line height)* These are usually accomplished by working on the `overrides/_variables.scss` partial, as Bootstrap "exposes" many basic traits of the style as SASS variables.
2. **Customize a Bootstrap component** *(i.e., changing the padding of `.panel`, adding a custom variation of `.btn`)* You  can do that in the `overrides/_variables.scss` file, or if Bootstrap doesn't provide a SASS variable for the change you need to implement, you can write the overriding style in the `overrides/style/_{component}.scss` partial.
3. **Add a custom component** *(i.e., wizard component)* There are some component in CiviCRM that do not have any equivalent in the Bootstrap component library, and as such they have to be written from scratch. Those components go in the `components/` folder, each in an individual partial.

### Things to keep in mind

#### Keep the exceptions to a minimum
Shoreditch is first and foremost a customized Bootstrap theme, and as such it should not deviate from the original Bootstrap as much as possible.

Any custom variable, override, modifier, or component should be considered exceptions to the basic theme and should be justified and carefully planned, as each adds complexity that over time would make the theme harder to maintain and to work on.

#### Use original Bootstrap variables as much as possible
Bootstrap comes packaged with plenty of SASS variables that covers many of the traits of the whole theme and of the individual components (like `$padding-base-vertical`, `$modal-inner-padding`, `$badge-font-weight`, etc), so always make sure to check first if the changes that you want to implement can be done so by simply tweaking a variable value.

#### Prefix the custom variables
Sometimes you might need to add custom SASS variables to the `overrides/_variable.scss` partial. In order to differentiate them from the original variables, always prefix them with `$crm-` (`$crm-badge-min-width`, `$crm-gray-lightest`, etc).

#### Match the overriding partials with the original partials
The `overriding/style` folder contains the partials that are meant to override and augment the original style, and the partials are organized so that they reflect the structure in `base/vendor/bootstrap/`.

If you need to add a partial in `overriding/style` make sure that it is matching one of the originals.

#### CSS naming convention
For custom modifiers, stick with the dasherized bootstrap convention. So `.panel-bigger`, `.badge-square`, etc

For custom components, use the [BEM](http://getbem.com/) convention, making sure to prefix the block name with `crm_`. So `.crm_custom-component`, `.crm_foo__bar--baz`, etc.

#### Add a custom component as a last resort
Adding a custom components to achieve a specific design should be considered as a last resort, when no suitable Bootstrap component had been found that could either be used as-is or being augmented with a custom modifier.

#### No ad-hoc changes
The theme is meant to be as generic as possible, and as such no site/extension specific changes and/or components should be added to it.
