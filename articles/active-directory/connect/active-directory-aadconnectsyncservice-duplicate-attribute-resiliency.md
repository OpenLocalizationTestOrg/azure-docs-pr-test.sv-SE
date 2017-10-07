---
title: "aaaIdentity synkronisering och dubblett attributet återhämtning | Microsoft Docs"
description: Nya beteende hur toohandle objekt med UPN- eller ProxyAddress konflikter under katalogsynkronisering med Azure AD Connect.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.openlocfilehash: e27dcbf9d71f83fa9566cae2fd99350297d1cd9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identitetssynkronisering och duplicerad attributåterhämtning
Dubbla attribut återhämtning är en funktion i Azure Active Directory som eliminerar friktion som orsakas av **UserPrincipalName** och **ProxyAddress** när du kör någon av Microsofts synkroniseringsverktyg i konflikt.

Dessa två attribut är krävs normalt toobe som är unikt för alla **användare**, **grupp**, eller **Kontakta** objekt i en viss Azure Active Directory-klient.

> [!NOTE]
> Endast användare kan ha UPN-namn.
> 
> 

hello som gör att den här funktionen är i hello molndelen av hello sync pipeline, är det därför klienten storleksoberoende och relevant för varje produkt för synkronisering av Microsoft inklusive Azure AD Connect, DirSync och MIM + Connector. hello allmänna termen ”synkroniseringsklient” används i det här dokumentet toorepresent någon av dessa produkter.

## <a name="current-behavior"></a>Aktuella beteende
Om det finns en försök tooprovision ett nytt objekt med ett UPN eller ProxyAddress värde som bryter mot denna unikhetsbegränsningen, blockerar objektet skapas i Azure Active Directory. På liknande sätt, om ett objekt uppdateras med en icke-unikt UPN- eller ProxyAddress hello uppdateringen misslyckas. hello etablering försök eller uppdatering igen av hello synkroniseringsklient vid varje cykel för export och fortsätter toofail tills hello konflikten har lösts. Ett e-postmeddelande med fel rapporten genereras vid varje försök och ett fel loggas av hello sync-klienten.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Problemet med Duplicerat attribut återhämtning
I stället för att helt misslyckas tooprovision eller uppdatera ett objekt med ett dubblettattribut Azure Active Directory ”sätts i karantän” hello Duplicerat attribut som skulle strider mot unikhetsbegränsningen hello. Om det här attributet är obligatoriskt för etablering som UserPrincipalName, tilldelar hello tjänst ett platshållarvärde som. hello format för dessa tillfälliga värden  
”***<OriginalPrefix>+ < 4DigitNumber > @<InitialTenantDomain>. onmicrosoft.com***”.  
Om hello-attributet inte är obligatoriskt, som en **ProxyAddress**, Azure Active Directory bara placerar i karantän hello konflikt attribut och fortsätter med hello objektet skapa eller uppdatera.

Vid sätta i karantän hello attribut, skickas information om hello konflikt i hello rapport av e-post med samma fel används i hello gamla beteende. Men den här informationen visas bara i hello felrapporten en gång när hello karantän sker det fortsätter inte toobe loggas i framtiden e-postmeddelanden. Dessutom eftersom hello export för det här objektet har slutförts hello synkroniseringsklient logga in inte med ett fel och har inte försök hello skapa / uppdatera igen vid nästa synkroniseringscykler.

toosupport problemet ett nytt attribut har lagts till toohello användare, grupp och kontakta objektklasser:  
**DirSyncProvisioningErrors**

Detta är ett flervärdesattribut som används toostore hello motstridiga attribut som bryter hello unikhetsbegränsningen ska de läggas till normalt. En bakgrundsaktivitet timern har aktiverats i Azure Active Directory som körs varje timme toolook för dubblettattribut konflikter som har lösts och tar automatiskt bort hello attribut i fråga från karantän.

