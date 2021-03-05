
## How do I upload a template? 


###  Preparing git repository

    When preparing the git Repository please use the proper directory structure.   
        
        ├── cim
            ├── extension-resources    
               ├── email-templates
               ├── messages
               ├── static
               |   ├── css
               |   ├── images
               |   └── js
               └── templates
                   └── personal
        ├── access
            ├── <template-set-name>
                ├── 
            ├── default
                ├── 
            ├── messages
        ├── delegation
            ├── locales
                ├── <locale code>
                    ├── translation.json
            ├── logo.png
            ├── palette.json


The `cim` directory contains login templates and messages. Within the `extension-resources` folder any of the following folders can be added:

  - Email templates: contains templates for emails.
  - Messages: contains key-value messages in multiple languages.
  - Static: contains js and css, images and other static resources.
  - Templates: contains templates displayed in the frontend.
  
The `access` directory contains template sets used to change look and feel of your web applications and mobile clients. Refer to the Access component 
documentation to learn more about switching the template set per client and contents of a particular template set. The name of the directory is resolved as the 
name of the particular template set. If the template cannot be found in the directory of the chosen set, the content will be served from the `default` directory.
The `messages` subdirectory contains message bundles that will be loaded by the Access Engine application. Please note that there is a single messages location
only for all template sets.  

The `delegation` directory contains resources that allow to customize web experience in DABP. The following resources can be customized:

  - Translations: full translations file is required, please refer to [the example](translation.json)
  - Logo: use `logo.png` file
  - Application theme: use `palette.json` file, [more information available here](https://material-ui.com/customization/theming/)

### Create git repository connection
1. Log into **Onegini Customer Identity Access Manager** >`System` > `Frontend styling`.
2. The window *Frontend styling* opens: <br><br>
![ERS1](./images/ers_1.png)
1. Fill in the following fields:

    | Field|Description   |
    |---|---|
    |  **Remote location** | Define the remote location to clone the Git Repository. Only https:// is supported.   | 
    |  **No authentication required** | If this option is enabled, username and password are not mandatory and the Git Repository will be public. This option is disabled by default which means that the Git Repository is private and protected by a username and password.
    |**Username**| The username for the git repository.|
    |**Password**| The password for the git repository. Instead of a password a GitHub personal access token can be generated here: [Personal access tokens](https://github.com/settings/tokens). |

2. Click [Next: select reference]. (*The form has been validated and stored.*) 

### Select reference

1. After completing the steps under [Create Git repository connection](#create-git-repository-connection), the *Select a Reference*-window opens: <br> <br>
![Selectareference](./images/Selectareference.png)

6. In the 'Select a branch' or 'Select a tag' field, all the available remote tags (when working in a staging or production environment) and branches (when working in a development or testing environment) can be selected. The first tag or branch in the list will be preselected.
7. When the correct tag or branch has been selected, click [Save]. 
   *The branch or tag is stored and sychronization will happen automatically*.
8. After saving, the overview page will be displayed:

![ERS1](./images/ers1.png)

The overview page shows the following information:

| Field|Description   |
|---|---|
|  **Clone URL** | The remote URL.| 
|  **Authentication** | The authentication method, for example 'basic'.|
|**Username**| The username if the authentication method is "Username and password".|
|**Branch or Tag**| The selected branch or tag. |
|**Last checked**| The last time the resources have been fetched from the remote repository.|

#### Edit connection   
To edit the Git repository connection click [Edit connection]. 

#### Change reference
To change the reference click [Change reference].

### Synchronize

To synchronize a Git Repository, click [Synchronize] in the overview page:

![ERS1](./images/ers1.png)

- During synchronization the external resources (html/css/images) are fetched from the selected Git repository and stored on the server. 
- After synchronization the templates have been uploaded and can be tested, for example by loading the login page.

!!!important 
    On the production environment changes in the templates will only be visible after restarting CIM. Please ask your contact within Onegini.

