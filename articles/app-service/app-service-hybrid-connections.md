---
title: "aaa ”Hybridanslutningar i Azure App Service | Microsoft Docs ”"
description: "Hur toocreate och använda Hybridanslutningar tooaccess resurser i olika nätverk"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: 61d58068ab0a7c803019e3f0e92bde4273d1a053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-hybrid-connections"></a><span data-ttu-id="bda4f-103">Azure Apptjänst Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="bda4f-103">Azure App Service Hybrid Connections</span></span> #

## <a name="overview"></a><span data-ttu-id="bda4f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bda4f-104">Overview</span></span> ##

<span data-ttu-id="bda4f-105">Hybridanslutningar är både en tjänst i Azure som en funktion i hello Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bda4f-105">Hybrid Connections is both a service in Azure as well as a feature in hello Azure App Service.</span></span>  <span data-ttu-id="bda4f-106">Som en tjänst har användning och funktioner utöver de som utnyttjas i hello Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bda4f-106">As a service it has use and capabilities beyond those that are leveraged in hello Azure App Service.</span></span>  <span data-ttu-id="bda4f-107">Mer om Hybridanslutningar och deras användning utanför hello Azure App Service kan du börja här toolearn [Azure Relay Hybridanslutningar][HCService]</span><span class="sxs-lookup"><span data-stu-id="bda4f-107">toolearn more about Hybrid Connections and their usage outside of hello Azure App Service you can start here, [Azure Relay Hybrid Connections][HCService]</span></span>

<span data-ttu-id="bda4f-108">I hello Azure App Service vara hybridanslutningar används tooaccess resurser i andra nätverk.</span><span class="sxs-lookup"><span data-stu-id="bda4f-108">Within hello Azure App Service, hybrid connections can be used tooaccess application resources in other networks.</span></span> <span data-ttu-id="bda4f-109">Det ger åtkomst från din app tooan programmet slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="bda4f-109">It provides access FROM your app tooan application endpoint.</span></span>  <span data-ttu-id="bda4f-110">Det kan inte ett alternativt kapaciteten tooaccess ditt program.</span><span class="sxs-lookup"><span data-stu-id="bda4f-110">It does not enable an alternative capability tooaccess your application.</span></span>  <span data-ttu-id="bda4f-111">Som används i hello Apptjänst korrelerar varje hybridanslutning tooa enda TCP-värd och port kombination.</span><span class="sxs-lookup"><span data-stu-id="bda4f-111">As used in hello App Service, each hybrid connection correlates tooa single TCP host and port combination.</span></span>  <span data-ttu-id="bda4f-112">Det innebär att hello hybrid Anslutningens slutpunkt kan på ett annat operativsystem och alla program ges du träffa en lyssnande TCP-port.</span><span class="sxs-lookup"><span data-stu-id="bda4f-112">This means that hello hybrid connection endpoint can be on any operating system and any application provided you are hitting a TCP listening port.</span></span> <span data-ttu-id="bda4f-113">Hybridanslutningar inte vet och se vilka hello protokoll är eller vad du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="bda4f-113">Hybrid connections does not know or care what hello application protocol is or what you are accessing.</span></span>  <span data-ttu-id="bda4f-114">Den bara ger åtkomst till nätverket.</span><span class="sxs-lookup"><span data-stu-id="bda4f-114">It is simply providing network access.</span></span>  


## <a name="how-it-works"></a><span data-ttu-id="bda4f-115">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="bda4f-115">How it works</span></span> ##
<span data-ttu-id="bda4f-116">hello hybrid anslutningar funktionen består av två utgående samtal tooService Bus Relay.</span><span class="sxs-lookup"><span data-stu-id="bda4f-116">hello hybrid connections feature consists of two outbound calls tooService Bus Relay.</span></span>  <span data-ttu-id="bda4f-117">Det finns en anslutning från ett bibliotek på hello värd där appen körs i hello app service och det finns en anslutning från hello Hybrid anslutning Manager(HCM) tooService Bus relay.</span><span class="sxs-lookup"><span data-stu-id="bda4f-117">There is a connection from a library on hello host where your app is running in hello app service and then there is a connection from hello Hybrid Connection Manager(HCM) tooService Bus relay.</span></span>  <span data-ttu-id="bda4f-118">hello HCM är en relay-tjänst som du distribuerar i hello nätverket värd</span><span class="sxs-lookup"><span data-stu-id="bda4f-118">hello HCM is a relay service that you deploy within hello network hosting</span></span> 

