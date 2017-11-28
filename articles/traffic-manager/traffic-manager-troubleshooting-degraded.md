---
title: "aaaTroubleshooting försämrad status på Azure Traffic Manager"
description: "Hur tootroubleshoot Traffic Manager-profiler när den visas som försämrad status."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
ms.assetid: 8af0433d-e61b-4761-adcc-7bc9b8142fc6
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: kumud
ms.openlocfilehash: fd95697781472b52e98d856e66beb7b89dfeaf23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a><span data-ttu-id="a316c-103">Felsöka försämrad tillstånd på Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="a316c-103">Troubleshooting degraded state on Azure Traffic Manager</span></span>

<span data-ttu-id="a316c-104">Den här artikeln beskriver hur tootroubleshoot en Azure Traffic Manager-profil som visas en status degraderad.</span><span class="sxs-lookup"><span data-stu-id="a316c-104">This article describes how tootroubleshoot an Azure Traffic Manager profile that is showing a degraded status.</span></span> <span data-ttu-id="a316c-105">Överväg att du har konfigurerat en Traffic Manager-profil pekar toosome cloudapp.net värdbaserade tjänster i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="a316c-105">For this scenario, consider that you have configured a Traffic Manager profile pointing toosome of your cloudapp.net hosted services.</span></span> <span data-ttu-id="a316c-106">Om hello hälsa din Traffic Manager visar en **degraderad** status och sedan hello status för en eller flera slutpunkter kan vara **degraderad**:</span><span class="sxs-lookup"><span data-stu-id="a316c-106">If hello health of your Traffic Manager displays a **Degraded** status, then hello status of one or more endpoints may be **Degraded**:</span></span>

