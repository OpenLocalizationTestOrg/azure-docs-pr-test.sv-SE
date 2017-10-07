---
title: aaaPublish appar med Azure AD Application Proxy | Microsoft Docs
description: Publicera lokala program toohello moln med Azure AD Application Proxy i hello klassiska portalen.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 7926998314c65521ae48aebcceb33cb0c67e0b87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publicera program med Azure AD Application Proxy

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-application-proxy-publish.md)

Azure AD Application Proxy hjälper dig att stöd för fjärranvändare genom att publicera lokala program toobe nås via hello internet. Med den här punkten, bör du redan har [aktiverade Application Proxy i hello klassiska Azure-portalen](active-directory-application-proxy-enable.md). Den här artikeln vägleder dig genom hello steg toopublish program som körs på nätverket och ge säker fjärråtkomst utanför nätverket. När du har slutfört den här artikeln kommer du att redo tooconfigure hello program med personlig information eller säkerhetskrav.

> [!NOTE]
> Application Proxy är en funktion som är bara tillgängligt om du har uppgraderat toohello Premium eller Basic-versionen av Azure Active Directory. Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md). Om du vill toouse Application Proxy kan du [publicera program i hello Azure-portalen](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-hello-wizard"></a>Publicera en app med hello-guiden
1. Logga in som administratör i hello [klassiska Azure-portalen](https://manage.windowsazure.com/).
2. Gå tooActive Directory och välj hello katalog där du aktiverade Application Proxy.
   
    ![Active Directory – ikon](./media/active-directory-application-proxy-publish/ad_icon.png)
3. Klicka på hello **program** fliken och klicka sedan på hello **Lägg till** knappen längst ned hello hello-skärmen
   
    ![Lägga till ett program](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. Välj **Publicera ett program som ska vara tillgängligt utanför ditt nätverk**.
   
    ![Publicera ett program som ska vara tillgängligt utanför ditt nätverk](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Ange följande information om programmet hello:
   
   * **Namnet**: hello användarvänligt namn för ditt program. Det måste vara unikt i din katalog.
   * **Intern URL**: hello-adress som hello Application Proxy Connector använder tooaccess hello programmet inifrån ditt privata nätverk. Du kan ange en specifik sökväg på hello backend-servern toopublish medan hello resten av hello-servern är opublicerad. På så sätt kan du publicera olika platser på hello samma server och ge vart och ett eget namn och regler.
     
     > [!TIP]
     > Om du publicerar en sökväg, kontrollerar du att den innehåller alla nödvändiga hello-avbildningar, skript och formatmallar för ditt program. Till exempel om din app på https://yourapp/app och använder avbildningar som finns på https://yourapp/media, bör du publicera https://yourapp/ som hello sökväg.
     > 
     > 
   * **Förautentiseringsmetod**: hur Application Proxy verifierar användare innan du ger dem åtkomst tooyour program. Välj något av alternativen hello hello nedrullningsbara menyn.
     
     * Azure Active Directory: Application Proxy dirigerar användarna toosign med Azure AD, som autentiserar deras behörigheter för hello katalog och program.
     * Genomströmning: Användarna har inte tooauthenticate tooaccess hello program.
     
     ![Egenskaper för program](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. toofinish hello guiden klickar du på hello bockmarkeringen längst ned hello hello-skärmen. Nu har programmet hello definierats i Azure AD.

## <a name="assign-users-and-groups-toohello-application"></a>Tilldela användare och grupper toohello program
I ordning för dina användare tooaccess ditt publicerade program måste du tooassign dem antingen individuellt eller i grupper. (Kom ihåg tooassign själv åtkomst för.) Varje användare som du tilldelar behöver en licens för Azure Basic eller högre. Du kan tilldela licenser individuellt eller toogroups. Mer information finns i [och tilldela användare tooan programmet](active-directory-applications-guiding-developers-assigning-users.md). 

För appar som kräver förautentisering ger tilldela en användare behörighet toouse hello program. För appar som inte kräver förautentisering, tilldela en användare innebär hello användaren kan komma åt programmet hello via hello åtkomstpanelen.

1. Efter färdigställa hello Lägg till App-guiden kan se du hello Snabbstart för ditt program. toomanage som har åtkomst toohello appen, Välj **användare och grupper**.
   
    ![Snabbstart för att tilldela användare för Application Proxy – skärmbild](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Sök efter specifika grupper i katalogen, eller visa alla användare. toodisplay hello sökresultaten klickar du på hello är markerat.
   
      ![Söka efter grupper eller användare – skärmbild](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Välj alla användare eller grupp som du vill tooassign toothis appen och klicka på **tilldela**. Du tillfrågas tooconfirm den här åtgärden.

> [!NOTE]
> För appar med integrerad Windows-autentisering kan du endast tilldela användare och grupper som synkroniseras från din lokala Active Directory. Användare som loggar in med ett Microsoft-konto och gäster kan inte tilldelas appar som publiceras med Azure Active Directory Application Proxy. Kontrollera att dina användare logga in med autentiseringsuppgifterna som ingår i hello samma domän som hello-app som du publicerar.
> 
> 

## <a name="test-your-published-application"></a>Testa ditt publicerade program
När du har publicerat programmet kan testa du den genom att gå toohello URL som du har publicerat. Kontrollera att du har åtkomst till det, att det återges korrekt och att allt fungerar som förväntat. Om du har problem med eller får ett felmeddelande, försök hello [felsökningsguide för](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurera ditt program
Du kan ändra publicerade appar eller konfigurera avancerade alternativ på sidan för hello konfigurera. Du kan anpassa din app genom att ändra hello namn eller ladda upp en logotyp på den här sidan. Du kan också hantera åtkomstregler som hello förautentiseringsmetoden eller Multi-Factor authentication.

![Avancerad konfiguration](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

När du publicerar program med hjälp av Azure Active Directory Application Proxy, de visas i listan med program hello i Azure AD och du kan hantera dem där.

Om du inaktiverar Application Proxy-tjänster när du har publicerat program hello program inte längre tillgängligt utanför ditt privata nätverk. Användarna kan fortfarande åtkomst hello program lokalt som vanligt.

tooview ett program och se till att den är tillgänglig genom att dubbelklicka på hello hello programmets namn. Om hello Application Proxy-tjänsten är inaktiverad och programmet hello är inte tillgängligt, visas ett varningsmeddelande hello överst i hello-skärmen.

toodelete ett program, Välj ett program i hello listan och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg
* [Publicera program med ditt domännamn](active-directory-application-proxy-custom-domains.md)
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Aktivera villkorlig åtkomst](active-directory-application-proxy-conditional-access.md)
* [Arbeta med anspråksmedvetna program](active-directory-application-proxy-claims-aware-apps.md)

Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)

