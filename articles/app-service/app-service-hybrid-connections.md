---
title: "Azure Apptjänst Hybridanslutningar | Microsoft Docs"
description: "Hur du skapar och använda Hybridanslutningar och komma åt resurser i olika nätverk"
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
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a><span data-ttu-id="be30c-103">Azure Apptjänst Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="be30c-103">Azure App Service Hybrid Connections</span></span> #

## <a name="overview"></a><span data-ttu-id="be30c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="be30c-104">Overview</span></span> ##

<span data-ttu-id="be30c-105">Hybridanslutningar är både en tjänst i Azure som en funktion i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="be30c-105">Hybrid Connections is both a service in Azure as well as a feature in the Azure App Service.</span></span>  <span data-ttu-id="be30c-106">Som en tjänst har användning och funktioner utöver de som utnyttjas i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="be30c-106">As a service it has use and capabilities beyond those that are leveraged in the Azure App Service.</span></span>  <span data-ttu-id="be30c-107">Mer information om Hybridanslutningar och deras användning utanför Azure App Service som du kan börja här [Azure Relay Hybridanslutningar][HCService]</span><span class="sxs-lookup"><span data-stu-id="be30c-107">To learn more about Hybrid Connections and their usage outside of the Azure App Service you can start here, [Azure Relay Hybrid Connections][HCService]</span></span>

<span data-ttu-id="be30c-108">I Azure App Service kan hybridanslutningar användas för att komma åt resurser i andra nätverk.</span><span class="sxs-lookup"><span data-stu-id="be30c-108">Within the Azure App Service, hybrid connections can be used to access application resources in other networks.</span></span> <span data-ttu-id="be30c-109">Det ger åtkomst från din app till en slutpunkt för programmet.</span><span class="sxs-lookup"><span data-stu-id="be30c-109">It provides access FROM your app TO an application endpoint.</span></span>  <span data-ttu-id="be30c-110">Det kan inte ett alternativt möjlighet att komma åt programmet.</span><span class="sxs-lookup"><span data-stu-id="be30c-110">It does not enable an alternative capability to access your application.</span></span>  <span data-ttu-id="be30c-111">Som används i App Service motsvarar varje hybridanslutning en enstaka kombination av TCP-värd och port.</span><span class="sxs-lookup"><span data-stu-id="be30c-111">As used in the App Service, each hybrid connection correlates to a single TCP host and port combination.</span></span>  <span data-ttu-id="be30c-112">Detta innebär att hybrid Anslutningens slutpunkt kan finnas på alla operativsystem och alla program som du träffa en lyssnande TCP-port.</span><span class="sxs-lookup"><span data-stu-id="be30c-112">This means that the hybrid connection endpoint can be on any operating system and any application provided you are hitting a TCP listening port.</span></span> <span data-ttu-id="be30c-113">Hybridanslutningar vet inte eller är protokollet är eller vad du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="be30c-113">Hybrid connections does not know or care what the application protocol is or what you are accessing.</span></span>  <span data-ttu-id="be30c-114">Den bara ger åtkomst till nätverket.</span><span class="sxs-lookup"><span data-stu-id="be30c-114">It is simply providing network access.</span></span>  


## <a name="how-it-works"></a><span data-ttu-id="be30c-115">Så här fungerar det</span><span class="sxs-lookup"><span data-stu-id="be30c-115">How it works</span></span> ##
<span data-ttu-id="be30c-116">Funktionen hybrid anslutningar består av två utgående samtal till Service Bus Relay.</span><span class="sxs-lookup"><span data-stu-id="be30c-116">The hybrid connections feature consists of two outbound calls to Service Bus Relay.</span></span>  <span data-ttu-id="be30c-117">Det finns en anslutning från ett bibliotek på värden där appen körs i app service och det finns en anslutning från Hybrid anslutning Manager(HCM) till Service Bus relay.</span><span class="sxs-lookup"><span data-stu-id="be30c-117">There is a connection from a library on the host where your app is running in the app service and then there is a connection from the Hybrid Connection Manager(HCM) to Service Bus relay.</span></span>  <span data-ttu-id="be30c-118">HCM är en relay-tjänst som du distribuerar i nätverket värd</span><span class="sxs-lookup"><span data-stu-id="be30c-118">The HCM is a relay service that you deploy within the network hosting</span></span> 

