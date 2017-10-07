---
title: aaaAuthenticate med lokala Active Directory i din Azure-app | Microsoft Docs
description: "Lär dig mer om hello olika alternativ för av branschspecifika appar i Azure App Service tooauthenticate med lokala Active Directory"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: dde6b7e6-bf6a-4fa5-8390-3a18155d21bd
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: 65bf25aaa0447fbbea7c754db55842d57e70757e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Autentisera med lokala Active Directory i din Azure-app
Den här artikeln visar hur tooauthenticate med lokala Active Directory (AD) i [Azure App Service](../app-service/app-service-value-prop-what-is.md). En Azure-program finns i hello moln, men det finns sätt tooauthenticate lokala AD-användare på ett säkert sätt. 

## <a name="authenticate-through-azure-active-directory"></a>Autentisera via Azure Active Directory
En Azure Active Directory-klient kan vara directory synkroniseras med en lokal AD. Den här metoden ger AD-användare åtkomst till din App från hello internet och autentiseras med deras lokala autentiseringsuppgifter. Azure App Service är dessutom en [nyckelfärdig lösning för den här metoden](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Du kan aktivera autentisering med en klient för directory-synkroniserade med några få klick på en knapp för din Azure-app. Den här metoden har hello följande fördelar:

* Kräver inte någon Autentiseringskod i din app. Låt Apptjänst vill hello autentiseringen åt dig och ägna tid på att tillhandahålla funktioner i din app.
* [Azure AD Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) aktiverar åtkomst till toodirectory data från din Azure-app.
* Ger enkel inloggning för[alla program som stöds av Azure Active Directory](/marketplace/active-directory/), inklusive Office 365, Dynamics CRM Online, Microsoft Intune och tusentals icke-Microsoft molnprogram. 
* Azure Active Directory stöder rollbaserad åtkomstkontroll. Du kan använda hello [Authorize(Roles="X")] mönster med minimala ändringar tooyour kod.

hur toowrite en Azure line-of-business-program som autentiserar med Azure Active Directory, se toosee [skapa en Azure line-of-business-app med Azure Active Directory-autentisering](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Autentisera via ett lokalt STS
Om du har en lokal säker token-tjänsten (STS) som Active Directory Federation Services (AD FS) kan använda du att toofederate autentisering för din Azure-app. Den här metoden passar bäst om företagets principer som förhindrar att AD-data som lagras i Azure. Observera dock hello följande:

* STS topologi måste vara distribuerad lokalt, med kostnad och hanteringen.
* Endast AD FS-administratörer kan konfigurera [förlitande part förtroenden och anspråksregler](http://technet.microsoft.com/library/dd807108.aspx), vilket kan begränsa hello utvecklaralternativ. På hello däremot det är möjligt toomanage och anpassa [anspråk](http://technet.microsoft.com/library/ee913571.aspx) på grundval av per program.
* Åtkomst till tooon lokala AD-data kräver en separat lösning via hello företagets brandvägg.

hur toowrite en Azure line-of-business-program som autentiserar med en lokal STS Se toosee [skapa Azure line-of-business-program med AD FS-autentisering](web-sites-dotnet-lob-application-adfs.md).

