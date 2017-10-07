---
title: aaaTroubleshooting anslutningsproblem mellan virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur tooTroubleshoot hello problem med nätverksanslutningen mellan virtuella Azure-datorer."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: ee0819178dcbee2bf529a495758e6c33f49e085e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Felsökning av problem med nätverksanslutningen mellan virtuella Azure-datorer

Kan det uppstå problem med nätverksanslutningen mellan virtuella Azure-datorer. Den här artikeln innehåller felsökning steg toohelp du lösa problemet. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symtom

En virtuell dator i Azure kan inte ansluta tooanother Azure VM.

## <a name="troubleshooting-guidance"></a>Felsökningsanvisningar 

1. [Kontrollera om nätverkskortet är felkonfigurerat](#step-1-check-if-nic-is-misconfigured)
2. [Kontrollera om nätverkstrafik blockeras av NSG eller UDR](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [Kontrollera om nätverkstrafik blockeras av VM-brandvägg](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [Kontrollera om VM app eller tjänst lyssnar på port hello](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Kontrollera om hello problemet orsakas av SNAT](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Kontrollera om trafik blockeras av ACL: er för hello klassiska VM](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Kontrollera om hello slutpunkten har skapats för hello klassiska VM](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Det går inte tooconnect tooa VM-nätverksresurs](#step-8-unable-to-connect-to-a-vm-network-share)
9. [Bland Vnet-anslutningar](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Felsökningssteg

Följ dessa steg tootroubleshoot hello problem. Kontrollera om hello problemet är löst efter varje steg. 

### <a name="step-1-check-if-nic-is-misconfigured"></a>Steg 1: Kontrollera om nätverkskortet är felkonfigurerat

Följ [hur tooreset nätverksgränssnittet för Azure Windows VM](../virtual-machines/windows/reset-network-interface.md). 

Om hello problem uppstår när du har ändrat hello nätverksgränssnitt (NIC), gör du följande:

**Mulit NIC virtuella datorer**

1. Lägg till ett nätverkskort.
2. Lösa problem hello i hello felaktiga NIC eller ta bort felaktiga nätverkskort.  Readd hello NIC.

Mer information finns i [Lägg till nätverket gränssnitt tooor ta bort från virtuella datorer](virtual-network-network-interface-vm.md).

**Enskild NIC VM** 

- [Distribuera Windows VM](../virtual-machines/windows/redeploy-to-new-node.md)
- [Distribuera virtuell Linux-dator](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a>Steg 2: Kontrollera om nätverkstrafik blockeras av NSG eller UDR

Använda [nätverk Watcher IP-flöde Kontrollera](../network-watcher/network-watcher-ip-flow-verify-overview.md) och [NSG flödet loggning](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify om det är en Nätverkssäkerhetsgrupp eller User-defined väg kan störa trafikflödet.

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a>Steg 3: Kontrollera om nätverkstrafik blockeras av VM-brandvägg

Inaktivera hello-brandväggen och sedan testa hello resultatet. Om hello problemet är löst verifiera hello inställningarna i hello brandvägg och aktivera igen.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-hello-port"></a>Steg 4: Kontrollera om VM app eller tjänst lyssnar på port hello

Du kan använda någon av följande metoder toocheck om VM-programmet eller tjänsten lyssnar på port hello hello

- Kör följande kommandon toocheck om hello servern lyssnar på porten hello.

**Virtuell Windows-dator**

    netstat –ano

**Virtuell Linux-dator**

    netstat -l

- Kör hello **Telnet** på hello VM tooitself tootest hello port. Om hello testet misslyckas, är programmet eller tjänsten inte konfigurerade toolisten på hello port.

### <a name="step-5-check-whether-hello-problem-is-caused-by-snat"></a>Steg 5: Kontrollera om hello problemet orsakas av SNAT

I vissa scenarier, är hello VM placerad bakom en lösning för utjämna belastningen som har ett beroende resurser utanför Azure. I dessa scenarier, om det uppstår återkommande anslutningsproblem, hello problemet kan orsakas av [SNAT port resursuttömning](../load-balancer/load-balancer-outbound-connections.md). tooresolve hello problemet, skapa en VIP (eller går för klassisk) för varje virtuell dator som är bakom hello belastningsutjämnare och skydda med NSG eller ACL. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-hello-classic-vm"></a>Steg 6: Kontrollera om trafik blockeras av ACL: er för hello klassiska VM

En ACL ger hello möjlighet tooselectively bevilja eller neka trafik för en virtuell dator-slutpunkten. Mer information finns i [hantera hello ACL på en slutpunkt](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-hello-endpoint-is-created-for-hello-classic-vm"></a>Steg 7: Kontrollera om hello slutpunkten har skapats för hello klassiska VM

Alla virtuella datorer som du skapar i Azure med hjälp av hello klassiska distributionsmodellen kan automatiskt kommunicera via en kanal för privat nätverk med andra virtuella datorer i hello samma cloud service eller virtuellt nätverk. Datorer med andra virtuella nätverk måste dock slutpunkter toodirect hello nätverk för inkommande trafik tooa virtuell dator. Mer information finns i [hur tooset av slutpunkter](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-unable-tooconnect-tooa-vm-network-share"></a>Steg 8: Det gick inte tooconnect tooa VM nätverksresurs

Om du är tooconnect tooa VM nätverksresurs orsakas hello problemet av tillgänglig nätverkskort i hello VM. toodelete Hej tillgänglig nätverkskort, se [hur toodelete hello tillgänglig nätverkskort](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-inter-vnet-connectivity"></a>Steg 9: Bland Vnet-anslutningen

Använda [nätverk Watcher IP-flöde Kontrollera](../network-watcher/network-watcher-ip-flow-verify-overview.md) och [NSG flödet loggning](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify om det är en Nätverkssäkerhetsgrupp eller User-defined väg kan störa trafikflödet. Du kan också verifiera konfigurationen av Inter-Vnet [här](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.
