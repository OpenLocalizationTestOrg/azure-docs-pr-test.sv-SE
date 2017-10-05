---
title: Anslut en personlig enhet till din organisation | Microsoft Docs
description: "Beskriver hur användarna kan registrera sina personliga enheter för Windows 10 till företagsnätverket och innehåller stegen för distributionen för en BYOD-scenario."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 9f3d38f5-1cfd-43d4-97da-4fed1255a1ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 9418365ea18b065551448742b21c8b17a1749fc8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-personal-device-to-your-organization"></a>Anslut en personlig enhet till din organisation
## <a name="to-join-a-windows-10-device-to-your-organization"></a>Att ansluta till en Windows 10-enhet till din organisation
1. Från den **starta** väljer du **inställningar**.
2. Välj **konton**, och klicka sedan på **ditt konto**.
3. Klicka på **lägga till arbetet eller skolan konto**, och skriv sedan i ditt organisationskonto.
4. Ange ditt användarnamn och lösenord på sidan logga in för din organisation och klicka sedan på **OK**.
5. Du uppmanas att en utmaning för multifaktorautentisering. (Den här utmaningen kan konfigureras av en IT-administratör.)
6. Azure Active Directory (AD Azure) kontrollerar om enheten kräver registrering för hantering av mobila enheter.
7. Windows registrerar enheten i organisationens katalog i Azure AD och registrerar den i hantering av mobila enheter.
8. Om du använder en hanterad går Windows du till skrivbordet via automatisk inloggning.
9. Om du är en federerad användare kommer du tas till en Windows-inloggning skärm att ange dina autentiseringsuppgifter.

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för företaget: Sätt att använda enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

