# Evoko theme for Zendesk Guide

This repository contains the [Evoko support website frontend](https://support.evoko.se/hc/en-us) running on the Zendesk Guide platform.

The theme is based of the default [Zendesk Copenhagen theme](https://github.com/zendesk/copenhagen_theme) which is designed to be responsive and accessible. Check out the [Using themes and customizing your Help Center](https://support.zendesk.com/hc/en-us/sections/206670747) section and [developer documentation](https://developer.zendesk.com/apps/docs/help-center-templates/introduction) to learn more.

## Project structure

When importing a theme to Zendesk Guide it will mainly look for the following files and folders:

- [`templates/`](#templates) - contains all markup.
- [`Style.css`](#styles) - contains all CSS.
- [`script.js`](#scripts) - main script file that will be included in the page `<head>`.
- [`manifest.json`](#manifest-file) - project metadata and settings.
- [`settings/`](#settings-folder) - files to be used in settings in [`manifest.json`](/blob/master/manifest.json).
- [`assets/`](#assets-folder) - assets such as scripts or images.

Other files and folders can be added to the project but will (to my knowledge) be ignored when importing.

### Templates

For markup Zendesk Guide uses [Handlebars](https://handlebarsjs.com/) and each template is stored in the [`templates/`](/blob/master/templates/) folder. All available templates for a Zendesk Guide theme that has all the features enabled are included however not all of them are used in our case (like "Community").

- Article page (`article_page.hbs`)
- Category page (`category_page.hbs`)
- Community post list page (`community_post_list_page.hbs`)
- Community post page (`community_post_page.hbs`)
- Community topic list page (`community_topic_list_page.hbs`)
- Community topic page (`community_topic_page.hbs`)
- Contributions page (`contributions_page.hbs`)
- Document head (`document_head.hbs`)
- Error page (`error_page.hbs`)
- Footer (`footer.hbs`)
- Header (`header.hbs`)
- Home page (`home_page.hbs`)
- New community post page (`new_community_post_page.hbs`)
- New request page (`new_request_page.hbs`)
- Requests page (`request_page.hbs`)
- Search results page (`search_results.hbs`)
- Section page (`section_page.hbs`)
- Subscriptions page (`subscriptions_page.hbs`)
- User profile page (`user_profile_page.hbs`)

Additionally you can add up to 10 optional templates for:

- Article page
- Category page
- Section page

You do this by creating files under the folders `templates/article_pages/`, `templates/category_pages/` or `templates/section_pages/`.
Learn more [here](https://support.zendesk.com/hc/en-us/articles/360001948367).

### Styles

The styles that Zendesk Guide will read are in the [`style.css`](/blob/master/style.css) file located in the project root.

For development this project uses the CSS preprocessor [Sass](https://sass-lang.com/) with the `.scss` syntax where we split styles into [Sass partials](https://sass-lang.com/guide#topic-4). All the partials are put under the [`styles/`](/blob/master/styles/) folder and included in the [`index.scss`](/blob/master/styles/index.scss) which is then compiled to [`style.css`](/blob/master/style.css).

`styles/_partial.scss` 🡆 `styles/index.scss` 🡆 `style.css`

### Scripts

The main JavaScript file [`script.js`](/blob/master/script.js) is located in the project root and will be added in the document `<head>` of every page.

JavaScript that you do not think belong in the document `<head>` can be added inline in [templates](#templates) or added as regular `*.js` files in the [assets folder](#assets-folder) and then be included in the appropriate template(s).

### Assets folder

You can add assets as images and files to the [`assets/`](/blob/master/assets/) folder and use them in your CSS and templates, for example:

```shell
# template
{{asset 'cat-image.jpg'}}

# css
$assets-cat-image-jpg
```

The assets will be uploaded to Zendesk CDN (`theme.zdassets.com`). You can read more about assets [here](https://support.zendesk.com/hc/en-us/articles/115012399428).

### Manifest file

The [`manifest.json`](/blob/master/manifest.json) contains theme metadata and allows you to define a group of settings for your theme that can then be changed via the UI in theming center.
You can read more about the manifest file [here](https://support.zendesk.com/hc/en-us/articles/115012547687).

### Settings folder

If you have a `type` variable of `file`, you need to provide a default file for that variable in the [`settings/`](/blob/master/settings/) folder. This file will be used on the settings panel by default and users can upload a different file if they like.
For Example, if you'd like to have a variable for the background image of a section, the variable in your manifest file would look something like this:

```json
{
  ...
  "settings": [{
    "label": "Images",
    "variables": [{
      "identifier": "background_image",
      "type": "file",
      "description": "Background image for X section",
      "label": "Background image",
    }]
  }]
}
```

And this would look for a file inside the [`settings/`](/blob/master/settings/) folder named `background_image`.

## Developing

To start working, clone the repository (`git clone https://github.com/rottbers/zendesk-theme.git`) and create a feature/bug branch (e.g. `git checkout -b feature/that-new-feature` or `bug/fixing-that-bug`).

### Local previewing

You can use your favorite IDE to develop and preview your changes locally in a web browser using the Zendesk Apps Tools (ZAT) which is installed as a Ruby gem. For more details, see [Previewing theme changes locally](https://support.zendesk.com/hc/en-us/articles/115014810447).

To avoid having to enter your Zendesk credentials every time you start your development environment you can create a `.zat` file in the project root which contains your credentials in json format, for example like this:

```json
{
  "subdomain": "erm",
  "username": "john.doe@evoko.se/token",
  "password": "YOUR_API_TOKEN"
}
```

### npm tools

To make use of some development tools you will need to have [Node.js](https://nodejs.org/) installed and then run the below command:

```shell
npm install
```

In [`package.json`](/blob/master/package.json) you will find a list of the dev dependencies along with some [npm scripts](https://docs.npmjs.com/misc/scripts.html). Example on how to run a script:

```shell
# Watch Sass changes and write to style.css
npm run styles:watch

# Complies Sass, minify and autoprefix to style.css
npm run styles:build
```

## Deploying

For deploying changes to production we use the [Zendesk GitHub integration](https://support.zendesk.com/hc/en-us/community/posts/360004400007), the workflow can be summarized to:

1. Increment the `version` in [`manifest.json`](/blob/master/manifest.json) (without this Zendesk won't recognize an update).
2. Commit and merge your branch(es) to the master branch.
3. In the [Zendesk Guide theming center](https://support.evoko.se/theming) press **Update from GitHub**.
4. Changes should now be live! 🎉

If you'd like to test your changes on a live website (instead of [Local previewing](#local-previewing)) before updating production then you could keep the changes on a separate branch than master and import the theme to the [Sandbox environment](https://erm.zendesk.com/agent/admin/sandbox).
