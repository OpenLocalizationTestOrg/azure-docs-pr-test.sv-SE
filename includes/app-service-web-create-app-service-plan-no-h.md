<span data-ttu-id="80e03-101">Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="80e03-101">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span>

[!INCLUDE [app-service-plan](app-service-plan.md)]

<span data-ttu-id="80e03-102">hello följande exempel skapas en apptjänstplan med namnet `myAppServicePlan` i hello **lediga** prisnivån:</span><span class="sxs-lookup"><span data-stu-id="80e03-102">hello following example creates an App Service plan named `myAppServicePlan` in hello **Free** pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

<span data-ttu-id="80e03-103">När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="80e03-103">When hello App Service plan has been created, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```