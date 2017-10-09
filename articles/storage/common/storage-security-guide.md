---
title: "säkerhetsguiden för aaaAzure Storage | Microsoft Docs"
description: "Information om hello många metoder för att skydda Azure Storage, inklusive men inte begränsat tooRBAC, Storage Service-kryptering, kryptering på klientsidan, SMB 3.0 och Azure Disk Encryption."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 6f931d94-ef5a-44c6-b1d9-8a3c9c327fb2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 1f5a4e724e00ea6d16f5511b9120154f89441758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-security-guide"></a>Azure Storage-säkerhetsguiden
## <a name="overview"></a>Översikt
Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner som tillsammans ger utvecklare möjligheten toobuild säkra program. Hej lagringskonto själva kan skyddas med hjälp av rollbaserad åtkomstkontroll och Azure Active Directory. Data kan skyddas vid överföring mellan en program- och Azure med hjälp av [Client Side Encryption](../storage-client-side-encryption.md), HTTPS- eller SMB 3.0. Data kan ställas in toobe krypteras automatiskt när skrivs tooAzure Storage med [Storage Service kryptering (SSE)](storage-service-encryption.md). Operativsystemet och datadiskarna som används av virtuella datorer kan ställas in toobe som krypterats med [Azure Disk Encryption](../../security/azure-security-disk-encryption.md). Delegerad åtkomst toohello dataobjekt i Azure Storage kan beviljas med [signaturer för delad åtkomst](../storage-dotnet-shared-access-signature-part-1.md).

Den här artikeln ger en översikt över var och en av dessa funktioner som kan användas med Azure Storage. Länkar finns tooarticles som ger information om varje funktion så att du enkelt göra ytterligare undersökningar på varje avsnitt.

Här följer hello avsnitt toobe som beskrivs i den här artikeln:

