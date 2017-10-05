---
title: "Etablera ett webbprogram som använder en SQL-databas"
description: "Använd en mall för Azure Resource Manager för att distribuera en webbapp som innehåller en SQL-databas."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: fb9648e1-9bf2-4537-bc4a-ab8d4953168c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
ms.openlocfilehash: cc34f684f8c50e95a62cb7b04fd2ddce5deb68d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="provision-a-web-app-with-a-sql-database"></a><span data-ttu-id="3cdfc-103">Etablera ett webbprogram med en SQL-databas</span><span class="sxs-lookup"><span data-stu-id="3cdfc-103">Provision a web app with a SQL Database</span></span>
<span data-ttu-id="3cdfc-104">I det här avsnittet får du lära dig hur du skapar en Azure Resource Manager-mall som distribuerar en webbapp och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-104">In this topic, you will learn how to create an Azure Resource Manager template that deploys a web app and SQL Database.</span></span> <span data-ttu-id="3cdfc-105">Du kommer lära dig hur du definierar vilka resurser har distribuerats och hur du definierar parametrar som anges när distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="3cdfc-106">Du kan använda den här mallen för dina egna distributioner eller anpassa den så att den uppfyller dina krav.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="3cdfc-107">Mer information om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3cdfc-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="3cdfc-108">Mer information om hur du distribuerar appar finns [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md).</span><span class="sxs-lookup"><span data-stu-id="3cdfc-108">For more information about deploying apps, see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md).</span></span>

