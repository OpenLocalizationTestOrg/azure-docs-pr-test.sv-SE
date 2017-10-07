---
title: "aaaHeader-baserad autentisering med PingAccess för Azure AD Application Proxy | Microsoft Docs"
description: Publicera program med PingAccess och App-Proxy toosupport huvud-baserad autentisering.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 38fe3e7a41a71f4ae6c75f014e44c722f773bd22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Rubrik-baserad autentisering för enkel inloggning med Application Proxy och PingAccess

Azure Active Directory Application Proxy och PingAccess samarbetar tillsammans tooprovide Azure Active Directory-kunder med åtkomst tooeven fler program. PingAccess expanderar hello [befintliga Application Proxy-erbjudanden](active-directory-application-proxy-get-started.md) tooinclude åtkomst för enkel inloggning tooapplications som använder huvuden för autentisering.

## <a name="what-is-pingaccess-for-azure-ad"></a>Vad är PingAccess för Azure AD?

PingAccess för Azure Active Directory är ett erbjudande PingAccess som aktiverar du toogive användare åtkomst med enkel inloggning tooapplications som använder huvuden för autentisering. Programproxy behandlas dessa appar som helst, med hjälp av Azure AD tooauthenticate åtkomst och skicka trafik via hello kopplingstjänsten. PingAccess placeras framför hello appar och översätter hello åtkomst-token från Azure AD till ett sidhuvud så att programmet hello tar emot hello autentisering i hello-format som den kan läsa.

Användarna kommer inte se något annat när de loggar in toouse ditt företags-appar. De kan fortsätta att arbeta från var som helst på vilken enhet som helst. 

Eftersom hello Application Proxy kopplingar direkt remote trafik tooall appar oavsett deras autentiseringstyp, kommer de fortsätta tooload saldo automatiskt, samt.

## <a name="how-do-i-get-access"></a>Hur skaffar åtkomst?

Eftersom det här scenariot erbjuds via ett partnerskap mellan Azure Active Directory och PingAccess, behöver du licenser för båda tjänsterna. Azure Active Directory Premium prenumerationer innehåller emellertid en grundläggande PingAccess licens som täcker in too20 program. Om du behöver toopublish fler än 20 huvud-baserade program, kan du köpa ytterligare en licens från PingAccess. 

Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).

## <a name="publish-your-application-in-azure"></a>Publicera programmet i Azure

