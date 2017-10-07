---
title: "aaaViews i hanteringslösningar för Operations Management Suite (OMS) | Microsoft Docs"
description: "En eller flera vyer toovisualize data inkluderar vanligtvis hanteringslösningar i Operations Management Suite (OMS).  Den här artikeln beskriver hur tooexport vyn skapats av hello Vydesigner och inkludera den i en lösning. "
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 303861465014a27289f831332b3d95925c0ae66d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Vyer i Operations Management Suite (OMS) hanteringslösningar (förhandsgranskning)
> [!NOTE]
> Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen. Ett schema som beskrivs nedan är ämne toochange.    
>
>

[Lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions.md) inkluderar vanligtvis en eller flera vyer toovisualize data.  Den här artikeln beskriver hur tooexport vyn skapats av hello [Vydesigner](../log-analytics/log-analytics-view-designer.md) och inkludera den i en lösning.  

> [!NOTE]
> hello exempel i den här artikeln använder parametrar och variabler är antingen obligatorisk eller vanliga toomanagement lösningar som beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du redan är bekant med för[skapar en lösning för](operations-management-suite-solutions-creating.md) och hello struktur för en lösningsfil.

## <a name="overview"></a>Översikt
tooinclude en vy i en lösning som du skapar en **resurs** för den i hello [lösningsfilen](operations-management-suite-solutions-creating.md).  hello JSON som beskriver hello visa detaljerad konfiguration är komplex men och inte något att en typisk lösning författare skulle vara kan toocreate manuellt.  hello vanligaste metoden är toocreate hello vy med hello [Vydesigner](../log-analytics/log-analytics-view-designer.md), exportera den och Lägg sedan till den detaljerade konfiguration toohello lösningen.

hello grundläggande steg tooadd en vy tooa lösning är som följer.  Varje steg beskrivs detaljerat i hello avsnitten nedan.

1. Exportera hello visa tooa fil.
2. Skapa hello visa resurs i hello-lösning.
3. Lägg till hello visa information.

## <a name="export-hello-view-tooa-file"></a>Exportera hello visa tooa fil
Följ instruktionerna för hello på [Log Analytics Vydesigner](../log-analytics/log-analytics-view-designer.md) tooexport en vy tooa-fil.  hello exporterade filen ska vara i JSON-format med hello samma [element som hello lösningsfilen](operations-management-suite-solutions-solution-file.md).  

Hej **resurser** element av hello visa filen har en resurs med en typ av **Microsoft.OperationalInsights/workspaces** som representerar hello OMS-arbetsyta.  Det här elementet har ett underelement av typen **vyer** som representerar hello vyn och innehåller detaljerade konfigurationen.  Du kopierar hello information om det här elementet och kopierar den till din lösning.

## <a name="create-hello-view-resource-in-hello-solution"></a>Skapa hello visa resurs i hello-lösning
Lägg till följande visa resursen toohello hello **resurser** element i din lösningsfil.  Den använder variabler som beskrivs nedan som du måste också lägga till.  Observera att hello **instrumentpanelen** och **OverviewTile** egenskaper är platshållare som kommer att skrivas över med hello motsvarande egenskaper från hello exporterade visa filen.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

Lägg till följande variabler toohello variabler element av hello lösningsfilen hello och Ersätt hello värden toothose för lösningen.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of hello view."
    "ViewName": "Provide a name for hello view here."


Observera att du kan kopiera hello hela vyn resurs från din exporterade visa filen, men du behöver toomake hello följande ändringar för den toowork i din lösning.  

* Hej **typen** för hello vyn resursbehov toobe har ändrats från **vyer** för**Microsoft.OperationalInsights/workspaces**.
* Hej **namn** egenskapen för hello visa resursen måste toobe ändras tooinclude hello arbetsytans namn.
* hello beroende hello arbetsytan måste toobe tas bort eftersom hello arbetsytan resursen har inte definierats i hello-lösning.
* **DisplayName** egenskapen behov toobe läggs toohello vyn.  Hej **Id**, **namn**, och **DisplayName** måste matcha alla.
* Parameternamn måste ändras toomatch hello krävs uppsättning parametrar.
* Variabler ska definieras i hello lösning och används i hello lämpliga egenskaper.

## <a name="add-hello-view-details"></a>Lägg till hello Visa detaljer
hello visa resurs i hello exporteras visa filen innehåller två element i hello **egenskaper** element med namnet **instrumentpanelen** och **OverviewTile** som innehåller hello detaljerade konfigurationen av hello vyn.  Kopiera dessa två element och deras innehåll till hello **egenskaper** element av hello visa resurs i din lösningsfilen.

## <a name="example"></a>Exempel
Till exempel visar hello som följande exempel en enkel lösning-fil med en vy.  Ellipserna (...) visas för hello **instrumentpanelen** och **OverviewTile** innehållet utrymme skäl.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Nästa steg
* Lär dig mer detaljerad information om hur du skapar [hanteringslösningar](operations-management-suite-solutions-creating.md).
* Inkludera [Automation-runbooks i din lösning](operations-management-suite-solutions-resources-automation.md).
