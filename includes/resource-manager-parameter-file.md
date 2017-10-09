<span data-ttu-id="a4052-101">Om du använder en fil toopass parametern parametervärden under distributionen måste toocreate en JSON-fil med ett format liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a4052-101">If you use a parameter file toopass parameter values during deployment, you need toocreate a JSON file with a format similar toohello following example:</span></span>

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

<span data-ttu-id="a4052-102">hello storlek hello parametern får inte vara mer än 64 KB.</span><span class="sxs-lookup"><span data-stu-id="a4052-102">hello size of hello parameter file cannot be more than 64 KB.</span></span>

<span data-ttu-id="a4052-103">Om du behöver tooprovide något känsligt värde för en parameter (till exempel ett lösenord), lägger du till att värdet tooa nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a4052-103">If you need tooprovide a sensitive value for a parameter (such as a password), add that value tooa key vault.</span></span> <span data-ttu-id="a4052-104">Hämta hello nyckelvalv under distributionen enligt hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="a4052-104">Retrieve hello key vault during deployment as shown in hello previous example.</span></span> <span data-ttu-id="a4052-105">Mer information finns i [skicka säkra värden under distributionen av](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="a4052-105">For more information, see [Pass secure values during deployment](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span> 

