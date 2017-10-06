---
title: "aaaHow toouse en Azure Active Direcory klient directory översikt | Microsoft Docs"
description: "Förklarar vad en Azure AD-klient är och hur toomanage Azure med hjälp av Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.openlocfilehash: ddb16d89bf06a3753ed5106bd95162130ad51b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-azure-ad-directory"></a>Hantera Azure AD-katalogen

## <a name="what-is-an-azure-ad-tenant"></a>Vad är en Azure AD-klientorganisation eller Azure AD-klient?
I Azure Active Directory (Azure AD) är en klient en dedikerad instans av en Azure AD-katalog som din organisation tilldelas när den registrerar sig för en Microsoft-molntjänst som Azure eller Office 365. Varje Azure AD-katalog är separat och åtskild från andra Azure AD-kataloger. Precis som ett företags kontorsbyggnad är en säker resurs specifika tooonly din organisation, Azure AD-katalog har också utformad toobe en säker tillgång för exklusiv användning av din organisation. hello Azure AD-arkitekturen isolerar kundinformation för data- och identitetsskydd så att användare och administratörer av en Azure AD-katalog inte oavsiktligt eller illvilligt komma åt data i en annan katalog.

![Hantera Azure Active Directory](./media/active-directory-administer/aad_portals.png)

## <a name="how-can-i-get-an-azure-ad-directory"></a>Hur kan jag skaffa en Azure AD-katalog?
Azure AD innehåller hello kärnor och identitetshanteringsfunktioner bakom de flesta av Microsofts molntjänster, inklusive:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Du får en Azure AD-katalog när du registrerar dig för någon av dessa Microsoft-molntjänster. Du kan skapa ytterligare kataloger efter behov. Du kan till exempel skapa din första katalog som en produktionskatalog och sedan skapa en annan katalog för testning eller mellanlagring.

### <a name="using-hello-azure-ad-directory-that-comes-with-a-new-azure-subscription"></a>Med hjälp av hello Azure AD-katalog som levereras med en ny Azure-prenumeration

Vi rekommenderar att du använder hello administratörskontot som du använde för din första tjänst när du registrerar dig för andra Microsoft-tjänster. hello information som du anger hello första gången du registrerar dig för en Microsoft-tjänst är används toocreate en ny Azure AD-kataloginstans för din organisation. Om du använder att directory tooauthenticate inloggningsförsök när du prenumererar tooother Microsoft-tjänster kan de använda hello befintliga användarkonton, principer, inställningar eller lokal katalogintegrering som du konfigurerar i din standardkatalog.

Till exempel om du registrerar dig för en prenumeration på Microsoft Intune och sedan ytterligare synkronisera din lokala Active Directory med Azure AD-katalogen du kan registrera dig för en annan Microsoft-tjänst som Office 365 och enkelt uppnå hello samma katalog Integration fördelar som du har med Microsoft Intune.

Mer information om hur du integrerar din lokala katalog med Azure AD finns i [Katalogintegrering med Azure AD Connect](active-directory-aadconnect.md).

### <a name="associate-an-existing-azure-ad-directory-with-a-new-azure-subscription"></a>Associera en befintlig Azure AD-katalog med en ny Azure-prenumeration
Du kan associera en ny Azure-prenumeration med hello samma katalog som autentiserar inloggningen för en befintlig Office 365 eller Microsoft Intune-prenumeration. Mer information om scenariot finns [överför ägarskapet för ett Azure-prenumeration tooanother konto](../billing/billing-subscription-transfer.md)

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Skapa en Azure AD-katalog genom att registrera dig för en Microsoft-molntjänst som en organisation
Om du ännu inte har en prenumeration tooa Microsoft-molntjänst kan använda du någon av följande länkar toosign hello. När du registrerar dig för din första tjänst skapas en Azure AD-katalog automatiskt.

