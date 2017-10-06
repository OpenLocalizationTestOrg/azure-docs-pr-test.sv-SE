---
title: "aaaWhat är hello åtkomstpanelen i Azure Active Directory? | Microsoft Docs"
description: "Lär dig hur toouse variationer av hello åt panel (webbläsare, Android-app, iPhone och iPad) tooaccess SaaS-appar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 800be6a69f13978c5b88e2fe28a416d4b763656c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-access-panel"></a>Vad är hello åtkomstpanelen?

Hej åtkomstpanelen är en webbaserad portal. Det gör det möjligt för en användare med ett arbets eller skolkonto i Azure Active Directory tooview och starta molnbaserade program som en Azure AD-administratör har gett dem åtkomst till. Du kan också använda självbetjäning och funktioner för hantering av appen via hello åtkomstpanelen.

Hej åtkomstpanelen skiljer sig från hello Azure-portalen och har inte du toohave en Azure-prenumeration.

![Åtkomstpanel][1]

Hej åtkomstpanelen kan tooedit vissa profilinställningar för din, inklusive hello möjlighet att:

- Ändra hello lösenordet associerat med ett arbets- eller skolkonto konto

- Redigera inställningar för återställning av lösenord

- Redigera kontakt- och inställningar inställningar relaterade toomulti-factor-autentisering (för konton som har nödvändiga toouse den av en administratör)

- Konto som information, till exempel användar-ID, alternativa e- och mobile och office telefonnummer och enheter

- Visa och starta molnbaserade program som hello Azure AD-administratör har gett dem åtkomst till. Mer information om hello åtkomstpanelen ur hello användare finns i använda hello åtkomstpanelen. 

- Själva hantera grupper. Mer specifikt kan hello-administratören skapa och hantera säkerhetsgrupper och begäran medlemskap i säkerhetsgrupper i Azure AD. Mer information finns i [grupphantering via Självbetjäning för användare i Azure AD](active-directory-accessmanagement-self-service-group-management.md) och [hantera grupper](active-directory-manage-groups.md).




## <a name="accessing-hello-access-panel"></a>Åtkomst till hello åtkomstpanelen

Du kan komma åt åtkomstpanelen hello genom att besöka hello följande URL i en webbläsare:`http://myapps.microsoft.com`

Om du har anpassning som konfigurerats för sidan logga in, kan du läsa in den här anpassningen genom att lägga till din organisations domän toohello slutet av hello-URL:`http://myapps.microsoft.com/<your domain>.com`

I det här fallet kan du använda alla aktiva eller verifierat domännamn som har konfigurerats i Azure-portalen.

![Wingtip Toys domännamn][2]  

Du måste toodistribute hello URL tooall användare som ska signera i tooapplications som är integrerade med Azure AD.

## <a name="authentication"></a>Autentisering

tooreach hello åtkomstpanelen, du måste autentiseras via ett arbets- eller skolkonto konto i Azure AD. Du kan vara autentiserade tooAzure AD direkt. Alternativt, om en organisation har konfigurerat federation med hjälp av Active Directory Federation Services (AD FS) eller andra tekniker, du kan autentiseras av Windows Server Active Directory.

Om du har en prenumeration på Azure eller Office 365 och du har använt hello Azure-portalen eller ett Office 365-program, kan du se hello listan med program utan att logga in igen. Om du inte har autentiserats du är ange toosign i med hjälp av hello användarnamn och lösenord för ditt konto i Azure AD. Om din organisation har konfigurerat federation, räcker att skriva hello användarnamn.