<span data-ttu-id="bda4f-119">Via hello fast två kopplade anslutningar som din app har en TCP-tunnel tooa värdnamn: port-kombination på hello andra sidan av hello HCM.</span><span class="sxs-lookup"><span data-stu-id="bda4f-119">Through hello two joined connections your app has a TCP tunnel tooa fixed host:port combination on hello other side of hello HCM.</span></span>  <span data-ttu-id="bda4f-120">hello-anslutning använder TLS 1.2 för säkerhets- och SAS-nycklar för autentisering/auktorisering.</span><span class="sxs-lookup"><span data-stu-id="bda4f-120">hello connection uses TLS 1.2 for security and SAS keys for authentication/authorization.</span></span>    

![][1]

<span data-ttu-id="bda4f-121">När appen skickar en DNS-begäran som matchar en konfigurera Hybridanslutning slutpunkt sedan hello utgående TCP-trafik dirigeras ned hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="bda4f-121">When your app makes a DNS request that matches a configure Hybrid Connection endpoint, then hello outbound TCP traffic will be redirected down hello hybrid connection.</span></span>  

> [!NOTE]
> <span data-ttu-id="bda4f-122">Det innebär att du borde testa tooalways används ett DNS-namn för din Hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="bda4f-122">This means that you should try tooalways use a DNS name for your Hybrid Connection.</span></span>  <span data-ttu-id="bda4f-123">Vissa klientprogrammet utför inte i en DNS-sökning om hello slutpunkten används en IP-adress i stället.</span><span class="sxs-lookup"><span data-stu-id="bda4f-123">Some client software does not do a DNS lookup if hello endpoint uses an IP address instead.</span></span>
>
>

<span data-ttu-id="bda4f-124">Det finns två typer av hybridanslutningar hello nya hybridanslutningar som erbjuds som en tjänst med Azure Relay och hello äldre BizTalk-Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-124">There are two types of hybrid connections, hello new hybrid connections that are offered as a service under Azure Relay and hello older BizTalk Hybrid Connections.</span></span>  <span data-ttu-id="bda4f-125">hello är äldre BizTalk Hybridanslutningar refererad tooas klassiska Hybridanslutningar hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bda4f-125">hello older BizTalk Hybrid Connections are referred tooas Classic Hybrid Connections in hello portal.</span></span>  <span data-ttu-id="bda4f-126">Det finns mer information i det här dokumentet senare om dem.</span><span class="sxs-lookup"><span data-stu-id="bda4f-126">There is more information later in this document about them.</span></span>

### <a name="app-service-hybrid-connection-benefits"></a><span data-ttu-id="bda4f-127">Fördelar med Apptjänst hybrid-anslutning</span><span class="sxs-lookup"><span data-stu-id="bda4f-127">App Service hybrid connection benefits</span></span> ###

<span data-ttu-id="bda4f-128">Det finns ett antal fördelar toohello hybrid anslutningar kapaciteten inklusive</span><span class="sxs-lookup"><span data-stu-id="bda4f-128">There are a number of benefits toohello hybrid connections capability including</span></span>

- <span data-ttu-id="bda4f-129">Appar kan på ett säkert sätt komma åt på lokala datorer och tjänster på ett säkert sätt</span><span class="sxs-lookup"><span data-stu-id="bda4f-129">Apps can securely access on premises systems and services securely</span></span>
- <span data-ttu-id="bda4f-130">hello-funktionen kräver inte en tillgänglig internet-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="bda4f-130">hello feature does not require an internet accessible endpoint</span></span>
- <span data-ttu-id="bda4f-131">Det går snabbt och enkelt tooset in</span><span class="sxs-lookup"><span data-stu-id="bda4f-131">it is quick and easy tooset up</span></span>  
- <span data-ttu-id="bda4f-132">varje hybridanslutning matchar tooa enda värdnamn: port kombinationen som är en utmärkt säkerhet aspekt</span><span class="sxs-lookup"><span data-stu-id="bda4f-132">each hybrid connection matches tooa single host:port combination which is an excellent security aspect</span></span>
- <span data-ttu-id="bda4f-133">Normalt behöver inte brandvägg hål som hello anslutningar är alla utgående över standard webbportar</span><span class="sxs-lookup"><span data-stu-id="bda4f-133">it normally does not require firewall holes as hello connections are all outbound over standard web ports</span></span>
- <span data-ttu-id="bda4f-134">eftersom hello-funktionen är nätverksnivån som också innebär att det är storleksoberoende toohello språk som används av din app och hello teknik som används av hello slutpunkt</span><span class="sxs-lookup"><span data-stu-id="bda4f-134">because hello feature is network level that also means that it is agnostic toohello language used by your app and hello technology used by hello endpoint</span></span>
- <span data-ttu-id="bda4f-135">Det kan vara används tooprovide åtkomst i flera nätverk från en enda app</span><span class="sxs-lookup"><span data-stu-id="bda4f-135">it can be used tooprovide access in multiple networks from a single app</span></span> 

