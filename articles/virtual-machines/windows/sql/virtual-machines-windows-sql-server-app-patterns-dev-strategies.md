---
title: "aaaSQL mönster för Server-programmet på virtuella datorer | Microsoft Docs"
description: "Den här artikeln beskriver programmet mönster för SQL Server på virtuella Azure-datorer. Det ger lösningsarkitekter och utvecklare en grund för bra programarkitektur och utformning."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: e18597598fdf9fe534ed07331d6f531d4dca0601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Programmönster och utvecklingsstrategier för SQL Server i Azure Virtual Machines
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Sammanfattning:
Bestämma vilka programmönster eller mönster toouse för dina SQL-Server-baserade program i Azure-miljö är ett viktigt beslut och kräver en god förståelse av hur SQL Server och varje komponent i nätverksinfrastrukturen Azure tillsammans . Med SQL Server i Azure Infrastructure Services kan du enkelt migrera, underhålla och övervaka din befintliga SQL Server-program bygger på Windows Server toovirtual datorer i Azure.

hello syftet med den här artikeln är tooprovide lösningsarkitekter och utvecklare en grund för bra programarkitektur och design som de kan följa när du migrerar befintliga program tooAzure som som utveckla nya program i Azure.

För varje program du hittar ett lokalt scenario, dess respektive moln-aktiverad lösning och hello relaterade tekniska rekommendationer. Dessutom beskrivs hello strategier för utveckling av Azure-specifika så att du kan utforma dina program på rätt sätt. På grund av toohello många möjliga programmet mönster rekommenderas att arkitekter och utvecklare bör välja hello lämpligaste mönster för sina program och användare.

**Teknisk deltagare:** Thomas Carlos Vargas sill, Madhan Arumugam Ramakrishnan

**Teknisk granskare:** Corey slipmaskiner, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Introduktion
Du kan utveckla många typer av flera nivåer program genom att avgränsa hello komponenter av hello annat program lager på olika datorer samt separata komponenter. Exempelvis kan du placera hello klientprogrammet och företag regler komponenter i en dator, frontend-webbnivån och data access nivå komponenter i en annan dator och en backend-databasens nivå i en annan dator. Den här typen av strukturera kan isolera varje nivå från varandra. Om du ändrar där data hämtas från du inte behöver toochange hello klienten eller programmet men endast hello nivå komponenter för dataåtkomst.

En typisk *n-nivå* program innehåller hello presentationsnivå hello affärsnivå och datanivå hello:

| Nivå | Beskrivning |
| --- | --- |
| **Presentation** |Hej *presentationsnivå* (web lager, frontend) är hello lager där användarna samverkar med ett program. |
| **Företag** |Hej *affärsnivå* (mellannivå) är hello lager som hello presentation nivå och hello data nivå Använd toocommunicate med varandra och innehåller hello huvudfunktionerna i hello system. |
| **Data** |Hej *datanivå* är i praktiken hello-server som lagrar data i ett program (till exempel en server som kör SQL Server). |

