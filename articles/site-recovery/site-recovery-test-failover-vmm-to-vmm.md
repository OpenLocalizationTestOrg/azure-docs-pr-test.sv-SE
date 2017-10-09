---
title: "aaaTest växling vid fel (VMM tooVMM) i Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery samordnar hello replikering, redundans och återställning av virtuella datorer och fysiska servrar. Läs mer om tooAzure för växling vid fel eller ett sekundärt datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.openlocfilehash: 6b4f65ab692cbb0665102c4f51ea0694151cd3ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-failover-vmm-toovmm-in-site-recovery"></a>Testa redundans (VMM tooVMM) i Site Recovery


Den här artikeln innehåller information och instruktioner för att göra ett redundanstest eller visa disaster recovery (DR) av virtuella datorer (VM) och fysiska servrar som skyddas med Azure Site Recovery. Du ska använda en System Center Virtual Machine Manager VMM-hanterade lokal plats som hello återställningsplatsen.

Du kör ett test redundans toovalidate din replikeringsstrategi för eller utföra en DR-test utan avbrott eller dataförlust. Testa redundans har inte någon effekt på hello pågående replikering eller i din produktionsmiljö. Du kan köra den på en virtuell dator eller en [återställningsplanen](site-recovery-create-recovery-plans.md). När du utlöst ett redundanstest måste toospecify hello nätverk som de virtuella testdatorer som hello ska ansluta till. Du kan följa hello förloppet hello testningen på hello **jobb** sidan.  

Om du har kommentarer eller frågor, publicera dem längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hello-infrastructure-for-test-failover"></a>Förbereda hello infrastrukturen för att testa redundans
Om du vill toorun ett redundanstest genom att använda ett befintligt nätverk kan du förbereda Active Directory, DHCP och DNS i nätverket.

Om du vill toorun testa redundans med hjälp av hello alternativet toocreate VM-nätverk automatiskt kan du lägga till en manuell åtgärd innan grupp 1 i hello återställningsplan att du ska toouse för hello testning av redundans. Lägg sedan till hello infrastruktur resurser toohello skapas automatiskt nätverket innan du kör hello testa redundans.

### <a name="things-toonote"></a>Saker toonote
När du replikerar tooa sekundär plats, hello typen av nätverk att hello replik datorn använder inte behöver toomatch hello typ av logiska nätverk som används för att testa redundans, men vissa kombinationer kanske inte fungerar. Om hello replik använder DHCP- och VLAN-baserad isolering, behöver inte en statisk IP-adresspool hello Virtuellt datornätverk för hello replik. Det med hjälp av Windows-Nätverksvirtualisering för att testa redundans hello fungerar inte eftersom ingen adresspool är tillgängliga. 

Dessutom fungerar hello testa redundans inte om hello replik nätverket använder ingen isolering och hello testnätverket använder Windows-Nätverksvirtualisering. Det beror på att hello no isolation nätverket har hello undernät krävs toocreate ett nätverk för Windows-Nätverksvirtualisering.

Hur replikerade virtuella datorerna är anslutna toomapped VM-nätverk efter växling vid fel som beror på hur hello VM-nätverk är konfigurerad i hello VMM-konsolen.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Virtuellt datornätverk utan isolering eller VLAN-isolering
Om DHCP har definierats för hello Virtuella datornätverket är hello replikerade virtuella datorn ansluten toohello VLAN-ID via hello-inställningarna som har angetts för hello nätverksplats i hello associerat logiskt nätverk. hello virtuell dator tar emot dess IP-adress från hello tillgänglig DHCP-server. 

Du behöver inte toodefine en statisk IP-adresspool för hello mål VM-nätverk. Om du använder en statisk IP-adresspool för hello VM-nätverk, är hello replikerade virtuella datorn ansluten toohello VLAN-ID via hello-inställningarna som har angetts för hello nätverksplats i hello associerat logiskt nätverk.