<span data-ttu-id="be30c-119">Via två kopplade anslutningar har din app en TCP-tunnel till en fast värdnamn: port-kombination på andra sidan av HCM.</span><span class="sxs-lookup"><span data-stu-id="be30c-119">Through the two joined connections your app has a TCP tunnel to a fixed host:port combination on the other side of the HCM.</span></span>  <span data-ttu-id="be30c-120">Anslutningen använder TLS 1.2 för säkerhets- och SAS-nycklar för autentisering/auktorisering.</span><span class="sxs-lookup"><span data-stu-id="be30c-120">The connection uses TLS 1.2 for security and SAS keys for authentication/authorization.</span></span>    

![][1]

<span data-ttu-id="be30c-121">När din app gör en DNS-begäran som matchar en konfigurera Hybridanslutning slutpunkt, dirigeras utgående TCP-trafik av hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="be30c-121">When your app makes a DNS request that matches a configure Hybrid Connection endpoint, then the outbound TCP traffic will be redirected down the hybrid connection.</span></span>  

> [!NOTE]
> <span data-ttu-id="be30c-122">Det innebär att försök att alltid använda ett DNS-namn för din Hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="be30c-122">This means that you should try to always use a DNS name for your Hybrid Connection.</span></span>  <span data-ttu-id="be30c-123">Vissa klientprogrammet utför inte i en DNS-sökning om slutpunkten används en IP-adress i stället.</span><span class="sxs-lookup"><span data-stu-id="be30c-123">Some client software does not do a DNS lookup if the endpoint uses an IP address instead.</span></span>
>
>

<span data-ttu-id="be30c-124">Det finns två typer av hybridanslutningar nya hybridanslutningar som erbjuds som en tjänst under Azure Relay och äldre BizTalk-Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="be30c-124">There are two types of hybrid connections, the new hybrid connections that are offered as a service under Azure Relay and the older BizTalk Hybrid Connections.</span></span>  <span data-ttu-id="be30c-125">Äldre BizTalk-Hybridanslutningar kallas klassiska Hybridanslutningar i portalen.</span><span class="sxs-lookup"><span data-stu-id="be30c-125">The older BizTalk Hybrid Connections are referred to as Classic Hybrid Connections in the portal.</span></span>  <span data-ttu-id="be30c-126">Det finns mer information i det här dokumentet senare om dem.</span><span class="sxs-lookup"><span data-stu-id="be30c-126">There is more information later in this document about them.</span></span>

### <a name="app-service-hybrid-connection-benefits"></a><span data-ttu-id="be30c-127">Fördelar med Apptjänst hybrid-anslutning</span><span class="sxs-lookup"><span data-stu-id="be30c-127">App Service hybrid connection benefits</span></span> ###

<span data-ttu-id="be30c-128">Det finns ett antal fördelar för hybrid anslutningar kapaciteten inklusive</span><span class="sxs-lookup"><span data-stu-id="be30c-128">There are a number of benefits to the hybrid connections capability including</span></span>

- <span data-ttu-id="be30c-129">Appar kan på ett säkert sätt komma åt på lokala datorer och tjänster på ett säkert sätt</span><span class="sxs-lookup"><span data-stu-id="be30c-129">Apps can securely access on premises systems and services securely</span></span>
- <span data-ttu-id="be30c-130">funktionen kräver inte en tillgänglig internet-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="be30c-130">the feature does not require an internet accessible endpoint</span></span>
- <span data-ttu-id="be30c-131">Det går snabbt och enkelt att konfigurera</span><span class="sxs-lookup"><span data-stu-id="be30c-131">it is quick and easy to set up</span></span>  
- <span data-ttu-id="be30c-132">varje hybridanslutning matchas med en enda värdnamn: port-kombination som är en del av utmärkt säkerhet</span><span class="sxs-lookup"><span data-stu-id="be30c-132">each hybrid connection matches to a single host:port combination which is an excellent security aspect</span></span>
- <span data-ttu-id="be30c-133">Normalt behöver inte brandvägg hål som anslutningarna är alla utgående över standard webbportar</span><span class="sxs-lookup"><span data-stu-id="be30c-133">it normally does not require firewall holes as the connections are all outbound over standard web ports</span></span>
- <span data-ttu-id="be30c-134">eftersom funktionen är nätverksnivån som betyder också att som är oberoende språket som används av din app och den teknik som används av slutpunkten</span><span class="sxs-lookup"><span data-stu-id="be30c-134">because the feature is network level that also means that it is agnostic to the language used by your app and the technology used by the endpoint</span></span>
- <span data-ttu-id="be30c-135">den kan användas för att ge åtkomst i flera nätverk från en enda app</span><span class="sxs-lookup"><span data-stu-id="be30c-135">it can be used to provide access in multiple networks from a single app</span></span> 

