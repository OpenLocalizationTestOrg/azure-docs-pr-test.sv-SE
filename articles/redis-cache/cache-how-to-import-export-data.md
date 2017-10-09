---
title: aaaImport och exportera data i Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooand för tooimport och exportera data från blob storage med dina instanser för premium Azure Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: sdanie
ms.openlocfilehash: f17464b207f1c652952f4da63ca147473fee2759
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Importera och exportera data i Azure Redis-Cache
Importera och exportera är en Azure Redis-Cache data management-åtgärd, som gör att du tooimport data till Azure Redis-Cache eller exportera data från Azure Redis-Cache genom att importera och exportera en ögonblicksbild för Redis-Cache databasen (RDB) från en premium cache tooa blob i en Azure Storage-konto. 

- **Exportera** -du kan exportera din Azure Redis-Cache RDB ögonblicksbilder tooa Sidblob.
- **Importera** -du kan importera Redis-Cache RDB-ögonblicksbilder från en Sidblob eller en Blockblob.

Import/Export kan du toomigrate mellan olika instanser av Azure Redis-Cache eller fylla hello cachen med data innan de används.

Den här artikeln innehåller en guide för import och export av data med Azure Redis-Cache samt hello svar toocommonly frågor och svar.

> [!IMPORTANT]
> Import/Export är i förhandsvisning och finns bara för [premiumnivån](cache-premium-tier-intro.md) cachelagrar.
>
>

## <a name="import"></a>Importera
Importera kan vara används toobring Redis kompatibel RDB filer från en Redis-server som kör i molnet eller miljö, inklusive Redis som körs på Linux, Windows eller valfri provider som molntjänster, till exempel Amazon Web Services och andra. Importera data är ett enkelt sätt toocreate en cache med data i förväg. Under importen hello Azure Redis-Cache läser in hello RDB filer från Azure storage i minnet och infogar hello nycklar i hello cache.

