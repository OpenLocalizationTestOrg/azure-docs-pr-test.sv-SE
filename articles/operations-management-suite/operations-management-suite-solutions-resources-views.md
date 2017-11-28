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
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a><span data-ttu-id="c4f3a-104">Vyer i Operations Management Suite (OMS) hanteringslösningar (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="c4f3a-104">Views in Operations Management Suite (OMS) management solutions (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="c4f3a-105">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="c4f3a-106">Ett schema som beskrivs nedan är ämne toochange.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-106">Any schema described below is subject toochange.</span></span>    
>
>

<span data-ttu-id="c4f3a-107">[Lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions.md) inkluderar vanligtvis en eller flera vyer toovisualize data.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-107">[Management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions.md) will typically include one or more views toovisualize data.</span></span>  <span data-ttu-id="c4f3a-108">Den här artikeln beskriver hur tooexport vyn skapats av hello [Vydesigner](../log-analytics/log-analytics-view-designer.md) och inkludera den i en lösning.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-108">This article describes how tooexport a view created by hello [View Designer](../log-analytics/log-analytics-view-designer.md) and include it in a management solution.</span></span>  

> [!NOTE]
> <span data-ttu-id="c4f3a-109">hello exempel i den här artikeln använder parametrar och variabler är antingen obligatorisk eller vanliga toomanagement lösningar som beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="c4f3a-109">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="c4f3a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c4f3a-110">Prerequisites</span></span>
<span data-ttu-id="c4f3a-111">Den här artikeln förutsätter att du redan är bekant med för[skapar en lösning för](operations-management-suite-solutions-creating.md) och hello struktur för en lösningsfil.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-111">This article assumes that you're already familiar with how too[create a management solution](operations-management-suite-solutions-creating.md) and hello structure of a solution file.</span></span>

## <a name="overview"></a><span data-ttu-id="c4f3a-112">Översikt</span><span class="sxs-lookup"><span data-stu-id="c4f3a-112">Overview</span></span>
<span data-ttu-id="c4f3a-113">tooinclude en vy i en lösning som du skapar en **resurs** för den i hello [lösningsfilen](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="c4f3a-113">tooinclude a view in a management solution, you create a **resource** for it in hello [solution file](operations-management-suite-solutions-creating.md).</span></span>  <span data-ttu-id="c4f3a-114">hello JSON som beskriver hello visa detaljerad konfiguration är komplex men och inte något att en typisk lösning författare skulle vara kan toocreate manuellt.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-114">hello JSON that describes hello view's detailed configuration is typically complex though and not something that a typical solution author would be able toocreate manually.</span></span>  <span data-ttu-id="c4f3a-115">hello vanligaste metoden är toocreate hello vy med hello [Vydesigner](../log-analytics/log-analytics-view-designer.md), exportera den och Lägg sedan till den detaljerade konfiguration toohello lösningen.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-115">hello most common method is toocreate hello view using hello [View Designer](../log-analytics/log-analytics-view-designer.md), export it, and then add its detailed configuration toohello solution.</span></span>

<span data-ttu-id="c4f3a-116">hello grundläggande steg tooadd en vy tooa lösning är som följer.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-116">hello basic steps tooadd a view tooa solution are as follows.</span></span>  <span data-ttu-id="c4f3a-117">Varje steg beskrivs detaljerat i hello avsnitten nedan.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-117">Each step is described in further detail in hello sections below.</span></span>

1. <span data-ttu-id="c4f3a-118">Exportera hello visa tooa fil.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-118">Export hello view tooa file.</span></span>
2. <span data-ttu-id="c4f3a-119">Skapa hello visa resurs i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-119">Create hello view resource in hello solution.</span></span>
3. <span data-ttu-id="c4f3a-120">Lägg till hello visa information.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-120">Add hello view details.</span></span>

## <a name="export-hello-view-tooa-file"></a><span data-ttu-id="c4f3a-121">Exportera hello visa tooa fil</span><span class="sxs-lookup"><span data-stu-id="c4f3a-121">Export hello view tooa file</span></span>
<span data-ttu-id="c4f3a-122">Följ instruktionerna för hello på [Log Analytics Vydesigner](../log-analytics/log-analytics-view-designer.md) tooexport en vy tooa-fil.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-122">Follow hello instructions at [Log Analytics View Designer](../log-analytics/log-analytics-view-designer.md) tooexport a view tooa file.</span></span>  <span data-ttu-id="c4f3a-123">hello exporterade filen ska vara i JSON-format med hello samma [element som hello lösningsfilen](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="c4f3a-123">hello exported file will be in JSON format with hello same [elements as hello solution file](operations-management-suite-solutions-solution-file.md).</span></span>  

<span data-ttu-id="c4f3a-124">Hej **resurser** element av hello visa filen har en resurs med en typ av **Microsoft.OperationalInsights/workspaces** som representerar hello OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-124">hello **resources** element of hello view file will have a resource with a type of **Microsoft.OperationalInsights/workspaces** that represents hello OMS workspace.</span></span>  <span data-ttu-id="c4f3a-125">Det här elementet har ett underelement av typen **vyer** som representerar hello vyn och innehåller detaljerade konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-125">This element will have a subelement with a type of **views** that represents hello view and contains its detailed configuration.</span></span>  <span data-ttu-id="c4f3a-126">Du kopierar hello information om det här elementet och kopierar den till din lösning.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-126">You will copy hello details of this element and then copy it into your solution.</span></span>

## <a name="create-hello-view-resource-in-hello-solution"></a><span data-ttu-id="c4f3a-127">Skapa hello visa resurs i hello-lösning</span><span class="sxs-lookup"><span data-stu-id="c4f3a-127">Create hello view resource in hello solution</span></span>
<span data-ttu-id="c4f3a-128">Lägg till följande visa resursen toohello hello **resurser** element i din lösningsfil.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-128">Add hello following view resource toohello **resources** element of your solution file.</span></span>  <span data-ttu-id="c4f3a-129">Den använder variabler som beskrivs nedan som du måste också lägga till.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-129">This uses variables that are described below that you must also add.</span></span>  <span data-ttu-id="c4f3a-130">Observera att hello **instrumentpanelen** och **OverviewTile** egenskaper är platshållare som kommer att skrivas över med hello motsvarande egenskaper från hello exporterade visa filen.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-130">Note that hello **Dashboard** and **OverviewTile** properties are placeholders that you will overwrite with hello corresponding properties from hello exported view file.</span></span>

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

<span data-ttu-id="c4f3a-131">Lägg till följande variabler toohello variabler element av hello lösningsfilen hello och Ersätt hello värden toothose för lösningen.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-131">Add hello following variables toohello variables element of hello solution file and replace hello values toothose for your solution.</span></span>

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of hello view."
    "ViewName": "Provide a name for hello view here."


<span data-ttu-id="c4f3a-132">Observera att du kan kopiera hello hela vyn resurs från din exporterade visa filen, men du behöver toomake hello följande ändringar för den toowork i din lösning.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-132">Note that you could copy hello entire view resource from your exported view file, but you would need toomake hello following changes for it toowork in your solution.</span></span>  

* <span data-ttu-id="c4f3a-133">Hej **typen** för hello vyn resursbehov toobe har ändrats från **vyer** för**Microsoft.OperationalInsights/workspaces**.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-133">hello **type** for hello view resource needs toobe changed from **views** too**Microsoft.OperationalInsights/workspaces**.</span></span>
* <span data-ttu-id="c4f3a-134">Hej **namn** egenskapen för hello visa resursen måste toobe ändras tooinclude hello arbetsytans namn.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-134">hello **name** property for hello view resource needs toobe changed tooinclude hello workspace name.</span></span>
* <span data-ttu-id="c4f3a-135">hello beroende hello arbetsytan måste toobe tas bort eftersom hello arbetsytan resursen har inte definierats i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-135">hello dependency on hello workspace needs toobe removed since hello workspace resource isn't defined in hello solution.</span></span>
* <span data-ttu-id="c4f3a-136">**DisplayName** egenskapen behov toobe läggs toohello vyn.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-136">**DisplayName** property needs toobe added toohello view.</span></span>  <span data-ttu-id="c4f3a-137">Hej **Id**, **namn**, och **DisplayName** måste matcha alla.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-137">hello **Id**, **Name**, and **DisplayName** must all match.</span></span>
* <span data-ttu-id="c4f3a-138">Parameternamn måste ändras toomatch hello krävs uppsättning parametrar.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-138">Parameter names must be changed toomatch hello required set of parameters.</span></span>
* <span data-ttu-id="c4f3a-139">Variabler ska definieras i hello lösning och används i hello lämpliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-139">Variables should be defined in hello solution and used in hello appropriate properties.</span></span>

## <a name="add-hello-view-details"></a><span data-ttu-id="c4f3a-140">Lägg till hello Visa detaljer</span><span class="sxs-lookup"><span data-stu-id="c4f3a-140">Add hello view details</span></span>
<span data-ttu-id="c4f3a-141">hello visa resurs i hello exporteras visa filen innehåller två element i hello **egenskaper** element med namnet **instrumentpanelen** och **OverviewTile** som innehåller hello detaljerade konfigurationen av hello vyn.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-141">hello view resource in hello exported view file will contain two elements in hello **properties** element named **Dashboard** and **OverviewTile** which contain hello detailed configuration of hello view.</span></span>  <span data-ttu-id="c4f3a-142">Kopiera dessa två element och deras innehåll till hello **egenskaper** element av hello visa resurs i din lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-142">Copy these two elements and their contents into hello **properties** element of hello view resource in your solution file.</span></span>

## <a name="example"></a><span data-ttu-id="c4f3a-143">Exempel</span><span class="sxs-lookup"><span data-stu-id="c4f3a-143">Example</span></span>
<span data-ttu-id="c4f3a-144">Till exempel visar hello som följande exempel en enkel lösning-fil med en vy.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-144">For example, hello following sample shows a simple solution file with a view.</span></span>  <span data-ttu-id="c4f3a-145">Ellipserna (...) visas för hello **instrumentpanelen** och **OverviewTile** innehållet utrymme skäl.</span><span class="sxs-lookup"><span data-stu-id="c4f3a-145">Ellipses (...) are shown for hello **Dashboard** and **OverviewTile** contents for space reasons.</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="c4f3a-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4f3a-146">Next steps</span></span>
* <span data-ttu-id="c4f3a-147">Lär dig mer detaljerad information om hur du skapar [hanteringslösningar](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="c4f3a-147">Learn complete details of creating [management solutions](operations-management-suite-solutions-creating.md).</span></span>
* <span data-ttu-id="c4f3a-148">Inkludera [Automation-runbooks i din lösning](operations-management-suite-solutions-resources-automation.md).</span><span class="sxs-lookup"><span data-stu-id="c4f3a-148">Include [Automation runbooks in your management solution](operations-management-suite-solutions-resources-automation.md).</span></span>
