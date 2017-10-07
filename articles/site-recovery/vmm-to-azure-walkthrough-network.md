---
title: "aaaPlan nätverk för Hyper-V (withSystem Center VMM) tooAzure replikering med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskrivs planering krävs när du replikerar virtuella Hyper-V-datorer (med VMM) tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 51ca8b939b6f96880f83599ea8009eb0bfa5b957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-hyper-v-with-vmm-tooazure-replication"></a>Steg 4: Planera nätverk för Hyper-V (med VMM) tooAzure replikering

När du utför [kapacitetsplanering](vmm-to-azure-walkthrough-capacity.md) (om du gör en fullständig distribution), läsa den här artikeln toolearn om nätverket några saker att tänka på när du replikera lokala virtuella Hyper-V-datorer i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) service.

Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="network-mapping-for-replication-tooazure"></a>Nätverksmappningen för replikering tooAzure

Nätverksmappning används vid replikering av Hyper-V virtuella datorer (hanteras i VMM) tooAzure. Mappningar för mappning mellan Virtuella nätverk på en VMM-källservern för nätverk och Azure-målnätverken. Mappningen hello följande:

- **Nätverksanslutning**– efter växling vid fel, alla replikerade virtuella Azure-datorer är anslutna toohello mappade Azure-nätverket. Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, även om de har redundansväxlats i olika återställningsplaner.
- **Nätverksgateway**– om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan virtuella datorer kan ansluta tooother lokala virtuella datorer i hello mappas på beträffande nätverk.

### <a name="prepare-vmm-for-network-mapping"></a>Förbereda VMM för nätverksmappning

Förbered VMM nätverksmappningen på följande sätt:

