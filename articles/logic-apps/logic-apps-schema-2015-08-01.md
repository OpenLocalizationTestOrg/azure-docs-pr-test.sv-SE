---
title: Schemat uppdateras augusti 1 2015 preview - Azure Logic Apps | Microsoft Docs
description: "Skapa JSON-definitioner för Logic Apps i Azure med schemaversionen 2015-08-01-preview"
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.openlocfilehash: 35d7a56d5607dcc18a4407c65b92962d3d0dcd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a><span data-ttu-id="e7fc3-103">Schema-uppdateringar för Logic Apps i Azure - preview 1 augusti 2015</span><span class="sxs-lookup"><span data-stu-id="e7fc3-103">Schema updates for Azure Logic Apps - August 1, 2015 preview</span></span>

<span data-ttu-id="e7fc3-104">Den här nya schemat och API-version för Logikappar i Azure innehåller viktiga förbättringar som gör logikappar mer tillförlitlig och enklare att använda:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier to use:</span></span>

*   <span data-ttu-id="e7fc3-105">Den **APIApp** åtgärdstyp uppdateras till en ny [ **APIConnection** ](#api-connections) åtgärdstyp.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-105">The **APIApp** action type is updated to a new [**APIConnection**](#api-connections) action type.</span></span>
*   <span data-ttu-id="e7fc3-106">**Upprepa** ändras till [ **Foreach**](#foreach).</span><span class="sxs-lookup"><span data-stu-id="e7fc3-106">**Repeat** is renamed to [**Foreach**](#foreach).</span></span>
*   <span data-ttu-id="e7fc3-107">Den [ **HTTP-lyssnaren** API-App](#http-listener) krävs inte längre.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-107">The [**HTTP Listener** API App](#http-listener) is no longer required.</span></span>
*   <span data-ttu-id="e7fc3-108">Anropar underordnade arbetsflöden använder en [nya schemat](#child-workflows).</span><span class="sxs-lookup"><span data-stu-id="e7fc3-108">Calling child workflows uses a [new schema](#child-workflows).</span></span>

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a><span data-ttu-id="e7fc3-109">Flytta till API-anslutningar</span><span class="sxs-lookup"><span data-stu-id="e7fc3-109">Move to API connections</span></span>

<span data-ttu-id="e7fc3-110">Den största förändringen är att du inte längre behöver distribuera API Apps i Azure-prenumerationen så att du kan använda API: er.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-110">The biggest change is that you no longer have to deploy API Apps into your Azure subscription so you can use APIs.</span></span> <span data-ttu-id="e7fc3-111">Här är det sätt som du kan använda API: er:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-111">Here are the ways that you can use APIs:</span></span>

* <span data-ttu-id="e7fc3-112">Hanterade API: er</span><span class="sxs-lookup"><span data-stu-id="e7fc3-112">Managed APIs</span></span>
* <span data-ttu-id="e7fc3-113">Dina anpassade webb-API: er</span><span class="sxs-lookup"><span data-stu-id="e7fc3-113">Your custom Web APIs</span></span>

<span data-ttu-id="e7fc3-114">Varje sätt hanteras lite annorlunda eftersom deras hantering och värd modeller är olika.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-114">Each way is handled slightly differently because their management and hosting models are different.</span></span> <span data-ttu-id="e7fc3-115">En fördel med den här modellen är du inte längre är begränsad till resurser som har distribuerats i din Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-115">One advantage of this model is you're no longer constrained to resources that are deployed in your Azure resource group.</span></span> 

### <a name="managed-apis"></a><span data-ttu-id="e7fc3-116">Hanterade API: er</span><span class="sxs-lookup"><span data-stu-id="e7fc3-116">Managed APIs</span></span>

<span data-ttu-id="e7fc3-117">Microsoft hanterar vissa API: er för din räkning, till exempel Office 365, Salesforce, Twitter och FTP.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-117">Microsoft manages some APIs on your behalf, such as Office 365, Salesforce, Twitter, and FTP.</span></span> <span data-ttu-id="e7fc3-118">Du kan använda vissa hanterade API: er som-är, till exempel Bing översätta, medan andra kräver konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-118">You can use some managed APIs as-is, such as Bing Translate, while others require configuration.</span></span> <span data-ttu-id="e7fc3-119">Den här konfigurationen kallas en *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-119">This configuration is called a *connection*.</span></span>

<span data-ttu-id="e7fc3-120">När du använder Office 365, måste du skapa en anslutning som innehåller din Office 365-inloggning token.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-120">For example, when you use Office 365, you must create a connection that contains your Office 365 sign-in token.</span></span> <span data-ttu-id="e7fc3-121">Denna token lagras på ett säkert sätt och uppdateras så att din logikapp alltid kan anropa API för Office 365.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-121">This token is securely stored and refreshed so that your logic app can always call the Office 365 API.</span></span> <span data-ttu-id="e7fc3-122">Om du vill ansluta till SQL- eller FTP-server måste du skapa en anslutning med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-122">Alternatively, if you want to connect to your SQL or FTP server, you must create a connection that has the connection string.</span></span> 

<span data-ttu-id="e7fc3-123">I den här definitionen åtgärderna kallas `APIConnection`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-123">In this definition, these actions are called `APIConnection`.</span></span> <span data-ttu-id="e7fc3-124">Här är ett exempel på en anslutning som anropar Office 365 om du vill skicka ett e-postmeddelande:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-124">Here is an example of a connection that calls Office 365 to send an email:</span></span>

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-125">Den `host` objekt som är del av indata som är unik för API-anslutningar och innehåller dra delar: `api` och `connection`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-125">The `host` object is portion of inputs that is unique to API connections, and contains tow parts: `api` and `connection`.</span></span>

<span data-ttu-id="e7fc3-126">Den `api` har körningsmiljön URL där som hanterad API finns.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-126">The `api` has the runtime URL of where that managed API is hosted.</span></span> <span data-ttu-id="e7fc3-127">Du kan se alla tillgängliga hanterade API: er genom att anropa `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-127">You can see all the available managed APIs by calling `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span></span>

<span data-ttu-id="e7fc3-128">När du använder ett API API: et kanske eller kanske inte har någon *anslutningsparametrar* definieras.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-128">When you use an API, the API might or might not have any *connection parameters* defined.</span></span> <span data-ttu-id="e7fc3-129">Om API: T inte är ingen *anslutning* krävs.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-129">If the API doesn't, no *connection* is required.</span></span> <span data-ttu-id="e7fc3-130">Om API: et matchar, måste du skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-130">If the API does, you must create a connection.</span></span> <span data-ttu-id="e7fc3-131">Den skapade anslutningen har namn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-131">The created connection has the name that you choose.</span></span> <span data-ttu-id="e7fc3-132">Sedan kan du referera namn i den `connection` objekt i den `host` objekt.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-132">You then reference the name in the `connection` object inside the `host` object.</span></span> <span data-ttu-id="e7fc3-133">Anropa för att skapa en anslutning i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-133">To create a connection in a resource group, call:</span></span>

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

<span data-ttu-id="e7fc3-134">Med följande text:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-134">With the following body:</span></span>

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a><span data-ttu-id="e7fc3-135">Distribuera hanterade API: er i en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="e7fc3-135">Deploy managed APIs in an Azure Resource Manager template</span></span>

<span data-ttu-id="e7fc3-136">Du kan skapa en fullständig program i en Azure Resource Manager-mall så länge interaktiv inloggning krävs inte.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-136">You can create a full application in an Azure Resource Manager template as long as interactive sign-in isn't required.</span></span>
<span data-ttu-id="e7fc3-137">Om inloggning krävs, kan du ställa in allt med hjälp av Azure Resource Manager-mallen, men du fortfarande behöver Besök portalen för att godkänna anslutningar.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-137">If sign-in is required, you can set up everything with the Azure Resource Manager template, but you still have to visit the portal to authorize the connections.</span></span> 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/', parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:, Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

<span data-ttu-id="e7fc3-138">Du kan se i det här exemplet att anslutningarna är bara resurser som bor i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-138">You can see in this example that the connections are just resources that live in your resource group.</span></span> <span data-ttu-id="e7fc3-139">De refererar till den hanterade API: er finns i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-139">They reference the managed APIs available to you in your subscription.</span></span>

### <a name="your-custom-web-apis"></a><span data-ttu-id="e7fc3-140">Dina anpassade webb-API: er</span><span class="sxs-lookup"><span data-stu-id="e7fc3-140">Your custom Web APIs</span></span>

<span data-ttu-id="e7fc3-141">Om du använder egna API: er, inte Microsoft-hanterad de använda inbyggt **HTTP** åtgärder för att anropa dem.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-141">If you use your own APIs, not Microsoft-managed ones, use the built-in **HTTP** action to call them.</span></span> <span data-ttu-id="e7fc3-142">För en perfekt upplevelse bör du exponera en Swagger-slutpunkt för din API.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-142">For an ideal experience, you should expose a Swagger endpoint for your API.</span></span> <span data-ttu-id="e7fc3-143">Den här slutpunkten aktiverar logiken App Designer ska renderas indata och utdata för din API.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-143">This endpoint enables the Logic App Designer to render the inputs and outputs for your API.</span></span> <span data-ttu-id="e7fc3-144">Swagger, kan designern bara visa in- och utgångar som täckande JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-144">Without Swagger, the designer can only show the inputs and outputs as opaque JSON objects.</span></span>

<span data-ttu-id="e7fc3-145">Här är ett exempel som visar den nya `metadata.apiDefinitionUrl` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-145">Here is an example showing the new `metadata.apiDefinitionUrl` property:</span></span>

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-146">Om du är värd för webb-API på Azure App Service, visas Web API automatiskt i listan över åtgärder som är tillgängliga i designern.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-146">If you host your Web API on Azure App Service, your Web API automatically appears in the list of actions available in the designer.</span></span> <span data-ttu-id="e7fc3-147">Om inte, måste du klistra in i URL-Adressen direkt.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-147">If not, you have to paste in the URL directly.</span></span> <span data-ttu-id="e7fc3-148">Swagger-slutpunkten måste vara oautentiserade kan användas i logik App Designer, men du kan skydda API sig själv med de metoder som har stöd för Swagger.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-148">The Swagger endpoint must be unauthenticated to be usable in the Logic App Designer, although you can secure the API itself with whatever methods that Swagger supports.</span></span>

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a><span data-ttu-id="e7fc3-149">Anropa distribuerade API-appar med 2015-08-01-preview</span><span class="sxs-lookup"><span data-stu-id="e7fc3-149">Call deployed API apps with 2015-08-01-preview</span></span>

<span data-ttu-id="e7fc3-150">Om du tidigare har distribuerat en API-App kan du anropa appen med den **HTTP** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-150">If you previously deployed an API App, you can call the app with the **HTTP** action.</span></span>

<span data-ttu-id="e7fc3-151">Om du använder Dropbox att lista filer, till exempel din **2014-12-01-preview** version schemadefinition kan ha något som liknar:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-151">For example, if you use Dropbox to list files, your **2014-12-01-preview** schema version definition might have something like:</span></span>

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-152">Du kan skapa motsvarande HTTP-åtgärden som exempel, när avsnittet parametrar av logik app definition förblir oförändrad:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-152">You can construct the equivalent HTTP action like this example, while the parameters section of the Logic app definition remains unchanged:</span></span>

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata": {
              "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method": "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-153">Gå igenom de här egenskaperna i taget:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-153">Walking through these properties one-by-one:</span></span>

| <span data-ttu-id="e7fc3-154">Åtgärdsegenskap</span><span class="sxs-lookup"><span data-stu-id="e7fc3-154">Action property</span></span> | <span data-ttu-id="e7fc3-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e7fc3-155">Description</span></span> |
| --- | --- |
| `type` |<span data-ttu-id="e7fc3-156">`Http`Istället för`APIapp`</span><span class="sxs-lookup"><span data-stu-id="e7fc3-156">`Http` instead of `APIapp`</span></span> |
| `metadata.apiDefinitionUrl` |<span data-ttu-id="e7fc3-157">Om du vill använda den här åtgärden i logik App Designer är metadataslutpunkten som har skapats från:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span><span class="sxs-lookup"><span data-stu-id="e7fc3-157">To use this action in the Logic App Designer, include the metadata endpoint, which is constructed from: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span></span> |
| `inputs.uri` |<span data-ttu-id="e7fc3-158">Konstrueras från:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14`</span><span class="sxs-lookup"><span data-stu-id="e7fc3-158">Constructed from: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14`</span></span> |
| `inputs.method` |<span data-ttu-id="e7fc3-159">Alltid`POST`</span><span class="sxs-lookup"><span data-stu-id="e7fc3-159">Always `POST`</span></span> |
| `inputs.body` |<span data-ttu-id="e7fc3-160">Identiskt med parametrarna API-App</span><span class="sxs-lookup"><span data-stu-id="e7fc3-160">Identical to the API App parameters</span></span> |
| `inputs.authentication` |<span data-ttu-id="e7fc3-161">Identisk med autentiseringen för API-App</span><span class="sxs-lookup"><span data-stu-id="e7fc3-161">Identical to the API App authentication</span></span> |

<span data-ttu-id="e7fc3-162">Den här metoden ska fungera för alla åtgärder i API-App.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-162">This approach should work for all API App actions.</span></span> <span data-ttu-id="e7fc3-163">Tänk dock på att dessa tidigare API Apps stöds inte längre.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-163">However, remember that these previous API Apps are no longer supported.</span></span> <span data-ttu-id="e7fc3-164">Därför bör du övergå till en av de två andra föregående alternativen, hanterade API eller värd för ditt anpassade webb-API.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-164">So you should move to one of the two other previous options, a managed API or hosting your custom Web API.</span></span>

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a><span data-ttu-id="e7fc3-165">Byta namn på 'repeat' till 'foreach'</span><span class="sxs-lookup"><span data-stu-id="e7fc3-165">Renamed 'repeat' to 'foreach'</span></span>

<span data-ttu-id="e7fc3-166">För den föregående schemaversionen vi har tagit emot mycket kundfeedback som **Upprepa** var förvirrande och inte korrekt avbilda som **Upprepa** verkligen är en för varje slinga.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-166">For the previous schema version, we received much customer feedback that **Repeat** was confusing and didn't properly capture that **Repeat** was really a for-each loop.</span></span> <span data-ttu-id="e7fc3-167">Därför har vi bytt namn `repeat` till `foreach`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-167">As a result, we have renamed `repeat` to `foreach`.</span></span> <span data-ttu-id="e7fc3-168">Till exempel tidigare du skulle kunna skriva:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-168">For example, previously you would write:</span></span>

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-169">Nu skulle du skriva:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-169">Now you would write:</span></span>

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-170">Funktionen `@repeatItem()` användes tidigare för att referera till det aktuella objektet som hävdade över.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-170">The function `@repeatItem()` was previously used to reference the current item being iterated over.</span></span> <span data-ttu-id="e7fc3-171">Den här funktionen förenklas nu till `@item()`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-171">This function is now simplified to `@item()`.</span></span> 

### <a name="reference-outputs-from-foreach"></a><span data-ttu-id="e7fc3-172">Referens för utdata från 'foreach'</span><span class="sxs-lookup"><span data-stu-id="e7fc3-172">Reference outputs from 'foreach'</span></span>

<span data-ttu-id="e7fc3-173">För enkelhetens skull utdata från `foreach` åtgärder som inte är omslutna i ett objekt som kallas `repeatItems`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-173">For simplification, the outputs from `foreach` actions are not wrapped in an object called `repeatItems`.</span></span> <span data-ttu-id="e7fc3-174">När utdata från den tidigare `repeat` exempel var:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-174">While the outputs from the previous `repeat` example were:</span></span>

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

<span data-ttu-id="e7fc3-175">Nu är dessa utdata:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-175">Now these outputs are:</span></span>

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

<span data-ttu-id="e7fc3-176">Tidigare få till i brödtexten för åtgärden när du refererar till dessa utdata:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-176">Previously, to get to the body of the action when referencing these outputs:</span></span>

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-177">Nu kan du göra i stället:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-177">Now you can do instead:</span></span>

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

<span data-ttu-id="e7fc3-178">Med de här ändringarna, funktionerna `@repeatItem()`, `@repeatBody()`, och `@repeatOutputs()` tas bort.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-178">With these changes, the functions `@repeatItem()`, `@repeatBody()`, and `@repeatOutputs()` are removed.</span></span>

<a name="http-listener"></a>
## <a name="native-http-listener"></a><span data-ttu-id="e7fc3-179">Inbyggda HTTP-lyssnare</span><span class="sxs-lookup"><span data-stu-id="e7fc3-179">Native HTTP listener</span></span>

<span data-ttu-id="e7fc3-180">HTTP-lyssnaren funktioner är nu inbyggda i.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-180">The HTTP Listener capabilities are now built in.</span></span> <span data-ttu-id="e7fc3-181">Så behöver du inte längre distribuera en http-lyssnare API-App.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-181">So you no longer need to deploy an HTTP Listener API App.</span></span> <span data-ttu-id="e7fc3-182">Se [fullständig information om hur du gör logik app slutpunkten callable här](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e7fc3-182">See [the full details for how to make your Logic app endpoint callable here](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<span data-ttu-id="e7fc3-183">Med de här ändringarna vi tagit bort den `@accessKeys()` funktion, som vi har ersatts med den `@listCallbackURL()` funktionen för att hämta slutpunkten vid behov.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-183">With these changes, we removed the `@accessKeys()` function, which we replaced with the `@listCallbackURL()` function for getting the endpoint when necessary.</span></span> <span data-ttu-id="e7fc3-184">Du måste också definiera minst en utlösare i din logikapp nu.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-184">Also, you must now define at least one trigger in your logic app.</span></span> <span data-ttu-id="e7fc3-185">Om du vill `/run` arbetsflödet, måste du ha något av dessa utlösare: `manual`, `apiConnectionWebhook`, eller `httpWebhook`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-185">If you want to `/run` the workflow, you must have one of these triggers: `manual`, `apiConnectionWebhook`, or `httpWebhook`.</span></span>

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a><span data-ttu-id="e7fc3-186">Anropa underordnade arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="e7fc3-186">Call child workflows</span></span>

<span data-ttu-id="e7fc3-187">Tidigare måste anropar underordnade arbetsflöden gå till arbetsflödet, få åtkomst-token och klistra in token i logik app definitionen där du vill anropa det underordnade arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-187">Previously, calling child workflows required going to the workflow, getting the access token, and pasting the token in the logic app definition where you want to call that child workflow.</span></span> <span data-ttu-id="e7fc3-188">Med det nya schemat genererar Logic Apps motorn automatiskt en SAS vid körning för det underordnade arbetsflödet så att du inte klistra in alla hemligheter i definitionen.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-188">With the new schema, the Logic Apps engine automatically generates a SAS at runtime for the child workflow so you don't have to paste any secrets into the definition.</span></span> <span data-ttu-id="e7fc3-189">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="e7fc3-189">Here is an example:</span></span>

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
        },
        "queries": {
            "extrafield": "specialValue"
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "conditions": []
}
```

<span data-ttu-id="e7fc3-190">En andra förbättringar är vi ger underordnade arbetsflöden fullständig åtkomst till den inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-190">A second improvement is we are giving the child workflows full access to the incoming request.</span></span> <span data-ttu-id="e7fc3-191">Det innebär att du kan skicka parametrar i den *frågor* avsnittet och i den *huvuden* objektet och att du helt kan definiera hela texten.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-191">That means that you can pass parameters in the *queries* section and in the *headers* object and that you can fully define the entire body.</span></span>

<span data-ttu-id="e7fc3-192">Slutligen finns arbetsflöden som ändringar som krävs.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-192">Finally, there are required changes to the child workflow.</span></span> <span data-ttu-id="e7fc3-193">När du tidigare kan anropa ett underordnat arbetsflöde direkt, måste nu du definiera en slutpunkt för utlösaren i arbetsflödet för överordnat att anropa.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-193">While you could previously call a child workflow directly, now you must define a trigger endpoint in the workflow for the parent to call.</span></span> <span data-ttu-id="e7fc3-194">I allmänhet kan du lägga till en utlösare som har `manual` skriver och använder sedan den utlösaren i definitionen för överordnade.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-194">Generally, you would add a trigger that has `manual` type, and then use that trigger in the parent definition.</span></span> <span data-ttu-id="e7fc3-195">Observera den `host` egenskapen specifikt har en `triggerName` eftersom du måste alltid ange vars utlösare du anropar.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-195">Note the `host` property specifically has a `triggerName` because you must always specify which trigger you are invoking.</span></span>

## <a name="other-changes"></a><span data-ttu-id="e7fc3-196">Andra ändringar</span><span class="sxs-lookup"><span data-stu-id="e7fc3-196">Other changes</span></span>

### <a name="new-queries-property"></a><span data-ttu-id="e7fc3-197">Ny 'frågor-egenskap</span><span class="sxs-lookup"><span data-stu-id="e7fc3-197">New 'queries' property</span></span>

<span data-ttu-id="e7fc3-198">Alla åtgärdstyper stöder nu en ny inmatning kallas `queries`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-198">All action types now support a new input called `queries`.</span></span> <span data-ttu-id="e7fc3-199">Den här indata kan vara en strukturerad objekt, snarare än du behöver samla strängen manuellt.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-199">This input can be a structured object, rather than you having to assemble the string by hand.</span></span>

### <a name="renamed-parse-function-to-json"></a><span data-ttu-id="e7fc3-200">Har bytt namn till parse() av funktionen 'json()'</span><span class="sxs-lookup"><span data-stu-id="e7fc3-200">Renamed 'parse()' function to 'json()'</span></span>

<span data-ttu-id="e7fc3-201">Vi lägger till mer innehåll av typen snart, så vi har fått nytt namn i `parse()` för `json()`.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-201">We are adding more content types soon, so we renamed the `parse()` function to `json()`.</span></span>

## <a name="coming-soon-enterprise-integration-apis"></a><span data-ttu-id="e7fc3-202">Kommer snart: Enterprise Integration-API: er</span><span class="sxs-lookup"><span data-stu-id="e7fc3-202">Coming soon: Enterprise Integration APIs</span></span>

<span data-ttu-id="e7fc3-203">Vi har inte hanterade versioner än av Enterprise Integration API: erna, t.ex. AS2.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-203">We don't have managed versions yet of the Enterprise Integration APIs, like AS2.</span></span> <span data-ttu-id="e7fc3-204">Under tiden kan du använda din befintliga distribuerade BizTalk-APIs via HTTP-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e7fc3-204">Meanwhile, you can use your existing deployed BizTalk APIs through the HTTP action.</span></span> <span data-ttu-id="e7fc3-205">Mer information, se ”med redan distribuerade API apps” i den [översikt över integrering](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span><span class="sxs-lookup"><span data-stu-id="e7fc3-205">For details, see "Using your already deployed API apps" in the [integration roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span></span> 
