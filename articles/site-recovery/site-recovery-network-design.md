---
title: "aaaDesigning nätverkets infrastruktur för katastrofåterställning | Microsoft Docs"
description: "Den här artikeln beskrivs nätverksdesign för Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pratshar
ms.openlocfilehash: 18bafa1b8b334192bfaf2f4aeba9f090011041ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>Utforma nätverket för katastrofåterställning

Den här artikeln är riktat tooIT tekniker som ansvarar för att bygga, implementera och stöd för affärskontinuitet och haveriberedskap (BCDR) infrastruktur och som vill tooleverage Microsoft Azure Site Recovery (ASR) toosupport och utöka sina BCDR-tjänster. Den här artikeln beskrivs praktiska överväganden i strukturera nätverk på återställningsplatsen för katastrofåterställning, är det Azure eller en annan lokal plats. 

## <a name="overview"></a>Översikt
[Azure Site Recovery (ASR)](https://azure.microsoft.com/services/site-recovery/) är en Microsoft Azure-tjänst som samordnar hello skydd och återställning av din virtualiserade program för affärsändamål kontinuitet disaster recovery (BCDR). Det här dokumentet är avsett tooguide hello läsaren genom hello processen med att skapa hello nätverk fokusera på att bygga om IP-adressintervall och undernät på hello disaster recovery platsen vid replikering av virtuella datorer (VM) med Site Recovery.

Dessutom visar den här artikeln hur Site Recovery kan utforma och implementera en multisite virtuella datacenter toosupport BCDR services när test och katastrofåterställning.

I en värld där alla förväntar sig 24/7 anslutning, är viktigare än någonsin tookeep din infrastruktur och program som är igång. hello syftar Business Continuity and Disaster Recovery BCDR-toorestore misslyckades komponenter så hello organisation kan snabbt återuppta normal drift. Utveckla disaster recovery strategier toodeal med osannolikt, förödande händelser är väldigt svårt. Detta är på grund av toohello inbyggd svårigheten att förutsäga framtida hello, särskilt när det gäller tooimprobable händelser och hello hög kostnad tooprovide lämpliga åtgärder för skydd mot omfattande catastrophes.

Avgörande för BCDR planering, återställning tid mål för Återställningstid och återställning punkt mål (RPO) måste definieras som en del av en plan för katastrofåterställning. När en katastrof inträffar hello kundens datacenter med hjälp av Azure Site Recovery kunder kan snabbt (låga RTO) Anslut de replikerade virtuella datorer som finns i hello sekundärt Datacenter eller Microsoft Azure med minimal dataförlust (låg RPO).

Redundans möjliggörs med ASR som från början kopierar avsedda virtuella datorer från hello primära center toohello sekundära Datacenter eller tooAzure (beroende på scenario hello) och regelbundet uppdaterar hello repliker. Under infrastrukturplanering, bör nätverksdesign betraktas som potentiell flaskhals som kan förhindra att du från möte företag RTO och Återställningspunktmål mål.  

När administratörer planerar toodeploy en lösning för katastrofåterställning, är en av hello viktiga frågor i minne hur hello virtuella datorn skulle kunna nås när hello växling vid fel har slutförts. ASR kan Hej administratör toochoose hello nätverket toowhich en virtuell dator är ansluten tooafter växling vid fel. Om hello primära platsen är Azure eller också är en lokal plats som hanteras av en VMM-server och sedan detta uppnås med hjälp av nätverksmappning. Lär dig mer om [nätverksmappning i Azure tooAzure DR](site-recovery-network-mapping-azure-to-azure.md) och [nätverksmappning från VMM](site-recovery-network-mapping.md)


När du utformar hello nätverk för hello återställningsplatsen har hello-administratören två alternativ:

* Använd en annan IP-adressintervall för hello nätverk på återställningsplatsen. I det här scenariot hello virtuella datorn efter redundans får en ny IP-adress och Hej administratör skulle ha toodo en DNS-uppdatering. 
* Använd samma IP-adressintervall för hello nätverk på hello återställningsplatsen. I vissa fall föredrar administratörer tooretain hello IP-adresser som de har på hello primär plats även efter hello växling vid fel. En administratör skulle ha tooupdate hello vägar tooindicate hello nya plats hello IP-adresser i ett vanligt scenario. Men i hello scenario där ett sträckta undernät distribueras mellan hello primära och hello återställningsplatser behålla hello IP-adresser för hello virtuella datorer blir ett bra alternativ. Det går inte att sträcka ut ett undernät från ett lokalt nätverk tooan Azure-nätverk eller mellan två Azure-nätverk.  

När administratörer planerar toodeploy en lösning för katastrofåterställning, är en hello viktiga frågor sina tänka på hur hello program kan nås när hello växling vid fel har slutförts. Moderna program är nästan alltid beroende nätverk toosome grad fysiskt flytta en tjänst från en plats tooanother representerar en nätverk utmaning. Det finns två huvudsakliga sätt som det här problemet behandlas i lösningar för katastrofåterställning. hello första tillvägagångssättet är toomaintain fasta IP-adresser. Trots hello tjänster flytta och hello som värd för servrar i olika fysiska platser, ta program hello IP-adresskonfiguration med dem toohello ny plats. hello andra sättet är att helt ändra hello IP-adress under hello övergången till hello återställda platsen. Varje metod har flera varianter av implementering som sammanfattas nedan.

När du utformar hello nätverk för hello återställningsplatsen har hello-administratören två alternativ:

## <a name="option-1-retain-ip-addresses"></a>Alternativ 1: Behåll IP-adresser
Ur disaster recovery processen med fast IP-adresser visas toobe hello enklaste metoden tooimplement, men det finns ett antal potentiella problem som gör det i praktiken hello minst populär metod. Azure Site Recovery tillhandahåller hello kapaciteten tooretain hello IP-adresser i samtliga scenarier. Innan en beslutar tooretain IP lämpliga bör du tänka toohello begränsningar inför på hello funktioner för redundans. Låt oss se hello faktorer som kan hjälpa dig att toomake ett beslut tooretain IP-adresser, eller inte. Detta kan ske på två sätt med hjälp av utsträckta undernät eller genom att göra en fullständig undernät växling vid fel.

### <a name="stretched-subnet"></a>Ambitiöst undernät
Här görs hello undernät tillgängliga samtidigt i både primär och DR-platser. Detta innebär i enkla villkor kan du flytta en server och dess IP (nivå 3) konfiguration toohello andra platsen och hello nätverket dirigerar hello trafik toohello ny plats automatiskt. Detta är trivial toodeal med ur server men det finns ett antal utmaningar:

* Det kräver nätverksutrustning som kan hantera ett ambitiöst VLAN ur Lager2 (data-länk lager), men det har blivit mindre problem som den är allmänt tillgänglig. hello andra och svårare problemet är att genom att dra ut hello VLAN hello potentiella feldomän utökas tooboth platser, i stort sett blir en enskild felpunkt. Även om detta är ett ovanligt inträffat att broadcast storm startats men det gick inte att isolerade. Vi har hittat blandat åsikter om problemet senaste och har sett många lyckade implementeringar samt ”vi kommer aldrig att implementera den här tekniken här”.
* Det går inte att sträckta undernät om du använder Microsoft Azure som hello DR-plats.

### <a name="subnet-failover"></a>Undernät växling vid fel
Det är möjligt tooimplement undernät redundans tooobtain hello fördelarna med hello utsträckt undernät lösningen som beskrivs ovan utan att sträcka ut hello undernät på flera platser. Här skulle några angivet undernät finnas på plats 1 eller 2 för platsen, men aldrig på båda platser samtidigt. I ordning toomaintain hello IP-adressutrymme i hello händelse av en växling vid fel, är det möjligt tooprogrammatically ordna för hello router infrastruktur toomove hello undernät från en plats tooanother. I ett redundanskluster scenariot hello undernät skulle associerade flytta med hello skyddade virtuella datorerna. hello huvudsakliga nackdelen toothis metod är i hello händelse av fel du har toomove hello hela undernät, vilket kan vara OK, men det kan påverka hello redundans granularitet överväganden.

Låt oss nu undersöka hur ett fiktivt företag med namnet Contoso är kan tooreplicate dess virtuella datorer tooa återställningsplats när misslyckande hello hela undernätet. Först beskrivs hur Contoso är kan toomanage sina undernät medan replikera virtuella datorer mellan två lokala platser och sedan diskuteras hur undernät redundans fungerar när [Azure används som hello disaster recovery plats](#failover-to-azure).

#### <a name="failover-from-on-premises-tooazure"></a>Växling vid fel från lokala tooAzure 
Azure Site Recovery (ASR) kan Azure toobe används som en återställningsplats för katastrofåterställning för dina virtuella datorer.  

Låt oss nu undersöka ett scenario där fiktiva företaget Woodgrove Bank har lokal infrastruktur som är värd för sina verksamhetsspecifika program och de är värd för sin mobila program på Azure. Anslutningen mellan Woodgrove Bank virtuella datorer i Azure och lokala servrar som en plats-till-plats (S2S) virtuella privata nätverk (VPN) eller ExpressRoute. S2S VPN tillåter Woodgrove Bank virtuellt nätverk i Azure toobe ses som ett tillägg på Woodgrove Bank lokalt nätverk. Den här kommunikationen aktiveras av S2S VPN mellan Woodgrove Bank kant och virtuella Azure-nätverket. Nu vill Woodgrove toouse ASR tooreplicate dess arbetsbelastningar som körs primära Azure-region tooanother Azure-region. Det här alternativet uppfyller hello behoven hos Woodgrove som vill ha ett ekonomiska DR-alternativ och kan toostore data i offentliga molnmiljöer. Woodgrove har toodeal med program och konfigurationer som är beroende av hårdkodade IP-adresser, därför de har ett krav tooretain IP-adresser för sina program efter misslyckas över tooanother region i Azure.

Woodgrove har beslutat tooassign IP-adresser i IP-adressintervallet (172.16.1.0/24, 172.16.2.0/24) tooits resurser som körs i Azure.

För Woodgrove toobe kan tooreplicate dess tooAzure för virtuella datorer samtidigt hello IP-adresser, ett Azure Virtual Network måste toobe skapas. Det bör vara en förlängning av hello lokalt nätverk så att program kan redundansväxla från hello lokal plats tooAzure sömlöst. Azure kan du tooadd plats-till-plats samt punkt-till-plats VPN-anslutningen toohello virtuella nätverk som skapats i Azure. När du konfigurerar din plats-till-plats-anslutning, kan Azure-nätverk du tooroute trafik toohello lokal plats (Azure anropar den lokala nätverk) endast om hello IP-adressintervall skiljer sig från hello lokala IP-adressintervall, eftersom Azure inte stöd sträcka ut undernät.  Det innebär att om du har ett undernät 192.168.1.0/24 lokala, det går inte att lägga till ett lokalt nätverk 192.168.1.0/24 i hello Azure-nätverk. Detta är förväntat eftersom Azure inte vet att det finns inga aktiva virtuella datorer i hello undernät och hello undernätet skapas endast för Katastrofåterställning. toobe kan toocorrectly dirigera nätverkstrafik utanför en Azure-nätverk hello undernät i hello nätverks- och hello lokala nätverk inte vara i konflikt.

![Innan du undernät redundans](./media/site-recovery-network-design/network-design7.png)

Innan du redundans

toohelp Woodgrove uppfyller deras affärskrav måste vi tooimplement hello följande arbetsflöden:

* Skapa ytterligare ett nätverk, låt oss anropa Recovery nätverk, där hello misslyckades över virtuella datorer skapas.
* tooensure att hello IP för en virtuell dator finns kvar efter en växling, gå toohello konfigurera på fliken Egenskaper för Virtuella datorer genom att ange hello samma IP-adress som hello VM har lokalt och klicka på Spara. När hello VM växlas över, tilldelar Azure Site Recovery hello angivna IP-toohello virtuella datorn.

![Egenskaper för nätverk](./media/site-recovery-network-design/network-design8.png)

När hello växling vid fel utlöses och hello virtuella datorer skapas i hello Recovery nätverk med hello önskad IP-anslutning toothis nätverk kan upprättas med hjälp av en [Vnet tooVnet anslutning](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Om det krävs för den här åtgärden kan skriptas.  Enligt beskrivningen i hello föregående avsnitt om undernätet redundans, även i hello fall av redundans tooAzure vägar skulle ha toobe korrekt ändrade tooreflect har som 192.168.1.0/24 nu flyttats tooAzure.

![Efter undernät redundans](./media/site-recovery-network-design/network-design9.png)

Efter växling vid fel

Om du inte har ett Azure nätverk som hello bilden ovan. Du kan skapa en plats toosite VPN-anslutning mellan 'Primära platsen' och 'Recovery nätverket' efter hello växling vid fel.  


#### <a name="failover-tooa-secondary-on-premises-site"></a>Redundans tooa sekundär lokal plats
Låt oss titta på ett scenario där vi vill behålla hello IP för varje hello virtuella datorer och failover hello Slutför undernät tillsammans. hello primär webbplats har program som körs i undernätet 192.168.1.0/24. När hello redundans inträffar alla hello virtuella datorer som ingår i det här undernätet kommer att redundansväxlas toohello återställningsplatsen och behålla sina IP-adresser. Vägar har toobe korrekt ändrade tooreflect hello faktum att alla hello virtuella datorer som tillhör toosubnet 192.168.1.0/24 har nu flyttats toohello återställningsplatsen.

Följande bild hello dirigerar mellan primära platsen och återställningsplatsen, tredje och primär plats i hello och tredje platsen och återställningsplatsen måste toobe ändras på lämpligt sätt.

hello följande bilder visar hello undernät innan hello redundans. Undernät 192.168.0.1/24 är aktiv på hello primär plats innan hello redundans och aktiveras av hello återställningsplatsen efter hello redundans

![Innan du redundans](./media/site-recovery-network-design/network-design2.png)

Innan du redundans

hello bilden nedan visar nätverk och undernät efter växling vid fel.

![Efter växling vid fel](./media/site-recovery-network-design/network-design3.png)

Efter växling vid fel

I din sekundära plats är lokalt och du använder en VMM-servern toomanage därefter när aktiverar skydd för en specifik virtuell dator, ASR allokeras nätverksresurser enligt toohello följande arbetsflöde:

* ASR tilldelar en IP-adress för varje nätverksgränssnitt på den virtuella datorn hello från hello statisk IP-adresspool definierats hello relevanta nätverket för varje System Center VMM-instans.
* Om hello administratören definierar hello samma IP-adresspoolen för hello nätverk på hello återställningsplatsen som att hello IP-adresspool för hello nätverk på hello primär plats när hello IP-adress toohello replik virtuella ASR skulle allokera hello samma IP-adress som hello primära virtuella datorn.  hello IP är reserverad i VMM men har angetts inte som redundans IP på hello Hyper-v-värd. Failover IP på en Hyper-v-värden anges innan hello växling vid fel.


Om hello samma IP inte är tillgänglig, skulle ASR allokera vissa andra tillgänglig IP-adress från hello definierats IP-adresspool.

Du kan använda följande exempel skriptet tooverify hello IP-adress som har allokerats toohello virtuella datorn efter hello VM har aktiverats för skydd. hello samma IP skulle ange Failover IP och tilldelats toohello VM hello tiden för växling vid fel:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> I hello scenario där virtuella datorer använder DHCP, är hello hantering av IP-adresser helt utanför hello kontroll över ASR. En administratör har tooensure hello DHCP-servern betjänar hello IP-adresser på hello återställningsplatsen kan fungera från hello samma intervall som hello primär plats.
>
>



## <a name="option-2-changing-ip-addresses"></a>Alternativ 2: Ändra IP-adresser
Den här metoden verkar toobe hello vanligaste baserat på vad vi sett. Det tar hello form av ändra hello IP-adress för varje virtuell dator som ingår i hello växling vid fel. En nackdel med den här metoden kräver hello inkommande nätverk too'learn' hello programmet som skedde vid IPx är nu vid IPy. Även om IPx och IPy är logiska namn, DNS-poster har vanligtvis toobe ändras eller rensade genom hello nätverk och cachelagrade poster i nätverket tabeller har toobe uppdateras eller rensade, därför ett driftstopp kan ses beroende på hur hello DNS-infrastrukturen har ställts in. De här problemen kan begränsas med hjälp av låg TTL-värden i hello skiftläget för intranätsprogram och använder [Azure Traffic Manager med ASR](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) för internet-baserade program

### <a name="changing-hello-ip-addresses---illustration"></a>Ändra hello IP-adresser - bild
Låt oss titta på hello scenario där du planerar toouse olika IP-adresser över hello primära och hello recovery platser. I följande exempel hello har vi också en tredje plats där hello-program som finns på den primära eller återställning platsen kan nås.

![Olika IP - före redundans](./media/site-recovery-network-design/network-design10.png)


Hello bilden ovan finns det vissa program som finns i undernät 192.168.1.0/24 undernät på hello primära platsen och de har konfigurerade toocome in på hello återställningsplatsen i undernätet 172.16.1.0/24 efter en redundansväxling. VPN-anslutningar/nätverksvägar har konfigurerats på rätt sätt så att alla tre platser som har åtkomst till varandra.

De kommer att återställas i hello recovery undernät för som hello bilden nedan visas efter ett eller flera program inte körs. I det här fallet är inte begränsad toofailover hello hela undernätet på hello samtidigt. Inga ändringar är nödvändiga tooreconfigure VPN eller nätverksvägar. En växling vid fel och DNS-uppdateringar ska kontrollera att program fortfarande är tillgänglig. Om hello DNS är konfigurerade tooallow dynamiska uppdateringar skulle sedan hello virtuella datorer registrera sig med hello nya IP när de startar efter en redundansväxling.

![Olika IP - efter växling vid fel](./media/site-recovery-network-design/network-design11.png)


När misslyckas över hello replikerade virtuella datorn kan ha hello en IP-adress som inte är samma som hello IP-adress för hello primär virtuell dator. Virtuella datorer uppdateras hello DNS-server som de använder när de startar. DNS-poster har vanligtvis toobe ändras eller rensade genom hello nätverk och cachelagrade poster i nätverket tabeller har toobe uppdateras eller rensade, så det inte är ovanligt toobe inför driftstopp medan de här ändringarna äga rum. Det här problemet kan begränsas med:

* Med hjälp av låg TTL-värden för intranätsprogram.
* Med ASR Azure Traffic Manager för internet-baserade program.
* Med hjälp av hello följande skript i din återställningspunkt planera tooupdate hello DNS-Server tooensure en rimlig uppdatering (hello skript är inte nödvändigt om hello dynamisk DNS-registrering har konfigurerats)

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-hello-ip-addresses--dr-tooazure"></a>Ändra hello IP-adresser – DR tooAzure
Hej [nätverk Infrastrukturinställningar för Microsoft Azure som en plats för Disaster Recovery](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) blogginlägget beskriver hur toosetup hello krävs Azure nätverksinfrastruktur när behåller IP-adresser är inte ett krav. Det börjar med som beskriver hello program och sedan titta på hur toosetup nätverk lokalt och på Azure och sedan sluta med toodo testa redundans och en planerad redundans.

## <a name="next-steps"></a>Nästa steg
[Läs](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) hur Site Recovery mappar käll- och nätverk när en VMM-server används toomanage hello primär plats.
