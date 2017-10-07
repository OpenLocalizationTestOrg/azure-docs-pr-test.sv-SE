---
title: "aaaConfigure en anslutningssträng för Azure Storage | Microsoft Docs"
description: "Konfigurera en anslutningssträng för ett Azure storage-konto. En anslutningssträng innehåller hello information behövs tooauthenticate åtkomst tooa lagringskontot från ditt program vid körning."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: marsma
ms.openlocfilehash: ac1d7d9bf11fa6f44243cda0c40d8faee12e513b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-storage-connection-strings"></a>Konfigurera Azure Storage-anslutningssträngar

En anslutningssträng innehåller hello autentiseringsinformation som krävs för dina programdata tooaccess i ett Azure Storage-konto vid körning. Du kan konfigurera anslutningssträngar för att:

* Ansluta toohello Azure storage-emulatorn.
* Åtkomst till ett lagringskonto i Azure.
* Åtkomst till angivna resurser i Azure via en signatur för delad åtkomst (SAS).

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Lagra anslutningssträngen
Programmet måste tooaccess hello anslutningssträngen vid körning tooauthenticate begäranden tooAzure lagring. Du har flera alternativ för att lagra anslutningssträngen:

* Ett program som körs på skrivbordet hello eller på en enhet kan lagra hello anslutningssträngen i en **app.config** eller **web.config** fil. Lägg till hello anslutning sträng toohello **AppSettings** avsnitt i dessa filer.
* Ett program som körs i en Azure-molntjänst kan lagra hello anslutningssträngen i hello [Azure-tjänstkonfigurationsfil schema (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). Lägg till hello anslutning sträng toohello **ConfigurationSettings** i hello service konfigurationsfil.
* Du kan använda anslutningssträngen direkt i din kod. Vi rekommenderar dock att du lagrar anslutningssträngen i en konfigurationsfil i de flesta fall.

Spara anslutningssträngen i en konfigurationsfil gör det enkelt tooupdate hello anslutning sträng tooswitch mellan hello storage-emulatorn och ett Azure storage-konto i hello molnet. Du behöver bara tooedit hello anslutning sträng toopoint tooyour målmiljön.

Du kan använda hello [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) tooaccess anslutningssträngen vid körningstillfället oavsett där programmet körs.

## <a name="create-a-connection-string-for-hello-storage-emulator"></a>Skapa en anslutningssträng för hello storage-emulatorn
[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Läs mer om hello storage-emulatorn [Använd hello Azure storage-emulatorn för utveckling och testning](storage-use-emulator.md).

## <a name="create-a-connection-string-for-an-azure-storage-account"></a>Skapa en anslutningssträng för ett Azure storage-konto
toocreate en anslutningssträng för Azure storage-konto, Använd hello följande format. Ange om du vill tooconnect toohello storage-konto via HTTPS (rekommenderas) eller HTTP ersätter `myAccountName` med hello namnet på ditt lagringskonto och Ersätt `myAccountKey` med din åtkomstnyckel:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Anslutningssträngen kan till exempel se ut:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

Azure Storage har stöd för både HTTP och HTTPS i en anslutningssträng *HTTPS rekommenderas*.

> [!TIP]
> Du hittar ditt lagringskonto anslutningssträngar i hello [Azure-portalen](https://portal.azure.com). Navigera för**inställningar** > **åtkomstnycklar** i ditt lagringskonto menyn bladet toosee-anslutningssträngar för båda primära och sekundära nycklarna.
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Skapa en anslutningssträng med hjälp av en signatur för delad åtkomst
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="create-a-connection-string-for-an-explicit-storage-endpoint"></a>Skapa en anslutningssträng för en explicit lagring-slutpunkt
Du kan ange Tjänsteslutpunkter explicit i anslutningssträngen i stället för hello slutpunkter. toocreate en anslutningssträng som anger en slutpunkt som explicit ange hello fullständig tjänstslutpunkten för varje tjänst, inklusive hello protokollspecifikation (HTTPS (rekommenderas) eller HTTP) i hello följande format:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
FileEndpoint=myFileEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Ett scenario där du kanske vill toospecify en explicit slutpunkt är när du har kopplat till ditt Blob storage endpoint tooa [domänen](../blobs/storage-custom-domain-name.md). I så fall kan du ange din anpassade slutpunkt för Blob storage i anslutningssträngen. Du kan också ange hello standardslutpunkterna för hello andra tjänster om programmet använder dem.

Här är ett exempel på en anslutningssträng som anger en explicit slutpunkt för hello Blob-tjänsten:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
AccountName=storagesample;
AccountKey=<account-key>
```

Det här exemplet anger explicit slutpunkter för alla tjänster, inklusive en anpassad domän för hello Blob-tjänsten:

```
# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=http://www.mydomain.com;
FileEndpoint=https://myaccount.file.core.windows.net;
QueueEndpoint=https://myaccount.queue.core.windows.net;
TableEndpoint=https://myaccount.table.core.windows.net;
AccountName=storagesample;
AccountKey=<account-key>
```

hello endpoint värden i en anslutningssträng är används tooconstruct hello begäran URI: er toohello storage-tjänster och styr hello form av alla URI: er som returneras tooyour kod.

Om du har kopplat en anpassad domän för lagring endpoint tooa och utesluter den slutpunkten från en anslutningssträng och du inte kommer att kunna toouse strängen anslutningen tooaccess data i tjänsten från din kod.

> [!IMPORTANT]
> Tjänsten endpoint värdena i anslutningssträngarna måste vara giltig URI: er, inklusive `https://` (rekommenderas) eller `http://`. Eftersom Azure Storage inte stöder ännu HTTPS för anpassade domäner du *måste* ange `http://` för valfri slutpunkt URI som pekar tooa anpassade domäner.
>

### <a name="create-a-connection-string-with-an-endpoint-suffix"></a>Skapa en anslutningssträng med en slutpunkt-suffix
toocreate en anslutningssträngen för en lagringstjänst i regioner eller instanser med olika endpoint-suffix som för Azure Kina eller Azure Government, Använd hello följande connection-strängformat. Ange om du vill tooconnect toohello storage-konto via HTTPS (rekommenderas) eller HTTP ersätter `myAccountName` med hello namn för ditt lagringskonto, Ersätt `myAccountKey` med din åtkomstnyckel och Ersätt `mySuffix` med hello URI suffix:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Här är en exempel-anslutningssträng för storage-tjänster i Azure Kina:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Parsning av en anslutningssträng
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Nästa steg
* [Använd hello Azure storage-emulatorn för utveckling och testning](storage-use-emulator.md)
* [Azure lagringsutforskare](storage-explorers.md)
* [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md)

