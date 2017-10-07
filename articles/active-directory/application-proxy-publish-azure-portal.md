---
title: aaaPublish appar med Azure AD Application Proxy | Microsoft Docs
description: Publicera lokala program toohello moln med Azure AD Application Proxy i hello Azure-portalen.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ed5458467fb7d4376f65a222f1ba5f23cfdfc57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publicera program med Azure AD Application Proxy

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-application-proxy-publish.md)

Azure Active Directory (AD) Application Proxy hjälper dig att stöd för fjärranvändare genom att publicera lokala program toobe nås via hello internet. Du kan publicera programmen via hello Azure portal tooprovide säker fjärråtkomst utanför nätverket.

Den här artikeln vägleder dig genom hello steg toopublish ett lokalt program med Application Proxy. När du har slutfört den här artikeln användarna kommer att kunna tooaccess appen via fjärranslutning. Och du kan klara tooconfigure extra funktioner för programmet hello som enkel inloggning, personlig information och säkerhetskrav.

Om du är ny tooApplication Proxy, mer information om den här funktionen med hello artikel [hur säkra tooprovide fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md).


## <a name="publish-an-on-premises-app-for-remote-access"></a>Publicera en lokal app för fjärråtkomst

Följ dessa steg toopublish dina appar med Application Proxy. Om du inte har redan hämtats och konfigurerat en koppling för din organisation, gå för[komma igång med Application Proxy och installera hello connector](active-directory-application-proxy-enable.md) först och sedan publicera din app.

> [!TIP]
> Om du testar programproxy för hello första gången, väljer du ett program som har ställts in för lösenordsbaserad autentisering. Application Proxy stöder andra typer av autentisering, men lösenordsbaserade appar är hello enklaste tooget dig snabbt och köra. 

1. Logga in som administratör i hello [Azure-portalen](https://portal.azure.com/).
2. Välj **Azure Active Directory** > **företagsprogram** > **nytt program**.

  ![Lägg till ett företagsprogram](./media/application-proxy-publish-azure-portal/add-app.png)

3. Välj **alla**och välj **lokalt program**.  

  ![Lägg till ditt eget program](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Ange följande information om programmet hello:

   - **Namnet**: hello namnet på hello-program som ska visas på hello åtkomstpanelen och hello Azure-portalen. 

   - **Intern URL**: hello URL som du använder tooaccess hello programmet från i ditt privata nätverk. Du kan ange en specifik sökväg på hello backend-servern toopublish medan hello resten av hello-servern är opublicerad. På så sätt kan du publicera olika platser på hello samma server som olika appar och ge vart och ett eget namn och regler.

     > [!TIP]
     > Om du publicerar en sökväg, kontrollerar du att den innehåller alla nödvändiga hello-avbildningar, skript och formatmallar för ditt program. Till exempel om din app på https://yourapp/app och använder avbildningar som finns på https://yourapp/media, bör du publicera https://yourapp/ som hello sökväg. Intern URL: en har inte toobe hello landningssida användarna finns. Mer information finns i [ange en anpassad hemsida för publicerade appar](application-proxy-office365-app-launcher.md).

   - **Extern URL**: hello adress som användarna kommer att gå tooin ordning tooaccess hello-app från utanför nätverket. Om du inte vill toouse hello standarddomän Application Proxy kan läsa om [anpassade domäner i Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).
   - **Förautentisering**: hur Application Proxy verifierar användare innan du ger dem åtkomst tooyour program. 

     - Azure Active Directory: Application Proxy dirigerar användarna toosign med Azure AD, som autentiserar deras behörigheter för hello katalog och program. Vi rekommenderar detta alternativ som standard hello så att du kan dra nytta av säkerhetsfunktionerna i Azure AD som villkorlig åtkomst och Multi-Factor Authentication.
     - Genomströmning: Användarna har inte tooauthenticate mot Azure Active Directory tooaccess hello program. Du kan fortfarande installera autentiseringskrav på hello serverdel.
   - **Kopplingen grupp**: kopplingar hello fjärråtkomst tooyour tillämpningsprogram och connector grupper kan du organisera kopplingar och appar efter region, nätverk eller syfte. Om du inte har någon koppling grupper som skapats ännu appen tilldelas för**standard**.

   ![Konfigurera ditt program](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Om det behövs kan du konfigurera ytterligare inställningar. De flesta fall bör du behålla inställningarna i sina standardtillstånd. 
   - **Tidsgränsen för backend-programmet**: Ange ett värde för**lång** endast om ditt program är långsam tooauthenticate och ansluta. 
   - **Översätta URL: er i sidhuvuden**: behålla det här värdet som **Ja** om ditt program krävs hello ursprungliga värdhuvudet i hello autentiseringsbegäran.
   - **Översätta URL: er i programmet brödtext**: behålla det här värdet som **nr** om du har hårdkodad HTML länkar tooother lokala program och Använd inte anpassade domäner. Mer information finns i [länka översättning med Application Proxy](application-proxy-link-translation.md).
   
   ![Konfigurera ditt program](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Välj **Lägg till**.


## <a name="add-a-test-user"></a>Lägg till en testanvändare 

tootest att appen har publicerats korrekt, lägga till ett användarkonto för testet. Kontrollera att det här kontot redan har behörigheter tooaccess hello-app från företagsnätverk hello.

1. Tillbaka på hello Snabbstart-bladet välj **tilldela en användare för att testa**.

  ![Tilldela en användare för testning](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Markera på hello användare och grupper bladet **Lägg till**.

  ![Lägg till en användare eller grupp](./media/application-proxy-publish-azure-portal/add-user.png)

3. Hello Lägg till tilldelning av bladet välj **användare och grupper** Välj hello-konto som du vill tooadd. 
4. Välj **tilldela**.

## <a name="test-your-published-app"></a>Testa din publicerade app

Navigera i din webbläsare toohello extern URL som du konfigurerade under hello publicera steg. Du bör se hello startskärmen och kan toosign in med hello testkonto du ställas in.

![Testa din publicerade app](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Nästa steg
- [Hämta kopplingar](active-directory-application-proxy-enable.md) och [skapa grupper för anslutningstjänsten](active-directory-application-proxy-connectors-azure-portal.md) toopublish program på separata nätverk och platser.

- [Konfigurera enkel inloggning](application-proxy-sso-azure-portal.md) för din nyligen publicerade app
