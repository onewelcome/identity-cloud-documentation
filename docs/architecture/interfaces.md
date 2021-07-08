The Onegini Identity cloud has several different (programming) interfaces that are used by clients to customize our solution or integrate our platform into their software. This paragraph explains how Onegini manages these interfaces when releasing new software versions. The general rule is that customers should have the least possible impact when Onegini releases new software versions.  

Everything explained below is to make sure that there are no disruptions due to changes made to the software by Onegini when a new software version is deployed. 

# Interface Lifecycle

Onegini will manage its interfaces in accordance with a interface lifecycle. Over time there will be new interfaces, but Onegini will also remove outdated interfaces. Clients will be informed in due time.  

# Backwards compatibility 

Onegini guarantees that its interfaces remain backwards compatible when releasing a new version of its software. Over time we may break backwards compatibility, but we will only do so after giving multiple notifications that a particular interface version is deprecated. More on deprecation can be read in the next paragraph. 

The general concept around backwards compatibility for our interfaces is as follows: Each interface is versioned and Onegini will not make breaking changes to existing stable interface versions. 

Onegini distinguishes different types of interfaces. For each of these interfaces backwards compatibility is guaranteed in a slightly different way. 

## Templates

The Onegini Identity cloud uses templates to render the UI in the end-users’ browser. These templates can be modified by either customers or our Professional Services department. To guarantee backwards compatibility on a template version, Onegini will: 

- not rename templates 
- not rename or remove variables used in templates to fill them with user specific data 
- not rename or remove variables used to fetch translations 

Onegini can introduce new templates or new variables, or translations to existing templates. These variables or translations are connected with a new feature that, if not enabled, will not impact existing template customizations. 

## APIs

The Onegini Identity cloud exposes many APIs that allow customers to fully integrate our platform in their software. This also means that Onegini will improve / change these APIs. To guarantee backwards compatibility APIs are versioned and for stable versions we will: 

- not rename or remove fields in the API request or response 
- not add or remove required fields / parameters in the API request 

Onegini may introduce new parameters or fields in an API request or response, as long as these are not required. This for example means that the software that interacts our APIs must be able to deal with new fields returned in an API response. This is a common practice when consuming APIs. 

## SDKs

The Onegini Identity Cloud has several (mobile) SDKs. These SDKs make it more convenient to integrate your software with our platform. All SDKs are versioned according to the Semantic versioning scheme ([https://semver.org](https://semver.org/)). This means that breaking changes will only be introduced in new major versions of the SDKs. 