Den här artikeln är avsedd för personer som publicerar en app med det här scenariot för hello första gången. Det går igenom hur tooget igång med både program- och PingAccess, dessutom toohello publishing steg. Om du redan har konfigurerat båda tjänsterna men vill en uppdaterare på hello publicering steg kan du hoppa över hello kopplingsinstallationen och vidare för[lägga till din app tooAzure AD med Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Eftersom det här scenariot är en koppling mellan Azure AD och PingAccess, vissa hello anvisningar finns på hello Ping identitet plats.

### <a name="install-an-application-proxy-connector"></a>Installera en Application Proxy connector

Om du redan har Application Proxy är aktiverat och har en koppling installeras, kan du hoppa över det här avsnittet och flyttar på för[lägga till din app tooAzure AD med Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).

hello Application Proxy connector är en Windows Server-tjänst som dirigerar hello trafik från din fjärranslutna anställda tooyour publicerade appar. Mer detaljerad Installationsinstruktioner finns [aktivera Application Proxy hello Azure-portalen](active-directory-application-proxy-enable.md).

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som global administratör.
2. Välj **Azure Active Directory** > **programproxy**.
3. Välj **hämta anslutning** toostart hello Application Proxy connector hämtning. Följ installationsanvisningarna hello.

   ![Aktivera Application Proxy och hämta hello connector](./media/application-proxy-ping-access/install-connector.png)

4. Hämta hello-anslutningen automatiskt aktivera Application Proxy för din katalog, men om inte du väljer **aktivera Application Proxy**.


### <a name="add-your-app-tooazure-ad-with-application-proxy"></a>Lägg till din app tooAzure AD med Application Proxy

Det finns två åtgärder som du behöver tootake i hello Azure-portalen. Du måste först toopublish ditt program med programproxy. Sedan måste toocollect viss information om hello-app som du kan använda under hello PingAccess steg.

Följ dessa steg toopublish din app. En mer detaljerad genomgång av steg 1 – 8, se [publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md).

1. Om du inte i hello sista avsnittet, loggar du in toohello [Azure-portalen](https://portal.azure.com) som global administratör.
2. Välj **Azure Active Directory** > **företagsprogram**.
3. Välj **Lägg till** hello överst i hello-bladet.
4. Välj **lokalt program**.
5. Fyll i hello krävs fält med information om den nya appen. Använd följande riktlinjer för hello inställningar hello:
   - **Intern URL**: anger normalt hello-URL som tar dig logga toohello appen på sidan när du är på hello företagsnätverket. I det här scenariot måste hello connector tootreat hello PingAccess proxy som hello framsidan hello-appen. Använd följande format: `https://<host name of your PA server>:<port>`. hello porten är 3000 som standard, men du kan konfigurera i PingAccess.
   - **Förautentiseringsmetoden**: Azure Active Directory
   - **Översätta URL: en i sidhuvuden**: Nej

   >[!NOTE]
   >Om det här är ditt första program använder port 3000 toostart och gå tillbaka tooupdate den här inställningen om du ändrar konfigurationen PingAccess. Om det här är en app i andra eller senare behöver detta toomatch hello lyssnare som du har konfigurerat i PingAccess. Lär dig mer om [lyssnare i PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Välj **Lägg till** längst hello hello-bladet. Programmet har lagts till och hello snabb start-menyn öppnas.
7. Hello snabb start-menyn, Välj **tilldela en användare för att testa**, och Lägg till minst en användare toohello program. Kontrollera att det här testet kontot har åtkomst toohello lokalt program.
8. Välj **tilldela** toosave hello test Användartilldelning.
9. På bladet för hantering av hello appen, Välj **enkel inloggning**.
10. Välj **huvud-baserade inloggning** hello nedrullningsbara menyn. Välj **Spara**.

   >[!TIP]
   >Om det här är första gången du använder huvud-baserade enkel inloggning måste tooinstall PingAccess. toomake till din Azure-prenumeration associeras automatiskt med PingAccess installationen, Använd hello länk på den här sidan för enkel inloggning toodownload PingAccess. Du kan öppna hello hämtningsplats nu eller kommer tillbaka toothis sidan senare. 

   ![Välj huvud-baserade inloggning](./media/application-proxy-ping-access/sso-header.PNG)

11. Stäng hello Enterprise program bladet eller bläddra alla hello sätt toohello vänstra tooreturn toohello Azure Active Directory-menyn.
12. Välj **App registreringar**.

   ![Välj App-registreringar](./media/application-proxy-ping-access/app-registrations.png)

13. Välj hello-app som du just lagt till, sedan **Reply URL: er**.

   ![Välj svars-URL: er](./media/application-proxy-ping-access/reply-urls.png)

14. Kontrollera toosee om hello externa URL: en som du tilldelade tooyour app i steg 5 i hello Reply URL-listan. Om den inte gör du det nu.
15. På inställningsbladet för hello app väljer **nödvändiga behörigheter**.

  ![Välj behörigheter som krävs](./media/application-proxy-ping-access/required-permissions.png)

16. Välj **Lägg till**. Hello API, Välj **Windows Azure Active Directory**, sedan **Välj**. Hello behörigheter, Välj **läsa och skriva alla program** och **logga in och Läs användarprofil**, sedan **Välj** och **klar**.  

  ![Välj behörigheter](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-hello-pingaccess-steps"></a>Samla in information för hello PingAccess steg

1. På inställningsbladet din app väljer **egenskaper**. 

  ![Välj egenskaper](./media/application-proxy-ping-access/properties.png)

2. Spara hello **program-Id** värde. Detta används för hello klient-ID när du konfigurerar PingAccess.
3. På inställningsbladet för hello app väljer **nycklar**.

  ![Välj nycklar](./media/application-proxy-ping-access/Keys.png)

4. Skapa en nyckel genom att ange en beskrivning av nyckeln och väljer ett förfallodatum hello nedrullningsbara menyn.
5. Välj **Spara**. Ett GUID som visas i hello **värdet** fältet.

  Spara det här värdet, som du inte kan toosee den igen när du stänga fönstret.

  ![Skapa en ny nyckel](./media/application-proxy-ping-access/create-keys.png)

6. Stäng hello App registreringar bladet eller bläddra alla hello sätt toohello vänstra tooreturn toohello Azure Active Directory-menyn.
7. Välj **egenskaper**.
8. Spara hello **katalog-ID** GUID.

### <a name="optional---update-graphapi-toosend-custom-fields"></a>Valfritt: uppdatera GraphAPI toosend anpassade fält

En lista över säkerhetstoken som Azure AD skickar för autentisering, se [Azure AD tokenreferens](./develop/active-directory-token-and-claims.md). Om du behöver ett anpassat anspråk som skickar andra token kan du använda GraphAPI tooset hello app fältet *acceptMappedClaims* för**SANT**. Du kan använda Azure AD Graph-Utforskaren eller MS Graph toomake den här konfigurationen. 

Det här exemplet använder diagram Explorer:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a>Hämta PingAccess och konfigurera din app

Nu när du har slutfört alla steg i hello Azure Active Directory-installationen, kan du gå vidare tooconfiguring PingAccess. 

hello detaljerade anvisningar för hello PingAccess en del av det här scenariot fortsätter i hello Ping Identity-dokumentation [konfigurera PingAccess för Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Dessa steg beskriver hur du hello processen för att skaffa en PingAccess-konto om du inte redan har ett, installera hello PingAccess Server och skapa en Azure AD OIDC leverantörsanslutning med hello katalog-ID som du kopierade från hello Azure-portalen. Sedan kan du använda hello program-ID och nyckel värden toocreate en webbsessionen på PingAccess. Sedan kan du konfigurera identitetsmappning och skapa en virtuell värd, plats och program.

### <a name="test-your-app"></a>Testa din app

När du har slutfört de här stegen kan ska din app vara igång. tootest, öppna en webbläsare och gå toohello externa URL: en som du skapade när du har publicerat hello app i Azure. Logga in med hello testkonto du tilldelade toohello appen.

## <a name="next-steps"></a>Nästa steg

- [Konfigurera PingAccess för Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Hur tillhandahåller Azure AD Application Proxy enkel inloggning?](application-proxy-sso-overview.md)
- [Felsöka Application Proxy](active-directory-application-proxy-troubleshoot.md)