### <a name="things-you-cannot-do-with-hybrid-connections"></a><span data-ttu-id="bda4f-136">Saker du kan göra med Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="bda4f-136">Things you cannot do with Hybrid Connections</span></span> ###

<span data-ttu-id="bda4f-137">Det finns några saker som du inte kan göra med hybridanslutningar och de inkluderar:</span><span class="sxs-lookup"><span data-stu-id="bda4f-137">There are a few things you cannot do with hybrid connections and they include:</span></span>

- <span data-ttu-id="bda4f-138">montera en enhet</span><span class="sxs-lookup"><span data-stu-id="bda4f-138">mounting a drive</span></span>
- <span data-ttu-id="bda4f-139">med hjälp av UDP</span><span class="sxs-lookup"><span data-stu-id="bda4f-139">using UDP</span></span>
- <span data-ttu-id="bda4f-140">åtkomst till TCP baserade tjänster som använder dynamiska portar, till exempel FTP passivt läge eller utökad passivt läge</span><span class="sxs-lookup"><span data-stu-id="bda4f-140">accessing TCP based services that use dynamic ports such as FTP Passive Mode or Extended Passive Mode</span></span>
- <span data-ttu-id="bda4f-141">LDAP-stöd, som ibland kräver UDP</span><span class="sxs-lookup"><span data-stu-id="bda4f-141">LDAP support, as it sometimes requires UDP</span></span>
- <span data-ttu-id="bda4f-142">Active Directory-support</span><span class="sxs-lookup"><span data-stu-id="bda4f-142">Active Directory support</span></span>

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a><span data-ttu-id="bda4f-143">Lägga till och skapa en Hybridanslutning i din App</span><span class="sxs-lookup"><span data-stu-id="bda4f-143">Adding and Creating a Hybrid Connection in your App</span></span> ##

<span data-ttu-id="bda4f-144">Hybridanslutningar kan skapas via hello app portalen eller från hello Hybridanslutning tjänstportalen.</span><span class="sxs-lookup"><span data-stu-id="bda4f-144">Hybrid Connections can be created through hello app portal or from hello Hybrid Connection service portal.</span></span>  <span data-ttu-id="bda4f-145">Vi rekommenderar starkt att du använder hello appen företagsportal toocreate hello Hybridanslutningar som du vill toouse med din app.</span><span class="sxs-lookup"><span data-stu-id="bda4f-145">It is highly recommended that you use hello app portal toocreate hello Hybrid Connections you wish toouse with your app.</span></span>  <span data-ttu-id="bda4f-146">toocreate en Hybridanslutning gå toohello [Azure-portalen] [ portal] och gå till hello Användargränssnittet för din app.</span><span class="sxs-lookup"><span data-stu-id="bda4f-146">toocreate a Hybrid Connection go toohello [Azure portal][portal] and go into hello UI for your app.</span></span>  <span data-ttu-id="bda4f-147">Välj **nätverk > Konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-147">Select **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="bda4f-148">Här hittar du hello Hybridanslutningar som är konfigurerade med din app</span><span class="sxs-lookup"><span data-stu-id="bda4f-148">From here you can see hello Hybrid Connections that are configured with your app</span></span>  

![][2]

<span data-ttu-id="bda4f-149">tooadd en ny Hybridanslutning, klicka på Lägg till Hybrid-anslutning.</span><span class="sxs-lookup"><span data-stu-id="bda4f-149">tooadd a new Hybrid Connection, click Add Hybrid Connection.</span></span>  <span data-ttu-id="bda4f-150">hello-användargränssnitt som öppnas visar hello Hybridanslutningar som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="bda4f-150">hello UI that opens up lists hello Hybrid Connections that you have already created.</span></span>  <span data-ttu-id="bda4f-151">tooadd en eller flera av dem tooyour appen, klicka på hello som du vill använda och tryck på **Lägg till valda hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-151">tooadd one or more of them tooyour app, click on hello ones you want and hit **Add selected hybrid connection**.</span></span>  

