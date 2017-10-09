---
title: "aaaVerify Azure Traffic Manager-inställningar | Microsoft Docs"
description: "Den här artikeln hjälper dig att kontrollera din Traffic Manager-inställningar"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 2180b640-596e-4fb2-be59-23a38d606d12
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: c670be6cf55e140c7ab63d5d526de08e14774d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="verify-traffic-manager-settings"></a><span data-ttu-id="18de0-103">Kontrollera inställningarna för Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="18de0-103">Verify Traffic Manager settings</span></span>

<span data-ttu-id="18de0-104">tootest din Traffic Manager-inställningar du behöver toohave flera klienter på olika platser, från vilken du kör dina tester.</span><span class="sxs-lookup"><span data-stu-id="18de0-104">tootest your Traffic Manager settings, you need toohave multiple clients, in various locations, from which you can run your tests.</span></span> <span data-ttu-id="18de0-105">Sedan sätta hello slutpunkter i Traffic Manager-profilen ned en i taget.</span><span class="sxs-lookup"><span data-stu-id="18de0-105">Then, bring hello endpoints in your Traffic Manager profile down one at a time.</span></span>

* <span data-ttu-id="18de0-106">Ange hello DNS TTL-värdet låg så att ändringar sprida snabbt (till exempel 30 sekunder).</span><span class="sxs-lookup"><span data-stu-id="18de0-106">Set hello DNS TTL value low so that changes propagate quickly (for example, 30 seconds).</span></span>
* <span data-ttu-id="18de0-107">Vet hello IP-adresserna för dina Azure-molntjänster och webbplatser i hello-profil som du vill testa.</span><span class="sxs-lookup"><span data-stu-id="18de0-107">Know hello IP addresses of your Azure cloud services and websites in hello profile you are testing.</span></span>
* <span data-ttu-id="18de0-108">Använd verktyg som kan matcha DNS-namn tooan IP-adress och visa den adressen.</span><span class="sxs-lookup"><span data-stu-id="18de0-108">Use tools that let you resolve a DNS name tooan IP address and display that address.</span></span>

<span data-ttu-id="18de0-109">Du kontrollerar toosee hello DNS-namn att matcha tooIP adresser hello slutpunkter i din profil.</span><span class="sxs-lookup"><span data-stu-id="18de0-109">You are checking toosee that hello DNS names resolve tooIP addresses of hello endpoints in your profile.</span></span> <span data-ttu-id="18de0-110">hello namn bör lösas i enlighet med hello trafikroutningsmetod definieras i hello Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="18de0-110">hello names should resolve in a manner consistent with hello traffic routing method defined in hello Traffic Manager profile.</span></span> <span data-ttu-id="18de0-111">Du kan använda hello verktyg som **nslookup** eller **gräva** tooresolve DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="18de0-111">You can use hello tools like **nslookup** or **dig** tooresolve DNS names.</span></span>

<span data-ttu-id="18de0-112">hello kan följande exempel du testa Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="18de0-112">hello following examples help you test your Traffic Manager profile.</span></span>

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a><span data-ttu-id="18de0-113">Kontrollera Traffic Manager-profil med nslookup och ipconfig i Windows</span><span class="sxs-lookup"><span data-stu-id="18de0-113">Check Traffic Manager profile using nslookup and ipconfig in Windows</span></span>