<span data-ttu-id="3cdfc-109">Den fullständiga mallen finns [Web App med SQL Database mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-sql-database/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="3cdfc-109">For the complete template, see [Web App With SQL Database template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-sql-database/azuredeploy.json).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a><span data-ttu-id="3cdfc-110">Vad du ska distribuera</span><span class="sxs-lookup"><span data-stu-id="3cdfc-110">What you will deploy</span></span>
<span data-ttu-id="3cdfc-111">I den här mallen kan du distribuera:</span><span class="sxs-lookup"><span data-stu-id="3cdfc-111">In this template, you will deploy:</span></span>

* <span data-ttu-id="3cdfc-112">En webbapp</span><span class="sxs-lookup"><span data-stu-id="3cdfc-112">a web app</span></span>
* <span data-ttu-id="3cdfc-113">SQL Database-server</span><span class="sxs-lookup"><span data-stu-id="3cdfc-113">SQL Database server</span></span>
* <span data-ttu-id="3cdfc-114">SQL Database</span><span class="sxs-lookup"><span data-stu-id="3cdfc-114">SQL Database</span></span>
* <span data-ttu-id="3cdfc-115">Autoskala inställningar</span><span class="sxs-lookup"><span data-stu-id="3cdfc-115">AutoScale settings</span></span>
* <span data-ttu-id="3cdfc-116">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="3cdfc-116">Alert rules</span></span>
* <span data-ttu-id="3cdfc-117">App Insights</span><span class="sxs-lookup"><span data-stu-id="3cdfc-117">App Insights</span></span>

<span data-ttu-id="3cdfc-118">Klicka på följande knapp för att köra distributionen automatiskt:</span><span class="sxs-lookup"><span data-stu-id="3cdfc-118">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="3cdfc-119">[![Distribuera till Azure](./media/app-service-web-arm-with-sql-database-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-sql-database%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="3cdfc-119">[![Deploy to Azure](./media/app-service-web-arm-with-sql-database-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-sql-database%2Fazuredeploy.json)</span></span>

## <a name="parameters-to-specify"></a><span data-ttu-id="3cdfc-120">Parametrar för att ange</span><span class="sxs-lookup"><span data-stu-id="3cdfc-120">Parameters to specify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="administratorlogin"></a><span data-ttu-id="3cdfc-121">administratorLogin</span><span class="sxs-lookup"><span data-stu-id="3cdfc-121">administratorLogin</span></span>
<span data-ttu-id="3cdfc-122">Namnet på kontot ska användas för server-databasadministratören.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-122">The account name to use for the database server administrator.</span></span>

    "administratorLogin": {
      "type": "string"
    }

### <a name="administratorloginpassword"></a><span data-ttu-id="3cdfc-123">administratorLoginPassword</span><span class="sxs-lookup"><span data-stu-id="3cdfc-123">administratorLoginPassword</span></span>
<span data-ttu-id="3cdfc-124">Lösenord för serveradministratören databasen.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-124">The password to use for the database server administrator.</span></span>

    "administratorLoginPassword": {
      "type": "securestring"
    }

### <a name="databasename"></a><span data-ttu-id="3cdfc-125">DatabaseName</span><span class="sxs-lookup"><span data-stu-id="3cdfc-125">databaseName</span></span>
<span data-ttu-id="3cdfc-126">Namnet på den nya databasen att skapa.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-126">The name of the new database to create.</span></span>

    "databaseName": {
      "type": "string",
      "defaultValue": "sampledb"
    }

### <a name="collation"></a><span data-ttu-id="3cdfc-127">Sortering</span><span class="sxs-lookup"><span data-stu-id="3cdfc-127">collation</span></span>
<span data-ttu-id="3cdfc-128">Databassorteringen för för rätt användning av tecken.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-128">The database collation to use for governing the proper use of characters.</span></span>

    "collation": {
      "type": "string",
      "defaultValue": "SQL_Latin1_General_CP1_CI_AS"
    }

### <a name="edition"></a><span data-ttu-id="3cdfc-129">Edition</span><span class="sxs-lookup"><span data-stu-id="3cdfc-129">edition</span></span>
<span data-ttu-id="3cdfc-130">Typ av databas för att skapa.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-130">The type of database to create.</span></span>

    "edition": {
      "type": "string",
      "defaultValue": "Basic",
      "allowedValues": [
        "Basic",
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "The type of database to create."
      }
    }

### <a name="maxsizebytes"></a><span data-ttu-id="3cdfc-131">maxSizeBytes</span><span class="sxs-lookup"><span data-stu-id="3cdfc-131">maxSizeBytes</span></span>
<span data-ttu-id="3cdfc-132">Den maximala storleken i byte för databasen.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-132">The maximum size, in bytes, for the database.</span></span>

    "maxSizeBytes": {
      "type": "string",
      "defaultValue": "1073741824"
    }

### <a name="requestedserviceobjectivename"></a><span data-ttu-id="3cdfc-133">requestedServiceObjectiveName</span><span class="sxs-lookup"><span data-stu-id="3cdfc-133">requestedServiceObjectiveName</span></span>
<span data-ttu-id="3cdfc-134">Namnet som motsvarar prestandanivån för edition.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-134">The name corresponding to the performance level for edition.</span></span> 

    "requestedServiceObjectiveName": {
      "type": "string",
      "defaultValue": "Basic",
      "allowedValues": [
        "Basic",
        "S0",
        "S1",
        "S2",
        "P1",
        "P2",
        "P3"
      ],
      "metadata": {
        "description": "Describes the performance level for Edition"
      }
    }

## <a name="variables-for-names"></a><span data-ttu-id="3cdfc-135">Variabler för namn</span><span class="sxs-lookup"><span data-stu-id="3cdfc-135">Variables for names</span></span>
<span data-ttu-id="3cdfc-136">Den här mallen innehåller variabler som skapar namn som används i mallen.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-136">This template includes variables that construct names used in the template.</span></span> <span data-ttu-id="3cdfc-137">Variabeln värden används den **uniqueString** funktion för att generera ett namn från resurs-id för gruppen.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-137">The variable values use the **uniqueString** function to generate a name from the resource group id.</span></span>

    "variables": {
        "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
        "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
        "sqlserverName": "[concat('sqlserver', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a><span data-ttu-id="3cdfc-138">Resurser som ska distribueras</span><span class="sxs-lookup"><span data-stu-id="3cdfc-138">Resources to deploy</span></span>
### <a name="sql-server-and-database"></a><span data-ttu-id="3cdfc-139">SQLServer och databas</span><span class="sxs-lookup"><span data-stu-id="3cdfc-139">SQL Server and Database</span></span>
<span data-ttu-id="3cdfc-140">Skapar en ny SQL Server och databas.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-140">Creates a new SQL Server and database.</span></span> <span data-ttu-id="3cdfc-141">Namnet på servern som har angetts i den **serverName** parameter och den plats som anges i den **serverLocation** parameter.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-141">The name of the server is specified in the **serverName** parameter and the location specified in the **serverLocation** parameter.</span></span> <span data-ttu-id="3cdfc-142">När du skapar den nya servern, måste du ange ett inloggningsnamn och lösenord för serveradministratören databasen.</span><span class="sxs-lookup"><span data-stu-id="3cdfc-142">When creating the new server, you must provide a login name and password for the database server administrator.</span></span> 

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
        },
        {
          "type": "firewallrules",
          "apiVersion": "2014-04-01-preview",
          "dependsOn": [
            "[variables('sqlserverName')]"
          ],
          "location": "[resourceGroup().location]",
          "name": "AllowAllAzureIps",
          "properties": {
            "endIpAddress": "0.0.0.0",
            "startIpAddress": "0.0.0.0"
          }
        }
      ]
    },

[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a><span data-ttu-id="3cdfc-143">Webbapp</span><span class="sxs-lookup"><span data-stu-id="3cdfc-143">Web app</span></span>
    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "connectionstrings",
          "dependsOn": [
            "[variables('webSiteName')]"
          ],
          "properties": {
            "DefaultConnection": {
              "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', variables('sqlserverName'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('databaseName'), ';User Id=', parameters('administratorLogin'), '@', variables('sqlserverName'), ';Password=', parameters('administratorLoginPassword'), ';')]",
              "type": "SQLServer"
            }
          }
        }
      ]
    },