### <a name="enabling-duplicate-attribute-resiliency"></a>Aktiverar återhämtning Duplicerat attribut
Dubbla attribut återhämtning blir hello nya standardfunktionen över alla Azure Active Directory-klienter. Den kommer att som standard för alla klienter som aktiverad synkronisering för hello första gången på 22 augusti 2016 eller senare. Klienter som aktiverad sync tidigare toothis datum har hello-funktionen aktiverad i batchar. Den här distributionen börjar i September 2016 och ett e-postmeddelande skickas tooeach klient tekniskt meddelande kontakt med hello specifika datum när hello funktionen aktiveras.

> [!NOTE]
> När återhämtning Duplicerat attribut har aktiverats kan inte inaktiveras.

toocheck om hello-funktionen är aktiverad för din klient, du kan göra detta genom att hämta hello senaste versionen av hello Azure Active Directory PowerShell-modulen och kör:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Du kan inte längre använda Set-MsolDirSyncFeature cmdlet tooproactively aktivera hello duplicera attribut återhämtning funktionen innan det är aktiverat för din klient. toobe kan tootest hello-funktionen måste toocreate en ny Azure Active Directory-klient.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Identifiera objekt med DirSyncProvisioningErrors
Det finns för närvarande två metoder tooidentify objekt som har de här felen på grund av tooduplicate egenskapen konflikter, Azure Active Directory PowerShell och hello administrationsportalen för Office 365. Det finns planer tooextend tooadditional portal baserat rapportering i hello framtida.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Hello följande är sant för hello PowerShell-cmdlets i det här avsnittet:

* Alla hello följande cmdlets är skiftlägeskänsliga.
* Hej **– ErrorCategory PropertyConflict** måste alltid finnas. Det finns för närvarande inga andra typer av **ErrorCategory**, men detta kan utökas i hello framtida.

Kom igång genom att köra först **Anslut MsolService** och ange autentiseringsuppgifter för en klientadministratör.

Använd sedan hello följande cmdlet: ar och operatörer tooview fel på olika sätt:

