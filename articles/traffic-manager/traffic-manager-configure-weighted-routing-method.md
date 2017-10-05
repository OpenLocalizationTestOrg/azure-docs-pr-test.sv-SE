---
title: "Konfigurera routningsmetoden för viktad resursallokering trafik med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur du läser in Utjämna trafiken genom att använda en resursallokering-metoden i Traffic Manager"
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
ms.openlocfilehash: 7aa4c9120d44ff1b3e59a57090ea04e3f8021fc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a><span data-ttu-id="30706-103">Konfigurera routningsmetoden för viktat trafik i Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="30706-103">Configure the weighted traffic routing method in Traffic Manager</span></span>

<span data-ttu-id="30706-104">En vanliga trafik routning metoden mönstret är att tillhandahålla en uppsättning identiska slutpunkter, bland annat molntjänster och webbplatser, och skicka trafik till varje resursallokering överskrids.</span><span class="sxs-lookup"><span data-stu-id="30706-104">A common traffic routing method pattern is to provide a set of identical endpoints, which include cloud services and websites, and send traffic to each in a round-robin fashion.</span></span> <span data-ttu-id="30706-105">Följande steg beskriver hur du konfigurerar den här typen av trafikroutningsmetod.</span><span class="sxs-lookup"><span data-stu-id="30706-105">The following steps outline how to configure this type of traffic routing method.</span></span>

> [!NOTE]
> <span data-ttu-id="30706-106">Azure webbplatser tillhandahåller redan resursallokering för webbplatser inom ett datacenter (även kallat en region) för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="30706-106">Azure Websites already provide round-robin load balancing functionality for websites within a datacenter (also known as a region).</span></span> <span data-ttu-id="30706-107">Traffic Manager kan du ange routningsmetoden för resursallokering för webbplatser i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="30706-107">Traffic Manager allows you to specify round-robin traffic routing method for websites in different datacenters.</span></span>

## <a name="to-configure-the-weighted-traffic-routing-method"></a><span data-ttu-id="30706-108">Så här konfigurerar du viktad trafikroutningsmetod</span><span class="sxs-lookup"><span data-stu-id="30706-108">To configure the weighted traffic routing method</span></span>

