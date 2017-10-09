Om du använder en fil toopass parametern parametervärden under distributionen måste toocreate en JSON-fil med ett format liknande toohello följande exempel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

hello storlek hello parametern får inte vara mer än 64 KB.

Om du behöver tooprovide något känsligt värde för en parameter (till exempel ett lösenord), lägger du till att värdet tooa nyckelvalvet. Hämta hello nyckelvalv under distributionen enligt hello föregående exempel. Mer information finns i [skicka säkra värden under distributionen av](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

