---
title: aaaHow toocreate, hantera eller ta bort ett lagringskonto i hello klassiska Azure-portalen | Microsoft Docs
description: "Skapa ett nytt lagringskonto, hantera åtkomstnycklarna för ditt konto eller ta bort ett lagringskonto i hello Azure-portalen. Läs mer om premium- och standardlagringskonton."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 5e4f4360-3f81-4d63-a0b1-e7771b67af11
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: robinsh
ms.openlocfilehash: 6ee2359e02c7c9e9c111e1fc87a6160bb8b785b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-azure-storage-accounts"></a>Om Azure-lagringskonton
[!INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Översikt
Ett Azure storage ger dig åtkomst toohello Azure Blob, kön, tabell, och Filtjänster i Azure Storage. Ditt lagringskonto tillhandahåller hello unika namnrymden för dina Azure Storage-dataobjekt. Hello data i ditt konto är tillgängligt endast tooyou hello kontoägaren som standard.

Det finns två typer av lagringskonton:

* Ett standardlagringskonto som erbjuder blobb-, tabell-, kö- och fillagring.
* Ett premiumlagringskonto som för närvarande endast stöder virtuella datorer i Azure. En detaljerad översikt över Premium Storage finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](storage-premium-storage.md).

## <a name="storage-account-billing"></a>Fakturering för lagringskonto
Du debiteras för användningen av Azure Storage baserat på ditt lagringskonto. Storage-kostnaderna baseras på fyra faktorer: lagringskapacitet, replikeringsschema, lagringstransaktioner och utgående datatrafik.

* Lagringskapacitet syftar toohow mycket av lagringskontots du använder toostore data. hello kostnaden för att lagra data bestäms av hur mycket data du lagrar och hur de replikeras.
* Replikeringen anger hur många kopior av dina data som hanteras på samma gång och på vilka platser.
* Transaktioner finns tooall läsa och skriva operations tooAzure lagring.
* Datatrafik refererar toodata överförs ut från en Azure-region. När hello data i ditt lagringskonto används av ett program som inte körs i hello samma region om som programmet är en molnbaserad tjänst eller någon annan typ av program och debiteras du för utgående datatrafik. (För Azure-tjänster, du kan vidta åtgärder toogroup dina data och tjänster i hello samma data centers tooreduce eller eliminera kostnaderna för utgående datatrafik.)  

Hej [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage) innehåller detaljerad prisinformation för lagringskapacitet, replikering och transaktioner. Hej [prisinformation om dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/) innehåller detaljerad prisinformation för utgående datatrafik.

Detaljerad information om ett lagringskontos kapacitets- och prestandamål finns i [Skalbarhets- och prestandamål för Azure Storage](storage-scalability-targets.md).

> [!NOTE]
> När du skapar en virtuell dator i Azure skapas storage-konto du automatiskt hello distribution plats om du inte redan har ett lagringskonto på den här platsen. Det är därför inte nödvändigt toofollow hello stegen nedan toocreate ett lagringskonto för virtuella diskar. hello lagringskontonamnet baseras på hello virtuellt datornamn. Se hello [dokumentation för Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) för mer information.
> 
> 

## <a name="create-a-storage-account"></a>skapar ett lagringskonto
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Klicka på **ny** i hello Aktivitetsfältet på hello hello sidans nederkant. Välj **Datatjänster** | **Storage** och klicka sedan på **Snabbregistrering**.
   
    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)
