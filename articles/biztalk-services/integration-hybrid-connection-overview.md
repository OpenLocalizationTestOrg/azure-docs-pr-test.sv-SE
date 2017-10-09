---
title: "Översikt över aaaHybrid-anslutningar | Microsoft Docs"
description: "Läs mer om hybridanslutningar, säkerhet, TCP-portar och konfigurationer som stöds. MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: 216e4927-6863-46e7-aa7c-77fec575c8a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: f092c6019aae761e1e73f13d1af8446a896515c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-connections-overview"></a>Översikt över hybridanslutningar

> [!IMPORTANT]
> BizTalk-Hybridanslutningar har dragits tillbaka och ersatts med App Service Hybrid-anslutningar. Mer information, inklusive hur toomanage befintliga BizTalk Hybridanslutningar, se [Hybridanslutningar för Azure App Service](../app-service/app-service-hybrid-connections.md).

Introduktion tooHybrid anslutningar, visar hello stöds konfigurationer och visar hello nödvändiga TCP-portar.

## <a name="what-is-a-hybrid-connection"></a>Vad är en hybridanslutning?
Hybridanslutningar är en funktion i Azure BizTalk Services. Hybridanslutningar ger ett enkelt och praktiskt sätt tooconnect hello funktionen Web Apps i Azure App Service (tidigare webbplatser) och hello Mobile Apps-funktionen i Azure App Service (tidigare för Mobile Services) tooon lokala resurser bakom brandväggen.

![Hybridanslutningar][HCImage]

Fördelarna med hybridanslutningar är:

* Web Apps och Mobile Apps får åtkomst till befintliga lokala data och tjänster på ett säkert sätt.
* Flera Webbappar eller Mobilappar kan dela en Hybridanslutning tooaccess en lokal resurs.
* Minimal TCP-portar är obligatoriska tooaccess nätverket.
* Program med hjälp av Hybridanslutningar åtkomst till endast hello specifika lokala resursen som publiceras via hello Hybridanslutning.
* Ansluta tooany lokala resursen som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er och de flesta anpassade webbtjänster.
  
  > [!NOTE]
  > TCP-baserade tjänster som använder dynamiska portar (till exempel Inaktivt läge för FTP eller Utökat inaktivt läge) stöds inte för närvarande. LDAP stöds inte heller. LDAP använder en statisk TCP-port, men kan också använda UDP. Därför stöds den inte.
  > 
  > 
* Kan användas med alla ramverk som stöds av Web Apps (.NET, PHP, Java, Python, Node.js) och Mobile Apps (Node.js, .NET).
* Webbappar och Mobilappar kan komma åt lokala resurser i hello exakt samma sätt som om hello Web eller Mobilapp finns på nätverket. Till exempel hello samma anslutningssträng som används lokalt kan även användas på Azure.

Hybridanslutningar kan också tillhandahålla enterprise administratörer med kontroll och insyn i hello företagets resurser via hybridprogram, inklusive:

* Använda grupprincipinställningar, Administratörer kan tillåta Hybridanslutningar hello nätverket och också ange resurser som kan användas av hybridprogram.
* Händelsen och granska loggarna hello företagsnätverk ger inblick i hello resurser som används av Hybridanslutningar.

## <a name="example-scenarios"></a>Exempelscenarier
Hybridanslutningar har stöd för följande kombinationer av framework och programmet hello:

* .NET framework åtkomst tooSQL Server
* .NET framework åtkomsttjänster tooHTTP/HTTPS med WebClient
* PHP åtkomst tooSQL Server, MySQL
* Java åtkomst tooSQL Server, MySQL och Oracle
* Java tooHTTP/HTTPS-tjänster

Tänk hello följande när du använder Hybridanslutningar tooaccess lokal SQL Server:

* SQL Express namngivna instanser måste vara konfigurerade toouse statiska portar. Som standard använder namngivna SQL Express-instanser dynamiska portar.
* Standardinstanser i SQL Express använder en statisk port, men TCP måste vara aktiverat. TCP är inte aktiverat som standard.
* När du använder kluster eller Tillgänglighetsgrupper hello `MultiSubnetFailover=true` läge stöds inte för närvarande.
* Hej `ApplicationIntent=ReadOnly` stöds inte för närvarande.
* SQL-autentisering kan krävas som hello slutpunkt till slutpunkt auktoriseringsmetod stöds av hello Azure-program och hello lokal SQLServer.

## <a name="security-and-ports"></a>Säkerhet och portar
Hybridanslutningar använda delade signatur åtkomst (SAS) auktorisering toosecure hello anslutningar från hello Azure program och hello lokal Hybridanslutningshanterare toohello Hybridanslutning. Nycklar för separat anslutning skapas för programmet hello och hello lokal Hybridanslutningshanterare. Anslutningsnycklarna kan återställas och återkallas fristående.

Hybridanslutningar införa sömlös och säker distribution av hello nycklar toohello program och hello lokal Hybridanslutningshanterare.

Se [Skapa och hantera hybridanslutningar](integration-hybrid-connection-create-manage.md).

*Programmet auktorisering skiljer sig från hello Hybridanslutning*. Någon av lämpliga auktoriseringsmetoder kan användas. Hej auktoriseringsmetod beror på hello slutpunkt till slutpunkt auktoriseringsmetoder som stöds i hello Azure-molnet och lokala komponenter hello. Till exempel kan ditt Azure-program ha åtkomst till en lokal SQL Server. I det här scenariot kanske SQL auktorisering hello auktoriseringsmetod som stöds från slutpunkt till slutpunkt.

#### <a name="tcp-ports"></a>TCP-portar
Hybridanslutningar kräver endast utgående TCP- eller HTTP-anslutning från det privata nätverket. Du inte behöver tooopen alla portar i brandväggen eller ändra ditt nätverk perimeter configuration tooallow alla inkommande anslutningar till nätverket.

hello följande TCP-portar som används av Hybridanslutningar:

| Port | Varför du behöver den |
| --- | --- |
| 9350–9354 |Dessa portar används till dataöverföring. hello Service Bus relay manager avsökningar port 9350 toodetermine om TCP-anslutning är tillgänglig. Om den är tillgänglig förutsätts det att även port 9352 är tillgänglig. Datatrafik går via port 9352. <br/><br/>Tillåt utgående anslutningar toothese portar. |
| 5671 |När du använder port 9352 för datatrafik används port 5671 som hello kontrollkanalen. <br/><br/>Tillåt utgående anslutningar toothis port. |
| 80, 443 |Dessa portar används för vissa data begäranden tooAzure. Även om portarna 9352 och 5671 inte kan användas, *sedan* portarna 80 och 443 är hello fallback-portar som används för dataöverföring och hello kontrollkanalen.<br/><br/>Tillåt utgående anslutningar toothese portar. <br/><br/>**Obs** inte bör toouse dessa eftersom hello återställningsplats portar i stället för hello andra TCP-portar. hello HTTP/WebSocket används som hello-protokollet i stället för inbyggd TCP för datakanaler. Det kan leda till lägre prestanda. |

## <a name="next-steps"></a>Nästa steg
[Skapa och hantera hybridanslutningar](integration-hybrid-connection-create-manage.md)<br/>
[Anslut Azure Web Apps tooan lokal resurs](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Ansluta tooon lokal SQL Server från en Azure-webbapp](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>

## <a name="see-also"></a>Se även
[REST API för att hantera BizTalk Services i Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk Services: Diagram över utgåvor](biztalk-editions-feature-chart.md)<br/>
[Skapa en BizTalk-tjänst med Azure-portalen](biztalk-provision-services.md)<br/>
[BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
