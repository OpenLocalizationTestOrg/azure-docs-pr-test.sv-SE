
Skapa en [API-app](../articles/app-service-api/app-service-api-apps-why-best-platform.md) i hello `myAppServicePlan` App Service-plan med hello [az webapp skapa](/cli/azure/appservice/web#create) kommando. 

hello webbprogrammet här värd för ditt API och ger en URL tooview hello distribuerad app.

Följande kommando, Ersätt i hello  *\<appnamn >* med ett unikt namn. Om `<app_name>` är inte unikt felmeddelande hello ”webbplats med namnet < programnamn > finns redan”. Hej standard webbadressen hello webbprogrammet `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel:

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