---
title: "aaaCheck anslutningen till Azure Nätverksbevakaren - Azure-portalen | Microsoft Docs"
description: "Den här sidan förklarar hur toouse anslutningen kontrolleras med Nätverksbevakaren med hello Azure-portalen"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.openlocfilehash: ef6ecccd688f06f70003a5b59771c15bcbe8f3e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a>Kontrollera anslutningen med Azure Nätverksbevakaren med hello Azure-portalen

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Azure REST-API](network-watcher-connectivity-rest.md)

Lär dig hur toouse anslutning tooverify om en direkt TCP-anslutning från en virtuell dator tooa angivna slutpunkten kan upprättas.

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln förutsätter att du har hello följande resurser:

* En instans Nätverksbevakaren i hello region som du vill toocheck anslutning.

* Virtuella datorer toocheck anslutningsmöjligheter med.

> [!IMPORTANT]
> Kontrollera anslutningen kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`. Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="check-connectivity-tooa-virtual-machine"></a>Kontrollera anslutningen tooa virtuell dator

Det här exemplet kontrollerar anslutningen tooa virtuella måldatorn via port 80.

Navigera tooyour Nätverksbevakaren och klicka på **anslutningen Kontrollera (förhandsgranskning)**. Välj hello virtuella toocheck anslutningen från. I hello **mål** väljer **väljer en virtuell dator** och välj hello rätt virtuell dator och port tootest.

När du klickar på **Kontrollera**, hello anslutningar mellan hello virtuella datorer på angivna hello-porten är markerade. I exemplet hello hello mål VM kan inte nås, visas en lista över hopp.

![Resultat av anslutningen för en virtuell dator][1]

## <a name="check-remote-endpoint-connectivity"></a>Kontrollera att fjärrslutpunkten anslutning

toocheck hello anslutning och svarstid tooa fjärrslutpunkten, Välj hello **ange manuellt** alternativknapp i hello **mål** avsnittet, ange hello url och hello porten och klicka på **Kontrollera** .  Det här används för fjärråtkomst-slutpunkter som slutpunkter för webbplatser och lagring.

![Resultat av anslutningen för en webbplats][2]

## <a name="next-steps"></a>Nästa steg

Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)

Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
