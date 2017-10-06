---
title: "aaaSelf tjänstprogrammet åtkomst och delegerad hantering med Azure Active Directory | Microsoft Docs"
description: "Den här artikeln beskriver hur tooenable självbetjäning program åt och delegerad hantering med Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 448a7fe8-a162-475e-9ba2-2e3ab59302bc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 90bec3bd71796f22a782929b028db0d18c3aa1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-application-access-and-delegated-management-with-azure-active-directory"></a>Självbetjäning programåtkomst och delegerad hantering med Azure Active Directory
Aktivera funktioner för självbetjäning för slutanvändare är ett vanligt scenario för företagets IT. Många användare, och många program och hello person best-informed toomake åtkomst bevilja beslut inte får vara hello directory-administratör. Ofta hello bästa person toodecide som kan komma åt ett program är ett team lead eller andra delegerad administratör. Det är dock hello-användare som använder hello app och hello användaren vet vad de behöver toobe kan toodo sitt jobb.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. 

Självbetjäning programåtkomst är en funktion i [Azure Active Directory Premium](https://azure.microsoft.com/trial/get-started-active-directory/) P1 och P2 licensiering som tillåter directory-administratörerna:

* Aktivera åtkomst för användare toorequest tooapplications med hjälp av en ”hämta fler program” panelen i hello [Azure AD-åtkomstpanelen](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)
* Ange vilka program som användare kan begära åtkomst till
* Ange om ett godkännande krävs för användare toobe kan tooself tilldela åtkomst tooan program
* Ange vem som ska godkänna hello begäranden och hantera åtkomsten för varje program

Idag den här funktionen stöds för alla förintegrerade och anpassade appar som stöder federerade eller lösenordsbaserade enkel inloggning i hello [Azure Active Directory-programgalleriet](https://azure.microsoft.com/marketplace/active-directory/all/), inklusive appar som Salesforce Dropbox, Google Apps och mycket mer.
Den här artikeln beskriver hur du:

* Konfigurera självbetjäning programåtkomst för slutanvändare, inklusive hur du konfigurerar ett valfritt Godkännandearbetsflöde 
* Delegera hantering för specifika program toohello lämpligaste personer i din organisation, och aktivera dem toouse hello Azure AD panelen tooapprove åtkomstförfrågningar, direkt tilldela åtkomst tooselected användare eller ange (valfritt) autentiseringsuppgifter för åtkomst till programmet när lösenordsbaserade enkel inloggning har konfigurerats

## <a name="configuring-self-service-application-access"></a>Konfigurera självbetjäning programåtkomst
tooenable självbetjäning programåtkomst och konfigurerat vilka program som kan läggas till eller begärs av slutanvändare, följer du dessa instruktioner.

1. Logga in på hello [klassiska Azure-portalen](https://manage.windowsazure.com/).

2.   Under hello **Active Directory** avsnittet, Välj din katalog och välj sedan hello **program** fliken. 

3. Välj hello **Lägg till** knappen använder hello galleriet alternativet tooselect och lägga till ett program.

4. När appen har lagts till, får du hello app snabbstartsidan. Klicka på **Konfigurera enkel inloggning**hello enkel inloggning läge och välj Spara hello-konfigurationen. 

5. Välj därefter hello **konfigurera** fliken tooenable användare toorequest åtkomst toothis app från hello Azure AD-åtkomstpanelen, ange **Tillåt självbetjäning programåtkomst** för**Ja**.
  
  ![][1]

6. toooptionally konfigurera ett Godkännandearbetsflöde för förfrågningar, ange **kräver godkännande innan åtkomst beviljas** för**Ja**. Sedan kan välja en eller flera granskare med hello **godkännare** knappen.

  Godkännare kan alla användare i hello organisation med Azure AD-kontot och kan ansvara för konto-etablering, licenser, eller andra affärsprocess din organisation kräver innan du beviljar åtkomst tooan app. hello godkännare kan även vara hello gruppägare av en eller flera delade kontogrupper och kan tilldela hello användare tooone av dessa grupper toogive dem kommer åt via ett delat konto. 

  Om ingen godkännande krävs få användare direkt hello programmet tillagda tootheir Azure AD-åtkomstpanelen. Om programmet hello har ställts in för [automatisk användaretablering](active-directory-saas-app-provisioning.md), eller har ställts in [”användarhanterat” lösenord SSO läge](active-directory-appssoaccess-whatis.md#password-based-single-sign-on), hello användaren har redan en användare-kontot och känna hello lösenordet.

7. Om programmet hello har konfigurerade toouse är lösenordsbaserade enkel inloggning, sedan ett alternativ för att tillåta hello godkännare tooset hello SSO autentiseringsuppgifter för varje användares räkning också tillgängligt. Mer information finns i avsnittet hello på [delegerad åtkomsthantering](#delegated-application-access-management).

8. Slutligen hello **grupp för Self-Assigned användare** visar hello namnet på hello-grupp som används toostore hello användare som har tilldelats eller tilldelad åtkomst toohello program. hello åtkomst godkännare blir hello ägare av den här gruppen. Om hello gruppnamn visas inte finns, skapas den automatiskt. Alternativt kan hello gruppnamn anges toohello namnet på en befintlig grupp.

9. toosave hello konfiguration, klickar du på **spara** längst hello hello-skärmen. Användarna kan nu toorequest access toothis-appen från hello åtkomstpanelen.

10. tootry hello slutanvändarens upplevelse, logga in på din organisations Azure AD-åtkomstpanelen på https://myapps.microsoft.com, helst med ett annat konto som inte är en app godkännare. 

11. Under hello **program** klickar du på hello **få fler program** panelen. Den här panelen visar ett galleri med alla hello-program som har aktiverats för självbetjäning programåtkomst i hello directory med hello möjlighet toosearch och filtrera efter kategori app hello vänster. 

12. Klicka på en app av systemtillstånd aktiveras hello process för begäran. Om inga godkännandeprocessen krävs kommer programmet hello läggs omedelbart under hello **program** fliken efter en kort bekräftelse. Om godkännande krävs, sedan visas en dialogruta som anger detta och ett e-postmeddelande skickas toohello godkännare. Du måste vara inloggad på hello panelen åtkomst som en icke-godkännare toosee för den här processen för begäran.

13. hello-leder hello godkännare toosign till hello Azure AD-åtkomstpanelen och Godkänn begäran om hello. När hello begäran har godkänts (och eventuella särskilda processer som du definierar har utförts av hello godkännare) hello användaren ser hello program visas under deras **program** flik där de kan logga in på den.

## <a name="delegated-application-access-management"></a>Delegerad åtkomst för programhantering
Godkännare programmet åtkomst kan alla användare i organisationen som är hello lämpligaste person tooapprove eller neka åtkomst toohello programmet ifråga. Den här användaren kan vara ansvarig för konto-etablering, licenser, eller andra affärsprocess din organisation kräver innan du beviljar åtkomst tooan app.

När du konfigurerar självbetjäning programåtkomst som beskrivs ovan, någon tilldelat program godkännare finns ytterligare **hantera program** panelen i hello Azure AD-åtkomstpanelen som visar dem vilka program som de är hello access-administratör. Klicka på en app visas en skärm flera alternativ.

![][2]

### <a name="approve-requests"></a>Godkänna begäranden
Hej **godkänna begäranden** panelen tillåter godkännare toosee väntande godkännanden specifika toothat appen och omdirigerar toohello godkännanden fliken där hello begär kan bekräftas eller nekas. hello godkännare tar också emot automatiserade e-postmeddelanden när en begäran skapas som gör dem vilka toodo.

### <a name="add-users"></a>Lägg till användare
Hej **Lägg till användare** panelen låter godkännare toodirectly valt bevilja användare åtkomst toohello program. När du klickar på den här panelen ser hello godkännare en dialogruta låter dem tooview och Sök efter användare i sina kataloger. Lägger till en användare blir i hello program som visas i dessa användarens Azure AD åtkomst paneler eller Office 365. Om det krävs någon manuell användare etableringsprocessen hello app innan hello användare är kan toosign i sedan hello godkännare ska utföra den här processen innan du tilldelar åtkomst.  

### <a name="manage-users"></a>Hantera användare
Hej **hantera användare** panelen tillåter godkännare toodirectly uppdaterings- eller vilka användare som har åtkomst till toohello program. 

### <a name="configure-password-sso-credentials-if-applicable"></a>Konfigurera enkel inloggning lösenordsinformation (om tillämpligt)
Hej **konfigurera** panelen visas bara om programmet hello har konfigurerats av hello IT-administratören toouse lösenordsbaserade enkel inloggning och Hej administratör beviljas hello godkännare hello möjlighet tooset lösenord SSO autentiseringsuppgifter som beskrivits ovan. När du väljer visas hello godkännare flera alternativ för hur hello autentiseringsuppgifter är spridda tooassigned användare:

![][3]

* **Användare som loggar in med sina egna lösenord** – i det här läget hello tilldelade användarna vet vad användarnamn och lösenord är för hello program och är begärd tooenter dem vid första inloggningen toohello tillämpningen. hello scenariot motsvarar toohello lösenord SSO fall där hello [användare hantera autentiseringsuppgifter](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Användarna automatiskt loggas in med separata konton som jag hanterar** – i det här läget hello tilldelade användare som inte är nödvändiga tooenter eller vet sina app-specifik autentiseringsuppgifter när du loggar in hello program. I stället hello godkännare anger hello autentiseringsuppgifter för varje användare efter att åtkomst med hjälp av hello **Lägg till användare** panelen. När hello användare klickar på hello program i deras åtkomstpanelen eller Office 365, loggas de automatiskt in med hello autentiseringsuppgifter som anges av hello godkännare. hello scenariot motsvarar toohello lösenord SSO fall där hello [administratörer hantera autentiseringsuppgifter](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).
* **Användarna automatiskt loggas in med ett konto som jag hanterar** -ett specialfall, det här fallet är lämplig toouse när alla tilldelade användare behöver toobe beviljas åtkomst med hjälp av en enda delad konto. Det är hello vanligaste användningsfall för den här funktionen med sociala mediaprogram, där en organisation har en enda ”” företag och flera användare behöver toomake uppdateringar toothat konto. hello scenariot också motsvarar toohello lösenord SSO fall där hello [administratörer hantera autentiseringsuppgifter](active-directory-appssoaccess-whatis.md#password-based-single-sign-on). Men när du har valt det här alternativet, kommer hello godkännare att tillfrågas tooenter hello användarnamn och lösenord för hello delade konto. När slutfört, är alla tilldelade användare inloggad med det här kontot när du klickar på hello program i sin Azure AD åtkomst paneler eller Office 365.

## <a name="additional-resources"></a>Ytterligare resurser
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[1]: ./media/active-directory-self-service-application-access/ssaa_admin.PNG
[2]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app.PNG
[3]: ./media/active-directory-self-service-application-access/ssaa_ap_manage_app_config.PNG