### <a name="autoscale"></a><span data-ttu-id="3cdfc-144">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="3cdfc-144">AutoScale</span></span>
    {
      "apiVersion": "2014-04-01",
      "name": "[concat(variables('hostingPlanName'), '-', resourceGroup().name)]",
      "type": "Microsoft.Insights/autoscalesettings",
      "location": "[resourceGroup().location]",
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "Resource",
        "displayName": "AutoScaleSettings"
      },
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      "properties": {
        "profiles": [
          {
            "name": "Default",
            "capacity": {
              "minimum": 1,
              "maximum": 2,
              "default": 1
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "CpuPercentage",
                  "metricResourceUri": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT10M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 80.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": 1,
                  "cooldown": "PT10M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "CpuPercentage",
                  "metricResourceUri": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT1H",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 60.0
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": 1,
                  "cooldown": "PT1H"
                }
              }
            ]
          }
        ],
        "enabled": false,
        "name": "[concat(variables('hostingPlanName'), '-', resourceGroup().name)]",
        "targetResourceUri": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      }
    },


### <a name="alert-rules-for-status-codes-403-and-500s-high-cpu-and-http-queue-length"></a><span data-ttu-id="3cdfc-145">Varningsregler för 403-statuskoder och 500's hög processor- och HTTP-Kölängd</span><span class="sxs-lookup"><span data-stu-id="3cdfc-145">Alert rules for status codes 403 and 500's, High CPU, and HTTP Queue Length</span></span>
    {
      "apiVersion": "2014-04-01",
      "name": "[concat('ServerErrors ', variables('webSiteName'))]",
      "type": "Microsoft.Insights/alertrules",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[variables('webSiteName')]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Web/sites', variables('webSiteName')))]": "Resource",
        "displayName": "ServerErrorsAlertRule"
      },
      "properties": {
        "name": "[concat('ServerErrors ', variables('webSiteName'))]",
        "description": "[concat(variables('webSiteName'), ' has some server errors, status code 5xx.')]",
        "isEnabled": false,
        "condition": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]",
            "metricName": "Http5xx"
          },
          "operator": "GreaterThan",
          "threshold": 0.0,
          "windowSize": "PT5M"
        },
        "action": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
          "sendToServiceOwners": true,
          "customEmails": [ ]
        }
      }
    },
    {
      "apiVersion": "2014-04-01",
      "name": "[concat('ForbiddenRequests ', variables('webSiteName'))]",
      "type": "Microsoft.Insights/alertrules",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[variables('webSiteName')]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Web/sites', variables('webSiteName')))]": "Resource",
        "displayName": "ForbiddenRequestsAlertRule"
      },
      "properties": {
        "name": "[concat('ForbiddenRequests ', variables('webSiteName'))]",
        "description": "[concat(variables('webSiteName'), ' has some requests that are forbidden, status code 403.')]",
        "isEnabled": false,
        "condition": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]",
            "metricName": "Http403"
          },
          "operator": "GreaterThan",
          "threshold": 0,
          "windowSize": "PT5M"
        },
        "action": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
          "sendToServiceOwners": true,
          "customEmails": [ ]
        }
      }
    },
    {
      "apiVersion": "2014-04-01",
      "name": "[concat('CPUHigh ', variables('hostingPlanName'))]",
      "type": "Microsoft.Insights/alertrules",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "Resource",
        "displayName": "CPUHighAlertRule"
      },
      "properties": {
        "name": "[concat('CPUHigh ', variables('hostingPlanName'))]",
        "description": "[concat('The average CPU is high across all the instances of ', variables('hostingPlanName'))]",
        "isEnabled": false,
        "condition": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
            "metricName": "CpuPercentage"
          },
          "operator": "GreaterThan",
          "threshold": 90,
          "windowSize": "PT15M"
        },
        "action": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
          "sendToServiceOwners": true,
          "customEmails": [ ]
        }
      }
    },
    {
      "apiVersion": "2014-04-01",
      "name": "[concat('LongHttpQueue ', variables('hostingPlanName'))]",
      "type": "Microsoft.Insights/alertrules",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "Resource",
        "displayName": "AutoScaleSettings"
      },
      "properties": {
        "name": "[concat('LongHttpQueue ', variables('hostingPlanName'))]",
        "description": "[concat('The HTTP queue for the instances of ', variables('hostingPlanName'), ' has a large number of pending requests.')]",
        "isEnabled": false,
        "condition": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]",
            "metricName": "HttpQueueLength"
          },
          "operator": "GreaterThan",
          "threshold": 100.0,
          "windowSize": "PT5M"
        },
        "action": {
          "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
          "sendToServiceOwners": true,
          "customEmails": [ ]
        }
      }
    },

