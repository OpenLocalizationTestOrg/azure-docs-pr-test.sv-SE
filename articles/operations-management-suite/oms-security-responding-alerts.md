---
title: "aaaMonitoring och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösningen | Microsoft Docs"
description: "Det här dokumentet hjälper du toouse hello hot intelligence alternativ som finns i OMS-säkerhet och granska toomonitor och svara toosecurity aviseringar."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: 3d92b6809b7bd934c889afc119e5e34ff2b85f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-responding-toosecurity-alerts-in-operations-management-suite-security-and-audit-solution"></a>Övervaka och svara toosecurity varningar i Operations Management Suite säkerhet och granska lösningen
Det här dokumentet hjälper dig att använda hello hot intelligence alternativ som finns i OMS-säkerhet och granska toomonitor och svara toosecurity aviseringar.

## <a name="what-is-oms"></a>Vad är OMS?
Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur. Mer information om OMS artikeln hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Hotinformation
I en företagsmiljö där användarna har bred åtkomst toohello nätverk och använda en mängd olika enheter tooconnect toocorporate data, är det viktigt att du kan övervaka dina resurser aktivt och snabbt svara toosecurity incidenter. Detta är särskilt viktigt från hello säkerhetsperspektiv livscykel eftersom vissa cybersecurity hot inte kan generera aviseringar eller misstänkta aktiviteter som kan identifieras av traditionella tekniska säkerhetsåtgärder. 

Med hjälp av hello **Hotinformation** alternativet finns i OMS-säkerhet och granskning, IT-administratörer kan identifiera säkerhetshot mot hello-miljön, till exempel, identifiera om en viss dator är en del av en [ botnät](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Datorer kan bli noder i ett botnät när angripare otillåtet sätt installera skadlig kod som någon gång ansluter den här datorn toohello kommando och kontroll. Det kan även identifiera potentiella hot som kommer från lagringsutrymmen kommunikationskanaler som [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

I ordning toobuild använder den här hotinformation OMS säkerhet och granska data som kommer från olika källor i Microsoft. OMS säkerhet och granska använder den här data tooidentify potentiella hot mot din miljö.

Hej Hotinformation rutan består av tre huvudsakliga alternativ:

* Servrar med utgående skadlig trafik
* Typer av identifierade hot
* Karta för hotinformation

> [!NOTE]
> en översikt över alla dessa alternativ, läsa [komma igång med Operations Management Suite säkerhet och granska lösningen](oms-security-getting-started.md).
> 
> 

### <a name="responding-toosecurity-alerts"></a>Svara toosecurity aviseringar
En av hello stegen i en [säkerhet incidenter](https://technet.microsoft.com/library/cc512623.aspx) processen är tooidentify hello allvarlighetsgraden hello röjande system. I det här steget ska du utföra hello följande uppgifter:

* Fastställa hello uppbyggnad hello-attack
* Fastställa hello angreppspunkt för ursprung
* Fastställa hello syftet med hello-attack. Var hello attack specifikt riktat mot din organisation tooacquire information eller var den slumpmässiga?
* Identifiera hello-datorer som har komprometterats
* Hello-filer som har öppnats och fastställa hello känslighet av dessa filer

Du kan utnyttja **Hotinformation** information i OMS-säkerhet och granska lösningen toohelp med dessa uppgifter. Gör hello nedan tooaccess detta **Hotinformation** alternativ:

1. I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **säkerhet och granska** panelen.
   
    ![Säkerhet och granskning](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. I hello **säkerhet och granska** instrumentpanelen visas hello **Hotinformation** alternativ i hello höger, enligt nedan:
   
    ![Hot intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Dessa tre paneler ger en översikt över hello aktuella hot. I hello **Server med utgående skadlig trafik** du kommer att kunna tooidentify om det är en dator som du övervakar (inom eller utanför ditt nätverk) som skickar skadlig trafik toohello Internet. 

Hej **upptäckte hot typer** panelen visas en sammanfattning av hello hot som är aktuella ”i vilda hello”, om du klickar på den här panelen visas mer information om dessa hot som visas nedan:

![Identifierade hottyper](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Du kan extrahera mer information om varje hot genom att klicka på den. hello exemplet nedan visar mer information om Botnät:

![Mer information om ett hot](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Enligt hello början av det här avsnittet kan kan den här informationen vara väldigt användbar vid ett ärende för incidenter. Det kan också vara viktigt vid kriminaltekniska undersökningar, där du behöver toofind hello källan för hello angrepp, vilket system som har drabbats och hello tidslinje. I den här rapporten som du lätt kan identifiera vissa viktiga uppgifter om hello angrepp, t.ex: hello källan för hello attack, hello lokala IP-adress som har drabbats och hello aktuella sessionstillstånd hello-anslutning. 

Hej **hot intelligence kartan** hjälper dig att tooidentify hello aktuell platser runt hello världen som har skadlig trafik. Det finns orange (inkommande) och röda (utgående) pilar i den här kartan som identifierar hello riktning på nätverkstrafik, om du klickar på någon av dessa pilar, visas hello typ av hot och hello riktning på nätverkstrafik som visas nedan:

![hotinformationkarta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> Du kan se en demonstration på hur toouse funktionen under en incidenter bearbeta genom att titta på hello presentation [minimera datacenter säkerhetshot interaktiv undersökningen med hjälp av Operations Management Suite](https://myignite.microsoft.com/videos/5000) leverera på Microsoft Ignite.
> 

### <a name="responding-toodistinct-malicious-ip-accessed"></a>Svara toodistinct skadliga IP nås
I vissa situationer kan finnas det en potentiellt skadlig IP-adress som öppnas från en övervakad dator:

![hotinformationkarta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.png)

Den här aviseringen och andra inom Hej samma kategori, genereras via OMS säkerhet genom att utnyttja [Microsoft Hotinformation](https://youtu.be/O4WtxgUrDc8). Hej Hotinformation data är samlas in av Microsoft som köpts från inledande hot intelligence providers. Dessa data uppdateras ofta och anpassade toofast flytta hot. På grund av tooits karaktär, bör det kombineras med andra informationskällor säkerhet vid [undersöker](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) en säkerhetsavisering. 

## <a name="customize-alerts-received-via-e-mail"></a>Anpassa aviseringar via e-post

Du kan anpassa vilka användare i din organisation ska meddelas när säkerhetsvarningar utlöses av OMS-säkerhet. Det här alternativet är tillgängligt under översikt / inställningar på hello OMS instrumentpanelen:

![E-post](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur toouse hello **Hotinformation** alternativ i OMS-säkerhet och granska lösningen toorespond toosecurity aviseringar. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Komma igång med Operations Management Suite säkerhet och granska lösningen](oms-security-getting-started.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

