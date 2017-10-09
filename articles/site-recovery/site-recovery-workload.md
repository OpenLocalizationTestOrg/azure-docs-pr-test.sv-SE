---
title: aaaWhat arbetsbelastningar kan du skydda med Azure Site Recovery?
description: "Azure Site Recovery skyddar dina arbetsbelastningar och program genom att samordna hello replikering, redundans och återställning av lokala virtuella datorer och fysiska servrar tooAzure eller tooa sekundär lokal plats"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: 4953948f-26c0-4699-8fe7-59d3bfc1d3da
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/08/2017
ms.author: raynew
ms.openlocfilehash: cab2e1ce3c2b7b2c5f899d957219f5c12eb5965c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Vilka arbetsbelastningar kan jag skydda med Azure Site Recovery?
Den här artikeln beskriver arbetsbelastningar och program som du kan replikera med hello Azure Site Recovery-tjänsten.

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Översikt
Organisationer behöver en kontinuerlig verksamhet och disaster recovery (BCDR) strategi tookeep arbetsbelastningar och data säkra och tillgängliga under planerade och oplanerade driftavbrott och återställa tooregular drifttillstånd så snart som möjligt.

Site Recovery är en Azure-tjänst som stödjer tooyour BCDR-strategi. Du kan använda Site Recovery för att distribuera Programmedveten replikering toohello molnet eller tooa sekundär plats. Om dina appar är Windows eller Linux-baserade, körs på fysiska servrar, virtuella VMware- eller Hyper-V, du kan använda Site Recovery tooorchestrate replikering, utföra tester för disaster recovery och köra redundansväxlingar och återställning efter fel.

Site Recovery integrerar med Microsoft-program som SharePoint, Exchange, Dynamics, SQL Server och Active Directory. Microsoft är även i nära samarbete med ledande leverantörer som Oracle, SAP, IBM och Red Hat. Du kan anpassa replikeringslösningar på appbasis.

## <a name="why-use-site-recovery-for-application-replication"></a>Varför ska man använda Site Recovery för programreplikering?
Site Recovery bidrar tooapplication nivå skydd och återställning på följande sätt:

* App-oberoende replikering för alla arbetsbelastningar som körs på en dator som stöds.
* Nästintill synkron replikering med Återställningspunktsmål så låga som 30 sekunder toomeet hello behoven hos de mest kritiska affärsapparna.
* Appkonsekventa ögonblicksbilder för program med en eller flera nivåer.
* Integrering med SQL Server AlwaysOn och samverkan med andra replikeringstekniker på programnivå som AD-replikering, SQL AlwaysOn, Exchange-databastillgänglighetsgrupper (DAG, Database Availability Group) och Oracle Data Guard.
* Flexibla återställningsplaner som aktiverar toorecover en hel programstack med en enda klickning och inkludera tooinclude externa skript och manuella åtgärder i hello plan.
* Avancerad nätverkshantering i Site Recovery och Azure toosimplify konfigurera nätverkskraven för appar, inklusive hello möjlighet tooreserve IP-adresser, belastningsutjämning och integrering med Azure Traffic Manager för låga RTO-nätverksväxlingar.
* Ett omfattande automationsbibliotek som ger tillgång till produktionsklara, programspecifika skript som kan ladas ned och integreras med återställningsplaner.

## <a name="workload-summary"></a>Översikt över arbetsbelastningar
Site Recovery kan replikera alla appar som körs på en dator som stöds. Dessutom samarbetar vi med produkten team toocarry ut ytterligare appspecifika tester.

| **Arbetsbelastning** | **Replikera virtuella Hyper-V-datorer tooa sekundär plats** | **Replikera virtuella Hyper-V-datorer tooAzure** | **Replikera virtuella VMware-datorer tooa sekundär plats** | **Replikera virtuella VMware-datorer tooAzure** |
| --- | --- | --- | --- | --- |
| Active Directory, DNS |Y |Y |Y |Y |
| Webbappar (IIS, SQL) |Y |Y |Y |Y |
| System Center Operations Manager |Y |Y |Y |Y |
| Sharepoint |Y |Y |Y |Y |
| SAP<br/><br/>Replikera SAP plats tooAzure för icke-kluster |Y (har testats av Microsoft) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |
| Exchange (icke-DAG) |Y |Y |Y |Y |
| Fjärrskrivbord/VDI |Y |Y |Y |Saknas |
| Linux (operativsystem och appar) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |
| Dynamics AX |Y |Y |Y |Y |
| Dynamics CRM |Y |Kommer snart |Y |Kommer snart |
| Oracle |Y (har testats av Microsoft) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |Y (har testats av Microsoft) |
| Windows-filserver |Y |Y |Y |Y |
| Citrix XenApp och XenDesktop |Saknas |Y |Saknas |Y |

