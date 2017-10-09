---
title: "aaaHow tooconfigure enkel inloggning tooan programmet på Application Proxy | Microsoft Docs"
description: Hur du kan konfigurera enkel inloggning tooyour proxy program snabbt
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: e1289203177c77b3a8bcc9058c5c0b8ae50f243e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-single-sign-on-tooan-application-proxy-application"></a>Hur tooconfigure enkel inloggning tooan Application Proxy-program

Enkel inloggning (SSO) låter du dina användare tooaccess ett program utan autentiseras flera gånger. Det kan hello enkel autentisering toooccur i hello molnet mot Azure Active Directory och hello tjänsten eller koppling tooimpersonate hello användaren toocomplete eventuella ytterligare autentiseringsfrågor från hello program.

## <a name="how-tooconfigure-single-sign-on"></a>Hur tooconfigure enkel inloggning på
tooconfigure SSO, kontrollera först att programmet har konfigurerats för förautentisering via Azure Active Directory. toodo detta, går för**Azure Active Directory**  - &gt; **företagsprogram**  - &gt; **alla program**  - &gt; Programmet  **- &gt; Application Proxy**. På den här sidan kan du se hello ”förautentisering” fältet och se till att som anges för ”Azure Active Directory. 

Mer information om hello före autentiseringsmetoder finns i steg fyra i hello [app publishing dokumentet](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![Före autentiseringsmetod i Azure Portal](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Konfigurera enkel inloggning lägen för Application Proxy-program
Därefter konfigurera vi hello viss typ av enkel inloggning. hello inloggning metoder klassificeras baserat på vilken typ av autentisering hello backend-programmet används. Appen Proxy program stöder tre typer av inloggning:

-   **Lösenordsbaserade inloggning**: lösenordsbaserade inloggning kan användas för alla program som använder fälten användarnamn och lösenord toosign på. Konfigurationssteg finns i vår [lösenord SSO configuration dokumentationen](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Integrerad Windows-autentisering**: för program som använder integrerad autentisering IWA (Windows), enkel inloggning är aktiverat via Kerberos-begränsad delegering (KCD). Detta ger Application Proxy Connectors behörighet i Active Directory tooimpersonate användare och toosend och ta emot token åt. Information om hur du konfigurerar KCD finns i hello [enkel inloggning med KCD dokumentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Rubrik-baserade inloggning**: huvud-baserade inloggning aktiveras genom ett partnerskap och kräver ytterligare konfiguration. Mer information om hello partnerskap och stegvisa instruktioner för hur du konfigurerar enkel inloggning tooan program som använder huvuden för autentisering finns hello [PingAccess för Azure AD-dokumentationen](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Dessa alternativ kan hittas genom att gå tooyour programmet i ”företagsprogram” och öppna hello **enkel inloggning** sida på hello vänstra menyn. Observera att om programmet har skapats i hello gamla portalen kan du inte kanske ser alla dessa alternativ.

På den här sidan kan du också se en ytterligare Sign-On-alternativet: länkade inloggning. Detta stöds också av Application Proxy. Tänk på att det här alternativet inte lägger till enkel inloggning toohello program. Som säger hello program kanske redan har enkel inloggning implementeras med hjälp av en annan tjänst, till exempel Active Directory Federation Services. 

Det här alternativet kan en administratör toocreate en länk tooan program som användarna mark först för att få åtkomst till programmet hello. Om det finns ett program som är konfigurerade tooauthenticate användare som använder Active Directory Federation Services 2.0, kan en administratör till exempel använda hello ”länkade inloggning” alternativet toocreate tooit en länk på hello åtkomstpanelen.

## <a name="next-steps"></a>Nästa steg
[Tillhandahålla enkel inloggning tooyour appar med Application Proxy](active-directory-application-proxy-sso-using-kcd.md)