* [Hantering av plan säkerhet](#management-plane-security) – skydda ditt Lagringskonto

  hello management plan som består av hello resurser används toomanage ditt lagringskonto. I det här avsnittet kommer vi hello Azure Resource Manager-distributionsmodellen och hur toouse rollbaserad åtkomstkontroll (RBAC) toocontrol åt tooyour storage-konton. Kommer dessutom pratar vi om hur du hanterar din lagringskontonycklar och hur tooregenerate dem.
* [Plan för datasäkerhet](#data-plane-security) – skydda åtkomst tooYour Data

  I det här avsnittet ska vi titta på att tillåta åtkomst toohello faktiska dataobjekt i ditt lagringskonto, till exempel blobbar, filer, köer och tabeller som använder signaturer för delad åtkomst och åtkomstregler lagras. Vi tar upp både servicenivåer SAS och konto-nivå SAS. Vi också se hur toolimit åt tooa specifik IP-adress (eller intervall av IP-adresser), hur toolimit hello protokoll som används tooHTTPS och hur toorevoke en signatur för delad åtkomst utan att vänta på den tooexpire.
* [Kryptering vid överföring](#encryption-in-transit)

  Det här avsnittet beskrivs hur toosecure data vid överföring till eller från Azure Storage. Lär dig om hello rekommenderar användning av HTTPS och hello kryptering som används av SMB 3.0 för Azure-filresurser. Vi kommer också ta en titt på klientsidan kryptering, vilket gör att du tooencrypt hello data innan det överförs till lagring i ett klientprogram och toodecrypt hello data när det överförs utanför lagring.
* [Kryptering i vila](#encryption-at-rest)

  Vi pratar om Storage Service-kryptering (SSE) och hur du kan aktivera det för ett lagringskonto, vilket resulterar i din blockblobbar, sidblobbar, och tilläggsblobar krypteras automatiskt när skrivas tooAzure lagring. Vi kommer också titta på hur du kan använda Azure Disk Encryption och utforska hello grundläggande skillnader och fall av diskkryptering jämfört med SSE jämfört med kryptering på klientsidan. Vi ser kort på FIPS-kompatibilitet för USA Offentliga datorer.
* Med hjälp av [Storage Analytics](#storage-analytics) tooaudit åtkomst till Azure Storage

  Det här avsnittet beskrivs hur toofind information i hello storage analytics loggar för en begäran. Vi ta en titt på verkliga storage analytics loggdata och se hur toodiscern om en begäran görs med hello lagring konto nyckel med en signatur för delad åtkomst eller anonymt, och om den har lyckats eller misslyckats.
* [Aktivera webbläsarbaserade klienter med hjälp av CORS](#Cross-Origin-Resource-Sharing-CORS)

  Det här avsnittet innehåller information om hur tooallow cross-origin resursdelning (CORS). Lär dig om korsdomänåtkomst och hur toohandle med hello CORS funktioner inbyggda i Azure Storage.

## <a name="management-plane-security"></a>Hantering av plan säkerhet
hello management plan består av åtgärder som påverkar hello lagringskonto sig själv. Du kan till exempel skapa eller ta bort ett lagringskonto, hämta en lista över storage-konton i en prenumeration, hämta hello lagringskontonycklar eller återskapa hello nycklar för lagringskonto.

När du skapar ett nytt lagringskonto, Välj en distributionsmodell för klassiska eller Resource Manager. hello modell för att skapa resurser i Azure kan bara fullständiga åtkomst toohello prenumerationen och i sin tur hello storage-konto.

Den här guiden fokuserar på hello Resource Manager-modellen som är hello rekommenderade metoder för att skapa storage-konton. Du kan styra åtkomsten på ett mer begränsad nivå toohello management plan med hjälp av rollbaserad åtkomstkontroll (RBAC) och hello Resource Manager storage-konton, i stället för ger åtkomst toohello hela prenumerationen och.

### <a name="how-toosecure-your-storage-account-with-role-based-access-control-rbac"></a>Hur toosecure lagringen konto med rollbaserad åtkomstkontroll (RBAC)
Vi pratar om RBAC är och hur du kan använda den. Varje Azure-prenumerationen har en Azure Active Directory. Användare, grupper och program från katalogen kan beviljas åtkomst toomanage resurser i hello Azure-prenumeration som använder hello Resource Manager-modellen. Detta är refererad tooas rollbaserad åtkomstkontroll (RBAC). toomanage detta åtkomst, kan du använda hello [Azure-portalen](https://portal.azure.com/), hello [Azure CLI-verktygen](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), eller hello [Azure Storage Resource Provider REST-API: ](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Med hello Resource Manager-modellen placera hello storage-konto i en resurs grupp och kontroll åtkomst toohello management plan för den specifika storage-konto med Azure Active Directory. Exempelvis kan du ge specifika användare hello möjlighet tooaccess hello lagringskontonycklar, medan andra användare kan visa information om hello storage-konto, men kan inte komma åt hello lagringskontonycklar.

#### <a name="granting-access"></a>Att bevilja åtkomst
Komma åt genom att tilldela hello lämpliga RBAC rollen toousers, grupper och program för hello rätt område. toogrant åtkomst toohello hela prenumerationen, tilldela en roll på hello prenumerationsnivå. Du kan bevilja åtkomst tooall hello resurser i en resursgrupp genom att bevilja behörigheter toohello resursgruppen sig själv. Du kan också tilldela specifika roller toospecific resurser, till exempel storage-konton.

Här följer hello huvudpunkter som du behöver tooknow om hur du använder RBAC tooaccess hello hanteringsåtgärder för ett Azure Storage-konto:

* När du tilldelar åtkomst kan tilldela du i princip ett konto som har rollen toohello som du vill toohave åtkomst. Du kan styra åtkomst toohello operations används toomanage detta lagringskonto, men toohello dataobjekt inte i hello-konto. Exempelvis kan du bevilja behörigheten tooretrieve hello egenskaper för hello storage-konto (till exempel redundans), men inte tooa behållare eller data i en behållare i Blob Storage.
* För någon toohave behörighet tooaccess hello dataobjekt i hello storage-konto, kan du ge dem behörighet tooread hello lagringskontonycklar och användaren kan sedan använda dessa nycklar tooaccess hello blobbar, köer, tabeller och filer.
* Roller kan tilldelas tooa specifikt användarkonto, en grupp av användare eller tooa specifika program.
* Varje roll har en lista över och inte åtgärder. Hello virtuella deltagarrollen har till exempel en åtgärd av ”listKeys” som kan läsa hello storage-konto nycklar toobe. hello deltagare har ”inte åtgärder”, som uppdatering hello åtkomst för användare i hello Active Directory.
* Roller för lagring inkluderar (men är inte begränsade till) hello följande:

  * Ägaren – de kan hantera allt, inklusive åtkomst.
  * Deltagare – de kan göra något hello ägare kan förutom tilldela åtkomst. Användare med den här rollen kan visa och återskapa hello nycklar för lagringskonto. De kan komma åt hello dataobjekt med hello lagringskontonycklar.
  * Läsare kan visa information om hello lagringskontot utom hemligheter. Till exempel om du tilldelar en roll med reader-behörighet på hello storage-konto toosomeone, de kan visa hello egenskaper för hello storage-konto, men de kan inte göra några ändringar toohello egenskaper eller visa hello lagringskontonycklar.
  * Storage-konto deltagare – de kan hantera hello storage-konto – de kan läsa hello prenumerationens resursgrupper och resurser, och skapa och hantera distributionen av resursgrupper prenumeration. De kan också komma åt hello lagringskontonycklar, vilket i sin tur innebär att de kan komma åt hello dataplan.
  * Administratör för användaråtkomst – de kan hantera lagring toohello-användarkontot. De kan till exempel ge läsaren åtkomst tooa specifika användare.
  * Virtual Machine-deltagare – som de kan hantera virtuella datorer men inte hello storage-konto toowhich de är anslutna. Den här rollen kan visa hello lagringskontonycklar, vilket innebär att hello användaren toowhom du tilldela den här rollen kan uppdatera hello dataplan.

    För en användare toocreate en virtuell dator ha måste de toobe kan toocreate hello motsvarande VHD-filen i ett lagringskonto. toodo, de behöver toobe kan tooretrieve hello lagring konto nyckeln och överför den toohello API skapar hello VM. De måste därför ha den här behörigheten så att de kan visa hello lagringskontonycklar.
* hello möjlighet toodefine anpassade roller är en funktion som gör att du toocompose en uppsättning åtgärder från en lista över tillgängliga åtgärder kan utföras på Azure-resurser.
* hello användare har toobe ställa in i Azure Active Directory innan du kan tilldela en roll toothem.
* Du kan skapa en rapport över som beviljats/återkallas vilken typ av åtkomst till och från vilken och på vilka scope med hjälp av PowerShell eller hello Azure CLI.

#### <a name="resources"></a>Resurser
* [Azure Active Directory rollbaserad åtkomstkontroll](../../active-directory/role-based-access-control-configure.md)

  Den här artikeln förklarar hello Azure Active Directory-rollbaserad åtkomstkontroll och hur det fungerar.
* [RBAC: inbyggda roller](../../active-directory/role-based-access-built-in-roles.md)

  Den här artikeln beskrivs alla hello inbyggda roller finns i RBAC.
* [Förstå Resource Manager-distribution och klassisk distribution](../../azure-resource-manager/resource-manager-deployment-model.md)

  Den här artikeln beskriver hello Resource Manager-distribution och klassisk distributionsmodeller och förklarar hello fördelarna med att använda hello Resource Manager och resursgrupper. Den förklarar hur hello Azure Compute-, nätverks- och Lagringsprovidrar fungerar under hello Resource Manager-modellen.
* [Hantera rollbaserad åtkomstkontroll med hello REST API](../../active-directory/role-based-access-control-manage-access-rest.md)

  Den här artikeln visar hur toouse hello REST API toomanage RBAC.
* [Azure Storage Resource Provider REST API-referens](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Detta är hello referens för hello API: er som du kan använda toomanage ditt lagringskonto via programmering.
* [Developer's guide tooauth med Azure Resource Manager API](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

  Den här artikeln visar hur tooauthenticate med hello Resource Manager API: er.
* [Rollbaserad åtkomstkontroll i Microsoft Azure från Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Det här är en video på Channel 9 från hello 2015 MS Ignite konferens tooa för länken. I den här sessionen de talar om åtkomst till hanterings- och rapporteringsfunktioner i Azure och utforska bästa metoderna för att skydda åtkomsten tooAzure prenumerationer med hjälp av Azure Active Directory.

### <a name="managing-your-storage-account-keys"></a>Hantera dina nycklar för Lagringskonto
Nycklarna är 512-bitars strängar som skapats av Azure och som tillsammans med hello lagring kontonamn, Storage-konto kan vara används tooaccess hello dataobjekt som lagras i hello storage-konto, t.ex. blobbar, entiteter i en tabell, Kömeddelanden och filer på en filresurs på Azure. Styra toohello storage-konto nycklar åtkomstkontroller komma åt toohello dataplan för det lagringskontot.

Varje lagringskonto har två nycklar som avses tooas ”nyckel 1” och ”nyckel 2” Hej [Azure-portalen](http://portal.azure.com/) och i hello PowerShell-cmdlets. Dessa kan återskapas manuellt med hjälp av en av flera metoder, inklusive men inte begränsat toousing hello [Azure-portalen](https://portal.azure.com/), PowerShell, hello Azure CLI eller programmässigt med hello Storage-klientbibliotek för .NET eller hello Azure Storage Services REST API.

Det finns flera orsaker tooregenerate din lagringskontonycklar.

* Du kan återskapa dem regelbundet av säkerhetsskäl.
* Du kan återskapa din lagringskontonycklar om någon hanteras toohack till ett program och hämta hello-nyckel som hårdkodad eller sparats i en konfigurationsfil och få fullständig åtkomst tooyour storage-konto.
* En annan fallet för nycklar är om ditt team använder ett Lagringsutforskaren program som behåller hello lagringskontonyckel och en av hello teammedlemmar lämnar. hello programmet fortsätter toowork, ger dem åtkomst tooyour storage-konto när de är borta. Detta är faktiskt hello Huvudorsaken de skapats kontonivå signaturer för delad åtkomst – du kan använda en kontonivå SAS istället för att lagra hello snabbtangenter i en konfigurationsfil.

#### <a name="key-regeneration-plan"></a>Nyckeln återskapas plan
Vill du inte toojust generera hello nyckel som du använder utan lite planering. Om du gör det kan du klippa ut alla åtkomst toothat storage-konto, vilket kan orsaka avbrott i större. Det är därför det finns två nycklar. Du bör Generera en nyckel i taget.

Innan du återskapar dina nycklar måste du ha en lista över alla program som är beroende av hello storage-konto, samt andra tjänster som du använder i Azure. Till exempel om du använder Azure Media Services som är beroende av ditt lagringskonto måste du synkroniserar hello åtkomstnycklarna med medietjänsten när du har återskapat hello nyckel. Om du använder alla program, till exempel en lagringsutforskare behöver tooprovide hello nya nycklar toothose program samt. Observera att om du har virtuella datorer vars VHD-filer som lagras i hello storage-konto kan de inte påverkas av de hello lagringskontonycklarna genereras.

Du kan återskapa dina nycklar i hello Azure-portalen. När nycklar genereras om de kan ta upp too10 minuter toobe synkroniserade över lagringstjänster.

När du är klar är här hello allmänna processen med information om hur du ska ändra din nyckel. I det här fallet hello antas att du använder nyckel 1 och du kommer toochange allt toouse nyckel 2 i stället.

1. Återskapa nyckeln 2 tooensure att den är skyddad. Du kan göra detta i hello Azure-portalen.
2. Ändra hello lagring viktiga toouse nyckel 2 nytt värde i alla hello program där hello lagringsnyckel lagras. Testa och publicera programmet hello.
3. När alla av hello program och tjänster som är igång och körs, återskapa nyckel 1. Detta säkerställer att vem som helst toowhom som du inte uttryckligen har gett hello ny nyckel inte längre har åtkomst till toohello storage-konto.

Om du använder nyckel 2, kan du använda samma process, men omvänd hello nyckelnamn hello.

Du kan migrera över ett par dagar, ändra varje ny nyckel för programmet toouse hello och publicera den. När alla är klar bör du gå tillbaka och återskapa hello gamla nyckeln så att den inte längre fungerar.

Ett annat alternativ är tooput hello lagringskontonyckel i en [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) som en hemlighet och ha dina program hämta hello nyckel därifrån. Sedan när du återskapa hello nyckeln och uppdaterar hello Azure Key Vault behöver hello program inte toobe omdistribueras eftersom de automatiskt ska hämta hello ny nyckel från hello Azure Key Vault. Observera att du kan ha hello programmet att läsa hello varje gång du behöver den eller du kan cachelagra i minnet och om det misslyckas när du använder den, hämta hello nyckeln igen från hello Azure Key Vault.

Med hjälp av Azure Key Vault lägger till ytterligare en säkerhetsnivå för dina lagringsnycklar. Om du använder den här metoden kan har du aldrig hello lagring viktiga hårdkodad i en konfigurationsfil, vilket tar bort den minimering av någon och få åtkomst toohello nycklar utan uttryckligt tillstånd.

En annan fördel med att använda Azure Key Vault är att du kan också styra tooyour snabbtangenter med Azure Active Directory. Det innebär att du kan bevilja åtkomst toohello handfull program som behöver tooretrieve hello nycklar från Azure Key Vault och veta att andra program inte kan tooaccess hello nycklar utan att ge dem behörighet specifikt.

Obs: Vi rekommenderar toouse endast en av hello nycklar i alla dina program på hello samma tid. Om du använder nyckel 1 på vissa platser och nyckel 2 i andra kommer inte att kunna toorotate dina nycklar utan ett program att förlora åtkomsten.

#### <a name="resources"></a>Resurser
* [Om Azure Storage-konton](storage-create-storage-account.md#regenerate-storage-access-keys)

  Den här artikeln ger en översikt över lagringskonton och beskriver visa, kopiera och återskapa åtkomstnycklar för lagring.
* [Azure Storage Resource Provider REST API-referens](https://msdn.microsoft.com/library/mt163683.aspx)

  Den här artikeln innehåller länkar toospecific artiklar om hämtning hello lagringskontonycklar och återskapar hello lagringskontonycklar för ett Azure-konto med hjälp av hello REST API. Obs: Detta är Resource Manager storage-konton.
* [Åtgärder på storage-konton](https://msdn.microsoft.com/library/ee460790.aspx)

  Den här artikeln i hello Storage Service Manager REST API Reference innehåller länkar toospecific artiklar om hämtning och de hello lagringskontonycklarna genereras med hjälp av hello REST API. Obs: Detta är hello klassiska lagringskonton.
* [Säg goodbye tookey management – hantera åtkomst tooAzure Storage data med hjälp av Azure AD](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

  Den här artikeln visar hur toouse Active Directory toocontrol åtkomstnycklar tooyour Azure Storage i Azure Key Vault. Den visar även hur toouse en Azure Automation jobbet tooregenerate hello nycklar timme.

## <a name="data-plane-security"></a>Säkerhet för data-plan
Plan för datasäkerhet refererar toohello-metoder används toosecure hello dataobjekt som lagras i Azure Storage – hello blobbar, köer, tabeller och filer. Vi har sett metoder tooencrypt hello data och säkerhet vid överföring av hello data, men hur skaffar du om att tillåta åtkomst toohello objekt?

Det finns två metoder för att styra åtkomst toohello dataobjekt sig själva i princip. hello först är genom att kontrollera åtkomst toohello lagringskontonycklar och hello andra använder signaturer för delad åtkomst toogrant åtkomst toospecific dataobjekt för en viss tidsperiod.

Ett undantag toonote är att du kan låta offentlig åtkomst tooyour blobbar genom att ange hello åtkomstnivån för hello-behållare som innehåller hello blobbar i enlighet med detta. Om du ställer in åtkomst för en tooBlob behållaren eller behållaren tillåter offentlig läsbehörighet för hello blobbar i behållaren. Det innebär att alla med en URL som pekar tooa blobb i behållaren kan öppna den i en webbläsare utan hjälp av en signatur för delad åtkomst eller hello lagringskontonycklar.

### <a name="storage-account-keys"></a>Lagringskontonycklar
Lagringskontonycklar är 512-bitars strängar som skapats av Azure som tillsammans med hello lagringskontonamnet kan vara används tooaccess hello dataobjekt som lagras i hello storage-konto.

Du kan till exempel läsa blobbar, skriva tooqueues, skapa tabeller och ändra filer. Många av de här åtgärderna kan utföras via hello Azure-portalen eller med hjälp av ett av många Lagringsutforskaren program. Du kan också skriva kod toouse hello REST API eller någon av hello Lagringsklientbiblioteken tooperform dessa åtgärder.

Enligt beskrivningen i avsnittet hello på hello [Management plan säkerhet](#management-plane-security), åtkomstnycklar toohello lagring för ett klassiskt storage-konto kan beviljas genom att ge fullständig åtkomst toohello Azure-prenumeration. Snabbtangenter toohello lagring för ett lagringskonto med hjälp av hello Azure Resource Manager-modellen kan kontrolleras via rollbaserad åtkomstkontroll (RBAC).

### <a name="how-toodelegate-access-tooobjects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Hur toodelegate åt tooobjects i ditt konto med hjälp av signaturer för delad åtkomst och åtkomstregler lagras
En signatur för delad åtkomst är en sträng som innehåller en säkerhetstoken som kan vara ansluten tooa URI som du kan använda toodelegate åtkomst toostorage objekt och ange begränsningarna, t.ex hello behörigheter och hello tidsvärdet mängd åtkomst.

Du kan bevilja åtkomst tooblobs, behållare, Kömeddelanden, filer och tabeller. Med tabeller, kan du faktiskt bevilja behörighet tooaccess en mängd entiteter i hello tabell genom att ange hello partition och raden nyckelintervall toowhich du vill hello toohave användaråtkomst. Till exempel om du har data som lagras med en partitionsnyckel för geografiska tillstånd, kan du ge någon åtkomst toojust hello data för California.

Ett annat exempel är kan du ge en SAS-token som gör det möjligt toowrite poster tooa kö för ett webbprogram och ger en arbetare rollen programmet en SAS-token tooget meddelanden från hello kö och bearbeta dem.. Eller du kan ge en kund en SAS-token som de kan använda tooupload bilder tooa behållare i Blob Storage och ge en web application behörighet tooread dessa bilder. I båda fallen finns en uppdelning av säkerhetsskäl – varje program kan få bara hello tillgång som som krävs för att ordna tooperform sina uppgifter. Detta är möjligt via hello signaturer för delad åtkomst.

#### <a name="why-you-want-toouse-shared-access-signatures"></a>Varför du vill toouse signaturer för delad åtkomst
Varför kan det vara bra toouse en SAS istället för att bara ge ut din lagringskontonyckel som är mycket enklare? Lämnar ut din lagringskontonyckel är som att dela hello nycklar kungarikets din lagring. Fullständig åtkomst beviljas. Någon kan använda dina nycklar och överföra sina hela musik biblioteket tooyour storage-konto. De kan även ersätta filerna med virus infekterade versioner eller stjäla dina data. Ge bort obegränsad åtkomst tooyour storage-konto är något som inte bör noga.

Du kan ge bara hello behörigheter som krävs för en begränsad tidsperiod på en klient med signaturer för delad åtkomst. Till exempel om någon laddar upp en blob tooyour konto, beviljar du dem skrivbehörighet för bara tillräckligt med tid tooupload hello blob (beroende på hello storleken på hello blob naturligtvis). Och om du ändrar dig kan du återkalla den åtkomsten.

Dessutom kan du ange att begäranden som görs med hjälp av en SAS är begränsad tooa vissa IP-adress eller IP-adressintervall externa tooAzure. Du kan också kräva att begäranden som görs med ett visst protokoll (HTTPS eller HTTP/HTTPS). Det innebär att om du bara vill tooallow HTTPS-trafik, du kan ange hello krävs protokollet tooHTTPS och HTTP-trafik blockeras.

#### <a name="definition-of-a-shared-access-signature"></a>Definition av en signatur för delad åtkomst
En signatur för delad åtkomst är en uppsättning frågeparametrar läggs toohello URL som pekar på hello resurs

som innehåller information om hello åtkomst tillåts och hello tidsperiod för vilken hello åtkomst är tillåten. Här är ett exempel. den här URI ger läsbehörighet tooa blob för fem minuter. Observera att SAS fråga parametrar måste vara URL-kodade, till exempel % 3A för kolon (:) eller % 20 för ett blanksteg.

```
http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL toohello blob)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for hello authentication of hello SAS)
```

#### <a name="how-hello-shared-access-signature-is-authenticated-by-hello-azure-storage-service"></a>Hur hello signatur för delad åtkomst autentiseras av hello Azure Storage-tjänst
När hello storage-tjänst tar emot hello begäran det hello indatafrågan parametrar och skapar en signatur hello med samma metod som hello anropande programmet. Den jämför sedan hello två signaturer. Om de accepterar sedan hello lagringstjänsten Kontrollera hello storage service version toomake att den är giltig, kontrollera att hello aktuellt datum och tid är inom hello angivet fönster, se till att hello åtkomst begärd motsvarar toohello begäran, osv.

Till exempel med våra URL misslyckas om du hello URL: en pekar tooa fil i stället för en blob denna begäran eftersom den anger att hello signatur för delad åtkomst är för en blob. Om hello REST-kommando som anropas var tooupdate en blob kan misslyckas det eftersom hello signatur för delad åtkomst anger att bara läsbehörighet tillåts.

#### <a name="types-of-shared-access-signatures"></a>Typer av signaturer för delad åtkomst
* En tjänstnivå SAS kan vara används tooaccess specifika resurser i ett lagringskonto. Några exempel på detta hämtar en lista över blobbar i en behållare, hämtar en blob, uppdaterar en entitet i en tabell, lägga till meddelanden tooa kön eller ladda upp en fil tooa filresurs.
* En kontonivå SAS kan vara att använda tooaccess allt som en tjänstnivå SAS kan användas för. Dessutom kan den ge alternativ tooresources som inte tillåts med en tjänstnivå SAS, till exempel hello möjlighet toocreate behållare, tabeller, köer och filresurser. Du kan också ange toomultiple tjänster på samma gång. Till exempel du kanske kan ge någon åtkomst tooboth blobbar och filer på ditt lagringskonto.

#### <a name="creating-an-sas-uri"></a>Skapa en SAS-URI
1. Du kan skapa en ad hoc-URI för begäran, definiera frågeparametrar Hej varje gång.

   Detta är mycket flexibelt, men om du har en logisk uppsättning parametrar som liknar varje gång med hjälp av en åtkomstprincip för lagras är en bättre uppfattning.
2. Du kan skapa en åtkomstprincip lagras för en hela behållaren, filresurser, tabell eller kön. Du kan sedan använda den som hello bas för hello SAS-URI som du skapar. Behörigheter som baseras på lagras åtkomstprinciper kan enkelt återkallas. Du kan ha upp too5 principer som definierats för varje behållare, kön, tabell eller filresurs.

   Till exempel om du tänkte toohave många läsa hello blobbar i en specifik behållare, du kan skapa en åtkomstprincip för lagras som säger ”ge läsbehörighet” och andra inställningar som kommer att vara hello samma varje gång. Du kan skapa ett SAS-URI med hello inställningarna för hello lagrat åtkomstprincip och ange hello förfallodatum för datum/tid. hello fördelen med detta är att du inte har toospecify alla hello fråga parametrar varje gång.

#### <a name="revocation"></a>Återkallade certifikat
Anta att din SAS har komprometterats eller om du vill toochange den på grund av företagets säkerhet och regelefterlevnad krav. Hur du återkalla åtkomst tooa resursen med hjälp av den SAS? Det beror på hur du skapar hello SAS-URI.

Om du använder ad hoc-URI finns det tre alternativ. Du kan utfärda SAS-token med kort giltighetsprinciper och bara vänta hello SAS tooexpire. Du kan byta namn på eller ta bort hello resurs (under förutsättning att hello token är begränsade tooa objekt). Du kan ändra hello lagringskontonycklar. Det här sista alternativet kan ha stor inverkan, beroende på hur många tjänster använder detta lagringskonto och förmodligen inte något som du vill ha toodo utan lite planering.

Om du använder en SAS som härletts från en lagrad åtkomstprincip du kan ta bort åtkomst genom att återkalla hello lagrat åtkomstprincip – du kan bara ändra den så att den har redan gått ut eller kan du ta bort det helt och hållet. Detta träder i kraft omedelbart och upphäver varje SAS som skapats med hjälp av den lagrade åtkomstprincip. Uppdatera eller ta bort hello lagras åtkomstprincip kan påverka personer som har åtkomst till den specifika behållare, filresurs, tabell eller kön via SAS men om hello klienter skrivs så att de begär en ny SAS när hello gamla blir ogiltig den här metoden fungerar bra.

Eftersom med hjälp av en SAS som härletts från en lagrad åtkomstprincip ger dig använda hello möjlighet toorevoke SAS omedelbart, är hello rekommenderad bästa praxis tooalways lagras åtkomstprinciper när det är möjligt.

#### <a name="resources"></a>Resurser
Mer detaljerad information om hur du använder signaturer för delad åtkomst och lagras åtkomstprinciper, slutfördes med exempel, finns toohello följande artiklar:

* Dessa är hello referensartiklar.

  * [Tjänst-SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    Den här artikeln innehåller exempel på användning av en tjänstnivå SAS med blobbar, Kömeddelanden, tabell intervall och filer.
  * [Hur du skapar en tjänst-SAS](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Hur du skapar en konto-SAS](https://msdn.microsoft.com/library/mt584140.aspx)
* Dessa är självstudier om att använda hello .NET klienten biblioteket toocreate signaturer för delad åtkomst och åtkomstregler lagras.

  * [Använda signaturer för delad åtkomst (SAS)](../storage-dotnet-shared-access-signature-part-1.md)
  * [Signaturer för delad åtkomst, del 2: Skapa och använda en SAS med hello Blob-tjänsten](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Den här artikeln innehåller en förklaring av hello SAS-modellen, exempel på signaturer för delad åtkomst och rekommendationer om bästa praxis för hello använder SAS. Vi beskriver även hello återkallande av hello behörighet.
* Begränsa åtkomst via IP-adress (IP-ACL: er)

  * [Vad är en slutpunkt åtkomstkontrollistan (ACL)?](../../virtual-network/virtual-networks-acl.md)
  * [Hur du skapar en tjänst-SAS](https://msdn.microsoft.com/library/azure/dn140255.aspx)

    Detta är hello referensartikeln om principer för servicenivåer SAS; den innehåller ett exempel på IP-ACLing.
  * [Hur du skapar en konto-SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx)

    Detta är hello referensartikeln om principer för kontonivå SAS; den innehåller ett exempel på IP-ACLing.
* Autentisering

  * [Autentisering för hello Azure Storage-tjänster](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Signaturer för delad åtkomst komma igång Självstudier

  * [SAS komma igång-Självstudier](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Kryptering under överföring
### <a name="transport-level-encryption--using-https"></a>Transportnivå kryptering – med hjälp av HTTPS
Ytterligare ett steg som du bör vidta tooensure hello säkerheten för dina Azure Storage-data är tooencrypt hello data mellan hello-klienten och Azure Storage. hello första rekommenderas att tooalways använda hello [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protokollet, vilket garanterar säker kommunikation över hello offentliga Internet.

toohave en säker kommunikationskanal du bör alltid använda HTTPS när anropar hello REST API: er eller få åtkomst till objekt i lagringen. Dessutom **signaturer för delad åtkomst**, som kan vara används toodelegate komma åt tooAzure lagringsobjekt får innehålla en alternativet toospecify som endast hello HTTPS-protokollet kan användas när du använder signaturer för delad åtkomst, att säkerställa att vem som helst Skicka länkar med SAS-token används hello rätt protokoll.

Du kan tillämpa hello HTTPS används när du anropar hello REST API: er tooaccess objekt i storage-konton genom att aktivera [säker överföring krävs](../storage-require-secure-transfer.md) för hello storage-konto. Anslutningar som använder HTTP ska avvisas när det här är aktiverat.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Med hjälp av kryptering under överföring med Azure-filresurser
Azure File storage stöder HTTPS när du använder hello REST-API, men är vanliga som en SMB filresurs kopplad tooa VM. SMB 2.1 stöder inte kryptering, så anslutningar är bara tillåtna inom hello samma region i Azure. Dock SMB 3.0 stöder kryptering, och den är tillgänglig i Windows Server 2012 R2, Windows 8, Windows 8.1 och Windows 10, vilket gör att flera region åtkomst och även åtkomst på hello skrivbordet.

Observera att medan Azure-filresurser kan användas med Unix, hello Linux SMB-klienten ännu inte stöder kryptering, så åtkomst tillåts endast i en Azure-region. Stöd för kryptering för Linux finns på hello översikt av Linux-utvecklare som ansvarar för SMB-funktioner. När de lägger till kryptering har du samma möjlighet för åtkomst till en Azure-filresurs på Linux som du gör för Windows hello.

Du kan tillämpa hello användning av kryptering med hello Azure Files tjänsten genom att aktivera [säker överföring krävs](../storage-require-secure-transfer.md) för hello storage-konto. Om du använder hello REST API: er, HTTPs krävs. För SMB, kan endast SMB-anslutningar som stöder kryptering ansluta.

#### <a name="resources"></a>Resurser
* [Hur toouse Azure File storage med Linux](../storage-how-to-use-files-linux.md)

  Den här artikeln visar hur toomount en Azure-fil för att dela på en Linux-system och överför/hämta filer.
* [Komma igång med Azure File Storage i Windows](../storage-dotnet-how-to-use-files.md)

  Den här artikeln ger en översikt över Azure-filresurser och hur toomount och använda dem med PowerShell och .NET.
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)

  Den här artikeln annonserar hello allmän tillgänglighet för Azure File storage och ger teknisk information om hello SMB 3.0-kryptering.

### <a name="using-client-side-encryption-toosecure-data-that-you-send-toostorage"></a>Med hjälp av klientsidan kryptering toosecure data som du skickar toostorage
Ett annat alternativ som hjälper dig att se till att dina data är säkra medan de överförs mellan ett klientprogram och lagring är kryptering på klientsidan. hello data krypteras innan de överförs till Azure Storage. När du hämtar hello data från Azure Storage, dekrypteras hello data när den tas emot på klientsidan för hello. Även om hello krypteras data gå över hello överföring, rekommenderar vi att du också använda HTTPS, eftersom den har dataintegritetskontroller inbyggda som minimera nätverksfel påverkar hello integriteten hos hello data.

Klientsidans kryptering är också en metod för att kryptera data i vila, som hello data lagras i krypterad form. Lär dig om detta i detalj i avsnittet hello på [kryptering i vila](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Kryptering i vila
Det finns tre Azure-funktioner som tillhandahåller kryptering i vila. Azure Disk Encryption är används tooencrypt hello OS- och datadiskar i IaaS-virtuella datorer. Hej andra två – klientsidan kryptering och SSE – är både används tooencrypt data i Azure Storage. Vi tittar på var och en av dessa, gör en jämförelse och se när var och en kan användas.

Medan du kan använda Client side Encryption tooencrypt hello data under överföring (som lagras också i krypterad form i lagring) måste kanske du föredrar toosimply Använd HTTPS under överföringen hello och har för hello data toobe krypteras automatiskt när det är lagras. Det finns två sätt toodo detta--Azure Disk Encryption och SSE. En används toodirectly kryptera hello data på Operativsystemet och datadiskarna som används av virtuella datorer och hello andra används tooencrypt data skrivs tooAzure Blob Storage.

### <a name="storage-service-encryption-sse"></a>Storage Service-kryptering (SSE)
SSE kan toorequest att hello lagringstjänsten automatiskt kryptera hello data när du skriver den tooAzure lagring. När du har läst hello data från Azure Storage, kommer det dekrypteras av hello lagringstjänsten innan de returneras. Detta gör att du toosecure dina data utan att behöva toomodify code eller lägga till kod tooany program.

Det här är en inställning som gäller toohello hela lagringskontot. Du kan aktivera och inaktivera den här funktionen genom att ändra hello hello inställningens värde. toodo, du kan använda hello Azure-portalen PowerShell hello Azure CLI, hello REST-API för Storage-Provider eller hello Storage-klientbibliotek för .NET. SSE är inaktiverat som standard.

För tillfället hanteras hello nycklar som används för kryptering av hello av Microsoft. Vi skapa hello nycklar ursprungligen och hantera hello säker lagring av hello nycklar samt hello reguljära rotation som definieras av intern Microsoft-princip. I framtida hello, kommer du få hello möjlighet toomanage era egna krypteringsnycklar och och ange en migreringssökväg till från Microsoft-hanterad nycklar toocustomer-hanterade nycklar.

Den här funktionen är tillgänglig för Standard- och Premium-lagring konton som skapats med hjälp av hello Resource Manager-modellen. SSE gäller endast tooblock blobbar, sidblobar, och tilläggsblobar. hello krypteras andra typer av data, inklusive tabeller, köer och filer, inte.

Data krypteras endast när SSE är aktiverat och hello data skrivs tooBlob lagring. Aktivera eller inaktivera SSE påverkar inte befintliga data. Med andra ord, när du aktiverar den här kryptering kommer det inte gå tillbaka och kryptera data som finns redan. inte heller att dekryptera hello data som redan finns när du inaktiverar SSE.

Om du vill toouse den här funktionen med ett klassiskt storage-konto kan du skapa ett nytt lagringskonto för hanteraren för filserverresurser och använder AzCopy toocopy hello data toohello nytt konto.

### <a name="client-side-encryption"></a>Kryptering på klientsidan
Kryptering på klientsidan nämndes när du ska diskutera hello kryptering av hello data under överföringen. Den här funktionen kan du tooprogrammatically kryptera data i klientprogram innan den skickas över hello överföring toobe skrivs tooAzure lagring och tooprogrammatically dekryptera dina data efter hämtning från Azure Storage.

Detta tillhandahåller kryptering under överföring, men det ger också hello-funktionen för kryptering i vila. Observera att även om hello data krypteras under överföringen, vi rekommenderar ändå med hjälp av HTTPS tootake nytta av hello inbyggda dataintegritet kontrollerar som minimera nätverksfel påverkar hello integriteten hos hello data.

Ett exempel på där du kan använda detta är om du har ett program som lagrar blobbar och hämtar BLOB och du vill att hello program och data toobe så säkert som möjligt. I så fall använder du kryptering på klientsidan. hello trafik mellan hello-klienten och hello Azure Blob-tjänsten innehåller hello krypterade resurs och ingen kan tolka hello data under överföringen och fylla i ditt privata blobbar.

Kryptering på klientsidan är inbyggd i hello Java och hello .NET lagringsklientbiblioteken, som i sin tur använder hello Azure Key Vault API: er, vilket gör det ganska enkelt för du tooimplement. hello-processen för att kryptera och dekryptera data hello använder hello kuvert teknik och lagrar metadata som används av hello kryptering i varje lagringsobjekt. Till exempel för BLOB, lagras den i hello blob-metadata, medan för köer, läggs tooeach kömeddelande.

Du kan generera och hantera egna krypteringsnycklar för hello kryptering själva. Du kan också använda nycklar som genereras av hello Azure Storage-klientbibliotek eller du kan ha hello Azure Key Vault generera hello nycklar. Du kan lagra krypteringsnycklar i din lokala lagring eller du kan lagra dem i en Azure Key Vault. Azure Key Vault kan toogrant åtkomst toohello hemligheter i Azure Key Vault toospecific användare med Azure Active Directory. Det innebär att inte bara alla kan läsa hello Azure Key Vault och hämta hello nycklar som du använder för kryptering på klientsidan.

#### <a name="resources"></a>Resurser
* [Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)

  Den här artikeln visar hur toouse klientsidan kryptering med Azure Key Vault, inklusive hur toocreate hello KEK och lagra den på hello valvet med hjälp av PowerShell.
* [Kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](../storage-client-side-encryption.md)

  Den här artikeln ger en förklaring av klientsidan kryptering och ger exempel på användning av hello lagring klienten tooencrypt och dekryptera biblioteksresurser från hello fyra lagringstjänster. Det är också pratar om Azure Key Vault.

### <a name="using-azure-disk-encryption-tooencrypt-disks-used-by-your-virtual-machines"></a>Med hjälp av Azure Disk Encryption tooencrypt diskar som används av virtuella datorer
Azure Disk Encryption är en ny funktion. Den här funktionen kan du tooencrypt hello OS diskar och datadiskar som används av en virtuell IaaS-dator. För Windows krypteras hello-enheter med BitLocker-kryptering branschstandard. För Linux krypteras hello diskar med hello DM-Crypt teknik. Detta är integrerad med Azure Key Vault tooallow du toocontrol och hantera hello disk krypteringsnycklar.

hello-lösningen stöder hello följande scenarier för virtuella IaaS-datorer när de är aktiverade i Microsoft Azure:

* Integrering med Azure Key Vault
* Standardnivån VMs: [A, D, DS, G, GS och så vidare serien IaaS-VM](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Aktivera kryptering på Windows och Linux virtuella IaaS-datorer
* Om du inaktiverar kryptering på Operativsystemet och enheter för Windows IaaS-VM
* Om du inaktiverar kryptering på dataenheter för Linux virtuella IaaS-datorer
* Aktivera kryptering på virtuella IaaS-datorer som kör windowsklient-OS
* Aktivera kryptering på volymer med monteringssökvägar
* Aktivera kryptering på den virtuella Linux-datorer som är konfigurerade med disk-striping (RAID) med hjälp av mdadm
* Aktivera kryptering på den virtuella Linux-datorer med hjälp av LVM för datadiskar
* Aktivera kryptering på virtuella Windows-datorer som har konfigurerats med hjälp av lagringsutrymmen
* Alla offentliga Azure-regioner som stöds

hello-lösningen stöder inte hello följande scenarier, funktioner och teknik i hello versionen:

* Grundnivån IaaS-VM
* Om du inaktiverar kryptering på en OS-enhet för Linux virtuella IaaS-datorer
* Virtuella IaaS-datorer som skapas med hjälp av hello klassiska Virtuella metod för skapande av
* Integrering med din lokala nyckelhanteringstjänst
* Azure File storage (delade filsystem), Network File System (NFS), dynamiska volymer och virtuella Windows-datorer som är konfigurerade med programvarubaserad RAID-system


> [!NOTE]
> Linux OS-diskkryptering stöds för närvarande på hello följande Linux-distributioner: RHEL 7.2, CentOS 7.2n och Ubuntu 16.04.
>
>

Den här funktionen garanterar att krypteras alla data på virtuella diskar i vila i Azure Storage.

#### <a name="resources"></a>Resurser
* [Azure Disk Encryption för Windows och Linux-IaaS-VM](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Jämförelse mellan Azure Disk Encryption, SSE och kryptering på klientsidan
#### <a name="iaas-vms-and-their-vhd-files"></a>Virtuella IaaS-datorer och deras VHD-filer
För diskar som används av virtuella IaaS-datorer, bör du använda Azure Disk Encryption. Du kan aktivera SSE tooencrypt hello VHD-filer som används tooback dessa diskar i Azure Storage, men krypteras bara nyskrivna data. Det innebär att om du skapar en virtuell dator och sedan aktivera SSE på hello storage-konto som innehåller hello VHD-filen bara hello ändringar krypteras, hello inte ursprungliga VHD-filen.

Om du skapar en virtuell dator med en bild från hello Azure Marketplace Azure utför en [ytlig kopiera](https://en.wikipedia.org/wiki/Object_copying) av hello avbildningen tooyour storage-konto i Azure Storage, och det är inte krypterat även om du har aktiverat SSE. När den skapar hello VM och börjar uppdatera hello bilden startar SSE kryptera hello data. Därför är det bästa toouse Azure Disk Encryption på virtuella datorer skapas från bilder i hello Azure Marketplace om du vill att de fullständigt krypterade.

Om du sätta förkrypterade VM i Azure från lokala kommer du att kunna tooupload hello kryptering nycklar tooAzure Key Vault och fortsätter med hello kryptering för den virtuella datorn som du använde lokala. Azure Disk Encryption är aktiverade toohandle det här scenariot.

Om du har en icke-krypterade VHD från lokala kan du överföra den till hello gallery som en anpassad avbildning och etablera en virtuell dator från den. Om du gör detta med hjälp av hello Resource Manager-mallar, ber du den tooturn på Azure Disk Encryption när den startar hello VM.

När du lägger till en datadisk och montera på hello VM, kan du aktivera Azure Disk Encryption på disken data. Den kommer att kryptera data disken lokalt först och sedan hello service management layer gör lazy skrivåtgärder mot storage så hello lagring innehåll krypteras.

#### <a name="client-side-encryption"></a>Kryptering av klientsidan
Kryptering på klientsidan är hello säkraste metod för att kryptera dina data eftersom den krypterar före överföringen och krypterar hello data i vila. Men det kräver att du lägger till kod tooyour program med hjälp av lagringen, vilket inte kanske du vill toodo. I sådana fall kan använda du HTTPs för dina data under överföringen och SSE tooencrypt hello data i vila.

Du kan kryptera tabellentiteter, Kömeddelanden och blobbar med kryptering på klientsidan. Du kan endast kryptera blobbar med SSE. Om du behöver tabell och kön data toobe krypterad, bör du använda kryptering på klientsidan.

Kryptering på klientsidan hanteras helt av programmet hello. Det här är hello säkraste metod, men kräver toomake programmässiga ändringar tooyour program och gjorda nyckelhantering processer. Du skulle använda detta när du vill hello extra säkerhet under överföring, och du vill att din toobe för lagrade data krypteras.

Kryptering på klientsidan är mer belastningen på hello klienten, och du har tooaccount för detta i planer för skalbarhet särskilt om du krypterar och överför stora mängder data.

#### <a name="storage-service-encryption-sse"></a>Storage Service-kryptering (SSE)
SSE hanteras av Azure Storage. Med hjälp av SSE tillhandahåller inte för hello säkerheten för hello data under överföring, men den krypterar hello data som skrivs tooAzure lagring. Det finns ingen inverkan på hello prestanda när du använder den här funktionen.

Du kan endast kryptera blockblobbar, tilläggsblobbar och sidblobbar med hjälp av SSE. Om du behöver tooencrypt tabelldata eller kön data bör du använda kryptering på klientsidan.

Om du har ett arkiv eller bibliotek av VHD-filer som du använder som grund för att skapa nya virtuella datorer kan du skapa ett nytt lagringskonto, aktivera SSE och sedan ladda upp hello VHD-filer toothat konto. Dessa VHD-filer krypteras av Azure Storage.

Om du har aktiverat för hello diskar i en virtuell dator och SSE aktiverad på hello storage-konto och hålla hello VHD-filer för Azure Disk Encryption fungerar den bra; Det leder till att alla nyligen skrivs data som krypteras två gånger.

## <a name="storage-analytics"></a>Lagringsanalys
### <a name="using-storage-analytics-toomonitor-authorization-type"></a>Med Storage Analytics toomonitor auktorisering typen
Du kan aktivera loggning för Azure Storage Analytics tooperform och lagra mätvärdesdata för varje storage-konto. Detta är ett bra verktyg toouse när du vill toocheck hello prestandamått för ett lagringskonto eller behöver tootroubleshoot storage-konto eftersom du har prestandaproblem med.

En annan typ av data som du ser i hello storage analytics loggar är hello autentiseringsmetod som används av någon när de har åtkomst till lagring. Du kan till exempel se om de används en signatur för delad åtkomst eller hello lagringskontonycklar, eller om hello blob nås var offentliga i med Blob Storage.

Det kan vara mycket användbart om du nära skyddar åtkomst toostorage. Till exempel i Blob Storage kan du ange alla hello behållare tooprivate och implementera hello användning av en SAS-tjänst i dina program. Du kan kontrollera hello loggar regelbundet toosee om dina blobbar används med hello lagringskontonycklar, vilket kan tyda på ett sekretessbrott, eller om hello blobbar är offentlig men de får inte vara.

#### <a name="what-do-hello-logs-look-like"></a>Vad hello loggar ut?
När du aktiverar hello storage-konto mätvärden och loggning via hello Azure-portalen analysdata startar tooaccumulate snabbt. hello loggning och mått för varje tjänst är separat; hello loggning skrivs endast när aktiviteten är i detta lagringskonto medan loggas hello mätvärden varje minut, varje timme eller varje dag, beroende på hur du konfigurerar.

hello loggfilerna lagras i blockblobbar i en behållare med namnet $logs i hello storage-konto. Den här behållaren skapas automatiskt när Storage Analytics är aktiverad. När den här behållaren har skapats kan du ta bort det, även om du tar bort innehållet.

Det finns en mapp för varje tjänst under hello $logs behållare, och det finns undermappar för hello år/månad/dag/timme. Timma numreras bara hello loggar. Det här är vad hello katalogstruktur ser ut:

![Visa loggfiler](./media/storage-security-guide/image1.png)

Varje begäran tooAzure lagring är inloggad. Här är en ögonblicksbild av en loggfil som visar hello första få fält.

![Ögonblicksbild av en loggfil](./media/storage-security-guide/image2.png)

Du kan se att du kan använda hello loggar tootrack alla slags anrop tooa storage-konto.

#### <a name="what-are-all-of-those-fields-for"></a>Vad är alla dessa fält för?
Det finns en artikel som anges i hello resurserna nedan som visar hello lista över hello många fält i hello loggar och vad de används. Här är hello lista med fält i ordning:

![Ögonblicksbild av fälten i en loggfil](./media/storage-security-guide/image3.png)

Vi är intresserad av hello poster för GetBlob och hur de autentiseras, så vi behöver toolook för transaktioner med åtgärdstypen ”Get-Blob” och kontrollera hello begäran-status (4<sup>th</sup> kolumn) och hello auktoriserings-typ (8<sup>th</sup> kolumn).

Till exempel i hello först några rader i hello listan ovan, hello begäran-statusen ”klar” och hello auktoriserings-type ”autentiseras”. Det innebär att hello begäran har verifierats med hjälp av hello lagringskontonyckel.

#### <a name="how-are-my-blobs-being-authenticated"></a>Hur mitt blob som autentiseras?
Vi har tre fall som vi är intresserad av.

1. hello blob är offentlig och den kan nås med hjälp av en URL utan en signatur för delad åtkomst. I det här fallet hello begäran-status är ”AnonymousSuccess” och hello auktoriserings-typ är ”anonym”.

   1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **anonym**; mystorage...
2. hello blob är privata och användes med en signatur för delad åtkomst. I det här fallet hello begäran-status är ”SASSuccess” och hello auktoriserings-typ är ”sas”.

   1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **SAS**; mystorage...
3. hello blob är privata och hello lagringsnyckel har använt tooaccess den. I det här fallet hello begäran-status är ”**lyckade**” och hello auktoriserings-typen är ”**autentiserade**”.

   1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Lyckade**; 206; 59; 22; **autentiserad**; mystorage...

Du kan använda hello Microsoft Message Analyzer tooview och analysera dessa loggar. Den innehåller Sök och filtrera funktioner. Du kanske exempelvis vill toosearch för instanser av GetBlob toosee om hello användningen är vad du förväntar dig, d.v.s. toomake att någon inte har åtkomst till ditt lagringskonto felaktigt.

#### <a name="resources"></a>Resurser
* [Lagringsanalys](../storage-analytics.md)

  Den här artikeln är en översikt över storage analytics och hur tooenable dem.
* [Storage Analytics loggformat](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  Den här artikeln visar hello Storage Analytics loggformat och information hello tillgängliga fält, inklusive autentisering-typ, som visar hello typ av autentisering som används för hello-begäran.
* [Övervaka ett Lagringskonto i hello Azure-portalen](../storage-monitor-storage-account.md)

  Den här artikeln visar hur tooconfigure övervakning av mätvärden och loggning för ett lagringskonto.
* [Slutpunkt till slutpunkt-felsökning med hjälp av Azure Storage-mätvärden och loggning, AzCopy och Message Analyzer](../storage-e2e-troubleshooting.md)

  Den här artikeln handlar om felsökning med hjälp av hello Storage Analytics och visar hur toouse hello Microsoft Message Analyzer.
* [Microsoft Message Analyzer fungerar Guide](https://technet.microsoft.com/library/jj649776.aspx)

  Den här artikeln är hello referens för hello Microsoft Message Analyzer och innehåller länkar tooa kursen, Snabbstart och Funktionssammanfattning.

## <a name="cross-origin-resource-sharing-cors"></a>Resursdelning för korsande ursprung (CORS)
### <a name="cross-domain-access-of-resources"></a>Korsdomänåtkomst för resurser
När en webbläsare som körs i en domän gör en HTTP-begäran för en resurs från en annan domän, kallas en cross-origin HTTP-begäran. Exempelvis begär en HTML-sida från contoso.com en JPEG-bild som finns på fabrikam.blob.core.windows.net. Av säkerhetsskäl begränsa cross-origin HTTP-begäranden som startas från skript, till exempel JavaScript i webbläsare. Det innebär att när JavaScript-kod på en webbsida på contoso.com begär att jpeg på fabrikam.blob.core.windows.net, hello webbläsare inte tillåter hello-begäran.

Vad gör detta har toodo med Azure Storage? Också, om du lagrar statiska resurser, t.ex JSON- eller XML-filer i Blob Storage med hjälp av ett lagringskonto kallas Fabrikam hello domän för hello tillgångar blir fabrikam.blob.core.windows.net och hello contoso.com webb-applikationen inte kan tooaccess dem. med hjälp av JavaScript eftersom hello domäner är olika. Detta gäller även om du försöker toocall en hello Azure Storage-tjänster – till exempel Table Storage – som returnerar JSON data toobe bearbetas av hello JavaScript-klienten.

#### <a name="possible-solutions"></a>Möjliga lösningar
Enkelriktade tooresolve är tooassign en anpassad domän som ”storage.contoso.com” toofabrikam.blob.core.windows.net. hello problemet är att du kan endast tilldela det anpassade domänen tooone storage-kontot. Vad händer om hello-tillgångar lagras i flera storage-konton?

Ett annat sätt tooresolve detta är toohave hello web application agera som en proxy för hello lagring anrop. Det innebär om du överför en fil tooBlob lagring hello webbprogrammet skulle antingen skriva det lokalt och sedan kopiera tooBlob lagring eller den kommer att läsa in alla i minnet och sedan skriva tooBlob lagring. Alternativt kan skriva du ett dedikerat webbprogram (till exempel en webb-API) som överför hello filer lokalt och skriver dem tooBlob lagring. Oavsett hur du har tooaccount för funktionen när måste bestämma hello skalbarhet.

#### <a name="how-can-cors-help"></a>CORS hur?
Azure Storage kan du tooenable CORS – Cross Origin Resource Sharing. Du kan ange domäner som har åtkomst till hello resurser i detta lagringskonto för varje storage-konto. I vårt fall som beskrivs ovan, vi till exempel aktivera CORS för hello fabrikam.blob.core.windows.net storage-konto och konfigurera den tooallow åtkomst toocontoso.com. Sedan hello web application contoso.com kan komma åt direkt hello resurser i fabrikam.blob.core.windows.net.

En sak toonote är att CORS tillåter åtkomst, men ger inte autentisering, vilket krävs för alla icke-offentlig åtkomst av lagringsresurser. Det innebär att du endast kan komma åt blobar om de är offentliga eller om du inkluderar en signatur för delad åtkomst som ger dig hello behörighet. Tabeller, köer och filer har ingen offentlig åtkomst och kräver en SAS.

Som standard är CORS inaktiverad på alla tjänster. Du kan aktivera CORS med hjälp av hello REST API: et eller hello lagring klienten biblioteket toocall en hello metoder tooset hello-principer. När du gör det kan inkludera du en regel för CORS som XML. Här är ett exempel på en CORS-regel som har angetts med hjälp av hello ange tjänstegenskaper åtgärd för hello Blob-tjänsten för ett lagringskonto. Du kan utföra åtgärden med hello storage-klientbibliotek eller hello REST API: er för Azure Storage.

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Här är vad det innebär att varje rad:

* **AllowedOrigins** ser som icke-matchande domäner kan begära och ta emot data från hello storage-tjänst. Detta innebär att både contoso.com och fabrikam.com kan begära data från Blob Storage för ett visst lagringskonto. Du kan också ange tooa jokertecknet (\*) tooallow alla domäner tooaccess begäranden.
* **AllowedMethods** är hello listan över metoder (http-begäran verb) som kan användas när du gör hello-begäran. I det här exemplet tillåts bara skicka och hämta. Du kan ange tooa jokertecknet (\*) tooallow alla metoder toobe används.
* **AllowedHeaders** detta är hello begäran huvuden som hello ursprungsdomän kan ange när du gör hello-begäran. I det här exemplet tillåts alla metadata huvuden som börjar med x-ms-metadata, x-ms-meta-mål- och x-ms-meta-abc. Hej jokertecknet (\*) anger att alla huvud som börjar med hello angetts prefix är tillåtet.
* **ExposedHeaders** detta talar om vilka svarshuvuden bör exponeras av hello webbläsare toohello begäran utfärdare. I det här exemplet, de huvuden som börjar med ”x-ms - meta-” visas.
* **MaxAgeInSeconds** detta är hello längsta tid att webbläsaren ska cachelagra hello preliminära OPTIONS-begäran. (Mer information om hello preflight-begäran att kontrollera hello första artikel nedan.)

#### <a name="resources"></a>Resurser
Mer information om CORS och hur tooenable, Läs dessa resurser.

* [Stöd för Cross-Origin Resource Sharing (CORS) för hello Azure Storage-tjänster på Azure.com](../storage-cors-support.md)

  Den här artikeln innehåller en översikt över CORS och hur tooset hello regler för hello olika storage-tjänster.
* [Stöd för Cross-Origin Resource Sharing (CORS) för hello Azure Storage-tjänster på MSDN](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Detta är hello i referensdokumentationen för CORS-stöd för hello Azure Storage-tjänster. Detta har länkar tooarticles tillämpa tooeach storage-tjänst och visar ett exempel och förklarar varje element i hello CORS-filen.
* [Microsoft Azure Storage: Introduktion till CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  Det här är en länk toohello inledande bloggartikel presenterar CORS och visar hur toouse den.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Vanliga frågor och svar om Azure Storage-säkerhet
1. **Hur kan jag bekräfta hello integriteten hos hello blobbar som jag överföra till eller från Azure Storage om jag inte använda hello HTTPS-protokollet?**

   Om du av någon anledning som du behöver toouse HTTP i stället för HTTPS och du arbetar med blockblobbar, du kan använda MD5 kontrollerar verifiera toohelp hello integritet hello blob som överförs. Detta hjälper med skydd från nätverket/transport layer fel, men inte nödvändigtvis med mellanliggande attacker.

   Om du kan använda HTTPS, vilket ger säkerhet på radnivå transport är med hjälp av MD5 kontrollerar redundant och onödiga.

   Mer information finns på hello [översikt över Azure Blob-MD5](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **Nyheter om FIPS-kompatibilitet för hello USA Government?**

   hello USA FIPS Federal Information Processing Standard () definierar kryptografiska algoritmer som godkänts för användning av USA Federala myndigheter datorsystem för hello skyddet av känsliga data. Aktivering av FIPS visar-läge på en Windows server eller fjärrskrivbord hello OS endast FIPS-validerade kryptografiska algoritmer som ska användas. Om ett program som använder icke-kompatibla algoritmer, bryts hello program. With.NET Framework version 4.5.2 eller högre, hello programmet automatiskt växlar hello kryptografiska algoritmer toouse FIPS-kompatibla algoritmer när hello dator är i FIPS-läge.

   Microsoft lämnar det upp tooeach kunden toodecide om tooenable FIPS-läge. Vi tror att det finns ingen tvingande anledning för kunder som inte är ämne toogovernment förordningar tooenable FIPS-läge som standard.

   **Resurser**

* [Varför inte rekommenderar vi ”FIPS-läge” längre](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

  Den här bloggen artikeln ger en översikt över FIPS och förklarar varför de inte aktiverar FIPS-läge som standard.
* [FIPS 140 verifiering](https://technet.microsoft.com/library/cc750357.aspx)

  Den här artikeln innehåller information om hur Microsoft-produkter och kryptografimoduler uppfyller hello FIPS-standarden för hello USA Federala myndigheter.
* [”Systemkryptografi: Använd FIPS-kompatibla algoritmer för kryptering, hashing och signering” inställningar effekterna på säkerheten i Windows XP och senare versioner av Windows](https://support.microsoft.com/kb/811833)

  Den här artikeln handlar om hello användning av FIPS-läge i äldre Windows-datorer.
