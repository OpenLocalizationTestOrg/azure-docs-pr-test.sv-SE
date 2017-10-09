---
title: aaaAzure Resource Manager mallstruktur och syntax | Microsoft Docs
description: "Beskriver hello struktur och egenskaperna för Azure Resource Manager-mallar med deklarativ JSON-syntax."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0709852f8777c91cc1704d6bca16257a017d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-structure-and-syntax-of-azure-resource-manager-templates"></a>Förstå hello struktur och syntaxen för Azure Resource Manager-mallar
Det här avsnittet beskriver hello strukturen i en Azure Resource Manager-mall. Den visar hello olika avsnitt i en mall och hello egenskaper som är tillgängliga i dessa avsnitt. hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution. En stegvis självstudiekurs om hur du skapar en mall finns i [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md).

## <a name="template-format"></a>Mallformat
I sin enklaste strukturen innehåller en mall hello följande element:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| $schema |Ja |Sökväg till hello JSON-schema som beskriver hello-versionen av hello mallen språket. Använd hello-URL som visas i föregående exempel hello. |
| contentVersion |Ja |Version av hello mall (till exempel 1.0.0.0). Du kan ange ett värde för det här elementet. När du distribuerar resurser med hjälp av mallen hello kan värdet vara används toomake till att rätt hello-mallen används. |
| parameters |Nej |Värden som anges när distributionen är köras toocustomize resurs distribution. |
| variabler |Nej |Värden som används som JSON-fragment i hello mallen toosimplify mallspråksuttryck. |
| Resurser |Ja |Resurstyper som distribuerats eller uppdateras i en resursgrupp. |
| utdata |Nej |Värden som returneras efter distributionen. |

Varje element innehåller egenskaper som du kan ange. hello följande exempel innehåller hello fullständig syntax för en mall:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-hello parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Vi undersöka hello avsnitt i hello mallen i större detalj längre fram i det här avsnittet.

## <a name="expressions-and-functions"></a>Uttryck och funktioner
grundläggande hello-syntaxen för hello mallen är JSON. Dock utöka uttryck och funktioner hello JSON-värden som är tillgängliga inom hello mall.  Uttryck skrivs i JSON-stränglitteraler vars första och sista tecknen är hello hakparenteser: `[` och `]`respektive. hello-värdet för hello uttrycket utvärderas när hello mallen distribueras. Medan skrivs som en teckensträng kan hello resultat av utvärderingen av hello uttryck vara av en annan JSON-typ, till exempel en matris eller ett heltal, beroende på hello faktiska uttryck.  toohave som en teckensträng börja med en hakparentes `[`, men inte har det tolkas som ett uttryck, lägga till en extra hakparentes toostart hello sträng med `[[`.

Normalt använder du uttryck med funktioner tooperform åtgärder för att konfigurera hello-distribution. Precis som i JavaScript-funktionsanrop som är formaterade som `functionName(arg1,arg2,arg3)`. Du kan referera egenskaper med hjälp av hello punkt och [index] operatörer.

Hej följande exempel visar hur toouse flera fungerar när man skapar värden:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Hello fullständig lista över Mallfunktioner kan se [Azure Resource Manager Mallfunktioner](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametrar
I hello parametrar i hello mall kan ange du vilka värden som du kan ange när du distribuerar hello resurser. Dessa parametervärden aktivera toocustomize hello distributionen med värden som är anpassade för en viss miljö (t.ex dev, test- och). Du har inte tooprovide parametrar i mallen, men utan parametrar mallen skulle du alltid distribuera hello hello i samma resurser med samma namn, platser och egenskaper.

Du kan definiera parametrar med hello följande struktur:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-hello parameter>" 
        }
    }
}
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| parameterName |Ja |Namnet på hello-parameter. Måste vara en giltig JavaScript-identifierare. |
| typ |Ja |Typ av hello parametervärdet. Se hello lista över tillåtna typer efter den här tabellen. |
| Standardvärde |Nej |Standardvärdet för hello parameter om inget värde har angetts för parametern hello. |
| allowedValues |Nej |Matris med tillåtna värdena för hello parametern toomake du att rätt hello-värdet är angett. |
| MinValue |Nej |hello minimivärdet för int typparametrar, det här värdet är inklusiva. |
| MaxValue |Nej |hello högsta värdet för int typparametrar, det här värdet är inklusiva. |
| minLength |Nej |hello minimilängden för string, secureString och array typparametrar, det här värdet är inklusiva. |
| maxLength |Nej |hello maxlängden för string, secureString och array typparametrar, det här värdet är inklusiva. |
| description |Nej |Beskrivning av hello parameter visas toousers hello-portalen. |