3. I **URL** anger du ett namn för ditt lagringskonto.
   
   > [!NOTE]
   > Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener.
   > 
   > Namnet på ditt lagringskonto måste vara unikt i Azure. hello klassiska Azure-portalen visar om hello lagringskontonamn du väljer är upptaget.
   > 
   > 
   
    Se [slutpunkter för Lagringskonto](#storage-account-endpoints) nedan för information om hur hello lagringskontonamnet kommer att använda tooaddress dina objekt i Azure Storage.
4. I **plats/Tillhörighetsgrupp**, Välj en plats för ditt lagringskonto som är nära tooyou eller tooyour kunder. Om data i ditt lagringskonto ska kunna nås från en annan Azure-tjänst, till exempel en virtuell Azure-dator eller tjänst i molnet, kanske du vill tooselect en tillhörighetsgrupp från hello listan toogroup ditt lagringskonto i hello samma datacenter med andra Azure-tjänster att du använder tooimprove prestanda och sänka kostnaderna.
   
    Observera att du måste välja en tillhörighetsgrupp när ditt lagringskonto skapas. Du kan inte flytta en befintlig affinitetsgrupp för kontot tooan. Mer information om tillhörighetsgrupper finns i  [Samplacering av tjänster med en tillhörighetsgrupp](#service-co-location-with-an-affinity-group) nedan.
   
   > [!IMPORTANT]
   > toodetermine vilka platser som är tillgänglig för din prenumeration, kan du anropa hello [lista över alla resursproviders](https://msdn.microsoft.com/library/azure/dn790524.aspx) igen. toolist providers med hjälp av PowerShell anropar [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Från .NET använder hello [lista](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) metod för hello klassen ProviderOperationsExtensions.
   > 
   > Mer information om vilka tjänster som är tillgängliga i vilken region finns i [Azure-regioner](https://azure.microsoft.com/regions/#services).
   > 
   > 
5. Om du har mer än en Azure-prenumeration kan sedan hello **prenumeration** fältet visas. I **prenumeration**, ange hello Azure-prenumeration som du vill toouse hello lagringskontot med.
6. I **replikering**, Välj hello önskad replikeringsnivå för ditt lagringskonto. hello rekommenderade replikeringsalternativet är geo-redundant replikering, som ger maximal hållbarhet för dina data. Mer information om replikeringsalternativen för Azure Storage finns i [Azure Storage-replikering](storage-redundancy.md).
7. Klicka på **Create Storage Account** (Skapa lagringskonto).
   
    Det kan ta några minuter toocreate ditt lagringskonto. Du kan övervaka hello meddelanden längst hello hello Azure klassiska Portal toocheck hello status. När hello storage-konto har skapats, har ditt nya lagringskonto **Online** status och är redo för användning.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)

### <a name="storage-account-endpoints"></a>Slutpunkter för lagringskonto
Alla objekt som du lagrar i Azure Storage har en unik URL-adress. hello storage-konto namnet formulär hello underdomänen i den adressen. hello kombinationen av underdomän och domännamn namn, som är specifika tooeach tjänst, utgör ett *endpoint* för ditt lagringskonto.

Om ditt lagringskonto heter exempelvis *mittlagringskonto*, hello standardslutpunkterna för ditt lagringskonto är:

* Blob-tjänst: http://*mittlagringskonto*.blob.core.windows.net
* Tabelltjänst: http://*mittlagringskonto*.table.core.windows.net
* Kötjänst: http://*mittlagringskonto*.queue.core.windows.net
* Filtjänst: http://*mittlagringskonto*.file.core.windows.net

Du kan se hello slutpunkter för ditt lagringskonto på instrumentpanelen för lagring av hello på hello [klassiska Azure-portalen](https://manage.windowsazure.com) när hello-konto har skapats.

hello URL: en för att komma åt ett objekt i ett lagringskonto skapas genom att lägga till hello objektets plats i hello storage-konto toohello slutpunkt. En blobbadress kan till exempel ha följande format: http://*mittlagringskonto*.blob.core.windows.net/*minbehållare*/*minblobb*.

Du kan också konfigurera en anpassad domän namnet toouse med ditt lagringskonto. Mer information finns i [Konfigurera ett eget domännamn för din slutpunkt för Blob Storage](storage-custom-domain-name.md).

### <a name="service-co-location-with-an-affinity-group"></a>Samplacering av tjänster med en tillhörighetsgrupp
En *tillhörighetsgrupp* är en geografisk gruppering av dina Azure-tjänster och virtuella datorer med ditt Azure Storage-konto. En tillhörighetsgrupp kan förbättra tjänstprestanda genom att placera datorarbetsbelastningar i hello samma data center eller nära hello målgrupp för användaren. Dessutom inga faktureringskostnader för utgående trafik när komma åt data i ett lagringskonto från en annan tjänst som är en del av hello samma tillhörighetsgrupp.

> [!NOTE]
> toocreate en tillhörighetsgrupp öppna hello <b>inställningar</b> område i hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på <b>Tillhörighetsgrupper</b>, och klicka sedan på antingen <b>Lägg till en tillhörighetsgruppen</b> eller hello <b>Lägg till</b> knappen. Du kan också skapa och hantera tillhörighetsgrupper med hjälp av hello Azure Service Management API. Mer information finns i <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Åtgärder med tillhörighetsgrupper</a>.
> 
> 

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Visa, kopiera och återskapa åtkomstnycklar för lagring
När du skapar ett lagringskonto genererar Azure två 512-bitars åtkomstnycklar för lagring, som används för autentisering vid åtkomst av hello storage-konto. Genom att tillhandahålla två åtkomstnycklar för lagring, kan du tooregenerate hello nycklarna utan avbrott tooyour lagringstjänsten eller toothat-tjänsten för dataåtkomst.

> [!NOTE]
> Vi rekommenderar att du inte delar dina åtkomstnycklar för lagring med andra. toopermit åtkomst toostorage resurser utan att lämna ut dina åtkomstnycklar, du kan använda en *signatur för delad åtkomst*. En signatur för delad åtkomst ger åtkomst tooa resurs i ditt konto för ett intervall som du definierar och med hello behörigheter som du anger. Mer information finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md).
> 
> 

I hello [klassiska Azure-portalen](https://manage.windowsazure.com), använda **hantera nycklar** på hello instrumentpanel eller hello **lagring** sidan tooview och kopiera åtkomstnycklar för generera hello lagring som används tooaccess hello Blob-, tabell-och kön.

### <a name="copy-a-storage-access-key"></a>Kopiera en lagringsåtkomstnyckel
Du kan använda **hantera nycklar** toocopy en lagring access key toouse i en anslutningssträng. hello anslutningssträngen kräver hello lagringskontonamn och en nyckel toouse vid autentisering. För information om hur du konfigurerar anslutningen strängar tooaccess Azure-lagringstjänster, se [konfigurera Azure Storage-anslutningssträngar](storage-configure-connection-string.md).

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **lagring**, och klicka sedan på hello namnet på hello konto tooopen hello lagringsinstrumentpanel.
2. Klicka på **Hantera nycklar**.
   
     **Hantera åtkomstnycklar** öppnas.
   
    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)
3. toocopy en lagringsåtkomstnyckel väljer hello nyckeltexten. Högerklicka sedan och klicka på **Kopiera**.

### <a name="regenerate-storage-access-keys"></a>Återskapa åtkomstnycklar för lagring
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
2. Återskapa hello primära åtkomstnyckeln för ditt lagringskonto. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), från hello instrumentpanelen eller hello **konfigurera** klickar du på **hantera nycklar**. Klicka på **återskapa** under hello primärnyckeln och klicka sedan på **Ja** tooconfirm som du vill toogenerate en ny nyckel.
3. Uppdatera hello anslutningssträngar i din kod tooreference hello nya primärnyckeln.
4. Återskapa hello sekundära åtkomstnyckeln.

## <a name="delete-a-storage-account"></a>Ta bort ett lagringskonto
tooremove ett lagringskonto som du inte längre använder, Använd **ta bort** på hello instrumentpanel eller hello **konfigurera** sidan. **Ta bort** borttagningar hello hela lagringskontot, inklusive alla hello blobbar, tabeller och köer i hello-konto.

> [!WARNING]
> Det är inte möjlig toorestore ett borttaget lagringskonto eller hämta hello innehåll som det innehöll före borttagningen. Vara säker på att tooback in något du toosave innan du tar bort hello-kontot. Detta gäller även alla resurser i hello-konto – när du tar bort en blob, tabell, kö eller fil den bort permanent.
> 
> Om ditt lagringskonto innehåller VHD-filer för en virtuell Azure-dator, du måste ta bort alla avbildningar och diskar som använder dessa VHD-filer innan du kan ta bort hello storage-konto. Stoppa först hello virtuella datorn om den körs, och ta sedan bort den. toodelete diskar, navigera toohello **diskar** och tar bort diskarna där. toodelete bilder, navigera toohello **bilder** och tar bort eventuella avbildningar som lagras i hello-kontot.
> 
> 

1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **lagring**.
2. Klicka någonstans i hello lagringskontot utom hello namn och klicka sedan på **ta bort**.
   
     Eller
   
    Klicka på hello konto tooopen hello lagringsinstrumentpanel hello namn och klicka sedan på **ta bort**.
3. Klicka på **Ja** tooconfirm som du vill toodelete hello storage-konto.

## <a name="next-steps"></a>Nästa steg
* toolearn mer om Azure Storage finns hello [Azure Storage-dokumentationen](https://azure.microsoft.com/documentation/services/storage/).
* Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/).
* [Överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md)

