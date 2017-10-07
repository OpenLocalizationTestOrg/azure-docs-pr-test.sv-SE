---
title: "aaaConfigure viktad resursallokering trafikroutningsmetoden med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur tooload balansera trafik med hjälp av en resursallokering metod i Traffic Manager"
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
ms.openlocfilehash: 7e2866ead0b2b653845435dd420a763c5e175f4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-weighted-traffic-routing-method-in-traffic-manager"></a><span data-ttu-id="446b5-103">Konfigurera hello viktas trafikroutningsmetod i Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="446b5-103">Configure hello weighted traffic routing method in Traffic Manager</span></span>

<span data-ttu-id="446b5-104">En gemensam trafik routning metoden mönstret är tooprovide en uppsättning identiska slutpunkter som omfattar molntjänster och webbplatser och skicka trafik tooeach resursallokering överskrids.</span><span class="sxs-lookup"><span data-stu-id="446b5-104">A common traffic routing method pattern is tooprovide a set of identical endpoints, which include cloud services and websites, and send traffic tooeach in a round-robin fashion.</span></span> <span data-ttu-id="446b5-105">hello följande steg beskriver hur tooconfigure detta typ av trafikroutningsmetoden.</span><span class="sxs-lookup"><span data-stu-id="446b5-105">hello following steps outline how tooconfigure this type of traffic routing method.</span></span>

> [!NOTE]
> <span data-ttu-id="446b5-106">Azure webbplatser tillhandahåller redan resursallokering för webbplatser inom ett datacenter (även kallat en region) för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="446b5-106">Azure Websites already provide round-robin load balancing functionality for websites within a datacenter (also known as a region).</span></span> <span data-ttu-id="446b5-107">Traffic Manager kan du toospecify resursallokering trafikroutningsmetod för webbplatser i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="446b5-107">Traffic Manager allows you toospecify round-robin traffic routing method for websites in different datacenters.</span></span>

## <a name="tooconfigure-hello-weighted-traffic-routing-method"></a><span data-ttu-id="446b5-108">tooconfigure hello viktas trafikroutningsmetod</span><span class="sxs-lookup"><span data-stu-id="446b5-108">tooconfigure hello weighted traffic routing method</span></span>

