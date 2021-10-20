# Evoko theme for Zendesk Guide

This repo contains the [Evoko support frontend](https://support.evoko.se/hc/en-us) running on the Zendesk Guide platform.

The theme is based of the default [Zendesk Copenhagen theme](https://github.com/zendesk/copenhagen_theme) which is designed to be responsive and accessible. Check out the [Using themes and customizing your Help Center](https://support.zendesk.com/hc/en-us/sections/206670747) section and [developer documentation](https://developer.zendesk.com/apps/docs/help-center-templates/introduction) to learn more.

## Project structure

When importing a theme to Zendesk Guide it will mainly look for the following files and folders:

- [`templates/`](#templates) - contains all markup.
- [`style.css`](#styles) - contains all CSS.
- [`script.js`](#scripts) - main script file.
- [`assets/`](#assets-folder) - assets such as scripts or fonts.
- [`manifest.json`](#manifest-file) - project metadata and settings.
- [`settings/`](#settings-folder) - files to be used in settings in [`manifest.json`](manifest.json).

Other files and folders can be added to the project but will (to my knowledge) be ignored when importing.

### Templates

For markup Zendesk Guide uses [Handlebars](https://handlebarsjs.com/) and each template is stored in the [`templates/`](templates/) folder. All available templates for a Zendesk Guide theme that has all the features enabled are included however not all of them are used in our case (like "Community").

- Article page ([`article_page.hbs`](templates/article_page.hbs))
- Category page ([`category_page.hbs`](templates/category_page.hbs))
- Community post list page ([`community_post_list_page.hbs`](templates/community_post_list_page.hbs))
- Community post page ([`community_post_page.hbs`](templates/community_post_page.hbs))
- Community topic list page ([`community_topic_list_page.hbs`](templates/community_topic_list_page.hbs))
- Community topic page ([`community_topic_page.hbs`](templates/community_topic_page.hbs))
- Contributions page ([`contributions_page.hbs`](templates/contributions_page.hbs))
- Document head ([`document_head.hbs`](templates/document_head.hbs))
- Error page ([`error_page.hbs`](templates/error_page.hbs))
- Footer ([`footer.hbs`](templates/footer.hbs))
- Header ([`header.hbs`](templates/header.hbs))
- Home page ([`home_page.hbs`](templates/home_page.hbs))
- New community post page ([`new_community_post_page.hbs`](templates/new_community_post_page.hbs))
- New request page ([`new_request_page.hbs`](templates/new_request_page.hbs))
- Requests page ([`request_page.hbs`](templates/request_page.hbs))
- Search results page ([`search_results.hbs`](templates/search_results.hbs))
- Section page ([`section_page.hbs`](templates/section_page.hbs))
- Subscriptions page ([`subscriptions_page.hbs`](templates/subscriptions_page.hbs))
- User profile page ([`user_profile_page.hbs`](templates/user_profile_page.hbs))

Additionally you can add up to 10 optional templates for:

- Article page
- Category page
- Section page

You do this by creating files under the folders `templates/article_pages/`, `templates/category_pages/` or `templates/section_pages/`.
Learn more [here](https://support.zendesk.com/hc/en-us/articles/360001948367).

### Styles

The styles that Zendesk Guide will read are in the [`style.css`](style.css) file located in the project root.

For development this project uses the CSS preprocessor [Sass](https://sass-lang.com/) with the `.scss` syntax where we split styles into [Sass partials](https://sass-lang.com/guide#topic-4). All the partials are put under the [`styles/`](styles/) folder and included in the [`index.scss`](styles/index.scss) which is then compiled to [`style.css`](style.css).

`styles/_partial.scss` 🡆 `styles/index.scss` 🡆 `style.css`

### Scripts

The main JavaScript file [`script.js`](script.js) is located in the project root and will be added in the document `<body>` of every page.

JavaScript that you do not think belong on all pages can be added inline in [templates](#templates) or added as regular `*.js` files in the [assets folder](#assets-folder) and then be included in the appropriate template(s).

### Assets folder

You can add assets as images and files to the [`assets/`](assets/) folder and use them in your CSS and templates, for example:

```
# template
{{asset 'cat-image.jpg'}}

# css
$assets-cat-image-jpg
```

The assets will be uploaded to Zendesk CDN (`theme.zdassets.com`). You can read more about assets [here](https://support.zendesk.com/hc/en-us/articles/115012399428).

### Manifest file

The [`manifest.json`](manifest.json) contains theme metadata and allows you to define a group of settings for your theme that can then be changed via the UI in theming center.
You can read more about the manifest file [here](https://support.zendesk.com/hc/en-us/articles/115012547687).

### Settings folder

If you have a `type` variable of `file`, you need to provide a default file for that variable in the [`settings/`](settings/) folder. This file will be used on the settings panel by default and users can upload a different file if they like.
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

And this would look for a file inside the [`settings/`](settings/) folder named `background_image`.

## Developing

You can use your favorite IDE to develop and preview your changes locally in a web browser using the Zendesk Apps Tools (ZAT) which is installed as a Ruby gem. For more details, see [Previewing theme changes locally](https://support.zendesk.com/hc/en-us/articles/115012793547).

To avoid having to enter your Zendesk credentials every time you start your development environment you can create a `.zat` file in the project root which contains your credentials in json format, for example like this:

```json
{
  "subdomain": "erm",
  "username": "john.doe@evoko.se/token",
  "password": "YOUR_API_TOKEN"
}
```

Once you have ZAT setup make sure [Node.js](https://nodejs.org/) is installed and then use the below commands:

```bash
# install dependencies
npm install

# start the development server
npm start
```

## Deploying

For deploying changes to production we use the [Zendesk GitHub integration](https://support.zendesk.com/hc/en-us/community/posts/360004400007), the workflow can be summarized to:

1. Increment the `version` in [`manifest.json`](manifest.json) (without this Zendesk won't recognize an update).
2. Commit your changes and merge your branch to the main branch.
3. In the [Zendesk Guide theming center](https://support.evoko.se/theming) press **Update from GitHub**.
4. Changes should now be live! 🎉
