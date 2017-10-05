---
title: "Azure App Service och dess påverkan på befintliga Azure-tjänster"
description: "Beskriver hur den nya Azure App Service och dess funktioner påverkar befintliga tjänster i Azure."
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
ms.openlocfilehash: ed967fda7a216ed49532be54228ebe888cf16b6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service och befintliga Azure-tjänster
Den här artikeln beskrivs ändringar av befintliga Azure-tjänster som en del av att ändra samordnar flera Azure-tjänster i [Azure App Service](https://azure.microsoft.com/services/app-service/), ett nytt integrerade erbjudande.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Översikt
[Azure Apptjänst](https://azure.microsoft.com/services/app-service/) är en nya och unika molnbaserad tjänst som gör att utvecklare kan skapa webb- och mobilappar för alla plattformar och alla enheter. Apptjänst är en integrerad lösning som utformats för att förenkla upprepade kodning funktioner, integrera med enterprise och SaaS-system och automatisera processer och uppfyller dina behov för säkerhet, tillförlitlighet och skalbarhet.

Apptjänst sammanför följande befintliga Azure-tjänster - [webbplatser](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), och [Biztalk-tjänst](https://azure.microsoft.com/services/biztalk-services/) till en enda kombineras tjänsten, när du lägger till kraftfulla nya funktioner.  Apptjänst kan du vara värd för följande typer:

* Web Apps
* Mobile Apps
* API Apps
* Logic Apps

I följande tabell beskrivs hur Azure-tjänster karta till App Service och apptyper som är tillgängliga i den.

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
<td align="left"><li>Apptjänst är begränsas till att ändra namnet på webbplatser för Web Apps för Azure Websites.
<p><li>Alla befintliga instanser av webbplatser är nu Web Apps i Apptjänst.</p>
<p><li>Du kan komma åt dina befintliga webbplatser via den <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, där du hittar alla befintliga platser under <em>Web Apps</em>.</p>
<p><li><em>Web värd planera</em> är nu <em>Apptjänstplan</em>. En <em>Apptjänstplan</em> kan vara värd för någon typ av app av App Service, till exempel webb, mobil, logik eller API apps.</p>
<p><li>Azure App Service Web Apps är i allmänhet tillgänglighet.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Lär dig mer om Web Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile Services</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>Mobiltjänster fortsätter att vara tillgänglig som en fristående tjänst och förblir fullt stöd.</p>
<p><li>Mobile Apps är en typ av app i App Service som integrerar alla funktionerna i Mobile Services och mycket mer.</p>
<p><li>Det är lätt att <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrera från Mobile Services till Mobile Apps</a>.</p>
<p><li>Som en del av Apptjänst hämta Mobile Apps nya funktioner utöver Mobile Services, till exempel integrering med lokala och SaaS-system, mellanlagring kortplatser, WebJobs, bättre skalningsalternativ och mycket mer.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Mer information om Mobile Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API Apps är en ny typ av app i App Service som gör att du enkelt skapa och använda API: er i molnet.</p>
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
<li><p>BizTalk-tjänster fortsätter att vara tillgänglig som en fristående tjänst och förblir fullt stöd.</p>
<li><p>Alla funktioner i BizTalk-tjänst är integrerade i App Service som API Apps att användare kan utföra integration av företagsprogram och B2B integrationsscenarier med typer av app i App Service.</p>
<li><p>Du kan nu automatisera affärsprocesser med en visuell design-upplevelse för att skapa arbetsflöden med Logic Apps.</p></td>
</tr>
</tbody>
</table>

Mer information finns [Apptjänst dokumentationen](https://azure.microsoft.com/documentation/services/app-service/).

