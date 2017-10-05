---
title: Automatisera Azure Application Insights med PowerShell | Microsoft Docs
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
ms.openlocfilehash: 88dbb9515300f847789bc889911cdeff5f5bdb53
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
#  <a name="create-application-insights-resources-using-powershell"></a><span data-ttu-id="c3d45-103">Skapa Application Insights-resurser med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3d45-103">Create Application Insights resources using PowerShell</span></span>
<span data-ttu-id="c3d45-104">Den här artikeln visar hur du automatisera skapandet och uppdatering av [Programinsikter](app-insights-overview.md) resurser automatiskt med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c3d45-104">This article shows you how to automate the creation and update of [Application Insights](app-insights-overview.md) resources automatically by using Azure Resource Management.</span></span> <span data-ttu-id="c3d45-105">Du kan till exempel göra det som en del av en build-process.</span><span class="sxs-lookup"><span data-stu-id="c3d45-105">You might, for example, do so as part of a build process.</span></span> <span data-ttu-id="c3d45-106">Tillsammans med grundläggande Application Insights-resursen kan du skapa [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md), Ställ in [aviseringar](app-insights-alerts.md), ange den [priser schemat](app-insights-pricing.md), och skapa andra Azure-resurser .</span><span class="sxs-lookup"><span data-stu-id="c3d45-106">Along with the basic Application Insights resource, you can create [availability web tests](app-insights-monitor-web-app-availability.md), set up [alerts](app-insights-alerts.md), set the [pricing scheme](app-insights-pricing.md), and create other Azure resources.</span></span>