1. Om du inte har någon förbereda en [logiskt nätverk för VMM](https://docs.microsoft.com/system-center/vmm/network-logical) som associeras med hello moln i vilka hello Hyper-V-värdarna finns.
2. Om du inte har någon, skapas en [Virtuellt datornätverk](https://docs.microsoft.com/system-center/vmm/network-virtual) länkade toohello logiskt nätverk som du har förberett ovan.
3. Ansluta virtuella datorer på hello Hyper-V server/värdkluster i hello toohello Virtuellt datornätverk på VMM-moln.

 
Tänk på följande: 
- Nya virtuella datorer läggs toohello källnätverket ansluts toohello mappade Azure-nätverket när replikeringen sker.
- Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet som hello källa virtual machine finns så hello replikerade virtuella datorn ansluter toothat målundernätverket efter en redundansväxling.
- Om det inte finns något målundernät med ett matchande namn, ansluter hello virtuella toohello första undernätet i hello nätverk.
- Du förbereder ett Azure-nätverk och ställa in nätverksmappningar, när du distribuerar hello scenario.

## <a name="connecting-tooazure-vms-after-failover"></a>Ansluta tooAzure virtuella datorer efter redundans

När du planerar din replikering och redundansstrategi, en av hello viktiga frågor är hur tooconnect toohello virtuella Azure-datorn efter redundans. Det finns ett par alternativ när du utformar din strategi för nätverket för replik virtuella Azure-datorer:

- **Använd en annan IP-adress**: du kan välja toouse en annan IP-adressintervall för hello replikerade Virtuella Azure-nätverk. I det här scenariot hello VM hämtar en ny IP-adress efter växling vid fel och en DNS-uppdatering krävs.
- **Behåll hello samma IP-adress**: du kanske vill toouse hello samma IP-adressintervall som som i din primära lokala nätverk, för hello Azure-nätverket efter redundans.  Att hålla hello samma IP-adresser förenklar hello återställning genom att minska nätverk problemen efter växling vid fel. Men när du replikerar tooAzure måste tooupdate vägar med hello nya plats hello IP-adresser efter växling vid fel.


## <a name="retain-ip-addresses"></a>Behåll IP-adresser

Site Recovery tillhandahåller hello kapaciteten tooretain fasta IP-adresser när växling tooAzure med ett undernät redundans.

Med undernätet redundans finns ett specifikt undernät på plats 1 eller 2 för platsen, men aldrig på båda platser samtidigt. I ordning toomaintain hello IP-adressutrymme i hello händelse av en växling vid fel ordna du programmässigt för hello router infrastruktur toomove hello undernät från en plats tooanother. Under växling vid fel associerade hello undernät flytta med hello skyddade virtuella datorerna. hello huvudsakliga Nackdelen är att i hello händelse av fel du toomove hello hela undernätet.



### <a name="failover-example"></a>Exempel för växling vid fel

Nu ska vi titta på ett exempel på tooAzure för växling vid fel.

- Ett ficticious företag, Woodgrove Bank har en lokal infrastruktur som är värd för sina appar. Sina mobila program finns på Azure.
- Anslutningen mellan Woodgrove Bank virtuella datorer i Azure och lokala servrar som en plats-till-plats (VPN)-anslutning mellan hello lokala edge nätverk och hello virtuella Azure-nätverket.
- Den här VPN-innebär att hello företagets virtuella nätverk i Azure visas som ett tillägg för sina lokala nätverk.
- Woodgrove vill toouse Site Recovery tooreplicate lokala arbetsbelastningar tooAzure.
 - Woodgrove har toodeal med program och konfigurationer som är beroende av hårdkodade IP-adresser och därför behöver tooretain IP-adresser för sina program efter växling vid fel tooAzure.
 - Woodgrove har tilldelats IP-adresser från intervallet 172.16.1.0/24 172.16.2.0/24 tooits resurser som körs i Azure.


Woodgrove toobe kan tooreplicate tooAzure dess virtuella datorer medan behålla hello IP-adresser, är här i vilka hello företaget måste toodo:

1. Skapa ett virtuellt Azure-nätverk. Det bör vara en förlängning av hello lokalt nätverk, så att program kan växla över sömlöst.
2. Azure kan du tooadd plats-till-plats VPN-anslutning dessutom toopoint-till-plats-anslutning toohello virtuella nätverk som skapats i Azure.
3. När du konfigurerar hello plats-till-plats-anslutning i hello Azure nätverk, du kan vidarebefordra trafiken toohello lokal plats (lokalt nätverk) endast om hello IP-adressintervall skiljer sig från hello lokala IP-adressintervall.
    - Det beror på att Azure inte stöder sträckta undernät. Så om du har undernät 192.168.1.0/24 lokalt kan du inte lägga till ett lokalt nätverk 192.168.1.0/24 i hello Azure-nätverk.
    - Detta är förväntat eftersom Azure inte vet att det finns inga aktiva virtuella datorer i hello undernät och hello undernätet skapas för katastrofåterställning endast.
    - toobe kan toocorrectly dirigera nätverkstrafik utanför en Azure-nätverk hello undernät i hello nätverk och hello lokala-nätverk får inte står i konflikt.

![Innan du undernät redundans](./media/vmm-to-azure-walkthrough-network/network-design7.png)

### <a name="before-failover"></a>Innan du redundans

1. Skapa ytterligare ett nätverk (till exempel återställning nätverk). Detta är hello nätverk som redundansväxlade virtuella datorer skapas.
2. tooensure som hello IP-adress för en virtuell dator finns kvar efter en redundansväxling i hello VM Egenskaper > **konfigurera**, ange hello samma IP-adressen som hello VM har lokalt och på **spara**.
3. När hello VM växlas över, tilldelar Azure Site Recovery hello angivna IP-adressen tooit.

    ![Egenskaper för nätverk](./media/vmm-to-azure-walkthrough-network/network-design8.png)

4. När redundansväxlingen är utlösare utlöses och hello virtuella datorer skapas i Azure med hello som krävs för IP-adress, du kan ansluta toohello med en [Vnet tooVnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Den här åtgärden kan skriptas.
5. Vägar måste korrekt ändrade toobe tooreflect som 192.168.1.0/24 har nu flyttats tooAzure.

    ![Efter undernät redundans](./media/vmm-to-azure-walkthrough-network/network-design9.png)

### <a name="after-failover"></a>Efter växling vid fel

Om du inte har ett Azure-nätverk som visas ovan, kan du skapa en plats-till-plats VPN-anslutning mellan den primära platsen och Azure, när failvoer.

## <a name="change-ip-addresses"></a>Ändra IP-adresser

Detta [blogginlägget](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) förklarar hur tooset in hello Azure nätverksinfrastruktur när du inte behöver tooretain IP-adresser efter växling vid fel. Den börjar med en beskrivning av programmet, ser på hur tooset in nätverk lokalt och i Azure och avslutas med information om att köra redundansväxlingar.  

## <a name="next-steps"></a>Nästa steg

Gå för[steg 5: Förbered Azure](vmm-to-azure-walkthrough-prepare-azure.md)
