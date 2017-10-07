---
title: "Azure AD Connect-synkronisering: hur toomanage hello Azure AD-tjänstkontot | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toorestore hello Azure AD-tjänstkontot."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, hello hur tooreset hello lösenordet för Azure AD Connect-synkronisering Connector-tjänstkontot"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e563518eae173de42a1d40bb5a76e63f29f9da42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomanage-hello-azure-ad-service-account"></a>Azure AD Connect-synkronisering: hur toomanage hello Azure AD-tjänstkonto
hello-tjänstkontot som används av hello Azure AD Connector måste toobe tjänsten gratis. Om du behöver tooreset autentiseringsuppgifterna, är det här avsnittet för dig. Om exempelvis en Global administratör har av misstag Återställ hello lösenord på hello-tjänstkontot med hjälp av PowerShell.

## <a name="reset-hello-credentials"></a>Återställ hello autentiseringsuppgifter
Om hello tjänstkontot som definierats på hello Azure AD Connector inte kan kontakta Azure AD på grund av problem med tooauthentication, kan hello lösenord återställas.

1. Logga in toohello Azure AD Connect sync-servern och starta PowerShell.
2. Kör `Add-ADSyncAADServiceAccount`.  
   ![PowerShell-cmdlet addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Ange autentiseringsuppgifter för Global administratör för Azure AD.

Denna cmdlet återställer hello lösenordet för tjänstkontot för hello och uppdatera den i Azure AD och hello Synkroniseringsmotorn.

## <a name="known-issues-these-steps-can-solve"></a>Kända problem som kan lösa de här stegen
Det här avsnittet är en lista över felen som rapporteras av kunder som korrigerades med autentiseringsuppgifter för att återställa på hello Azure AD-tjänstkontot.

- - -
Händelsen 6900  
hello-servern påträffade ett oväntat fel vid bearbetning av en lösenordsändring:  
AADSTS70002: Fel verifierar referenser. AADSTS50054: Gammalt lösenord används för autentisering.

- - -
Händelsen 659  
Fel vid hämtning av lösenord principkonfigurationen synkronisering. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Fel verifierar referenser. AADSTS50054: Gammalt lösenord används för autentisering.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)

