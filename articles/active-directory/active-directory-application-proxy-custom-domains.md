---
title: "aaaCustom domäner i Azure AD Application Proxy | Microsoft Docs"
description: "Hantera anpassade domäner i Azure AD Application Proxy hello URL: en för hello app är hello samma oavsett där användarna komma åt den."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7a433c411976077210a2435c3c087991c7430755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Arbeta med anpassade domäner i Azure AD Application Proxy

När du publicerar ett program via Azure Active Directory Application Proxy kan skapa du en extern URL för din användare toogo toowhen som de arbetar via fjärranslutning. Denna URL hämtar hello standarddomän *yourtenant.msappproxy.net*. Till exempel om du har publicerat en app med namnet kostnader och din klient namnet Contoso och hello externa URL: en skulle vara https://expenses-contoso.msappproxy.net. Om du vill toouse ditt eget domännamn kan du konfigurera en anpassad domän för ditt program. 

Vi rekommenderar att du konfigurerar anpassade domäner för dina program när det är möjligt. Några av hello fördelarna med anpassade domäner är:

- Användarna kan få toohello program med hello samma URL, oavsett om de arbetar i eller utanför nätverket.
- Om alla program har hello samma interna och externa URL: er, fortsätter länkar i ett program som pekar tooanother toowork även utanför hello företagsnätverket. 
- Du styr din företagsanpassning och skapa hello webbadresser som du vill. 


## <a name="configure-a-custom-domain"></a>Konfigurera en anpassad domän

### <a name="prerequisites"></a>Krav

Innan du konfigurerar en anpassad domän måste du kontrollera att du har hello följande krav förberedd: 
- En [verifierad domän läggs tooAzure Active Directory](active-directory-domains-add-azure-portal.md).
- Ett anpassat certifikat hello domän i hello form av en PFX-fil. 
- En lokal app [som publicerats via Application Proxy](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Konfigurera den anpassade domänen

Följ dessa steg tooset in den anpassade domänen när du har dessa tre krav som är redo:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Navigera för**Azure Active Directory** > **företagsprogram** > **alla program** och välj hello app du vill toomanage.
3. Välj **programproxy**. 
4. Använd hello nedrullningsbara listan tooselect domänen i hello extern URL-fältet. Om du inte ser din domän i listan hello har inte sedan den verifierats ännu. 
5. Välj **spara**
5. Hej **certifikat** fält som har inaktiverats aktiveras. Välj det här fältet. 

   ![Klicka på tooupload ett certifikat](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Om du redan har överfört ett certifikat för den här domänen visar hello certifikatfältet hello certifikatinformationen. 

6. Överför hello PFX-certifikat och ange hello lösenord för hello certifikat. 
7. Välj **spara** toosave ändringarna. 
8. Lägg till en [DNS-post](../dns/dns-operations-recordsets-portal.md) att omdirigeringar hello ny externa URL: en toohello msappproxy.net domän. 

>[!TIP] 
>Du behöver bara ett certifikat för tooupload per domän. När du överföra ett certifikat kan du välja hello domänen när du publicerar en ny app och inte har toodo ytterligare konfiguration förutom hello DNS-post. 

## <a name="manage-certificates"></a>Hantera certifikat

### <a name="certificate-format"></a>Certifikatets format
Det finns ingen begränsning för hello certifikat signatur metoder. Elliptic Curve Cryptography (ECC), alternativt namn på CERTIFIKATMOTTAGARE och andra vanliga typer av certifikat kan användas. 

Du kan använda ett jokerteckencertifikat så länge som hello jokertecken matchar hello önskad externa URL: en. 

Du kan använda självsignerade certifikat, samt. Om du använder en privat certifikatsutfärdare ska hello CDP (certifikat punkt distributionsplats återkallade) för hello certifikatet vara offentlig.

### <a name="changing-hello-domain"></a>Ändra hello domän
Alla verifierade domäner som visas i listrutan för hello externa URL: en för ditt program. toochange hello domänen bara uppdatering som fältet för hello program. Om det inte finns i listan hello hello-domän som du vill [Lägg till den som en verifierad domän](active-directory-domains-add-azure-portal.md). Följ steg 5 – 7 tooadd hello certifikatet om du väljer en domän som inte redan har ett associerat certifikat. Kontrollera sedan att du uppdaterar hello DNS-poster tooredirect från hello nya externa URL: en. 

### <a name="certificate-management"></a>Certifikathantering
Du kan använda samma certifikat för flera program, om inte hello program dela en extern värd hello. 

Du får en varning när certifikatet upphör att gälla, uppmanar tooupload ett annat certifikat hello-portalen. Hello-certifikatet har återkallats användarna ser en säkerhetsvarning vid åtkomst till programmet hello. Vi utföra inte återkallelsekontroller för certifikat.  tooupdate hello certifikat för ett visst program, navigera toohello program och följ steg 5 – 7 för att konfigurera anpassade domäner på publicerade program tooupload ett nytt certifikat. Om hello gammalt certifikat inte används av andra program, raderas det automatiskt. 

Alla certifikathantering är för närvarande via enskilda programsidor så du måste toomanage certifikat i hello ramen för hello relevant program. 

## <a name="next-steps"></a>Nästa steg
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md) tooyour publicerade appar med Azure AD-autentisering.
* [Aktivera villkorlig åtkomst](active-directory-application-proxy-conditional-access.md) tooyour publicerade appar.
* [Lägg till din anpassade domän namn tooAzure AD](active-directory-domains-add-azure-portal.md)