1. <span data-ttu-id="18de0-114">Öppna ett kommando eller en Windows PowerShell-Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="18de0-114">Open a command or Windows PowerShell prompt as an administrator.</span></span>
2. <span data-ttu-id="18de0-115">Typen `ipconfig /flushdns` tooflush hello cacheminnet för DNS-matchare.</span><span class="sxs-lookup"><span data-stu-id="18de0-115">Type `ipconfig /flushdns` tooflush hello DNS resolver cache.</span></span>
3. <span data-ttu-id="18de0-116">Skriv `nslookup <your Traffic Manager domain name>`.</span><span class="sxs-lookup"><span data-stu-id="18de0-116">Type `nslookup <your Traffic Manager domain name>`.</span></span> <span data-ttu-id="18de0-117">Hej exempelvis följande kommando kontrollerar hello domännamn med hello prefix *myapp.contoso*</span><span class="sxs-lookup"><span data-stu-id="18de0-117">For example, hello following command checks hello domain name with hello prefix *myapp.contoso*</span></span>

        nslookup myapp.contoso.trafficmanager.net

    <span data-ttu-id="18de0-118">En typisk resultatet visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="18de0-118">A typical result shows hello following information:</span></span>

    + <span data-ttu-id="18de0-119">hello DNS-namn och IP-adressen för hello DNS-server som åt tooresolve det här domännamnet för Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="18de0-119">hello DNS name and IP address of hello DNS server being accessed tooresolve this Traffic Manager domain name.</span></span>
    + <span data-ttu-id="18de0-120">hello Traffic Manager-domännamn du angav på kommandoraden för hello efter ”nslookup” och löser hello IP-adress toowhich hello Traffic Manager-domän.</span><span class="sxs-lookup"><span data-stu-id="18de0-120">hello Traffic Manager domain name you typed on hello command line after "nslookup" and hello IP address toowhich hello Traffic Manager domain resolves.</span></span> <span data-ttu-id="18de0-121">hello andra IP-adressen är hello viktiga en toocheck.</span><span class="sxs-lookup"><span data-stu-id="18de0-121">hello second IP address is hello important one toocheck.</span></span> <span data-ttu-id="18de0-122">Den måste matcha en offentlig virtuella IP-adresser (VIP)-adress för en av hello molntjänster eller webbplatser i hello Traffic Manager-profil som du vill testa.</span><span class="sxs-lookup"><span data-stu-id="18de0-122">It should match a public virtual IP (VIP) address for one of hello cloud services or websites in hello Traffic Manager profile you are testing.</span></span>

## <a name="how-tootest-hello-failover-traffic-routing-method"></a><span data-ttu-id="18de0-123">Hur tootest hello redundans trafikroutningsmetoden</span><span class="sxs-lookup"><span data-stu-id="18de0-123">How tootest hello failover traffic routing method</span></span>

1. <span data-ttu-id="18de0-124">Lämna alla slutpunkter upp.</span><span class="sxs-lookup"><span data-stu-id="18de0-124">Leave all endpoints up.</span></span>
2. <span data-ttu-id="18de0-125">Med en enda klient begär DNS-matchning för företagets domännamn med hjälp av nslookup eller en liknande funktion.</span><span class="sxs-lookup"><span data-stu-id="18de0-125">Using a single client, request DNS resolution for your company domain name using nslookup or a similar utility.</span></span>
3. <span data-ttu-id="18de0-126">Se till att hello matcha IP-adressen matchar hello primära slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="18de0-126">Ensure that hello resolved IP address matches hello primary endpoint.</span></span>
4. <span data-ttu-id="18de0-127">Avslutar primära slutpunkten eller ta bort hello övervakning filen så att Traffic Manager tror som hello programmet inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="18de0-127">Bring down your primary endpoint or remove hello monitoring file so that Traffic Manager thinks that hello application is down.</span></span>
5. <span data-ttu-id="18de0-128">Vänta tills hello DNS-Time-to-Live (TTL) av hello Traffic Manager-profil plus en ytterligare två minuter.</span><span class="sxs-lookup"><span data-stu-id="18de0-128">Wait for hello DNS Time-to-Live (TTL) of hello Traffic Manager profile plus an additional two minutes.</span></span> <span data-ttu-id="18de0-129">Till exempel måste din DNS-TTL är 300 sekunder (fem minuter), du vänta tills sju minuter.</span><span class="sxs-lookup"><span data-stu-id="18de0-129">For example, if your DNS TTL is 300 seconds (5 minutes), you must wait for seven minutes.</span></span>
6. <span data-ttu-id="18de0-130">Rensa din DNS-klienten cache och begäran DNS-matchning med nslookup.</span><span class="sxs-lookup"><span data-stu-id="18de0-130">Flush your DNS client cache and request DNS resolution using nslookup.</span></span> <span data-ttu-id="18de0-131">I Windows kan tömma du DNS-cache med hello ipconfig/flushdns kommando.</span><span class="sxs-lookup"><span data-stu-id="18de0-131">In Windows, you can flush your DNS cache with hello ipconfig /flushdns command.</span></span>
7. <span data-ttu-id="18de0-132">Se till att hello matcha IP-adressen matchar din sekundära slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="18de0-132">Ensure that hello resolved IP address matches your secondary endpoint.</span></span>
8. <span data-ttu-id="18de0-133">Upprepa hello-processen, som i sin tur att stänga av varje slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="18de0-133">Repeat hello process, bringing down each endpoint in turn.</span></span> <span data-ttu-id="18de0-134">Kontrollera att hello DNS returnerar hello IP-adressen för nästa hello-slutpunkt i hello lista.</span><span class="sxs-lookup"><span data-stu-id="18de0-134">Verify that hello DNS returns hello IP address of hello next endpoint in hello list.</span></span> <span data-ttu-id="18de0-135">När alla slutpunkter är nere, bör du hämta hello IP-adressen för primär slutpunkt för hello igen.</span><span class="sxs-lookup"><span data-stu-id="18de0-135">When all endpoints are down, you should obtain hello IP address of hello primary endpoint again.</span></span>