![][3]

<span data-ttu-id="bda4f-152">Om du vill toocreate en ny Hybridanslutning **Skapa ny hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-152">If you want toocreate a new Hybrid Connection, click **Create new hybrid connection**.</span></span>  <span data-ttu-id="bda4f-153">Här anger du det:</span><span class="sxs-lookup"><span data-stu-id="bda4f-153">From here you specify the:</span></span> 

- <span data-ttu-id="bda4f-154">namnet på slutpunkten</span><span class="sxs-lookup"><span data-stu-id="bda4f-154">endpoint name</span></span>
- <span data-ttu-id="bda4f-155">slutpunktens värdnamn</span><span class="sxs-lookup"><span data-stu-id="bda4f-155">endpoint hostname</span></span>
- <span data-ttu-id="bda4f-156">port för slutpunkt</span><span class="sxs-lookup"><span data-stu-id="bda4f-156">endpoint port</span></span>
- <span data-ttu-id="bda4f-157">servicebus-namnområde som du vill att toouse</span><span class="sxs-lookup"><span data-stu-id="bda4f-157">servicebus namespace you wish toouse</span></span>

![][4]

<span data-ttu-id="bda4f-158">Varje hybridanslutning är bundet tooa service bus-namnrymd och varje service bus-namnrymd är i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="bda4f-158">Every hybrid connection is tied tooa service bus namespace and each service bus namespace is in an Azure region.</span></span>  <span data-ttu-id="bda4f-159">Det är viktigt tootry och använder en service bus-namnområde i hello samma region som din app så tooavoid framkallas Nätverksfördröjningen.</span><span class="sxs-lookup"><span data-stu-id="bda4f-159">It is important tootry and use a service bus namespace in hello same region as your app so as tooavoid network induced latency.</span></span>

<span data-ttu-id="bda4f-160">Om du vill tooremove din hybridanslutning från din app, högerklicka på den och välj **frånkoppling**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-160">If you want tooremove your hybrid connection from your app, right click on it and select **Disconnect**.</span></span>  

<span data-ttu-id="bda4f-161">När en hybridanslutning läggs tooyour webbprogrammet kan se du information på den genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="bda4f-161">Once a hybrid connection is added tooyour web app, you can see details on it by simply clicking on it.</span></span>  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a><span data-ttu-id="bda4f-162">Hybridanslutningar och Apptjänstplaner</span><span class="sxs-lookup"><span data-stu-id="bda4f-162">Hybrid Connections and App Service Plans</span></span> ##

<span data-ttu-id="bda4f-163">hello endast hybridanslutningar du nu är hello nya hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-163">hello only hybrid connections you can now make are hello new hybrid connections.</span></span>  <span data-ttu-id="bda4f-164">De är bara tillgängliga i Basic, Standard, Premium och isolerad priser SKU: er.</span><span class="sxs-lookup"><span data-stu-id="bda4f-164">They are only available in Basic, Standard, Premium and Isolated pricing SKUs.</span></span>  <span data-ttu-id="bda4f-165">Det finns begränsningar knutna toohello priser plan.</span><span class="sxs-lookup"><span data-stu-id="bda4f-165">There are limits tied toohello pricing plan.</span></span>  

| <span data-ttu-id="bda4f-166">Priser för planen</span><span class="sxs-lookup"><span data-stu-id="bda4f-166">Pricing Plan</span></span> | <span data-ttu-id="bda4f-167">Planera för antalet hybridanslutningar som kan användas i hello</span><span class="sxs-lookup"><span data-stu-id="bda4f-167">Number of hybrid connections usable in hello plan</span></span> |
|----|----|
| <span data-ttu-id="bda4f-168">Basic</span><span class="sxs-lookup"><span data-stu-id="bda4f-168">Basic</span></span> | <span data-ttu-id="bda4f-169">5</span><span class="sxs-lookup"><span data-stu-id="bda4f-169">5</span></span> |
| <span data-ttu-id="bda4f-170">Standard</span><span class="sxs-lookup"><span data-stu-id="bda4f-170">Standard</span></span> | <span data-ttu-id="bda4f-171">25</span><span class="sxs-lookup"><span data-stu-id="bda4f-171">25</span></span> |
| <span data-ttu-id="bda4f-172">Premium</span><span class="sxs-lookup"><span data-stu-id="bda4f-172">Premium</span></span> | <span data-ttu-id="bda4f-173">200</span><span class="sxs-lookup"><span data-stu-id="bda4f-173">200</span></span> |
| <span data-ttu-id="bda4f-174">Isolerad</span><span class="sxs-lookup"><span data-stu-id="bda4f-174">Isolated</span></span> | <span data-ttu-id="bda4f-175">200</span><span class="sxs-lookup"><span data-stu-id="bda4f-175">200</span></span> |

