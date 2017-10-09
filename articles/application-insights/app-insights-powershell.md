---
title: aaaAutomate Azure Application Insights med PowerShell | Microsoft Docs
description: "Automatisera skapar resursen, aviseringen och tillgänglighet tester i PowerShell med en Azure Resource Manager-mall."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: bwren
ms.openlocfilehash: ebd336eafba58a690a0e8ffbd1c74f7e93dbb682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="create-application-insights-resources-using-powershell"></a><span data-ttu-id="52ea9-103">Skapa Application Insights-resurser med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="52ea9-103">Create Application Insights resources using PowerShell</span></span>
<span data-ttu-id="52ea9-104">Den här artikeln visar hur tooautomate hello skapande och uppdatering av [Programinsikter](app-insights-overview.md) resurser automatiskt med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="52ea9-104">This article shows you how tooautomate hello creation and update of [Application Insights](app-insights-overview.md) resources automatically by using Azure Resource Management.</span></span> <span data-ttu-id="52ea9-105">Du kan till exempel göra det som en del av en build-process.</span><span class="sxs-lookup"><span data-stu-id="52ea9-105">You might, for example, do so as part of a build process.</span></span> <span data-ttu-id="52ea9-106">Tillsammans med hello grundläggande Application Insights-resurs, kan du skapa [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md), Ställ in [aviseringar](app-insights-alerts.md), Ställ in hello [priser schemat](app-insights-pricing.md), och skapa andra Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="52ea9-106">Along with hello basic Application Insights resource, you can create [availability web tests](app-insights-monitor-web-app-availability.md), set up [alerts](app-insights-alerts.md), set hello [pricing scheme](app-insights-pricing.md), and create other Azure resources.</span></span>

