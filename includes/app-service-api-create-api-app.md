
<span data-ttu-id="1a544-101">Skapa en [API-app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) i `myAppServicePlan` App Service-planen med kommandot [az webapp create](/cli/azure/appservice/web#create).</span><span class="sxs-lookup"><span data-stu-id="1a544-101">Create a [API app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) in the `myAppServicePlan` App Service plan with the [az webapp create](/cli/azure/appservice/web#create) command.</span></span> 

<span data-ttu-id="1a544-102">Webbappen ger dig ett lagringsutrymme för ditt API och du får en URL så att du kan visa den distribuerade appen.</span><span class="sxs-lookup"><span data-stu-id="1a544-102">The web app provides a hosting space for your API and provides a URL to view the deployed app.</span></span>

<span data-ttu-id="1a544-103">Ersätt *\<app_name>* med ett unikt namn i följande kommando.</span><span class="sxs-lookup"><span data-stu-id="1a544-103">In the following command, replace *\<app_name>* with a unique name.</span></span> <span data-ttu-id="1a544-104">Om `<app_name>` inte är unikt får du ett felmeddelande om att webbplatsen med det angivna namnet <app_name> redan finns.</span><span class="sxs-lookup"><span data-stu-id="1a544-104">If `<app_name>` is not unique, you get the error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="1a544-105">Standardwebbadressen för webbappen är `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="1a544-105">The default URL of the web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="1a544-106">När webbappen har skapats visar Azure CLI information liknande den i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1a544-106">When the web app has been created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```