## <a name="replicate-active-directory-and-dns"></a>Replikera Active Directory och DNS
En Active Directory och DNS-infrastruktur är mycket viktigt toomost enterprise appar. Vid katastrofåterställning, du behöver tooprotect och återställa dessa infrastrukturkomponenter innan du återställer dina arbetsbelastningar och appar.

Du kan använda Site Recovery toocreate en fullständig automatiserad haveriberedskapsplan för Active Directory och DNS. Till exempel om du vill toofail över SharePoint och SAP från en primär tooa sekundär plats kan du ställa in en återställningsplan som växlar över Active Directory först och sedan en ytterligare appspecifik plan toofail över hello andra appar som förlitar sig på Active Katalog.

[Lär dig mer](site-recovery-active-directory.md) om hur du skyddar Active Directory och DNS.

## <a name="protect-sql-server"></a>Skydda SQL Server
SQL Server utgör grunden för datatjänster i många affärsappar i ett lokalt datacenter.  Site Recovery kan användas tillsammans med SQL Server hr/DR-tekniker, tooprotect flera nivåer enterprise appar som använder SQL Server. Site Recovery tillhandahåller:

* En enkel och kostnadseffektiv haveriberedskapslösning för SQL Server. Replikera flera versioner och utgåvor av SQL Server fristående servrar och kluster, tooAzure eller tooa sekundär plats.  
* Integrering med SQL AlwaysOn-Tillgänglighetsgrupper, toomanage redundans och återställning med Azure Site Recovery-återställningsplaner.
* Slutpunkt till slutpunkt-återställningsplaner för hello alla nivåer i ett program, inklusive hello SQL Server-databaser.
* Skalning av SQL Server för hög arbetsbelastning med Site Recovery genom att ”bursta” dem till större virtuella IaaS-datorstorlekar i Azure.
* Enkel testning av SQL Server-haveriberedskap. Du kan köra redundanstester tooanalyze data och köra efterlevnadskontroller, utan att påverka produktionsmiljön.

[Lär dig mer](site-recovery-sql.md) om hur du skyddar SQL Server.

## <a name="protect-sharepoint"></a>Skydda SharePoint
Azure Site Recovery hjälper dig att skydda SharePoint-distributioner på följande sätt:

* Eliminerar behovet av hello och associerade infrastrukturkostnader för en reservservergrupp för haveriberedskap. Använd Site Recovery tooreplicate en hel servergrupp (webb, app- och databasnivåer) tooAzure eller tooa sekundär plats.
* Förenklar programdistribution och programhantering. Uppdateringar som distribueras toohello primära platsen replikeras automatiskt och är därför tillgängliga efter redundans och återställning av en grupp på en sekundär plats. Minskar också hello och kostnaderna för att hålla en reservservergrupp uppdaterad.
* Förenklar utvecklingen och testningen av SharePoint-program genom att på begäran skapa en produktionslik kopia av replikmiljön för testning och felsökning.
* Förenklar övergången toohello moln via Site Recovery toomigrate SharePoint distributioner tooAzure.

[Lär dig mer](site-recovery-sharepoint.md) om hur du kan skydda SharePoint.

## <a name="protect-dynamics-ax"></a>Skydda Dynamics AX
Azure Site Recovery hjälper dig att skydda din Dynamics AX ERP-lösning genom att:

* Samordna replikeringen av hela Dynamics AX-miljön (webb- och AOS-nivåer, databasnivåer, SharePoint) tooAzure, eller tooa sekundär plats.
* Förenkla migreringen av Dynamics AX-distributioner toohello molnet (Azure).
* Förenkla utvecklingen och testningen av Dynamics AX-program genom att på begäran skapa en produktionslik kopia för testning och felsökning.

[Lär dig mer](site-recovery-dynamicsax.md) om hur du kan skydda Dynamic AX.

## <a name="protect-rds"></a>Skydda Fjärrskrivbordstjänster
Remote Desktop Services (RDS) aktiverar virtuell datorinfrastruktur (VDI), sessionsbaserade skrivbord och program, vilket gör att användare toowork var som helst. Med Azure Site Recovery kan du:

* Replikera hanterade eller ohanterade grupperade virtuella skrivbord tooa sekundär plats, och program och sessioner tooa sekundär fjärrplats eller Azure.
* Du kan replikera det här:

| **RDS** | **Replikera virtuella Hyper-V-datorer tooa sekundär plats** | **Replikera virtuella Hyper-V-datorer tooAzure** | **Replikera virtuella VMware-datorer tooa sekundär plats** | **Replikera virtuella VMware-datorer tooAzure** | **Replikera fysiska servrar tooa sekundär plats** | **Replikera fysiska servrar tooAzure** |
| --- | --- | --- | --- | --- | --- | --- |
| **Poolat virtuellt skrivbord (ohanterat)** |Ja |Nej |Ja |Nej |Ja |Nej |
| **Poolat virtuellt skrivbord (hanterat och utan UPD)** |Ja |Nej |Ja |Nej |Ja |Nej |
| **Fjärrprogram och skrivbordssessioner (utan UPD)** |Ja |Ja |Ja |Ja |Ja |Ja |