1. <span data-ttu-id="446b5-109">Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="446b5-109">From a browser, sign in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="446b5-110">Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="446b5-110">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="446b5-111">I sökfältet hello-portalen, söka efter hello **Traffic Manager-profiler** och klicka sedan på hello-profilnamn som du vill tooconfigure hello routningsmetod för.</span><span class="sxs-lookup"><span data-stu-id="446b5-111">In hello portal’s search bar, search for hello **Traffic Manager profiles** and then click hello profile name that you want tooconfigure hello routing method for.</span></span>
3. <span data-ttu-id="446b5-112">I hello **trafikhanterarprofil** bladet, kontrollera att både hello molntjänster och webbplatser som du vill tooinclude i konfigurationen finns.</span><span class="sxs-lookup"><span data-stu-id="446b5-112">In hello **Traffic Manager profile** blade, verify that both hello cloud services and websites that you want tooinclude in your configuration are present.</span></span>
4. <span data-ttu-id="446b5-113">I hello **inställningar** klickar du på **Configuration**, och i hello **Configuration** bladet slutförts enligt följande:</span><span class="sxs-lookup"><span data-stu-id="446b5-113">In hello **Settings** section, click **Configuration**, and in hello **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="446b5-114">För **trafik routning inställningar**, kontrollera att hello trafikroutningsmetod **viktat**.</span><span class="sxs-lookup"><span data-stu-id="446b5-114">For **traffic routing method settings**, verify that hello traffic routing method is **Weighted**.</span></span> <span data-ttu-id="446b5-115">Om det inte är det, klickar du på **viktat** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="446b5-115">If it is not, click **Weighted** from hello dropdown list.</span></span>
    2. <span data-ttu-id="446b5-116">Ange hello **övervakaren slutpunktsinställningar** identiska för alla alla slutpunkter i den här profilen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="446b5-116">Set hello **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="446b5-117">Välj lämplig hello **protokollet**, och ange hello **Port** nummer.</span><span class="sxs-lookup"><span data-stu-id="446b5-117">Select hello appropriate **Protocol**, and specify hello **Port** number.</span></span> 
        2. <span data-ttu-id="446b5-118">För **sökväg** skriver ett snedstreck  */* .</span><span class="sxs-lookup"><span data-stu-id="446b5-118">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="446b5-119">toomonitor slutpunkter som du måste ange en sökväg och filnamn.</span><span class="sxs-lookup"><span data-stu-id="446b5-119">toomonitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="446b5-120">Ett snedstreck ”/” är ett giltigt för hello relativ sökväg och innebär att hello-filen är i hello rotkatalog (standard).</span><span class="sxs-lookup"><span data-stu-id="446b5-120">A forward slash "/" is a valid entry for hello relative path and implies that hello file is in hello root directory (default).</span></span>
        3. <span data-ttu-id="446b5-121">Hello överst på hello-sidan, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="446b5-121">At hello top of hello page, click **Save**.</span></span>
5. <span data-ttu-id="446b5-122">Testa hello ändringar i konfigurationen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="446b5-122">Test hello changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="446b5-123">Sök efter hello Traffic Manager-profilnamn hello portal sökfältet, och klicka på hello Traffic Manager-profilen i hello resultat som hello visas.</span><span class="sxs-lookup"><span data-stu-id="446b5-123">In hello portal’s search bar, search for hello Traffic Manager profile name and click hello Traffic Manager profile in hello results that hello displayed.</span></span>
    2.  <span data-ttu-id="446b5-124">I hello **Traffic Manager** profilen bladet, klickar du på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="446b5-124">In hello **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="446b5-125">Hej **trafikhanterarprofil** bladet visar hello DNS-namnet för nyskapade Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="446b5-125">hello **Traffic Manager profile** blade displays hello DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="446b5-126">Detta kan användas av alla klienter (till exempel genom att gå tooit med en webbläsare) tooget dirigeras toohello rätt slutpunkten enligt hello routning.</span><span class="sxs-lookup"><span data-stu-id="446b5-126">This can be used by any clients (for example,by navigating tooit using a web browser) tooget routed toohello right endpoint as determined by hello routing type.</span></span> <span data-ttu-id="446b5-127">I det här fallet alla begäranden dirigeras varje slutpunkt på ett sätt för resursallokering.</span><span class="sxs-lookup"><span data-stu-id="446b5-127">In this case all requests are routed each endpoint in a round-robin fashion.</span></span>
6. <span data-ttu-id="446b5-128">Redigera hello DNS-post på din auktoritära DNS-server toopoint företagets namn toohello Traffic Manager-domän domännamn när Traffic Manager-profilen fungerar.</span><span class="sxs-lookup"><span data-stu-id="446b5-128">Once your Traffic Manager profile is working, edit hello DNS record on your authoritative DNS server toopoint your company domain name toohello Traffic Manager domain name.</span></span>

![Konfigurera viktat trafikroutningsmetod med Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="446b5-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="446b5-130">Next steps</span></span>

- <span data-ttu-id="446b5-131">Lär dig mer om [prioritet trafikroutningsmetoden](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="446b5-131">Learn about [priority traffic routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="446b5-132">Lär dig mer om [prestanda trafikroutningsmetoden](traffic-manager-configure-performance-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="446b5-132">Learn about [performance traffic routing method](traffic-manager-configure-performance-routing-method.md).</span></span>
- <span data-ttu-id="446b5-133">Lär dig mer om [geografiska routningsmetoden](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="446b5-133">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="446b5-134">Lär dig hur för[testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="446b5-134">Learn how too[test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
