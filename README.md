# HTML / C# Azure Static Web App Template

Minimal set of files required to start a Static Web App project.


## Running SWA locally

Dependencies for running locally:

- [.NET sdk 8.0 (dotnet-sdk-8.0)](https://learn.microsoft.com/en-gb/dotnet/core/install/)
- [Azure Functions Core Tools (azure-functions-core-tools-4)](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local)
- [Azure Static Web Apps CLI](https://github.com/Azure/static-web-apps-cli)


Create the file api/local.settings.json with the default contents given below.
Do not commit this file (it may be used to store secrets; the provided gitignore
already excludes it).
```
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated"
  }
}
```


Run local SWA cli emulator with:

```
swa start www --api-location api
```

Note: as of April 2024, swa cli emulator
[still does not observe trailingSlash setting](https://github.com/Azure/static-web-apps-cli/issues/418)
in local.settings.json - requested routes such as /example will result
in 404 instead of serving /example.html as expected.


## Publishing to Azure via GitHub

- Create private GitHub repository for application
- Commit and push project to GitHub
- Create Static Web App in Azure, with following settings:
  - Under "Deployment details" select the created repository
  - Under "Build details" select Build Presets `HTML`
  - App location: `/www`
  - Api location: `/api`
  - Output location: `/`

Once Static Web App has deployed, any required settings/environment variables
can be added via "Settings | Environment Variables > [+ Add application setting]".
These likely duplicate any settings the project adds to api/local.settings.json
beyond the default given above.


## Steps used to create this template

Working in VSCode with Azure Functions extension installed:

- Create api and www directories
- [Azure icon] > Workspace > [Azure Functions icon] > Create Function...
- Select the folder: `Browse...` > *select the api/ directory just created*
- Select a language: `C#`
- Select a .NET runtime: `.NET 8.0 Isolated LTS`
- Select a template: `HTTP trigger`
- Provide a function name: `HttpTrigger1`
- Provide a namespace: `Company.Function`
- AccessRights: `Anonymous`
- Modify response in HttpTrigger1 to say hello
- Add staticwebapp.config.json to www directory
- Add index.html to www directory
- Modify .gitignore and move from api/ directory to project root
- Remove generated .vscode directory and project.sln file


## License

This template project is licensed under the MIT No Attribution license.
