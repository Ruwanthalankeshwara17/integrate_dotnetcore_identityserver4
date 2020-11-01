# Integrate identityserver4 and dotnet-core 3.1

# Used Technologies

1. dotnetcore 3.1
2. IdentityServer4

# Quick Started

1. Checkout the code.
2. Open a cmd inside the folder where you cloned the code.
3. Run 'dotnet run --project ./Identity.csproj' or 'dotnet run'
4. Open a browser and type 'https://localhost:5000'

# Getting Started

Create new Asp.Net Core Web Application project using VisualStudio2019 IDE. After that select empty template and make sure the ‘Configure for HTTPS’ is ticked. 
Same can be achieved by executing the follwing command 'dotnet run web'

Remove IIS profile from launchSettings.json file. Then change the 'applicationUrl' to https://localhost:5000.

Install IdentityServer4 package.

![idebtity-server-4](./images/identityserver4.PNG)

After that installation, IdentityServer need to be added to application by modifying the ConfigureServices method in Startup.cs file.

![configure-service](./images/configureservice.PNG)

Above of code will register the IdentityServer4 in Dependancy Injection Container.

![configure](./images/configure.PNG)

Additionally, IdentityServer need to be added to the request pipeline by modifying the Configure method in Startup.cs file. By doing so IdentityServer start handling the OAuth and OpenId Connect requests.

Next step is to add configurations related to Users, Clients, IdentityResources etc. For that new folder 'Configuration' need to be added to the solution at the root level. Then create a class 'Config.cs' inside that folder.

Clients

Let IdentityServrer know which clients have access to it. Here the Client Credentials grant type is used. Allowed scopes defines the allowed list of permissions which the client can request from IdentityServer.

![client](./images/client.PNG)

Identity Resources.

Identity resource allows client to view subset of claims about user. Here first 3 are standard OpenId Connect scopes while the last one is a custom identity resource 'role'.

![identity-resource](./images/identity-resource.PNG)

Api Resources,

An Api resource defines one Api that IdentityServer protects.

![api-resources](./images/api-resources.PNG)

Api Scopes

An API scope defines individual authorization level on an API that client application can request. In this case scopes are api.read and api.write.

![scopes](./images/scopes.PNG)

Users.

![users](./images/users.PNG)

Add following namespaces in-order to get rid of the errors.

![using](./images/using-config.cs.PNG)

Finally modify the ConfigureServices method in Startup.cs file.

![configureservice-v2](./images/configureservice-v2.PNG)

That's it, time for testing.

First visit the discovery document (https://localhost:5000/.well-known/openid-configuration) and check the 'scopes_supported' and 'claims_supported'. Supported scopes include 'api.read', 'api.write' and 'role' while supported claims contains the custom claim 'role' as well.

Next try to grab an access token through postman by filling the values shown in the image. 

![postman](./images/postman.PNG)

Finally decode the token to see the claims using jwt.io.

![jwt](./images/jwt.PNG)


