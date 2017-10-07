---
title: "aaaManage paket som samlar in med Azure Nätverksbevakaren - Azure-portalen | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage hello paket avbilda funktion i Nätverksbevakaren med hjälp av Azure portal"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 431b145ee215b8d9421fb2aacdce08a0de90b35e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-hello-portal"></a>Hantera paket insamlingar med Azure Nätverksbevakaren med hello-portalen

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [CLI 1.0](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-packet-capture-manage-cli.md)
> - [Azure REST-API](network-watcher-packet-capture-manage-rest.md)

Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator. Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda. Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt. Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer. Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.

Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.

- [**Starta en paketinsamling**](#start-a-packet-capture)
- [**Stoppa en paketinsamling**](#stop-a-packet-capture)
- [**Ta bort en paketinsamling**](#delete-a-packet-capture)
- [**Hämta en paketinsamling**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Innan du börjar

Den här artikeln förutsätter att du har hello följande resurser:

- En instans Nätverksbevakaren i hello region som du vill toocreate en paketinsamling
- En virtuell dator med hello paket avbilda tillägget aktiverat.

> [!IMPORTANT]
> Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`. Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-hello-portal"></a>Tillägget av paketet avbilda agent hello-portalen

tooinstall hello paketinsamling VM-agenten via hello portal, navigerar tooyour virtuella datorn, klickar på **tillägg** > **Lägg till** och Sök efter **Network Watcher Agent för Windows**

![Visa Agent][agent]

## <a name="packet-capture-overview"></a>Översikt över avbildning av paket

Navigera toohello [Azure-portalen](https://portal.azure.com) och på **nätverk** > **Nätverksbevakaren** > **paket avbilda**

hello översiktssidan visar en lista över alla paket fångar som finns oavsett hello tillstånd.

> [!NOTE]
> Paketinsamling kräver anslutning toohello storage-konto via port 443.

![paketet avbilda översiktsskärm][1]

## <a name="start-a-packet-capture"></a>Starta en paketinsamling

Klicka på **Lägg till** toocreate en paketinsamling.

hello-egenskaper som kan definieras på en paketinsamling är:

**Huvudinställningarna**

- **Prenumerationen** -värdet är hello-prenumeration som används, krävs en instans av nätverksbevakaren i varje prenumeration.
- **Resursgruppen** -hello resursgruppen av hello virtuell dator som bearbetas.
- **Rikta virtuella** -hello virtuell dator som kör hello paketinsamling
- **Avbilda paketnamn** -värdet är hello paketinsamling hello namn.

**Samla in konfiguration**

- **Lagringskontot** -anger om paketinsamling sparas i ett lagringskonto.
- **Filen** -avgör om en paketinsamling sparas lokalt på hello virtuella datorn.
- **Storage-konton** -hello valt paketinsamling för storage-konto toosave hello i. Standardplatsen är name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription https://{storage konto-id} /resourcegroups/ {resursgruppens name}/providers/microsoft.compute/virtualmachines/{virtual datornamn} / {åå} / {MM} / {DD} / packetcapture_ {HH}_{MM}_{SS} _ {XXX} CAP. (Endast aktiverad om **lagring** har valts)
- **Lokal filsökväg** -hello lokal sökväg på en virtuell dator toosave hello paketinsamling. (Endast aktiverad om **filen** har valts). En giltig sökväg måste anges
- **Maximalt antal byte per paket** -hello antalet byte från varje paket som har hämtats avbildas alla byte om värdet är tomt.
- **Maximalt antal byte per session** – Totalt antal byte som har hämtats när hello-värdet har nåtts hello paket avbilda stoppas.
- **Tidsgräns (sekunder)** -anger en tidsgräns för hello paket avbilda toostop. Standardvärdet är 18000 sekunder.

> [!NOTE]
> Premium-lagringskonton stöds inte för närvarande för lagring av paket avbildas.

**Filtrering (valfritt)**

- **Protokollet** -hello protokollet toofilter för hello paketinsamling. hello tillgängliga värden är TCP, UDP och alla.
- **Lokal IP-adress** -värdet filtrerar hello paket avbilda toopackets där hello lokala IP-adressen matchar den här filtervärdet.
- **Lokal port** -värdet filtrerar hello paket avbilda toopackets där hello lokal port matchar filtret värdet.
- **IP-Fjärradress** -värdet filtrerar hello paket avbilda toopackets där hello fjärr-IP-matchar filtret värdet.
- **Fjärrport** -värdet filtrerar hello paket avbilda toopackets där hello Fjärrport matchar filtret värdet.

> [!NOTE]
> Värden för porten och IP-adress kan vara ett värde, värdeintervallet eller en uppsättning. (det vill säga 80 1024 för port) Du kan definiera så många filter som du vill.

När hello värdena fylls, klickar du på **OK** toocreate hello paketinsamling.

![Skapa en paketinsamling][2]

När tidsgränsen för hello har angett för hello paketinsamling har upphört att gälla, hello paketinsamling stoppas och kan granskas. Du kan också manuellt stoppa hello paket insamlingar sessionerna.

## <a name="delete-a-packet-capture"></a>Ta bort en paketinsamling

Samla in vyn i hello-paket, klickar du på hello **snabbmenyn** (...) eller högerklicka och klicka på **ta bort** toostop hello paketinsamling

![Ta bort en paketinsamling][3]

> [!NOTE]
> Om du tar bort en paketinsamling tar inte bort hello-filen i hello storage-konto.

Du uppmanas tooconfirm som du vill toodelete hello paketinsamling, klicka på **Ja**

![Bekräftelse][4]

## <a name="stop-a-packet-capture"></a>Stoppa en paketinsamling

Samla in vyn i hello-paket, klickar du på hello **snabbmenyn** (...) eller högerklicka och klicka på **stoppa** toostop hello paketinsamling

## <a name="download-a-packet-capture"></a>Hämta en paketinsamling

När paketet avbildningssessionen är klar hello avbilda filen är överförda tooblob lagring eller tooa lokala på hello VM. hello lagringsplats för hello paketinsamling har definierats vid skapande av hello. En lämplig verktyget tooaccess dessa avbilda filer sparade tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/

Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)

Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













