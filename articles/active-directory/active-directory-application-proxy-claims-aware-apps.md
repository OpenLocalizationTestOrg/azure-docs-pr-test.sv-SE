---
title: "Anspråksmedvetna appar – Azure AD App Proxy | Microsoft Docs"
description: "Så här publicerar du en lokal ASP.NET-program som accepterar AD FS-anspråk för säker fjärråtkomst av användare."
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
ms.openlocfilehash: 5784222608b01509fc4ff84b1a8792cbcfea89e6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Arbeta med anspråksmedvetna appar i Application Proxy
[Anspråksmedvetna appar](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) utför en omdirigering till den säkerhet säkerhetstokentjänst (STS). STS begär autentiseringsuppgifter från användare mot en token och omdirigeras användaren till programmet. Det finns några sätt att aktivera Application Proxy ska fungera med dessa omdirigeringar. Använd den här artikeln för att konfigurera distributionen för anspråksmedvetna program. 

## <a name="prerequisites"></a>Krav
Se till att STS som appen anspråksmedvetna omdirigerar till är tillgängligt utanför ditt lokala nätverk. Du kan göra STS tillgängliga genom att exponera den via en proxyserver eller genom att tillåta externa anslutningar. 

## <a name="publish-your-application"></a>Publicera programmet

1. Publicera programmet enligt instruktionerna i [publicera program med programproxy](application-proxy-publish-azure-portal.md).
2. Gå till sidan program i portalen och välj **enkel inloggning**.
3. Om du väljer **Azure Active Directory** som din **förautentisering metoden**väljer **Azure AD enkel inloggning inaktiverat** som din **internt Autentiseringsmetod**. Om du väljer **Passthrough** som din **förautentisering metoden**, behöver du inte ändra något.

## <a name="configure-adfs"></a>Konfigurera AD FS

Du kan konfigurera AD FS för anspråksmedvetna program i ett av två sätt. Först är att använda anpassade domäner. Andra är med WS-Federation. 

### <a name="option-1-custom-domains"></a>Alternativ 1: Anpassade domäner

Om alla interna URL: er för dina program är fullständigt kvalificerade domännamn (FQDN), så du kan konfigurera [anpassade domäner](active-directory-application-proxy-custom-domains.md) för dina program. Använd anpassade domäner för att skapa externa URL: er som är samma som de interna URL: er. När de externa URL: er matchar din interna URL: er, fungerar STS omdirigeringar om användarna är lokala eller fjärranslutna. 

### <a name="option-2-ws-federation"></a>Alternativ 2: WS-Federation

1. Öppna AD FS-hantering.
2. Gå till **förtroende för förlitande part**, högerklicka på appen som du publicerar med Application Proxy och välj **egenskaper**.  

   ![Förtroenden för förlitande part högerklickar du på appens namn - skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. På den **slutpunkter** fliken, under **slutpunktstyp**väljer **WS-Federation**.
4. Under **betrodda URL**, ange den URL som du angav i programproxy under **externa URL: en** och på **OK**.  

   ![Lägga till en slutpunkt - värdet betrodda URL – skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Nästa steg
* [Aktivera enkel inloggning på](application-proxy-sso-overview.md) för program som inte är anspråksmedvetna
* [Aktivera ursprunglig klientappar att interagera med proxy-program](active-directory-application-proxy-native-client.md)


