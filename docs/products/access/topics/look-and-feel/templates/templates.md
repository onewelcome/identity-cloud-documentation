# Templates

OneWelcome Access uses the [Thymeleaf](https://www.thymeleaf.org/) template engine to serve pages to the end user via a browser. The templates can be customized to
align their look and feel with your mobile application or company standards.

## Customizing templates

Access uses template sets that contain one or more custom templates. The default template is used when a specific template is not available in the
template set. One custom template set can be marked as the default template set for all applications and web clients.

Customized templates can be uploaded and managed via the [Self Styling application](../../../../../self-styling). To add the new template set, follow
the [directory structure](../../../../../self-styling/configuration-page.md)
in the configured git repository. Refer to the [default template set](./content/access-engine-templates.zip) to further customize templates.

In the templates, the [Thymeleaf Layout Dialect](https://github.com/ultraq/thymeleaf-layout-dialect) and
[Thymeleaf With Dialect](https://github.com/Antibrumm/thymeleaf-extras-with-dialect) are supported. The template file names are hardcoded and should not be changed in
the custom template set. The internals of the template, like message keys and fragments, can be different. For more details about the different templates
supported by Access see the [templates section in the appendix](../../../appendix/templates/templates.md).

## Configuring an application-specific template set

For each mobile application and web client, a template set can be configured to render screens in the look and feel of this application or web client. When no
template set is configured for an application or web client. The default template set is used to render these screens.

![App template set](img/app-template-set.png)

## Resolving templates

Access resolves templates in the following order:

1. The template set configured for the application or web client of the current request.
2. When the template is not present in this set, the default template set.
3. When no default template set is available, we fall back to the templates bundled with Access.

## Hidden form inputs

Some templates and fragments contain a hidden form input to submit a Cross-Site Request Forgery (CSRF) token. These fields are required for the application to function properly. Do not remove these input fields from the templates.  

## Static resources

An application server like OneWelcome Access is not optimized for server static resources like javascript, images, and CSS files. Therefore we do not
support serving custom static resources. Some options on how to use custom static resources:

- Embed the static resources in the HTML of the templates. The downside is that the client cannot cache the static resource.
- Serve the static resources via your own content management system.