<span data-ttu-id="c3d45-107">Nyckeln till att skapa dessa resurser är JSON-mallar för [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c3d45-107">The key to creating these resources is JSON templates for [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span> <span data-ttu-id="c3d45-108">I kort sagt kan proceduren är: hämta JSON-definitioner av befintliga resurser. parameterstyra vissa värden, till exempel namn. och kör sedan mallen när du vill skapa en ny resurs.</span><span class="sxs-lookup"><span data-stu-id="c3d45-108">In a nutshell, the procedure is: download the JSON definitions of existing resources; parameterize certain values such as names; and then run the template whenever you want to create a new resource.</span></span> <span data-ttu-id="c3d45-109">Du kan även paketera flera resurser tillsammans, för att skapa dem i ett går alla - till exempel en app Övervakare med tillgänglighetstester, aviseringar och lagring för löpande export.</span><span class="sxs-lookup"><span data-stu-id="c3d45-109">You can package several resources together, to create them all in one go - for example, an app monitor with availability tests, alerts, and storage for continuous export.</span></span> <span data-ttu-id="c3d45-110">Det finns vissa detaljerad och nyansrik till vissa av parameterizations som beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="c3d45-110">There are some subtleties to some of the parameterizations, which we'll explain here.</span></span>

## <a name="one-time-setup"></a><span data-ttu-id="c3d45-111">Enstaka installationen</span><span class="sxs-lookup"><span data-stu-id="c3d45-111">One-time setup</span></span>
<span data-ttu-id="c3d45-112">Om du inte har använt PowerShell med din Azure-prenumeration innan du:</span><span class="sxs-lookup"><span data-stu-id="c3d45-112">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="c3d45-113">Installera Azure Powershell-modulen på datorn där du vill köra skript:</span><span class="sxs-lookup"><span data-stu-id="c3d45-113">Install the Azure Powershell module on the machine where you want to run the scripts:</span></span>

1. <span data-ttu-id="c3d45-114">Installera [Microsoft Web Platform Installer (v5 eller högre)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3d45-114">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
2. <span data-ttu-id="c3d45-115">Använd den för att installera Microsoft Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="c3d45-115">Use it to install Microsoft Azure Powershell.</span></span>

## <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="c3d45-116">Skapa en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c3d45-116">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="c3d45-117">Skapa en ny fil i JSON - vi anropa den `template1.json` i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="c3d45-117">Create a new .json file - let's call it `template1.json` in this example.</span></span> <span data-ttu-id="c3d45-118">Kopiera innehållet till den:</span><span class="sxs-lookup"><span data-stu-id="c3d45-118">Copy this content into it:</span></span>

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter the application name."
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
                    "description": "Enter the application type."
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
                    "description": "Enter the application location."
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
                    "description": "Enter daily quota reset hour in UTC (0 to 23). Values outside the range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter the % value of daily quota after which warning mail to be sent. "
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



## <a name="create-application-insights-resources"></a><span data-ttu-id="c3d45-119">Skapa Application Insights-resurser</span><span class="sxs-lookup"><span data-stu-id="c3d45-119">Create Application Insights resources</span></span>
1. <span data-ttu-id="c3d45-120">Logga in på Azure i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c3d45-120">In PowerShell, sign in to Azure:</span></span>
   
    `Login-AzureRmAccount`
2. <span data-ttu-id="c3d45-121">Kör ett kommando som detta:</span><span class="sxs-lookup"><span data-stu-id="c3d45-121">Run a command like this:</span></span>
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * <span data-ttu-id="c3d45-122">`-ResourceGroupName`är gruppen som du vill skapa nya resurser.</span><span class="sxs-lookup"><span data-stu-id="c3d45-122">`-ResourceGroupName` is the group where you want to create the new resources.</span></span>
   * <span data-ttu-id="c3d45-123">`-TemplateFile`måste inträffa innan anpassade parametrar.</span><span class="sxs-lookup"><span data-stu-id="c3d45-123">`-TemplateFile` must occur before the custom parameters.</span></span>
   * <span data-ttu-id="c3d45-124">`-appName`Namnet på resursen som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="c3d45-124">`-appName` The name of the resource to create.</span></span>

<span data-ttu-id="c3d45-125">Du kan lägga till andra parametrar - hittar du beskrivningar i avsnittet parametrar i mallen.</span><span class="sxs-lookup"><span data-stu-id="c3d45-125">You can add other parameters - you'll find their descriptions in the parameters section of the template.</span></span>

## <a name="to-get-the-instrumentation-key"></a><span data-ttu-id="c3d45-126">Att hämta nyckeln instrumentation</span><span class="sxs-lookup"><span data-stu-id="c3d45-126">To get the instrumentation key</span></span>
<span data-ttu-id="c3d45-127">När du skapar en resurs för programmet, vill du instrumentation nyckeln:</span><span class="sxs-lookup"><span data-stu-id="c3d45-127">After creating an application resource, you'll want the instrumentation key:</span></span> 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a><span data-ttu-id="c3d45-128">Ange pris planen</span><span class="sxs-lookup"><span data-stu-id="c3d45-128">Set the price plan</span></span>

<span data-ttu-id="c3d45-129">Du kan ange den [prisplan](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="c3d45-129">You can set the [price plan](app-insights-pricing.md).</span></span>

<span data-ttu-id="c3d45-130">Skapa en resurs i appen med pris företagsplan, med den här mallen ovan:</span><span class="sxs-lookup"><span data-stu-id="c3d45-130">To create an app resource with the Enterprise price plan, using the template above:</span></span>

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|<span data-ttu-id="c3d45-131">priceCode</span><span class="sxs-lookup"><span data-stu-id="c3d45-131">priceCode</span></span>|<span data-ttu-id="c3d45-132">Planera</span><span class="sxs-lookup"><span data-stu-id="c3d45-132">plan</span></span>|
|---|---|
|<span data-ttu-id="c3d45-133">1</span><span class="sxs-lookup"><span data-stu-id="c3d45-133">1</span></span>|<span data-ttu-id="c3d45-134">Basic</span><span class="sxs-lookup"><span data-stu-id="c3d45-134">Basic</span></span>|
|<span data-ttu-id="c3d45-135">2</span><span class="sxs-lookup"><span data-stu-id="c3d45-135">2</span></span>|<span data-ttu-id="c3d45-136">Enterprise</span><span class="sxs-lookup"><span data-stu-id="c3d45-136">Enterprise</span></span>|

* <span data-ttu-id="c3d45-137">Om du endast vill använda baspris standardplanen kan du utelämna CurrentBillingFeatures resursen från mallen.</span><span class="sxs-lookup"><span data-stu-id="c3d45-137">If you only want to use the default Basic price plan, you can omit the CurrentBillingFeatures resource from the template.</span></span>
* <span data-ttu-id="c3d45-138">Om du vill ändra pris planen när komponenten resursen har skapats kan du använda en mall som utesluter resursen ”microsoft.insights/components”.</span><span class="sxs-lookup"><span data-stu-id="c3d45-138">If you want to change the price plan after the component resource has been created, you can use a template that omits the "microsoft.insights/components" resource.</span></span> <span data-ttu-id="c3d45-139">Dessutom utelämna den `dependsOn` noden från resursen för fakturering.</span><span class="sxs-lookup"><span data-stu-id="c3d45-139">Also, omit the `dependsOn` node from the billing resource.</span></span> 

<span data-ttu-id="c3d45-140">Kontrollera uppdaterade priset planen genom att titta på den ”funktioner + pris” bladet i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c3d45-140">To verify the updated price plan, look at the "Features+pricing" blade in the browser.</span></span> <span data-ttu-id="c3d45-141">**Uppdatera webbläsaren vyn** så att du kan se det aktuella tillståndet.</span><span class="sxs-lookup"><span data-stu-id="c3d45-141">**Refresh the browser view** to make sure you see the latest state.</span></span>



## <a name="add-a-metric-alert"></a><span data-ttu-id="c3d45-142">Lägg till ett mått varning</span><span class="sxs-lookup"><span data-stu-id="c3d45-142">Add a metric alert</span></span>

<span data-ttu-id="c3d45-143">Koppla koden så här om du vill konfigurera en avisering om mått samtidigt som din app resurs till mallfilen:</span><span class="sxs-lookup"><span data-stu-id="c3d45-143">To set up a metric alert at the same time as your app resource, merge code like this into the template file:</span></span>

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
      // Ensure this resource is created after the app resource:
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

<span data-ttu-id="c3d45-144">När du anropar mallen kan du lägga till den här parametern:</span><span class="sxs-lookup"><span data-stu-id="c3d45-144">When you invoke the template, you can optionally add this parameter:</span></span>

    `-responseTime 2`

<span data-ttu-id="c3d45-145">Du kan självklart parameterstyra andra fält.</span><span class="sxs-lookup"><span data-stu-id="c3d45-145">You can of course parameterize other fields.</span></span> 

<span data-ttu-id="c3d45-146">Om du vill ta reda på namn och konfigurationsinformation för andra Varningsregler kan manuellt skapa en regel och kontrollera sedan i [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c3d45-146">To find out the type names and configuration details of other alert rules, create a rule manually and then inspect it in [Azure Resource Manager](https://resources.azure.com/).</span></span> 


## <a name="add-an-availability-test"></a><span data-ttu-id="c3d45-147">Lägg till ett tillgänglighetstest</span><span class="sxs-lookup"><span data-stu-id="c3d45-147">Add an availability test</span></span>

<span data-ttu-id="c3d45-148">Det här exemplet är för en ping-testet (att testa en enstaka sida).</span><span class="sxs-lookup"><span data-stu-id="c3d45-148">This example is for a ping test (to test a single page).</span></span>  

<span data-ttu-id="c3d45-149">**Det finns två delar** i ett tillgänglighetstest: testet sig själv och den avisering som meddelar dig om fel.</span><span class="sxs-lookup"><span data-stu-id="c3d45-149">**There are two parts** in an availability test: the test itself, and the alert that notifies you of failures.</span></span>

<span data-ttu-id="c3d45-150">Sammanfoga följande kod till mallfilen som skapar appen.</span><span class="sxs-lookup"><span data-stu-id="c3d45-150">Merge the following code into the template file that creates the app.</span></span>

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
      // Availability test: part 1 configures the test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after the app resource:
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
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, the alert rule
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

<span data-ttu-id="c3d45-151">Identifiera koderna för andra test platser eller automatisera skapandet av mer komplexa webbtester, skapa ett exempel manuellt och sedan parameterstyra koden från [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c3d45-151">To discover the codes for other test locations, or to automate the creation of more complex web tests, create an example manually and then parameterize the code from [Azure Resource Manager](https://resources.azure.com/).</span></span>

## <a name="add-more-resources"></a><span data-ttu-id="c3d45-152">Lägg till fler resurser</span><span class="sxs-lookup"><span data-stu-id="c3d45-152">Add more resources</span></span>

<span data-ttu-id="c3d45-153">Om du vill automatisera skapandet av någon annan resurs av något slag, skapa ett exempel manuellt kopiera och parameterstyra dess kod från [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c3d45-153">To automate the creation of any other resource of any kind, create an example manually, and then copy and parameterize its code from [Azure Resource Manager](https://resources.azure.com/).</span></span> 

1. <span data-ttu-id="c3d45-154">Öppna [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c3d45-154">Open [Azure Resource Manager](https://resources.azure.com/).</span></span> <span data-ttu-id="c3d45-155">Bläddra nedåt i `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, program-resurs.</span><span class="sxs-lookup"><span data-stu-id="c3d45-155">Navigate down through `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, to your application resource.</span></span> 
   
    ![Navigering i Azure Resursläsaren](./media/app-insights-powershell/01.png)
   
    <span data-ttu-id="c3d45-157">*Komponenter* är de grundläggande Application Insights-resurserna för att visa program.</span><span class="sxs-lookup"><span data-stu-id="c3d45-157">*Components* are the basic Application Insights resources for displaying applications.</span></span> <span data-ttu-id="c3d45-158">Det finns olika resurser för associerade Varningsregler och tillgänglighetstester för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c3d45-158">There are separate resources for the associated alert rules and availability web tests.</span></span>
2. <span data-ttu-id="c3d45-159">Kopiera JSON av komponenten till lämplig plats i `template1.json`.</span><span class="sxs-lookup"><span data-stu-id="c3d45-159">Copy the JSON of the component into the appropriate place in `template1.json`.</span></span>
3. <span data-ttu-id="c3d45-160">Ta bort dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="c3d45-160">Delete these properties:</span></span>
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. <span data-ttu-id="c3d45-161">Öppna avsnitten webtests och alertrules och kopiera JSON för enskilda objekt i mallen.</span><span class="sxs-lookup"><span data-stu-id="c3d45-161">Open the webtests and alertrules sections and copy the JSON for individual items into your template.</span></span> <span data-ttu-id="c3d45-162">(Inte kopiera från webtests eller alertrules noder: Gå till posterna under dem.)</span><span class="sxs-lookup"><span data-stu-id="c3d45-162">(Don't copy from the webtests or alertrules nodes: go into the items under them.)</span></span>
   
    <span data-ttu-id="c3d45-163">Varje webbtest har en associerad aviseringsregel, så du måste kopiera båda.</span><span class="sxs-lookup"><span data-stu-id="c3d45-163">Each web test has an associated alert rule, so you have to copy both of them.</span></span>
   
    <span data-ttu-id="c3d45-164">Du kan även inkludera aviseringar på mått.</span><span class="sxs-lookup"><span data-stu-id="c3d45-164">You can also include alerts on metrics.</span></span> <span data-ttu-id="c3d45-165">[Tjänstmåttets namn](app-insights-powershell-alerts.md#metric-names).</span><span class="sxs-lookup"><span data-stu-id="c3d45-165">[Metric names](app-insights-powershell-alerts.md#metric-names).</span></span>
5. <span data-ttu-id="c3d45-166">Infoga raden i varje resurs:</span><span class="sxs-lookup"><span data-stu-id="c3d45-166">Insert this line in each resource:</span></span>
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a><span data-ttu-id="c3d45-167">Parameterstyra mallen</span><span class="sxs-lookup"><span data-stu-id="c3d45-167">Parameterize the template</span></span>
<span data-ttu-id="c3d45-168">Nu har du ersätta de specifika namn med parametrar.</span><span class="sxs-lookup"><span data-stu-id="c3d45-168">Now you have to replace the specific names with parameters.</span></span> <span data-ttu-id="c3d45-169">Att [parameterstyra en mall](../azure-resource-manager/resource-group-authoring-templates.md), du kan skriva uttryck med en [uppsättning Hjälpfunktioner](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="c3d45-169">To [parameterize a template](../azure-resource-manager/resource-group-authoring-templates.md), you write expressions using a [set of helper functions](../azure-resource-manager/resource-group-template-functions.md).</span></span> 

<span data-ttu-id="c3d45-170">Du kan inte parameterstyra bara en del av en sträng, så Använd `concat()` att skapa strängar.</span><span class="sxs-lookup"><span data-stu-id="c3d45-170">You can't parameterize just part of a string, so use `concat()` to build strings.</span></span>

<span data-ttu-id="c3d45-171">Här följer exempel på ersättningar som du vill se.</span><span class="sxs-lookup"><span data-stu-id="c3d45-171">Here are examples of the substitutions you'll want to make.</span></span> <span data-ttu-id="c3d45-172">Det finns flera förekomster av varje ersättning.</span><span class="sxs-lookup"><span data-stu-id="c3d45-172">There are several occurrences of each substitution.</span></span> <span data-ttu-id="c3d45-173">Du kan behöva andra i mallen.</span><span class="sxs-lookup"><span data-stu-id="c3d45-173">You might need others in your template.</span></span> <span data-ttu-id="c3d45-174">Dessa exempel används parametrar och variabler som vi har definierat överst i mallen.</span><span class="sxs-lookup"><span data-stu-id="c3d45-174">These examples use the parameters and variables we defined at the top of the template.</span></span>

| <span data-ttu-id="c3d45-175">hitta</span><span class="sxs-lookup"><span data-stu-id="c3d45-175">find</span></span> | <span data-ttu-id="c3d45-176">Ersätt med</span><span class="sxs-lookup"><span data-stu-id="c3d45-176">replace with</span></span> |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| <span data-ttu-id="c3d45-177">`"myappname"`(gemen)</span><span class="sxs-lookup"><span data-stu-id="c3d45-177">`"myappname"` (lower case)</span></span> |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/><span data-ttu-id="c3d45-178">Ta bort Guid och -Id.</span><span class="sxs-lookup"><span data-stu-id="c3d45-178">Delete Guid and Id.</span></span> |

### <a name="set-dependencies-between-the-resources"></a><span data-ttu-id="c3d45-179">Ange beroenden mellan resurser</span><span class="sxs-lookup"><span data-stu-id="c3d45-179">Set dependencies between the resources</span></span>
<span data-ttu-id="c3d45-180">Azure bör ställa in resurser i strikt ordning.</span><span class="sxs-lookup"><span data-stu-id="c3d45-180">Azure should set up the resources in strict order.</span></span> <span data-ttu-id="c3d45-181">Lägg till beroende rader om du vill kontrollera en installationen har slutförts innan nästa börjar:</span><span class="sxs-lookup"><span data-stu-id="c3d45-181">To make sure one setup completes before the next begins, add dependency lines:</span></span>

* <span data-ttu-id="c3d45-182">Testa resurs i tillgänglighet:</span><span class="sxs-lookup"><span data-stu-id="c3d45-182">In the availability test resource:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* <span data-ttu-id="c3d45-183">I aviseringen resursen för ett tillgänglighetstest:</span><span class="sxs-lookup"><span data-stu-id="c3d45-183">In the alert resource for an availability test:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a><span data-ttu-id="c3d45-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3d45-184">Next steps</span></span>
<span data-ttu-id="c3d45-185">Andra automation-artiklar:</span><span class="sxs-lookup"><span data-stu-id="c3d45-185">Other automation articles:</span></span>

* <span data-ttu-id="c3d45-186">[Skapa en resurs för Application Insights](app-insights-powershell-script-create-resource.md) -snabb metod utan att använda en mall.</span><span class="sxs-lookup"><span data-stu-id="c3d45-186">[Create an Application Insights resource](app-insights-powershell-script-create-resource.md) - quick method without using a template.</span></span>
* [<span data-ttu-id="c3d45-187">Konfigurera aviseringar</span><span class="sxs-lookup"><span data-stu-id="c3d45-187">Set up Alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="c3d45-188">Skapa webbtester</span><span class="sxs-lookup"><span data-stu-id="c3d45-188">Create web tests</span></span>](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [<span data-ttu-id="c3d45-189">Skicka Azure Diagnostics-data till Application Insights</span><span class="sxs-lookup"><span data-stu-id="c3d45-189">Send Azure Diagnostics to Application Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="c3d45-190">Distribuera till Azure från GitHub</span><span class="sxs-lookup"><span data-stu-id="c3d45-190">Deploy to Azure from GitHub</span></span>](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [<span data-ttu-id="c3d45-191">Skapa versionen anteckningar</span><span class="sxs-lookup"><span data-stu-id="c3d45-191">Create release annotations</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

