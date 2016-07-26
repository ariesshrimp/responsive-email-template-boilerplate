# Mailchimp Template System

## What is this repo?
This repo is a build system for generating new HTML emails, with an emphasis on reusable templates that can be edited from within the Mailchimp.com UI.  

The `gulpfile.babel.js` compiles the code written in `src/` into `dist/`. As a developer, you would write or modify files in `src/`, then run npm tasks that generate new `dist/` files. You could then upload those files to Mailchimp as new campaigns (one off emails), or as new generic templates for interchangeable use. This readme explains the basics of that process. Unfortunately it is subject to change depending on Mailchimp's UI future, and product/stake-holder needs.

## Where are the emails?
An HTML email is composed of the HTML in `layouts/default.html`. That file imports / embeds any partials like our generic footer or logo as Handlebars templates. The main body of any email is imported as a Handlebars template written in `src/pages`.  

The `src/pages` templates represent the interchangeable content of particular emails, or types of email.

+ *If you are writing a brand new kind of email template*, or you have been asked to write a very specific email body that is distinct from any existing template, you would add a page, or edit one of the existing pages.  

+ *If you are editing the generic styles or content of the template layout (_like the footer or logo_)*, you would edit files in `src/partials` and / or `src/layouts/default.html`.  

## Build Process  
### Dev Preview
To build an e-mail and preview it in a local browser environment, save your changes and run `npm run build` from the root directory (`templates/`). This will compile all of the Handlebars and Sass, and serve up a browser preview. It will reload on change.  

### Production Upload  
To build a production ready zip file for Mailchimp, run `npm run zip` to inline all the stuff into a zip file. You'll find it by the same name as your `src/pages/*.html` file in `dist/`

## Mailchimp syntax  
The current (6/15/2016) Mailchimp templating syntax is to add `mc:edit=""` attributes to any elements that you want Mailchimp.com users to be free to edit from the website's UI. Every name should be unique. For example:  

```
<row>
    <columns>
        <h6 mc:edit="intro">Write an intro header</h6>
        <h1 mc:edit="intro">Main thing you're announcing!</h1>
    </columns>
</row>
```

Would have the undesired effect that end-users could edit these blocks, _but any edit to one would also edit the other_.

The name's given to `mc:edit=""` values can be arbitrary, as long as they are unique.

## I'm nervous about breaking existing templates
These changes are not tracked on Mailchimp.com. Anything you break will only be propagated if you build the broken template and upload it to Mailchimp. Even then, you would have to overwrite or delete the existing Mailchimp.com templates for serious damage to be done.  

## My CSS is not working  
It's very likely that the CSS feature you're using works in your local browser environment, but not in some target email client. For example, `height` and `padding` are not well supported in Microsoft Outlook clients.  

You should expect the Zurb Foundation framework employed by this build process to help you very little in that regard. Study the existing Sass files carefully, and assume that anything there exists for a reason.  

*For this reason, it is important that you document any additions with explanatory comments!* There is no good central resource for exactly what CSS and HTML you can and can't use for your intended audience.