Programmet lager beskrivs hello logiska grupperingar av hello funktioner och komponenter i ett program. medan nivåer beskriver hello fysiska distribution av hello funktioner och komponenter på separata fysiska servrar, datorer, nätverk eller fjärrplatser. hello lager i ett program kan finnas på hello samma fysiska dator (hello samma nivå) kan fördelas på separata datorer (n-nivå) och hello komponenter i varje lager kommunicerar med komponenter i andra lager via väldefinierade gränssnitt. Du kan tänka dig hello termen nivån som toophysical distribution mönster, till exempel två nivåer, tre nivåer och n-nivå. En **2-nivån, programmönster** innehåller två programnivåerna: programservern och databasserver. hello direkt kommunikation som sker mellan hello programservern och hello databasserver. hello Programserver innehåller både webb-nivå och affärsnivå komponenter. I **3-nivåprogram mönster**, det finns tre programnivåerna: webbserver, programservern, som innehåller hello affärslogiknivå och/eller business nivå data access-komponenterna och hello databasservern. hello kommunikation mellan hello webbservern och hello databasserver sker över hello-programserver. Detaljerad information om programmet lager och nivåer finns [Guide för Microsoft-arkitektur](https://msdn.microsoft.com/library/ff650706.aspx).

Innan du börjar läsa den här artikeln bör du ha kunskap om hello grundläggande begrepp för SQL Server och Azure. Mer information finns i [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) och [Azure.com](https://azure.microsoft.com/).

Den här artikeln beskriver flera program mönster som kan vara lämplig för din enkla program samt hello komplexa företagsprogram. Innan du med detaljer om varje mönster, rekommenderar vi att du bekanta dig med hello tillgängliga data storage-tjänster i Azure, t.ex [Azure Storage](../../../storage/common/storage-introduction.md), [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md), och [ SQLServer i en virtuell Azure-dator](virtual-machines-windows-sql-server-iaas-overview.md). toomake hello bästa designbeslut för dina program, Förstå när toouse vilka datalagring tjänsten tydligt.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Välj SQLServer i en virtuell Azure-dator, när:
* Du behöver behörighet i SQL Server och Windows. Detta kan exempelvis omfatta hello SQL Server-version, särskilda snabbkorrigeringar, prestanda, konfiguration och så vidare.
* Du måste en fullständig kompatibilitet med SQL Server on-premises och vill toomove befintliga program tooAzure som-är.
* Du vill tooleverage hello funktionerna i hello Azure-miljön, men Azure SQL Database stöder inte alla hello-funktioner som krävs för ditt program. Det kan innefatta hello följande områden:
  
  * **Databasstorlek**: samtidigt hello artikeln har uppdaterats SQL-databasen har stöd för en databas med upp too1 TB data. Om ditt program kräver mer än 1 TB data och du inte vill tooimplement anpassade horisontell partitionering lösningar, rekommenderas att du använder SQL Server i en virtuell dator i Azure. Hello senaste information finns i [skala ut Azure SQL-databaser](https://msdn.microsoft.com/library/azure/dn495641.aspx) och [Azure SQL Database servicenivåer och prestandanivåer](../../../sql-database/sql-database-service-tiers.md).
  * **HIPAA kompatibilitet**: sjukvården kunder och oberoende programvaruleverantörer (ISV) kan välja [SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) i stället för [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md) eftersom SQL Server i en virtuell Azure-dator som omfattas av HIPAA (BAA Business associera avtalet). Mer information om kompatibilitet finns [Microsoft Azure Trust Center: kompatibilitet](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Funktioner på instansnivå**: just nu, SQL Database stöder funktioner som live utanför hello-databasen (till exempel länkade servrar Agent-jobb, FileStream, Service Broker osv.). Mer information finns i [Azure SQL Database riktlinjer och begränsningar](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1-nivån (enkel): enskild virtuell dator
I det här programmet i mönstret distribuera SQL Server-programmet och databasen tooa fristående virtuell dator i Azure. hello innehåller samma virtuella dator din program-och klienten, business-komponenter, dataåtkomstlagret och hello databasserver. hello presentation, företag och data access kod separeras logiskt men är fysiskt belägna i en enskild server-dator. De flesta kunder börjar med det här programmet mönstret och sedan skala ut genom att lägga till flera web-roller eller virtuella datorer tootheir system.

Det här programmet mönstret är användbart när:

* Du tooperform en enkel migrering tooAzure plattform tooevaluate om hello plattform svar programmets krav eller inte.
* Du vill tookeep alla hello programnivåerna finns i hello samma virtuella dator i hello samma Azure-Datacenter tooreduce hello fördröjning mellan nivåer.
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Vill du tooperform test för att variera arbetsbelastningsnivåer men på hello samtidigt som du inte vill tooown och underhålla många fysiska datorer alla hello tid.

hello visar följande diagram en enkel lokalt scenariot och hur du kan distribuera dess aktiverat molnlösning i en enskild virtuell dator i Azure.

![1-nivån, programmönster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Distribuera hello business layer (affärslogik och data som har åtkomst till komponenter) på hello samma fysiska skikt som hello presentation lager kan du maximera programprestandan, om du måste använda en separat nivå på grund av tooscalability eller säkerhetsgrupp frågor.

Eftersom det är ett mycket vanligt mönstret toostart med kanske hello följande artikel på migrering som är användbar för att flytta dina data tooyour SQL Server-VM: [migrera en databas tooSQL Server på en Azure VM](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-skikts (enkel): flera virtuella datorer
I det här programmet i mönstret distribuera 3-nivåprogram i Azure genom att varje nivå för program i en annan virtuell dator. Detta ger en flexibel miljö för ett enkelt skala upp och skala ut scenarier. När en virtuell dator innehåller tillämpningsprogrammet-och klienten, hello andra värd för företagskomponenter och hello andra en värdar hello-databasservern.

Det här programmet mönstret är användbart när:

* Vill du tooperform en migrering av komplexa databasen program tooAzure virtuella datorer.
* Vill du annat program nivåer toobe finns i olika regioner. Du kan till exempel har delat databaser som är distribuerade toomultiple regioner för rapportering.
* Vill du toomove företagsprogram från lokala virtualiserade plattformar tooAzure virtuella datorer. En detaljerad beskrivning på företagsprogram finns [vad är ett företagsprogram](https://msdn.microsoft.com/library/aa267045.aspx).
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Vill du tooperform test för att variera arbetsbelastningsnivåer men på hello samtidigt som du inte vill tooown och underhålla många fysiska datorer alla hello tid.

hello visar följande diagram hur du kan placera en enkel 3-nivåprogram i Azure genom att varje nivå för program i en annan virtuell dator.

![3-nivåprogram mönster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

I det här programmet i mönstret finns bara en virtuell dator (VM) på varje nivå. Om du har flera virtuella datorer i Azure, rekommenderar vi att du konfigurerar ett virtuellt nätverk. [Virtuella Azure-nätverket](../../../virtual-network/virtual-networks-overview.md) skapar en betrodd säkerhetsgräns och låter också virtuella datorer toocommunicate sinsemellan över hello privata IP-adress. Dessutom alltid se till att alla Internet-anslutningar bara gå toohello presentationsnivå. När det här mönstret för programmet, hantera hello säkerhet grupp regler toocontrol nätverksåtkomst. Mer information finns i [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

I hello diagram vara Internetprotokoll TCP, UDP, HTTP eller HTTPS.

> [!NOTE]
> Ställa in ett virtuellt nätverk i Azure är kostnadsfritt. Dock debiteras du för hello VPN-gateway som ansluter tooon lokalt. Det här tillägget är baserad på hello mängden tid som anslutningen är etablerad och tillgängliga.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>nivå 2 och 3-skikts med presentation nivå skalbar
I det här programmet i mönstret distribuera 2 eller 3 nivåer databasen programmet tooAzure virtuella datorer genom att varje nivå för program i en annan virtuell dator. Dessutom kan skala du ut hello presentationsnivå på grund av tooincreased mängden inkommande klientbegäranden.

Det här programmet mönstret är användbart när:

* Vill du toomove företagsprogram från lokala virtualiserade plattformar tooAzure virtuella datorer.
* Vill du tooscale ut hello presentationsnivå på grund av tooincreased mängden inkommande klientbegäranden.
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Vill du tooperform test för att variera arbetsbelastningsnivåer men på hello samtidigt som du inte vill tooown och underhålla många fysiska datorer alla hello tid.
* Vill du tooown en infrastruktur-miljö som kan skalas upp och ned på begäran.

hello visar följande diagram hur du kan placera hello programnivåerna i flera virtuella datorer i Azure genom att skala ut hello presentationsnivå på grund av tooincreased mängden inkommande klientbegäranden. Som visas i diagram hello ansvarar Azure belastningsutjämnare för distribuerar trafik över flera virtuella datorer och även bestämma vilka web server tooconnect till. Flera instanser av hello servrar bakom en belastningsutjämnare försäkrar du dig hello hög tillgänglighet för hello presentationsnivå.

![Programmönster - presentation nivå skala ut](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Metodtips för nivå 2, 3-skikts eller n-nivå mönster som har flera virtuella datorer i ett lager
Vi rekommenderar att du placerar hello virtuella datorer som tillhör samma nivå i hello samma molntjänst och hello i samma toohello hello tillgänglighetsuppsättning. Till exempel placera en uppsättning webbservrar i **CloudService1** och **AvailabilitySet1** och en uppsättning databasservrar i **CloudService2** och **AvailabilitySet2**. En tillgänglighetsuppsättning i Azure kan du tooplace hello hög tillgänglighet noder i separata feldomäner och uppgraderingsdomäner.

tooleverage flera VM-instanser av en nivå måste tooconfigure Azure belastningsutjämnare mellan programnivåerna. tooconfigure belastningsutjämnare i varje nivå, skapa en belastningsutjämnad slutpunkt separat på varje nivå virtuella datorer. För en viss nivå, först skapa virtuella datorer i hello samma molntjänst. Detta säkerställer att de har hello samma offentliga virtuella IP-adress. Skapa sedan en slutpunkt på en av hello virtuella datorer på nivån. Sedan hello tilldela samma slutpunkt toohello andra virtuella datorer på nivån för belastningsutjämning. Genom att skapa en belastningsutjämnad uppsättning distribuerar trafik över flera virtuella datorer och även tillåta hello belastningsutjämnare toodetermine vilken nod tooconnect när serverdelens VM nodfel. Till exempel garanterar har flera instanser av hello servrar bakom en belastningsutjämnare hello hög tillgänglighet för hello presentationsnivå.

Som bästa praxis bör alltid se till att alla internet-anslutningar först gå toohello presentationsnivå. hello presentation layer kommer åt hello affärsnivå och sedan hello affärsnivå har åtkomst till hello datanivå. Mer information om hur tooallow åt toohello presentation layer finns [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Observera att hello belastningsutjämnare i Azure fungerar liknande tooload belastningsutjämnare i en lokal miljö. Mer information finns i [belastningsutjämning för Azure infrastrukturtjänster](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Vi rekommenderar dessutom att du konfigurerar ett privat nätverk för virtuella datorer med hjälp av Azure Virtual Network. Detta innebär att de toocommunicate sinsemellan över hello privata IP-adress. Mer information finns i [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>nivå 2 och 3-skikts med business nivå skalbar
I det här programmet i mönstret distribuera 2 eller 3 nivåer databasen programmet tooAzure virtuella datorer genom att varje nivå för program i en annan virtuell dator. Dessutom kanske du vill toodistribute hello programmet server-komponenter toomultiple virtuella datorer på grund av toohello komplexiteten i ditt program.

Det här programmet mönstret är användbart när:

* Vill du toomove företagsprogram från lokala virtualiserade plattformar tooAzure virtuella datorer.
* Du vill toodistribute hello programmet server-komponenter toomultiple virtuella datorerna på grund av toohello komplexiteten i ditt program.
* Vill du toomove business logic tunga lokalt LOB (line-of-business) program tooAzure virtuella datorer. LOB-program är en uppsättning program kritiska datorer som är viktiga toorunning företaget, till exempel redovisning, personalavdelningen (HR), lön, styrning och resursplanering program.
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Vill du tooperform test för att variera arbetsbelastningsnivåer men på hello samtidigt som du inte vill tooown och underhålla många fysiska datorer alla hello tid.
* Vill du tooown en infrastruktur-miljö som kan skalas upp och ned på begäran.

hello följande diagram visar ett lokalt scenario och dess aktiverat molnlösning. I det här scenariot kan placera du hello programnivåerna i flera virtuella datorer i Azure genom att skala ut hello affärsnivå som innehåller hello affärslogiknivå och komponenter för dataåtkomst. Som visas i diagram hello ansvarar Azure belastningsutjämnare för distribuerar trafik över flera virtuella datorer och även bestämma vilka web server tooconnect till. Flera instanser av hello programservrar bakom en belastningsutjämnare försäkrar du dig hello hello affärsnivå har hög tillgänglighet. Mer information finns i [bästa praxis för nivå 2, 3-skikts eller n-tier application-mönster som har flera virtuella datorer i ett lager](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Programmet mönster med business nivå skala ut](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>nivå 2 och 3-skikts med presentation och företag nivåer skalbar och HADR
I det här programmet i mönstret distribuera 2 eller 3 nivåer databasen programmet tooAzure virtuella datorer genom att distribuera hello presentationsnivå (webbserver) och hello business-nivån (Programserver) komponenter toomultiple virtuella datorer. Dessutom kan implementera du lösningar för hög tillgänglighet och katastrofåterställning för dina databaser i virtuella Azure-datorer.

Det här programmet mönstret är användbart när:

* Vill du toomove företagsprogram från virtualiserade plattformar lokalt tooAzure genom att implementera SQL Server hög tillgänglighet och katastrofåterställning återställningsfunktioner.
* Vill du tooscale ut hello presentationsnivån och hello affärsnivå förfallodatum tooincreased mängden inkommande klientbegäranden och hello komplexiteten i ditt program.
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Vill du tooperform test för att variera arbetsbelastningsnivåer men på hello samtidigt som du inte vill tooown och underhålla många fysiska datorer alla hello tid.
* Vill du tooown en infrastruktur-miljö som kan skalas upp och ned på begäran.

hello följande diagram visar ett lokalt scenario och dess aktiverat molnlösning. I det här scenariot kan du skala ut hello presentationsnivån och hello business nivå komponenter i flera virtuella datorer i Azure. Dessutom kan implementera du hög tillgänglighet och haveriberedskap (HADR) återställningsfunktioner för SQL Server-databaser i Azure.

Kör flera kopior av ett program i olika Se virtuella datorer till att du är belastningsutjämning begäranden mellan dem. När du har flera virtuella datorer måste toomake till att dina virtuella datorer är tillgänglig och körs på en punkt i tiden. Om du konfigurerar belastningsutjämning Azure belastningsutjämnare spårar hello hälsotillståndet för virtuella datorer och dirigerar inkommande samtal toohello felfritt fungerande Virtuella noder korrekt. Mer information om hur tooset in belastningsutjämning hello virtuella datorer, se [belastningsutjämning för Azure infrastrukturtjänster](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Flera instanser av webbservrar och programservrar bakom en belastningsutjämnare försäkrar du dig hello hög tillgänglighet för hello presentation och nivåer.

![Skalbar och hög tillgänglighet](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Metodtips för mönster för program som kräver SQL HADR
När du konfigurerar SQL Server med hög tillgänglighet och katastrofåterställning i Azure Virtual Machines, konfigurera ett virtuellt nätverk för virtuella datorer med hjälp av [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) är obligatoriskt.  Virtuella datorer i ett virtuellt nätverk måste ha en stabil privata IP-adress även efter ett avbrott i tjänsten, vilket du kan undvika hello Uppdateringstid krävs för DNS-namnmatchning. Dessutom hello virtuellt nätverk kan tooextend ditt lokala nätverk tooAzure och skapar en betrodd säkerhetsgräns. Om programmet har domänbegränsningar för företagets (till exempel Windows-autentisering, Active Directory), till exempel konfigurera [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) krävs.

De flesta kunder som kör kod i Azure, hindrar både primära och sekundära repliker i Azure.

Omfattande information och självstudier om hög tillgänglighet och katastrofåterställning återställningsfunktioner finns [hög tillgänglighet och Haveriberedskap för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>nivå 2 och 3-skikts med molntjänster och virtuella Azure-datorer
I det här programmet i mönstret du distribuerar 2 eller 3 nivåer program tooAzure med både [Azure Cloud Services](../../../cloud-services/cloud-services-choose-me.md#tellmecs) (webb- och arbetsroller roller - plattform som en tjänst (PaaS)) och [Azure Virtual Machines](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) () Infrastruktur som en tjänst (IaaS)). Med hjälp av [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) för hello presentation nivå/affärsnivå och SQL Server i [Azure Virtual Machines](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för hello data nivå är bra för de flesta program som körs på Azure. hello orsak är att ha en beräknings-instans som körs på molntjänster tillhandahåller en enkel hantering, distribution, övervakning och skalbar.

Med Cloud Services, Azure underhåller hello infrastruktur du, utför rutinunderhåll, korrigeringsfiler hello operativsystem och försöker toorecover från tjänsten och maskinvarufel. När ditt program måste skalbar, är automatisk och manuell skalbar alternativ tillgängliga för ditt molntjänstprojekt genom att öka eller minska hello antal instanser eller virtuella datorer som används av ditt program. Du kan dessutom använda lokala Visual Studio toodeploy ditt program tooa molntjänstprojekt i Azure.

Sammanfattningsvis: Om du inte vill tooown omfattande administrativa uppgifter för hello presentation/affärsnivå och programmet inte kräver komplexa konfiguration av programvara eller hello operativsystem, kan du använda Azure Cloud Services. Om Azure SQL Database inte stöder alla hello-funktioner som du letar efter, använder du SQL Server i en virtuell dator i Azure för hello datanivå. Kör ett program på Azure Cloud Services och lagra data i Azure Virtual Machines kombinerar hello fördelarna med båda tjänsterna. En detaljerad jämförelse finns hello i det här avsnittet på [jämföra development strategier i Azure](#comparing-web-development-strategies-in-azure).

I det här programmet i mönstret innehåller hello presentationsnivån en webbroll som körs en Cloud Services-komponenten i hello Azure körningsmiljö och den är anpassad för web application programming som stöds av IIS och ASP.NET. hello företag eller backend nivå innehåller en arbetsroll som är en Cloud Services-komponent som körs i hello Azure körningsmiljö och det är användbart för generaliserad utveckling och kan utföra Bakgrundsbearbetning för en webbroll. hello databasnivå finns i en SQL Server-dator i Azure. hello kommunikation mellan hello presentationsnivån och hello databasnivå sker direkt eller via hello affärsnivå – komponenter för worker-rollen.

Det här programmet mönstret är användbart när:

* Vill du toomove företagsprogram från virtualiserade plattformar lokalt tooAzure genom att implementera SQL Server hög tillgänglighet och katastrofåterställning återställningsfunktioner.
* Vill du tooown en infrastruktur-miljö som kan skalas upp och ned på begäran.
* Azure SQL Database stöder inte alla hello-funktioner som ditt program eller en databas måste.
* Vill du tooperform test för att variera arbetsbelastningsnivåer men på hello samtidigt som du inte vill tooown och underhålla många fysiska datorer alla hello tid.

hello följande diagram visar ett lokalt scenario och dess aktiverat molnlösning. I det här scenariot kan placera du hello presentationsnivå i webbroller, hello affärsnivå i arbetsroller men hello datanivå i virtuella datorer i Azure. Kör flera kopior av hello presentationsnivå i olika webbroller garanterar Utjämna begäranden om tooload över dem. När du kombinerar Azure Cloud Services med Azure Virtual Machines rekommenderar vi att du ställer in [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) samt. Med [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), du kan ha stabilt och beständiga privata IP-adresser i samma molntjänst i hello hello molnet. När du definierar ett virtuellt nätverk för virtuella datorer och molntjänster kan starta de kommunicera med varandra via hello privat IP-adress. Dessutom har virtuella datorer och Azure web/worker-roller i hello samma [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) ger mindre fördröjning och säkrare anslutning. Mer information finns i [vad är en molnbaserad tjänst](../../../cloud-services/cloud-services-choose-me.md).

Som visas i diagram hello Azure belastningsutjämnare distribuerar trafik över flera virtuella datorer och avgör också vilken webbserver eller programserver tooconnect till. Flera instanser av hello-webbservrar och programservrar bakom en belastningsutjämnare försäkrar du dig hello hög tillgänglighet för hello presentationsnivån och affärsnivån hello. Mer information finns i [Metodtips för programmet mönster som kräver SQL HADR](#best-practices-for-application-patterns-requiring-sql-hadr).

![Programmet mönster med molntjänster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

En annan metod tooimplement programmet mönstret är toouse en konsoliderad webbroll som innehåller både presentationsnivån och nivå-komponenterna som visas i hello följande diagram. Det här programmet mönstret är användbart för program som kräver tillståndskänslig design. Eftersom Azure tillhandahåller tillståndslös compute-noder på webb-och arbetsroller, rekommenderar vi att du implementera logik toostore sessionstillståndet med någon av följande tekniker hello: [Azure Caching](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../../../cosmos-db/table-storage-how-to-use-dotnet.md) eller [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).

![Programmet mönster med molntjänster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Mönster med virtuella Azure-datorer, Azure SQL Database och Azure App Service (webbprogram)
hello syftet med det här programmet mönstret är tooshow du hur toocombine Azure-infrastrukturen som en tjänst (IaaS)-komponenter med Azure platform as a service komponenter (PaaS) i din lösning. Det här mönstret fokuserar på Azure SQL-databas för lagring av relationella data. Det omfattar inte SQL Server på en virtuell Azure-dator, som ingår i hello Azure-infrastrukturen som ett tjänsterbjudande.

Du distribuerar en databas program tooAzure genom att placera hello presentation och affärsnivåerna i hello samma virtuella datorn och ansluter till en databas i Azure SQL Database (SQL Database)-servrar i det här programmet i mönstret. Du kan implementera hello presentationsnivå med hjälp av traditionella IIS-baserade webblösningar. Eller, du kan implementera en kombinerad presentation och affärsnivå med hjälp av [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Det här programmet mönstret är användbart när:

* Du har redan en befintlig SQL Database-server som konfigurerats i Azure och du vill att tootest programmet snabbt.
* Vill du tootest hello funktionerna i Azure-miljön.
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Komponenter för åtkomst av logik och data som ditt företag kan vara fristående inom ett webbprogram.

hello följande diagram visar ett lokalt scenario och dess aktiverat molnlösning. I det här scenariot kan du placera hello programmet nivåer i en enskild virtuell dator i Azure och komma åt data i Azure SQL Database.

![Blandad, programmönster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Om du väljer tooimplement kombinerade webb- och programmet på datanivå med hjälp av Azure Web Apps rekommenderar vi att du behåller hello mellannivå eller ett program nivå som dynamiska länkbibliotek (dll) i hello kontexten för ett webbprogram.

Dessutom granska hello rekommendationer som anges i hello [jämföra web development strategier i Azure](#comparing-web-development-strategies-in-azure) avsnittet hello slutet av den här artikeln toolearn mer om hur du programmerar tekniker.

## <a name="n-tier-hybrid-application-pattern"></a>N-nivås hybrid programmönster
I n-nivås hybrid programmönster implementera du ditt program i flera nivåer som distribueras mellan lokala och Azure. Därför kan du skapa ett flexibelt och återanvändbara hybrid-system som du kan ändra eller lägga till en specifik nivå utan att ändra hello andra nivåer. tooextend företagsnätverket-toohello molnet kan du använda [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) service.

Det här programmet hybrid-mönstret är användbart när:

* Vill du toobuild program som kör delvis i hello molnet och delvis lokalt.
* Vill du toomigrate vissa eller alla element i ett befintligt lokalt program toohello moln.
* Vill du toomove företagsprogram från lokala virtualiserade plattformar tooAzure.
* Vill du tooown en infrastruktur-miljö som kan skalas upp och ned på begäran.
* Du vill tooquickly etablera utveckling och testmiljöer för korta tidsperioder.
* Du vill använda ett kostnadseffektivt sätt tootake säkerhetskopieringar för databasen företagsprogram.

hello följande diagram visar en n-nivås hybrid programmönster som sträcker sig över lokalt och Azure. Som det visas i hello diagram, lokal infrastruktur innehåller [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) domain controller toosupport användarautentisering och auktorisering. Observera att hello diagram visar ett scenario där vissa delar av hello datanivå bor i ett lokalt Datacenter medan vissa delar av hello datanivå live i Azure. Du kan implementera flera andra hybridscenarion beroende på programmets behov. Exempelvis kan du behålla hello presentationsnivån och affärsnivån hello i en lokal miljö men hello datanivå i Azure.

![N-nivån, programmönster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

I Azure, kan du använda Active Directory som en fristående molnkatalog för din organisation och du kan också integrera befintliga lokala Active Directory med [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Som visas i diagram hello hello business nivå komponenter kan komma åt toomultiple datakällor som för[SQL Server i Azure](virtual-machines-windows-sql-server-iaas-overview.md) via en privat interna IP-adress, tooon lokal SQL Server via [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), eller för[SQL-databas](../../../sql-database/sql-database-technical-overview.md) använder hello .NET Framework data provider tekniker. I det här diagrammet är en valfri data storage-tjänst i Azure SQL Database.

Du kan implementera hello följande arbetsflöde i hello ordning som anges i flera nivåer hybrid programmet mönster:

1. Identifiera databasen företagsprogram som behöver toobe flyttas upp toocloud med hjälp av hello [Microsoft Assessment and Planning (MAP) Toolkit](http://microsoft.com/map). hello MAP Toolkit samlar in lager-och prestandadata från datorer som du planerar för virtualisering och ger rekommendationer om kapacitet och planera assessment.
2. Planera hello resurser och konfiguration som behövs i hello Azure-plattformen, till exempel storage-konton och virtuella datorer.
3. Konfigurera nätverksanslutningen mellan hello företagsnätverket lokalt och [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). tooset hello anslutning mellan hello företagsnätverket lokalt och en virtuell dator i Azure, med någon av följande två metoder hello:
   
   1. Upprätta en anslutning mellan lokala och Azure via offentliga slutpunkter på en virtuell dator i Azure. Den här metoden ger ett enkelt installationen och du toouse SQL Server-autentisering i den virtuella datorn. Dessutom kan ställa in din network security grupp regler toocontrol trafiken toohello VM. Mer information finns i [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. Upprätta en anslutning mellan lokala och Azure via Azure virtuella privata nätverk (VPN)-tunnel. Den här metoden kan du tooextend principer tooa virtuell dator i Azure. Du kan dessutom konfigurera brandväggsregler och använder Windows-autentisering i den virtuella datorn. För närvarande stöder Azure säker plats-till-plats-VPN- och punkt-till-plats VPN-anslutningar:
      
      * Med säker plats-till-plats-anslutning, kan du upprätta nätverksanslutningen mellan ditt lokala nätverk och virtuella nätverk i Azure. Det rekommenderas för att ansluta din lokala data center miljö tooAzure.
      * Du kan använda säker punkt-till-plats-anslutning för att upprätta nätverksanslutningen mellan det virtuella nätverket i Azure och enskilda datorer var som helst med. Det rekommenderas oftast för utveckling och testning.
      
      Mer information om hur tooconnect tooSQL Server i Azure, se [ansluta tooa virtuell dator med SQL Server på Azure](virtual-machines-windows-sql-connect.md).
4. Konfigurera schemalagda jobb och aviseringar som säkerhetskopiera lokala data i en disk för virtuell dator i Azure. Mer information finns i [SQL Server-säkerhetskopiering och återställning med Azure Blob Storage-tjänsten](https://msdn.microsoft.com/library/jj919148.aspx) och [säkerhetskopiering och återställning av SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).
5. Du kan implementera ett av följande tre vanliga scenarier hello beroende på programmets behov:
   
   1. Du kan behålla din webbserver och programserver okänsliga data i en databasserver i Azure medan du behåller hello känsliga data lokalt.
   2. Du kan behålla din webbserver och programserver lokalt medan hello databasserver i en virtuell dator i Azure.
   3. Du kan behålla din databasserver, webbserver och programserver lokalt medan du behåller hello databasrepliker i virtuella datorer i Azure. Den här inställningen hello lokala webbservrar eller reporting program tooaccess hello databasrepliker i Azure. Därför kan du uppnå toolower hello arbetsbelastning i en lokal databas. Vi rekommenderar att du använder det här scenariot för frekventa Läs arbetsbelastningar och dess syfte. Information om hur du skapar databasrepliker i Azure finns i AlwaysOn-Tillgänglighetsgrupper på [hög tillgänglighet och Haveriberedskap för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Jämföra web development strategier i Azure
tooimplement och distribuera en SQL Server-baserat program i Azure med flera nivåer, kan du använda något av följande två metoder för programmering hello:

* Ställa in en traditionell webbserver (IIS - Internet Information Services) i Azure och access-databaser i SQL Server i Azure Virtual Machines.
* Implementera och distribuera en cloud service tooAzure. Kontrollerar du att den här Molntjänsten kan komma åt databaser i SQL Server i Azure virtuella datorer. En molnbaserad tjänst kan innehålla flera webb- och arbetsroller roller.

hello följande tabell innehåller en jämförelse av traditionella webbutveckling med Azure Cloud Services och Azure Web Apps med avseende tooSQL Server i Azure Virtual Machines. hello tabell innehåller Azure Web Apps eftersom det är möjligt toouse SQL Server i Azure VM som datakälla för Azure Web Apps via dess offentliga virtuella IP-adressen eller DNS-namn.

|  | Traditionella webbutveckling i Azure Virtual Machines | Cloud Services i Azure | Webbvärd med Azure Webbappar |
| --- | --- | --- | --- |
| **Programmet migrering från lokalt** |Befintliga program som-är. |Program behöver webb-och arbetsroller. |Befintliga program som – men lämpar sig för fristående webbprogram och webbtjänster som kräver snabb skalbarhet. |
| **Utveckling och distribution** |Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS-hanteraren, PowerShell. |Visual Studio Azure SDK, TFS, PowerShell. Varje molntjänst har två miljöer toowhich kan du distribuera din servicepaket och konfiguration: mellanlagring och produktion. Du kan distribuera en cloud service toohello mellanlagring miljö tootest den innan du befordrar tooproduction. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, ett, TFS, Web distribuera PowerShell. |
| **Installation och administration** |Du är ansvarig för administrativa uppgifter på hello program, data, brandväggsregler, virtuella nätverk och operativsystem. |Du är ansvarig för administrativa uppgifter på hello program, data, brandväggsregler och virtuella nätverk. |Du är ansvarig för administrativa uppgifter på hello program och endast data. |
| **Hög tillgänglighet och katastrofåterställning (HADR)** |Vi rekommenderar att du placerar virtuella datorer i hello samma tillgänglighet ange och hello i samma molntjänst. Att hålla dina virtuella datorer hello samma tillgänglighetsuppsättning som låter Azure tooplace hello noder för hög tillgänglighet i separata feldomäner och uppgraderingsdomäner. På liknande sätt att hålla dina virtuella datorer i hello samma molntjänst aktiverar belastningsutjämning och virtuella datorer kan kommunicera direkt med varandra via hello lokalt nätverk i ett Azure-datacenter.<br/><br/>Du ansvarar för att implementera en hög tillgänglighet och haveriberedskap lösning för SQL Server i Azure Virtual Machines tooavoid driftavbrott. HADR teknik som stöds, se [hög tillgänglighet och Haveriberedskap för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Du är ansvarig för säkerhetskopiering av data och program.<br/><br/>Azure kan flytta virtuella datorer om hello värddatorn i hello Datacenter misslyckas på grund av problem med toohardware. Dessutom kan det kan finnas planerade driftstopp på den virtuella datorn när hello värddatorn uppdateras för säkerhets- eller uppdateringar. Därför rekommenderar vi att du har minst två virtuella datorer i varje nivå tooensure hello kontinuerlig tillgänglighet. Azure tillhandahåller inte SLA för en enskild virtuell dator. Mer information finns i [Azure återhämtning teknisk vägledning](../../../resiliency/resiliency-technical-guidance.md). |Azure hanterar hello-fel som uppstår till följd av hello underliggande maskinvara eller programvara. Vi rekommenderar att du implementera flera instanser av en webb- eller arbetarroll rollen tooensure hello med hög tillgänglighet för ditt program. Mer information finns i [molntjänster, virtuella datorer och virtuella nätverk servicenivåavtalet](http://www.microsoft.com/download/details.aspx?id=38427) och [katastrofåterställning och hög tillgänglighet för Azure-program](../../../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Du är ansvarig för säkerhetskopiering av data och program.<br/><br/>Du är ansvarig för att implementera en hög tillgänglighet och disaster recovery lösning tooavoid driftavbrott för databaser som finns i en SQL Server-databas i en Azure VM. HDAR teknik som stöds, finns i hög tillgänglighet och katastrofåterställning för SQL Server i Azure Virtual Machines.<br/><br/>**SQL Server-databasspegling**: användning med Azure Cloud Services (web/worker-roller). SQL Server-datorer och ett molntjänstprojekt kan vara i hello samma virtuella Azure-nätverk. Om inte SQL Server-VM är hello samma virtuella nätverk måste du toocreate en SQL Server-Alias tooroute kommunikation toohello instans av SQL Server. Dessutom måste hello aliasnamn matcha hello SQL Server-namn. |Hög tillgänglighet har ärvts från arbetsroller i Azure, Azure blob storage och Azure SQL Database. Till exempel underhålls Azure lagring tre repliker av alla blob-, tabell- och kön data. När som helst en Azure SQL Database håller tre kopior av data som körs, en primär replik och två sekundära repliker. Mer information finns i [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) och [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).<br/><br/>När du använder SQL Server i Azure VM som datakälla för Azure-Webbappar, Kom ihåg att Azure Web Apps inte har stöd för Azure Virtual Network. Med andra ord måste alla anslutningar från Azure Web Apps tooSQL Server virtuella datorer i Azure gå igenom offentliga slutpunkterna för virtuella datorer. Detta kan orsaka vissa begränsningar för hög tillgänglighet och katastrofåterställning. Till exempel skulle hello-klientprogrammet på Azure Web Apps ansluta tooSQL Server VM med databasspegling inte kan tooconnect toohello nya primära servern som databasspegling kräver att du konfigurerar Azure Virtual Network mellan SQL Server-värden virtuella datorer i Azure. Därför använder **databasspegling i SQL Server** med Azure Web Apps stöds inte för närvarande.<br/><br/>**SQL Server AlwaysOn-Tillgänglighetsgrupper**: du kan konfigurera AlwaysOn-Tillgänglighetsgrupper när du använder Azure Web Apps med SQL Server-datorer i Azure. Men du måste tooconfigure AlwaysOn tillgänglighetsgruppens lyssnare tooroute hello kommunikation toohello primära repliken via offentliga belastningsutjämnade slutpunkter. |
| **Korsanslutningar** |Du kan använda Azure Virtual Network tooconnect tooon lokalt. |Du kan använda Azure Virtual Network tooconnect tooon lokalt. |Virtuella Azure-nätverket stöds. Mer information finns i [Web Apps virtuell nätverksintegration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Skalbarhet** |Skala upp är tillgänglig genom att öka hello storlekar för virtuella datorer eller att lägga till fler diskar. Mer information om storlekar för virtuella datorer finns [storlekar för virtuella datorer för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**För databasservern**: skalbar är tillgänglig via databasen partitionering tekniker och SQL Server AlwaysOn-Tillgänglighetsgrupper.<br/><br/>Du kan använda för tunga skrivskyddade arbetsbelastningar [AlwaysOn Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx) på flera sekundära noder såväl som SQL Server-replikering.<br/><br/>Du kan implementera horisontell partitionering data över flera fysiska servrar tooprovide programmet skalbar för tunga arbetsbelastningar.<br/><br/>Dessutom kan du implementera en skalbar med hjälp av [SQL Server med Data beroende routning](https://technet.microsoft.com/library/cc966448.aspx). Med Data beroende routning (DDR), behöver du tooimplement hello partitionering mekanism i hello klientprogram, vanligtvis i hello business nivå lagret, tooroute hello databasen begär toomultiple SQL Server-noder. hello affärsnivå innehåller mappningar toohow hello data är partitionerad och vilken nod som innehåller hello data.<br/><br/>Du kan skala program som körs på virtuella datorer. Mer information finns i [hur tooScale ett program](../../../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Viktigt meddelande**: hello **Autoskala** funktion i Azure kan du tooautomatically öka eller minska hello virtuella datorer som används av ditt program. Den här funktionen garanterar att hello slutanvändarens upplevelse inte är påverkas negativt under perioder och virtuella datorer måste stängas när hello-begäran är låg. Vi rekommenderar att du inte anger hello Autoskala alternativet för din molntjänst om den innehåller virtuella SQL Server-datorer. hello beror på att hello Autoskala-funktionen kan Azure tooturn på en virtuell dator när hello CPU-användning i den virtuella datorn är högre än vissa tröskelvärde och tooturn av en virtuell dator när hello CPU-användning är lägre än den. hello Autoskala funktionen är användbart för tillståndslösa program, till exempel webbservrar, där någon virtuell dator kan hantera hello arbetsbelastning utan några referenser tooany tidigare tillstånd. Hello Autoskala funktionen är inte användbar för tillståndskänsliga program som SQL Server, där endast en instans kan skriva toohello databas. |Skala upp är tillgänglig genom att använda flera webb- och arbetsroller roller. Mer information om storlekar för virtuella datorer för webb- och arbetsroller finns [konfigurera storlekar för molntjänster](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>När du använder **molntjänster**, du kan definiera flera roller toodistribute bearbetning och även uppnå flexibel skalning av ditt program. Varje tjänst i molnet innehåller en eller flera webbroller och/eller arbetsroller var och en med sin egen programfiler och konfiguration. Du kan skala upp en tjänst i molnet genom att öka hello antalet rollinstanser (virtuella datorer) distribueras för en roll och skala ned en tjänst i molnet genom att minska hello antalet rollinstanser. Detaljerad information finns i [Azure körning modeller](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Skalbar är tillgänglig via inbyggd hög tillgänglighet för Azure-supporten via [molntjänster, virtuella datorer och virtuella nätverk servicenivåavtalet](http://www.microsoft.com/download/details.aspx?id=38427) och belastningsutjämnare.<br/><br/>För en flernivåapp rekommenderar vi att du ansluter web/worker toodatabase programserver virtuella datorer via Azure Virtual Network roller. Azure tillhandahåller dessutom belastningsutjämning för virtuella datorer i hello samma tjänst i molnet, sprida användarförfrågningar över dem. Virtuella datorer som är anslutna på så sätt kan kommunicera direkt med varandra via hello lokalt nätverk i ett Azure-datacenter.<br/><br/>Du kan ställa in **Autoskala** på hello Azure portal samt hello schema gånger. Mer information finns i [hur tooconfigure automatisk skalning för en tjänst i molnet i hello portal](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Skala upp eller ned**: du kan öka/minska hello storleken på hello instans (VM) som reserverats för din webbplats.<br/><br/>Skala ut: du kan lägga till fler reserverade instanser (VM) för webbplatsen.<br/><br/>Du kan ställa in **Autoskala** på hello portal samt hello schema gånger. Mer information finns i [hur tooScale Web Apps](../../../app-service-web/web-sites-scale.md). |

Mer information om att välja mellan metoderna programming finns [Azure Web Apps molntjänster och virtuella datorer: när toouse som](../../../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Nästa steg
Mer information om hur du kör SQL Server i Azure virtuella datorer finns [SQL Server på Azure virtuella datorer – översikt](virtual-machines-windows-sql-server-iaas-overview.md).