### <a name="things-you-cannot-do-with-hybrid-connections"></a><span data-ttu-id="be30c-136">Saker du kan göra med Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="be30c-136">Things you cannot do with Hybrid Connections</span></span> ###

<span data-ttu-id="be30c-137">Det finns några saker som du inte kan göra med hybridanslutningar och de inkluderar:</span><span class="sxs-lookup"><span data-stu-id="be30c-137">There are a few things you cannot do with hybrid connections and they include:</span></span>

- <span data-ttu-id="be30c-138">montera en enhet</span><span class="sxs-lookup"><span data-stu-id="be30c-138">mounting a drive</span></span>
- <span data-ttu-id="be30c-139">med hjälp av UDP</span><span class="sxs-lookup"><span data-stu-id="be30c-139">using UDP</span></span>
- <span data-ttu-id="be30c-140">åtkomst till TCP baserade tjänster som använder dynamiska portar, till exempel FTP passivt läge eller utökad passivt läge</span><span class="sxs-lookup"><span data-stu-id="be30c-140">accessing TCP based services that use dynamic ports such as FTP Passive Mode or Extended Passive Mode</span></span>
- <span data-ttu-id="be30c-141">LDAP-stöd, som ibland kräver UDP</span><span class="sxs-lookup"><span data-stu-id="be30c-141">LDAP support, as it sometimes requires UDP</span></span>
- <span data-ttu-id="be30c-142">Active Directory-support</span><span class="sxs-lookup"><span data-stu-id="be30c-142">Active Directory support</span></span>

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a><span data-ttu-id="be30c-143">Lägga till och skapa en Hybridanslutning i din App</span><span class="sxs-lookup"><span data-stu-id="be30c-143">Adding and Creating a Hybrid Connection in your App</span></span> ##

<span data-ttu-id="be30c-144">Hybridanslutningar kan skapas via portalen app eller från tjänstportalen Hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="be30c-144">Hybrid Connections can be created through the app portal or from the Hybrid Connection service portal.</span></span>  <span data-ttu-id="be30c-145">Vi rekommenderar starkt att du använder app-portalen för att skapa Hybridanslutningar som du vill använda med din app.</span><span class="sxs-lookup"><span data-stu-id="be30c-145">It is highly recommended that you use the app portal to create the Hybrid Connections you wish to use with your app.</span></span>  <span data-ttu-id="be30c-146">Om du vill skapa en Hybridanslutning går du till den [Azure-portalen] [ portal] och gå till Gränssnittet för din app.</span><span class="sxs-lookup"><span data-stu-id="be30c-146">To create a Hybrid Connection go to the [Azure portal][portal] and go into the UI for your app.</span></span>  <span data-ttu-id="be30c-147">Välj **nätverk > Konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="be30c-147">Select **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="be30c-148">Här hittar du Hybridanslutningar som är konfigurerade med din app</span><span class="sxs-lookup"><span data-stu-id="be30c-148">From here you can see the Hybrid Connections that are configured with your app</span></span>  

![][2]

<span data-ttu-id="be30c-149">Om du vill lägga till en ny Hybridanslutning, klickar du på Lägg till Hybrid-anslutning.</span><span class="sxs-lookup"><span data-stu-id="be30c-149">To add a new Hybrid Connection, click Add Hybrid Connection.</span></span>  <span data-ttu-id="be30c-150">Användargränssnittet som öppnas visar Hybrid-anslutningar som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="be30c-150">The UI that opens up lists the Hybrid Connections that you have already created.</span></span>  <span data-ttu-id="be30c-151">Om du vill lägga till en eller flera av dem i appen, klicka på de som du vill använda och tryck **Lägg till valda hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="be30c-151">To add one or more of them to your app, click on the ones you want and hit **Add selected hybrid connection**.</span></span>  

![][3]

