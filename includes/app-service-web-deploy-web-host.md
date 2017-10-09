### <a name="app-service-plan"></a>App Service-plan
Skapar hello service-plan som värd för hello webbprogrammet. Du anger hello namnet på hello plan via hello **hostingPlanName** parameter. hello finns hello planen hello samma plats som används för hello resursgruppen. hello prisnivå nivå och worker storlek har angetts i hello **sku** och **workerSize** parametrar

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

