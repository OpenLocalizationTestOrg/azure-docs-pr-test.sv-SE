---
title: "aaaSet distributionsordning för Azure-resurser | Microsoft Docs"
description: "Beskriver hur tooset en resurs som är beroende av en annan resurs under distributionen tooensure resurser distribueras i rätt ordning för hello."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 2f658f4c85236966c46b34a65aafb8426c92806c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="define-hello-order-for-deploying-resources-in-azure-resource-manager-templates"></a>Definiera hello ordning för att distribuera resurser i Azure Resource Manager-mallar
För en viss resurs kan det finnas andra resurser som måste finnas innan hello resurs har distribuerats. Till exempel måste en SQLServer finnas innan du försöker toodeploy en SQL-databas. Du definierar relationen genom att markera en resurs som är beroende av hello andra resurser. Du definierar ett samband med hello **dependsOn** elementet eller genom att använda hello **referens** funktion. 

Resource Manager utvärderar hello beroenden mellan resurser och distribuerar dem i ordningsföljden beroende. När resurserna inte är beroende av varandra, distribuerar dem i Resource Manager parallellt. Behöver du bara toodefine beroenden för resurser som har distribuerats i hello samma mall. 

## <a name="dependson"></a>dependsOn
Hello dependsOn-elementet låter toodefine en resurs i mallen, som beroende på en eller flera resurser. Värdet kan vara en kommaavgränsad lista över resurser. 

hello visas följande exempel en skaluppsättning för virtuell dator som är beroende av en belastningsutjämnare, virtuella nätverk och en loop som skapar flera lagringskonton. Dessa andra resurser visas inte i följande exempel hello, men de måste tooexist någon annanstans i hello mallen.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

I föregående exempel hello, finns ett beroende på hello-resurser som har skapats via en kopia skapas med namnet **storageLoop**. Ett exempel finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).

När du definierar beroenden kan du inkludera hello resource provider namnområde och resursen typen tooavoid tvetydighet. Till exempel tooclarify en belastningsutjämnare och virtuella nätverk som kan ha hello samma namn som andra resurser, Använd hello följande format:

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

Du kanske lutande toouse dependsOn toomap relationer mellan dina resurser, är det viktigt toounderstand varför du gör det. Till exempel toodocument hur resurser är sammankopplade, är dependsOn inte hello rätt metod. Du kan inte fråga vilka resurser som har definierats i hello dependsOn-elementet efter distributionen. Genom att använda dependsOn kan påverka du eventuellt distributionen eftersom Resource Manager inte distribuerar i parallella två resurser som är beroende av. toodocument relationer mellan resurser, i stället använda [resurslänkningen](/rest/api/resources/resourcelinks).

## <a name="child-resources"></a>Underordnade resurser
hello resurser egenskap kan toospecify underordnade resurser som är relaterade toohello resurs som definieras. Underordnade resurser kan bara vara definierade fem nivåers djup. Det är viktigt toonote som inte är en implicit beroende skapats mellan en underordnad resurs och hello överordnade resursen. Om du behöver hello underordnade resursen toobe distribueras efter hello överordnade resursen, måste du uttryckligen ange sambandet med hello dependsOn-egenskap. 

Varje överordnad resurs accepterar endast vissa typer av resurser som underordnade resurser. hello accepteras resurstyper som anges i hello [mallsschemat](https://github.com/Azure/azure-resource-manager-schemas) hello överordnade resurs. hello namnet på resurstypen för underordnade innehåller hello namnet på resurstypen för hello överordnade **Microsoft.Web/sites/config** och **Microsoft.Web/sites/extensions** är både underordnade resurser till hello  **Microsoft.Web/sites**.

hello som följande exempel visar en SQLServer och SQL-databas. Observera att en explicit beroende definieras mellan hello SQL database och SQLServer, trots att hello-databasen är en underordnad server hello.

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a>referens-funktion
Hej [referera funktionen](resource-group-template-functions-resource.md#reference) kan ett uttryck tooderive sitt värde från andra JSON namn och värdepar eller runtime-resurser. Referensuttryck deklarerar implicit att en resurs beror på en annan. hello allmänna format är:

```json
reference('resourceName').propertyPath
```

I följande exempel hello, en CDN-slutpunkt beror uttryckligen på hello CDN-profilen och beror implicit på ett webbprogram.

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

Du kan använda det här elementet eller hello dependsOn elementet toospecify beroenden, men du behöver inte toouse både för hello samma beroende resurs. Använd en referera implicit tooavoid lägger till en onödiga beroende om möjligt.

Det finns fler toolearn [referera funktionen](resource-group-template-functions-resource.md#reference).

## <a name="recommendations-for-setting-dependencies"></a>Rekommendationer för att ställa in beroenden

Använd hello följande riktlinjer när du bestämmer vilka beroenden tooset:

* Ange beroenden så lite som möjligt.
* Ange en underordnad resurs som är beroende av den överordnade resursen.
* Använd hello **referens** fungerar tooset implicit beroenden mellan resurser behöver tooshare en egenskap. Lägg inte till ett explicit beroende (**dependsOn**) när du redan har definierat ett indirekt samband. Den här metoden minskar hello risken att onödiga beroenden. 
* Ange ett beroende när en resurs inte får vara **skapade** utan funktioner från en annan resurs. Ange inte ett beroende om endast interagera hello resurser efter distributionen.
* Låt beroenden cascade utan att ange dem explicit. Till exempel den virtuella datorn är beroende av ett virtuellt nätverksgränssnitt och hello virtuella nätverksgränssnittet beror på ett virtuellt nätverk och offentliga IP-adresser. Därför hello virtuella datorn är distribuerad när alla tre resurser, men uttryckligen anges inte hello virtuell dator som är beroende av alla tre resurser. Den här metoden klargör hello beroendeordningen och gör det enklare toochange hello mallen senare.
* Om ett värde kan fastställas innan distribution, försök att distribuera hello resursen utan ett beroende. Om ett konfigurationsvärde måste hello namnet på en annan resurs, kanske du inte behöver ett beroende. Den här vägledningen fungerar inte alltid eftersom vissa resurser verifiera hello att hello andra resurser. Om du får ett felmeddelande, lägger du till ett beroende. 

Resource Manager identifierar cirkulärt tjänstberoende vid verifiering av mallen. Om du får ett felmeddelande om att det finns ett cirkulärt beroende, utvärdera din mall toosee om eventuella beroenden som inte behövs och kan tas bort. Om du tar bort beroenden inte fungerar kan undvika du Cirkelberoenden genom att flytta vissa distributionsåtgärder till underordnade resurser som distribueras efter hello-resurser som har hello cirkulärt beroende. Anta exempelvis att du distribuerar två virtuella datorer, men du måste ange egenskaper för var och en som refererar toohello andra. Du kan distribuera dem i hello följande ordning:

1. vm1
2. vm2
3. Tillägg på vm1 beror på vm1 och vm2. hello-tillägget anger värden för vm1 får från vm2.
4. Tillägg på vm2 beror på vm1 och vm2. hello-tillägget anger värden för vm2 får från vm1.

Information om utvärdera hello distributionsordning och beroende kodproblem finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Nästa steg
* toolearn om felsökning av beroenden under distributionen, se [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).
* toolearn om hur du skapar mallar för Azure Resource Manager finns [Webbsidemallar](resource-group-authoring-templates.md). 
* En lista över hello tillgängliga funktioner i en mall finns [Mallfunktioner](resource-group-template-functions.md).

