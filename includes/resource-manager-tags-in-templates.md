tootag en resurs under distributionen, lägga till hello `tags` elementet toohello resursen som du distribuerar. Ange hello taggnamn och värde.

### <a name="apply-a-literal-value-toohello-tag-name"></a>Använd ett litteralvärde toohello taggnamn
hello följande exempel visas ett lagringskonto med två taggar (`Dept` och `Environment`) som är inställda tooliteral värden:

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

### <a name="apply-an-object-toohello-tag-element"></a>Använd ett objekt toohello taggen element
Du kan definiera ett objekt som parameter som lagrar flera taggar och använda objektet toohello taggelementet. Varje egenskap i objektet hello blir en separat tagg för hello resurs. hello följande exempel har en parameter med namnet `tagValues` som är kopplade toohello taggen element.

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

### <a name="apply-a-json-string-toohello-tag-name"></a>Använda en JSON-sträng toohello taggnamn

toostore många värden i en enda tagg gäller en JSON-sträng som representerar hello värden. hello hela JSON-strängen lagras som en tagg som får innehålla högst 256 tecken. hello följande exempel har en enda tagg med namnet `CostCenter` som innehåller flera värden från en JSON-sträng:  

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