## <a name="how-tootest-hello-weighted-traffic-routing-method"></a><span data-ttu-id="18de0-136">Hur tootest hello viktas trafikroutningsmetod</span><span class="sxs-lookup"><span data-stu-id="18de0-136">How tootest hello weighted traffic routing method</span></span>

1. <span data-ttu-id="18de0-137">Lämna alla slutpunkter upp.</span><span class="sxs-lookup"><span data-stu-id="18de0-137">Leave all endpoints up.</span></span>
2. <span data-ttu-id="18de0-138">Med en enda klient begär DNS-matchning för företagets domännamn med hjälp av nslookup eller en liknande funktion.</span><span class="sxs-lookup"><span data-stu-id="18de0-138">Using a single client, request DNS resolution for your company domain name using nslookup or a similar utility.</span></span>
3. <span data-ttu-id="18de0-139">Se till att hello matcha IP-adressen matchar ett av dina slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="18de0-139">Ensure that hello resolved IP address matches one of your endpoints.</span></span>
4. <span data-ttu-id="18de0-140">Rensa din DNS-klientens cacheminne och upprepa steg 2 och 3 för varje slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="18de0-140">Flush your DNS client cache and repeat steps 2 and 3 for each endpoint.</span></span> <span data-ttu-id="18de0-141">Du bör se olika IP-adresser som returneras för var och en av dina slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="18de0-141">You should see different IP addresses returned for each of your endpoints.</span></span>

## <a name="how-tootest-hello-performance-traffic-routing-method"></a><span data-ttu-id="18de0-142">Hur tootest hello prestanda trafikroutningsmetoden</span><span class="sxs-lookup"><span data-stu-id="18de0-142">How tootest hello performance traffic routing method</span></span>

<span data-ttu-id="18de0-143">tooeffectively testa en routningsmetoden för prestanda-trafik, måste du ha klienter som finns i olika delar av hello world.</span><span class="sxs-lookup"><span data-stu-id="18de0-143">tooeffectively test a performance traffic routing method, you must have clients located in different parts of hello world.</span></span> <span data-ttu-id="18de0-144">Du kan skapa klienter i olika Azure-regioner som kan använda tootest dina tjänster.</span><span class="sxs-lookup"><span data-stu-id="18de0-144">You can create clients in different Azure regions that can be used tootest your services.</span></span> <span data-ttu-id="18de0-145">Om du har ett globalt nätverk kan du via fjärranslutning logga in tooclients i andra delar av hello world och köra dina tester därifrån.</span><span class="sxs-lookup"><span data-stu-id="18de0-145">If you have a global network, you can remotely sign in tooclients in other parts of hello world and run your tests from there.</span></span>

<span data-ttu-id="18de0-146">Du kan också finns kostnadsfria webbaserade DNS-sökning och gräva tjänster som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="18de0-146">Alternatively, there are free web-based DNS lookup and dig services available.</span></span> <span data-ttu-id="18de0-147">Några av dessa verktyg ger du hello möjlighet toocheck DNS-namnmatchningen från olika platser runt hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="18de0-147">Some of these tools give you hello ability toocheck DNS name resolution from various locations around hello world.</span></span> <span data-ttu-id="18de0-148">Gör en sökning efter ”DNS-sökning” exempel.</span><span class="sxs-lookup"><span data-stu-id="18de0-148">Do a search on "DNS lookup" for examples.</span></span> <span data-ttu-id="18de0-149">Tjänster från tredje part som Gomez eller presentation kan vara används tooconfirm att dina profiler distribuerar trafik som förväntat.</span><span class="sxs-lookup"><span data-stu-id="18de0-149">Third-party services like Gomez or Keynote can be used tooconfirm that your profiles are distributing traffic as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18de0-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18de0-150">Next steps</span></span>

* [<span data-ttu-id="18de0-151">Om Traffic Manager-trafikroutningsmetoder</span><span class="sxs-lookup"><span data-stu-id="18de0-151">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="18de0-152">Prestandaöverväganden för Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="18de0-152">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
* [<span data-ttu-id="18de0-153">Felsök degraderat tillstånd i Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="18de0-153">Troubleshooting Traffic Manager degraded state</span></span>](traffic-manager-troubleshooting-degraded.md)
