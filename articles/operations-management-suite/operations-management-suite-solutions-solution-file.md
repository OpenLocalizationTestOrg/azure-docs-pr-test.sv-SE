---
title: "aaaCreating hanteringslösningar i Operations Management Suite (OMS) | Microsoft Docs"
description: "Hanteringslösningar utöka hello funktioner i Operations Management Suite (OMS) genom att tillhandahålla paketerade hanteringsscenarier som kunder kan lägga till tootheir OMS-arbetsyta.  Den här artikeln innehåller information om hur du kan skapa management lösningar toobe används i din egen miljö eller göras tillgänglig tooyour kunder."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f408df1b21f519fd1eb2cbeb19cca18f6c4161f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a>Skapa en management lösningsfilen i Operations Management Suite (OMS) (förhandsgranskning)
> [!NOTE]
> Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen. Ett schema som beskrivs nedan är ämne toochange.  

Lösningar för hantering i Operations Management Suite (OMS) implementeras som [Resource Manager-mallar](../azure-resource-manager/resource-manager-template-walkthrough.md).  hello huvudsakliga uppgiften lär du dig hur tooauthor hanteringslösningar learning hur för[skapar en mall](../azure-resource-manager/resource-group-authoring-templates.md).  Den här artikeln innehåller unik information om mallar som används för lösningar och hur tooconfigure vanliga lösning resurser.


## <a name="tools"></a>Verktyg

Du kan använda alla text editor toowork med lösningsfiler, men vi rekommenderar att utnyttja hello-funktioner som finns i Visual Studio eller Visual Studio Code som beskrivs i följande artiklar hello.

