---
title: "aaaHow toomanage produktaktivering rollinställningar | Microsoft Docs"
description: "Lär dig hur toochange hello standardinställningarna för privilegierade identiteter med hello Azure Active Directory Privileged Identity Management – tillägget."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 453bb6f8f8e0fd2598cb073ef86c1dd55458dc72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Hur toomanage produktaktivering rollinställningar i Azure AD Privileged Identity Management
En administratör av Privilegierade roller kan anpassa Azure AD Privileged Identity Management (PIM) i organisationen, inklusive ändra hello upplevelse för användare som aktivera en berättigad rolltilldelning.

## <a name="manage-hello-role-activation-settings"></a>Hantera hello produktaktivering rollinställningar
1. Gå toohello [Azure-portalen](https://portal.azure.com) och välj hello **Azure AD Privileged Identity Management** app hello instrumentpanel.
2. Välj **hantera Privilegierade roller** > **inställningar** > **Privilegierade roller**.
3. Välj hello roll vars inställningar du vill toomanage.

Det finns ett antal inställningar som du kan konfigurera hello på sidan Inställningar för varje roll. Dessa inställningar påverkar endast användare som är berättigade administratörer, inte permanenta administratörer.

**Aktiveringar**: hello tiden, i timmar, som en roll förblir aktiv innan den upphör. Detta kan vara mellan 1 och 72 timmar.

**Meddelanden**: du kan välja huruvida hello skickas e-postmeddelanden tooadmins bekräftar att de har aktiverat en roll. Detta kan vara användbart för att upptäcka otillåten eller otillåtna aktiveringar.

**Incident/begäran biljett**: du kan välja huruvida toorequire berättigade administratörer tooinclude en biljett number när de aktiverar rollen. Detta kan vara användbart när du utför roll åtkomst granskningar.

**Multifaktorautentisering**: du kan välja huruvida toorequire användare tooverify sin identitet med MFA innan de kan aktivera deras roller. De har bara tooverify detta en gång per session inte varje gång de aktivera en roll. Det finns två tips tookeep i åtanke när du aktiverar MFA:

* Användare som har Microsoft-konton för sina e-postadresser (vanligtvis @outlook.com, men inte alltid) kan inte registrera dig för Azure MFA. Om du vill tooassign roller toousers med Microsoft-konton, bör du göra dem permanenta administratörer eller inaktivera MFA för rollen.
* Du kan inte inaktivera MFA för mycket Privilegierade roller för Azure AD och Office 365. Det här är en säkerhetsfunktion eftersom dessa roller ska skyddas noggrant:  
  
  * Programadministratör
  * Programmet proxyserverns administratör
  * Faktureringsadministratör  
  * Kompatibilitet administratör  
  * CRM-tjänstadministratör
  * Kunden LockBox åtkomst godkännare
  * Directory-skrivare  
  * Exchange-administratören  
  * Global administratör
  * Intune-tjänstadministratören
  * Administratör för postlåda  
  * Support för partner tier1  
  * Support för partner tier2  
  * Administratör av Privilegierade roller   
  * Säkerhetsadministratör  
  * SharePoint-administratör  
  * Skype for Business-administratör  
  * Kontoadministratör för användaren  

Läs mer om hur du använder MFA med PIM [hur tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what hello temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

