
<span data-ttu-id="6499e-101">Skapa en [API-app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) i hello `myAppServicePlan` App Service-plan med hello [az webapp skapa](/cli/azure/appservice/web#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="6499e-101">Create a [API app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) in hello `myAppServicePlan` App Service plan with hello [az webapp create](/cli/azure/appservice/web#create) command.</span></span> 

<span data-ttu-id="6499e-102">hello webbprogrammet här värd för ditt API och ger en URL tooview hello distribuerad app.</span><span class="sxs-lookup"><span data-stu-id="6499e-102">hello web app provides a hosting space for your API and provides a URL tooview hello deployed app.</span></span>

<span data-ttu-id="6499e-103">Följande kommando, Ersätt i hello  *\<appnamn >* med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="6499e-103">In hello following command, replace *\<app_name>* with a unique name.</span></span> <span data-ttu-id="6499e-104">Om `<app_name>` är inte unikt felmeddelande hello ”webbplats med namnet < programnamn > finns redan”.</span><span class="sxs-lookup"><span data-stu-id="6499e-104">If `<app_name>` is not unique, you get hello error message "Website with given name <app_name> already exists."</span></span> <span data-ttu-id="6499e-105">Hej standard webbadressen hello webbprogrammet `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="6499e-105">hello default URL of hello web app is `https://<app_name>.azurewebsites.net`.</span></span> 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="6499e-106">När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="6499e-106">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span>

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