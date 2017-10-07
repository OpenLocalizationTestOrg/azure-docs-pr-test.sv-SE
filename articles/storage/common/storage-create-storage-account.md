---
title: aaaHow toocreate, hantera eller ta bort ett lagringskonto i hello Azure-portalen | Microsoft Docs
description: "Skapa ett nytt lagringskonto, hantera åtkomstnycklarna för ditt konto eller ta bort ett lagringskonto i hello Azure-portalen. Läs mer om premium- och standardlagringskonton."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 87c37da0-6cc6-4d88-a330-ef2896a1531d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
f1_keywords: sql13.swb.windowsazurestorage.connect.f1
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: 3a2a07c4131fbe594c7007f59950bf94339809fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-storage-accounts"></a>Om Azure-lagringskonton
[!INCLUDE [storage-selector-portal-create-storage-account](../../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Översikt
Ett Azure storage-konto innehåller ett unikt namnområde toostore och komma åt dina Azure Storage-dataobjekt. Alla objekt i ett lagringskonto faktureras tillsammans som en grupp. Hello data i ditt konto är tillgängligt endast tooyou hello kontoägaren som standard.

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Fakturering för lagringskonto
[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

> [!NOTE]
> När du skapar en virtuell dator i Azure skapas storage-konto du automatiskt hello distribution plats om du inte redan har ett lagringskonto på den här platsen. Det är därför inte nödvändigt toofollow hello stegen nedan toocreate ett lagringskonto för virtuella diskar. hello lagringskontonamnet baseras på hello virtuellt datornamn. Se hello [dokumentation för Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) för mer information.
> 
> 

## <a name="storage-account-endpoints"></a>Slutpunkter för lagringskonto
Alla objekt som du lagrar i Azure Storage har en unik URL-adress. hello storage-konto namnet formulär hello underdomänen i den adressen. hello kombinationen av underdomän och domännamn namn, som är specifika tooeach tjänst, utgör ett *endpoint* för ditt lagringskonto.

Om ditt lagringskonto heter exempelvis *mittlagringskonto*, hello standardslutpunkterna för ditt lagringskonto är:

* Blob-tjänst: http://*mittlagringskonto*.blob.core.windows.net
* Tabelltjänst: http://*mittlagringskonto*.table.core.windows.net
* Kötjänst: http://*mittlagringskonto*.queue.core.windows.net
* Filtjänst: http://*mittlagringskonto*.file.core.windows.net

> [!NOTE]
> Ett Blob storage-konto exponerar endast hello Blob-tjänsteslutpunkt.
> 
> 

hello URL: en för att komma åt ett objekt i ett lagringskonto skapas genom att lägga till hello objektets plats i hello storage-konto toohello slutpunkt. En blobbadress kan till exempel ha följande format: http://*mittlagringskonto*.blob.core.windows.net/*minbehållare*/*minblobb*.

Du kan också konfigurera en anpassad domän namnet toouse med ditt lagringskonto. Mer information finns i [Konfigurera ett eget domännamn för din slutpunkt för Blob Storage](../blobs/storage-custom-domain-name.md). Du kan också konfigurera det med PowerShell. Mer information finns i hello [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) cmdlet.  


## <a name="create-a-storage-account"></a>skapar ett lagringskonto
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. På navmenyn hello väljer **ny** -> **lagring** -> **lagringskonto**.
3. Ange ett namn för lagringskontot. Se [slutpunkter för Lagringskonto](#storage-account-endpoints) för information om hur hello lagringskontonamnet kommer att använda tooaddress dina objekt i Azure Storage.
   
   > [!NOTE]
   > Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.
   > 
   > Namnet på ditt lagringskonto måste vara unikt i Azure. hello Azure-portalen visar om hello lagringskontonamn du väljer används redan.
   > 
   > 
4. Ange hello distribution modellen toobe används: **Resource Manager** eller **klassiska**. **Hanteraren för filserverresurser** rekommenderas hello distributionsmodell. Mer information finns i [Förstå Resource Manager-distribution och klassisk distribution](../../azure-resource-manager/resource-manager-deployment-model.md).
   
   > [!NOTE]
   > BLOB storage-konton kan bara skapas med hjälp av hello Resource Manager-modellen.

5. Välj hello typ av lagringskonto: **generella** eller **Blob storage**. **Generella** hello standard.
   
    Om **generella** har valts och sedan ange hello prestandanivån: **Standard** eller **Premium**. hello standardvärdet är **Standard**. Mer information om standard och premium storage-konton finns [introduktion tooMicrosoft Azure Storage](storage-introduction.md) och [Premium-lagring: högpresterande lagring för arbetsbelastningar på virtuella Azure](storage-premium-storage.md).
   
    Om **Blobblagring** har valts och sedan ange hello åtkomstnivå: **frekvent** eller **kall**. hello standardvärdet är **frekvent**. Mer information finns i [Azure Blob Storage: nivåerna Kall och Het](../blobs/storage-blob-storage-tiers.md).
6. Välj hello replikeringsalternativ för lagringskontot hello: **LRS**, **GRS**, **RA-GRS**, eller **ZRS**. hello standardvärdet är **RA-GRS**. Mer information om replikeringsalternativen för Azure Storage finns i [Azure Storage-replikering](storage-redundancy.md).
7. Välj hello prenumeration där du vill toocreate hello nytt lagringskonto.
8. Ange en ny resursgrupp eller välj en befintlig resursgrupp. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
9. Välj hello geografisk plats för ditt lagringskonto. Mer information om vilka tjänster som är tillgängliga i vilken region finns i [Azure-regioner](https://azure.microsoft.com/regions/#services).
10. Klicka på **skapa** toocreate hello storage-konto.

## <a name="manage-your-storage-account"></a>Hantera ditt lagringskonto
### <a name="change-your-account-configuration"></a>Ändra kontokonfigurationen
När du har skapat ditt lagringskonto kan du ändra dess konfiguration, till exempel ändra hello replikeringsalternativet som används för hello konto eller ändra hello åtkomstnivå för ett Blob storage-konto. I hello [Azure-portalen](https://portal.azure.com), navigera tooyour storage-konto, Sök och klickar på **Configuration** under **inställningar** tooview och/eller ändra hello-kontokonfigurationen.

> [!NOTE]
> Vissa replikeringsalternativ kanske inte tillgänglig beroende på hello prestandanivå du valde när du skapar hello storage-konto.
> 
> 

Ändra hello replikeringsalternativ så ändras ditt pris. Mer information finns på sidan med [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

För Blob storage-konton, ändra åtkomstnivå för hello kan tillkomma ändras avgifter för hello dessutom toochanging ditt pris. Se hello [Blob storage-konton - priser och fakturering](../blobs/storage-blob-storage-tiers.md#pricing-and-billing) för mer information.

### <a name="manage-your-storage-access-keys"></a>Hantera dina åtkomstnycklar för lagring
När du skapar ett lagringskonto genererar Azure två 512-bitars åtkomstnycklar för lagring, som används för autentisering vid åtkomst av hello storage-konto. Genom att tillhandahålla två åtkomstnycklar för lagring, kan du tooregenerate hello nycklarna utan avbrott tooyour lagringstjänsten eller toothat-tjänsten för dataåtkomst.

> [!NOTE]
> Vi rekommenderar att du inte delar dina åtkomstnycklar för lagring med andra. toopermit åtkomst toostorage resurser utan att lämna ut dina åtkomstnycklar, du kan använda en *signatur för delad åtkomst*. En signatur för delad åtkomst ger åtkomst tooa resurs i ditt konto för ett intervall som du definierar och med hello behörigheter som du anger. Mer information finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md).
> 
> 
<a id="view-and-copy-storage-access-keys"/></a>
#### <a name="view-and-copy-storage-access-keys"></a>Visa och kopiera åtkomstnycklar för lagring
I hello [Azure-portalen](https://portal.azure.com)navigerar tooyour storage-konto, klickar du på **alla inställningar** och klicka sedan på **åtkomstnycklar** tooview, kopiera och återskapa åtkomstnycklarna för ditt konto. Hej **åtkomstnycklar** bladet innehåller också förkonfigurerade anslutningssträngar med dina primära och sekundära nycklar som du kan kopiera toouse i dina program.

#### <a name="regenerate-storage-access-keys"></a>Återskapa åtkomstnycklar för lagring
Vi rekommenderar att du ändrar hello snabbtangenter tooyour lagring konto regelbundet toohelp Behåll säker lagringsanslutningar. Två åtkomstnycklar tilldelas så att du kan upprätthålla anslutningar toohello storage-konto med hjälp av ena åtkomstnyckeln medan du återskapar hello andra snabbtangent.

> [!WARNING]
> Återskapar åtkomstnycklarna kan påverka tjänster i Azure som dina program som är beroende av hello storage-konto. Alla klienter som använder hello access key tooaccess hello storage-konto måste vara uppdaterade toouse hello ny nyckel.
> 
> 

**Media services** -om du har medietjänster som är beroende av ditt lagringskonto, du måste nlängd hello åtkomstnycklarna med medietjänsten när du har återskapat hello nycklar.

**Program** - om du har webbprogram eller cloud services att använda hello storage-konto, förlorar du hello anslutningar om du återskapa nycklar, såvida inte du lanserar dina nycklar.

**Lagringsutforskare** - om du använder något [lagringsutforskare](storage-explorers.md), behöver du förmodligen tooupdate hello lagringsnyckel som används av programmen.

Här är hello processen för att rotera åtkomstnycklar för lagring:

1. Uppdatera hello anslutningssträngar i ditt program kod tooreference hello sekundära åtkomstnyckeln för hello storage-konto.
2. Återskapa hello primära åtkomstnyckeln för ditt lagringskonto. På hello **åtkomstnycklar** bladet, klickar du på **återskapa Nyckel1**, och klicka sedan på **Ja** tooconfirm som du vill toogenerate en ny nyckel.
3. Uppdatera hello anslutningssträngar i din kod tooreference hello nya primärnyckeln.
4. Generera hello sekundär åtkomst nyckeln i hello samma sätt.

## <a name="delete-a-storage-account"></a>Ta bort ett lagringskonto
tooremove ett lagringskonto som du inte längre använder navigerar toohello storage-konto i hello [Azure-portalen](https://portal.azure.com), och klicka på **ta bort**. Tar bort ett lagringskonto hello hela konto, inklusive alla data i hello-konto.

> [!WARNING]
> Det är inte möjlig toorestore ett borttaget lagringskonto eller hämta hello innehåll som det innehöll före borttagningen. Vara säker på att tooback in något du toosave innan du tar bort hello-kontot. Detta gäller även alla resurser i hello-konto – när du tar bort en blob, tabell, kö eller fil den bort permanent.
> 

Om du försöker toodelete ett lagringskonto som är kopplad till en virtuell Azure-dator, kan du får ett felmeddelande om hello lagringskontot som fortfarande används. Information om hur du felsöker det här felet finns i [Felsöka fel när du tar bort lagringskonton](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

## <a name="next-steps"></a>Nästa steg
* [Microsoft Azure Lagringsutforskaren](../../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Azure Blob Storage: nivåerna Kall och Het](../blobs/storage-blob-storage-tiers.md)
* [Azure Storage-replikering](storage-redundancy.md)
* [Konfigurera anslutningssträngar för Azure Storage](../storage-configure-connection-string.md)
* [Överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md)
* Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).