> [!NOTE]
> Se till att din Redis-databasen (RDB) eller filer överförs till sidan eller blockera BLOB-objekt i Azure storage i hello samma innan du påbörjar hello importen, region och prenumerationen som Azure Redis-Cache-instans. Mer information finns i [komma igång med Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Om du har exporterat RDB filen med hello [Azure Redis-Cache exportera](#export) funktionen RDB filen lagras redan i en sidblob och är redo för att importera.
>
>

1. tooimport en eller flera exporteras cache blobbar [Bläddra tooyour cache](cache-configure.md#configure-redis-cache-settings) i hello Azure-portalen och klicka på **dataimport** från hello **resurs menyn**.

    ![Importera data][cache-import-data]
2. Klicka på **Välj Blob(s)** och välj hello storage-konto som innehåller hello data tooimport.

    ![Välj lagringskonto][cache-import-choose-storage-account]
3. Klicka på hello-behållare som innehåller hello data tooimport.

    ![Välj behållare][cache-import-choose-container]
4. Markera en eller flera blobbar tooimport genom att klicka på hello area toohello vänster om hello blob namn och klicka på **Välj**.

    ![Välj blobbar][cache-import-choose-blobs]
5. Klicka på **importera** toobegin hello importen.

   > [!IMPORTANT]
   > hello cachelagret är inte tillgänglig av cacheklienter under importen hello och alla befintliga data i hello cache tas bort.
   >
   >

    ![Importera][cache-import-blobs]

    Du kan övervaka hello hello importen av följande hello meddelanden från hello Azure-portalen eller genom att visa hello händelser i hello [granskningsloggen](../azure-resource-manager/resource-group-audit.md).

    ![Importera pågår][cache-import-data-import-complete]

## <a name="export"></a>Exportera
Exportera kan tooexport hello data som lagras i Azure Redis-Cache tooRedis kompatibel RDB fil(er). Du kan använda den här funktionen toomove data från en Azure Redis-Cache-instansen tooanother eller tooanother Redis-servern. Under exportprocessen hello skapas en temporär fil på hello VM att värdar hello Azure Redis-Cache server-instansen och hello fil överförda toohello avses storage-konto. När hello exporten har slutförts med antingen en status för lyckad eller misslyckad tas hello tillfälliga filen bort.

1. tooexport hello aktuella innehållet i hello cache toostorage [Bläddra tooyour cache](cache-configure.md#configure-redis-cache-settings) i hello Azure-portalen och på **exportera data** från hello **resurs menyn**.

    ![Välj lagringsbehållare][cache-export-data-choose-storage-container]
2. Klicka på **Välj lagringsbehållaren** och välj önskad hello storage-konto. hello storage-konto måste vara i hello samma prenumeration och region som ditt cacheminne.

   > [!IMPORTANT]
   > Exportera fungerar med sidblobbar som stöds av både klassiska eller Resource Manager storage-konton, men stöds inte av [Blob storage-konton](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) just nu.
   >
   >

    ![Lagringskonto][cache-export-data-choose-account]
3. Välj hello önskad blob-behållaren och klicka på **Välj**. toouse nya behållaren, klicka på **lägga till behållaren** tooadd den första och välj sedan den hello-listan.

    ![Välj lagringsbehållare][cache-export-data-container]
4. Ange en **Blob namnprefix** och på **exportera** toostart hello exporten. hello blob prefixet är används tooprefix hello namnen på de filer som genererats av den här exporten.

    ![Exportera][cache-export-data]

    Du kan övervaka hello hello export åtgärdens förlopp genom följande hello meddelanden från hello Azure-portalen eller genom att visa hello händelser i hello [granskningsloggen](../azure-resource-manager/resource-group-audit.md).

    ![Exportera data är klar][cache-export-data-export-complete]

    Cacheminnen är tillgängliga för användning under hello exporten.

## <a name="importexport-faq"></a>Importera och exportera vanliga frågor och svar
Det här avsnittet innehåller vanliga frågor och svar om hello Import/Export-funktionen.

* [Priser för vilka nivåer använder Import/Export?](#what-pricing-tiers-can-use-importexport)
* [Kan jag importera data från Redis-servern?](#can-i-import-data-from-any-redis-server)
* [Vilka RDB versioner kan importera?](#what-rdb-versions-can-i-import)
* [Är Mina cache under en Import/Export-åtgärden?](#is-my-cache-available-during-an-importexport-operation)
* [Kan jag använda Import/Export med Redis-kluster?](#can-i-use-importexport-with-redis-cluster)
* [Hur fungerar Import/Export med en anpassad databaser inställningen?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Hur skiljer sig Import/Export från Redis-persistence?](#how-is-importexport-different-from-redis-persistence)
* [Kan jag automatisera Import/Export med PowerShell, CLI eller annan av hanteringsklienter?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Jag har fått ett timeout-fel under min Import/Export-åtgärd. Vad innebär det?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Jag fick ett fel när du exporterar Mina data tooAzure Blob Storage. Vad hände?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Priser för vilka nivåer använder Import/Export?
Import/Export är endast tillgänglig i hello premium-prisnivån.

### <a name="can-i-import-data-from-any-redis-server"></a>Kan jag importera data från Redis-servern?
Ja, dessutom tooimporting-data som har exporterats från Azure Redis-Cache instanser du kan importera RDB filer från en Redis-server som kör i molnet eller miljö, till exempel Linux, Windows eller molntjänstleverantörer som Amazon Web Services. toodo detta överför hello RDB filen från hello önskad Redis-servern till en sida eller block-blob i ett Azure Storage-konto och sedan importera den till din premium Azure Redis-Cache-instans. Du kan till exempel vill tooexport hello data från ditt cacheminne för produktion och importera den till en cache som används som en del av en mellanlagringsmiljön för testning eller migrering.

> [!IMPORTANT]
> toosuccessfully importera data som har exporterats från Redis-servrar än Azure Redis-Cache när med en sidblob hello blob sidstorleken måste vara lika justerade på en 512 byte-gräns. För exempel kod tooperform alla obligatoriska byte utfyllnad, se [exempel sidan blogg överför](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Vilka RDB versioner kan importera?

Azure Redis-Cache stöder RDB importera upp via RDB version 7.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Är Mina cache under en Import/Export-åtgärden?
* **Exportera** - cacheminnen fortfarande är tillgängliga och du kan fortsätta toouse ditt cacheminne när du exporterar.
* **Importera** - cacheminnet blir inte tillgänglig när en importåtgärden startas och blir tillgängliga för användning när hello importen är klar.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Kan jag använda Import/Export med Redis-kluster?
Ja, och du kan importera och exportera mellan en klustrad cache och en icke-klustrade cache. Eftersom Redis cluster [endast stöder databasen 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), inte importera alla data i databaser än 0. När du importerar Cachedata för klustrade, omdistribueras hello nycklar bland hello shards hello-klustret.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Hur fungerar Import/Export med en anpassad databaser inställningen?
Vissa prisnivåer har olika [databaser gränser](cache-configure.md#databases), så det finns vissa saker när du importerar om du har konfigurerat ett anpassat värde för hello `databases` anger under Skapa cache.

* När du importerar tooa prisnivån med en lägre `databases` gränsen än hello nivå som du exporterade:
  * Om du använder hello standardantalet `databases`, vilket är 16 för alla prisnivåer, inga data går förlorade.
  * Om du använder ett anpassat antal `databases` att nivån faller inom hello gränser för hello toowhich som du importerar, inga data går förlorade.
  * Om din exporterade data innehöll data i en databas som överskrider hello gränserna för hello ny nivå, importeras inte hello data från de högre databaserna.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Hur skiljer sig Import/Export från Redis-persistence?
Azure Redis-Cache beständiga kan toopersist data som lagras i Redis tooAzure lagring. När beständiga konfigureras kvarstår en ögonblicksbild av hello Redis-cache i en Redis binärformat toodisk baserat på en konfigurerbar säkerhetskopieringsfrekvens Azure Redis-Cache. Om ett allvarligt fel inträffar som inaktiverar såväl hello primär replik cache återställs hello cachelagrade data automatiskt med hello senaste ögonblicksbild. Mer information finns i [hur tooconfigure datapersistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md).

Import / Export kan du toobring data till eller exporteras från Azure Redis-Cache. Det inte konfigurera säkerhetskopiering och återställning med hjälp av Redis-persistence.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kan jag automatisera Import/Export med PowerShell, CLI eller annan av hanteringsklienter?
Ja, PowerShell instruktioner finns i [tooimport Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) och [tooexport Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Jag har fått ett timeout-fel under min Import/Export-åtgärd. Vad innebär det?
Om du finns kvar i hello **dataimport** eller **exportera data** bladet längre än 15 minuter innan du påbörjar hello åtgärden ett felmeddelande med ett fel meddelande liknande toohello följande exempel:

    hello request tooimport data into cache 'contoso55' failed with status 'error' and error 'One of hello SAS URIs provided could not be used for hello following reason: hello SAS token end time (se) must be at least 1 hour from now and hello start time (st), if given, must be at least 15 minutes in hello past.

tooresolve, initiera hello importera eller exportera åtgärden innan 15 minuter har gått ut.

### <a name="i-got-an-error-when-exporting-my-data-tooazure-blob-storage-what-happened"></a>Jag fick ett fel när du exporterar Mina data tooAzure Blob Storage. Vad hände?
Export fungerar bara med RDB filer som lagras som sidblobar. Andra blob-typer stöds för närvarande inte, inklusive blob storage-konton med frekvent och lågfrekvent nivåer. Mer information finns i [Blob storage-konton](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).

## <a name="next-steps"></a>Nästa steg
Lär dig hur toouse mer premium cachelagra funktioner.

* [Introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