hello virtuell dator tar emot dess IP-adress från hello-pool som har definierats för hello VM-nätverk. Om en statisk IP-adresspool har inte definierats i hello mål VM-nätverk, misslyckas IP-adressallokering. Skapa hello IP-adresspool på båda hello käll- och VMM-servrar som du ska använda för skydd och återställning.

#### <a name="vm-network-with-windows-network-virtualization"></a>Virtuellt datornätverk med Nätverksvirtualisering i Windows
Om ett Virtuellt datornätverk är konfigurerad med Windows-Nätverksvirtualisering, bör du definiera en statisk adresspool för hello mål VM-nätverk, oavsett om hello källnätverket är konfigurerade toouse DHCP eller en statisk IP-adresspool. 

Om du definierar DHCP hello VMM-målservern fungerar som en DHCP-server och tillhandahåller en IP-adress hello-pool som har definierats för hello målnätverket VM. Om användning av en statisk IP-adresspool har definierats för hello källserver allokerar en IP-adress från hello poolen hello VMM-målservern. I båda fallen misslyckas IP-adressallokering om en statisk IP-adresspool inte har definierats.


### <a name="prepare-dhcp"></a>Förbereda DHCP
Om hello virtuella datorer som är involverad i testa redundans använda DHCP, skapa en test DHCP-server inom hello isolerat nätverk i hello syftet att testa redundans.