* [Microsoft Azure](https://account.azure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="how-toochange-hello-default-directory-for-a-subscription"></a>Hur toochange hello standardkatalogen för en prenumeration

1. Logga in toohello [Azure Kontocenter](https://account.windowsazure.com/Home/Index) med ett konto som är hello kontoadministratör ägandekostnad hello prenumeration tootransfer prenumeration.
2. Se till att hello som användaren har toobe hello prenumerationsägaren finns i katalogen för hello mål.
3. Klicka på **överföra äganderätten till prenumerationen**.
4. Ange hello mottagare. hello mottagare får automatiskt ett e-postmeddelande med en länk för godkännande.
5. hello mottagaren klickar hello länken och följer hello instruktioner, inklusive att ange sina betalningsinformation. När hello mottagaren lyckas, överförs hello prenumeration. 
6. hello standardkatalogen hello prenumeration ändras toohello katalog som hello användaren är i om hello ägarskap prenumerationsöverföring är lyckas.

### <a name="manage-hello-default-directory-in-azure"></a>Hantera hello standardkatalogen i Azure
När du registrerar dig för Azure associeras en standardkatalog för Azure AD med din prenumeration. Det kostar inget att använda Azure AD och dina kataloger är en kostnadsfri resurs. Det finns betalda Azure AD-tjänster som licensieras separat och innehåller ytterligare funktioner, till exempel företagsanpassning vid inloggning och lösenordsåterställning via självbetjäning. Du kan också skapa en anpassad domän med DNS-namnet som du äger i stället för hello standard *. onmicrosoft.com-domän.

## <a name="how-can-i-manage-directory-data"></a>Hur hanterar jag katalogdata?
tooadminister en eller flera Microsoft-molntjänstprenumerationer, kan du använda hello [administrationscentret för Azure AD](https://aad.portal.azure.com), hello Microsoft Intune-kontoportalen eller hello [Office 365 Admin Center](https://portal.office.com/) toomanage din organisationens katalogdata. Du kan också använda [Azure Active Directory PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/active-directory) toohelp du hantera data som lagras i Azure AD.

Från dessa portaler (eller cmdletar) kan du:

* Skapa och hantera användar- och gruppkonton
* Hantera relaterade molntjänster för din organisations prenumerationer
* Konfigurera lokal integrering med identitets- och autentiseringstjänster i Azure AD

hello Azure AD Administrationscenter, Administrationscenter för Office 365, Microsoft Intune-kontoportalen och hello Azure AD-cmdlets läser alla från och skriva tooa delad instans av Azure AD som associeras med din organisations katalog. Alla dessa verktyg fungerar som ett klientdelsgränssnitt som hämtar eller ändrar dina katalogdata.

När du ändrar organisationens data med hjälp av hello portaler eller cmdlets loggat in under hello kontexten för någon av dessa tjänster visas hello ändringar även i hello andra portaler hello nästa gång du loggar in. Dessa data delas mellan hello Microsoft cloud services toowhich du prenumererar på.

Till exempel om du använder hello Office 365 Admin Center tooblock en användare från att logga in som åtgärden block hello användare från att logga in tooany andra tjänsten toowhich din organisation prenumererar på. Om du visar hello samma användarkonto i hello Microsoft Intunes kontoportal du också se hello användaren är blockerad.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Hur kan jag lägga till och hantera flera kataloger?
Du kan [lägga till en Azure AD-katalog i hello Azure-portalen](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Fylla i hello information och välj **skapa**.

Du kan hantera alla kataloger som helt oberoende resurser. Varje katalog är en peer med en fullständig funktionsuppsättning som är logiskt oberoende av andra kataloger som du hanterar. Det finns ingen överordnad-underordnad-relation mellan kataloger. Detta oberoende mellan kataloger gäller resursoberoende, administrativt oberoende och synkroniseringsoberoende.

* **Resursoberoende**. Om du skapar eller tar bort en resurs i en katalog har ingen inverkan på alla resurser i en annan katalog med hello partiella undantag av externa användare. Om du använder en anpassad ”contoso.com”-domän med en katalog kan den inte användas med någon annan katalog.
* **Administrativt oberoende**.  Om en användare som inte är administratör för katalogen ”Contoso” skapar testkatalogen ”Test”:
  
  * hello administratörer för katalogen ”Contoso” har ingen direkt administratörsbehörighet toodirectory 'Test' om inte en administratör ”test” uttryckligen beviljar dem sådan behörighet. ”Contoso”-administratörerna kan styra åtkomst toodirectory ”Test” tack vare deras kontroll av hello användarkontot som skapade ”Test”.
    
  * Om du kan tilldela eller ta bort en administratörsroll för en användare i en katalog, hello ändringen påverkar inte ändringen administratörsrollen kan som användaren ha i en annan katalog.
* **Synkroniseringsoberoende**. Du kan konfigurera varje Azure AD-klient oberoende tooget synkronisera data från en enda instans hello Azure AD Connect verktyget katalogsynkronisering.

Till skillnad från andra Azure-resurser är dina kataloger inte underordnade resurser till en Azure-prenumeration. Så om du avbryter eller låter din Azure-prenumeration tooexpire, du kan fortfarande komma åt dina katalogdata med Azure AD PowerShell, hello Azure Graph API eller andra gränssnitt, till exempel hello Office 365 Admin Center. Du kan även associera en annan prenumeration med hello directory.

## <a name="how-tooprepare-toodelete-an-azure-ad-directory"></a>Hur tooprepare toodelete Azure AD-katalog
En global administratör kan ta bort en Azure AD-katalog från hello-portalen. När en katalog tas bort raderas även alla resurser som ingår i hello directory. Kontrollera att du inte behöver hello directory innan du tar bort den.

> [!NOTE]
> Om hello användare är inloggad med ett konto för arbetet eller skolan, måste hello användare inte försöker toodelete deras hemkatalog. Till exempel om hello användare är inloggad som joe@contoso.onmicrosoft.com, att användaren kan inte ta bort hello-katalogen som har contoso.onmicrosoft.com som standarddomän.

Azure AD kräver att vissa villkor är uppfyllda toodelete en katalog. Detta minskar risken för att ta bort en katalog negativt påverkar användare eller program, till exempel hello möjligheten för användare toosign i tooOffice 365 eller komma åt resurser i Azure. Till exempel om en katalog för en prenumeration är tas bort av misstag, sedan användare kan inte komma åt hello Azure-resurser för den prenumerationen.

hello följande villkor kontrolleras:

* hello ska endast användare i katalogen för hello hello globala administratören som är toodelete hello katalog. Andra användare måste tas bort innan hello katalogen kan tas bort. Om användarna synkroniseras lokalt, och sedan synkronisera måste stängas av och hello användare måste tas bort i molnkatalogen hello med hjälp av hello Azure-portalen eller Azure PowerShell-cmdlets. Det finns inga krav toodelete grupper eller kontakter, till exempel kontakter som lagts till från hello Office 365 Admin Center.
* Det kan finnas några program i hello directory. Alla program måste tas bort innan hello katalogen kan tas bort.
* Inga Multi-Factor authentication-leverantörer kan vara länkade toohello katalog.
* Det kan finnas några prenumerationer för Microsoft Online Services, till exempel Microsoft Azure, Office 365 eller Azure AD Premium som är associerade med hello directory. Om en standardkatalog skapades för dig i Azure kan du inte ta bort den katalogen om din Azure-prenumeration fortfarande är beroende av den här katalogen för autentisering. På liknande sätt kan du inte ta bort en katalog om en annan användare har associerat en prenumeration med den. 


## <a name="next-steps"></a>Nästa steg
* [Azure AD-forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
* [Azure Multi-Factor Authentication-forum](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
* [Frågor om Stack Overflow för Azure](http://stackoverflow.com/questions/tagged/azure)
* [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory)
* [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md)
