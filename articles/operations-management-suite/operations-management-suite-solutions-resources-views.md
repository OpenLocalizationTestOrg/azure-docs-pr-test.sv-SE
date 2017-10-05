---
title: "Vyer i Operations Management Suite (OMS) hanteringslösningar | Microsoft Docs"
description: "En eller flera vyer om du vill visualisera data inkluderar vanligtvis hanteringslösningar i Operations Management Suite (OMS).  Den här artikeln beskriver hur du exporterar en vy som skapats av Vydesigner och inkludera den i en lösning. "
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
ms.openlocfilehash: 533b5564a805e0b41f2b1a4ad92e12b133220952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a><span data-ttu-id="b7db8-104">Vyer i Operations Management Suite (OMS) hanteringslösningar (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="b7db8-104">Views in Operations Management Suite (OMS) management solutions (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="b7db8-105">Den här är dokumentationen preliminär för att skapa lösningar för hantering i OMS som för närvarande finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="b7db8-106">Ett schema som beskrivs nedan kan ändras.</span><span class="sxs-lookup"><span data-stu-id="b7db8-106">Any schema described below is subject to change.</span></span>    
>
>

<span data-ttu-id="b7db8-107">[Lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions.md) inkluderar vanligtvis en eller flera vyer om du vill visualisera data.</span><span class="sxs-lookup"><span data-stu-id="b7db8-107">[Management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions.md) will typically include one or more views to visualize data.</span></span>  <span data-ttu-id="b7db8-108">Den här artikeln beskriver hur du exporterar en vy som skapades av den [Vydesigner](../log-analytics/log-analytics-view-designer.md) och inkludera den i en lösning.</span><span class="sxs-lookup"><span data-stu-id="b7db8-108">This article describes how to export a view created by the [View Designer](../log-analytics/log-analytics-view-designer.md) and include it in a management solution.</span></span>  

> [!NOTE]
> <span data-ttu-id="b7db8-109">Exemplen i den här artikeln använder parametrar och variabler som är obligatoriska eller vanligt att hanteringslösningar och beskrivs i [och skapa lösningar för hantering i Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="b7db8-109">The samples in this article use parameters and variables that are either required or common to management solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="b7db8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b7db8-110">Prerequisites</span></span>
<span data-ttu-id="b7db8-111">Den här artikeln förutsätter att du redan är bekant med [skapar en lösning för](operations-management-suite-solutions-creating.md) och strukturen för en lösningsfil.</span><span class="sxs-lookup"><span data-stu-id="b7db8-111">This article assumes that you're already familiar with how to [create a management solution](operations-management-suite-solutions-creating.md) and the structure of a solution file.</span></span>

## <a name="overview"></a><span data-ttu-id="b7db8-112">Översikt</span><span class="sxs-lookup"><span data-stu-id="b7db8-112">Overview</span></span>
<span data-ttu-id="b7db8-113">Om du vill inkludera en vy i en lösning som du skapar en **resurs** för den i den [lösningsfilen](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="b7db8-113">To include a view in a management solution, you create a **resource** for it in the [solution file](operations-management-suite-solutions-creating.md).</span></span>  <span data-ttu-id="b7db8-114">JSON som beskriver den visa detaljerad konfiguration är komplex men och inte något att en typisk lösning författare skulle kunna skapa manuellt.</span><span class="sxs-lookup"><span data-stu-id="b7db8-114">The JSON that describes the view's detailed configuration is typically complex though and not something that a typical solution author would be able to create manually.</span></span>  <span data-ttu-id="b7db8-115">Den vanligaste metoden är att skapa en vy med hjälp av den [Vydesigner](../log-analytics/log-analytics-view-designer.md), exportera den och sedan lägga till den detaljerade konfigurationen i lösningen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-115">The most common method is to create the view using the [View Designer](../log-analytics/log-analytics-view-designer.md), export it, and then add its detailed configuration to the solution.</span></span>

<span data-ttu-id="b7db8-116">De grundläggande stegen för att lägga till en vy i en lösning är som följer.</span><span class="sxs-lookup"><span data-stu-id="b7db8-116">The basic steps to add a view to a solution are as follows.</span></span>  <span data-ttu-id="b7db8-117">Varje steg beskrivs detaljerat i nedanstående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b7db8-117">Each step is described in further detail in the sections below.</span></span>

1. <span data-ttu-id="b7db8-118">Exportera vyn till en fil.</span><span class="sxs-lookup"><span data-stu-id="b7db8-118">Export the view to a file.</span></span>
2. <span data-ttu-id="b7db8-119">Skapa vy resursen i lösningen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-119">Create the view resource in the solution.</span></span>
3. <span data-ttu-id="b7db8-120">Lägga till vyn Detaljer.</span><span class="sxs-lookup"><span data-stu-id="b7db8-120">Add the view details.</span></span>

## <a name="export-the-view-to-a-file"></a><span data-ttu-id="b7db8-121">Exportera vyn till en fil</span><span class="sxs-lookup"><span data-stu-id="b7db8-121">Export the view to a file</span></span>
<span data-ttu-id="b7db8-122">Följ anvisningarna på [Log Analytics Vydesigner](../log-analytics/log-analytics-view-designer.md) att exportera en vy till en fil.</span><span class="sxs-lookup"><span data-stu-id="b7db8-122">Follow the instructions at [Log Analytics View Designer](../log-analytics/log-analytics-view-designer.md) to export a view to a file.</span></span>  <span data-ttu-id="b7db8-123">Den exporterade filen kommer att i JSON-format med samma [element som lösningsfilen](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="b7db8-123">The exported file will be in JSON format with the same [elements as the solution file](operations-management-suite-solutions-solution-file.md).</span></span>  

<span data-ttu-id="b7db8-124">Den **resurser** element i filen vyn har en resurs med en typ av **Microsoft.OperationalInsights/workspaces** som representerar OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="b7db8-124">The **resources** element of the view file will have a resource with a type of **Microsoft.OperationalInsights/workspaces** that represents the OMS workspace.</span></span>  <span data-ttu-id="b7db8-125">Det här elementet har ett underelement av typen **vyer** som representerar vyn och innehåller detaljerade konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-125">This element will have a subelement with a type of **views** that represents the view and contains its detailed configuration.</span></span>  <span data-ttu-id="b7db8-126">Du kan kopiera information om det här elementet och kopierar den till din lösning.</span><span class="sxs-lookup"><span data-stu-id="b7db8-126">You will copy the details of this element and then copy it into your solution.</span></span>

## <a name="create-the-view-resource-in-the-solution"></a><span data-ttu-id="b7db8-127">Skapa vy resursen i lösningen</span><span class="sxs-lookup"><span data-stu-id="b7db8-127">Create the view resource in the solution</span></span>
<span data-ttu-id="b7db8-128">Lägg till följande vy resursen till den **resurser** element i din lösningsfil.</span><span class="sxs-lookup"><span data-stu-id="b7db8-128">Add the following view resource to the **resources** element of your solution file.</span></span>  <span data-ttu-id="b7db8-129">Den använder variabler som beskrivs nedan som du måste också lägga till.</span><span class="sxs-lookup"><span data-stu-id="b7db8-129">This uses variables that are described below that you must also add.</span></span>  <span data-ttu-id="b7db8-130">Observera att den **instrumentpanelen** och **OverviewTile** egenskaper är platshållare som kommer att skrivas över med motsvarande egenskaper från den exportera visa filen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-130">Note that the **Dashboard** and **OverviewTile** properties are placeholders that you will overwrite with the corresponding properties from the exported view file.</span></span>

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

<span data-ttu-id="b7db8-131">Lägg till följande variabler i elementet variabler i lösningsfilen och Ersätt värdena som de för din lösning.</span><span class="sxs-lookup"><span data-stu-id="b7db8-131">Add the following variables to the variables element of the solution file and replace the values to those for your solution.</span></span>

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


<span data-ttu-id="b7db8-132">Observera att du kan kopiera hela vyn resursen från din exporterade visa filen, men måste du göra följande ändringar att fungera i din lösning.</span><span class="sxs-lookup"><span data-stu-id="b7db8-132">Note that you could copy the entire view resource from your exported view file, but you would need to make the following changes for it to work in your solution.</span></span>  

* <span data-ttu-id="b7db8-133">Den **typen** för vyn resurs ska ändras från **vyer** till **Microsoft.OperationalInsights/workspaces**.</span><span class="sxs-lookup"><span data-stu-id="b7db8-133">The **type** for the view resource needs to be changed from **views** to **Microsoft.OperationalInsights/workspaces**.</span></span>
* <span data-ttu-id="b7db8-134">Den **namn** -egenskapen för resursen visa behöver ändras för att inkludera namnet på arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="b7db8-134">The **name** property for the view resource needs to be changed to include the workspace name.</span></span>
* <span data-ttu-id="b7db8-135">Beroende på arbetsytan måste tas bort eftersom den arbetsyta resursen har inte definierats i lösningen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-135">The dependency on the workspace needs to be removed since the workspace resource isn't defined in the solution.</span></span>
* <span data-ttu-id="b7db8-136">**DisplayName** egenskapen måste läggas till vyn.</span><span class="sxs-lookup"><span data-stu-id="b7db8-136">**DisplayName** property needs to be added to the view.</span></span>  <span data-ttu-id="b7db8-137">Den **Id**, **namn**, och **DisplayName** måste matcha alla.</span><span class="sxs-lookup"><span data-stu-id="b7db8-137">The **Id**, **Name**, and **DisplayName** must all match.</span></span>
* <span data-ttu-id="b7db8-138">Parameternamn måste ändras för att matcha uppsättningen parametrar som krävs.</span><span class="sxs-lookup"><span data-stu-id="b7db8-138">Parameter names must be changed to match the required set of parameters.</span></span>
* <span data-ttu-id="b7db8-139">Variabler ska definieras i lösningen och används i lämpliga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b7db8-139">Variables should be defined in the solution and used in the appropriate properties.</span></span>

## <a name="add-the-view-details"></a><span data-ttu-id="b7db8-140">Lägg till Visa detaljer</span><span class="sxs-lookup"><span data-stu-id="b7db8-140">Add the view details</span></span>
<span data-ttu-id="b7db8-141">Visa resursen i filen exporterade vyn innehåller två element i den **egenskaper** element med namnet **instrumentpanelen** och **OverviewTile** som innehåller den detaljerade konfiguration av vyn.</span><span class="sxs-lookup"><span data-stu-id="b7db8-141">The view resource in the exported view file will contain two elements in the **properties** element named **Dashboard** and **OverviewTile** which contain the detailed configuration of the view.</span></span>  <span data-ttu-id="b7db8-142">Kopiera dessa två element och innehållet i den **egenskaper** element i vyn resurs i din lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="b7db8-142">Copy these two elements and their contents into the **properties** element of the view resource in your solution file.</span></span>

## <a name="example"></a><span data-ttu-id="b7db8-143">Exempel</span><span class="sxs-lookup"><span data-stu-id="b7db8-143">Example</span></span>
<span data-ttu-id="b7db8-144">I följande exempel visar exempelvis en enkel lösning-fil med en vy.</span><span class="sxs-lookup"><span data-stu-id="b7db8-144">For example, the following sample shows a simple solution file with a view.</span></span>  <span data-ttu-id="b7db8-145">Ellipserna (...) visas för den **instrumentpanelen** och **OverviewTile** innehållet utrymme skäl.</span><span class="sxs-lookup"><span data-stu-id="b7db8-145">Ellipses (...) are shown for the **Dashboard** and **OverviewTile** contents for space reasons.</span></span>

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




## <a name="next-steps"></a><span data-ttu-id="b7db8-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7db8-146">Next steps</span></span>
* <span data-ttu-id="b7db8-147">Lär dig mer detaljerad information om hur du skapar [hanteringslösningar](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="b7db8-147">Learn complete details of creating [management solutions](operations-management-suite-solutions-creating.md).</span></span>
* <span data-ttu-id="b7db8-148">Inkludera [Automation-runbooks i din lösning](operations-management-suite-solutions-resources-automation.md).</span><span class="sxs-lookup"><span data-stu-id="b7db8-148">Include [Automation runbooks in your management solution](operations-management-suite-solutions-resources-automation.md).</span></span>
