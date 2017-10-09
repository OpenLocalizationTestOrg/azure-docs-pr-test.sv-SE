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
#  <a name="create-application-insights-resources-using-powershell"></a>Skapa Application Insights-resurser med hjälp av PowerShell
Den här artikeln visar hur tooautomate hello skapande och uppdatering av [Programinsikter](app-insights-overview.md) resurser automatiskt med Azure Resource Manager. Du kan till exempel göra det som en del av en build-process. Tillsammans med hello grundläggande Application Insights-resurs, kan du skapa [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md), Ställ in [aviseringar](app-insights-alerts.md), Ställ in hello [priser schemat](app-insights-pricing.md), och skapa andra Azure resurser.

Hej viktiga toocreating resurserna är JSON-mallar för [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md). I kort sagt kan hello proceduren är: hämta hello JSON definitioner av befintliga resurser. parameterstyra vissa värden, till exempel namn. och kör sedan hello mallen när du vill toocreate en ny resurs. Du kan paketera flera resurser tillsammans, toocreate dem i en Gå - till exempel, en app Övervakare med tillgänglighetstester, aviseringar och lagring för löpande export. Det finns vissa detaljerad och nyansrik toosome av hello parameterizations som beskrivs här.

## <a name="one-time-setup"></a>Enstaka installationen
Om du inte har använt PowerShell med din Azure-prenumeration innan du:

Installera hello Azure Powershell-modulen på hello datorn där du vill att toorun hello skript:

1. Installera [Microsoft Web Platform Installer (v5 eller högre)](http://www.microsoft.com/web/downloads/platform.aspx).
2. Använd den tooinstall Microsoft Azure Powershell.

## <a name="create-an-azure-resource-manager-template"></a>Skapa en Azure Resource Manager-mall
Skapa en ny fil i JSON - vi anropa den `template1.json` i det här exemplet. Kopiera innehållet till den:

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



## <a name="create-application-insights-resources"></a>Skapa Application Insights-resurser
1. Logga in tooAzure i PowerShell:
   
    `Login-AzureRmAccount`
2. Kör ett kommando som detta:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName`är hello grupp där du vill att toocreate hello nya resurser.
   * `-TemplateFile`måste inträffa innan hello anpassade parametrar.
   * `-appName`hello namnet på hello resurs toocreate.

Du kan lägga till andra parametrar - hittar du beskrivningar under hello parametrar i hello mallen.

## <a name="tooget-hello-instrumentation-key"></a>tooget hello instrumentation nyckel
När du skapar en resurs för programmet, vill du hello instrumentation nyckel: 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-hello-price-plan"></a>Ange hello prisplan

Du kan ange hello [prisplan](app-insights-pricing.md).

toocreate en appresursen med hello Enterprise prisplan med hello mallen ovan:

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|Planera|
|---|---|
|1|Basic|
|2|Enterprise|

* Om du bara vill toouse hello baspris standardplanen kan du utelämna hello CurrentBillingFeatures resurs från hello mallen.
* Om du vill toochange hello pris när hello komponenten resursen har skapats, kan du använda en mall som utesluter hello ”microsoft.insights/components” resurs. Dessutom utelämna hello `dependsOn` nod från hello fakturering resurs. 

tooverify Hej uppdaterade prisplan, titta på bladet hello ”funktioner + pris” i hello webbläsare. **Uppdatera hello webbläsaren visa** toomake som du kan se hello senaste tillstånd.



## <a name="add-a-metric-alert"></a>Lägg till ett mått varning

tooset upp ett mått varning vid hello samma tid som din app resurs merge kod så här i hello mallfilen:

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

När du anropar hello mallen du om du vill lägga till den här parametern:

    `-responseTime 2`

Du kan självklart parameterstyra andra fält. 

toofind hello typnamn och konfigurationsinformation för andra Varningsregler manuellt skapa en regel och kontrollera sedan i [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Lägg till ett tillgänglighetstest

Det här exemplet är för ping-testet (tootest en enstaka sida).  

**Det finns två delar** i ett tillgänglighetstest: hello test sig själv och hello varning som meddelar dig om fel.

Sammanfoga hello följande kod i hello mallfilen som skapar hello app.

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

toodiscover hello koder för andra test platser eller tooautomate hello skapa mer komplexa webbtester, skapa ett exempel manuellt och parameterstyra hello kod från [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Lägg till fler resurser

tooautomate hello skapandet av någon annan resurs av något slag, skapa ett exempel manuellt och sedan kopiera och parameterstyra dess kod från [Azure Resource Manager](https://resources.azure.com/). 

1. Öppna [Azure Resource Manager](https://resources.azure.com/). Bläddra nedåt i `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour programresursen. 
   
    ![Navigering i Azure Resursläsaren](./media/app-insights-powershell/01.png)
   
    *Komponenter* är hello grundläggande Application Insights-resurser för att visa program. Det finns separata resurser för hello associerade Varningsregler och tillgänglighetstester för webbprogram.
2. Kopiera hello JSON för hello-komponenten i hello lämplig plats i `template1.json`.
3. Ta bort dessa egenskaper:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Öppna hello webtests och alertrules avsnitt och kopiera hello JSON för enskilda objekt i mallen. (Inte kopiera från hello webtests eller alertrules noder: Gå till hello posterna under dem.)
   
    Varje webbtest har en associerad aviseringsregel så att du har toocopy båda av dessa.
   
    Du kan även inkludera aviseringar på mått. [Tjänstmåttets namn](app-insights-powershell-alerts.md#metric-names).
5. Infoga raden i varje resurs:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-hello-template"></a>Parameterstyra hello mall
Nu har du tooreplace hello specifika namn med parametrar. för[parameterstyra en mall](../azure-resource-manager/resource-group-authoring-templates.md), du kan skriva uttryck med en [uppsättning Hjälpfunktioner](../azure-resource-manager/resource-group-template-functions.md). 

Du kan inte parameterstyra bara en del av en sträng, så Använd `concat()` toobuild strängar.

Här följer exempel på hello ersättningar ska du toomake. Det finns flera förekomster av varje ersättning. Du kan behöva andra i mallen. Dessa exempel används hello parametrar och variabler som vi har definierat hello överst i hello mallen.

| hitta | Ersätt med |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"`(gemen) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Ta bort Guid och -Id. |

### <a name="set-dependencies-between-hello-resources"></a>Ange beroenden mellan hello resurser
Azure bör ställa in hello resurser i strikt ordning. toomake till filen som installationsprogrammet slutförs innan nästa börjar hello Lägg till beroende rader:

* Testa resurs i hello tillgänglighet:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* I hello avisering resurs för ett tillgänglighetstest:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Nästa steg
Andra automation-artiklar:

* [Skapa en resurs för Application Insights](app-insights-powershell-script-create-resource.md) -snabb metod utan att använda en mall.
* [Konfigurera aviseringar](app-insights-powershell-alerts.md)
* [Skapa webbtester](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Skicka Azure Diagnostics tooApplication insikter](app-insights-powershell-azure-diagnostics.md)
* [Distribuera tooAzure från GitHub](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Skapa versionen anteckningar](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

