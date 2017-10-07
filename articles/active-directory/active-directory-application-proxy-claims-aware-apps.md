---
title: "aaaClaims-kompatibla appar – Azure AD App Proxy | Microsoft Docs"
description: "Hur toopublish lokalt ASP.NET-program som accepterar AD FS-anspråk för säker fjärråtkomst av användare."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 7be633225de700226c7c94815eb91b3de2b61cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Arbeta med anspråksmedvetna appar i Application Proxy
[Anspråksmedvetna appar](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) utför en omdirigering toohello säkerhet säkerhetstokentjänst (STS). hello STS begär autentiseringsuppgifter från hello användare mot en token och omdirigerar hello toohello användarprogram. Det finns ett par sätt tooenable Application Proxy toowork med dessa omdirigeringar. Använd den här artikeln tooconfigure distributionen för anspråksmedvetna program. 

## <a name="prerequisites"></a>Krav
Kontrollera att hello STS som hello anspråksmedvetna app omdirigerar toois tillgängligt utanför ditt lokala nätverk. Du kan göra hello STS tillgängliga genom att exponera den via en proxyserver eller genom att låta utanför anslutningar. 

## <a name="publish-your-application"></a>Publicera programmet

1. Publicera programmet enligt instruktionerna i toohello [publicera program med programproxy](application-proxy-publish-azure-portal.md).
2. Navigera toohello program sida i hello portalen och väljer **enkel inloggning**.
3. Om du väljer **Azure Active Directory** som din **förautentisering metoden**väljer **Azure AD enkel inloggning inaktiverat** som din **internt Autentiseringsmetod**. Om du väljer **Passthrough** som din **förautentisering metoden**, behöver du inte toochange något annat.

## <a name="configure-adfs"></a>Konfigurera AD FS

Du kan konfigurera AD FS för anspråksmedvetna program i ett av två sätt. hello är först med hjälp av anpassade domäner. hello är andra med WS-Federation. 

### <a name="option-1-custom-domains"></a>Alternativ 1: Anpassade domäner

Om alla hello interna URL: er för dina program är fullständigt kvalificerade domännamn (FQDN), så du kan konfigurera [anpassade domäner](active-directory-application-proxy-custom-domains.md) för dina program. Använd hello anpassade domäner toocreate externa URL: er som är hello samma som hello interna URL: er. När de externa URL: er matchar din interna URL: er, fungerar hello STS omdirigeringar om användarna är lokala eller fjärranslutna. 

### <a name="option-2-ws-federation"></a>Alternativ 2: WS-Federation

1. Öppna AD FS-hantering.
2. Gå för**förtroende för förlitande part**, högerklicka på hello-app som du publicerar med Application Proxy och välj **egenskaper**.  

   ![Förtroenden för förlitande part högerklickar du på appens namn - skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. På hello **slutpunkter** fliken, under **slutpunktstyp**väljer **WS-Federation**.
4. Under **betrodda URL**, ange hello-URL som du angav i hello Application Proxy under **externa URL: en** och på **OK**.  

   ![Lägga till en slutpunkt - värdet betrodda URL – skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Nästa steg
* [Aktivera enkel inloggning på](application-proxy-sso-overview.md) för program som inte är anspråksmedvetna
* [Aktivera ursprunglig klient appar toointeract med proxy-program](active-directory-application-proxy-native-client.md)