![försämrad slutpunktstillstånd](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

<span data-ttu-id="a316c-108">Om hello hälsa din Traffic Manager visar en **inaktiv** status båda slutpunkterna kanske att **inaktiverad**:</span><span class="sxs-lookup"><span data-stu-id="a316c-108">If hello health of your Traffic Manager displays an **Inactive** status, then both end points may be **Disabled**:</span></span>

![Inaktiva Traffic Manager-status](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a><span data-ttu-id="a316c-110">Förstå Traffic Manager-avsökningar</span><span class="sxs-lookup"><span data-stu-id="a316c-110">Understanding Traffic Manager probes</span></span>

* <span data-ttu-id="a316c-111">Traffic Manager anser att en slutpunkt toobe ONLINE endast när hello avsökningen tar emot ett 200 HTTP-svar tillbaka från hello avsökningen sökväg.</span><span class="sxs-lookup"><span data-stu-id="a316c-111">Traffic Manager considers an endpoint toobe ONLINE only when hello probe receives an HTTP 200 response back from hello probe path.</span></span> <span data-ttu-id="a316c-112">Andra icke-200-svaret är ett fel.</span><span class="sxs-lookup"><span data-stu-id="a316c-112">Any other non-200 response is a failure.</span></span>
* <span data-ttu-id="a316c-113">30 x redirect misslyckas, även om hello omdirigeras URL returnerar en 200.</span><span class="sxs-lookup"><span data-stu-id="a316c-113">A 30x redirect fails, even if hello redirected URL returns a 200.</span></span>
* <span data-ttu-id="a316c-114">Certifikatfel ignoreras för HTTPs-avsökningar.</span><span class="sxs-lookup"><span data-stu-id="a316c-114">For HTTPs probes, certificate errors are ignored.</span></span>
* <span data-ttu-id="a316c-115">hello själva innehållet hello avsökningen sökväg spelar ingen roll, så länge en 200 returneras.</span><span class="sxs-lookup"><span data-stu-id="a316c-115">hello actual content of hello probe path doesn't matter, as long as a 200 is returned.</span></span> <span data-ttu-id="a316c-116">Avsökning av en URL toosome statiskt innehåll liknande ”/ favicon.ico” är en vanlig metod.</span><span class="sxs-lookup"><span data-stu-id="a316c-116">Probing a URL toosome static content like "/favicon.ico" is a common technique.</span></span> <span data-ttu-id="a316c-117">Dynamiskt innehåll, t.ex. hello ASP-sidor kan inte alltid returnerar 200, även om programmet hello är felfritt.</span><span class="sxs-lookup"><span data-stu-id="a316c-117">Dynamic content, like hello ASP pages, may not always return 200, even when hello application is healthy.</span></span>
* <span data-ttu-id="a316c-118">Ett bra tips är tooset hello avsökningen sökvägen toosomething som har tillräckligt med logik toodetermine som hello plats är uppåt eller nedåt.</span><span class="sxs-lookup"><span data-stu-id="a316c-118">A best practice is tooset hello Probe path toosomething that has enough logic toodetermine that hello site is up or down.</span></span> <span data-ttu-id="a316c-119">I hello föregående exempel genom att ange hello sökvägen too"/favicon.ico”, är du bara testar den w3wp.exe svarar.</span><span class="sxs-lookup"><span data-stu-id="a316c-119">In hello previous example, by setting hello path too"/favicon.ico", you are only testing that w3wp.exe is responding.</span></span> <span data-ttu-id="a316c-120">Den här avsökningen kan inte ange att ditt webbprogram är felfri.</span><span class="sxs-lookup"><span data-stu-id="a316c-120">This probe may not indicate that your web application is healthy.</span></span> <span data-ttu-id="a316c-121">Ett bättre alternativ skulle vara tooset en sökväg tooa något som ”/ Probe.aspx” som har logik toodetermine hello hälsotillstånd hello plats.</span><span class="sxs-lookup"><span data-stu-id="a316c-121">A better option would be tooset a path tooa something such as "/Probe.aspx" that has logic toodetermine hello health of hello site.</span></span> <span data-ttu-id="a316c-122">Du kan till exempel använda prestandaräknare tooCPU användning eller mått hello antalet misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="a316c-122">For example, you could use performance counters tooCPU utilization or measure hello number of failed requests.</span></span> <span data-ttu-id="a316c-123">Eller kan du försöker tooaccess databasresurser eller session tillstånd toomake till att hello webbprogrammet fungerar.</span><span class="sxs-lookup"><span data-stu-id="a316c-123">Or you could attempt tooaccess database resources or session state toomake sure that hello web application is working.</span></span>
* <span data-ttu-id="a316c-124">Om alla slutpunkter i en profil är försämrade sedan Traffic Manager behandlar alla slutpunkter som felfri och vägar trafik tooall slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a316c-124">If all endpoints in a profile are degraded, then Traffic Manager treats all endpoints as healthy and routes traffic tooall endpoints.</span></span> <span data-ttu-id="a316c-125">Det här beteendet garanteras att problem med sökning mekanism hello inte resulterar i en fullständig avbrott i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a316c-125">This behavior ensures that problems with hello probing mechanism do not result in a complete outage of your service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="a316c-126">Felsökning</span><span class="sxs-lookup"><span data-stu-id="a316c-126">Troubleshooting</span></span>

<span data-ttu-id="a316c-127">tootroubleshoot avsökningen fel, behöver du ett verktyg som visar hello HTTP-statuskod returnerade från hello URL för webbavsökning.</span><span class="sxs-lookup"><span data-stu-id="a316c-127">tootroubleshoot a probe failure, you need a tool that shows hello HTTP status code return from hello probe URL.</span></span> <span data-ttu-id="a316c-128">Det finns många verktyg som är tillgängliga som visar hello av rådata HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="a316c-128">There are many tools available that show you hello raw HTTP response.</span></span>

* [<span data-ttu-id="a316c-129">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a316c-129">Fiddler</span></span>](http://www.telerik.com/fiddler)
* [<span data-ttu-id="a316c-130">cURL</span><span class="sxs-lookup"><span data-stu-id="a316c-130">curl</span></span>](https://curl.haxx.se/)
* [<span data-ttu-id="a316c-131">wget</span><span class="sxs-lookup"><span data-stu-id="a316c-131">wget</span></span>](http://gnuwin32.sourceforge.net/packages/wget.htm)

<span data-ttu-id="a316c-132">Du kan också använda fliken för hello-nätverk av hello F12 felsökningsverktyg i Internet Explorer tooview hello HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="a316c-132">Also, you can use hello Network tab of hello F12 Debugging Tools in Internet Explorer tooview hello HTTP responses.</span></span>

<span data-ttu-id="a316c-133">Det här exemplet som vi vill toosee hello svar från våra URL för webbavsökning: http://watestsdp2008r2.cloudapp.net:80/avsökning.</span><span class="sxs-lookup"><span data-stu-id="a316c-133">For this example we want toosee hello response from our probe URL: http://watestsdp2008r2.cloudapp.net:80/Probe.</span></span> <span data-ttu-id="a316c-134">hello följande PowerShell-exempel illustrerar hello problem.</span><span class="sxs-lookup"><span data-stu-id="a316c-134">hello following PowerShell example illustrates hello problem.</span></span>

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

<span data-ttu-id="a316c-135">Exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="a316c-135">Example output:</span></span>

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

<span data-ttu-id="a316c-136">Observera att vi tagit emot ett svar för omdirigering.</span><span class="sxs-lookup"><span data-stu-id="a316c-136">Notice that we received a redirect response.</span></span> <span data-ttu-id="a316c-137">Som nämnts tidigare anger, alla StatusCode än anses 200 vara fel.</span><span class="sxs-lookup"><span data-stu-id="a316c-137">As stated previously, any StatusCode other than 200 is considered a failure.</span></span> <span data-ttu-id="a316c-138">Ändringarna i Traffic Manager hello endpoint status tooOffline.</span><span class="sxs-lookup"><span data-stu-id="a316c-138">Traffic Manager changes hello endpoint status tooOffline.</span></span> <span data-ttu-id="a316c-139">tooresolve hello problem, kontrollera hello webbplats configuration tooensure som hello rätt StatusCode kan returneras från hello avsökningen sökväg.</span><span class="sxs-lookup"><span data-stu-id="a316c-139">tooresolve hello problem, check hello website configuration tooensure that hello proper StatusCode can be returned from hello probe path.</span></span> <span data-ttu-id="a316c-140">Konfigurera om hello Traffic Manager avsökningen toopoint tooa sökväg som returnerar en 200.</span><span class="sxs-lookup"><span data-stu-id="a316c-140">Reconfigure hello Traffic Manager probe toopoint tooa path that returns a 200.</span></span>

<span data-ttu-id="a316c-141">Om din avsökningen använder hello HTTPS-protokollet kan behöva toodisable certifikat Kontrollera tooavoid SSL/TLS-fel under testet.</span><span class="sxs-lookup"><span data-stu-id="a316c-141">If your probe is using hello HTTPS protocol, you may need toodisable certificate checking tooavoid SSL/TLS errors during your test.</span></span> <span data-ttu-id="a316c-142">hello inaktivera följande PowerShell-satser validering för hello aktuella PowerShell-sessionen:</span><span class="sxs-lookup"><span data-stu-id="a316c-142">hello following PowerShell statements disable certificate validation for hello current PowerShell session:</span></span>

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a><span data-ttu-id="a316c-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a316c-143">Next Steps</span></span>

[<span data-ttu-id="a316c-144">Om Traffic Manager-trafikroutningsmetoder</span><span class="sxs-lookup"><span data-stu-id="a316c-144">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)

[<span data-ttu-id="a316c-145">Vad är Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="a316c-145">What is Traffic Manager</span></span>](traffic-manager-overview.md)

[<span data-ttu-id="a316c-146">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="a316c-146">Cloud Services</span></span>](http://go.microsoft.com/fwlink/?LinkId=314074)

[<span data-ttu-id="a316c-147">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="a316c-147">Azure Web Apps</span></span>](https://azure.microsoft.com/documentation/services/app-service/web/)

[<span data-ttu-id="a316c-148">Åtgärder för Traffic Manager (REST API-referens)</span><span class="sxs-lookup"><span data-stu-id="a316c-148">Operations on Traffic Manager (REST API Reference)</span></span>](http://go.microsoft.com/fwlink/?LinkId=313584)

<span data-ttu-id="a316c-149">[Azure Traffic Manager-Cmdlets][1]</span><span class="sxs-lookup"><span data-stu-id="a316c-149">[Azure Traffic Manager Cmdlets][1]</span></span>

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
