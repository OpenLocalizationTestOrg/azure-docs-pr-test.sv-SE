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
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Schema-uppdateringar för Logic Apps i Azure - preview 1 augusti 2015

Den här nya schemat och API-version för Logikappar i Azure innehåller viktiga förbättringar som gör logikappar mer tillförlitlig och enklare toouse:

*   Hej **APIApp** åtgärdstypen är uppdaterade tooa nya [ **APIConnection** ](#api-connections) åtgärdstyp.
*   **Upprepa** har bytt namn för[**Foreach**](#foreach).
*   Hej [ **HTTP-lyssnaren** API-App](#http-listener) krävs inte längre.
*   Anropar underordnade arbetsflöden använder en [nya schemat](#child-workflows).

<a name="api-connections"></a>
## <a name="move-tooapi-connections"></a>Flytta tooAPI anslutningar

hello största förändringen är att du inte längre har toodeploy API Apps i Azure-prenumerationen så att du kan använda API: er. Här följer hello sätt som du kan använda API: er:

* Hanterade API: er
* Dina anpassade webb-API: er

Varje sätt hanteras lite annorlunda eftersom deras hantering och värd modeller är olika. En fördel med den här modellen är att du inte längre är begränsad tooresources som har distribuerats i din Azure-resursgrupp. 

### <a name="managed-apis"></a>Hanterade API: er

Microsoft hanterar vissa API: er för din räkning, till exempel Office 365, Salesforce, Twitter och FTP. Du kan använda vissa hanterade API: er som-är, till exempel Bing översätta, medan andra kräver konfiguration. Den här konfigurationen kallas en *anslutning*.

När du använder Office 365, måste du skapa en anslutning som innehåller din Office 365-inloggning token. Denna token lagras på ett säkert sätt och uppdateras så att din logikapp alltid kan anropa hello API för Office 365. Om du vill tooconnect tooyour SQL- eller FTP-server, måste du skapa en anslutning med hello anslutningssträngen. 

I den här definitionen åtgärderna kallas `APIConnection`. Här är ett exempel på en anslutning som anropar Office 365 toosend ett e-postmeddelande:

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

Hej `host` objekt är del av indata som är unikt tooAPI anslutningar och innehåller dra delar: `api` och `connection`.

Hej `api` har hello runtime URL där som hanterad API finns. Du kan se alla tillgängliga hello hanterade API: er genom att anropa `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

När du använder ett API hello API kanske eller kanske inte har någon *anslutningsparametrar* definieras. Om hello API inte några *anslutning* krävs. Om hello API matchar, måste du skapa en anslutning. hello skapade anslutningen har hello-namn som du väljer. Sedan kan du referera hello namn i hello `connection` objekt i hello `host` objekt. toocreate en anslutning i en resursgrupp, anropet:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Med hello följande text:

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

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Distribuera hanterade API: er i en Azure Resource Manager-mall

Du kan skapa en fullständig program i en Azure Resource Manager-mall så länge interaktiv inloggning krävs inte.
Om inloggning krävs, kan du ställa in allt med hello Azure Resource Manager-mall, men du fortfarande har toovisit hello portal tooauthorize hello anslutningar. 

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

Du kan se i det här exemplet att hello anslutningar är bara resurser som bor i resursgruppen. De refererar till hello hanterade API: er tillgängliga tooyou i din prenumeration.

### <a name="your-custom-web-apis"></a>Dina anpassade webb-API: er

Om du använder egna API: er, inte Microsoft-hanterad de använda inbyggda hello **HTTP** åtgärd toocall dem. För en perfekt upplevelse bör du exponera en Swagger-slutpunkt för din API. Den här slutpunkten kan hello logik App Designer toorender hello indata och utdata för din API. Swagger, kan hello designer bara visa hello in- och utgångar som täckande JSON-objekt.

Här är ett exempel som visar hello nya `metadata.apiDefinitionUrl` egenskapen:

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

Om du är värd för webb-API på Azure App Service, visas automatiskt webb-API i hello lista över åtgärder som är tillgängliga i hello designer. Om inte du har toopaste i hello URL direkt. Hej Swagger slutpunkten måste vara oautentiserad toobe kan användas i hello logik App Designer, men du kan skydda hello API sig själv med de metoder som har stöd för Swagger.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Anropa distribuerade API-appar med 2015-08-01-preview

Om du tidigare har distribuerat en API-App kan du anropa hello app med hello **HTTP** åtgärd.

Om du använder Dropbox toolist filer, till exempel din **2014-12-01-preview** version schemadefinition kan ha något som liknar:

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

Du kan skapa hello motsvarande HTTP-åtgärd som exempel, medan hello parametrar avsnitt i hello logik app definition förblir oförändrad:

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

Gå igenom de här egenskaperna i taget:

| Åtgärdsegenskap | Beskrivning |
| --- | --- |
| `type` |`Http`Istället för`APIapp` |
| `metadata.apiDefinitionUrl` |toouse åtgärden i hello logik App Designer är hello metadata slutpunkt som konstrueras utifrån:`{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Konstrueras från:`{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Alltid`POST` |
| `inputs.body` |Parametrar för identiska toohello API-App |
| `inputs.authentication` |Identiska toohello API-App-autentisering |

Den här metoden ska fungera för alla åtgärder i API-App. Tänk dock på att dessa tidigare API Apps stöds inte längre. Därför bör du flytta tooone av hello två andra föregående alternativ, en hanterade API: et eller värd för ditt anpassade webb-API.

<a name="foreach"></a>
## <a name="renamed-repeat-tooforeach"></a>Byta namn på Upprepa too'foreach'

Hello tidigare schemaversionen vi emot mycket kundfeedback som **Upprepa** var förvirrande och inte korrekt avbilda som **Upprepa** verkligen är en för varje slinga. Därför har vi bytt namn `repeat` för`foreach`. Till exempel tidigare du skulle kunna skriva:

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

Nu skulle du skriva:

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

Hej funktionen `@repeatItem()` har använts tidigare tooreference hello aktuella objekt som hävdade över. Den här funktionen är nu förenklad för`@item()`. 

### <a name="reference-outputs-from-foreach"></a>Referens för utdata från 'foreach'

För enkelhetens skull hello matar ut från `foreach` åtgärder som inte är omslutna i ett objekt som kallas `repeatItems`. Medan hello matar ut från hello tidigare `repeat` exempel var:

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

Nu är dessa utdata:

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

Tidigare tooget toohello brödtext hello åtgärd när du refererar till dessa utdata:

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

Nu kan du göra i stället:

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

Med de här ändringarna hello funktioner `@repeatItem()`, `@repeatBody()`, och `@repeatOutputs()` tas bort.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Inbyggda HTTP-lyssnare

hello funktioner är nu inbyggda i HTTP-lyssnare. Så behöver du inte längre toodeploy en http-lyssnare API-App. Se [hello fullständig information om hur toomake din logik app endpoint callable här](../logic-apps/logic-apps-http-endpoint.md). 

Med de här ändringarna vi tagit bort hello `@accessKeys()` funktion, vilket vi ersätts med hello `@listCallbackURL()` funktion för att hämta hello slutpunkt vid behov. Du måste också definiera minst en utlösare i din logikapp nu. Om du vill använda för`/run` Hej arbetsflödet, måste du ha något av dessa utlösare: `manual`, `apiConnectionWebhook`, eller `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Anropa underordnade arbetsflöden

Anropar underordnade arbetsflöden krävs tidigare, kommer toohello arbetsflöde kan få hello åtkomst-token och klistra in hello-token i hello logik app definition där du vill att toocall det underordnade arbetsflödet. Med nya hello-schemat hello hello Logic Apps motorn genererar automatiskt en SAS vid körning för underordnade arbetsflödet så att du inte har för Klistra in alla hemligheter i hello definition. Här är ett exempel:

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

En andra förbättringar är vi ger hello underordnade arbetsflöden fullständig åtkomst toohello inkommande begäran. Det innebär att du kan skicka parametrar i hello *frågor* avsnittet och i hello *huvuden* objektet och att du kan definiera fullständigt hello hela brödtext.

Slutligen finns nödvändiga ändringar toohello underordnat arbetsflöde. När du gick tidigare anropar ett underordnat arbetsflöde direkt, måste nu du definiera en slutpunkt för utlösaren i hello arbetsflöde för hello överordnade toocall. I allmänhet kan du lägga till en utlösare som har `manual` skriver och använder sedan den utlösaren i hello överordnade definition. Obs hello `host` egenskapen specifikt har en `triggerName` eftersom du måste alltid ange vars utlösare du anropar.

## <a name="other-changes"></a>Andra ändringar

### <a name="new-queries-property"></a>Ny 'frågor-egenskap

Alla åtgärdstyper stöder nu en ny inmatning kallas `queries`. Den här indata kan vara en strukturerad objekt, snarare än du har tooassemble hello sträng manuellt.

### <a name="renamed-parse-function-toojson"></a>Byta namn på 'parse()' funktionen too'json()'

Vi lägger till mer innehåll av typen snart, så vi har fått nytt namn hello `parse()` fungerar för`json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Kommer snart: Enterprise Integration-API: er

Vi har inte hanterade-versioner än hello Enterprise Integration-API: er, som AS2. Under tiden kan du använda din befintliga distribuerade BizTalk-APIs via hello HTTP-åtgärd. Mer information, se ”med redan distribuerade API apps” i hello [översikt över integrering](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