### <a name="app-insights"></a><span data-ttu-id="3cdfc-146">App Insights</span><span class="sxs-lookup"><span data-stu-id="3cdfc-146">App Insights</span></span>
    {
      "apiVersion": "2014-04-01",
      "name": "[concat('AppInsights', variables('webSiteName'))]",
      "type": "Microsoft.Insights/components",
      "location": "Central US",
      "dependsOn": [
        "[variables('webSiteName')]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Web/sites', variables('webSiteName')))]": "Resource",
        "displayName": "AppInsightsComponent"
      },
      "properties": {
        "ApplicationId": "[variables('webSiteName')]"
      }
    }

## <a name="commands-to-run-deployment"></a><span data-ttu-id="3cdfc-147">Kommandon för att köra distributionen</span><span class="sxs-lookup"><span data-stu-id="3cdfc-147">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="3cdfc-148">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3cdfc-148">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json

### <a name="azure-cli"></a><span data-ttu-id="3cdfc-149">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3cdfc-149">Azure CLI</span></span>

    azure config mode arm
    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json

### <a name="azure-cli-20"></a><span data-ttu-id="3cdfc-150">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3cdfc-150">Azure CLI 2.0</span></span>

    az resource deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE]
> <span data-ttu-id="3cdfc-151">Innehållet i JSON-fil med parametrar finns [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-sql-database/azuredeploy.parameters.json).</span><span class="sxs-lookup"><span data-stu-id="3cdfc-151">For content of the parameters JSON file, see [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-sql-database/azuredeploy.parameters.json).</span></span>
>
>