<span data-ttu-id="be30c-152">Om du vill skapa en ny Hybridanslutning klickar du på **Skapa ny hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="be30c-152">If you want to create a new Hybrid Connection, click **Create new hybrid connection**.</span></span>  <span data-ttu-id="be30c-153">Här anger du det:</span><span class="sxs-lookup"><span data-stu-id="be30c-153">From here you specify the:</span></span> 

- <span data-ttu-id="be30c-154">namnet på slutpunkten</span><span class="sxs-lookup"><span data-stu-id="be30c-154">endpoint name</span></span>
- <span data-ttu-id="be30c-155">slutpunktens värdnamn</span><span class="sxs-lookup"><span data-stu-id="be30c-155">endpoint hostname</span></span>
- <span data-ttu-id="be30c-156">port för slutpunkt</span><span class="sxs-lookup"><span data-stu-id="be30c-156">endpoint port</span></span>
- <span data-ttu-id="be30c-157">servicebus-namnområde som du vill använda</span><span class="sxs-lookup"><span data-stu-id="be30c-157">servicebus namespace you wish to use</span></span>

![][4]

<span data-ttu-id="be30c-158">Varje hybridanslutning är knuten till en service bus-namnrymd och varje service bus-namnrymd är i en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="be30c-158">Every hybrid connection is tied to a service bus namespace and each service bus namespace is in an Azure region.</span></span>  <span data-ttu-id="be30c-159">Det är viktigt att försöka använda en service bus-namnrymd i samma region som din app för att undvika framkallas Nätverksfördröjningen.</span><span class="sxs-lookup"><span data-stu-id="be30c-159">It is important to try and use a service bus namespace in the same region as your app so as to avoid network induced latency.</span></span>

<span data-ttu-id="be30c-160">Om du vill ta bort din hybridanslutning från din app, högerklicka på den och väljer **frånkoppling**.</span><span class="sxs-lookup"><span data-stu-id="be30c-160">If you want to remove your hybrid connection from your app, right click on it and select **Disconnect**.</span></span>  

<span data-ttu-id="be30c-161">När en hybridanslutning läggs till ditt webbprogram, kan du se information på den genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="be30c-161">Once a hybrid connection is added to your web app, you can see details on it by simply clicking on it.</span></span>  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a><span data-ttu-id="be30c-162">Hybridanslutningar och Apptjänstplaner</span><span class="sxs-lookup"><span data-stu-id="be30c-162">Hybrid Connections and App Service Plans</span></span> ##

<span data-ttu-id="be30c-163">Endast hybridanslutningar som du nu är de nya hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="be30c-163">The only hybrid connections you can now make are the new hybrid connections.</span></span>  <span data-ttu-id="be30c-164">De är bara tillgängliga i Basic, Standard, Premium och isolerad priser SKU: er.</span><span class="sxs-lookup"><span data-stu-id="be30c-164">They are only available in Basic, Standard, Premium and Isolated pricing SKUs.</span></span>  <span data-ttu-id="be30c-165">Det finns begränsningar som är knutna till prisplanen.</span><span class="sxs-lookup"><span data-stu-id="be30c-165">There are limits tied to the pricing plan.</span></span>  

| <span data-ttu-id="be30c-166">Priser för planen</span><span class="sxs-lookup"><span data-stu-id="be30c-166">Pricing Plan</span></span> | <span data-ttu-id="be30c-167">Antal hybridanslutningar som kan användas i planen</span><span class="sxs-lookup"><span data-stu-id="be30c-167">Number of hybrid connections usable in the plan</span></span> |
|----|----|
| <span data-ttu-id="be30c-168">Basic</span><span class="sxs-lookup"><span data-stu-id="be30c-168">Basic</span></span> | <span data-ttu-id="be30c-169">5</span><span class="sxs-lookup"><span data-stu-id="be30c-169">5</span></span> |
| <span data-ttu-id="be30c-170">Standard</span><span class="sxs-lookup"><span data-stu-id="be30c-170">Standard</span></span> | <span data-ttu-id="be30c-171">25</span><span class="sxs-lookup"><span data-stu-id="be30c-171">25</span></span> |
| <span data-ttu-id="be30c-172">Premium</span><span class="sxs-lookup"><span data-stu-id="be30c-172">Premium</span></span> | <span data-ttu-id="be30c-173">200</span><span class="sxs-lookup"><span data-stu-id="be30c-173">200</span></span> |
| <span data-ttu-id="be30c-174">Isolerad</span><span class="sxs-lookup"><span data-stu-id="be30c-174">Isolated</span></span> | <span data-ttu-id="be30c-175">200</span><span class="sxs-lookup"><span data-stu-id="be30c-175">200</span></span> |

