---
title: "aaaConfiguring din Azure-projekt med flera tjänstkonfiguration | Microsoft Docs"
description: "Lär dig hur tooconfigure en Azure cloud service-projekt genom att ändra hello ServiceDefinition.csdef och ServiceConfiguration.cscfg filer."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 14222266093eb876db0ac9ce8d3d17a04c65d1a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurera din Azure-projekt med flera tjänstkonfiguration
Ett Azure cloud service-projekt innehåller två konfigurationsfiler: ServiceDefinition.csdef och ServiceConfiguration.cscfg. De här filerna paketeras med ditt Azure-molnet tjänstprogram och distribueras tooAzure.

* Hej **ServiceDefinition.csdef** filen innehåller hello metadata som krävs av hello Azure-miljön för hello kraven för ditt tjänstprogram för molntjänster, inklusive vilka roller som den innehåller. Den här filen innehåller också inställningar som gäller tooall instanser. Dessa konfigurationsinställningar kan läsas under körning med hello Azure-tjänsten värd för Runtime-API: et. Den här filen kan inte uppdateras, medan tjänsten körs i Azure.
* Hej **ServiceConfiguration.cscfg** filen anger värden för hello konfigurationsinställningar som definierats i tjänstdefinitionsfilen hello och anger hello antal instanser toorun för varje roll. Den här filen kan uppdateras när Molntjänsten körs i Azure.

