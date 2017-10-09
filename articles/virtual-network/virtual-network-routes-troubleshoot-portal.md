---
title: "aaaTroubleshoot vägar - Portal | Microsoft Docs"
description: "Lär dig hur tootroubleshoot vägar i hello Azure Resource Manager distribution modellen med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bdd8b6dc-02fb-4057-bb23-8289caa9de89
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 579bae91ef3200852032b3953d3cc5d26deada86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-hello-azure-portal"></a>Felsöka flöden med hjälp av hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
> * [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
>
>

Om det uppstår problem tooor för network connectivity från din Azure virtuell dator (VM) kan vägar påverka din VM trafikflöden. Den här artikeln innehåller en översikt över diagnostikfunktionerna för vägar toohelp fortsätta felsökningen.

Vägtabeller är kopplad till undernät och börjar gälla på alla nätverksgränssnitt (NIC) i det undernätet. hello följande typer av routrar kan vara tillämpade tooeach nätverksgränssnittet:

* **Systemvägar:** varje undernät som skapats i ett Azure Virtual Network (VNet) har som standard system-routningstabeller som Tillåt lokal VNet-trafik, lokal trafik via VPN-gatewayer och Internet-trafik. Det finns också systemvägar för peerkoppla Vnet.
* **BGP-vägar:** sprids toonetwork gränssnitt via ExpressRoute eller plats-till-plats VPN-anslutningar. Mer information om BGP-routning genom att läsa hello [BGP med VPN-gatewayer](../vpn-gateway/vpn-gateway-bgp-overview.md) och [ExpressRoute översikt](../expressroute/expressroute-introduction.md) artiklar.
* **Användardefinierade vägar (UDR):** om du använder virtuella installationer i nätverket eller är Tvingad tunneltrafik trafik tooan lokalt nätverk via ett plats-till-plats-VPN, kan du ha användardefinierade vägar (udr: er) som är associerade med din routningstabellen för undernätet. Om du inte är bekant med udr: er kan läsa hello [användardefinierade vägar](virtual-networks-udr-overview.md#user-defined-routes) artikel.

Med hello olika vägar som kan vara tillämpade tooa nätverksgränssnitt, det kan vara svårt toodetermine vilka sammanställd vägar är effektiva. toohelp felsöka anslutningar för VM-nätverk kan du visa alla hello effektiva vägar för ett nätverksgränssnitt i hello Azure Resource Manager-distributionsmodellen.

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a>Med hjälp av effektiva vägar tootroubleshoot VM trafikflöde
Den här artikeln använder hello följande scenario som ett exempel tooillustrate hur tootroubleshoot hello gällande vägar för ett nätverksgränssnitt:

En virtuell dator (*VM1*) ansluten toohello VNet (*VNet1*, prefix: 10.9.0.0/16) misslyckas tooconnect tooa VM(VM3) i ett nyligen peered VNet (*VNet3*, prefixet 10.10.0.0/16). Det finns inga udr: er eller BGP-vägar tillämpade tooVM1 NIC1 nätverk gränssnitt som är anslutet toohello VM, endast systemvägar tillämpas.

Den här artikeln förklarar hur toodetermine hello orsaken till hello anslutningsfel med effektiva vägar funktionen i Azure Resource Manager-distributionsmodellen.
Medan hello exemplet används endast systemvägar, hello samma steg kan vara används toodetermine inkommande och utgående anslutningsfel via väg typer.

> [!NOTE]
> Om den virtuella datorn har mer än ett nätverkskort anslutet, kontrollerar du effektivt vägar för varje hello nätverkskort toodiagnose network connectivity problem tooand från en virtuell dator.
>
>

### <a name="view-effective-routes-for-a-virtual-machine"></a>Visa effektiva flöden för en virtuell dator
toosee hello sammanställd vägar som är kopplade tooa VM, fullständig hello följande steg:

1. Inloggningen toohello Azure-portalen på https://portal.azure.com.
2. Klicka på **fler tjänster**, klicka på **virtuella datorer** i hello-lista som visas.
3. Markera en VM-tootroubleshoot hello listan som visas och en VM-bladet med alternativ visas.
4. Klicka på **diagnostisera & lösa problem** och välj sedan ett vanligt problem. I det här exemplet **jag kan inte ansluta toomy Windows VM** är markerad.

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)
5. Stegen visas under hello problem, som visas i följande bild hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klicka på *effektiva vägar* i hello lista över rekommenderade steg.
6. Hej **effektiva vägar** bladet visas som i följande bild hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Om den virtuella datorn har bara ett nätverkskort, är den markerad som standard. Om du har mer än ett nätverkskort markerar du hello NIC som du vill tooview hello effektiva vägar.

   > [!NOTE]
   > Om hello VM som är associerade med hello NIC är inte i körningstillstånd, effektiv vägar ska inte visas. Endast som hello första 200 effektiva vägar visas i hello-portalen. Hello fullständig lista, klickar du på **hämta**. Du kan filtrera ytterligare på hello resultaten från hello hämtade CSV-fil.
   >
   >

    Observera följande hello i hello utdata:

   * **Källan**: Anger hello typ för vägen. Systemvägar visas som *standard*, udr: er visas som *användare* och gateway-vägar (statisk eller BGP) visas som *VPNGateway*.
   * **Tillstånd**: Visar hello effektiva vägen. Möjliga värden är *Active* eller *ogiltigt*.
   * **Matrisen AddressPrefixes**: Anger hello-adressprefix för hello effektiva vägen i CIDR-notering.
   * **nextHopType**: Anger hello nästa hopp för hello angivna vägen. Möjliga värden är *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, eller *Null*. Ett värde av *Null* för **nextHopType** i en UDR kan tyda på en ogiltig väg. Till exempel om **nextHopType** är *VirtualAppliance* och hello virtuell nätverksenhet virtuella datorn inte är i tillståndet etablerats/körs. Om **nextHopType** är *VPNGateway* och det finns ingen gateway etablerats/körs i hello anges VNet, hello flödet kan bli ogiltig.
7. Det finns ingen väg visas toohello *WestUS VNET3* VNet (10.10.0.0/16 Prefix) från hello *WestUS VNet1* (Prefix 10.9.0.0/16) i hello bild i hello föregående steg. I följande bild hello, hello peering länken är i hello *frånkopplad* tillstånd:

    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    hello dubbelriktad länk för hello peering har brutits, som förklarar varför VM1 gick inte att ansluta tooVM3 i hello *WestUS VNet3* VNet.
8. hello följande bild visar hello vägar när du har etablerat hello dubbelriktad peering länk:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

För mer felsökning scenarier för Tvingad tunneltrafik och flöde utvärdering läsa hello [överväganden](virtual-network-routes-troubleshoot-portal.md#considerations) i den här artikeln.

### <a name="view-effective-routes-for-a-network-interface"></a>Visa effektiva vägar för ett nätverksgränssnitt
Om flödet i nätverkstrafiken påverkas för ett visst nätverksgränssnitt (NIC), kan du visa en fullständig lista över giltiga vägar på ett nätverkskort direkt. toosee hello sammanställd vägar som är tillämpade tooa NIC, fullständig hello följande steg:

1. Inloggningen toohello Azure-portalen på https://portal.azure.com.
2. Klicka på **fler tjänster**, klicka på **nätverksgränssnitt**
3. Söklistan hello hello namnet på ett nätverkskort eller välj hello listan som visas. I det här exemplet **VM1 NIC1** är markerad.
4. Välj **effektiva vägar** i hello **nätverksgränssnittet** bladet som visas i följande bild hello:

       ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Hej **omfång** standardvärden toohello nätverksgränssnittet som valts.

      ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)

### <a name="view-effective-routes-for-a-route-table"></a>Visa effektiva flöden för en routningstabell
Du kanske vill tooreview hello effekten av hello vägar som läggs till på en viss virtuell dator när du ändrar användardefinierade vägar (udr: er) i en routningstabell. En routingtabell kan associeras med flera undernät. Nu kan du visa alla hello effektiva vägar för alla hello nätverkskort som en given routingtabell tillämpas på, utan att behöva tooswitch kontext från hello angivna vägen tabell bladet.

Till exempel en UDR (*UDRoute*) har angetts i en routningstabell (*UDRouteTable*). Den här vägen skickar alla Internet-trafiken från *Undernät1* i hello *WestUS VNet1* VNet via en virtuell nätverksenhet (NVA) i *Undernät2* av hello samma virtuella nätverk. hello flödet visas i följande bild hello:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

toosee hello sammanställd vägar för en routningstabell, fullständig hello följande steg:

1. Inloggningen toohello Azure-portalen på https://portal.azure.com.
2. Klicka på **fler tjänster**, klicka på **routningstabeller**
3. Hello söklistan för hello routningstabellen toosee sammanställd vägar för och markera den. I det här exemplet **UDRouteTable** är markerad. Ett blad för vägtabellen hello valt visas som visas i följande bild hello:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)
4. Välj **effektiva vägar** i hello **routningstabellen** bladet. Hej **omfång** anges toohello routningstabellen som du har valt.
5. En routingtabell kan vara tillämpade toomultiple undernät. Välj hello **undernät** du vill tooreview hello-listan. I det här exemplet **Undernät1** är markerad.
6. Välj en **nätverksgränssnittet**. Alla nätverkskort anslutna toohello valda undernät visas. I det här exemplet **VM1 NIC1** är markerad.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

   > [!NOTE]
   > Om hello NIC inte är associerad med en aktiv virtuell dator, visas inga effektiva vägar.
   >
   >

