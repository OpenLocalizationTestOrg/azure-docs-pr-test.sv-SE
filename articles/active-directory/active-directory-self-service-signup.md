---
title: "aaaWhat är självbetjäningsregistrering för Azure? | Microsoft Docs"
description: "En översikt över självbetjäningsregistrering för Azure, hur toomanage hello registreringsprocessen och hur tootake över ett DNS-domännamn."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: dbf3b59e3807e98f7bf39f3d5591fcde01667323
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Vad är självbetjäningsregistrering för Azure?
Det här avsnittet beskriver hello självbetjäning registreringsprocessen och hur tootake över ett DNS-domännamn.  

## <a name="why-use-self-service-signup"></a>Varför använda självbetjäningsregistrering?
* Hämta kunder tooservices de vill snabbare.
* Skapa e-postbaserad erbjudanden för en tjänst.
* Skapa e-postbaserad signup flöden där snabbt användare toocreate identiteter med hjälp av deras arbete lätt att komma ihåg e-alias.
* Ohanterad Azure kataloger kan göras i hanterade kataloger senare och återanvändas för andra tjänster.

## <a name="terms-and-definitions"></a>Termer och definitioner
* **Självbetjäningsregistreringen**: Detta är hello-metod som en användare som registrerar sig för en tjänst i molnet och har en identitet skapas automatiskt för dem i Azure Active Directory (AD Azure) baserat på deras e-postdomän.
* **Ohanterad Azure directory**: det här är hello katalog där identitet skapas. En ohanterad katalog är en katalog som har ingen global administratör.
* **E-post verifierade användaren**: Detta är en typ av konto i Azure AD. En användare som har en identitet skapas automatiskt när du registrerar dig för ett erbjudande för självbetjäning kallas en e-post verifierade användare. Ett e-post verifierade användaren är medlem reguljära i en katalog som är märkta med creationmethod = EmailVerified.

## <a name="user-experience"></a>Användarens upplevelse
Anta exempelvis att en användare vars e-post är Dan@BellowsCollege.com tar emot känsliga filer via e-post. hello-filer har skyddats av Azure Rights Management (Azure RMS). Men Lisas organisation, bälgar universitet har inte registrerat dig för Azure RMS eller har distribuerat Active Directory RMS. I det här fallet kan Dan registrera dig för en kostnadsfri prenumeration tooRMS för enskilda användare i ordning tooread hello skyddade filer.

Om Dan hello första användare med en e-postadress från BellowsCollege.com toosign för den här självbetjäning erbjudande, kommer sedan en ohanterad katalog att skapas för BellowsCollege.com i Azure AD. Om andra användare från hello BellowsCollege.com domän registrerar dig för detta erbjudande eller ett liknande erbjudande för självbetjäning, måste de även e-post verifierade användarkonton som skapats i hello samma ohanterad katalog i Azure.

## <a name="admin-experience"></a>Administratörsupplevelse
En administratör som äger hello DNS-domännamnet för en ohanterad Azure-katalog kan ta över eller sammanfoga hello directory när bevisar ägarskap. hello nästa avsnitt beskrivs hello-administratörens miljö i detalj, men här är en sammanfattning:

* När du tar över en ohanterad Azure directory blir du helt enkelt hello global administratör för hello ohanterade katalog. Detta kallas ibland ett internt övertag.
* När du kopplar en ohanterad Azure-katalog, du lägger till hello DNS-domännamnet för hello ohanterade directory tooyour hanterad Azure katalog och en mappning av användare till resurser skapas så användare kan fortsätta tooaccess tjänster utan avbrott. Detta kallas ibland ett externt övertag.

## <a name="what-gets-created-in-azure-active-directory"></a>Vad skapas i Azure Active Directory?
#### <a name="directory"></a>Directory
* En Azure Active Directory-katalog för hello domänen skapas en katalog per domän.
* hello Azure AD-katalog har ingen global administratör.

#### <a name="users"></a>Användare
* För varje användare som loggar in, skapas ett användarobjekt i hello Azure AD-katalog.
* Varje användarobjekt har markerats som extern.
* Varje användare får åtkomst toohello tjänst som de har registrerat sig för.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Hur anspråksregelmallar en Azure AD med självbetjäning katalog för en domän som jag äger?
Du kan begära en Azure AD med självbetjäning katalogen genom att utföra verifiering av domän. Verifiering av domän bevisar egna hello domänen genom att skapa DNS-poster.

