---
title: "aaaAzure App Service och dess påverkan på befintliga Azure-tjänster"
description: "Förklarar hur hello nya Azure App Service och dess funktioner påverkar befintliga tjänster i Azure."
services: app-service
documentationcenter: 
author: yochay
manager: nirma
editor: 
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: yochaykk
ms.openlocfilehash: a831a88fee38465e5b0b7c2c2340cf8a0d64c864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service och befintliga Azure-tjänster
Den här artikeln beskrivs hello ändringar tooexisting Azure services som en del av hello ändra toobring tillsammans flera Azure-tjänster i [Azure App Service](https://azure.microsoft.com/services/app-service/), ett nytt integrerade erbjudande.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Översikt
[Azure Apptjänst](https://azure.microsoft.com/services/app-service/) är en nya och unika molnbaserad tjänst som gör att utvecklare toocreate webb- och mobilappar för alla plattformar och alla enheter. Apptjänst är en integrerad lösning utformad toostreamline upprepas kodning funktioner, integrera med enterprise och SaaS-system och automatisera processer och uppfyller dina behov för säkerhet, tillförlitlighet och skalbarhet.

Apptjänst sammanför hello följa befintliga Azure-tjänster - [webbplatser](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), och [Biztalk-tjänst](https://azure.microsoft.com/services/biztalk-services/) kombineras till en enda tjänsten, medan lägga till kraftfulla nya funktioner.  Apptjänst kan du toohost hello följande typer:

* Web Apps
* Mobile Apps
* API Apps
* Logic Apps

hello i tabellen nedan beskrivs hur befintliga Azure services mappa tooApp tjänsten och hello app som är tillgängliga i den.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Befintlig Azure-tjänst</th>
<th align="left", style="width:10%">Azure App Service</th>
<th align="left", style="width:80%">Detta har ändrats</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure Websites</td>
<td align="left">Web Apps</td>
<td align="left"><li>Apptjänst är strikt begränsat toochanging hello namn webbplatser tooWeb appar för Azure Websites.
<p><li>Alla befintliga instanser av webbplatser är nu Web Apps i Apptjänst.</p>
<p><li>Du kan komma åt dina befintliga webbplatser via hello <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, där du hittar alla befintliga platser under <em>Web Apps</em>.</p>
<p><li><em>Web värd planera</em> är nu <em>Apptjänstplan</em>. En <em>Apptjänstplan</em> kan vara värd för någon typ av app av App Service, till exempel webb, mobil, logik eller API apps.</p>
<p><li>Azure App Service Web Apps är i allmänhet tillgänglighet.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Lär dig mer om Web Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile Services</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>Mobile Services fortsätta toobe som är tillgängliga som en fristående tjänst och förblir fullt stöd.</p>
<p><li>Mobile Apps är en typ av app i App Service som integrerar hello funktioner för Mobile Services med mera.</p>
<p><li>Det är enkelt för<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrera från Mobile Services tooMobile appar</a>.</p>
<p><li>Som en del av Apptjänst hämta Mobile Apps nya funktioner utöver Mobile Services, till exempel integrering med lokala och SaaS-system, mellanlagring kortplatser, WebJobs, bättre skalningsalternativ och mycket mer.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Mer information om Mobile Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API Apps är en ny typ av app i App Service som gör att du enkelt skapa och använda API: er i hello molnet.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Mer information om API Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logic Apps</td>
<td align="left">
<p><li>Logic Apps är en ny typ av app i App Service som gör att du enkelt automatisera affärsprocesser.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Mer information om Logic Apps</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk Services</td>
<td align="left">BizTalk API Apps</td>
<td align="left">
<li><p>BizTalk-tjänst fortsätta toobe som är tillgängliga som en fristående tjänst och förblir fullt stöd.</p>
<li><p>Alla hello-funktionerna i BizTalk-tjänst är integrerade i Apptjänst API Apps aktiverar användare tooperform integration av företagsprogram och B2B integrationsscenarier med någon av hello apptyper i Apptjänst.</p>
<li><p>Du kan nu automatisera affärsprocesser med hjälp av en visuell design upplevelse toocreate arbetsflöden med Logic Apps.</p></td>
</tr>
</tbody>
</table>

toolearn fler besök [Apptjänst dokumentationen](https://azure.microsoft.com/documentation/services/app-service/).

