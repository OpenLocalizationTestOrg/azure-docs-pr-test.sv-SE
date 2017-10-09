<span data-ttu-id="6bdff-101">tootag en resurs under distributionen, lägga till hello `tags` elementet toohello resursen som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="6bdff-101">tootag a resource during deployment, add hello `tags` element toohello resource you are deploying.</span></span> <span data-ttu-id="6bdff-102">Ange hello taggnamn och värde.</span><span class="sxs-lookup"><span data-stu-id="6bdff-102">Provide hello tag name and value.</span></span>

### <a name="apply-a-literal-value-toohello-tag-name"></a><span data-ttu-id="6bdff-103">Använd ett litteralvärde toohello taggnamn</span><span class="sxs-lookup"><span data-stu-id="6bdff-103">Apply a literal value toohello tag name</span></span>
<span data-ttu-id="6bdff-104">hello följande exempel visas ett lagringskonto med två taggar (`Dept` och `Environment`) som är inställda tooliteral värden:</span><span class="sxs-lookup"><span data-stu-id="6bdff-104">hello following example shows a storage account with two tags (`Dept` and `Environment`) that are set tooliteral values:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

### <a name="apply-an-object-toohello-tag-element"></a><span data-ttu-id="6bdff-105">Använd ett objekt toohello taggen element</span><span class="sxs-lookup"><span data-stu-id="6bdff-105">Apply an object toohello tag element</span></span>
<span data-ttu-id="6bdff-106">Du kan definiera ett objekt som parameter som lagrar flera taggar och använda objektet toohello taggelementet.</span><span class="sxs-lookup"><span data-stu-id="6bdff-106">You can define an object parameter that stores several tags, and apply that object toohello tag element.</span></span> <span data-ttu-id="6bdff-107">Varje egenskap i objektet hello blir en separat tagg för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="6bdff-107">Each property in hello object becomes a separate tag for hello resource.</span></span> <span data-ttu-id="6bdff-108">hello följande exempel har en parameter med namnet `tagValues` som är kopplade toohello taggen element.</span><span class="sxs-lookup"><span data-stu-id="6bdff-108">hello following example has a parameter named `tagValues` that is applied toohello tag element.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "tagValues": {
      "type": "object",
      "defaultValue": {
        "Dept": "Finance",
        "Environment": "Production"
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": "[parameters('tagValues')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {}
    }
  ]
}
```

### <a name="apply-a-json-string-toohello-tag-name"></a><span data-ttu-id="6bdff-109">Använda en JSON-sträng toohello taggnamn</span><span class="sxs-lookup"><span data-stu-id="6bdff-109">Apply a JSON string toohello tag name</span></span>

<span data-ttu-id="6bdff-110">toostore många värden i en enda tagg gäller en JSON-sträng som representerar hello värden.</span><span class="sxs-lookup"><span data-stu-id="6bdff-110">toostore many values in a single tag, apply a JSON string that represents hello values.</span></span> <span data-ttu-id="6bdff-111">hello hela JSON-strängen lagras som en tagg som får innehålla högst 256 tecken.</span><span class="sxs-lookup"><span data-stu-id="6bdff-111">hello entire JSON string is stored as one tag that cannot exceed 256 characters.</span></span> <span data-ttu-id="6bdff-112">hello följande exempel har en enda tagg med namnet `CostCenter` som innehåller flera värden från en JSON-sträng:</span><span class="sxs-lookup"><span data-stu-id="6bdff-112">hello following example has a single tag named `CostCenter` that contains several values from a JSON string:</span></span>  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "tags": {
        "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```