1. [Visa alla](#see-all)
2. [Av egenskapstyp](#by-property-type)
3. [Med motstridiga värde](#by-conflicting-value)
4. [Med hjälp av en sträng-sökning](#using-a-string-search)
5. [Sortera](#sorted)
6. [I ett begränsat antal eller alla](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Se alla
När du är ansluten, toosee en allmän lista över attribut etablering fel i hello klient kör:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Detta ger ett resultat som hello följande:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Av egenskapstyp
toosee fel av egenskapstypen, lägga till hello **- PropertyName** flagga hello **UserPrincipalName** eller **ProxyAddresses** argument:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Eller

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Med motstridiga värde
toosee fel relaterade tooa specifik egenskap lägga till hello **- PropertyValue** flagga (**- PropertyName** måste användas samt när du lägger till den här flaggan):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Med hjälp av en sträng-sökning
toodo en bred sträng sökning använder hello **- söksträng** flaggan. Detta kan användas oberoende från alla hello ovan flaggor, med undantag för hello av **- ErrorCategory PropertyConflict**, vilket krävs alltid:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>I ett begränsat antal eller alla
1. **MaxResults <Int>**  kan vara används toolimit hello frågan tooa specifikt antal värden.
2. **Alla** kan vara används tooensure alla resultat hämtas i hello fall som ett stort antal fel finns.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Administrationsportalen för Office 365
Du kan visa directory synkroniseringsfel i hello administrationscentret för Office 365. Hej rapporten i hello Office 365-portalen bara visar **användaren** objekt som har dessa fel. Visar inte information om konflikter mellan **grupper** och **kontakter**.

![Aktiva användare](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "aktiva användare")

Anvisningar för hur tooview directory synkroniseringsfel i hello Office 365 admin center finns [identifiera directory synkroniseringsfel i Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Identitet felrapport för synkronisering
När ett objekt med en dubblettattribut konflikt hanteras med den här nya funktionen ett meddelande som ingår i skickas hello standard identitet felrapport för synkronisering av e-post som toohello tekniskt meddelande kontakt för hello-klient. Det finns dock en viktig förändring i det här beteendet. I tidigare hello, skulle information om en konflikt med Duplicerat attribut tas med i varje efterföljande felrapporten tills hello konflikten har lösts. Med den här nya funktionen hello felmeddelanden för en given konflikt visas endast en gång - tidpunkt hello hello motstridiga attributet har satts i karantän.

Här är ett exempel på vilka hello e-postmeddelande som ser ut som ProxyAddress konflikter:  
    ![Aktiva användare](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "aktiva användare")  

## <a name="resolving-conflicts"></a>Lösa konflikter
Felsöka strategi och upplösning medvetande för dessa fel bör inte skilja sig från hello hanteringen av fel med Duplicerat attribut har i hello tidigare. hello enda skillnaden är att hello timer uppgiften Svep via hello på hello tjänsten på klientsidan tooautomatically lägger till hello-attributet i frågan toohello rätt objekt när hello konflikten har lösts.

hello följande artikel beskrivs olika strategier för felsökning och upplösning: [dubblett eller ogiltigt attribut förhindra katalogsynkronisering i Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Kända problem
Ingen av de här problemen gör att data går förlorade eller tjänst försämras. Flera av dem är tycke, andra orsaka standard ”*före återhämtning*” dubblettattribut fel toobe utlöstes i stället för att sätta i karantän hello konflikt attribut, och en annan gör vissa fel toorequire extra Manuell korrigering av.

**Core beteende:**

1. Objekt med ett specifikt attribut konfigurationer Fortsätt tooreceive exportfel som skillnad från toohello dubbla attribut i karantän.  
   Exempel:
   
    a. Ny användare skapas i AD med UPN för  **Joe@contoso.com**  och ProxyAddress**smtp:Joe@contoso.com**
   
    b. hello egenskaperna för det här objektet är i konflikt med en befintlig grupp, där ProxyAddress är  **SMTP:Joe@contoso.com** .
   
    c. Vid export en **ProxyAddress konflikt** felmeddelande i stället för att hello konflikt attribut i karantän. hello åtgärden försöks vid varje efterföljande synkronisering cykel som skulle ha varit innan hello återhämtning funktionen har aktiverats.
2. Om två grupper skapas lokalt med hello samma SMTP-adress, ett misslyckas tooprovision på hello första försöket med en standard som är duplicerade **ProxyAddress** fel. Dock hello Dubblettvärdet är korrekt i karantän på hello nästa synkronisering cykel.

**Office-portalen rapporten**:

1. hello detaljerat felmeddelande för två objekt i en konflikt UPN-mängd är hello samma. Detta anger att de har både haft deras UPN ändras / sätts i karantän, när i själva verket bara en av dem har alla data som har ändrats.
2. hello detaljerat felmeddelande för en UPN-konflikt visar hello fel displayName för en användare som har haft deras UPN ändras/i karantän. Exempel:
   
    a. **Användare A** synkroniseras först med **UPN = User@contoso.com** .
   
    b. **Användare B** är försök toobe synkroniserad nästa med **UPN = User@contoso.com** .
   
    c. **Användare B** UPN ändras för **User1234@contoso.onmicrosoft.com**  och  **User@contoso.com**  har lagts till för**DirSyncProvisioningErrors**.
   
    d. hello felmeddelandet **användare B** ska visa att **användare A** har redan  **User@contoso.com**  som ett UPN, men den visar **användare B** egna displayName.

**Identitet synkronisering felrapporten**:

hello länk för *instruktioner för hur tooresolve problemet* är felaktig:  
    ![Aktiva användare](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "aktiva användare")  

Den ska peka för[https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Se även
* [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
* [Identifiera directory synkroniseringsfel i Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