<span data-ttu-id="bda4f-176">Eftersom det finns begränsningar för App Service-Plan finns även Användargränssnittet i hello App Service-Plan som visar hur många hybridanslutningar används och vilka appar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-176">Since there are App Service Plan restrictions there is also UI in hello App Service Plan that shows you how many hybrid connections are being used and by what apps.</span></span>  

![][6]

<span data-ttu-id="bda4f-177">Precis som med hello app vy, kan du se information på din hybridanslutning genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="bda4f-177">Just as with hello app view, you can see details on your hybrid connection by clicking on it.</span></span>  <span data-ttu-id="bda4f-178">I hello egenskaperna som visas här och du kan se alla hello information som du hade hello app vy men kan också se hur många andra appar i hello använder samma App Service-Plan hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="bda4f-178">In hello properties shown here you can see all hello information you had at hello app view but can also see how many other apps in hello same App Service Plan are using that hybrid connection.</span></span>

![][7]

<span data-ttu-id="bda4f-179">När det finns en gräns på hello antal slutpunkter för hybridanslutning som kan användas i en Apptjänstplan, kan varje hybridanslutning används användas till alla appar i den App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="bda4f-179">While there is a limit on hello number of hybrid connection endpoints that can be used in an App Service Plan, each hybrid connection used can be used across any number of apps in that App Service Plan.</span></span>  <span data-ttu-id="bda4f-180">Det är toosay om jag hade 1 hybridanslutning som jag använde i 5 separata appar i min App Service-Plan som inte är fortfarande bara 1 hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="bda4f-180">That is toosay that if I had 1 hybrid connection that I used in 5 separate apps in my App Service Plan, that is still just 1 hybrid connection.</span></span>

<span data-ttu-id="bda4f-181">Det finns ett extra kostnad toohybrid anslutningar utöver som endast kan användas i en Basic, Standard, Premium eller isolerade SKU.</span><span class="sxs-lookup"><span data-stu-id="bda4f-181">There is an additional cost toohybrid connections beyond being only usable in a Basic, Standard, Premium or Isolated SKU.</span></span>  <span data-ttu-id="bda4f-182">För information om priser för hybrid anslutning finns här: [priser för Service Bus][sbpricing].</span><span class="sxs-lookup"><span data-stu-id="bda4f-182">For details on hybrid connection pricing please go here: [Service Bus pricing][sbpricing].</span></span>

## <a name="hybrid-connection-manager"></a><span data-ttu-id="bda4f-183">Hybridanslutningshanterare</span><span class="sxs-lookup"><span data-stu-id="bda4f-183">Hybrid Connection Manager</span></span> ##