Det finns två sätt toodo en DNS-övertag av Azure AD-katalog:

* internt övertag (Admin identifierar en ohanterad Azure-katalog och vill tooturn i en katalog som hanterad)
* externa övertag (Admin försöker tooadd en ny domän tootheir hanterade Azure katalog)

Du kan vara intresserad av att verifiera att du äger en domän eftersom du tar över en ohanterad katalog när en användare har genomfört självbetjäningsregistrering eller du kanske att lägga till en ny domän tooan befintliga hanterade katalog. Till exempel du har en domän med namnet contoso.com och du vill tooadd en ny domän med namnet contoso.co.uk eller contoso.uk.

## <a name="what-is-domain-takeover"></a>Vad är domänen övertag?
Det här avsnittet beskriver hur toovalidate som du äger en domän

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Vad är verifiering av domän och varför används den?
I ordning tooperform åtgärder på en katalog kräver Azure AD att du verifierar ägarskap hello DNS-domän.  Validering av hello domän kan du tooclaim hello directory och antingen befordra hello självbetjäning directory tooa hanterad katalog eller merge hello självbetjäning directory till en befintlig hanteras directory.

## <a name="examples-of-domain-validation"></a>Exempel på verifiering av domän
Det finns två sätt toodo en DNS-övertag i en katalog:

* internt övertag (t.ex, en administratör identifierar en självbetjäning, ohanterad katalog och vill tooturn i hanterad katalog)
* externa övertag (t.ex, en administratör försöker tooadd en ny domän tooa hanterade katalog)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-toobe-a-managed-directory"></a>Internt övertag - befordra en självbetjäning, ohanterad directory toobe hanterad katalog
När du gör interna övertag konverteras hello directory från en ohanterad directory tooa hanterad katalog. Du måste toocomplete DNS-domän valideringen, där du skapar en MX-post eller en TXT-post i hello DNS-zon. Åtgärden:

* Verifierar att du äger hello domän
* Gör hello directory hanteras
* Gör du hello global administratör för hello katalog

Anta att en IT-administratör bälgar kollega upptäcker att användare från hello skolan har registrerat sig för erbjudanden för självbetjäning. Hello registrerade ägaren av hello DNS-namnet BellowsCollege.com, hello IT-administratören kan verifiera ägarskapet för hello DNS-namnet i Azure och sedan ta över ohanterade hello-katalogen. hello directory blir en hanterad katalog och hello IT-administratören har tilldelats hello rollen som global administratör för hello BellowsCollege.com katalogen.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externa övertag - koppla en självbetjäning katalog till en befintlig hanterad katalog
I en extern övertag du har redan en hanterad katalog och du vill att alla användare och grupper från en ohanterad directory toojoin som hanteras katalog i stället egna två separata kataloger.

Du lägger till en domän som administratör för en hanterad katalog och domänen händer toohave en ohanterad katalog som hör till den.

Till exempel anta att du är IT-administratör och du redan har en hanterad katalog för Contoso.com, ett domännamn som är registrerade tooyour organisation. Du upptäcker att användare från din organisation har utfört självbetjäning logga in för ett erbjudande via e-domännamn user@contoso.co.uk, vilket är ett annat domännamn som din organisation äger. Dessa användare har för närvarande konton i en ohanterad för contoso.co.uk.

Vill du inte toomanage två separata kataloger, så du slå samman hello ohanterade katalogen för contoso.co.uk till din befintliga IT-hanterad katalog för contoso.com.

Externa gäller följande hello samma DNS-valideringen som interna övertag.  Skillnad vara: användare och tjänster är ommappad toohello IT hanterad katalog.

#### <a name="whats-hello-impact-of-performing-an-external-takeover"></a>Vad är hello effekten av den en extern övertag?
Med en extern övertag skapas en mappning av användare till resurser så att användarna kan fortsätta tooaccess tjänster utan avbrott. Många program, inklusive RMS för enskilda användare, hanterar hello mappningen av användare till resurser och och användare kan fortsätta tooaccess dessa tjänster utan att ändra. Om ett program inte hanterar hello mappningen av användare till resurser effektivt, vara externa övertag explicit blockerade tooprevent användare från en dålig upplevelse.

#### <a name="directory-takeover-support-by-service"></a>Directory gäller stöd av tjänsten
Hello följande tjänster för närvarande stöd för övertag:

* RMS

hello följande tjänster kommer snart att stödja övertag:

* PowerBI

hello följande inte och kräver ytterligare admin åtgärd toomigrate användardata efter en extern övertag.

* SharePoint/OneDrive

## <a name="how-tooperform-a-dns-domain-name-takeover"></a>Hur tooperform en DNS-domän namn övertag
Du har några alternativ för hur tooperform verifiering av domän (och göra en övertag om du vill):

1. Hanteringsportalen för Azure

   En övertag utlöses genom att göra ett tillägg för domänen.  Om det finns redan en katalog för hello domän, har du hello alternativet tooperform en extern övertag.

   Logga in toohello Azure-portalen med dina autentiseringsuppgifter.  Navigera tooyour befintlig katalog och sedan för**Lägg till domän**.
2. Office 365

   Du kan använda hello alternativ på hello [hantera domäner](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) sida i Office 365 toowork med domäner och DNS-poster. Se [verifiera din domän i Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   hello följande steg är nödvändiga tooperform en verifiering med Windows PowerShell.

   | Steg | Cmdlet toouse |
   | --- | --- |
   | Skapa ett autentiseringsuppgiftobjekt |Get-Credential |
   | Ansluta tooAzure AD |Connect-MsolService |
   | Hämta en lista över domäner |Get-MsolDomain |
   | Skapa en utmaning |Get-MsolDomainVerificationDns |
   | Skapa DNS-post |Gör detta på DNS-servern |
   | Kontrollera hello utmaning |Confirm-MsolEmailVerifiedDomain |

Exempel:

1. Ansluta tooAzure AD med hello-autentiseringsuppgifter som har använt toorespond toohello självbetjäning erbjudande:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Hämta en lista över domäner:

    Get-MsolDomain
3. Kör hello Get-MsolDomainVerificationDns cmdlet toocreate en utmaning:

    Get-MsolDomainVerificationDns – DomainName *your_domain_name* – läge DnsTxtRecord

    Exempel:

    Get-MsolDomainVerificationDns – DomainName contoso.com – läge DnsTxtRecord
4. Kopiera hello-värdet (hello challenge) som returneras från det här kommandot.

    Exempel:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9
5. Skapa en DNS txt-post som innehåller hello-värdet som du kopierade i föregående steg i hello i det offentliga DNS-namnområdet.

    hello namnet för den här posten är Hej hello överordnade domänens namn, så om du skapar denna resurspost med hello DNS-serverrollen från Windows Server inget hello postens namn och klistra in bara hello värdet i textrutan för hello
6. Kör hello bekräfta MsolDomain cmdlet tooverify hello challenge:

    Bekräfta MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Exempel:

    Bekräfta MsolEmailVerifiedDomain - DomainName contoso.com

En lyckad utmaning returnerar toohello prompt utan fel.

## <a name="how-do-i-control-self-service-settings"></a>Hur kan jag styra självbetjäning inställningar?
Administratörer har två självbetjäning kontroller idag. De kan styra:

* Huruvida användare kan ansluta till hello directory via e-post.
* Huruvida användare kan licensiera själva för program och tjänster.

### <a name="how-can-i-control-these-capabilities"></a>Hur kan du styra dessa funktioner?
En administratör kan konfigurera de här funktionerna med dessa Azure AD-cmdlet Set-MsolCompanySettings-parametrar:

* **AllowEmailVerifiedUsers** styr om en användare kan skapa eller Anslut till en ohanterad katalog. Om du anger parametern för$ false, inte e-post verifieras användare kan ansluta till hello directory.
* **AllowAdHocSubscriptions** styr hello möjligheten för användare tooperform självbetjäning registrering. Om du anger parametern för$ false, inga användare kan utföra självbetjäningsregistrering.

### <a name="how-do-hello-controls-work-together"></a>Hur fungerar hello kontroller tillsammans?
Dessa två parametrar kan användas tillsammans toodefine mer exakt kontroll över självbetjäning logga. Till exempel hello följande kommando Tillåt användare tooperform självbetjäningsregistreringen, men endast om de användarna som redan har ett konto i Azure AD (användare som behöver en verifierad e-post konto toobe skapade kan med andra ord utför självbetjäning logga in):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

hello förklarar följande flödesschema alla hello olika kombinationer för dessa parametrar och hello resulterande villkor för hello katalog och självbetjäningsregistreringen.

![][1]

Mer information och exempel på hur toouse parametrarna, se [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Se även
* [Hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet-referens](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
