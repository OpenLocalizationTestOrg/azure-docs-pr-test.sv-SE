---
title: "Anpassade domäner i Azure AD Application Proxy | Microsoft Docs"
description: "Hantera anpassade domäner i Azure AD Application Proxy så att URL: en för appen är detsamma oavsett där användarna komma åt den."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6a5b7731cfd98a53f83a9882529a713381b4f848
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/05/2018
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Arbeta med anpassade domäner i Azure AD Application Proxy

När du publicerar ett program via Azure Active Directory Application Proxy kan skapa du en extern Webbadress för dina användare att gå till när de arbetar via fjärranslutning. Denna URL hämtar standarddomänen *yourtenant.msappproxy.net*. Till exempel om du har publicerat en app med namnet kostnader och din klient namnet Contoso, den externa URL: en skulle vara https://expenses-contoso.msappproxy.net. Om du vill använda ditt eget domännamn kan du konfigurera en anpassad domän för ditt program. 

Vi rekommenderar att du konfigurerar anpassade domäner för dina program när det är möjligt. Några av fördelarna med anpassade domäner är:

- Dina användare får åtkomst till programmet med samma URL, oavsett om de arbetar i eller utanför nätverket.
- Om alla program har samma interna och externa URL: er, och sedan länkar i ett program som pekar på en annan fortsätter att fungera även utanför företagsnätverket. 
- Du styr din företagsanpassning och URL: er som du vill skapa. 


## <a name="configure-a-custom-domain"></a>Konfigurera en anpassad domän

### <a name="prerequisites"></a>Förutsättningar

Innan du konfigurerar en anpassad domän måste du kontrollera att du har följande krav förberedd: 
- En [verifierade domän som lagts till i Azure Active Directory](active-directory-domains-add-azure-portal.md).
- En anpassad certifikatet för domänen, i form av en PFX-fil. 
- En lokal app [som publicerats via Application Proxy](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Konfigurera den anpassade domänen

När du har dessa tre krav som är redo följer du dessa steg för att ställa in den anpassade domänen:

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Gå till **Azure Active Directory** > **företagsprogram** > **alla program** och välj den app som du vill hantera.
3. Välj **programproxy**. 
4. Använd den nedrullningsbara listan för att välja den anpassade domänen i extern URL-fältet. Om du inte ser din domän i listan, har inte sedan den verifierats ännu. 
5. Välj **spara**
5. Den **certifikat** fält som har inaktiverats aktiveras. Välj det här fältet. 

   ![Klicka om du vill överföra ett certifikat](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Om du redan har överfört ett certifikat för den här domänen visar certifikatfältet certifikatinformationen. 

6. Ladda upp PFX-certifikat och ange lösenordet för certifikatet. 
7. Välj **spara** att spara ändringarna. 
8. Lägg till en [DNS-post](../dns/dns-operations-recordsets-portal.md) som omdirigerar nya externa URL: en till domänen msappproxy.net. 

>[!TIP] 
>Du behöver bara ladda upp ett certifikat per domän. När du överföra ett certifikat kan du välja den anpassade domänen när du publicerar en ny app och inte behöver göra ytterligare konfigurationsinställningar förutom DNS-posten. 

## <a name="manage-certificates"></a>Hantera certifikat

### <a name="certificate-format"></a>Certifikatets format
Det finns ingen begränsning för certifikatet signatur metoder. Elliptic Curve Cryptography (ECC), alternativt namn på CERTIFIKATMOTTAGARE och andra vanliga typer av certifikat kan användas. 

Du kan använda ett jokerteckencertifikat så länge som jokertecken matchar den önskade externa URL: en. 

### <a name="changing-the-domain"></a>Ändra domänen
Alla verifierade domäner visas i listrutan externa URL: en för ditt program. Om du vill ändra domänen bara uppdatera fältet för programmet. Om det inte finns i listan med den domän som du vill [Lägg till den som en verifierad domän](active-directory-domains-add-azure-portal.md). Om du väljer en domän som inte har ett associerat certifikat ännu Följ steg 5 – 7 för att lägga till certifikatet. Kontrollera sedan att du uppdaterar DNS-post för att omdirigera från en ny extern URL. 

### <a name="certificate-management"></a>Certifikathantering
Du kan använda samma certifikat för flera program såvida programmen dela en extern värd. 

Du får en varning när certifikatet upphör att gälla, talar om att överföra ett annat certifikat via portalen. Certifikatet har återkallats användarna ser en säkerhetsvarning vid åtkomst till programmet. Vi utföra inte återkallelsekontroller för certifikat.  Om du vill uppdatera certifikatet för ett visst program, navigera till programmet och följ steg 5 – 7 för att konfigurera anpassade domäner på publicerade program för att överföra ett nytt certifikat. Om det gamla certifikatet inte används av andra program, raderas det automatiskt. 

Alla certifikathantering är för närvarande via enskilda programsidor så du behöver hantera certifikat i kontexten för relevant program. 

## <a name="next-steps"></a>Nästa steg
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md) till publicerade appar med Azure AD-autentisering.
* [Aktivera villkorlig åtkomst](application-proxy-enable-remote-access-sharepoint.md) till publicerade appar.
* [Lägga till ett eget domännamn i Azure AD](active-directory-domains-add-azure-portal.md)


