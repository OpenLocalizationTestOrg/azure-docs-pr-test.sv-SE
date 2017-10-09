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
# <a name="how-toouse-hello-api-inspector-tootrace-calls-in-azure-api-management"></a><span data-ttu-id="a8c62-103">Hur toouse hello API Inspector tootrace anropar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a8c62-103">How toouse hello API Inspector tootrace calls in Azure API Management</span></span>
<span data-ttu-id="a8c62-104">API Management tillhandahåller ett API-Inspector verktyget toohelp dig med felsökning och felsökning av dina API: er.</span><span class="sxs-lookup"><span data-stu-id="a8c62-104">API Management provides an API Inspector tool toohelp you with debugging and troubleshooting your APIs.</span></span> <span data-ttu-id="a8c62-105">hello API Inspector kan användas via programmering och kan också användas direkt från hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8c62-105">hello API Inspector can be used programmatically and can also be used directly from hello developer portal.</span></span> 

<span data-ttu-id="a8c62-106">Dessutom tootracing operations API Inspector spårar också [principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx) utvärderingar.</span><span class="sxs-lookup"><span data-stu-id="a8c62-106">In addition tootracing operations, API Inspector also traces [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx) evaluations.</span></span> <span data-ttu-id="a8c62-107">En demonstration finns [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too21:00.</span><span class="sxs-lookup"><span data-stu-id="a8c62-107">For a demonstration, see [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too21:00.</span></span>

<span data-ttu-id="a8c62-108">Den här guiden innehåller en beskrivning av med hjälp av API-Inspector.</span><span class="sxs-lookup"><span data-stu-id="a8c62-108">This guide provides a walk-through of using API Inspector.</span></span>

> [!NOTE]
> <span data-ttu-id="a8c62-109">API-Inspector spårningar endast genereras och tillgängliga för begäranden som innehåller prenumeration nycklar som hör toohello [administratör](api-management-howto-create-groups.md) konto.</span><span class="sxs-lookup"><span data-stu-id="a8c62-109">API Inspector traces are only generated and made available for requests containing subscription keys that belong toohello [administrator](api-management-howto-create-groups.md) account.</span></span>
> 
> 

## <span data-ttu-id="a8c62-110"><a name="trace-call"></a> Använd API Inspector tootrace ett anrop</span><span class="sxs-lookup"><span data-stu-id="a8c62-110"><a name="trace-call"> </a> Use API Inspector tootrace a call</span></span>
<span data-ttu-id="a8c62-111">toouse API Inspector lägga till en **ocp-apim-spårning: true** begära huvud tooyour funktionsanrop, och sedan ladda ned och inspektera hello spårningen med hello-URL som anges av hello **ocp-apim-trace-plats** Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="a8c62-111">toouse API Inspector, add an **ocp-apim-trace: true** request header tooyour operation call, and then download and inspect hello trace using hello URL indicated by hello **ocp-apim-trace-location** response header.</span></span> <span data-ttu-id="a8c62-112">Detta kan göras genom att programmera och kan även utföras direkt från hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8c62-112">This can be done programatically, and can also be done directly from hello developer portal.</span></span>

<span data-ttu-id="a8c62-113">Den här kursen visar hur toouse hello API Inspector tootrace åtgärder med hjälp av hello grundläggande Kalkylatorn API som konfigurerats i hello [hantera din första API](api-management-get-started.md) komma igång-självstudiekurs.</span><span class="sxs-lookup"><span data-stu-id="a8c62-113">This tutorial shows how toouse hello API Inspector tootrace operations using hello Basic Calculator API that is configured in hello [Manage your first API](api-management-get-started.md) getting started tutorial.</span></span> <span data-ttu-id="a8c62-114">Om du inte har slutfört den här självstudiekursen tar bara några ögonblick tooimport hello grundläggande Kalkylatorn API eller du kan använda en annan API du väljer, till exempel hello Echo-API.</span><span class="sxs-lookup"><span data-stu-id="a8c62-114">If you haven't completed that tutorial it only takes a few moments tooimport hello Basic Calculator API, or you can use another API of your choosing such as hello Echo API.</span></span> <span data-ttu-id="a8c62-115">Varje instans för API Management-tjänsten har redan konfigurerats med en Echo-API som kan använda tooexperiment med och lär dig mer om API-hantering.</span><span class="sxs-lookup"><span data-stu-id="a8c62-115">Each API Management service instance comes pre-configured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="a8c62-116">hello Echo API returnerar tillbaka oavsett indata skickas tooit.</span><span class="sxs-lookup"><span data-stu-id="a8c62-116">hello Echo API returns back whatever input is sent tooit.</span></span> <span data-ttu-id="a8c62-117">toouse, du kan anropa någon HTTP-verb och hello returvärdet kommer bara att du har skickat.</span><span class="sxs-lookup"><span data-stu-id="a8c62-117">toouse it, you can invoke any HTTP verb, and hello return value will simply be what you sent.</span></span> 

<span data-ttu-id="a8c62-118">tooget har startats klickar du på **utvecklarportalen** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8c62-118">tooget started, click **Developer portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="a8c62-119">Åtgärder kan anropas direkt från hello developer-portalen som ger ett bekvämt sätt tooview och testa hello driften av ett API.</span><span class="sxs-lookup"><span data-stu-id="a8c62-119">Operations can be called directly from hello developer portal which provides a convenient way tooview and test hello operations of an API.</span></span>

> <span data-ttu-id="a8c62-120">Om du inte har skapat en instans för API Management-tjänsten, se [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="a8c62-120">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

![API Management developer-portalen][api-management-developer-portal-menu]

<span data-ttu-id="a8c62-122">Klicka på **API: er** hello översta menyn och klicka sedan på **grundläggande Kalkylatorn**.</span><span class="sxs-lookup"><span data-stu-id="a8c62-122">Click **APIs** from hello top menu, and then click **Basic Calculator**.</span></span>

![Echo API][api-management-api]

<span data-ttu-id="a8c62-124">Klicka på **prova** tootry hello **lägga till två heltal** igen.</span><span class="sxs-lookup"><span data-stu-id="a8c62-124">Click **Try it** tootry hello **Add two integers** operation.</span></span>

![Prova][api-management-open-console]

<span data-ttu-id="a8c62-126">Förhindra att hello Standardparametervärden och välj hello prenumeration nyckel för hello produkt som du vill använda toouse hello **prenumeration nyckeln** listrutan.</span><span class="sxs-lookup"><span data-stu-id="a8c62-126">Keep hello default parameter values, and select hello subscription key for hello product you want toouse from hello **subscription-key** drop-down.</span></span>

<span data-ttu-id="a8c62-127">Som standard i hello developer portal hello **Ocp-Apim-Trace** huvud har redan angetts för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="a8c62-127">By default in hello developer portal hello **Ocp-Apim-Trace** header is already set too**true**.</span></span> <span data-ttu-id="a8c62-128">Det här sidhuvudet konfigurerar huruvida en spårning genereras.</span><span class="sxs-lookup"><span data-stu-id="a8c62-128">This header configures whether or not a trace is generated.</span></span>

![Skicka][api-management-http-get]

<span data-ttu-id="a8c62-130">Klicka på **skicka** tooinvoke hello igen.</span><span class="sxs-lookup"><span data-stu-id="a8c62-130">Click **Send** tooinvoke hello operation.</span></span>

![Skicka][api-management-send-results]

<span data-ttu-id="a8c62-132">Hello svar huvuden blir en **ocp-apim-trace-plats** med ett värde liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a8c62-132">In hello response headers will be an **ocp-apim-trace-location** with a value similar toohello following example.</span></span>

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

<span data-ttu-id="a8c62-133">hello spårning kan hämtas från hello angetts plats och granskas som visas i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a8c62-133">hello trace can be downloaded from hello specified location and reviewed as demonstrated in hello next step.</span></span> <span data-ttu-id="a8c62-134">Observera att endast hello senaste 100 loggposter lagras och loggfilernas placering återanvänds i rotation.</span><span class="sxs-lookup"><span data-stu-id="a8c62-134">Note that only hello last 100 log entries are stored and log locations are reused in rotation.</span></span> <span data-ttu-id="a8c62-135">Om fler än 100 anrop med aktiverad spårning slutligen börjar du skriva över hello första spårningar på plats.</span><span class="sxs-lookup"><span data-stu-id="a8c62-135">So if you make more than 100 calls with tracing enabled you will eventually start overwriting hello first traces in place.</span></span>

## <span data-ttu-id="a8c62-136"><a name="inspect-trace"></a>Inspektera hello spårning</span><span class="sxs-lookup"><span data-stu-id="a8c62-136"><a name="inspect-trace"> </a>Inspect hello trace</span></span>
<span data-ttu-id="a8c62-137">tooreview hello värden i hello spårning hämta hello spårningsfilen från hello **ocp-apim-trace-plats** URL.</span><span class="sxs-lookup"><span data-stu-id="a8c62-137">tooreview hello values in hello trace, download hello trace file from hello **ocp-apim-trace-location** URL.</span></span> <span data-ttu-id="a8c62-138">Det är en textfil i JSON-format och innehåller poster liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a8c62-138">It is a text file in JSON format, and contains entries similar toohello following example.</span></span>

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

## <span data-ttu-id="a8c62-139"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8c62-139"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="a8c62-140">Titta på en demonstration av spårning princip uttryck i [moln omfattar avsnitt 177: mer API Management-funktioner](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span><span class="sxs-lookup"><span data-stu-id="a8c62-140">Watch a demo of tracing policy expressions in [Cloud Cover Episode 177: More API Management Features](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/).</span></span> <span data-ttu-id="a8c62-141">Spola framåt too21:00 toosee hello demo.</span><span class="sxs-lookup"><span data-stu-id="a8c62-141">Fast-forward too21:00 toosee hello demo.</span></span>

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




