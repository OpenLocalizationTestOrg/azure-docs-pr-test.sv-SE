---
title: aaaTrace anrop med API-Inspector - Azure API Management | Microsoft Docs
description: "Lär dig hur tootrace anrop med hello API Inspector i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: b0c401caa8da1b789f6cfe5edf97a5f118d78f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-api-inspector-tootrace-calls-in-azure-api-management"></a>Hur toouse hello API Inspector tootrace anropar i Azure API Management
API Management tillhandahåller ett API-Inspector verktyget toohelp dig med felsökning och felsökning av dina API: er. hello API Inspector kan användas via programmering och kan också användas direkt från hello developer-portalen. 

Dessutom tootracing operations API Inspector spårar också [principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx) utvärderingar. En demonstration finns [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too21:00.

Den här guiden innehåller en beskrivning av med hjälp av API-Inspector.

> [!NOTE]
> API-Inspector spårningar endast genereras och tillgängliga för begäranden som innehåller prenumeration nycklar som hör toohello [administratör](api-management-howto-create-groups.md) konto.
> 
> 

## <a name="trace-call"></a> Använd API Inspector tootrace ett anrop
toouse API Inspector lägga till en **ocp-apim-spårning: true** begära huvud tooyour funktionsanrop, och sedan ladda ned och inspektera hello spårningen med hello-URL som anges av hello **ocp-apim-trace-plats** Svarsrubrik. Detta kan göras genom att programmera och kan även utföras direkt från hello developer-portalen.

Den här kursen visar hur toouse hello API Inspector tootrace åtgärder med hjälp av hello grundläggande Kalkylatorn API som konfigurerats i hello [hantera din första API](api-management-get-started.md) komma igång-självstudiekurs. Om du inte har slutfört den här självstudiekursen tar bara några ögonblick tooimport hello grundläggande Kalkylatorn API eller du kan använda en annan API du väljer, till exempel hello Echo-API. Varje instans för API Management-tjänsten har redan konfigurerats med en Echo-API som kan använda tooexperiment med och lär dig mer om API-hantering. hello Echo API returnerar tillbaka oavsett indata skickas tooit. toouse, du kan anropa någon HTTP-verb och hello returvärdet kommer bara att du har skickat. 

tooget har startats klickar du på **utvecklarportalen** i hello Azure-portalen för API Management-tjänsten. Åtgärder kan anropas direkt från hello developer-portalen som ger ett bekvämt sätt tooview och testa hello driften av ett API.

> Om du inte har skapat en instans för API Management-tjänsten, se [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

![API Management developer-portalen][api-management-developer-portal-menu]

Klicka på **API: er** hello översta menyn och klicka sedan på **grundläggande Kalkylatorn**.

![Echo API][api-management-api]

Klicka på **prova** tootry hello **lägga till två heltal** igen.

![Prova][api-management-open-console]

Förhindra att hello Standardparametervärden och välj hello prenumeration nyckel för hello produkt som du vill använda toouse hello **prenumeration nyckeln** listrutan.

Som standard i hello developer portal hello **Ocp-Apim-Trace** huvud har redan angetts för**SANT**. Det här sidhuvudet konfigurerar huruvida en spårning genereras.

![Skicka][api-management-http-get]

Klicka på **skicka** tooinvoke hello igen.

![Skicka][api-management-send-results]

Hello svar huvuden blir en **ocp-apim-trace-plats** med ett värde liknande toohello följande exempel.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

hello spårning kan hämtas från hello angetts plats och granskas som visas i hello nästa steg. Observera att endast hello senaste 100 loggposter lagras och loggfilernas placering återanvänds i rotation. Om fler än 100 anrop med aktiverad spårning slutligen börjar du skriva över hello första spårningar på plats.

## <a name="inspect-trace"></a>Inspektera hello spårning
tooreview hello värden i hello spårning hämta hello spårningsfilen från hello **ocp-apim-trace-plats** URL. Det är en textfil i JSON-format och innehåller poster liknande toohello följande exempel.

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded toohello backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent toohello caller. Starting toostream hello response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming toohello caller is complete."
                }
            }
        ]
    }
}
```

## <a name="next-steps"> </a>Nästa steg
* Titta på en demonstration av spårning princip uttryck i [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Spola framåt too21:00 toosee hello demo.

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector tootrace a call]: #trace-call
[Inspect hello trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




