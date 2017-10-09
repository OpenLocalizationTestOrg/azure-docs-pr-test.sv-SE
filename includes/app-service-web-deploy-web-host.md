### <a name="app-service-plan"></a><span data-ttu-id="bb510-101">App Service-plan</span><span class="sxs-lookup"><span data-stu-id="bb510-101">App Service plan</span></span>
<span data-ttu-id="bb510-102">Skapar hello service-plan som värd för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="bb510-102">Creates hello service plan for hosting hello web app.</span></span> <span data-ttu-id="bb510-103">Du anger hello namnet på hello plan via hello **hostingPlanName** parameter.</span><span class="sxs-lookup"><span data-stu-id="bb510-103">You provide hello name of hello plan through hello **hostingPlanName** parameter.</span></span> <span data-ttu-id="bb510-104">hello finns hello planen hello samma plats som används för hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="bb510-104">hello location of hello plan is hello same location used for hello resource group.</span></span> <span data-ttu-id="bb510-105">hello prisnivå nivå och worker storlek har angetts i hello **sku** och **workerSize** parametrar</span><span class="sxs-lookup"><span data-stu-id="bb510-105">hello pricing tier and worker size are specified in hello **sku** and **workerSize** parameters</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