## <a name="considerations"></a>Överväganden
Några saker tookeep i åtanke när du granskar hello lista över vägar som returnerades:

* Routning baserat på längsta Prefix-matchning (LPM) bland udr: er, systemet och BGP-vägar. Om det finns fler än en väg med hello samma LPM matchar, och sedan väljs en väg baserat på dess ursprung i hello följande ordning:

  * Användardefinierad väg
  * BGP-väg
  * Systemväg (standard)

    Du kan bara se effektiva vägar som är baserat på alla hello tillgängliga vägar LPM-matchning med effektiva vägar. Genom att visa hur hello vägar faktiskt utvärderas till ett visst nätverkskort, blir det mycket enklare tootroubleshoot vägar driftstörningar anslutning till eller från den virtuella datorn.
* Om du har udr: er och skickar trafik tooa virtuell nätverksenhet (NVA) med *VirtualAppliance* som **nextHopType**, kontrollera att IP-vidarebefordring är aktiverat på hello NVA mottagande hello trafik eller paket tappas.
* Om framtvingad tunneltrafik aktiveras, blir utgående Internet-trafik dirigeras tooon lokala. RDP/SSH från Internet tooyour VM inte fungerar med den här inställningen, beroende på hur hello lokalt hanterar den här trafiken.
  Tvingad tunneltrafik kan aktiveras:
  * Om du använder plats-till-plats-VPN genom att ange en användardefinierad väg (UDR) med nextHopType som VPN-Gateway
  * Om en standardväg annonseras över BGP
* För VNet-peering trafik toowork korrekt en systemväg med **nextHopType** *VNetPeering* måste finnas för hello peerkoppla virtuella-prefix för intervallet. Om en sådan väg finns inte och hello VNet-peering ut länken OK:
  * Vänta några sekunder och försök igen om det är en nyetablerade peering länk. Ibland tar det längre toopropagate vägar tooall hello-nätverksgränssnitt i ett undernät.
  * Nätverkssäkerhetsgrupp (NSG) regler kan påverka hello trafikflöden. Mer information finns i hello [felsöka Nätverkssäkerhetsgrupper](virtual-network-nsg-troubleshoot-portal.md) artikel.