<span data-ttu-id="be30c-176">Eftersom det finns begränsningar för App Service-Plan finns även Användargränssnittet i i App Service-Plan som visar hur många hybridanslutningar används och vilka appar.</span><span class="sxs-lookup"><span data-stu-id="be30c-176">Since there are App Service Plan restrictions there is also UI in the App Service Plan that shows you how many hybrid connections are being used and by what apps.</span></span>  

![][6]

<span data-ttu-id="be30c-177">Precis som med app-vyn, kan du se information på din hybridanslutning genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="be30c-177">Just as with the app view, you can see details on your hybrid connection by clicking on it.</span></span>  <span data-ttu-id="be30c-178">I egenskaperna som visas här kan se all information som du hade vid vyn appen men kan också se hur många andra appar i den samma App Service-Plan använder hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="be30c-178">In the properties shown here you can see all the information you had at the app view but can also see how many other apps in the same App Service Plan are using that hybrid connection.</span></span>

![][7]

<span data-ttu-id="be30c-179">När det finns en gräns för antalet slutpunkter för hybridanslutning som kan användas i en Apptjänstplan, kan varje hybridanslutning används användas till alla appar i den App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="be30c-179">While there is a limit on the number of hybrid connection endpoints that can be used in an App Service Plan, each hybrid connection used can be used across any number of apps in that App Service Plan.</span></span>  <span data-ttu-id="be30c-180">Dvs om jag hade 1 hybridanslutning som jag använde i 5 separata appar i min App Service-Plan som inte är fortfarande bara 1 hybridanslutning.</span><span class="sxs-lookup"><span data-stu-id="be30c-180">That is to say that if I had 1 hybrid connection that I used in 5 separate apps in my App Service Plan, that is still just 1 hybrid connection.</span></span>

<span data-ttu-id="be30c-181">Det finns en extra kostnad till hybridanslutningar utöver som endast kan användas i en Basic, Standard, Premium eller isolerade SKU.</span><span class="sxs-lookup"><span data-stu-id="be30c-181">There is an additional cost to hybrid connections beyond being only usable in a Basic, Standard, Premium or Isolated SKU.</span></span>  <span data-ttu-id="be30c-182">För information om priser för hybrid anslutning finns här: [priser för Service Bus][sbpricing].</span><span class="sxs-lookup"><span data-stu-id="be30c-182">For details on hybrid connection pricing please go here: [Service Bus pricing][sbpricing].</span></span>

## <a name="hybrid-connection-manager"></a><span data-ttu-id="be30c-183">Hybridanslutningshanterare</span><span class="sxs-lookup"><span data-stu-id="be30c-183">Hybrid Connection Manager</span></span> ##

