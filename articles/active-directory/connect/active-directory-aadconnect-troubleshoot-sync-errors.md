---
title: "Azure AD Connect: Felsökning av fel vid synkronisering | Microsoft Docs"
description: "Förklarar hur tootroubleshoot fel uppstod vid synkronisering med Azure AD Connect."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: af262dfe95d686e34697454c0dfe8b4a6d8693c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-errors-during-synchronization"></a>Felsökning av fel under synkroniseringen
Fel kan inträffa när identitetsdata synkroniseras från Windows Server Active Directory (AD DS) tooAzure Active Directory (AD Azure). Den här artikeln innehåller en översikt över olika typer av synkroniseringsfel hello möjliga scenarier som orsakar dessa fel och potentiella sätt toofix hello fel. Den här artikeln innehåller hello vanliga feltyper och kan inte omfatta alla hello möjliga fel.

 Den här artikeln förutsätter hello läsaren är bekant med hello underliggande [designbegreppen av Azure AD och Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Med hello senaste versionen av Azure AD Connect \(augusti 2016 eller senare\), en rapport över synkroniseringsfel finns i hello [Azure Portal](https://aka.ms/aadconnecthealth) som en del av Azure AD Connect Health för synkronisering.

Från den 1 September 2016 [Azure Active Directory duplicera attribut återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funktionen aktiveras som standard för alla hello *nya* Azure Active Directory-klienter. Den här funktionen ska aktiveras automatiskt för befintliga klienter hello kommande månaderna.

Azure AD Connect utför 3 typer av åtgärder från hello kataloger den bevarar synkroniserade: Import, synkronisering och Export. Fel kan ske i alla hello-åtgärder. Den här artikeln fokuserar huvudsakligen på fel under Export tooAzure AD.

## <a name="errors-during-export-tooazure-ad"></a>Fel under Export tooAzure AD
Följande avsnitt beskrivs olika typer av synkronisering hello fel som kan uppstå under hello export åtgärden tooAzure AD med hjälp av Azure AD-koppling. Den här anslutningen kan identifieras av hello namnformat som ”contoso. *onmicrosoft.com*”.
Fel vid Export tooAzure AD visar hello åtgärden \(lägga till, uppdatera, ta bort etc.\) gjordes ett försök av Azure AD Connect \(Synkroniseringsmotorn\) på Azure Active Directory misslyckades.

![Översikt över fel](./media/active-directory-aadconnect-troubleshoot-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Felaktig matchning av datafel
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beskrivning
* När Azure AD Connect \(Synkroniseringsmotorn\) instruerar Azure Active Directory-objekt för tooadd eller uppdatering, Azure AD matchar hello inkommande objekt med hello **sourceAnchor** attributet toohello  **immutableId** attribut för objekt i Azure AD. Matchningen kallas en **hårda matchar**.
* När Azure AD **inte hittar** alla objekt som matchar hello **immutableId** attribut med hello **sourceAnchor** attribut för hello inkommande objekt innan du etablerar en nya objekt faller tillbaka toouse hello ProxyAddresses och UserPrincipalName attribut toofind en matchning. Matchningen kallas en **mjuka matchar**. hello mjuka matchning är utformad toomatch objekt finns redan i Azure AD (som tillhandahålls i Azure AD) med hello nya-objekt som lagts till/uppdateras under synkroniseringen som representerar hello samma entitet (användare, grupper) lokalt.
* **InvalidSoftMatch** fel uppstår när hello hårda matchar inte hitta alla matchande objekt **och** mjuka matchar hittar en matchande objekt men objektet har ett annat värde för *immutableId* än hello inkommande objektet *SourceAnchor*, tyder på att hello matchande objekt synkroniserades med ett annat objekt från lokala Active Directory.

Med andra ord för hello mjuka matchar toowork hello objektet toobe mjuk matchas med ska inte ha ett värde för hello *immutableId*. Om något objekt med *immutableId* med ett värde misslyckas hello hårda matchar men uppfyller hello soft-matchningsvillkor, hello åtgärden skulle leda till en InvalidSoftMatch synkroniseringsfel.

Azure Active Directory som schemat inte tillåter två eller flera objekt toohave hello samma värde hello följande attribut. \(Detta är inte en fullständig förteckning.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* objekt-ID

> [!NOTE]
> [Azure AD-attributet duplicera attribut återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funktion är också på att släppas som hello standardbeteendet för Azure Active Directory.  Detta minskar hello antal synkroniseringsfel ses av Azure AD Connect (samt andra sync-klienter) genom att Azure AD mer robust i hello hanteringen av den duplicerade ProxyAddresses och UserPrincipalName attribut finns i lokala AD miljöer. Den här funktionen löser inte hello dubblettfel. Hello data måste därför fortfarande toobe fast. Men det gör etablering av nya objekt som annars blockeras från tillhandahålls på grund av tooduplicated värden i Azure AD. Detta minskar även hello antal synkroniseringsfel returnerade toohello synkronisering klienten.
> Hej InvalidSoftMatch synkroniseringsfel visas vid etablering av nya objekt visas inte om den här funktionen är aktiverad för din klient.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>Exempelscenarier för InvalidSoftMatch
1. Två eller flera objekt med samma värde för attributet ProxyAddresses finns i lokala Active Directory hello. Endast en etableras om i Azure AD.
2. Två eller flera objekt med samma värde userPrincipalName finns i lokala Active Directory hello. Endast en etableras om i Azure AD.
3. Ett objekt har lagts till i hello lokala Active Directory med hello samma värde för ProxyAddresses attribut med ett befintligt objekt i Azure Active Directory. hello-objektet som läggs till på lokala komma inte etableras i Azure Active Directory.
4. Ett objekt har lagts till i lokala Active Directory med hello samma värde för attributet userPrincipalName som ett konto i Azure Active Directory. hello objektet komma inte etableras i Azure Active Directory.
5. Synkroniserade konto har flyttats från skog A tooForest B. Azure AD Connect (Synkroniseringsmotorn) med ObjectGUID attributet toocompute hello SourceAnchor. Efter hello skog flytta är hello värdet för hello SourceAnchor olika. hello nytt objekt (från skog B) misslyckas toosync med hello befintligt objekt i Azure AD.
6. Synkroniserade objekt har tagits bort av misstag från lokala Active Directory och ett nytt objekt har skapats i Active Directory för hello samma entitet (till exempel användare) utan att ta bort hello konto i Azure Active Directory. hello nytt konto misslyckas toosync med hello befintliga Azure AD-objekt.
7. Azure AD Connect har avinstalleras och installeras igen. Ett annat attribut valdes under installationen av hello igen som hello SourceAnchor. Alla hello-objekt som tidigare har synkroniserats stoppats synkroniserar med InvalidSoftMatch fel.

#### <a name="example-case"></a>Exempel fall:
1. **Bob Smith** är en synkroniserade användare i Azure Active Directory från på lokala Active Directory för *contoso.com*
2. Bob Smith **UserPrincipalName** har angetts som  **bobs@contoso.com** .
3. **”abcdefghijklmnopqrstuv ==”** är hello **SourceAnchor** beräknas av Azure AD Connect med Bob Smith **objectGUID** från lokala Active Directory, vilket är hello **immutableId** för Bob Smith i Azure Active Directory.
4. Bob innehåller också följande värden för hello **proxyAddresses** attribut:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
5. En ny användare **Bob Taylor**, läggs toohello lokala Active Directory.
6. Bob Taylor **UserPrincipalName** har angetts som  **bobt@contoso.com** .
7. **”abcdefghijkl0123456789 ==” ”** är hello **sourceAnchor** beräknas av Azure AD Connect med Bob Taylor **objectGUID** från på lokala Active Directory. Bob Taylor objektet har inte synkroniserats tooAzure Active Directory ännu.
8. Bob Taylor har följande värden för hello proxyAddresses attributet hello
   * smtp:bobt@contoso.com
   * smtp:bob.taylor@contoso.com
   * **smtp:bob@contoso.com**
9. Under synkroniseringen, Azure AD Connect identifierar hello lägga till Bob Taylor i lokala Active Directory och be Azure AD toomake hello samma ändringar.
10. Azure AD först utför hårda matchning. Det vill säga söker om det inte finns något objekt med hello immutableId lika för ”abcdefghijkl0123456789 ==”. Hårda matchar misslyckas eftersom inga objekt i Azure AD har den immutableId.
11. Azure AD görs försök toosoft matchar Bob Taylor. Det vill säga söker om det inte finns något objekt med proxyAddresses lika toohello tre värden, inklusivesmtp:bob@contoso.com
12. Azure AD hittar Bob Smith objektet toomatch hello soft-matcha villkor. Men det här objektet har hello värdet för immutableId = ”abcdefghijklmnopqrstuv ==”. Anger det här objektet har synkroniserats från ett annat objekt från lokala Active Directory. Därför Azure AD kan inte soft-matcha de här objekten och resulterar i en **InvalidSoftMatch** synkroniseringsfel.

#### <a name="how-toofix-invalidsoftmatch-error"></a>Hur toofix InvalidSoftMatch fel
hello vanligaste anledningen till hello InvalidSoftMatch fel är två objekt med olika SourceAnchor \(immutableId\) har samma värde för hello ProxyAddresses och/eller UserPrincipalName attribut som används under hello hello Soft-matcha process på Azure AD. I ordning toofix hello ogiltig mjuka matchning

1. Identifiera hello dupliceras proxyAddresses, userPrincipalName eller andra attribut-värde som orsakar hello-fel. Också identifiera vilka två \(eller flera\) objekt ingår i hello konflikt. Hej rapporten som genereras av [Azure AD Connect Health för synkronisering](https://aka.ms/aadchsyncerrors) kan hjälpa dig att identifiera hello två objekt.
2. Identifiera vilka objekttyper som ska fortsätta toohave hello dupliceras värdet och vilket objekt bör inte.
3. Ta bort dubbletter hello värdet från hello-objekt som inte ska ha värdet. Observera att du bör kontrollera hello ändra i hello katalogen där hello objektet kommer från. I vissa fall kan behöva du toodelete ett hello objekt i konflikt.
4. Om du har gjort hello ändras i hello på lokala AD kan Azure AD Connect sync hello ändra.

Observera att synkronisera felrapport i Azure AD Connect Health för synkronisering uppdateras var 30: e minut och innehåller hello fel från hello senaste synkroniseringsförsöket.

> [!NOTE]
> ImmutableId, bör per definition inte ändra i hello livstid hello-objektet. Om Azure AD Connect inte har konfigurerats med hello scenarier i åtanke från hello ovanför listan, du kan hamna i en situation där Azure AD Connect beräknar ett annat värde för hello SourceAnchor för hello AD-objekt som representerar hello samma entitet (samma användare / gruppen/kontakt osv.) som har en befintlig Azure AD-objekt du vill använda toocontinue.
>
>

#### <a name="related-articles"></a>Relaterade artiklar
* [Dubblett eller ogiltigt attribut förhindra katalogsynkronisering i Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beskrivning
Azure AD försöker toosoft matchar två objekt, är det möjligt att två objekt i olika ”objekttypen” (till exempel användare, grupp, kontakta etc.) har hello samma värdena för hello-attribut används tooperform hello mjuka matchning. Som duplicering av dessa attribut inte är tillåten i Azure AD, kan hello åtgärden resultera i ”ObjectTypeMismatch” synkroniseringsfel.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Exempelscenarier för ObjectTypeMismatch fel
* En e-aktiverade säkerhetsgruppen har skapats i Office 365. Administratören lägger till en ny användare eller kontakta i lokala AD (som inte har synkroniserats ännu tooAzure AD) med samma värde för hello ProxyAddresses attribut som hello Office 365 hello gruppen.

#### <a name="example-case"></a>Exempel fallet
1. Administratören skapar en ny e-post aktiverad säkerhetsgrupp i Office 365 för hello skatt avdelning och ger en e-postadress som tax@contoso.com. Den här tilldelas hello ProxyAddresses attribut för den här gruppen med hello värdet**smtp:tax@contoso.com**
2. En ny användare ansluter Contoso.com och ett konto skapas för hello användaren lokalt med hello proxyAddress som**smtp:tax@contoso.com**
3. När Azure AD Connect synkroniserar hello nytt användarkonto, får den hello ”ObjectTypeMismatch” fel.

#### <a name="how-toofix-objecttypemismatch-error"></a>Hur toofix ObjectTypeMismatch fel
hello vanligaste anledningen till hello ObjectTypeMismatch fel är två objekt av en annan typ (användare, grupp, kontakta etc.) har samma värde för hello ProxyAddresses attributet hello. I ordning toofix hello ObjectTypeMismatch:

1. Identifiera hello dupliceras proxyAddresses (eller andra attribut) värdet som orsakar hello-fel. Också identifiera vilka två \(eller flera\) objekt ingår i hello konflikt. Hej rapporten som genereras av [Azure AD Connect Health för synkronisering](https://aka.ms/aadchsyncerrors) kan hjälpa dig att identifiera hello två objekt.
2. Identifiera vilka objekttyper som ska fortsätta toohave hello dupliceras värdet och vilket objekt bör inte.
3. Ta bort dubbletter hello värdet från hello-objekt som inte ska ha värdet. Observera att du bör kontrollera hello ändra i hello katalogen där hello objektet kommer från. I vissa fall kan behöva du toodelete ett hello objekt i konflikt.
4. Om du har gjort hello ändras i hello på lokala AD kan Azure AD Connect sync hello ändra. Synkronisera felrapport i Azure AD Connect Health för synkronisering uppdateras var 30: e minut och innehåller hello fel från hello senaste synkroniseringsförsöket.

## <a name="duplicate-attributes"></a>Duplicera attribut
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beskrivning
Azure Active Directory som schemat inte tillåter två eller flera objekt toohave hello samma värde hello följande attribut. Det är varje objekt i Azure AD är framtvingad toohave ett unikt värde för attributen på en viss instans.

* ProxyAddresses
* UserPrincipalName

Om Azure AD Connect försöker tooadd ett nytt objekt eller uppdaterar ett befintligt objekt med ett värde för hello ovan attribut som redan har tilldelats tooanother objektet i Azure Active Directory, resulterar hello åtgärden i hello ”AttributeValueMustBeUnique” sync-fel.

#### <a name="possible-scenarios"></a>Möjliga scenarier:
1. Dubblettvärdet är tilldelade tooan redan synkroniserade objekt som står i konflikt med ett annat synkroniserade objekt.

#### <a name="example-case"></a>Exempel fall:
1. **Bob Smith** är en synkroniserade användare i Azure Active Directory från på lokala Active Directory för contoso.com
2. Bob Smith **UserPrincipalName** har angetts som lokala  **bobs@contoso.com** .
3. Bob innehåller också följande värden för hello **proxyAddresses** attribut:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
4. En ny användare **Bob Taylor**, läggs toohello lokala Active Directory.
5. Bob Taylor **UserPrincipalName** har angetts som  **bobt@contoso.com** .
6. **Bob Taylor** har följande värden för hello hello **ProxyAddresses** attribut i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylor objektet synkroniseras med Azure AD har.
8. Admin valt tooupdate Bob Taylor **ProxyAddresses** hello följande värde för attributet: jag. **smtp:bob@contoso.com**
9. Azure AD försöker tooupdate Bob Taylor objektet i Azure AD med hello över värde, men att misslyckas åtgärden som att ProxyAddresses värdet har redan tilldelats tooBob Smith, vilket resulterar i ”AttributeValueMustBeUnique”-fel.

#### <a name="how-toofix-attributevaluemustbeunique-error"></a>Hur toofix AttributeValueMustBeUnique fel
hello vanligaste anledningen till hello AttributeValueMustBeUnique fel är två objekt med olika SourceAnchor \(immutableId\) har hello samma värde för hello ProxyAddresses och/eller UserPrincipalName attribut. I ordning toofix AttributeValueMustBeUnique fel

1. Identifiera hello dupliceras proxyAddresses, userPrincipalName eller andra attribut-värde som orsakar hello-fel. Också identifiera vilka två \(eller flera\) objekt ingår i hello konflikt. Hej rapporten som genereras av [Azure AD Connect Health för synkronisering](https://aka.ms/aadchsyncerrors) kan hjälpa dig att identifiera hello två objekt.
2. Identifiera vilka objekttyper som ska fortsätta toohave hello dupliceras värdet och vilket objekt bör inte.
3. Ta bort dubbletter hello värdet från hello-objekt som inte ska ha värdet. Observera att du bör kontrollera hello ändra i hello katalogen där hello objektet kommer från. I vissa fall kan behöva du toodelete ett hello objekt i konflikt.
4. Om du har gjort hello ändras i hello på lokala AD kan Azure AD Connect sync hello ändra för hello fel tooget fast.

#### <a name="related-articles"></a>Relaterade artiklar
-[Dubblett eller ogiltigt attribut förhindra katalogsynkronisering i Office 365](https://support.microsoft.com/en-us/kb/2647098)

## <a name="data-validation-failures"></a>Dataverifiering
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beskrivning
Azure Active Directory tillämpar olika begränsningar för hello själva informationen innan du tillåter att toobe för data som skrivs till hello directory. Detta är tooensure som slutanvändare får hello bästa möjliga upplevelse när du använder hello-program som är beroende av dessa data.

#### <a name="scenarios"></a>Scenarier
a. hello UserPrincipalName attributvärdet har tecken som inte ogiltig stöds.
b. attributet UserPrincipalName för hello följer inte hello format som krävs.

#### <a name="how-toofix-identitydatavalidationfailed-error"></a>Hur toofix IdentityDataValidationFailed fel
a. Se till att attributet userPrincipalName hello har stöds tecken och format som krävs.

#### <a name="related-articles"></a>Relaterade artiklar
* [Förbereda tooprovision användare via directory synkronisering tooOffice 365](https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Beskrivning
Detta är ett specialfall som resulterar i en **”FederatedDomainChangeError”** synkroniseringsfel när hello suffix för en användares UserPrincipalName ändras från en federerad domän tooanother federerad domän.

#### <a name="scenarios"></a>Scenarier
För en synkroniserad användare ändrades hello UserPrincipalName suffix från en federerad domän tooanother federerade domänen lokalt. Till exempel *UserPrincipalName = bob@contoso.com*  har ändrats för*UserPrincipalName = bob@fabrikam.com* .

#### <a name="example"></a>Exempel
1. Bob Smith, ett konto för Contoso.com, hämtar läggas till som en ny användare i Active Directory med hello UserPrincipalNamebob@contoso.com
2. Bob flyttar tooa olika division contoso.com kallas Fabrikam.com och sin UserPrincipalName ändrastoobob@fabrikam.com
3. Både contoso.com och fabrikam.com domäner är federerade med Azure Active Directory-domäner.
4. Bobs userPrincipalName uppdateras inte och resulterar i ett ”FederatedDomainChangeError” synkroniseringsfel.

#### <a name="how-toofix"></a>Hur toofix
Om en användares UserPrincipalName suffix uppdaterades från bob @**contoso.com** toobob @**fabrikam.com**, där både **contoso.com** och **fabrikam.com** är **federerad domäner**, Följ dessa steg toofix hello sync-fel

1. Uppdatera hello användares UserPrincipalName i Azure AD från bob@contoso.com toobob@contoso.onmicrosoft.com. Du kan använda följande PowerShell-kommando med hello Azure AD PowerShell-modulen hello:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Tillåt hello nästa synkronisering cykel tooattempt synkronisering. Den här tidssynkronisering kommer att lyckas och uppdaterar den hello UserPrincipalName Bob toobob@fabrikam.com som förväntat.

#### <a name="related-articles"></a>Relaterade artiklar
* [Ändringarna synkroniserats inte med hello Azure Active Directory Sync tool när du har ändrat hello UPN-namnet för en användare konto toouse en annan federerad domän](https://support.microsoft.com/en-us/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beskrivning
När ett attribut överskrider hello tillåtna storleksgränsen, längdbegränsningen eller antal gränsvärdet i Azure Active Directory-schemat, hello synkroniseringsåtgärden resulterar i hello **LargeObject** eller **ExceededAllowedLength** synkroniseringsfel. Det här felet uppstår vanligen för hello följande attribut

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>Möjliga scenarier
1. För många certifikat som har tilldelats tooBob lagrar bObs userCertificate attribut. Dessa kan innehålla äldre, utgångna certifikat. hello hård gräns är 15 certifikat. Mer information om hur toohandle LargeObject fel med userCertificate attributet du referera tooarticle [hantering LargeObject fel som orsakats av userCertificate attributet](active-directory-aadconnectsync-largeobjecterror-usercertificate.md).
2. För många certifikat som har tilldelats tooBob lagrar bObs userSMIMECertificate attribut. Dessa kan innehålla äldre, utgångna certifikat. hello hård gräns är 15 certifikat.
3. Bobs thumbnailPhoto i Active Directory är för stor toobe synkroniseras i Azure AD.
4. Ett objekt har för många ProxyAddresses som tilldelas under automatisk ifyllning av hello ProxyAddresses attribut i Active Directory.

### <a name="how-toofix"></a>Hur toofix
1. Kontrollera hello attributet orsakar fel hello ligger inom hello tillåts begränsning.

## <a name="related-links"></a>Relaterade länkar
* [Hitta Active Directory-objekt i Active Directory Administrationscenter](https://technet.microsoft.com/library/dd560661.aspx)
* [Hur tooquery Azure Active Directory för ett objekt med Azure Active Directory PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
