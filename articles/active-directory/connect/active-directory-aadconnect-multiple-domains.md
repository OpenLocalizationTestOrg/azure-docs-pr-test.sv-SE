---
title: "aaaAzure AD ansluta flera domäner"
description: "Det här dokumentet beskriver hur du konfigurerar och konfigurera flera domäner på toppnivå med O365 och Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 91d87875ceacee4e34f132938e4bb0294fb954e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Stöd för flera domäner för federering med Azure AD
hello följande dokumentation innehåller information om hur toouse flera domäner på toppnivå och underdomäner när federering med Office 365 eller Azure AD-domäner.

## <a name="multiple-top-level-domain-support"></a>Stöd för flera toppdomäner
Federering flera domäner på toppnivå med Azure AD kräver ytterligare konfiguration som inte krävs när federerar med en toppnivådomän.

När en domän är federerat med Azure AD kan flera egenskaper anges på hello domän i Azure.  Ett är viktigt IssuerUri.  Detta är en URI som används av Azure AD tooidentify hello-domän som hello token som är associerad med.  hello URI behöver inte tooresolve tooanything men det måste vara en giltig URI.  Standard Azure AD anger toohello värdet för hello federationstjänstidentifierare i din lokala AD FS konfiguration.

> [!NOTE]
> Hej federationstjänstidentifierare är en URI som unikt identifierar en federationstjänst.  hello federationstjänsten är en instans av AD FS som fungerar som hello säkerhetstokentjänsten. 
> 
> 

Du kan visa IssuerUri med hello PowerShell-kommandot `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Ett problem uppstår när vi vill tooadd mer än en toppnivådomän.  Till exempel anta att du har installationen federation mellan Azure AD och din lokala miljö.  Jag använder bmcontoso.com för det här dokumentet.  Nu jag har lagt till en andra, på den översta nivån domän, bmfabrikam.com.

![Domäner](./media/active-directory-multiple-domains/domains.png)

När vi försöker tooconvert våra bmfabrikam.com domän toobe federerad felmeddelande vi ett.  hello anledningen är Azure AD har en begränsning som inte tillåter hello IssuerUri egenskapen toohave hello samma värde för fler än en domän.  

![Federation fel](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain Parameter
tooworkaround detta, behöver vi tooadd en annan IssuerUri som kan göras med hjälp av hello `-SupportMultipleDomain` parameter.  Den här parametern används med hello följande cmdlets:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Denna parameter gör Azure AD konfigurera hello IssuerUri så att den är baserad på hello namnet på hello domän.  Detta är unik i kataloger i Azure AD.  Med hjälp av parametern hello kan hello PowerShell-kommandot toocomplete har.

![Federation fel](./media/active-directory-multiple-domains/convert.png)

Tittar på hello inställningar på vår nya bmfabrikam.com domänen som du kan se hello följande:

![Federation fel](./media/active-directory-multiple-domains/settings.png)

Observera att `-SupportMultipleDomain` ändras inte hello slutpunkter som är fortfarande konfigurerade toopoint tooour federationstjänsten på adfs.bmcontoso.com.

En annan sak som `-SupportMultipleDomain` har är det garanterar att hello AD FS system innehåller hello rätt utfärdaren värdet i token som utfärdats för Azure AD. Detta åstadkoms genom att använda hello domän delen av hello användare UPN och ange detta som hello domän i hello IssuerUri, d.v.s. https://{upn suffix} / adfs-services-förtroendet. 

Under autentiseringen tooAzure AD eller Office 365 är hello IssuerUri element i hello användartoken därför används toolocate hello domän i Azure AD.  Om en matchning inte hittas hello autentiseringen misslyckas. 

Till exempel om en användares UPN är bsimon@bmcontoso.com, hello IssuerUri element i hello token AD FS problem anges toohttp://bmcontoso.com/adfs/services/trust. Detta kommer att matcha hello Azure AD-konfiguration och autentiseringen lyckas.

hello följer hello anpassad anspråksregel som implementerar denna logik:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> I ordning toouse hello - SupportMultipleDomain växla vid försök tooadd som är nya eller konvertera redan lagts till domäner, måste du toohave installationsprogrammet federerat förtroende-toosupport dem ursprungligen.  
> 
> 

## <a name="how-tooupdate-hello-trust-between-ad-fs-and-azure-ad"></a>Hur tooupdate hello förtroende mellan AD FS och Azure AD
Om du inte konfigurera hello federerat förtroende mellan AD FS och din instans av Azure AD, måste du kanske toore-skapa förtroendet.  Detta beror på att när den är ursprungligen installationen utan hello `-SupportMultipleDomain` parametern hello IssuerUri anges med hello standardvärdet.  I hello ser skärmbilden nedan du hello IssuerUri anges toohttps://adfs.bmcontoso.com/adfs/services/trust.

Så om vi har lagt till en ny domän i hello Azure AD-portalen och försök sedan tooconvert nu den med hjälp av `Convert-MsolDomaintoFederated -DomainName <your domain>`, vi hämta hello följande fel.

![Federation fel](./media/active-directory-multiple-domains/trust1.png)

Om du försöker tooadd hello `-SupportMultipleDomain` växel som vi får hello följande fel:

![Federation fel](./media/active-directory-multiple-domains/trust2.png)

Bara försök toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` på hello ursprungliga domänen också resulterar i ett fel.

