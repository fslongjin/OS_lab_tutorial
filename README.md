Example [GitBook] website using GitLab with [Netlify](https://www.netlify.com/).

## Netlify Configuration

In order to build this site with Netlify, simply log in or register at 
https://app.netlify.com/, then select "New site from Git" from the top
right. Select GitLab, authenticate if needed, and then select this
project from the list. 

You will need to set the build command to `gitbook build` and set the publish
directory to `_book`. Netlify will handle the rest.

In the meantime, you can take advantage of all the great GitLab features
like merge requests, issue tracking, epics, and everything else GitLab has
to offer.

## Building locally

To work locally with this project, you'll have to follow the steps below:

1. Fork, clone or download this project
1. [Install][] GitBook `npm install gitbook-cli -g`
1. Fetch GitBook's latest stable version `gitbook fetch latest`
1. Preview your project: `gitbook serve`
1. Add content
1. Generate the website: `gitbook build` (optional)
1. Push your changes to the master branch: `git push`

Read more at GitBook's [documentation].

## Did you fork this project?

If you forked this project for your own use, please go to your project's
**Settings** and remove the forking relationship, which won't be necessary
unless you want to contribute back to the upstream project.

## Troubleshooting

1. CSS is missing! That means two things:

    Either that you have wrongly set up the CSS URL in your templates, or
    your static generator has a configuration option that needs to be explicitly
    set in order to serve static assets under a relative URL.

----

Forked from @virtuacreative

[GitBook]: https://www.gitbook.com/
[host the book]: https://gitlab.com/pages/gitbook/tree/pages
[install]: http://toolchain.gitbook.com/setup.html
[documentation]: http://toolchain.gitbook.com
