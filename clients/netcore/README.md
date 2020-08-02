# . NET Core clients

## Usage 

### Install
The clients for .NET Core are available as a NuGet package (requires netstandard2.0 or higher):

| Package | Release version |
|-|-|
| [Tributech.Dsk.Api.Clients](https://www.nuget.org/packages/Tributech.Dsk.Api.Clients) | https://img.shields.io/nuget/v/Tributech.Dsk.Api.Clients |

### Authentication / Authorization
You can use the included [APIAuthHandler](./APIAuthHandler.cs) as demonstrated in [the .NET Core Usage Example](../../examples/netcore).

``` csharp
var authHandler = new APIAuthHandler(tokenUrl, scope, clientId, clientSecret);
```

You will need to provide the following parameters:
| Parameter | Value | Remark |
|-|-|-|
| tokenUrl | https://id.your-hub.dataspace-hub.com/connect/token | Url to retrieve the access token from the dataspace hub | 
| scope | data-api-endpoint / trust-api-endpoint | Depending on which Api you wish to access either data-api-endpoint or trust-api-endpoint |
| clientId | your-node-specific-api-client | Can be found in the Dataspace Admin
| clientSecret | your-node-specific-api-client-secret | Can be found in the Dataspace Admin

This authHandler instance can then be used to create a HttpClient

``` csharp
var authorizedHttpClient = new HttpClient(authHandler);
```

### Interacting with the Apis

Create a DataAPIClient or TrustAPIClient using the authorizedHttpClient from the previous step. Note that you have to pass in the baseUrl. 

| Parameter | Value | Remark |
|-|-|-|
| baseUrl | http://data-api.your-node.dataspace-node.com / http://trust-layer-api.your-node.dataspace-node.com | Data Api / Trust Api Endpoint Url for the node you wish to integrate with | 

``` csharp
var apiClient = new DataAPIClient(baseUrl, authorizedHttpClient);

// or for the Trust Api:
var trustApiClient = new TrustAPIClient(baseUrl, authorizedHttpClient);
```

Now you can access all methods of the respective Api client like following:
``` csharp
// Get values within the last 7 days
ICollection<ReadValueDoubleModel> data = await apiClient.GetValuesAsDoubleAsync(dataStreamId, DateTime.Now, DateTime.Now.AddDays(-7), fromSyncNumber: null, "asc", pageNumber: null, pageSize: null);
```