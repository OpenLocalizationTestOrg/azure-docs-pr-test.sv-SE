---
title: "aaaConfigure prestanda trafikroutningsmetoden med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur tooconfigure Traffic Manager tooroute trafik toohello slutpunkt med lägsta svarstid"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: d0ccd4c9de411844c6f36068859265244f4aa52b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-performance-traffic-routing-method"></a><span data-ttu-id="f810e-103">Konfigurera routningsmetoden för hello prestanda-trafik</span><span class="sxs-lookup"><span data-stu-id="f810e-103">Configure hello performance traffic routing method</span></span>

<span data-ttu-id="f810e-104">hello routningsmetoden för prestanda-trafik kan toodirect trafik toohello slutpunkt med hello lägsta svarstid från hello klientnätverk.</span><span class="sxs-lookup"><span data-stu-id="f810e-104">hello Performance traffic routing method allows you toodirect traffic toohello endpoint with hello lowest latency from hello client's network.</span></span> <span data-ttu-id="f810e-105">Hello datacenter med hello lägsta fördröjningen är vanligtvis hello närmaste i geografiska avstånd.</span><span class="sxs-lookup"><span data-stu-id="f810e-105">Typically, hello datacenter with hello lowest latency is hello closest in geographic distance.</span></span> <span data-ttu-id="f810e-106">Den här trafikroutningsmetod kan inte kontot för realtid ändringarna i nätverkskonfigurationen eller läsa in.</span><span class="sxs-lookup"><span data-stu-id="f810e-106">This traffic routing method cannot account for real-time changes in network configuration or load.</span></span>

##  <a name="tooconfigure-performance-routing-method"></a><span data-ttu-id="f810e-107">tooconfigure routningsmetoden för prestanda</span><span class="sxs-lookup"><span data-stu-id="f810e-107">tooconfigure performance routing method</span></span>

1. <span data-ttu-id="f810e-108">Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f810e-108">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="f810e-109">Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="f810e-109">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="f810e-110">I sökfältet hello-portalen, söka efter hello **Traffic Manager-profiler** och klicka sedan på hello-profilnamn som du vill tooconfigure hello routningsmetod för.</span><span class="sxs-lookup"><span data-stu-id="f810e-110">In hello portal’s search bar, search for hello **Traffic Manager profiles** and then click hello profile name that you want tooconfigure hello routing method for.</span></span>
3. <span data-ttu-id="f810e-111">I hello **trafikhanterarprofil** bladet, kontrollera att både hello molntjänster och webbplatser som du vill tooinclude i konfigurationen finns.</span><span class="sxs-lookup"><span data-stu-id="f810e-111">In hello **Traffic Manager profile** blade, verify that both hello cloud services and websites that you want tooinclude in your configuration are present.</span></span>
4. <span data-ttu-id="f810e-112">I hello **inställningar** klickar du på **Configuration**, och i hello **Configuration** bladet slutförts enligt följande:</span><span class="sxs-lookup"><span data-stu-id="f810e-112">In hello **Settings** section, click **Configuration**, and in hello **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="f810e-113">För **trafik routning inställningar**, för **routningsmetod** Välj **prestanda**.</span><span class="sxs-lookup"><span data-stu-id="f810e-113">For **traffic routing method settings**, for **Routing method** select **Performance**.</span></span>
    2. <span data-ttu-id="f810e-114">Ange hello **övervakaren slutpunktsinställningar** identiska för alla alla slutpunkter i den här profilen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f810e-114">Set hello **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="f810e-115">Välj lämplig hello **protokollet**, och ange hello **Port** nummer.</span><span class="sxs-lookup"><span data-stu-id="f810e-115">Select hello appropriate **Protocol**, and specify hello **Port** number.</span></span> 
        2. <span data-ttu-id="f810e-116">För **sökväg** skriver ett snedstreck  */* .</span><span class="sxs-lookup"><span data-stu-id="f810e-116">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="f810e-117">toomonitor slutpunkter som du måste ange en sökväg och filnamn.</span><span class="sxs-lookup"><span data-stu-id="f810e-117">toomonitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="f810e-118">Ett snedstreck ”/” är ett giltigt för hello relativ sökväg och innebär att hello-filen är i hello rotkatalog (standard).</span><span class="sxs-lookup"><span data-stu-id="f810e-118">A forward slash "/" is a valid entry for hello relative path and implies that hello file is in hello root directory (default).</span></span>
        3. <span data-ttu-id="f810e-119">Hello överst på hello-sidan, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="f810e-119">At hello top of hello page, click **Save**.</span></span>
5.  <span data-ttu-id="f810e-120">Testa hello ändringar i konfigurationen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f810e-120">Test hello changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="f810e-121">Sök efter hello Traffic Manager-profilnamn hello portal sökfältet, och klicka på hello Traffic Manager-profilen i hello resultat som hello visas.</span><span class="sxs-lookup"><span data-stu-id="f810e-121">In hello portal’s search bar, search for hello Traffic Manager profile name and click hello Traffic Manager profile in hello results that hello displayed.</span></span>
    2.  <span data-ttu-id="f810e-122">I hello **Traffic Manager** profilen bladet, klickar du på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="f810e-122">In hello **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="f810e-123">Hej **trafikhanterarprofil** bladet visar hello DNS-namnet för nyskapade Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="f810e-123">hello **Traffic Manager profile** blade displays hello DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="f810e-124">Detta kan användas av alla klienter (till exempel genom att gå tooit med en webbläsare) tooget dirigeras toohello rätt slutpunkten enligt hello routning.</span><span class="sxs-lookup"><span data-stu-id="f810e-124">This can be used by any clients (for example, by navigating tooit using a web browser) tooget routed toohello right endpoint as determined by hello routing type.</span></span> <span data-ttu-id="f810e-125">I det här fallet är alla begäranden routade toohello slutpunkt med hello lägsta fördröjningen från hello klientnätverk.</span><span class="sxs-lookup"><span data-stu-id="f810e-125">In this case all requests are routed toohello endpoint with hello lowest latency from hello client's network.</span></span>
6. <span data-ttu-id="f810e-126">Redigera hello DNS-post på din auktoritära DNS-server toopoint företagets namn toohello Traffic Manager-domän domännamn när Traffic Manager-profilen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f810e-126">Once your Traffic Manager profile is working, edit hello DNS record on your authoritative DNS server toopoint your company domain name toohello Traffic Manager domain name.</span></span>

![Konfigurera prestanda trafikroutningsmetod med Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="f810e-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f810e-128">Next steps</span></span>

- <span data-ttu-id="f810e-129">Lär dig mer om [viktas trafikroutningsmetod](traffic-manager-configure-weighted-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="f810e-129">Learn about [weighted traffic routing method](traffic-manager-configure-weighted-routing-method.md).</span></span>
- <span data-ttu-id="f810e-130">Lär dig mer om [prioritet routningsmetoden](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="f810e-130">Learn about [priority routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="f810e-131">Lär dig mer om [geografiska routningsmetoden](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="f810e-131">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="f810e-132">Lär dig hur för[testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="f810e-132">Learn how too[test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png