---
title: aaaJoin en personlig enhet tooyour organisation | Microsoft Docs
description: "Beskriver hur användarna kan registrera sina personliga Windows 10 enheter tootheir företagsnätverket och innehåller distributionsstegen för en BYOD-scenario."
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
ms.openlocfilehash: 979e2461dd9ad0438aa402a0a3223cc84c4c625c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-personal-device-tooyour-organization"></a>Ansluta till en personlig enhet tooyour organisation
## <a name="toojoin-a-windows-10-device-tooyour-organization"></a>toojoin Windows 10-enhet tooyour organisation
1. Från hello **starta** väljer du **inställningar**.
2. Välj **konton**, och klicka sedan på **ditt konto**.
3. Klicka på **lägga till arbetet eller skolan konto**, och skriv sedan i ditt organisationskonto.
4. Ange ditt användarnamn och lösenord på hello-inloggningssida för din organisation, och klicka sedan på **OK**.
5. Du uppmanas att en utmaning för multifaktorautentisering. (Den här utmaningen kan konfigureras av en IT-administratör.)
6. Azure Active Directory (AD Azure) kontrollerar om hello enheten kräver registrering för hantering av mobila enheter.
7. Windows registrerar hello enheten i hello organisations katalog i Azure AD och registrerar den i hantering av mobila enheter.
8. Om du använder en hanterad, tar Windows toohello via hello automatisk inloggning.
9. Om du är en federerad användare kan du kommer att vidtas tooa Windows-inloggning skärmen tooenter dina autentiseringsuppgifter.

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

