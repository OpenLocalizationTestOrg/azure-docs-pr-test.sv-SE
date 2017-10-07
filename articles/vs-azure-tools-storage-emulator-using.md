---
title: "aaaConfiguring och använda hello Storage-emulatorn med Visual Studio | Microsoft Docs"
description: "Konfigurera och använda hello Storage-emulatorn med Visual Studio"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: c8e7996f-6027-4762-806e-614b93131867
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/17/2017
ms.author: kraigb
ms.openlocfilehash: d590f21146c86bcb7bfa6b6164b92c6df5938d5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-and-using-hello-storage-emulator-with-visual-studio"></a>Konfigurera och använda hello Storage-emulatorn med Visual Studio
[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Översikt
hello Azure SDK-utvecklingsmiljö innehåller hello storage-emulatorn, ett verktyg som simulerar hello Blob, köer och tabellen lagringstjänster finns i Azure på utvecklingsdatorn lokala. Om du skapar en molnbaserad tjänst som använder hello Azure storage-tjänster eller skriva alla externa program att anrop hello lagringstjänster, kan du testa din kod lokalt mot hello storage-emulatorn. hello Azure Tools för Microsoft Visual Studio kan du integrera hanteringen av hello storage-emulatorn i Visual Studio. hello Azure verktyg initiera hello storage-emulatorn databasen vid första användning, startar hello storage-emulatorn-tjänsten när du kör eller felsöka din kod från Visual Studio och tillhandahåller skrivskyddad åtkomst toohello storage-emulatorn data via hello Azure Lagringsutforskaren.

Detaljerad information om hello storage-emulatorn, inklusive systemkrav och anpassade konfigurationsanvisningar finns [Använd hello Azure Storage-emulatorn för utveckling och testning](storage/common/storage-use-emulator.md).

> [!NOTE]
> Det finns vissa skillnader i funktionalitet mellan hello storage-emulatorn simulering och hello Azure storage-tjänster. Se [skillnader mellan hello Storage-emulatorn och Azure Storage-tjänster](storage/common/storage-use-emulator.md) i hello Azure SDK-dokumentationen för mer information om hello specifika skillnader.
> 
> 

## <a name="configuring-a-connection-string-for-hello-storage-emulator"></a>Konfigurera en anslutningssträng för hello storage-emulatorn
tooaccess hello storage-emulatorn från kod inom en roll vill tooconfigure en anslutning sträng som punkter toohello storage-emulatorn och som kan vara ändrade toopoint tooan Azure storage-konto senare. En anslutningssträng är en konfigurationsinställning som din roll kan läsa vid körning tooconnect tooa storage-konto. Mer information om hur toocreate anslutningssträngar, se [konfigurera hello Azure-programmet](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

> [!NOTE]
> Du kan returnera ett referens toohello emulatorn lagringskonto från din kod med hjälp av hello **DevelopmentStorageAccount** egenskapen. Den här metoden fungerar om du vill tooaccess hello storage-emulatorn från din kod, men om du planerar toopublish program-tooAzure du behöver toocreate en anslutning sträng tooaccess Azure storage-konto och ändra din kod toouse som anslutningssträngen innan du publicerar den. Om du byter mellan hello storage-emulatorn-konto och ett Azure storage-konto ofta, förenklar en anslutningssträng processen.
> 
> 

## <a name="initializing-and-running-hello-storage-emulator"></a>Initiera och kör hello storage-emulatorn
Du kan ange att när du kör eller felsöka din tjänst i Visual Studio, Visual Studio startas automatiskt hello storage-emulatorn. I Solution Explorer öppnar du hello snabbmenyn för din **Azure** projektet och välj **egenskaper**. På hello **Development** på fliken hello **starta Azure Storage-emulatorn** Välj **SANT** (om den inte har angetts toothat värdet).

hello startar första gången du kör eller felsöka din tjänst från Visual Studio, hello storage-emulatorn en initieringsprocessen. Den här processen reserverar lokala portar för hello storage-emulatorn och skapar hello storage-emulatorn databas. När du är klar, behöver den här processen inte toorun igen såvida inte hello storage-emulatorn databasen tas bort.

> [!NOTE]
> Från och med juni 2012 hello av hello Azure-verktyg, körs hello storage-emulatorn som standard i SQL Express LocalDB. I tidigare versioner av hello Azure verktyg hello storage-emulatorn körs mot en standardinstans av SQL Express 2005 eller 2008, som du måste installera innan du kan installera hello Azure SDK. Du kan också köra hello storage-emulatorn mot en namngiven instans av SQL Express eller en namngiven eller standardinstansen av Microsoft SQL Server. Om du behöver tooconfigure hello storage-emulatorn toorun mot en instans än hello standardinstansen finns [Använd hello Azure Storage-emulatorn för utveckling och testning](storage/common/storage-use-emulator.md).
> 
> 

hello storage-emulatorn ger användaren gränssnittet tooview hello statusen hello lokala lagringstjänster och toostart, stoppa och återställa dem. När hello storage-emulatorn tjänsten har startats, kan du visa hello användargränssnittet eller starta eller stoppa hello-tjänst genom att högerklicka på hello ikon i meddelandefältet för hello Microsoft Azure-emulatorn i hello Aktivitetsfältet.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Visa storage-emulatorn data i Server Explorer
hello Azure Storage-nod i Server Explorer kan du tooview data och ändra inställningarna för blob och tabelldata i storage-konton, inklusive hello storage-emulatorn. Se [hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) för mer information.