Du kan interagera med hello-program som administratören har integrerat med hello directory när du är autentiserad. hur toointegrate program med Azure AD, se toolearn [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="web-browser-requirements"></a>Webbläsarkrav

Minst hello åtkomstpanelen kräver en webbläsare som stöder JavaScript och har aktiverat för CSS. För hello användaren toobe inloggad tooapplications via lösenordsbaserade enkel inloggning (SSO), måste tillägget för hello åtkomst panelen installeras i webbläsaren. hello tillägg laddas ned automatiskt när du väljer ett program som har konfigurerats för lösenordsbaserad enkel inloggning.

tillägget för hello åtkomst panelen är tillgänglig för Internet Explorer 8 och senare, gräns, Chrome, Firefox webbläsare och.

## <a name="mobile-app-support"></a>Stöd för mobila appar

hello Azure Active Directory-teamet publicerar hello min mobilapp för appar. När du installerar hello app måste logga du in SSO toopassword-baserade program på iOS och Android-enheter.

> [!NOTE]
> Du kan logga in tooapplications som stöder federation med Azure AD (inklusive Salesforce, Google Apps, Dropbox, rutan, Concur, Workday, Office 365 och mycket mer än 70 andra) på valfri webbläsare på alla enheter utan att behöva en plugin-program eller mobil app. Alla andra [åtkomst till panelen upplevelser](https://myapps.microsoft.com/) kräver också inte hello min mobilapp toobe för appar som används på en mobil enhet.
>
>

### <a name="my-apps-for-android"></a>Mina appar för Android

Mina appar för Android fungerar på alla Android-enhet som kör Android version 4.1 och senare.  
Det är tillgängligt i hello [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.myapps).

![Mina appar för Android][3]   

### <a name="my-apps-for-iphone-and-ipad"></a>Mina appar för iPhone och iPad

Mina appar för iOS stöds på en iPhone eller iPad som kör iOS version 7 och senare.  
Det är tillgängligt i hello [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).

![Mina appar för iOS][4]    



## <a name="managed-browser-for-my-apps"></a>Hanterad webbläsare för Mina appar

Mina appar är även integrerad i hello Intune Managed Browser. Hej Intune Managed Browser för iOS och Android-enheter spelar en viktig roll i att säkerställa att data på mobila enheter förblir säkra. Gör det möjligt att på ett säkert sätt visa och navigera på webbsidor som kan innehålla företagsinformation och tillhandahåller en säker surfa på webben.  
Du hittar Snabbåtkomst toomy appar på startsidan för hanterad webbläsare och i dina bokmärken ger färre klickar tooreach de program du vill tooaccess.

Det är tillgängligt i hello [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) och [Google Play-butiken](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).

![Mananged webbläsare för Mina appar][5]    





## <a name="tips-for-testing-hello-user-experience"></a>Tips för tester hello-användarupplevelse

Om du är administratör för Azure och du är inloggad toohello Azure-portalen med ett konto i hello directory, är toohello åtkomstpanelen automatiskt inloggad som ditt aktuella konto. I det här fallet kan du se alla program som har tilldelats tooyou.

**tootest som en *olika* användarkonto:**

1. Klicka på hello användaren i hello övre högra hörnet av hello Azure-portalen eller hello åtkomstpanelen och välj sedan **logga ut**. 
2. Gå toohello [åtkomstpanelen](http://myapps.microsoft.com).
3. Hello-inloggningssida typen hello användarnamn och lösenord för hello konto i din katalog vill du tootest.


## <a name="starting-applications"></a>Start av program

Flera typer av program kan visas på hello åtkomstpanelen.

### <a name="office-365-applications"></a>Office 365-program

Om din organisation använder Office 365-program och du är licensierad för dem, visas hello Office 365-program på din åtkomstpanelen.

När du klickar på ett program sida vid sida för ett Office 365-program är omdirigerade toohello program och automatiskt inloggad.

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a>Microsoft och tredje parts program som har konfigurerats med federationsbaserat enkel inloggning

Din administratör kan lägga till program i hello Active Directory-delen av hello Azure-portalen hello SSO inställd för**Azure AD enkel inloggning**. Du kan bara se dessa program om administratören har beviljat explicit toohello-program.

När du klickar på en panel för de här programmen du omdirigeras och automatiskt inloggad toohello program.

### <a name="password-based-sso-without-identity-provisioning"></a>Lösenordsbaserade SSO utan identitet etablering

Din administratör kan lägga till program i hello Active Directory-delen av hello Azure-portalen hello SSO inställd för**lösenordsbaserade enkel inloggning**. Alla användare i katalogen hello kan se alla program som har konfigurerats i det här läget.

hello första gången du klickar på en panel för en av dessa program är du tillfrågas tooinstall hello lösenord SSO plugin-program för Internet Explorer eller Chrome. hello installation kan kräva du toorestart din webbläsare. När du returnera toohello åtkomstpanelen och på hello programmet panelen igen, du uppmanas att ange ett användarnamn och lösenord för hello program. När du har angett ditt användarnamn och lösenord kan dessa autentiseringsuppgifter lagras på ett säkert sätt och länkat tooyour konto i Azure AD.

hello nästa gång du klickar hello ikonen för programmet, du loggas automatiskt in toohello program.  
Du inte har tooenter dina autentiseringsuppgifter igen och installera hello lösenord SSO plugin-programmet.

Om dina autentiseringsuppgifter har ändrats i hello målprogrammet från tredje part, måste du uppdatera dina autentiseringsuppgifter som lagras i Azure AD. 

**tooupdate autentiseringsuppgifter:**

1. Välj hello-ikon på hello program sida vid sida.
2. Välj **uppdatera autentiseringsuppgifterna** tooreenter hello användarnamn och lösenord för hello program.


### <a name="password-based-sso-with-identity-provisioning"></a>Lösenordsbaserade enkel inloggning med identiteten etablering

Din administratör kan lägga till program i hello **Active Directory** delen av hello Azure-portalen med hello SSO läge inställd för**lösenordsbaserade enkel inloggning**, tillsammans med identiteten etablering.

hello första gången du klickar på ett program sida vid sida för en av dessa program, är du tillfrågas tooinstall hello **lösenord SSO plugin-program för Internet Explorer eller Chrome**. hello installation kan kräva du toorestart din webbläsare.  
När du returnerar toohello åtkomstpanelen och på hello programmet panelen igen, loggas du automatiskt i toohello program.

Vissa program kan kräva du toochange lösenord på hello första inloggning. Om dina autentiseringsuppgifter har ändrats i hello målprogrammet från tredje part, måste du även uppdatera hello-autentiseringsuppgifter som lagras i Azure AD. 

**tooupdate autentiseringsuppgifter:**

1. Välj hello-ikon på hello program sida vid sida.
2. Välj **uppdatera autentiseringsuppgifterna** tooreenter hello användarnamn och lösenord för hello program.


### <a name="application-with-existing-sso-solutions"></a>Program med befintliga lösningar för enkel inloggning

tooconfigure enkel inloggning för ett program hello Azure-portalen innehåller ett tredje alternativ som heter **befintliga enkel inloggning**. Det här alternativet gör att din administratör toocreate ett länk tooan program och placera den på hello åtkomstpanelen för valda användare.

Till exempel om ett program är konfigurerade tooauthenticate användare med hjälp av AD FS 2.0 kan administratören använda hello **befintliga enkel inloggning** alternativet toocreate en länk tooit på hello åtkomstpanelen. När du använder hello länken kan autentiseras via AD FS 2.0 eller det befintliga SSO lösning hello programmet tillhandahåller.


## <a name="next-steps"></a>Nästa steg

- toosee en lista över alla avsnitt som är relaterade tooapplication hantering, se hello [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md).
 
- toolearn hur toointegrate en SaaS-app i Azure AD, se hello [lista över självstudier om hur toointegrate SaaS-appar](active-directory-saas-tutorial-list.md).
 
- toolearn mer information om hur du hanterar appar med Azure AD finns hello [introduktion toosingle inloggning och hantera åtkomst till appen med Azure Active Directory](active-directory-appssoaccess-whatis.md).
 
- toolearn mer information om användaretablering, se [automatisera användaretablering och avetablering tooSaaS program](active-directory-saas-app-provisioning.md).

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
