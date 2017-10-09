---
title: "aaaTroubleshoot Azure virtuell nätverksgateway och anslutningar - Azure CLI 1.0 | Microsoft Docs"
description: "Den här sidan förklarar hur toouse hello Azure Nätverksbevakaren felsöka Azure CLI 1.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: a0511689d773f9c7216b0fe5470e27f754eb600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a>Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher Azure CLI 1.0

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST-API](network-watcher-troubleshoot-manage-rest.md)

Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure. En av dessa funktioner är resurs felsökning. Felsökning av resursen kan anropas via hello portal, PowerShell, CLI eller REST API. När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.

Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux. 

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.

En lista över stöds gateway typer besök [stöd för Gateway-typer](/network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Översikt

Felsökning av resursen ger hello möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar. När en förfrågan görs tooresource felsökning, loggar som efterfrågas och kontrolleras. Hello resultat returneras när kontroll har slutförts. Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter tooreturn ett resultat. hello loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Hämta en Gateway för virtuell nätverksanslutning

I det här exemplet är resurs felsökning som kördes på en anslutning. Du kan också flytta den virtuella Nätverksgatewayen. hello visar följande cmdlet hello vpn-anslutningar i en resursgrupp.

```azurecli
azure network vpn-connection list -g resourceGroupName
```

Du kan också köra hello kommandot toosee hello anslutningar i en prenumeration.

```azurecli
azure network vpn-connection list -s subscription
```

När du har hello namnet på hello-anslutningen kan köra du det här kommandot tooget dess resursens Id:

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Felsökning av resursen returnerar data om hello hälsotillstånd hello resurs, sparas även loggar tooa storage-konto toobe ses över. I det här steget ska vi skapa ett lagringskonto, om det finns ett befintligt lagringskonto kan du använda den.

1. Skapa hello storage-konto

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. Hämta hello lagringskontonycklar

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. Skapa hello behållare

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Kör Nätverksbevakaren resurs felsökning

Felsökning av resurser med hello `network watcher troubleshoot` cmdlet. Vi skickar hello cmdlet hello resursgrupp, hello namnet på hello Nätverksbevakaren hello-Id för hello anslutning hello-Id för hello storage-konto och hello sökvägen toohello blob toostore hello felsöka resulterar i.

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

När du kör cmdlet hello granskar Nätverksbevakaren hello resurshälsa tooverify hello. Den returnerar hello resultat toohello shell och lagrar loggar hello resultat i hello storage-konto anges.

## <a name="understanding-hello-results"></a>Förstå hello resultat

hello åtgärd texten innehåller allmänna råd om hur tooresolve hello problemet. Om en åtgärd kan vidtas för hello problemet, tillhandahålls en länk med mer information. I hello fall där det finns inga ytterligare riktlinjer, hello svaret innehåller hello url tooopen ett supportärende.  Mer information om hello egenskaper för hello svar och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)

Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Ett annat verktyg som kan användas är Lagringsutforskaren. Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)

## <a name="next-steps"></a>Nästa steg

Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack ned hello grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.