![Federation fel](./media/active-directory-multiple-domains/trust3.png)

Använd hello steg nedan tooadd en ytterligare toppnivådomän.  Om du redan har lagt till en domän och inte har använt hello `-SupportMultipleDomain` parametern start med hello steg för att ta bort och uppdatera din ursprungliga domän.  Om du inte har lagt till en toppnivådomän kan än du starta med hello steg för att lägga till en domän med hjälp av PowerShell för Azure AD Connect.

Använd följande steg tooremove hello Microsoft Online förtroende hello och uppdatera din ursprungliga domän.

1. På din AD FS-federationsserver öppna **AD FS-hantering.** 
2. Hello vänster, expandera **förtroenden** och **förtroende för förlitande part**
3. I högra hello, tar du bort hello **Identitetsplattformen för Microsoft Office 365** post.
   ![Ta bort Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
4. På en dator som har [Azure Active Directory-modulen för Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installerad kör hello följande: `$cred=Get-Credential`.  
5. Ange hello användarnamn och lösenord för en global administratör för hello Azure AD-domän du federerar med
6. Ange i PowerShell`Connect-MsolService -Credential $cred`
7. Ange i PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Detta är hello ursprungliga domän.  Det med hjälp av hello över domäner som den skulle ha:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Använd hello följande steg tooadd hello ny domän på toppnivå med hjälp av PowerShell

1. På en dator som har [Azure Active Directory-modulen för Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installerad kör hello följande: `$cred=Get-Credential`.  
2. Ange hello användarnamn och lösenord för en global administratör för hello Azure AD-domän du federerar med
3. Ange i PowerShell`Connect-MsolService -Credential $cred`
4. Ange i PowerShell`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Använd hello följande steg tooadd hello ny domän på toppnivå med Azure AD Connect.

1. Starta Azure AD Connect från hello skrivbordet eller start-menyn
2. Välj ”Lägg till en ytterligare Azure AD-domän” ![Lägg till en Azure AD-domän](./media/active-directory-multiple-domains/add1.png)
3. Ange din Azure AD och Active Directory-autentiseringsuppgifter
4. Välj andra hello domän som du vill tooconfigure för federation.
   ![Lägg till en Azure AD-domän](./media/active-directory-multiple-domains/add2.png)
5. Klicka på Installera

### <a name="verify-hello-new-top-level-domain"></a>Kontrollera hello nya toppnivådomänen
Med hjälp av PowerShell-kommando för hello `Get-MsolDomainFederationSettings -DomainName <your domain>`kan du visa hello uppdateras IssuerUri.  hello skärmbilden nedan visar hello federation inställningarna har uppdaterats i vår ursprungliga domän http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Och hello IssuerUri på vår nya domänen har ställts in toohttps://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Stöd för underordnade domäner
När du lägger till en underordnad domän på grund av hello sätt Azure AD hanterade domäner, ärver den hello inställningar för hello överordnade.  Det innebär att hello IssuerUri måste toomatch hello överordnade.

Så kan anta till exempel att jag har bmcontoso.com och Lägg sedan till corp.bmcontoso.com.  Det innebär att hello IssuerUri för en användare från corp.bmcontoso.com måste toobe **http://bmcontoso.com/adfs/services/trust.**  Men hello standard regeln implementeras ovan för Azure AD ska generera en token med en utfärdare som **http://corp.bmcontoso.com/adfs/services/trust.** som inte matchar hello domän obligatoriskt värde och autentiseringen misslyckas.

### <a name="how-tooenable-support-for-sub-domains"></a>Hur tooenable stöd för underordnade domäner
I ordning toowork runt den här hello måste AD FS förtroende för förlitande part för Microsoft Online toobe uppdateras.  toodo, måste du konfigurera en anpassad anspråksregel så att den tar ut alla underordnade domäner från hello användarens UPN-suffixet vid hello anpassat utfärdaren värde. 

hello följande anspråk kommer att göra det:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
hello sista nummer i hello reguljärt uttryck ange hello hur många överordnade domäner finns i rotdomänen. Här finns i bmcontoso.com så att två överordnade domäner krävs. Om tre överordnade domäner har toobe behålls (d.v.s.: corp.bmcontoso.com), och sedan hello nummer skulle ha varit tre. Eventualy en intervallet kan anges, hello matchar görs alltid toomatch hello maximalt domäner. {2,3}-matchar två toothree domäner (d.v.s.: bmfabrikam.com och corp.bmcontoso.com).

Använd följande steg tooadd hello ett anpassat anspråk toosupport underordnade domäner.

1. Öppna AD FS-hantering
2. Högerklicka på hello Microsoft Online RP förtroende och väljer Redigera anspråksregler
3. Välj hello tredje anspråksregel och Ersätt ![redigera anspråk](./media/active-directory-multiple-domains/sub1.png)
4. Ersätt hello aktuella anspråk:
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Ersätt anspråk](./media/active-directory-multiple-domains/sub2.png)

5. Klicka på Ok.  Klicka på Använd.  Klicka på Ok.  Stäng AD FS-hantering.