<span data-ttu-id="52ea9-107">Hej viktiga toocreating resurserna är JSON-mallar för [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="52ea9-107">hello key toocreating these resources is JSON templates for [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span> <span data-ttu-id="52ea9-108">I kort sagt kan hello proceduren är: hämta hello JSON definitioner av befintliga resurser. parameterstyra vissa värden, till exempel namn. och kör sedan hello mallen när du vill toocreate en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="52ea9-108">In a nutshell, hello procedure is: download hello JSON definitions of existing resources; parameterize certain values such as names; and then run hello template whenever you want toocreate a new resource.</span></span> <span data-ttu-id="52ea9-109">Du kan paketera flera resurser tillsammans, toocreate dem i en Gå - till exempel, en app Övervakare med tillgänglighetstester, aviseringar och lagring för löpande export.</span><span class="sxs-lookup"><span data-stu-id="52ea9-109">You can package several resources together, toocreate them all in one go - for example, an app monitor with availability tests, alerts, and storage for continuous export.</span></span> <span data-ttu-id="52ea9-110">Det finns vissa detaljerad och nyansrik toosome av hello parameterizations som beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="52ea9-110">There are some subtleties toosome of hello parameterizations, which we'll explain here.</span></span>

## <a name="one-time-setup"></a><span data-ttu-id="52ea9-111">Enstaka installationen</span><span class="sxs-lookup"><span data-stu-id="52ea9-111">One-time setup</span></span>
<span data-ttu-id="52ea9-112">Om du inte har använt PowerShell med din Azure-prenumeration innan du:</span><span class="sxs-lookup"><span data-stu-id="52ea9-112">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="52ea9-113">Installera hello Azure Powershell-modulen på hello datorn där du vill att toorun hello skript:</span><span class="sxs-lookup"><span data-stu-id="52ea9-113">Install hello Azure Powershell module on hello machine where you want toorun hello scripts:</span></span>

1. <span data-ttu-id="52ea9-114">Installera [Microsoft Web Platform Installer (v5 eller högre)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="52ea9-114">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
2. <span data-ttu-id="52ea9-115">Använd den tooinstall Microsoft Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="52ea9-115">Use it tooinstall Microsoft Azure Powershell.</span></span>

## <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="52ea9-116">Skapa en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="52ea9-116">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="52ea9-117">Skapa en ny fil i JSON - vi anropa den `template1.json` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="52ea9-117">Create a new .json file - let's call it `template1.json` in this example.</span></span> <span data-ttu-id="52ea9-118">Kopiera innehållet till den:</span><span class="sxs-lookup"><span data-stu-id="52ea9-118">Copy this content into it:</span></span>

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter hello application name."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "HockeyAppBridge",
                    "other"
                ],
                "metadata": {
                    "description": "Enter hello application type."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "East US",
                "allowedValues": [
                    "South Central US",
                    "West Europe",
                    "East US",
                    "North Europe"
                ],
                "metadata": {
                    "description": "Enter hello application location."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "1 = Basic, 2 = Enterprise"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 24,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 too23). Values outside hello range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter hello % value of daily quota after which warning mail toobe sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```



## <a name="create-application-insights-resources"></a><span data-ttu-id="52ea9-119">Skapa Application Insights-resurser</span><span class="sxs-lookup"><span data-stu-id="52ea9-119">Create Application Insights resources</span></span>
1. <span data-ttu-id="52ea9-120">Logga in tooAzure i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="52ea9-120">In PowerShell, sign in tooAzure:</span></span>
   
    `Login-AzureRmAccount`
2. <span data-ttu-id="52ea9-121">Kör ett kommando som detta:</span><span class="sxs-lookup"><span data-stu-id="52ea9-121">Run a command like this:</span></span>
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * <span data-ttu-id="52ea9-122">`-ResourceGroupName`är hello grupp där du vill att toocreate hello nya resurser.</span><span class="sxs-lookup"><span data-stu-id="52ea9-122">`-ResourceGroupName` is hello group where you want toocreate hello new resources.</span></span>
   * <span data-ttu-id="52ea9-123">`-TemplateFile`måste inträffa innan hello anpassade parametrar.</span><span class="sxs-lookup"><span data-stu-id="52ea9-123">`-TemplateFile` must occur before hello custom parameters.</span></span>
   * <span data-ttu-id="52ea9-124">`-appName`hello namnet på hello resurs toocreate.</span><span class="sxs-lookup"><span data-stu-id="52ea9-124">`-appName` hello name of hello resource toocreate.</span></span>

<span data-ttu-id="52ea9-125">Du kan lägga till andra parametrar - hittar du beskrivningar under hello parametrar i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="52ea9-125">You can add other parameters - you'll find their descriptions in hello parameters section of hello template.</span></span>

## <a name="tooget-hello-instrumentation-key"></a><span data-ttu-id="52ea9-126">tooget hello instrumentation nyckel</span><span class="sxs-lookup"><span data-stu-id="52ea9-126">tooget hello instrumentation key</span></span>
<span data-ttu-id="52ea9-127">När du skapar en resurs för programmet, vill du hello instrumentation nyckel:</span><span class="sxs-lookup"><span data-stu-id="52ea9-127">After creating an application resource, you'll want hello instrumentation key:</span></span> 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-hello-price-plan"></a><span data-ttu-id="52ea9-128">Ange hello prisplan</span><span class="sxs-lookup"><span data-stu-id="52ea9-128">Set hello price plan</span></span>

<span data-ttu-id="52ea9-129">Du kan ange hello [prisplan](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="52ea9-129">You can set hello [price plan](app-insights-pricing.md).</span></span>

<span data-ttu-id="52ea9-130">toocreate en appresursen med hello Enterprise prisplan med hello mallen ovan:</span><span class="sxs-lookup"><span data-stu-id="52ea9-130">toocreate an app resource with hello Enterprise price plan, using hello template above:</span></span>

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|<span data-ttu-id="52ea9-131">priceCode</span><span class="sxs-lookup"><span data-stu-id="52ea9-131">priceCode</span></span>|<span data-ttu-id="52ea9-132">Planera</span><span class="sxs-lookup"><span data-stu-id="52ea9-132">plan</span></span>|
|---|---|
|<span data-ttu-id="52ea9-133">1</span><span class="sxs-lookup"><span data-stu-id="52ea9-133">1</span></span>|<span data-ttu-id="52ea9-134">Basic</span><span class="sxs-lookup"><span data-stu-id="52ea9-134">Basic</span></span>|
|<span data-ttu-id="52ea9-135">2</span><span class="sxs-lookup"><span data-stu-id="52ea9-135">2</span></span>|<span data-ttu-id="52ea9-136">Enterprise</span><span class="sxs-lookup"><span data-stu-id="52ea9-136">Enterprise</span></span>|

* <span data-ttu-id="52ea9-137">Om du bara vill toouse hello baspris standardplanen kan du utelämna hello CurrentBillingFeatures resurs från hello mallen.</span><span class="sxs-lookup"><span data-stu-id="52ea9-137">If you only want toouse hello default Basic price plan, you can omit hello CurrentBillingFeatures resource from hello template.</span></span>
* <span data-ttu-id="52ea9-138">Om du vill toochange hello pris när hello komponenten resursen har skapats, kan du använda en mall som utesluter hello ”microsoft.insights/components” resurs.</span><span class="sxs-lookup"><span data-stu-id="52ea9-138">If you want toochange hello price plan after hello component resource has been created, you can use a template that omits hello "microsoft.insights/components" resource.</span></span> <span data-ttu-id="52ea9-139">Dessutom utelämna hello `dependsOn` nod från hello fakturering resurs.</span><span class="sxs-lookup"><span data-stu-id="52ea9-139">Also, omit hello `dependsOn` node from hello billing resource.</span></span> 

<span data-ttu-id="52ea9-140">tooverify Hej uppdaterade prisplan, titta på bladet hello ”funktioner + pris” i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="52ea9-140">tooverify hello updated price plan, look at hello "Features+pricing" blade in hello browser.</span></span> <span data-ttu-id="52ea9-141">**Uppdatera hello webbläsaren visa** toomake som du kan se hello senaste tillstånd.</span><span class="sxs-lookup"><span data-stu-id="52ea9-141">**Refresh hello browser view** toomake sure you see hello latest state.</span></span>



## <a name="add-a-metric-alert"></a><span data-ttu-id="52ea9-142">Lägg till ett mått varning</span><span class="sxs-lookup"><span data-stu-id="52ea9-142">Add a metric alert</span></span>

<span data-ttu-id="52ea9-143">tooset upp ett mått varning vid hello samma tid som din app resurs merge kod så här i hello mallfilen:</span><span class="sxs-lookup"><span data-stu-id="52ea9-143">tooset up a metric alert at hello same time as your app resource, merge code like this into hello template file:</span></span>

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after hello app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

<span data-ttu-id="52ea9-144">När du anropar hello mallen du om du vill lägga till den här parametern:</span><span class="sxs-lookup"><span data-stu-id="52ea9-144">When you invoke hello template, you can optionally add this parameter:</span></span>

    `-responseTime 2`

<span data-ttu-id="52ea9-145">Du kan självklart parameterstyra andra fält.</span><span class="sxs-lookup"><span data-stu-id="52ea9-145">You can of course parameterize other fields.</span></span> 

<span data-ttu-id="52ea9-146">toofind hello typnamn och konfigurationsinformation för andra Varningsregler manuellt skapa en regel och kontrollera sedan i [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="52ea9-146">toofind out hello type names and configuration details of other alert rules, create a rule manually and then inspect it in [Azure Resource Manager](https://resources.azure.com/).</span></span> 


## <a name="add-an-availability-test"></a><span data-ttu-id="52ea9-147">Lägg till ett tillgänglighetstest</span><span class="sxs-lookup"><span data-stu-id="52ea9-147">Add an availability test</span></span>

<span data-ttu-id="52ea9-148">Det här exemplet är för ping-testet (tootest en enstaka sida).</span><span class="sxs-lookup"><span data-stu-id="52ea9-148">This example is for a ping test (tootest a single page).</span></span>  

<span data-ttu-id="52ea9-149">**Det finns två delar** i ett tillgänglighetstest: hello test sig själv och hello varning som meddelar dig om fel.</span><span class="sxs-lookup"><span data-stu-id="52ea9-149">**There are two parts** in an availability test: hello test itself, and hello alert that notifies you of failures.</span></span>

<span data-ttu-id="52ea9-150">Sammanfoga hello följande kod i hello mallfilen som skapar hello app.</span><span class="sxs-lookup"><span data-stu-id="52ea9-150">Merge hello following code into hello template file that creates hello app.</span></span>

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures hello test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after hello app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies hello existence of hello specified text in hello response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, hello alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

<span data-ttu-id="52ea9-151">toodiscover hello koder för andra test platser eller tooautomate hello skapa mer komplexa webbtester, skapa ett exempel manuellt och parameterstyra hello kod från [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="52ea9-151">toodiscover hello codes for other test locations, or tooautomate hello creation of more complex web tests, create an example manually and then parameterize hello code from [Azure Resource Manager](https://resources.azure.com/).</span></span>

## <a name="add-more-resources"></a><span data-ttu-id="52ea9-152">Lägg till fler resurser</span><span class="sxs-lookup"><span data-stu-id="52ea9-152">Add more resources</span></span>

<span data-ttu-id="52ea9-153">tooautomate hello skapandet av någon annan resurs av något slag, skapa ett exempel manuellt och sedan kopiera och parameterstyra dess kod från [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="52ea9-153">tooautomate hello creation of any other resource of any kind, create an example manually, and then copy and parameterize its code from [Azure Resource Manager](https://resources.azure.com/).</span></span> 

1. <span data-ttu-id="52ea9-154">Öppna [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="52ea9-154">Open [Azure Resource Manager](https://resources.azure.com/).</span></span> <span data-ttu-id="52ea9-155">Bläddra nedåt i `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour programresursen.</span><span class="sxs-lookup"><span data-stu-id="52ea9-155">Navigate down through `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour application resource.</span></span> 
   
    ![Navigering i Azure Resursläsaren](./media/app-insights-powershell/01.png)
   
    <span data-ttu-id="52ea9-157">*Komponenter* är hello grundläggande Application Insights-resurser för att visa program.</span><span class="sxs-lookup"><span data-stu-id="52ea9-157">*Components* are hello basic Application Insights resources for displaying applications.</span></span> <span data-ttu-id="52ea9-158">Det finns separata resurser för hello associerade Varningsregler och tillgänglighetstester för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="52ea9-158">There are separate resources for hello associated alert rules and availability web tests.</span></span>
2. <span data-ttu-id="52ea9-159">Kopiera hello JSON för hello-komponenten i hello lämplig plats i `template1.json`.</span><span class="sxs-lookup"><span data-stu-id="52ea9-159">Copy hello JSON of hello component into hello appropriate place in `template1.json`.</span></span>
3. <span data-ttu-id="52ea9-160">Ta bort dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="52ea9-160">Delete these properties:</span></span>
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. <span data-ttu-id="52ea9-161">Öppna hello webtests och alertrules avsnitt och kopiera hello JSON för enskilda objekt i mallen.</span><span class="sxs-lookup"><span data-stu-id="52ea9-161">Open hello webtests and alertrules sections and copy hello JSON for individual items into your template.</span></span> <span data-ttu-id="52ea9-162">(Inte kopiera från hello webtests eller alertrules noder: Gå till hello posterna under dem.)</span><span class="sxs-lookup"><span data-stu-id="52ea9-162">(Don't copy from hello webtests or alertrules nodes: go into hello items under them.)</span></span>
   
    <span data-ttu-id="52ea9-163">Varje webbtest har en associerad aviseringsregel så att du har toocopy båda av dessa.</span><span class="sxs-lookup"><span data-stu-id="52ea9-163">Each web test has an associated alert rule, so you have toocopy both of them.</span></span>
   
    <span data-ttu-id="52ea9-164">Du kan även inkludera aviseringar på mått.</span><span class="sxs-lookup"><span data-stu-id="52ea9-164">You can also include alerts on metrics.</span></span> <span data-ttu-id="52ea9-165">[Tjänstmåttets namn](app-insights-powershell-alerts.md#metric-names).</span><span class="sxs-lookup"><span data-stu-id="52ea9-165">[Metric names](app-insights-powershell-alerts.md#metric-names).</span></span>
5. <span data-ttu-id="52ea9-166">Infoga raden i varje resurs:</span><span class="sxs-lookup"><span data-stu-id="52ea9-166">Insert this line in each resource:</span></span>
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-hello-template"></a><span data-ttu-id="52ea9-167">Parameterstyra hello mall</span><span class="sxs-lookup"><span data-stu-id="52ea9-167">Parameterize hello template</span></span>
<span data-ttu-id="52ea9-168">Nu har du tooreplace hello specifika namn med parametrar.</span><span class="sxs-lookup"><span data-stu-id="52ea9-168">Now you have tooreplace hello specific names with parameters.</span></span> <span data-ttu-id="52ea9-169">för[parameterstyra en mall](../azure-resource-manager/resource-group-authoring-templates.md), du kan skriva uttryck med en [uppsättning Hjälpfunktioner](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="52ea9-169">too[parameterize a template](../azure-resource-manager/resource-group-authoring-templates.md), you write expressions using a [set of helper functions](../azure-resource-manager/resource-group-template-functions.md).</span></span> 

<span data-ttu-id="52ea9-170">Du kan inte parameterstyra bara en del av en sträng, så Använd `concat()` toobuild strängar.</span><span class="sxs-lookup"><span data-stu-id="52ea9-170">You can't parameterize just part of a string, so use `concat()` toobuild strings.</span></span>

<span data-ttu-id="52ea9-171">Här följer exempel på hello ersättningar ska du toomake.</span><span class="sxs-lookup"><span data-stu-id="52ea9-171">Here are examples of hello substitutions you'll want toomake.</span></span> <span data-ttu-id="52ea9-172">Det finns flera förekomster av varje ersättning.</span><span class="sxs-lookup"><span data-stu-id="52ea9-172">There are several occurrences of each substitution.</span></span> <span data-ttu-id="52ea9-173">Du kan behöva andra i mallen.</span><span class="sxs-lookup"><span data-stu-id="52ea9-173">You might need others in your template.</span></span> <span data-ttu-id="52ea9-174">Dessa exempel används hello parametrar och variabler som vi har definierat hello överst i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="52ea9-174">These examples use hello parameters and variables we defined at hello top of hello template.</span></span>

| <span data-ttu-id="52ea9-175">hitta</span><span class="sxs-lookup"><span data-stu-id="52ea9-175">find</span></span> | <span data-ttu-id="52ea9-176">Ersätt med</span><span class="sxs-lookup"><span data-stu-id="52ea9-176">replace with</span></span> |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| <span data-ttu-id="52ea9-177">`"myappname"`(gemen)</span><span class="sxs-lookup"><span data-stu-id="52ea9-177">`"myappname"` (lower case)</span></span> |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/><span data-ttu-id="52ea9-178">Ta bort Guid och -Id.</span><span class="sxs-lookup"><span data-stu-id="52ea9-178">Delete Guid and Id.</span></span> |

### <a name="set-dependencies-between-hello-resources"></a><span data-ttu-id="52ea9-179">Ange beroenden mellan hello resurser</span><span class="sxs-lookup"><span data-stu-id="52ea9-179">Set dependencies between hello resources</span></span>
<span data-ttu-id="52ea9-180">Azure bör ställa in hello resurser i strikt ordning.</span><span class="sxs-lookup"><span data-stu-id="52ea9-180">Azure should set up hello resources in strict order.</span></span> <span data-ttu-id="52ea9-181">toomake till filen som installationsprogrammet slutförs innan nästa börjar hello Lägg till beroende rader:</span><span class="sxs-lookup"><span data-stu-id="52ea9-181">toomake sure one setup completes before hello next begins, add dependency lines:</span></span>

* <span data-ttu-id="52ea9-182">Testa resurs i hello tillgänglighet:</span><span class="sxs-lookup"><span data-stu-id="52ea9-182">In hello availability test resource:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* <span data-ttu-id="52ea9-183">I hello avisering resurs för ett tillgänglighetstest:</span><span class="sxs-lookup"><span data-stu-id="52ea9-183">In hello alert resource for an availability test:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a><span data-ttu-id="52ea9-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52ea9-184">Next steps</span></span>
<span data-ttu-id="52ea9-185">Andra automation-artiklar:</span><span class="sxs-lookup"><span data-stu-id="52ea9-185">Other automation articles:</span></span>

* <span data-ttu-id="52ea9-186">[Skapa en resurs för Application Insights](app-insights-powershell-script-create-resource.md) -snabb metod utan att använda en mall.</span><span class="sxs-lookup"><span data-stu-id="52ea9-186">[Create an Application Insights resource](app-insights-powershell-script-create-resource.md) - quick method without using a template.</span></span>
* [<span data-ttu-id="52ea9-187">Konfigurera aviseringar</span><span class="sxs-lookup"><span data-stu-id="52ea9-187">Set up Alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="52ea9-188">Skapa webbtester</span><span class="sxs-lookup"><span data-stu-id="52ea9-188">Create web tests</span></span>](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [<span data-ttu-id="52ea9-189">Skicka Azure Diagnostics tooApplication insikter</span><span class="sxs-lookup"><span data-stu-id="52ea9-189">Send Azure Diagnostics tooApplication Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="52ea9-190">Distribuera tooAzure från GitHub</span><span class="sxs-lookup"><span data-stu-id="52ea9-190">Deploy tooAzure from GitHub</span></span>](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [<span data-ttu-id="52ea9-191">Skapa versionen anteckningar</span><span class="sxs-lookup"><span data-stu-id="52ea9-191">Create release annotations</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

