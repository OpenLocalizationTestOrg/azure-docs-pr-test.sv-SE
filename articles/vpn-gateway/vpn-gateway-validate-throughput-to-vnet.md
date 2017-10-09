---
title: "Verifiera VPN-genomströmning tooa Microsoft Azure Virtual Network | Microsoft Docs"
description: "hello syftet med det här dokumentet är toohelp en användare verifiera hello dataflödet i nätverket från sina lokala resurser tooan virtuella Azure-datorn."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 9cf0b67145645a3c2a9556b0cc910066cc62a9ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toovalidate-vpn-throughput-tooa-virtual-network"></a>Hur toovalidate VPN-genomströmning tooa virtuellt nätverk

En VPN-gateway-anslutningen kan du tooestablish säker anslutning mellan ditt virtuella nätverk i Azure och ditt lokala IT-infrastruktur.

Den här artikeln visar hur toovalidate dataflödet i nätverket från hello lokala resurser tooan virtuella Azure-datorn. Det ger också felsökningsinformation.

>[!NOTE]
>Den här artikeln är avsedd toohelp diagnostisera och lösa vanliga problem. Om du toosolve hello problemet med hjälp av följande information hello [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Översikt

hello VPN-gatewayanslutning omfattar hello följande komponenter:

- Lokala VPN-enhet (visa en lista över [verifiera VPN-enheter)](vpn-gateway-about-vpn-devices.md#devicetable).
- Offentliga Internet
- Azure VPN-gateway
- Virtuell Azure-dator

hello följande diagram visar hello logiska anslutningen för ett lokalt nätverk tooan virtuella Azure-nätverket via VPN.

![Logisk anslutning i kundens nätverk tooMSFT nätverket med VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-hello-maximum-expected-ingressegress"></a>Beräkna hello högsta förväntade ingång-/ utgång

1.  Fastställa kraven för ditt program baslinjen genomflöde.
2.  Bestämma din Azure VPN gateway genomflöde. Mer hjälp finns hello ”sammanställda genomflöde av SKU- och VPN-typ” i [planering och design för VPN-Gateway](vpn-gateway-plan-design.md).
3.  Fastställa hello [Azure VM genomströmning vägledning](../virtual-machines/virtual-machines-windows-sizes.md) för VM-storlek.
4.  Kontrollera din Internet-leverantör (ISP) bandbredd.
5.  Beräkna din förväntade genomströmning - minsta bandbredd (VM-Gateway ISP) * 0,8.

Om din beräknade genomströmning inte uppfyller kraven för ditt program baslinjen dataflöde, måste tooincrease hello bandbredden för hello-resurs som du har identifierat som hello flaskhals. tooresize Azure VPN-Gateway, se [ändra en gateway-SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku). tooresize en virtuell dator finns [ändra storlek på en virtuell dator](../virtual-machines/virtual-machines-windows-resize-vm.md). Om du inte har den förväntade internetbandbredd, kan du också toocontact din Internetleverantör.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Verifiera dataflödet i nätverket med hjälp av prestandaverktygen

Den här verifieringen ska utföras vid låg belastning, som VPN-tunnel genomströmning mättnad under testningen inte ger korrekta resultat.

hello-verktyget som vi använder för det här testet är iPerf som fungerar i både Windows och Linux och har både klienten och servern lägen. Det är begränsad too3 Gbit/s för virtuella Windows-datorer.

Det här verktyget inte att utföra några åtgärder toodisk för läsning och skrivning. Den genererar endast självgenererat TCP-trafik från en end toohello andra. Den genererade statistik baserat på experiment som mäter hello bandbredd som är tillgänglig mellan klienten och servern noder. När du testar mellan två noder som en fungerar som hello server och hello andra som en klient. När det här testet har slutförts, rekommenderar vi att du återställer hello roller tootest både ladda upp och hämta genomströmning på båda noderna.

### <a name="download-iperf"></a>Hämta iPerf
Hämta [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). Mer information finns i [iPerf dokumentationen](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >hello tredjepartsprodukter som den här artikeln beskrivs tillverkas oberoende av Microsoft. Microsoft lämnar inga garantier, underförstådda eller, om hello prestanda och pålitlighet.
 >
 >

### <a name="run-iperf-iperf3exe"></a>Kör iPerf (iperf3.exe)
1. Aktivera en NSG/regeln tillåter hello trafik (för offentliga IP-adressen testa på Azure VM).

2. Aktivera ett brandväggsundantag för port 5001 på båda noderna.

    **Windows:** kör hello följande kommando som administratör:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    tooremove hello regeln när testningen är klar, kör det här kommandot:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    **Azure Linux:** Azure Linux bilder har Tillåtande brandväggar. Om det finns ett program som lyssnar på en port, tillåts hello trafiken. Anpassade avbildningar som är skyddade behöva portar som öppnats explicit. Vanliga Linux OS-lager brandväggar inkluderar `iptables`, `ufw`, eller `firewalld`.

3. Ändra toohello directory där iperf3.exe extraherade på hello server-noden. Sedan kör iPerf i serverläge och ange den toolisten på port 5001 som hello följande kommandon:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. Ändra toohello katalogen där iperf verktyget extraheras och kör sedan följande kommando hello på hello klientnod:

    ```CMD
    iperf3.exe -c <IP of hello iperf Server> -t 30 -p 5001 -P 32
    ```

    hello-klienten att trafik på port 5001 toohello server i 30 sekunder. hello flaggan '-P-värde som anger att vi använder 32 toohello-servernoden för samtidiga anslutningar.

    hello visar följande skärmbild hello utdata från det här exemplet:

    ![Resultat](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. (Valfritt) toopreserve hello testresultaten, köra det här kommandot:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. När du har slutfört föregående steg i hello köra hello samma steg med hello roller återställs, så att hello servernoden kommer nu att hello klienten och vice versa.

## <a name="address-slow-file-copy-issues"></a>Åtgärda problem med långsam kopia
Det kan uppstå långsam filen kopieringen när Utforskaren eller dra och släppa via en RDP-session. Det här problemet är vanligtvis på grund av tooone eller båda av hello följande faktorer:

- Kopiera program, till exempel Utforskaren och RDP-filen kan inte använda flera trådar när du kopierar filer. För bättre prestanda, Använd ett flertrådat filen kopiera program som [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy filer med hjälp av 16 eller 32 trådar. toochange hello tråd nummer för filkopiering i Richcopy, klickar du på **åtgärd** > **kopiera alternativ** > **filkopieringen**.<br><br>
![Problem med långsam filens kopia](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Otillräcklig VM disk läsning och skrivning hastighet. Mer information finns i [felsökning av Azure Storage](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>Lokal enhet externa Internetriktade gränssnittet
Om hello lokala VPN-enhet mot Internet IP-adress som ingår i hello [lokala nätverket](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition i Azure kan uppstå när det gäller toobring hello kopplar från VPN, sporadiska eller prestandaproblem.

## <a name="checking-latency"></a>Kontroll av svarstid
Använd tracert tootrace tooMicrosoft Azure Edge enheten toodetermine om fördröjningar mer än 100 millisekunder, mellan hopp.

Kör från hello lokalt nätverk *tracert* toohello VIP hello Azure Gateway eller VM. När du ser endast * returneras genom att du vet hello Azure gränsen har uppnåtts. När DNS-namn som innehåller ”MSN” returneras visas vet du hello Microsoft stamnät har uppnåtts.<br><br>
![Kontroll av svarstid](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Nästa steg
Mer information eller hjälp, ta en titt hello följande länkar:

- [Optimera dataflödet i nätverket för virtuella Azure-datorer](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
