---
title: "aaaTroubleshoot Azure virtuell nätverksgateway och anslutningar - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur toouse hello Azure Nätverksbevakaren felsöka PowerShell-cmdlet"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: bd568d34091209390c57d22475559bdb99ad2c59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher PowerShell

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST-API](network-watcher-troubleshoot-manage-rest.md)

Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure. En av dessa funktioner är resurs felsökning. Felsökning av resursen kan anropas via hello portal, PowerShell, CLI eller REST API. När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Översikt

Felsökning av resursen ger hello möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar. När en förfrågan görs tooresource felsökning, loggar som efterfrågas och kontrolleras. Hello resultat returneras när kontroll har slutförts. Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter tooreturn ett resultat. hello loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.

## <a name="troubleshoot-a-gateway-or-connection"></a>Felsöka en gateway eller -anslutning

1. Navigera toohello [Azure-portalen](https://portal.azure.com) och på **nätverk** > **Nätverksbevakaren** > **VPN-diagnostik**
2. Välj en **prenumeration**, **resursgruppen**, och **plats**.
3. Felsökning av resursen returnerar data om hello hälsotillstånd hello resurs. Här sparas även relevanta loggar tooa lagringskonto toobe ses över. Klicka på **Lagringskonto** tooselect ett lagringskonto.
4. På hello **lagringskonton** bladet Välj ett befintligt lagringskonto eller klicka på **+ lagringskonto** toocreate en ny.
5. På hello **behållare** bladet Välj en befintlig behållare eller klicka på **+ behållare** toocreate en ny behållare. När du är klar klickar du på **Välj**
6. Välj tootroubleshoot för hello Gateway och anslutning till resurser och klicka på **Starta felsökning**

Om du har valt flera resurser körs felsökning samtidigt på gateway-resurser. Det kan inte köras på en anslutning och dess tillhörande gatewayen på hello samtidigt. Det rekommenderas tootroubleshoot gateways först anslutningen felsökning är en längre process. När VPN-diagnostik körs på en resurs hello **felsökning STATUS** kolumnen visar statusen **körs**

När du är färdig hello kolumnen statusändringar för**felfri** eller **ohälsosam**.

![Felsöka klar][2]

## <a name="understanding-hello-results"></a>Förstå hello resultat

I hello **information** avsnitt av hello fönstret hello **Status** fliken visar hello status hello senaste felsökning kör på hello valda resursen. Resultat av hello senaste diagnostiken är visas xx minuter efter hello senast kördes.

|Egenskap  |Beskrivning  |
|---------|---------|
|Resurs     | En länk toohello resurs.        |
|Sökvägen till lagring     |  Sökvägen toohello storage-konto och en behållare som innehåller hello-loggar (om någon har producerats under hello kör). Den här inställningen sparas inte när du lämnar hello-portalen.        |
|Sammanfattning     | Sammanfattning av hello resurshälsa.        |
|Information om     | Detaljerad information om hello resurshälsa.        |
|Senaste körning     | hello tid hello senaste gången felsökning har körts.        |


Hej **åtgärd** fliken innehåller allmänna råd om hur tooresolve hello problemet. Om en åtgärd kan vidtas för hello problemet, tillhandahålls en länk med mer information. I hello fall där det finns inga ytterligare riktlinjer, hello svaret innehåller hello url tooopen ett supportärende.  Mer information om hello egenskaper för hello svar och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)


## <a name="next-steps"></a>Nästa steg

Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack ned hello grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