- [Skapa och distribuera Azure-resursgrupper via Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Arbeta med Azure Resource Manager-mallar i Visual Studio Code](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a>struktur
hello grundstrukturen för ett management lösningsfilen är hello samma som en [Resource Manager-mall](../azure-resource-manager/resource-group-authoring-templates.md#template-format) som är som följer.  Hello avsnitten nedan beskrivs hello toppnivå element och och innehållet i en lösning.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametrar
[Parametrarna](../azure-resource-manager/resource-group-authoring-templates.md#parameters) är värden som du behöver från hello användare när de installerar hello hanteringslösning.  Det finns standardparametrar som har alla lösningar och du kan lägga till ytterligare parametrar som krävs för din lösning.  Hur användare ange parametervärden när de installerar lösningen beror på hello viss parameter och hur hello lösningen installeras.

När en användare installerar din lösning för hantering via hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) eller [Azure-snabbstartsmallar](operations-management-suite-solutions.md#finding-and-installing-management-solutions) ange tooselect är en [OMS arbetsytan och Automation-konto ](operations-management-suite-solutions.md#oms-workspace-and-automation-account).  Detta är används toopopulate hello värden för varje hello-standardparametrar.  hello användaren behöver inte ange toodirectly ange värden för standard hello-parametrar, men de kan ange tooprovide värden för alla ytterligare parametrar.

När hello användaren installerar lösningen [en annan metod](operations-management-suite-solutions.md#finding-and-installing-management-solutions), måste de ange ett värde för alla parametrar som standard och alla ytterligare parametrar.

En exempel-parametern visas nedan.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

hello i den följande tabellen beskrivs hello attributen för en parameter.

| Attribut | Beskrivning |
|:--- |:--- |
| typ |Datatypen för hello-parametern. hello inkommande kontrollen visas för användaren hello beror på hello-datatypen.<br><br>bool - listrutan<br>String - textruta<br>int - textruta<br>SecureString - fält för lösenord<br> |
| category |Valfri kategori för hello-parametern.  Parametrar i hello samma kategori grupperas tillsammans. |
| Kontrollen |Ytterligare funktioner för string-parametrar.<br><br>datetime - Datetime kontrollen visas.<br>GUID - Guid-värde genereras automatiskt och visas inte hello-parametern. |
| description |Valfri beskrivning för hello-parametern.  Visas i en information pratbubblor nästa toohello-parameter. |

### <a name="standard-parameters"></a>Standard-parametrar
hello visas följande tabell hello-standardparametrar för alla hanteringslösningar för.  Dessa värden är fyllda för hello användaren i stället för att fråga om dem när lösningen har installerats från hello Azure Marketplace eller Quickstart mallar.  användaren hello måste ange värden för dem om hello lösningen har installerats med en annan metod.

> [!NOTE]
> hello användargränssnittet i hello mallar för Azure Marketplace och Quickstart förväntas hello parameternamn i hello tabell.  Om du använder olika parameternamn sedan hello användaren uppmanas att dem och fylls de inte automatiskt.
>
>

| Parameter | Typ | Beskrivning |
|:--- |:--- |:--- |
| Kontonamn |Sträng |Azure Automation-kontonamn. |
| pricingTier |Sträng |Prisnivån för både logganalys-arbetsytan och Azure Automation-konto. |
| regionId |Sträng |Region i hello Azure Automation-konto. |
| SolutionName |Sträng |Namnet på hello lösning.  Om du distribuerar din lösning med snabbstartsmallar bör sedan du definiera solutionName som en parameter där du kan definiera en sträng i stället kräver hello användaren toospecify en. |
| workspaceName |Sträng |Log Analytics arbetsytans namn. |
| workspaceRegionId |Sträng |Region i hello logganalys-arbetsytan. |


Följande är hello strukturen för hello-standardparametrar som du kan kopiera och klistra in i din lösningsfilen.  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of hello Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of hello Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


Du referera tooparameter värden i andra element för hello-lösning med hello syntax **parametrar (parametern name)**.  Till exempel tooaccess hello arbetsytans namn, använder du **parameters('workspaceName')**

## <a name="variables"></a>Variabler
[Variabler](../azure-resource-manager/resource-group-authoring-templates.md#variables) är värden som du ska använda i hello resten av hello hanteringslösning.  Dessa värden är inte synliga toohello användare som installerar hello lösning.  De är avsedda tooprovide hello författare med en enda plats där de kan hantera värden som kan användas flera gånger i hela hello lösning. Du bör placera alla värden specifika tooyour lösningar på variabler som skillnad från toohard kodning dem i hello **resurser** element.  Det gör det lättare att läsa hello koden och att du tooeasily ändra dessa värden i senare versioner.

Följande är ett exempel på en **variabler** element med vanliga parametrar som används i lösningar.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Du referera toovariable värden via hello-lösning med hello syntax **variabler (variabel namn)**.  Till exempel tooaccess hello SolutionName variabeln, använder du **variables('SolutionName')**.

Du kan också definiera komplex variabler som flera uppsättningar av värden.  Detta är särskilt användbart i hanteringslösningar där du definierar flera egenskaper för olika typer av resurser.  Du kan till exempel omstrukturera hello lösning variabler ovan toohello följande.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

I så fall måste du referera toovariable värden via hello-lösning med hello syntax **variables('variable name').property**.  Till exempel tooaccess hello lösningsnamn variabeln, använder du **variables('Solution'). Namnet**.

## <a name="resources"></a>Resurser
[Resurser](../azure-resource-manager/resource-group-authoring-templates.md#resources) definiera hello olika resurser som din lösning ska installeras och konfigureras.  Det här är hello största och mest komplexa del av hello mallen.  Du kan hämta hello struktur och fullständig beskrivning av resursen element i [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md#resources).  Olika resurser som du vanligtvis definierar beskrivs i andra artiklar i den här dokumentationen. 


### <a name="dependencies"></a>Beroenden
Hej **dependsOn** element anger en [beroende](../azure-resource-manager/resource-group-define-dependencies.md) på en annan resurs.  När hello lösningen installeras, skapas inte en resurs tills alla dess beroenden har skapats.  Till exempel din lösning kan [startar en runbook](operations-management-suite-solutions-resources-automation.md#runbooks) när den är installerad med hjälp av en [jobbet resurs](operations-management-suite-solutions-resources-automation.md#automation-jobs).  hello jobbet resurs skulle vara beroende av hello runbook resurs toomake till att hello runbook har skapats innan hello jobb skapas.

### <a name="oms-workspace-and-automation-account"></a>OMS-arbetsytan och Automation-konto
Av hanteringslösningar kräver en [OMS-arbetsytan](../log-analytics/log-analytics-manage-access.md) toocontain vyer och en [Automation-konto](../automation/automation-security-overview.md#automation-account-overview) toocontain runbooks och relaterade resurser.  Dessa måste vara tillgänglig innan hello resurser i hello lösningen skapas och ska inte definieras i själva hello-lösningen.  hello användaren kommer [ange arbetsytan och kontot](operations-management-suite-solutions.md#oms-workspace-and-automation-account) när de distribuerar din lösning, men som hello författare bör du hello följande punkter.

## <a name="solution-resource"></a>Lösning för resurs
Varje lösning kräver att en resurs i hello **resurser** element som definierar själva hello-lösningen.  Det har en typ av **Microsoft.OperationsManagement/solutions** och ha hello följande struktur. Detta inkluderar [standardparametrar](#parameters) och [variabler](#variables) som är vanliga toodefine egenskaper för hello lösning.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Beroenden
hello lösning resursen måste ha en [beroende](../azure-resource-manager/resource-group-define-dependencies.md) på alla andra resurser i hello lösning eftersom de behöver tooexist innan hello-lösning kan skapas.  Det gör du genom att lägga till en post för varje resurs i hello **dependsOn** element.

### <a name="properties"></a>Egenskaper
hello resurs har hello egenskaper i hello i den följande tabellen.  Detta inkluderar hello resurser refereras och finns i hello-lösning som definierar hur hello resursen hanteras när hello lösningen har installerats.  Varje resurs i hello lösningen ska visas i antingen hello **referencedResources** eller hello **containedResources** egenskapen.

| Egenskap | Beskrivning |
|:--- |:--- |
| workspaceResourceId |ID för hello logganalys-arbetsytan i form av hello  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Arbetsytenamn\>*. |
| referencedResources |Lista över resurser i hello-lösning som inte tas bort när hello lösningen tas bort. |
| containedResources |Listan över resurser i hello-lösning som ska tas bort när hello lösningen tas bort. |

hello-exemplet ovan är för en lösning med en runbook ett schema och visa.  hello-schemat och runbook är *refererade* i hello **egenskaper** elementet så att de inte tas bort när hello lösningen tas bort.  hello är *innehöll* så att den tas bort när hello lösningen tas bort.

### <a name="plan"></a>Planera
Hej **plan** entiteten av hello resurs har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| namn |Namnet på hello lösning. |
| Version |Version av hello lösningen utifrån hello författare. |
| Produkten |Unik sträng tooidentify hello lösning. |
| Publisher |Utgivaren av hello lösning. |



## <a name="sample"></a>Exempel
Du kan visa prover av lösningsfiler med en resurs på hello följande platser.

- [Automation-resurser](operations-management-suite-solutions-resources-automation.md#sample)
- [Sök och avisering resurser](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Nästa steg
* [Lägg till sparade sökningar och aviseringar](operations-management-suite-solutions-resources-searches-alerts.md) tooyour hanteringslösning.
* [Lägga till vyer](operations-management-suite-solutions-resources-views.md) tooyour hanteringslösning.
* [Lägg till runbooks och andra resurser för Automation](operations-management-suite-solutions-resources-automation.md) tooyour hanteringslösning.
* Läs hello information om [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).
* Sök [Azure Quickstart mallar](https://azure.microsoft.com/documentation/templates) exempel på olika Resource Manager-mallar.
