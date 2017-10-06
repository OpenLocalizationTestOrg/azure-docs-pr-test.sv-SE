---
title: "aaaHow tooconfigure säkerhetsaviseringar | Microsoft Docs"
description: "Lär dig hur tooconfigure säkerhetsvarningar för Azure Privileged Identity Management-tillägg."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4e0c911a-36c6-42a0-8f79-a01c03d2d04f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 1b3c4a7d36fa3f81bb3fe2574d675fdf0ab34909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-security-alerts-in-azure-ad-privileged-identity-management"></a>Hur tooconfigure säkerhet aviseringar i Azure AD Privileged Identity Management
## <a name="security-alerts"></a>Säkerhetsaviseringar
Azure Privileged Identity Management (PIM) genererar aviseringar när det är misstänkt eller unsafe aktivitet i din miljö. När en avisering utlöses visas den på hello PIM-instrumentpanelen. Välj hello avisering toosee en rapport som visar hello användare eller roller som utlöste aviseringen hello.

![PIM instrumentpanelen säkerhetsaviseringar – skärmbild][1]

| Varning | Utlösare | Rekommendation |
| --- | --- | --- |
| **Roller tilldelas utanför PIM** |En administratör har permanent tilldelade tooa roll utanför hello PIM-gränssnittet. |Granska hello ny rolltilldelning. Eftersom andra tjänster kan endast tilldelas permanenta administratörer, ändra den tooan berättigade tilldelning om det behövs. |
| **Roller som ska aktiveras för ofta** |Det fanns för många omaktiveringar av hello samma roll inom hello tiden som tillåts i hello inställningar. |Kontakta hello användaren toosee varför de har aktiverat hello rollen så många gånger. Kanske hello tid gränsen är för kort för dem toocomplete sina uppgifter, eller kanske de använder skript tooautomatically aktivera en roll. |
| **Roller som inte kräver autentisering på flera plan för aktivering** |Det finns roller utan MFA är aktiverat i hello inställningar. |Vi kräver MFA för hello mest mycket Privilegierade roller, men rekommenderar starkt att du aktiverar MFA för aktivering av alla roller. |
| **Administratörer som inte använder sina Privilegierade roller** |Det finns tillgängliga administratörer som inte har aktiverat sina roller nyligen. |Starta en granska toodetermine hello användare åtkomst som inte längre behöver åtkomst. |
| **Det finns för många globala administratörer** |Det finns fler globala administratörer än vad som rekommenderas. |Om du har ett stort antal globala administratörer är det troligt att användare får fler behörigheter än de behöver. Flytta användare tooless Privilegierade roller eller se några av dem tillgängliga för hello roll i stället för permanent tilldelade. |

## <a name="configure-security-alert-settings"></a>Konfigurera säkerhetsinställningar för avisering
Du kan anpassa vissa av hello säkerhetsaviseringar i PIM toowork med din miljö och säkerhetsmål. Följ dessa steg tooreach hello inställningsbladet:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) och välj hello **Azure AD Privileged Identity Management** panelen hello instrumentpanel.
2. Välj **hanteras Privilegierade roller** > **inställningar** > **aviseringar inställningar**.
   
    ![Navigera toosecurity aviseringsinställningar][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>”Roller som ska aktiveras för ofta” avisering
Den här aviseringen utlösare om en användare aktiverar hello samma Privilegierade roll flera gånger inom en angiven tidsperiod. Du kan konfigurera både hello tid period och hello antalet aktiveringar.

* **Aktivera förnyelse tidsram**: Ange i dagar, timmar, minuter och andra hello gång tidsperiod vill toouse tootrack misstänkta förnyelser.
* **Antal aktivering förnyelser**: Ange hello antalet aktiveringar, från 2 too100 som du anser vara worthy för avisering inom hello tidsintervall som du har valt. Du kan ändra den här inställningen genom att flytta skjutreglaget för hello eller ange ett värde i textrutan för hello.

### <a name="there-are-too-many-global-administrators-alert"></a>”Det finns för många globala administratörer” avisering
PIM utlöser den här aviseringen om två olika villkor är uppfyllda och du kan konfigurera båda. Du måste först tooreach ett visst tröskelvärde för globala administratörer. En viss procentandel av din totala rolltilldelningar måste dessutom vara globala administratörer. Om du bara uppfyller något av dessa mätningar visas hello aviseringen inte.  

* **Minsta antal globala administratörer**: Ange hello antal globala administratörer från 2 too100, som du anser att en osäker belopp.
* **Procentandelen globala administratörer**: Ange hello procentandelen administratörer som är globala administratörer från 0% too100%, som inte är säkert i din miljö.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>”Administratörer inte använder sina Privilegierade roller” avisering
Den här aviseringen utlöses om en användare blir mängden tid utan att aktivera en roll.

* **Antal dagar**: Ange hello antal dagar från 0 too100 som en användare kan gå utan att aktivera en roll.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