hello Azure Tools för Microsoft Visual Studio innehåller egenskapssidor som du kan använda tooset konfigurationsinställningar som lagras i dessa filer. tooaccess hello egenskapssidor, dubbelklickar du på hello rollen referens under hello Azure cloud service-projekt i Solution Explorer eller högerklicka hello rollen referens och välj **egenskaper**som visas i följande bild hello.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Information om hello underliggande scheman för hello tjänstdefinitionen och konfigurationsfiler för tjänsten finns hello [Schemareferens](https://msdn.microsoft.com/library/azure/dd179398.aspx). Läs mer om tjänstkonfiguration [hur tooConfigure Cloud Services](cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurera rollegenskaper för en
hello egenskapssidor för en webbroll och en arbetsroll liknar varandra, även om det finns några skillnader som framgår i hello följande avsnitt.

Från hello **cachelagring** kan du konfigurera hello Azure caching services.

### <a name="configuration-page"></a>Konfigurationssidan
På hello **Configuration** kan du ange dessa egenskaper:

**Instanser**

Ange hello **instans** antal egenskapen toohello instanser hello-tjänsten ska köras för den här rollen.

Ange hello **VM-storlek** egenskapen för**Extra liten**, **små**, **medel**, **stor**, eller **Extra stor**.  Mer information finns i [storlekar för molntjänster](cloud-services/cloud-services-sizes-specs.md).

**Startåtgärden** (Web roll endast)

Ange den här egenskapen toospecify som Visual Studio ska starta en webbläsare för hello HTTP-slutpunkter och/eller hello HTTPS-slutpunkter när du startar felsökning.

hello HTTPS-slutpunkt-alternativet är bara tillgängligt om du redan har definierat en HTTPS-slutpunkt för din roll. Du kan definiera en HTTPS-slutpunkt på hello **slutpunkter** egenskapssida.

Om du redan har lagt till en HTTPS-slutpunkt hello HTTPS-slutpunkt-alternativet är aktiverat som standard och Visual Studio startas en webbläsare för den här slutpunkten när du startar felsökning dessutom tooa webbläsare för HTTP-slutpunkten. Detta förutsätter att båda startalternativ är aktiverade.

**Diagnostik**

Som standard aktiveras diagnostik för hello webbroll. hello Azure cloud service-projekt och storage-konto anges toouse hello lokala lagringsemulatorn. När du är klar toodeploy tooAzure, kan du välja hello builder knappen (**...** ) tooupdate hello storage-konto toouse Azure storage i hello molnet. Du kan överföra hello data toohello diagnostiklagringskonto på begäran eller vid automatiskt schemalagda intervall. Läs mer om Azure diagnostics [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Inställningssidan
På hello **inställningar** sida, du kan lägga till konfigurationsinställningarna för tjänsten. Konfigurationsinställningarna är namn / värde-par. Kod som körs i hello roll kan läsa hello värdena i inställningarna under körning med klasser som tillhandahålls av hello [Azure hanteras Library](http://go.microsoft.com/fwlink?LinkID=171026). Mer specifikt hello [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) metoden returnerar hello-värdet för en namngiven konfigurationsinställning vid körning.

### <a name="configuring-a-connection-string-tooa-storage-account"></a>Konfigurera en anslutning sträng tooa storage-konto
En anslutningssträng är en konfigurationsinställning som tillhandahåller information om anslutning och autentisering för hello storage-emulatorn eller ett Azure storage-konto. När din kod måste komma åt Azure storage services-data – det vill säga blob, kö eller tabelldata – från kod som körs i en roll måste har du toodefine en anslutningssträng för detta lagringskonto.

En anslutningssträng som pekar tooan Azure storage-konto måste använda ett definierat format. Information om hur toocreate anslutningssträngar, se [konfigurera Azure Storage-anslutningssträngar](storage/common/storage-configure-connection-string.md).

När du är klar tootest din tjänst mot hello Azure storage-tjänster eller när du är klar toodeploy tooAzure din moln-tjänsten kan du ändra hello-värdet för varje anslutning strängar toopoint tooyour Azure storage-konto. Välj (**...** ), Välj **ange lagringskontouppgifter**. Ange din kontoinformation som innehåller ditt kontonamn och kontonyckel. I hello **Lagringsanslutningssträng för kontot** dialogrutan, du kan också ange om du vill toouse hello HTTPS slutpunkter (hello standardalternativet), http-slutpunkter hello eller anpassade slutpunkter. Du kan bestämma toouse anpassade slutpunkter om du har registrerat ett anpassat domännamn för din tjänst, enligt beskrivningen i [konfigurera ett anpassat domännamn för blob-data i ett Azure storage-konto](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> Du måste ändra din anslutning strängar toopoint tooan Azure storage-konto innan du distribuerar din tjänst. Detta kan orsaka din roll inte toostart misslyckas toodo eller toocycle via hello initiera upptagen och stoppa tillstånd.
> 
> 

## <a name="endpoints-page"></a>Slutpunkter
En arbetsroll kan ha valfritt antal HTTP, HTTPS eller TCP-slutpunkter. Slutpunkter kan vara inkommande slutpunkter som är tillgängliga tooexternal klienter, eller interna slutpunkter som är tillgängliga tooother roller som körs i hello-tjänsten.

* toomake HTTP endpoint tillgängliga tooexternal klienter och webbläsare, ändra hello endpoint typen tooinput och ange ett namn och en offentlig portnummer.
* toomake ett HTTPS endpoint tillgängliga tooexternal klienter och webbläsare, ändra hello slutpunktstyp för**inkommande**, och ange ett namn, en offentlig portnummer och ett namn på certifikatet.
  
    Observera att innan du kan ange ett certifikat, måste du definiera hello certifikat på hello **certifikat** egenskapssida.
* toomake en slutpunkt som är tillgängliga för intern åtkomst av andra roller i hello molnbaserad tjänst bör ändra hello endpoint typen toointernal och ange ett namn och möjliga privata portar för den här slutpunkten.

## <a name="local-storage-page"></a>Lokal lagring sida
Du kan använda hello **lokal lagring** egenskapen sidan tooreserve en eller flera lokala lagringsresurser för en roll. En resurs för lokal lagring är en reserverad katalog i hello filsystem hello Azure-dator i en instans av en roll är igång.

## <a name="certificates-page"></a>Sidan Certifikat
På hello **certifikat** sidan kan du koppla certifikat till din roll. hello-certifikat som du lägger till kan vara används tooconfigure HTTPS-slutpunkter på hello **slutpunkter** egenskapssida.

Hej **certifikat** egenskapssidan lägger till information om ditt certifikat tooyour tjänstkonfigurationen. Observera att certifikaten inte paketeras med din tjänst. Du måste ladda upp dina certifikat separat tooAzure via hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885).

tooassociate ett certifikat med din roll, ange ett namn för hello certifikatet. Du använder det här namnet toorefer toohello certifikatet när du konfigurerar en HTTPS-slutpunkt på hello **slutpunkter** egenskapssida. Ange sedan om hello certifikatarkivet är **lokal dator** eller **aktuell användare** och hello namnet på hello store. Slutligen ange hello certifikatets tumavtryck. Om hello certifikatet i hello aktuella User\Personal (min) store, kan du ange hello certifikatets tumavtryck genom att välja hello certifikat från en lista. Om det finns någon annanstans, ange hello tumavtrycksvärde manuellt.

När du lägger till ett certifikat från certifikatarkivet hello läggs automatiskt toohello konfigurationsinställningar du alla mellanliggande certifikat. Dessa mellanliggande certifikat måste överföras tooAzure i ordning toocorrectly konfigurerar tjänsten för SSL.

Alla certifikat som associeras med din tjänst gäller tooyour tjänsten endast när den körs i hello molnet. När din tjänst körs i hello lokala utvecklingsmiljö använder en standard certifikat som hanteras av hello beräkningsemulatorn.

## <a name="configuring-hello-azure-cloud-service-project"></a>Konfigurera hello Azure cloud service-projekt
tooconfigure inställningar som gäller tooan hela Azure cloud service-projekt, öppnas hello snabbmenyn för projektnoden, och välj Egenskaper tooopen egenskapssidorna. hello följande tabell visar de egenskapssidorna.

| Egenskapssida | Beskrivning |
| --- | --- |
| Program |Du kan visa information om hello-versionen av Azure-verktyg som använder den här molntjänstprojekt från den här sidan och du kan uppgradera toohello nuvarande version av hello verktyg. |
| Skapa händelser |Du kan ange build före och efter build händelser från den här sidan. |
| Utveckling |Du kan ange build konfigurationsanvisningar och hello villkor under vilka händelser efter build körs från den här sidan. |
| Webb |Du kan konfigurera inställningar som gäller toohello webbservern från den här sidan. |

