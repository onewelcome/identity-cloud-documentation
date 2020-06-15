# Public docs parent

The parent docs site that is deployed to [readthedocs.com](https://readthedocs.com). Readthedocs supports a hierarchy of docs sites. This project is the root of that hierarchy.

On this website the custom domain is configured so all other websites deployed to readthedocs.com can benefit from this setup.

## Why is this project an empty mkdocs site?

Since a project deployed to readthedocs has to be a mkdocs website, but we don't want to store our documentation landing page here. Therefore this project is an empty mkdocs website that only redirects the index.html page to our actual documentation landing page.