1. <span data-ttu-id="30706-109">Logga in på [Azure Portal](http://portal.azure.com) från en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="30706-109">From a browser, sign in to the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="30706-110">Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="30706-110">If you don’t already have an account, you can sign up for a [free one-month trial](https://azure.microsoft.com/free/).</span></span> 
2. <span data-ttu-id="30706-111">I portalens sökfältet, söker du efter den **Traffic Manager-profiler** och klicka sedan på namnet på profilen som du vill konfigurera routningsmetoden för.</span><span class="sxs-lookup"><span data-stu-id="30706-111">In the portal’s search bar, search for the **Traffic Manager profiles** and then click the profile name that you want to configure the routing method for.</span></span>
3. <span data-ttu-id="30706-112">I den **trafikhanterarprofil** bladet, kontrollera att både molntjänster och webbplatser som du vill ska ingå i konfigurationen finns kvar.</span><span class="sxs-lookup"><span data-stu-id="30706-112">In the **Traffic Manager profile** blade, verify that both the cloud services and websites that you want to include in your configuration are present.</span></span>
4. <span data-ttu-id="30706-113">I den **inställningar** klickar du på **Configuration**, och i den **Configuration** bladet slutförts enligt följande:</span><span class="sxs-lookup"><span data-stu-id="30706-113">In the **Settings** section, click **Configuration**, and in the **Configuration** blade, complete as follows:</span></span>
    1. <span data-ttu-id="30706-114">För **trafik routning inställningar**, kontrollera att trafikroutningsmetod **viktat**.</span><span class="sxs-lookup"><span data-stu-id="30706-114">For **traffic routing method settings**, verify that the traffic routing method is **Weighted**.</span></span> <span data-ttu-id="30706-115">Om det inte är det, klickar du på **viktat** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="30706-115">If it is not, click **Weighted** from the dropdown list.</span></span>
    2. <span data-ttu-id="30706-116">Ange den **övervakaren slutpunktsinställningar** identiska för alla alla slutpunkter i den här profilen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="30706-116">Set the **Endpoint monitor settings** identical for all every endpoint within this profile as follows:</span></span>
        1. <span data-ttu-id="30706-117">Välj lämpliga **protokollet**, och ange den **Port** nummer.</span><span class="sxs-lookup"><span data-stu-id="30706-117">Select the appropriate **Protocol**, and specify the **Port** number.</span></span> 
        2. <span data-ttu-id="30706-118">För **sökväg** skriver ett snedstreck  */* .</span><span class="sxs-lookup"><span data-stu-id="30706-118">For **Path** type a forward slash */*.</span></span> <span data-ttu-id="30706-119">Du måste ange en sökväg och filnamn för att övervaka slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="30706-119">To monitor endpoints, you must specify a path and filename.</span></span> <span data-ttu-id="30706-120">Ett snedstreck ”/” är ett giltigt för den relativa sökvägen och innebär att filen är i rotkatalogen (standard).</span><span class="sxs-lookup"><span data-stu-id="30706-120">A forward slash "/" is a valid entry for the relative path and implies that the file is in the root directory (default).</span></span>
        3. <span data-ttu-id="30706-121">Överst på sidan, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="30706-121">At the top of the page, click **Save**.</span></span>
5. <span data-ttu-id="30706-122">Testa ändringar i konfigurationen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="30706-122">Test the changes in your configuration as follows:</span></span>
    1.  <span data-ttu-id="30706-123">I portalens sökfältet, söka efter namnet på Traffic Manager-profilen och klickar på Traffic Manager-profilen i resultatet som de visas.</span><span class="sxs-lookup"><span data-stu-id="30706-123">In the portal’s search bar, search for the Traffic Manager profile name and click the Traffic Manager profile in the results that the displayed.</span></span>
    2.  <span data-ttu-id="30706-124">I den **Traffic Manager** profilen bladet, klickar du på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="30706-124">In the **Traffic Manager** profile blade, click **Overview**.</span></span>
    3.  <span data-ttu-id="30706-125">Den **trafikhanterarprofil** bladet visar DNS-namnet för nyskapade Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="30706-125">The **Traffic Manager profile** blade displays the DNS name of your newly created Traffic Manager profile.</span></span> <span data-ttu-id="30706-126">Detta kan användas av alla klienter (till exempel genom att navigera till den med hjälp av en webbläsare) att hämta dirigeras till rätt slutpunkten som bestäms av typen routning.</span><span class="sxs-lookup"><span data-stu-id="30706-126">This can be used by any clients (for example,by navigating to it using a web browser) to get routed to the right endpoint as determined by the routing type.</span></span> <span data-ttu-id="30706-127">I det här fallet alla begäranden dirigeras varje slutpunkt på ett sätt för resursallokering.</span><span class="sxs-lookup"><span data-stu-id="30706-127">In this case all requests are routed each endpoint in a round-robin fashion.</span></span>
6. <span data-ttu-id="30706-128">När Traffic Manager-profilen fungerar redigera DNS-posten på auktoritära DNS-servern att peka företagets domännamn till domännamnet för Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="30706-128">Once your Traffic Manager profile is working, edit the DNS record on your authoritative DNS server to point your company domain name to the Traffic Manager domain name.</span></span>

![Konfigurera viktat trafikroutningsmetod med Traffic Manager][1]

## <a name="next-steps"></a><span data-ttu-id="30706-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30706-130">Next steps</span></span>

- <span data-ttu-id="30706-131">Lär dig mer om [prioritet trafikroutningsmetoden](traffic-manager-configure-priority-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="30706-131">Learn about [priority traffic routing method](traffic-manager-configure-priority-routing-method.md).</span></span>
- <span data-ttu-id="30706-132">Lär dig mer om [prestanda trafikroutningsmetoden](traffic-manager-configure-performance-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="30706-132">Learn about [performance traffic routing method](traffic-manager-configure-performance-routing-method.md).</span></span>
- <span data-ttu-id="30706-133">Lär dig mer om [geografiska routningsmetoden](traffic-manager-configure-geographic-routing-method.md).</span><span class="sxs-lookup"><span data-stu-id="30706-133">Learn about [geographic routing method](traffic-manager-configure-geographic-routing-method.md).</span></span>
- <span data-ttu-id="30706-134">Lär dig hur du [testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).</span><span class="sxs-lookup"><span data-stu-id="30706-134">Learn how to [test Traffic Manager settings](traffic-manager-testing-settings.md).</span></span>

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
