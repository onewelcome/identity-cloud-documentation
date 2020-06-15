# Public docs parent
[![Documentation Status](https://readthedocs.com/projects/onegini-onegini-public-docs-parent/badge/?version=latest&token=c64f387fcc48becb6c9e51883f501ebf351e220e3fb2a39a720f28f24b68ddb6)](https://onegini-onegini-public-docs-parent.readthedocs-hosted.com/en/latest/?badge=latest)

The parent docs site that is deployed to [readthedocs.com](https://readthedocs.com). Readthedocs supports a hierarchy of docs sites. This project is the root of that hierarchy.

On this website the custom domain is configured so all other websites deployed to readthedocs.com can benefit from this setup.

## Why is this project an empty mkdocs site?

Since a project deployed to readthedocs has to be a mkdocs website, but we don't want to store our documentation landing page here. Therefore this project is an empty mkdocs website that only redirects the index.html page to our actual documentation landing page.
