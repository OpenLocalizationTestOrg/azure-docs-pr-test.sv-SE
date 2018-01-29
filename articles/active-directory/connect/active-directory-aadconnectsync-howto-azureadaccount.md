---
title: "Azure AD Connect-synkronisering: hur du hanterar Azure AD-tjänstkontot | Microsoft Docs"
description: "Det här avsnittet beskrivs hur du återställer Azure AD-tjänstkontot."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, hur du återställer lösenordet för Azure AD Connect-synkronisering Connector-tjänstkontot"
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: cfd807706ebbf0bfa6ea699129cb197f1c79db8c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect-synkronisering: hur du hanterar Azure AD-tjänstkontot
Tjänstkontot som används av Azure AD-anslutningen ska vara tjänsten gratis. Om du behöver återställa referenserna är det här avsnittet för dig. Till exempel om en Global administratör har av misstag återställa lösenordet för kontot med hjälp av PowerShell.

## <a name="reset-the-credentials"></a>Återställa autentiseringsuppgifterna
Om tjänstkontot som definierats i Azure AD Connector inte kan kontakta Azure AD på grund av autentiseringsproblem med, kan du återställa lösenordet.

1. Logga in på Azure AD Connect sync-servern och starta PowerShell.
2. Kör `Add-ADSyncAADServiceAccount`.  
   ![PowerShell-cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Ange autentiseringsuppgifter för Global administratör för Azure AD.

Denna cmdlet återställer lösenordet för kontot och uppdatera den i Azure AD och Synkroniseringsmotorn.

## <a name="known-issues-these-steps-can-solve"></a>Kända problem som kan lösa de här stegen
Det här avsnittet är en lista över felen som rapporteras av kunder som korrigerades med autentiseringsuppgifter för att återställa på Azure AD-tjänstkontot.

- - -
Händelsen 6900  
Ett oväntat fel uppstod under bearbetning av en lösenordsändring:  
AADSTS70002: Fel verifierar referenser. AADSTS50054: Gammalt lösenord används för autentisering.

- - -
Händelsen 659  
Fel vid hämtning av lösenord principkonfigurationen synkronisering. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fel verifierar referenser. AADSTS50054: Gammalt lösenord används för autentisering.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