<span data-ttu-id="bda4f-184">För hybrid anslutningar toowork måste en relay agent i hello-nätverk som är värd för din hybrid Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="bda4f-184">In order for hybrid connections toowork you need a relay agent in hello network that hosts your hybrid connection endpoint.</span></span>  <span data-ttu-id="bda4f-185">Den relay agenten kallas hello Manager för Hybrid-anslutning (HCM).</span><span class="sxs-lookup"><span data-stu-id="bda4f-185">That relay agent is called hello Hybrid Connection Manager (HCM).</span></span>  <span data-ttu-id="bda4f-186">Det här verktyget kan hämtas från hello **nätverk > Konfigurera slutpunkter för din hybridanslutning** användargränssnitt som är tillgängliga från din app i hello [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="bda4f-186">This tool can be downloaded from hello **Networking > Configure your hybrid connection endpoints** UI available from your app in hello [Azure portal][portal].</span></span>  

<span data-ttu-id="bda4f-187">Det här verktyget körs på Windows server 2008 R2 och senare versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="bda4f-187">This tool runs on Windows server 2008 R2 and later versions of Windows.</span></span>  <span data-ttu-id="bda4f-188">När du installerade hello HCM körs som en tjänst.</span><span class="sxs-lookup"><span data-stu-id="bda4f-188">Once installed hello HCM runs as a service.</span></span>  <span data-ttu-id="bda4f-189">Den här tjänsten ansluter tooAzure servicebus relay baserat på konfigurerad hello slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="bda4f-189">This service connects tooAzure servicebus relay based on hello configured endpoints.</span></span>  <span data-ttu-id="bda4f-190">hello anslutningar från hello HCM är utgående tooports 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="bda4f-190">hello connections from hello HCM are outbound tooports 80 and 443.</span></span>    

<span data-ttu-id="bda4f-191">hello HCM har ett användargränssnitt tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="bda4f-191">hello HCM has a UI tooconfigure it.</span></span>  <span data-ttu-id="bda4f-192">Efter hello HCM har installerats kan du ta fram hello Användargränssnittet genom att köra hello HybridConnectionManagerUi.exe som placeras i installationskatalogen för hello Hybridanslutningshanteraren.</span><span class="sxs-lookup"><span data-stu-id="bda4f-192">After hello HCM is installed you can bring up hello UI by running hello HybridConnectionManagerUi.exe that sits in hello Hybrid Connection Manager installation directory.</span></span>  <span data-ttu-id="bda4f-193">Uppnås också enkelt på Windows 10 genom att skriva *Hybridanslutningshanteraren UI* i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="bda4f-193">It is also easily reached on Windows 10 by typing *Hybrid Connection Manager UI* in your search box.</span></span>  

<span data-ttu-id="bda4f-194">När hello HCM UI startas hello först öppna du är en tabell med alla hello hybridanslutningar som är konfigurerade med den här instansen av hello HCM finns.</span><span class="sxs-lookup"><span data-stu-id="bda4f-194">When hello HCM UI is started, hello first thing you see is a table that lists all of hello hybrid connections that are configured with this instance of hello HCM.</span></span>  <span data-ttu-id="bda4f-195">Om du inte vill toomake ändringar behöver tooauthenticate med Azure.</span><span class="sxs-lookup"><span data-stu-id="bda4f-195">If you wish toomake any changes you will need tooauthenticate with Azure.</span></span> 

<span data-ttu-id="bda4f-196">tooadd en eller flera Hybridanslutningar tooyour HCM:</span><span class="sxs-lookup"><span data-stu-id="bda4f-196">tooadd one or more Hybrid Connections tooyour HCM:</span></span>

1. <span data-ttu-id="bda4f-197">Starta hello HCM UI</span><span class="sxs-lookup"><span data-stu-id="bda4f-197">Start hello HCM UI</span></span>
1. <span data-ttu-id="bda4f-198">Klicka på Konfigurera en annan hybridanslutning![][8]</span><span class="sxs-lookup"><span data-stu-id="bda4f-198">Click Configure another hybrid connection ![][8]</span></span>

1. <span data-ttu-id="bda4f-199">Logga in med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="bda4f-199">Sign in with your Azure account</span></span>
1. <span data-ttu-id="bda4f-200">Välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="bda4f-200">Choose a subscription</span></span>
1. <span data-ttu-id="bda4f-201">Klicka på hello hybridanslutningar du vill använda den här HCM toorelay![][9]</span><span class="sxs-lookup"><span data-stu-id="bda4f-201">Click on hello hybrid connections you want this HCM toorelay ![][9]</span></span>

1. <span data-ttu-id="bda4f-202">Klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="bda4f-202">Click Save</span></span>

<span data-ttu-id="bda4f-203">Nu visas hello hybridanslutningar som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="bda4f-203">At this point you will see hello hybrid connections you added.</span></span>  <span data-ttu-id="bda4f-204">Du kan också klicka på hello konfigurerad hybridanslutning och se information om hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="bda4f-204">You can also click on hello configured hybrid connection and see details about hello connection.</span></span>

![][10]

<span data-ttu-id="bda4f-205">För HCM toobe kan toosupport hello hybrid anslutningarna konfigureras den med, måste den:</span><span class="sxs-lookup"><span data-stu-id="bda4f-205">For your HCM toobe able toosupport hello hybrid connections it is configured with, it needs:</span></span>

- <span data-ttu-id="bda4f-206">TCP åtkomst tooAzure via portarna 80 och 443</span><span class="sxs-lookup"><span data-stu-id="bda4f-206">TCP access tooAzure over ports 80 and 443</span></span>
- <span data-ttu-id="bda4f-207">TCP åtkomst toohello hybrid Anslutningens slutpunkt</span><span class="sxs-lookup"><span data-stu-id="bda4f-207">TCP access toohello hybrid connection endpoint</span></span>
- <span data-ttu-id="bda4f-208">möjlighet toodo DNS-sökningar på hello endpoint värd och hello azure servicebus-namnområde</span><span class="sxs-lookup"><span data-stu-id="bda4f-208">ability toodo DNS look ups on hello endpoint host and hello azure servicebus namespace</span></span>

<span data-ttu-id="bda4f-209">hello HCM stöder både nya hybridanslutningar samt hello äldre BizTalk hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-209">hello HCM supports both new hybrid connections as well as hello older BizTalk hybrid connections.</span></span>

### <a name="redundancy"></a><span data-ttu-id="bda4f-210">Redundans</span><span class="sxs-lookup"><span data-stu-id="bda4f-210">Redundancy</span></span> ###

<span data-ttu-id="bda4f-211">Varje HCM kan stödja flera hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-211">Each HCM can support multiple hybrid connections.</span></span>  <span data-ttu-id="bda4f-212">Alla angivna hybridanslutning stöds också av flera HCMs.</span><span class="sxs-lookup"><span data-stu-id="bda4f-212">Also, any given hybrid connection can be supported by multiple HCMs.</span></span>  <span data-ttu-id="bda4f-213">hello standardbeteendet är tooround robin trafiken över hello konfigurerade HCMs för alla angivna slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="bda4f-213">hello default behavior is tooround robin traffic across hello configured HCMs for any given endpoint.</span></span>  <span data-ttu-id="bda4f-214">Om du vill hög tillgänglighet på hybridanslutningar från ditt nätverk kan bara skapa en instans av flera HCMs på separata datorer.</span><span class="sxs-lookup"><span data-stu-id="bda4f-214">If you want high availability on your hybrid connections from your network, simply instantiate multiple HCMs on separate machines.</span></span> 

### <a name="manually-adding-a-hybrid-connection"></a><span data-ttu-id="bda4f-215">Att lägga till en hybridanslutning manuellt</span><span class="sxs-lookup"><span data-stu-id="bda4f-215">Manually adding a hybrid connection</span></span> ###

<span data-ttu-id="bda4f-216">Om du vill att någon utanför din prenumeration toohost en HCM-instans för en given hybridanslutning, kan du dela med dem hello anslutningssträng för gateway för hello hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="bda4f-216">If you wish somebody outside of your subscription toohost an HCM instance for a given hybrid connection, you can share with them hello gateway connection string for hello hybrid connection.</span></span>  <span data-ttu-id="bda4f-217">Du kan se detta i hello egenskaperna för en hybridanslutning i hello [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="bda4f-217">You can see this in hello properties for a hybrid connection in hello [Azure portal][portal].</span></span> <span data-ttu-id="bda4f-218">toouse sträng, klicka på hello **konfigurera manuellt** i hello HCM och klistra in i anslutningssträngen för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="bda4f-218">toouse that string, click hello **Configure manually** button in hello HCM and paste in hello gateway connection string.</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="bda4f-219">Felsökning</span><span class="sxs-lookup"><span data-stu-id="bda4f-219">Troubleshooting</span></span> ##

<span data-ttu-id="bda4f-220">När din hybridanslutning ställs in med ett program som körs och det finns minst en HCM som har konfigurerats hybridanslutningen och sedan hello status står **ansluten** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="bda4f-220">When your hybrid connection is set up with a running application and there is at least one HCM that has that hybrid connection configured, then hello status will say **Connected** in hello portal.</span></span>  <span data-ttu-id="bda4f-221">Om meddelandet inte anger **ansluten** innebär att din app är nere eller din HCM kan inte ansluta ut tooAzure på port 80 eller 443.</span><span class="sxs-lookup"><span data-stu-id="bda4f-221">If it does not say **Connected** then it means that either your app is down or your HCM cannot connect out tooAzure on ports 80 or 443.</span></span>  

<span data-ttu-id="bda4f-222">hello främsta skälet att klienter inte kan ansluta tootheir slutpunkten är eftersom hello endpoint angavs med en IP-adress i stället för ett DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="bda4f-222">hello primary reason that clients cannot connect tootheir endpoint is because hello endpoint was specified using an IP address instead of a DNS name.</span></span>  <span data-ttu-id="bda4f-223">Om din app kan inte nå hello önskad slutpunkt och du använde en IP-adress, växla toousing ett DNS-namn som är giltiga på hello värd där hello HCM körs.</span><span class="sxs-lookup"><span data-stu-id="bda4f-223">If your app cannot reach hello desired endpoint and you used an IP address, switch toousing a DNS name that is valid on hello host where hello HCM is running.</span></span>  <span data-ttu-id="bda4f-224">Andra saker toocheck är att hello DNS-namn matchas korrekt på hello värd där hello HCM körs och att det finns en anslutning från hello värden där hello HCM körs toohello hybrid Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="bda4f-224">Other things toocheck are that hello DNS name resolves properly on hello host where hello HCM is running and that there is connectivity from hello host where hello HCM is running toohello hybrid connection endpoint.</span></span>  

<span data-ttu-id="bda4f-225">Det finns ett verktyg i hello App Service som kan anropas från hello-konsolen som kallas tcpping.</span><span class="sxs-lookup"><span data-stu-id="bda4f-225">There is a tool in hello App Service that can be invoked from hello console called tcpping.</span></span>  <span data-ttu-id="bda4f-226">Det här verktyget kan berätta om du har åtkomst tooa TCP-slutpunkt men inte berättar om du har åtkomst tooa hybrid Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="bda4f-226">This tool can tell you if you have access tooa TCP endpoint but does not tell you if you have access tooa hybrid connection endpoint.</span></span>  <span data-ttu-id="bda4f-227">När den används i hello konsolen mot en hybrid Anslutningens slutpunkt, talar en lyckad ping endast om att du har en hybridanslutning som konfigurerats för din app som använder som värdnamn: port-kombination.</span><span class="sxs-lookup"><span data-stu-id="bda4f-227">When used in hello console against a hybrid connection endpoint, a successful ping will only tell you that you have a hybrid connection configured for your app that uses that host:port combination.</span></span>  

## <a name="biztalk-hybrid-connections"></a><span data-ttu-id="bda4f-228">BizTalks hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="bda4f-228">BizTalk Hybrid Connections</span></span> ##

<span data-ttu-id="bda4f-229">hello äldre BizTalk Hybridanslutningar kapaciteten har stängts av toofurther BizTalk Hybridanslutning skapande.</span><span class="sxs-lookup"><span data-stu-id="bda4f-229">hello older BizTalk Hybrid Connections capability has been closed off toofurther BizTalk Hybrid Connection creations.</span></span>  <span data-ttu-id="bda4f-230">Du kan fortsätta använda din befintliga BizTalk-Hybridanslutningar med dina appar, men du bör migrera toohello ny tjänst.</span><span class="sxs-lookup"><span data-stu-id="bda4f-230">You can continue using your preexisting BizTalk Hybrid Connections with your apps but should migrate toohello new service.</span></span>  <span data-ttu-id="bda4f-231">Bland hello finns fördelarna hello ny tjänst över hello BizTalk-version:</span><span class="sxs-lookup"><span data-stu-id="bda4f-231">Among hello benefits in hello new service over hello BizTalk version are:</span></span>

- <span data-ttu-id="bda4f-232">Det krävs inga ytterligare BizTalk-konto</span><span class="sxs-lookup"><span data-stu-id="bda4f-232">no additional BizTalk account is required</span></span>
- <span data-ttu-id="bda4f-233">TLS är 1.2 i stället för 1.0 som BizTalk Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="bda4f-233">TLS is 1.2 instead of 1.0 as in BizTalk Hybrid Connections</span></span>
- <span data-ttu-id="bda4f-234">Kommunikation är via portarna 80 och 443 med hjälp av en DNS-namnet tooreach Azure i stället för IP-adresser och ett antal ytterligare andra portar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-234">Communication is over ports 80 and 443 using a DNS name tooreach Azure instead of IP addresses and a range of additional other ports.</span></span>  

<span data-ttu-id="bda4f-235">tooadd en BizTalk hybrid anslutning tooyour app, gå tooyour app i hello [Azure-portalen] [ portal] och på **nätverk > Konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-235">tooadd a BizTalk hybrid connection tooyour app, go tooyour app in hello [Azure portal][portal] and click **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="bda4f-236">Klicka i hello klassisk hybrid Anslutningstabell **lägga till klassiska hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="bda4f-236">In hello Classic hybrid connections table click **Add classic hybrid connection**.</span></span>  <span data-ttu-id="bda4f-237">Här visas en lista över dina BizTalk hybrid-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="bda4f-237">From here you will see a list of your BizTalk hybrid connections.</span></span>  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

