# Azure Mobile Services for Unity3d
For game developers looking to use Azure Mobile Services or Azure App Services in their Unity3D project.

Also available as a [MobileServices Demo Project](https://github.com/Unity3dAzure/MobileServicesDemo) which will run inside UnityEditor and on iOS, Android and Windows. The demo project has got everything bundled in and does not require any additional assets to work. If you want to integrate with an existing Unity3d project then follow instructions below:

## How to add MobileServices into Unity3d project
1. [Download UnityRestClient](https://github.com/ProjectStratus/UnityRestClient/archive/master.zip)
 	* Copy JsonFx 'plugins' and the UnityRestClient 'Source' into project `Assets` folder
2. [Download MobileServices](https://github.com/Unity3dAzure/MobileServices/archive/master.zip)  
	* Copy 'MobileServices' into project `Assets` folder.
3. Create an [Azure Mobile Service](https://manage.windowsazure.com) or an [Azure App Service](https://portal.azure.com)
	* Create a table for app data.

## Supported Features
### MobileServiceClient
API | Description
--- | -----------
Login | Client-directed login using access token.
InvokeApi | Get custom API

### MobileServiceTable
API | Description
--- | -----------
Insert | Create a new item.
Read | Get a list of items.
Update | Update an item’s data using id property.
Delete | Delete an item using id property.  
Query | Get a list of results using a custom query.
Lookup | Get an item’s data using id property.

### MobileServiceClient Interface
	void Login(MobileServiceAuthenticationProvider provider, string token, Action<IRestResponse<MobileServiceUser>> callback = null);
	void InvokeApi<T>(string apiName, Action<IRestResponse<T>> callback = null) where T : new();

### MobileServiceTable Interface
	void Insert<T>(T item, Action<IRestResponse<T>> callback = null) where T : new();
	void Read<T>(Action<IRestResponse<List<T>>> callback = null) where T : new();
	void Update<T>(T item, Action<IRestResponse<T>> callback = null) where T : new();
	void Delete<T>(string id, Action<IRestResponse<T>> callback = null) where T : new();
	void Query<T>(CustomQuery query, Action<IRestResponse<List<T>>> callback = null) where T : new();
	void Lookup<T>(string id, Action<IRestResponse<T>> callback = null) where T : new();


## Sample usage
```
using UnityEngine;
using System;
using System.Net;
using System.Collections.Generic;
using RestSharp;
using Pathfinding.Serialization.JsonFx;
using Unity3dAzure.MobileServices;

```

```
private MobileServiceClient _client;
private MobileServiceTable<TodoItem> _table;
```

```
void Start () {
	_client = new MobileServiceClient(appUrl, appKey); // <- add your app connection strings here.
	_table = _client.GetTable<TodoItem>("TodoItem");
}
```
```
private void ReadItems() {
	_table.Read<TodoItem>(OnReadItemsCompleted);
}

private void OnReadItemsCompleted(IRestResponse<List<TodoItem>> response) {
	if ( response.StatusCode == HttpStatusCode.OK) {
		Debug.Log("OnReadItemsCompleted data: " + response.Content);
		List<TodoItem> items = response.Data;
		Debug.Log( "Todo items count: " + items.Count);
	} else {
		ResponseError err = JsonReader.Deserialize<ResponseError>(response.Content);
		Debug.Log("Error " + err.code.ToString() + " " + err.error + " Uri: " + response.ResponseUri);
	}
}
```

## Dependencies
The REST service implements [UnityRestClient](https://github.com/ProjectStratus/UnityRestClient) which uses [JsonFx](https://bitbucket.org/TowerOfBricks/jsonfx-for-unity3d-git/) to parse JSON data.

Questions or tweet #Azure #GameDev [@deadlyfingers](https://twitter.com/deadlyfingers)