[Lär dig mer](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) om hur du kan skydda Fjärrskrivbordstjänster.

## <a name="protect-exchange"></a>Skydda Exchange
Site Recovery skyddar Exchange på följande sätt:

* För små Exchange-distributioner, till exempel en enskild eller fristående servrar, Site Recovery replikera och redundansväxla tooAzure eller tooa sekundär plats.
* För större distributioner integrerar Site Recovery med Exchange DAG:ar.
* Exchange-Databastillgänglighetsgrupper är hello rekommenderad lösning för haveriberedskap i ett företag.  Site Recovery-återställningsplaner kan innehålla dag tooorchestrate DAG redundans mellan platser.

[Lär dig mer](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) om hur du kan skydda Exchange.

## <a name="protect-sap"></a>Skydda SAP
Använd Site Recovery tooprotect din SAP-distribution, enligt följande:

* Aktivera skydd för SAP NetWeaver och icke - NetWeaver produktion program som körs lokalt, genom att replikera komponenter tooAzure.
* Aktivera skydd för SAP NetWeaver och icke - NetWeaver produktion program som körs Azure, genom att replikera komponenter tooanother Azure-datacenter.
* Förenkla molnmigreringen, genom att använda Site Recovery toomigrate tooAzure din SAP-distribution.
* Förenkla SAP-projektuppgradering, testning och prototyper genom att skapa en produktionsklon på begäran för att testa SAP-program.

[Lär dig mer](site-recovery-sap.md) om hur du kan skydda SAP.

## <a name="protect-iis"></a>Skydda IIS
Använd Site Recovery tooprotect IIS-distributionen, enligt följande:

Azure Site Recovery tillhandahåller katastrofåterställning genom att replikera hello viktiga komponenter i din miljö tooa kalla fjärrplats eller ett offentligt moln som Microsoft Azure. Eftersom hello virtuell dator med hello webbservern och hello databasen som replikerade toohello återställningsplatsen, finns det ingen krav toobackup configuration-filer eller certifikat separat. Hej programavbildningar och bindningar beroende av miljövariabler som har ändrats efter växling vid fel kan uppdateras via skript som är integrerade i hello disaster recovery planer. Virtuella datorer förs upp hello återställningsplatsen endast i ett redundanskluster hello-händelse. Inte bara, Azure Site Recovery hjälper dig också att samordna hello slutet tooend redundans genom att tillhandahålla du hello följande funktioner:

-   Stäng av ordningsföljd hello och Start för virtuella datorer i hello olika nivåer.
-   Lägga till skript tooallow uppdatering av programberoenden och bindningar på hello virtuella datorer när de har startats. hello skript kan även vara används tooupdate hello DNS server toopoint toohello återställningsplatsen.
-   Tilldela IP-adresser toovirtual datorer pre-redundans genom att mappa hello primära platsen och återställningsplatsen nätverk och därför använda skript som inte behöver toobe uppdateras efter växling vid fel.
-   Möjligheten för en enkelklickning redundans för flera webbprogram på hello webbservrar, vilket eliminerar hello scope förvirring hello händelse av en katastrofåterställning.
-   Möjlighet tootest hello återställningsplaner i en isolerad miljö för DR övningar.

[Läs mer](https://aka.ms/asr-iis) om hur du skyddar en IIS-webbservergrupp.

## <a name="protect-citrix-xenapp-and-xendesktop"></a>Skydda Citrix XenApp och XenDesktop
Använd Site Recovery tooprotect Citrix XenApp och XenDesktop distributioner, enligt följande:

* Aktivera skydd för hello Citrix XenApp och XenDesktop distributionen genom att replikera olika distribution skikt inklusive (AD, DNS-server, SQL databasserver server, Citrix leverans styrenhet, StoreFront, XenApp Master (VDA), Citrix XenApp licensservern) tooAzure.
* Förenkla molnmigreringen, med hjälp av Site Recovery toomigrate din Citrix XenApp och XenDesktop tooAzure för distributionen.
* Förenkla Citrix XenApp-/XenDesktop-testningen genom att skapa en produktionslik kopia på begäran för testning och felsökning.
* Den här lösningen gäller endast för Windows Server-operativsystemens virtuella skrivbord och inte för virtuella klientskrivbord eftersom virtuella klientskrivbord fortfarande inte har stöd för licensiering i Azure.
[Lär dig mer](https://azure.microsoft.com/pricing/licensing-faq/) om licensiering för klient/server-datorer i Azure.

[Lär dig mer](site-recovery-citrix-xenapp-and-xendesktop.md) om att skydda Citrix XenApp- och XenDesktop-distributioner. Du kan också se hello [White Paper från Citrix](https://aka.ms/citrix-xenapp-xendesktop-with-asr) med hello samma.

## <a name="next-steps"></a>Nästa steg
[Kontrollera krav](site-recovery-prereq.md)