### <a name="prepare-active-directory"></a>Förbereda Active Directory
toorun ett redundanstest för program, testa, behöver du en kopia av Active Directory-produktionsmiljö hello i din testmiljö. Mer information finns i hello [redundanstestning för Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>Förbereda DNS
Förbered en DNS-server för att testa redundans hello på följande sätt:

* **DHCP**: om virtuella datorer använder DHCP hello IP-adressen för hello test DNS ska uppdateras på hello test DHCP-server. Om du använder en nätverkstyp av Nätverksvirtualisering för Windows fungerar hello VMM-servern som hello DHCP-server. Hello IP-adressen för DNS ska vara uppdaterade i hello nätverket. I det här fallet hello virtuella datorer kan registrera sig själva toohello relevanta DNS-server.
* **Statisk adress**: om virtuella datorer använder en statisk IP-adress, hello IP-adressen för hello-test som ska vara uppdaterade DNS-server i nätverket. Du kan behöva tooupdate DNS med hello IP-adressen för hello virtuella testdatorer. Du kan använda följande exempelskript för detta ändamål hello:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Köra ett redundanstest
Den här proceduren beskriver hur toorun ett redundanstest för en återställning planerar. Du kan också köra hello redundans för en enskild virtuell dator på hello **virtuella datorer** fliken.

![Testa redundans-bladet](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Välj **Återställningsplaner** > *recoveryplan_name*. Klicka på **redundans** > **Redundanstestningen**.
1. På hello **Redundanstestning** bladet ange hur virtuella datorer måste vara anslutna toonetworks efter hello testa redundans. Mer information finns i hello [alternativ](#network-options-in-site-recovery).
1. Följa redundansförloppet på hello **jobb** fliken.
1. När redundansväxlingen är klar kan du kontrollera att hello virtuella datorer starta korrekt.
1. När du är klar klickar du på **Rensa redundanstestet** på hello återställningsplan. I **anteckningar**, registrera och spara observationer från redundanstestningen hello. Det här steget tar bort hello virtuella datorer och nätverk som har skapats under redundanstestningen.


## <a name="network-options-in-site-recovery"></a>Nätverksalternativ i Site Recovery

När du kör ett redundanstest uppmanas du tooselect nätverksinställningar för replik testdatorer. Du har flera alternativ.  

| **Testa redundans alternativet** | **Beskrivning** | **Kontrollera redundans** | **Detaljer** |
| --- | --- | --- | --- |
| **Växla över tooa sekundär VMM-plats--utan nätverk** |Markera inte ett Virtuellt datornätverk. |Växling vid fel kontrollerar du att testdatorer skapas.<br/><br/>hello testa virtuell dator skapas på hello värd där hello replikerade virtuella datorn finns. Den är inte läggas till toohello moln där hello replikerade virtuella datorn finns. |<p>hello misslyckades över datorn är inte anslutna tooany nätverk.<br/><br/>hello datorn kan vara anslutna tooa Virtuella datornätverket när den har skapats. |
| **Växla över tooa sekundär VMM-plats--med nätverk** |Välj ett befintligt Virtuellt datornätverk. |Växling vid fel kontrollerar du att virtuella datorer skapas. |hello testa virtuell dator skapas på hello värd där hello replikerade virtuella datorn finns. Den är inte läggas till toohello moln där hello replikerade virtuella datorn finns.<br/><br/>Skapa ett Virtuellt datornätverk som är isolerat från produktionsnätverket.<br/><br/>Om du använder ett VLAN-baserade nätverk, rekommenderar vi att du skapar ett separat logiskt nätverk (används inte i produktion) i VMM för detta ändamål. Den här logiska nätverket är används toocreate Virtuella datornätverk för redundanstestning.<br/><br/>hello logiska nätverket ska kopplas till minst en av hello nätverkskort för alla hello Hyper-V-servrar som är värd för virtuella datorer.<br/><br/>Hej nätverksplatser som du lägger till isoleras toohello logiskt nätverk för logiska VLAN-nätverk.<br/><br/>Om du använder ett Windows-Nätverksvirtualisering – logiska nätverk med skapar Azure Site Recovery automatiskt isolerade Virtuella nätverk. |
| **Växla över tooa sekundär VMM-plats--skapa ett nätverk** |En tillfällig testnätverket skapas automatiskt utifrån hello-inställningen som du anger i **logiskt nätverk** och dess relaterade nätverksplatser. |Växling vid fel kontrollerar du att virtuella datorer skapas. |Använd det här alternativet om hello återställningsplan använder mer än ett Virtuellt datornätverk. Om du använder Windows nätverksvirtualiseringsnätverk kan det här alternativet automatiskt skapa Virtuella datornätverk med hello samma inställningar (undernät och IP-adresspooler) i hello nätverk av hello replikerade virtuella datorn. Dessa Virtuella datornätverk rensas automatiskt efter hello redundanstestet har slutförts.</p><p>hello testa virtuell dator skapas på hello värd där hello replikerade virtuella datorn finns. Den är inte läggas till toohello moln där hello replikerade virtuella datorn finns. |

> [!TIP]
> hello IP-adress anges tooa virtuella datorn under redundanstestningen är hello samma IP-adress som hello virtuell dator kan få en planerad eller oplanerad växling vid fel (förutsatt att hello IP-adress är tillgänglig i nätverket hello). Om hello samma IP-adress inte är tillgänglig i nätverket hello får en annan IP-adress som är tillgänglig i nätverket hello hello virtuell dator.
>
>


## <a name="test-failover-tooa-production-network-on-a-recovery-site"></a>Testa redundans tooa produktionsnätverk på en återställningsplats
Vi rekommenderar att du väljer ett nätverk som skiljer sig från nätverket produktion recovery plats som du angav i när du utför ett redundanstest [nätverksmappning](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Men om du verkligen toovalidate slutpunkt till slutpunkt nätverksanslutningen på en virtuell dator misslyckades över hello följande punkter:

* Kontrollera att hello den primära virtuella datorn stängs av när du gör hello testa redundans. Om du inte två virtuella datorer med samma identitet som ska köras i samma nätverk på hello hello hello samma tid. Denna situation kan leda tooundesired konsekvenser.
* Alla ändringar du gör toohello testa redundans virtuella datorer går förlorade när du rensar hello testa redundans virtuella datorer. Dessa ändringar är inte replikeras tillbaka toohello primära virtuella datorn.
* Det här kan du göra testning leder toodowntime för ditt produktionsprogram. Be användare av programmet hello inte toouse hello programmet när hello DR detaljgranska pågår.  


## <a name="next-steps"></a>Nästa steg
När du har kört ett redundanstest kan du prova en [redundans](site-recovery-failover.md).
