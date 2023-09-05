## Release date 2023-09-05
### Bugs
* Fixed a bug where the Access Engine Cache was not cleared on a template synchronization.

## 1.9.0
### Improvements
* New UI for configuring repository connection
* Added timezone information on the `Last synchronisation`
* Added `Synchronisation mode` configuration option.

## 1.8.0
### Improvements
* Better integration into the console-ui
### Bug fixes
* Fix for git sync error status code

## 1.7.1
### Bug fixes
* Fix for configuration synchronisation API call from UI (axios) where an empty request body was used.

## 1.7.0
### Improvements
* Branches that were configured and then deleted can now be updated
* Spring boot and other dependency updates

## 1.6.0
### Improvements
* Updated the front-end to be compatible with tenant selection from the console-ui
* The console-ui sidebar is now passed to the parent micro-frontend for display

## 1.5.0
### Improvements
* New Onegini logo
* Customer Self Styling will trigger a cache clear in Onegini Access Engine
* Application responds on both `/` and `/self-styling` for backwards compatibility

## 1.4.0
### Improvements
* Close connection when file is done downloading from s3.
* Add a bigger http connection pool.
* Prevent raising alerts based on application logs in flows that are considered expected in certain conditions.
* Application can now be integrated with Onegini's new multi-tenant administration console.

## 1.3.0
### Improvement
* Cypress integration test added
* Support multiple configuration types
* Update resource endpoint with option to return only folders
* Dependency upgrades

## 1.2.0
### Improvements
* Last checked is now always updated when synchronize process is called. 

## 1.1.0

### Improvements
* When changing a tag or branch, the call to synchronize the templates is done without waiting for it in the Reference screen.
* In the overview screen there is a notification when the synchronisation of the templates is finished or if it failed.


## 1.0.0

Onegini is pleased to present Version 1.0.0 of the Customer Self Styling feature. 
The feature consists of two parts: 'Frontend styling' functionality in the 'Onegini Customer Access and Identity Manager' and the tool 'Onegini Connect Template Styling'.