hello tillåtna typer och värden är:

* **sträng**
* **secureString**
* **int**
* **bool**
* **objektet** 
* **secureObject**
* **matris**

toospecify en parameter som valfria, ange defaultValue (kan vara en tom sträng). 

Om du anger ett parameternamn i mallen som matchar en parameter i hello kommandot toodeploy hello mallen, finns det potentiella tvetydighet om hello-värden som du anger. Hanteraren för filserverresurser löser den här förvirring genom att lägga till hello username@Domain **från mall** toohello mallparameter. Till exempel om du lägger till en parameter med namnet **ResourceGroupName** i mallen, den orsakar en konflikt med hello **ResourceGroupName** parameter i hello [ Nya AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Under distributionen, är du tillfrågas tooprovide ett värde för **ResourceGroupNameFromTemplate**. I allmänhet bör du undvika den här förvirring genom att inte namnges parametrar med samma namn som parametrar som används för distributionsåtgärder hello.

> [!NOTE]
> Alla lösenord, nycklar och andra hemligheter ska använda hello **secureString** typen. Om du skickar känsliga data i en JSON-objekt använder hello **secureObject** typen. Mallparametrar med secureString eller secureObject typer går inte att läsa efter resurs distributionen. 
> 
> Till exempel visar hello följande post i hello distributionshistoriken hello värdet för en sträng och objekt men inte för secureString och secureObject.
>
> ![Visa värden för distribution](./media/resource-group-authoring-templates/show-parameters.png)  
>

följande exempel visar hur hello toodefine parametrar:

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

Hur tooinput hello parametern värden under distributionen finns [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md). 

## <a name="variables"></a>Variabler
Under hello variabler skapa värden som kan användas i hela din mall. Du behöver inte toodefine variabler, men de förenkla ofta din mall genom att minska komplexa uttryck.

Du kan definiera variabler med hello följande struktur:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

följande exempel visar hur hello toodefine en variabel som konstrueras utifrån två parametervärden:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

hello nästa exempel visas en variabel som är en komplex typ av JSON och variabler som skapas från andra variabler:

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>Resurser
Under hello resurser kan du definiera hello resurser som distribueras eller uppdateras. Det här avsnittet får komplicerade eftersom du måste förstå hello skriver du distribuerar tooprovide hello rätt värden. Hej resursspecifika värden (apiVersion, typ och egenskaper) som du behöver tooset hittar [definiera resurser i Azure Resource Manager-mallar](/azure/templates/). 

Du kan definiera resurser med hello följande struktur:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| Villkor | Nej | Booleskt värde som anger om hello resurs har distribuerats. |
| apiVersion |Ja |Version av hello REST API toouse för att skapa hello resurs. |
| typ |Ja |Typ av hello resurs. Det här värdet är en kombination av hello namnområde med hello resursprovidern och resurstypen hello (exempelvis **Microsoft.Storage/storageAccounts**). |
| namn |Ja |Namn på hello resurs. hello-namnet måste följa URI komponenten begränsningar som definierats i RFC3986. Dessutom Azure-tjänster som visar hello resurs namn toooutside parter Validera hello namnet toomake att den inte är en försök toospoof en annan identitet. |
| location |Det varierar |Geo-platser som stöds av hello som resurs. Du kan välja någon av hello tillgängliga platser, men vanligtvis det gör meningsfullt toopick ett som är nära tooyour användare. Vanligtvis är det också klokt tooplace resurser som samverkar med varandra i hello samma region. De flesta resurstyper kräver en plats, men vissa typer (till exempel en rolltilldelning) kräver inte en plats. Se [som resursplats i Azure Resource Manager-mallar](resource-manager-template-location.md). |
| tags |Nej |Taggar som är associerade med hello resurs. Se [tagga resurser i Azure Resource Manager-mallar](resource-manager-template-tags.md). |
| Kommentarer |Nej |Anteckningar för dokumentation hello resurser i mallen |
| Kopiera |Nej |Hej antalet resurser toocreate om mer än en instans krävs. hello standardläget är parallellt. Ange seriell läge när du inte vill att alla eller hello resurser toodeploy på hello samtidigt. Mer information finns i [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nej |Resurser som måste distribueras innan den här resursen har distribuerats. Resource Manager utvärderar hello beroenden mellan resurser och distribuerar dem i hello rätt ordning. Om resurserna inte är beroende av varandra kan distribueras de parallellt. hello värdet kan vara en kommaavgränsad lista över en resurs namn eller resurs unika identifierare. Endast lista över resurser som distribueras i den här mallen. Resurser som inte har definierats i denna mall måste redan finnas. Undvik att lägga till onödiga beroenden som de långsamma distributionen och skapa Cirkelberoenden. Information om inställningen beroenden finns [definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md). |
| properties |Nej |Resurs-specifika konfigurationsinställningar. hello-värden för egenskaper för hello hello samma som hello-värden som du anger i hello begärandetexten för hello REST-API (PUT-metoden) toocreate hello resursen. Du kan också ange en kopia matris toocreate flera instanser av en egenskap. Mer information finns i [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md). |
| Resurser |Nej |Underordnade resurser som är beroende av hello resurs som definieras. Ange endast resurstyper som tillåts av hello schemat för hello överordnade resursen. hello fullständiga typ av hello underordnade resursen innehåller hello överordnade resurstyp, t.ex. **Microsoft.Web/sites/extensions**. Beroende på hello överordnade resursen är inte underförstådd. Du måste uttryckligen definiera sambandet. |

hello resurser avsnittet innehåller en matris med hello resurser toodeploy. Du kan också definiera en matris med underordnade resurser inom varje resurs. Resurser-avsnitt kan därför ha en struktur som:

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

Mer information om hur du definierar underordnade resurser finns [ange namn och typ för underordnade resursen i Resource Manager-mall](resource-manager-template-child-resource.md).

Hej **villkoret** elementet anger huruvida hello resurs har distribuerats. hello-värdet för det här elementet löser tootrue eller false. Till exempel toospecify om ett nytt lagringskonto har distribuerats, Använd:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Ett exempel på hur du använder en ny eller befintlig resurs finns [ny eller befintlig mall för villkoret](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

toospecify om en virtuell dator distribueras med ett lösenord eller SSH-nyckeln du definierar två versioner av hello virtuell dator för i din mall och använda **villkoret** toodifferentiate användning. Skicka en parameter som anger vilket scenario toodeploy.

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

Ett exempel på med ett lösenord eller SSH-nyckel toodeploy virtuella finns [användarnamn eller SSH villkoret mallen](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="outputs"></a>utdata
Ange värden som returneras från distribution under hello utdata. Du kan till exempel returnera hello URI tooaccess en distribuerad resurs.

hello visar följande exempel hello struktur för en definition av utdata:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| outputName |Ja |Namnet på hello utdatavärde. Måste vara en giltig JavaScript-identifierare. |
| typ |Ja |Typ av hello utdatavärde. Utdatavärden stöder hello samma typer som mall indataparametrar. |
| värde |Ja |Mallspråksuttrycket som utvärderas och returneras som utdata. |

hello visar följande exempel ett värde som returneras i hello utdata avsnittet.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

Mer information om hur du arbetar med utdata finns [dela tillstånd i Azure Resource Manager-mallar](best-practices-resource-manager-state.md).

## <a name="template-limits"></a>Mallen gränser

Begränsa hello av mall-too1 MB och varje parameter fil too64 KB. hello 1 MB gränsen gäller toohello sluttillstånd för hello mall när den har utökats med iterativ resursdefinitionerna och värden för parametrar och variabler. 

Du är begränsad till:

* 256 parametrar
* 256 variabler
* 800 resurserna (inklusive antal kopior)
* 64 utdatavärden
* 24,576 tecken i ett malluttryck för

Du kan överskrida vissa mallen med hjälp av en kapslad mall. Mer information finns i [använda länkade mallar när du distribuerar Azure-resurser](resource-group-linked-templates.md). tooreduce hello antalet parametrar, variabler eller utdata, du kan kombinera flera värden i ett objekt. Mer information finns i [objekt som parametrar](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Nästa steg
* tooview fullständig mallar för många olika typer av lösningar, se hello [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates/).
* Mer information om hello-funktioner som du kan använda från i en mall finns [Azure Resource Manager mallen Functions](resource-group-template-functions.md).
* toocombine flera mallar under distributionen, se [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* Du kan behöva toouse resurser som finns i en annan resursgrupp. Det här scenariot är vanligt när du arbetar med lagringskonton eller virtuella nätverk som delas mellan flera resursgrupper. Mer information finns i hello [resourceId funktionen](resource-group-template-functions-resource.md#resourceid).
