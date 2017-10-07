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
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Felsöka försämrad tillstånd på Azure Traffic Manager

Den här artikeln beskriver hur tootroubleshoot en Azure Traffic Manager-profil som visas en status degraderad. Överväg att du har konfigurerat en Traffic Manager-profil pekar toosome cloudapp.net värdbaserade tjänster i det här scenariot. Om hello hälsa din Traffic Manager visar en **degraderad** status och sedan hello status för en eller flera slutpunkter kan vara **degraderad**:

![försämrad slutpunktstillstånd](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Om hello hälsa din Traffic Manager visar en **inaktiv** status båda slutpunkterna kanske att **inaktiverad**:

![Inaktiva Traffic Manager-status](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Förstå Traffic Manager-avsökningar

* Traffic Manager anser att en slutpunkt toobe ONLINE endast när hello avsökningen tar emot ett 200 HTTP-svar tillbaka från hello avsökningen sökväg. Andra icke-200-svaret är ett fel.
* 30 x redirect misslyckas, även om hello omdirigeras URL returnerar en 200.
* Certifikatfel ignoreras för HTTPs-avsökningar.
* hello själva innehållet hello avsökningen sökväg spelar ingen roll, så länge en 200 returneras. Avsökning av en URL toosome statiskt innehåll liknande ”/ favicon.ico” är en vanlig metod. Dynamiskt innehåll, t.ex. hello ASP-sidor kan inte alltid returnerar 200, även om programmet hello är felfritt.
* Ett bra tips är tooset hello avsökningen sökvägen toosomething som har tillräckligt med logik toodetermine som hello plats är uppåt eller nedåt. I hello föregående exempel genom att ange hello sökvägen too"/favicon.ico”, är du bara testar den w3wp.exe svarar. Den här avsökningen kan inte ange att ditt webbprogram är felfri. Ett bättre alternativ skulle vara tooset en sökväg tooa något som ”/ Probe.aspx” som har logik toodetermine hello hälsotillstånd hello plats. Du kan till exempel använda prestandaräknare tooCPU användning eller mått hello antalet misslyckade begäranden. Eller kan du försöker tooaccess databasresurser eller session tillstånd toomake till att hello webbprogrammet fungerar.
* Om alla slutpunkter i en profil är försämrade sedan Traffic Manager behandlar alla slutpunkter som felfri och vägar trafik tooall slutpunkter. Det här beteendet garanteras att problem med sökning mekanism hello inte resulterar i en fullständig avbrott i tjänsten.

## <a name="troubleshooting"></a>Felsökning

tootroubleshoot avsökningen fel, behöver du ett verktyg som visar hello HTTP-statuskod returnerade från hello URL för webbavsökning. Det finns många verktyg som är tillgängliga som visar hello av rådata HTTP-svar.

* [Fiddler](http://www.telerik.com/fiddler)
* [cURL](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Du kan också använda fliken för hello-nätverk av hello F12 felsökningsverktyg i Internet Explorer tooview hello HTTP-svar.

Det här exemplet som vi vill toosee hello svar från våra URL för webbavsökning: http://watestsdp2008r2.cloudapp.net:80/avsökning. hello följande PowerShell-exempel illustrerar hello problem.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Exempel på utdata:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Observera att vi tagit emot ett svar för omdirigering. Som nämnts tidigare anger, alla StatusCode än anses 200 vara fel. Ändringarna i Traffic Manager hello endpoint status tooOffline. tooresolve hello problem, kontrollera hello webbplats configuration tooensure som hello rätt StatusCode kan returneras från hello avsökningen sökväg. Konfigurera om hello Traffic Manager avsökningen toopoint tooa sökväg som returnerar en 200.

Om din avsökningen använder hello HTTPS-protokollet kan behöva toodisable certifikat Kontrollera tooavoid SSL/TLS-fel under testet. hello inaktivera följande PowerShell-satser validering för hello aktuella PowerShell-sessionen:

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

## <a name="next-steps"></a>Nästa steg

[Om Traffic Manager-trafikroutningsmetoder](traffic-manager-routing-methods.md)

[Vad är Traffic Manager](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Åtgärder för Traffic Manager (REST API-referens)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
