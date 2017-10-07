---
title: aaaSchema uppdaterar augusti 1 2015 preview - Azure Logic Apps | Microsoft Docs
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
ms.openlocfilehash: 950cd18a27aa1859c4f0b6116de3fb8699d746c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a><span data-ttu-id="2fbfb-103">Schema-uppdateringar för Logic Apps i Azure - preview 1 augusti 2015</span><span class="sxs-lookup"><span data-stu-id="2fbfb-103">Schema updates for Azure Logic Apps - August 1, 2015 preview</span></span>

<span data-ttu-id="2fbfb-104">Den här nya schemat och API-version för Logikappar i Azure innehåller viktiga förbättringar som gör logikappar mer tillförlitlig och enklare toouse:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier toouse:</span></span>

*   <span data-ttu-id="2fbfb-105">Hej **APIApp** åtgärdstypen är uppdaterade tooa nya [ **APIConnection** ](#api-connections) åtgärdstyp.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-105">hello **APIApp** action type is updated tooa new [**APIConnection**](#api-connections) action type.</span></span>
*   <span data-ttu-id="2fbfb-106">**Upprepa** har bytt namn för[**Foreach**](#foreach).</span><span class="sxs-lookup"><span data-stu-id="2fbfb-106">**Repeat** is renamed too[**Foreach**](#foreach).</span></span>
*   <span data-ttu-id="2fbfb-107">Hej [ **HTTP-lyssnaren** API-App](#http-listener) krävs inte längre.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-107">hello [**HTTP Listener** API App](#http-listener) is no longer required.</span></span>
*   <span data-ttu-id="2fbfb-108">Anropar underordnade arbetsflöden använder en [nya schemat](#child-workflows).</span><span class="sxs-lookup"><span data-stu-id="2fbfb-108">Calling child workflows uses a [new schema](#child-workflows).</span></span>

<a name="api-connections"></a>
## <a name="move-tooapi-connections"></a><span data-ttu-id="2fbfb-109">Flytta tooAPI anslutningar</span><span class="sxs-lookup"><span data-stu-id="2fbfb-109">Move tooAPI connections</span></span>

<span data-ttu-id="2fbfb-110">hello största förändringen är att du inte längre har toodeploy API Apps i Azure-prenumerationen så att du kan använda API: er.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-110">hello biggest change is that you no longer have toodeploy API Apps into your Azure subscription so you can use APIs.</span></span> <span data-ttu-id="2fbfb-111">Här följer hello sätt som du kan använda API: er:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-111">Here are hello ways that you can use APIs:</span></span>

* <span data-ttu-id="2fbfb-112">Hanterade API: er</span><span class="sxs-lookup"><span data-stu-id="2fbfb-112">Managed APIs</span></span>
* <span data-ttu-id="2fbfb-113">Dina anpassade webb-API: er</span><span class="sxs-lookup"><span data-stu-id="2fbfb-113">Your custom Web APIs</span></span>

<span data-ttu-id="2fbfb-114">Varje sätt hanteras lite annorlunda eftersom deras hantering och värd modeller är olika.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-114">Each way is handled slightly differently because their management and hosting models are different.</span></span> <span data-ttu-id="2fbfb-115">En fördel med den här modellen är att du inte längre är begränsad tooresources som har distribuerats i din Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-115">One advantage of this model is you're no longer constrained tooresources that are deployed in your Azure resource group.</span></span> 

### <a name="managed-apis"></a><span data-ttu-id="2fbfb-116">Hanterade API: er</span><span class="sxs-lookup"><span data-stu-id="2fbfb-116">Managed APIs</span></span>

<span data-ttu-id="2fbfb-117">Microsoft hanterar vissa API: er för din räkning, till exempel Office 365, Salesforce, Twitter och FTP.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-117">Microsoft manages some APIs on your behalf, such as Office 365, Salesforce, Twitter, and FTP.</span></span> <span data-ttu-id="2fbfb-118">Du kan använda vissa hanterade API: er som-är, till exempel Bing översätta, medan andra kräver konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-118">You can use some managed APIs as-is, such as Bing Translate, while others require configuration.</span></span> <span data-ttu-id="2fbfb-119">Den här konfigurationen kallas en *anslutning*.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-119">This configuration is called a *connection*.</span></span>

<span data-ttu-id="2fbfb-120">När du använder Office 365, måste du skapa en anslutning som innehåller din Office 365-inloggning token.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-120">For example, when you use Office 365, you must create a connection that contains your Office 365 sign-in token.</span></span> <span data-ttu-id="2fbfb-121">Denna token lagras på ett säkert sätt och uppdateras så att din logikapp alltid kan anropa hello API för Office 365.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-121">This token is securely stored and refreshed so that your logic app can always call hello Office 365 API.</span></span> <span data-ttu-id="2fbfb-122">Om du vill tooconnect tooyour SQL- eller FTP-server, måste du skapa en anslutning med hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-122">Alternatively, if you want tooconnect tooyour SQL or FTP server, you must create a connection that has hello connection string.</span></span> 

<span data-ttu-id="2fbfb-123">I den här definitionen åtgärderna kallas `APIConnection`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-123">In this definition, these actions are called `APIConnection`.</span></span> <span data-ttu-id="2fbfb-124">Här är ett exempel på en anslutning som anropar Office 365 toosend ett e-postmeddelande:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-124">Here is an example of a connection that calls Office 365 toosend an email:</span></span>

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

<span data-ttu-id="2fbfb-125">Hej `host` objekt är del av indata som är unikt tooAPI anslutningar och innehåller dra delar: `api` och `connection`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-125">hello `host` object is portion of inputs that is unique tooAPI connections, and contains tow parts: `api` and `connection`.</span></span>

<span data-ttu-id="2fbfb-126">Hej `api` har hello runtime URL där som hanterad API finns.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-126">hello `api` has hello runtime URL of where that managed API is hosted.</span></span> <span data-ttu-id="2fbfb-127">Du kan se alla tillgängliga hello hanterade API: er genom att anropa `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-127">You can see all hello available managed APIs by calling `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span></span>

<span data-ttu-id="2fbfb-128">När du använder ett API hello API kanske eller kanske inte har någon *anslutningsparametrar* definieras.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-128">When you use an API, hello API might or might not have any *connection parameters* defined.</span></span> <span data-ttu-id="2fbfb-129">Om hello API inte några *anslutning* krävs.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-129">If hello API doesn't, no *connection* is required.</span></span> <span data-ttu-id="2fbfb-130">Om hello API matchar, måste du skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-130">If hello API does, you must create a connection.</span></span> <span data-ttu-id="2fbfb-131">hello skapade anslutningen har hello-namn som du väljer.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-131">hello created connection has hello name that you choose.</span></span> <span data-ttu-id="2fbfb-132">Sedan kan du referera hello namn i hello `connection` objekt i hello `host` objekt.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-132">You then reference hello name in hello `connection` object inside hello `host` object.</span></span> <span data-ttu-id="2fbfb-133">toocreate en anslutning i en resursgrupp, anropet:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-133">toocreate a connection in a resource group, call:</span></span>

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

<span data-ttu-id="2fbfb-134">Med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-134">With hello following body:</span></span>

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{hello name of hello storage account -- hello set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a><span data-ttu-id="2fbfb-135">Distribuera hanterade API: er i en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="2fbfb-135">Deploy managed APIs in an Azure Resource Manager template</span></span>

<span data-ttu-id="2fbfb-136">Du kan skapa en fullständig program i en Azure Resource Manager-mall så länge interaktiv inloggning krävs inte.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-136">You can create a full application in an Azure Resource Manager template as long as interactive sign-in isn't required.</span></span>
<span data-ttu-id="2fbfb-137">Om inloggning krävs, kan du ställa in allt med hello Azure Resource Manager-mall, men du fortfarande har toovisit hello portal tooauthorize hello anslutningar.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-137">If sign-in is required, you can set up everything with hello Azure Resource Manager template, but you still have toovisit hello portal tooauthorize hello connections.</span></span> 

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

<span data-ttu-id="2fbfb-138">Du kan se i det här exemplet att hello anslutningar är bara resurser som bor i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-138">You can see in this example that hello connections are just resources that live in your resource group.</span></span> <span data-ttu-id="2fbfb-139">De refererar till hello hanterade API: er tillgängliga tooyou i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-139">They reference hello managed APIs available tooyou in your subscription.</span></span>

### <a name="your-custom-web-apis"></a><span data-ttu-id="2fbfb-140">Dina anpassade webb-API: er</span><span class="sxs-lookup"><span data-stu-id="2fbfb-140">Your custom Web APIs</span></span>

<span data-ttu-id="2fbfb-141">Om du använder egna API: er, inte Microsoft-hanterad de använda inbyggda hello **HTTP** åtgärd toocall dem.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-141">If you use your own APIs, not Microsoft-managed ones, use hello built-in **HTTP** action toocall them.</span></span> <span data-ttu-id="2fbfb-142">För en perfekt upplevelse bör du exponera en Swagger-slutpunkt för din API.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-142">For an ideal experience, you should expose a Swagger endpoint for your API.</span></span> <span data-ttu-id="2fbfb-143">Den här slutpunkten kan hello logik App Designer toorender hello indata och utdata för din API.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-143">This endpoint enables hello Logic App Designer toorender hello inputs and outputs for your API.</span></span> <span data-ttu-id="2fbfb-144">Swagger, kan hello designer bara visa hello in- och utgångar som täckande JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-144">Without Swagger, hello designer can only show hello inputs and outputs as opaque JSON objects.</span></span>

<span data-ttu-id="2fbfb-145">Här är ett exempel som visar hello nya `metadata.apiDefinitionUrl` egenskapen:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-145">Here is an example showing hello new `metadata.apiDefinitionUrl` property:</span></span>

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

<span data-ttu-id="2fbfb-146">Om du är värd för webb-API på Azure App Service, visas automatiskt webb-API i hello lista över åtgärder som är tillgängliga i hello designer.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-146">If you host your Web API on Azure App Service, your Web API automatically appears in hello list of actions available in hello designer.</span></span> <span data-ttu-id="2fbfb-147">Om inte du har toopaste i hello URL direkt.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-147">If not, you have toopaste in hello URL directly.</span></span> <span data-ttu-id="2fbfb-148">Hej Swagger slutpunkten måste vara oautentiserad toobe kan användas i hello logik App Designer, men du kan skydda hello API sig själv med de metoder som har stöd för Swagger.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-148">hello Swagger endpoint must be unauthenticated toobe usable in hello Logic App Designer, although you can secure hello API itself with whatever methods that Swagger supports.</span></span>

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a><span data-ttu-id="2fbfb-149">Anropa distribuerade API-appar med 2015-08-01-preview</span><span class="sxs-lookup"><span data-stu-id="2fbfb-149">Call deployed API apps with 2015-08-01-preview</span></span>

<span data-ttu-id="2fbfb-150">Om du tidigare har distribuerat en API-App kan du anropa hello app med hello **HTTP** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-150">If you previously deployed an API App, you can call hello app with hello **HTTP** action.</span></span>

<span data-ttu-id="2fbfb-151">Om du använder Dropbox toolist filer, till exempel din **2014-12-01-preview** version schemadefinition kan ha något som liknar:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-151">For example, if you use Dropbox toolist files, your **2014-12-01-preview** schema version definition might have something like:</span></span>

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

<span data-ttu-id="2fbfb-152">Du kan skapa hello motsvarande HTTP-åtgärd som exempel, medan hello parametrar avsnitt i hello logik app definition förblir oförändrad:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-152">You can construct hello equivalent HTTP action like this example, while hello parameters section of hello Logic app definition remains unchanged:</span></span>

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

<span data-ttu-id="2fbfb-153">Gå igenom de här egenskaperna i taget:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-153">Walking through these properties one-by-one:</span></span>

| <span data-ttu-id="2fbfb-154">Åtgärdsegenskap</span><span class="sxs-lookup"><span data-stu-id="2fbfb-154">Action property</span></span> | <span data-ttu-id="2fbfb-155">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2fbfb-155">Description</span></span> |
| --- | --- |
| `type` |<span data-ttu-id="2fbfb-156">`Http`Istället för`APIapp`</span><span class="sxs-lookup"><span data-stu-id="2fbfb-156">`Http` instead of `APIapp`</span></span> |
| `metadata.apiDefinitionUrl` |<span data-ttu-id="2fbfb-157">toouse åtgärden i hello logik App Designer är hello metadata slutpunkt som konstrueras utifrån:`{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span><span class="sxs-lookup"><span data-stu-id="2fbfb-157">toouse this action in hello Logic App Designer, include hello metadata endpoint, which is constructed from: `{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span></span> |
| `inputs.uri` |<span data-ttu-id="2fbfb-158">Konstrueras från:`{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14`</span><span class="sxs-lookup"><span data-stu-id="2fbfb-158">Constructed from: `{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14`</span></span> |
| `inputs.method` |<span data-ttu-id="2fbfb-159">Alltid`POST`</span><span class="sxs-lookup"><span data-stu-id="2fbfb-159">Always `POST`</span></span> |
| `inputs.body` |<span data-ttu-id="2fbfb-160">Parametrar för identiska toohello API-App</span><span class="sxs-lookup"><span data-stu-id="2fbfb-160">Identical toohello API App parameters</span></span> |
| `inputs.authentication` |<span data-ttu-id="2fbfb-161">Identiska toohello API-App-autentisering</span><span class="sxs-lookup"><span data-stu-id="2fbfb-161">Identical toohello API App authentication</span></span> |

<span data-ttu-id="2fbfb-162">Den här metoden ska fungera för alla åtgärder i API-App.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-162">This approach should work for all API App actions.</span></span> <span data-ttu-id="2fbfb-163">Tänk dock på att dessa tidigare API Apps stöds inte längre.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-163">However, remember that these previous API Apps are no longer supported.</span></span> <span data-ttu-id="2fbfb-164">Därför bör du flytta tooone av hello två andra föregående alternativ, en hanterade API: et eller värd för ditt anpassade webb-API.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-164">So you should move tooone of hello two other previous options, a managed API or hosting your custom Web API.</span></span>

<a name="foreach"></a>
## <a name="renamed-repeat-tooforeach"></a><span data-ttu-id="2fbfb-165">Byta namn på Upprepa too'foreach'</span><span class="sxs-lookup"><span data-stu-id="2fbfb-165">Renamed 'repeat' too'foreach'</span></span>

<span data-ttu-id="2fbfb-166">Hello tidigare schemaversionen vi emot mycket kundfeedback som **Upprepa** var förvirrande och inte korrekt avbilda som **Upprepa** verkligen är en för varje slinga.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-166">For hello previous schema version, we received much customer feedback that **Repeat** was confusing and didn't properly capture that **Repeat** was really a for-each loop.</span></span> <span data-ttu-id="2fbfb-167">Därför har vi bytt namn `repeat` för`foreach`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-167">As a result, we have renamed `repeat` too`foreach`.</span></span> <span data-ttu-id="2fbfb-168">Till exempel tidigare du skulle kunna skriva:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-168">For example, previously you would write:</span></span>

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

<span data-ttu-id="2fbfb-169">Nu skulle du skriva:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-169">Now you would write:</span></span>

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

<span data-ttu-id="2fbfb-170">Hej funktionen `@repeatItem()` har använts tidigare tooreference hello aktuella objekt som hävdade över.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-170">hello function `@repeatItem()` was previously used tooreference hello current item being iterated over.</span></span> <span data-ttu-id="2fbfb-171">Den här funktionen är nu förenklad för`@item()`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-171">This function is now simplified too`@item()`.</span></span> 

### <a name="reference-outputs-from-foreach"></a><span data-ttu-id="2fbfb-172">Referens för utdata från 'foreach'</span><span class="sxs-lookup"><span data-stu-id="2fbfb-172">Reference outputs from 'foreach'</span></span>

<span data-ttu-id="2fbfb-173">För enkelhetens skull hello matar ut från `foreach` åtgärder som inte är omslutna i ett objekt som kallas `repeatItems`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-173">For simplification, hello outputs from `foreach` actions are not wrapped in an object called `repeatItems`.</span></span> <span data-ttu-id="2fbfb-174">Medan hello matar ut från hello tidigare `repeat` exempel var:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-174">While hello outputs from hello previous `repeat` example were:</span></span>

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

<span data-ttu-id="2fbfb-175">Nu är dessa utdata:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-175">Now these outputs are:</span></span>

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

<span data-ttu-id="2fbfb-176">Tidigare tooget toohello brödtext hello åtgärd när du refererar till dessa utdata:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-176">Previously, tooget toohello body of hello action when referencing these outputs:</span></span>

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

<span data-ttu-id="2fbfb-177">Nu kan du göra i stället:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-177">Now you can do instead:</span></span>

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

<span data-ttu-id="2fbfb-178">Med de här ändringarna hello funktioner `@repeatItem()`, `@repeatBody()`, och `@repeatOutputs()` tas bort.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-178">With these changes, hello functions `@repeatItem()`, `@repeatBody()`, and `@repeatOutputs()` are removed.</span></span>

<a name="http-listener"></a>
## <a name="native-http-listener"></a><span data-ttu-id="2fbfb-179">Inbyggda HTTP-lyssnare</span><span class="sxs-lookup"><span data-stu-id="2fbfb-179">Native HTTP listener</span></span>

<span data-ttu-id="2fbfb-180">hello funktioner är nu inbyggda i HTTP-lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-180">hello HTTP Listener capabilities are now built in.</span></span> <span data-ttu-id="2fbfb-181">Så behöver du inte längre toodeploy en http-lyssnare API-App.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-181">So you no longer need toodeploy an HTTP Listener API App.</span></span> <span data-ttu-id="2fbfb-182">Se [hello fullständig information om hur toomake din logik app endpoint callable här](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="2fbfb-182">See [hello full details for how toomake your Logic app endpoint callable here](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<span data-ttu-id="2fbfb-183">Med de här ändringarna vi tagit bort hello `@accessKeys()` funktion, vilket vi ersätts med hello `@listCallbackURL()` funktion för att hämta hello slutpunkt vid behov.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-183">With these changes, we removed hello `@accessKeys()` function, which we replaced with hello `@listCallbackURL()` function for getting hello endpoint when necessary.</span></span> <span data-ttu-id="2fbfb-184">Du måste också definiera minst en utlösare i din logikapp nu.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-184">Also, you must now define at least one trigger in your logic app.</span></span> <span data-ttu-id="2fbfb-185">Om du vill använda för`/run` Hej arbetsflödet, måste du ha något av dessa utlösare: `manual`, `apiConnectionWebhook`, eller `httpWebhook`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-185">If you want too`/run` hello workflow, you must have one of these triggers: `manual`, `apiConnectionWebhook`, or `httpWebhook`.</span></span>

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a><span data-ttu-id="2fbfb-186">Anropa underordnade arbetsflöden</span><span class="sxs-lookup"><span data-stu-id="2fbfb-186">Call child workflows</span></span>

<span data-ttu-id="2fbfb-187">Anropar underordnade arbetsflöden krävs tidigare, kommer toohello arbetsflöde kan få hello åtkomst-token och klistra in hello-token i hello logik app definition där du vill att toocall det underordnade arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-187">Previously, calling child workflows required going toohello workflow, getting hello access token, and pasting hello token in hello logic app definition where you want toocall that child workflow.</span></span> <span data-ttu-id="2fbfb-188">Med nya hello-schemat hello hello Logic Apps motorn genererar automatiskt en SAS vid körning för underordnade arbetsflödet så att du inte har för Klistra in alla hemligheter i hello definition.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-188">With hello new schema, hello Logic Apps engine automatically generates a SAS at runtime for hello child workflow so you don't have too paste any secrets into hello definition.</span></span> <span data-ttu-id="2fbfb-189">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="2fbfb-189">Here is an example:</span></span>

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

<span data-ttu-id="2fbfb-190">En andra förbättringar är vi ger hello underordnade arbetsflöden fullständig åtkomst toohello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-190">A second improvement is we are giving hello child workflows full access toohello incoming request.</span></span> <span data-ttu-id="2fbfb-191">Det innebär att du kan skicka parametrar i hello *frågor* avsnittet och i hello *huvuden* objektet och att du kan definiera fullständigt hello hela brödtext.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-191">That means that you can pass parameters in hello *queries* section and in hello *headers* object and that you can fully define hello entire body.</span></span>

<span data-ttu-id="2fbfb-192">Slutligen finns nödvändiga ändringar toohello underordnat arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-192">Finally, there are required changes toohello child workflow.</span></span> <span data-ttu-id="2fbfb-193">När du gick tidigare anropar ett underordnat arbetsflöde direkt, måste nu du definiera en slutpunkt för utlösaren i hello arbetsflöde för hello överordnade toocall.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-193">While you could previously call a child workflow directly, now you must define a trigger endpoint in hello workflow for hello parent toocall.</span></span> <span data-ttu-id="2fbfb-194">I allmänhet kan du lägga till en utlösare som har `manual` skriver och använder sedan den utlösaren i hello överordnade definition.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-194">Generally, you would add a trigger that has `manual` type, and then use that trigger in hello parent definition.</span></span> <span data-ttu-id="2fbfb-195">Obs hello `host` egenskapen specifikt har en `triggerName` eftersom du måste alltid ange vars utlösare du anropar.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-195">Note hello `host` property specifically has a `triggerName` because you must always specify which trigger you are invoking.</span></span>

## <a name="other-changes"></a><span data-ttu-id="2fbfb-196">Andra ändringar</span><span class="sxs-lookup"><span data-stu-id="2fbfb-196">Other changes</span></span>

### <a name="new-queries-property"></a><span data-ttu-id="2fbfb-197">Ny 'frågor-egenskap</span><span class="sxs-lookup"><span data-stu-id="2fbfb-197">New 'queries' property</span></span>

<span data-ttu-id="2fbfb-198">Alla åtgärdstyper stöder nu en ny inmatning kallas `queries`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-198">All action types now support a new input called `queries`.</span></span> <span data-ttu-id="2fbfb-199">Den här indata kan vara en strukturerad objekt, snarare än du har tooassemble hello sträng manuellt.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-199">This input can be a structured object, rather than you having tooassemble hello string by hand.</span></span>

### <a name="renamed-parse-function-toojson"></a><span data-ttu-id="2fbfb-200">Byta namn på 'parse()' funktionen too'json()'</span><span class="sxs-lookup"><span data-stu-id="2fbfb-200">Renamed 'parse()' function too'json()'</span></span>

<span data-ttu-id="2fbfb-201">Vi lägger till mer innehåll av typen snart, så vi har fått nytt namn hello `parse()` fungerar för`json()`.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-201">We are adding more content types soon, so we renamed hello `parse()` function too`json()`.</span></span>

## <a name="coming-soon-enterprise-integration-apis"></a><span data-ttu-id="2fbfb-202">Kommer snart: Enterprise Integration-API: er</span><span class="sxs-lookup"><span data-stu-id="2fbfb-202">Coming soon: Enterprise Integration APIs</span></span>

<span data-ttu-id="2fbfb-203">Vi har inte hanterade-versioner än hello Enterprise Integration-API: er, som AS2.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-203">We don't have managed versions yet of hello Enterprise Integration APIs, like AS2.</span></span> <span data-ttu-id="2fbfb-204">Under tiden kan du använda din befintliga distribuerade BizTalk-APIs via hello HTTP-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2fbfb-204">Meanwhile, you can use your existing deployed BizTalk APIs through hello HTTP action.</span></span> <span data-ttu-id="2fbfb-205">Mer information, se ”med redan distribuerade API apps” i hello [översikt över integrering](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span><span class="sxs-lookup"><span data-stu-id="2fbfb-205">For details, see "Using your already deployed API apps" in hello [integration roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span></span> 