<span data-ttu-id="be30c-184">Hybridanslutningar ska fungera måste för en relay agent i nätverket som är värd för din hybrid Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="be30c-184">In order for hybrid connections to work you need a relay agent in the network that hosts your hybrid connection endpoint.</span></span>  <span data-ttu-id="be30c-185">Den relay agenten kallas Hybrid anslutning Manager (HCM).</span><span class="sxs-lookup"><span data-stu-id="be30c-185">That relay agent is called the Hybrid Connection Manager (HCM).</span></span>  <span data-ttu-id="be30c-186">Det här verktyget kan hämtas från den **nätverk > Konfigurera slutpunkter för din hybridanslutning** användargränssnitt som är tillgängliga från din app i den [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="be30c-186">This tool can be downloaded from the **Networking > Configure your hybrid connection endpoints** UI available from your app in the [Azure portal][portal].</span></span>  

<span data-ttu-id="be30c-187">Det här verktyget körs på Windows server 2008 R2 och senare versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="be30c-187">This tool runs on Windows server 2008 R2 and later versions of Windows.</span></span>  <span data-ttu-id="be30c-188">När den har installerats HCM körs som en tjänst.</span><span class="sxs-lookup"><span data-stu-id="be30c-188">Once installed the HCM runs as a service.</span></span>  <span data-ttu-id="be30c-189">Den här tjänsten ansluter till Azure servicebus-relä baserat på de konfigurerade slutpunkterna.</span><span class="sxs-lookup"><span data-stu-id="be30c-189">This service connects to Azure servicebus relay based on the configured endpoints.</span></span>  <span data-ttu-id="be30c-190">Anslutningar från HCM är utgående portarna 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="be30c-190">The connections from the HCM are outbound to ports 80 and 443.</span></span>    

<span data-ttu-id="be30c-191">HCM har ett gränssnitt för att konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="be30c-191">The HCM has a UI to configure it.</span></span>  <span data-ttu-id="be30c-192">När du har installerat HCM kan du ta fram Användargränssnittet genom att köra HybridConnectionManagerUi.exe som placeras i installationskatalogen för Hybridanslutningshanteraren.</span><span class="sxs-lookup"><span data-stu-id="be30c-192">After the HCM is installed you can bring up the UI by running the HybridConnectionManagerUi.exe that sits in the Hybrid Connection Manager installation directory.</span></span>  <span data-ttu-id="be30c-193">Uppnås också enkelt på Windows 10 genom att skriva *Hybridanslutningshanteraren UI* i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="be30c-193">It is also easily reached on Windows 10 by typing *Hybrid Connection Manager UI* in your search box.</span></span>  

<span data-ttu-id="be30c-194">När HCM UI startas, är det första du ser en tabell som listar alla hybridanslutningar som är konfigurerade med den här instansen av HCM.</span><span class="sxs-lookup"><span data-stu-id="be30c-194">When the HCM UI is started, the first thing you see is a table that lists all of the hybrid connections that are configured with this instance of the HCM.</span></span>  <span data-ttu-id="be30c-195">Om du vill göra några ändringar som du behöver för att autentisera med Azure.</span><span class="sxs-lookup"><span data-stu-id="be30c-195">If you wish to make any changes you will need to authenticate with Azure.</span></span> 

<span data-ttu-id="be30c-196">Lägga till en eller flera Hybridanslutningar i din HCM:</span><span class="sxs-lookup"><span data-stu-id="be30c-196">To add one or more Hybrid Connections to your HCM:</span></span>

1. <span data-ttu-id="be30c-197">Starta Användargränssnittet för HCM</span><span class="sxs-lookup"><span data-stu-id="be30c-197">Start the HCM UI</span></span>
1. <span data-ttu-id="be30c-198">Klicka på Konfigurera en annan hybridanslutning![][8]</span><span class="sxs-lookup"><span data-stu-id="be30c-198">Click Configure another hybrid connection ![][8]</span></span>

1. <span data-ttu-id="be30c-199">Logga in med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="be30c-199">Sign in with your Azure account</span></span>
1. <span data-ttu-id="be30c-200">Välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="be30c-200">Choose a subscription</span></span>
1. <span data-ttu-id="be30c-201">Klicka på hybridanslutningar som du vill använda den här HCM vidarebefordra![][9]</span><span class="sxs-lookup"><span data-stu-id="be30c-201">Click on the hybrid connections you want this HCM to relay ![][9]</span></span>

1. <span data-ttu-id="be30c-202">Klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="be30c-202">Click Save</span></span>

<span data-ttu-id="be30c-203">Nu visas hybridanslutningar som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="be30c-203">At this point you will see the hybrid connections you added.</span></span>  <span data-ttu-id="be30c-204">Du kan också klicka på den konfigurerade hybridanslutningen och se information om anslutningen.</span><span class="sxs-lookup"><span data-stu-id="be30c-204">You can also click on the configured hybrid connection and see details about the connection.</span></span>

![][10]

<span data-ttu-id="be30c-205">För din HCM för att kunna stödja hybridanslutningar konfigureras den med måste den:</span><span class="sxs-lookup"><span data-stu-id="be30c-205">For your HCM to be able to support the hybrid connections it is configured with, it needs:</span></span>

- <span data-ttu-id="be30c-206">TCP-åtkomst till Azure via portarna 80 och 443</span><span class="sxs-lookup"><span data-stu-id="be30c-206">TCP access to Azure over ports 80 and 443</span></span>
- <span data-ttu-id="be30c-207">TCP-åtkomst till hybrid Anslutningens slutpunkt</span><span class="sxs-lookup"><span data-stu-id="be30c-207">TCP access to the hybrid connection endpoint</span></span>
- <span data-ttu-id="be30c-208">möjligheten att göra DNS sökningar på endpoint-värd och azure servicebus-namnområde</span><span class="sxs-lookup"><span data-stu-id="be30c-208">ability to do DNS look ups on the endpoint host and the azure servicebus namespace</span></span>

<span data-ttu-id="be30c-209">HCM stöder både nya hybridanslutningar samt äldre BizTalk-hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="be30c-209">The HCM supports both new hybrid connections as well as the older BizTalk hybrid connections.</span></span>

### <a name="redundancy"></a><span data-ttu-id="be30c-210">Redundans</span><span class="sxs-lookup"><span data-stu-id="be30c-210">Redundancy</span></span> ###

<span data-ttu-id="be30c-211">Varje HCM kan stödja flera hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="be30c-211">Each HCM can support multiple hybrid connections.</span></span>  <span data-ttu-id="be30c-212">Alla angivna hybridanslutning stöds också av flera HCMs.</span><span class="sxs-lookup"><span data-stu-id="be30c-212">Also, any given hybrid connection can be supported by multiple HCMs.</span></span>  <span data-ttu-id="be30c-213">Standardinställningen är att resursallokering trafik över de konfigurerade HCMs för alla angivna slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="be30c-213">The default behavior is to round robin traffic across the configured HCMs for any given endpoint.</span></span>  <span data-ttu-id="be30c-214">Om du vill hög tillgänglighet på hybridanslutningar från ditt nätverk kan bara skapa en instans av flera HCMs på separata datorer.</span><span class="sxs-lookup"><span data-stu-id="be30c-214">If you want high availability on your hybrid connections from your network, simply instantiate multiple HCMs on separate machines.</span></span> 

### <a name="manually-adding-a-hybrid-connection"></a><span data-ttu-id="be30c-215">Att lägga till en hybridanslutning manuellt</span><span class="sxs-lookup"><span data-stu-id="be30c-215">Manually adding a hybrid connection</span></span> ###

<span data-ttu-id="be30c-216">Om du vill att någon utanför din prenumeration som värd för en HCM-instans för en given hybridanslutning, kan du dela med dem anslutningssträng för gateway för hybridanslutningen.</span><span class="sxs-lookup"><span data-stu-id="be30c-216">If you wish somebody outside of your subscription to host an HCM instance for a given hybrid connection, you can share with them the gateway connection string for the hybrid connection.</span></span>  <span data-ttu-id="be30c-217">Du kan se detta i egenskaperna för en hybridanslutning i den [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="be30c-217">You can see this in the properties for a hybrid connection in the [Azure portal][portal].</span></span> <span data-ttu-id="be30c-218">Om du vill använda denna sträng, klickar du på den **konfigurera manuellt** i HCM och klistra in i anslutningssträngen för gatewayen.</span><span class="sxs-lookup"><span data-stu-id="be30c-218">To use that string, click the **Configure manually** button in the HCM and paste in the gateway connection string.</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="be30c-219">Felsökning</span><span class="sxs-lookup"><span data-stu-id="be30c-219">Troubleshooting</span></span> ##

<span data-ttu-id="be30c-220">När din hybridanslutning ställs in med ett program som körs och det finns minst en HCM som har konfigurerats hybridanslutningen och sedan status står **ansluten** i portalen.</span><span class="sxs-lookup"><span data-stu-id="be30c-220">When your hybrid connection is set up with a running application and there is at least one HCM that has that hybrid connection configured, then the status will say **Connected** in the portal.</span></span>  <span data-ttu-id="be30c-221">Om meddelandet inte anger **ansluten** innebär att din app är nere eller din HCM kan inte ansluta ut till Azure på port 80 eller 443.</span><span class="sxs-lookup"><span data-stu-id="be30c-221">If it does not say **Connected** then it means that either your app is down or your HCM cannot connect out to Azure on ports 80 or 443.</span></span>  

<span data-ttu-id="be30c-222">Det främsta skälet som klienter inte kan ansluta till sina slutpunkten är eftersom slutpunkten angavs med en IP-adress i stället för ett DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="be30c-222">The primary reason that clients cannot connect to their endpoint is because the endpoint was specified using an IP address instead of a DNS name.</span></span>  <span data-ttu-id="be30c-223">Om din app går inte att nå slutpunkten som önskad och används av en IP-adress, växla till med DNS-namnet som är giltig på värden där HCM körs.</span><span class="sxs-lookup"><span data-stu-id="be30c-223">If your app cannot reach the desired endpoint and you used an IP address, switch to using a DNS name that is valid on the host where the HCM is running.</span></span>  <span data-ttu-id="be30c-224">Annat att kontrollera är att DNS-namn matchas korrekt på värden där HCM körs och att det finns en anslutning från värden där HCM körs till hybrid Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="be30c-224">Other things to check are that the DNS name resolves properly on the host where the HCM is running and that there is connectivity from the host where the HCM is running to the hybrid connection endpoint.</span></span>  

<span data-ttu-id="be30c-225">Det är ett verktyg i App Service som kan anropas från konsolen kallas tcpping.</span><span class="sxs-lookup"><span data-stu-id="be30c-225">There is a tool in the App Service that can be invoked from the console called tcpping.</span></span>  <span data-ttu-id="be30c-226">Det här verktyget kan berätta om du har åtkomst till en TCP-slutpunkt men inte berättar om du har åtkomst till en hybrid Anslutningens slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="be30c-226">This tool can tell you if you have access to a TCP endpoint but does not tell you if you have access to a hybrid connection endpoint.</span></span>  <span data-ttu-id="be30c-227">När den används i konsolen mot en hybrid Anslutningens slutpunkt, talar en lyckad ping endast om att du har en hybridanslutning som konfigurerats för din app som använder som värdnamn: port-kombination.</span><span class="sxs-lookup"><span data-stu-id="be30c-227">When used in the console against a hybrid connection endpoint, a successful ping will only tell you that you have a hybrid connection configured for your app that uses that host:port combination.</span></span>  

## <a name="biztalk-hybrid-connections"></a><span data-ttu-id="be30c-228">BizTalks hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="be30c-228">BizTalk Hybrid Connections</span></span> ##

<span data-ttu-id="be30c-229">Den äldre BizTalk Hybridanslutningar kapaciteten har stängts av att ytterligare BizTalk Hybridanslutning skapande.</span><span class="sxs-lookup"><span data-stu-id="be30c-229">The older BizTalk Hybrid Connections capability has been closed off to further BizTalk Hybrid Connection creations.</span></span>  <span data-ttu-id="be30c-230">Du kan fortsätta använda din befintliga BizTalk-Hybridanslutningar med dina appar, men du bör migrera till den nya tjänsten.</span><span class="sxs-lookup"><span data-stu-id="be30c-230">You can continue using your preexisting BizTalk Hybrid Connections with your apps but should migrate to the new service.</span></span>  <span data-ttu-id="be30c-231">Bland fördelarna i den nya tjänsten via BizTalk-versionen finns:</span><span class="sxs-lookup"><span data-stu-id="be30c-231">Among the benefits in the new service over the BizTalk version are:</span></span>

- <span data-ttu-id="be30c-232">Det krävs inga ytterligare BizTalk-konto</span><span class="sxs-lookup"><span data-stu-id="be30c-232">no additional BizTalk account is required</span></span>
- <span data-ttu-id="be30c-233">TLS är 1.2 i stället för 1.0 som BizTalk Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="be30c-233">TLS is 1.2 instead of 1.0 as in BizTalk Hybrid Connections</span></span>
- <span data-ttu-id="be30c-234">Kommunikation är över portarna 80 och 443 med DNS-namnet till andra Azure i stället för IP-adresser och ett antal ytterligare portar.</span><span class="sxs-lookup"><span data-stu-id="be30c-234">Communication is over ports 80 and 443 using a DNS name to reach Azure instead of IP addresses and a range of additional other ports.</span></span>  

<span data-ttu-id="be30c-235">Om du vill lägga till en BizTalk hybridanslutning i appen, gå till din app i den [Azure-portalen] [ portal] och på **nätverk > Konfigurera slutpunkter för din hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="be30c-235">To add a BizTalk hybrid connection to your app, go to your app in the [Azure portal][portal] and click **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="be30c-236">I tabellen klassisk hybrid anslutningar klickar du på **lägga till klassiska hybridanslutning**.</span><span class="sxs-lookup"><span data-stu-id="be30c-236">In the Classic hybrid connections table click **Add classic hybrid connection**.</span></span>  <span data-ttu-id="be30c-237">Här visas en lista över dina BizTalk hybrid-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="be30c-237">From here you will see a list of your BizTalk hybrid connections.</span></span>  


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

