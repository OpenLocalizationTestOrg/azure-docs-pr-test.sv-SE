---
title: "Åtkomst- och rapporter för Azure MFA | Microsoft Docs"
description: "Här beskrivs hur du använder funktionen Azure Multi-Factor Authentication - rapporter."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 696f4ae3cb479a208e73e53a9a9a437caeabd294
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/06/2018
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Rapporter i Azure Multi-Factor Authentication

Azure Multi-Factor Authentication innehåller flera rapporter som kan användas av du och din organisation som är tillgängliga via Azure-portalen. I följande tabell visas de tillgängliga rapporterna:

| Rapport | Plats | Beskrivning |
|:--- |:--- |:--- |
| Blockeringshistorik för användare | Azure AD > MFA-Server > blockera/avblockera användare | Visar historiken över förfrågningar om att blockera eller avblockera användare. |
| Användnings- och bedrägeri aviseringar | Azure AD > inloggningar | Innehåller information om totala användningen och Användarsammanfattning användarinformation; samt en historik över bedrägerivarningar som skickats inom det angivna datumintervallet. |
| Användning för lokala komponenter | Azure AD > MFA-Server > Aktivitetsrapport | Innehåller information om totala användningen för MFA via tillägg för NPS, AD FS, och MFA-servern. |
| Förbikopplingshistorik för användare | Azure AD > MFA-Server > engångsförbikoppling | Innehåller en historik över förfrågningar om förbikoppling Multifaktorautentisering för en användare. |
| Serverstatus | Azure AD > MFA-Server > serverstatus | Visar status för multi-Factor Authentication-servrar som är kopplade till ditt konto. |

## <a name="view-reports"></a>Visa rapporter 

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Till vänster, Välj **Azure Active Directory** > **MFA-Server**.
3. Välj den rapport som du vill visa.

   <center>![Moln](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>PowerShell-rapportering

Identifiera användare som har registrerats för MFA med hjälp av PowerShell som följer.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifiera användare som inte har registrerats för MFA med hjälp av PowerShell som följer.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Nästa steg

* [För användare](end-user/multi-factor-authentication-end-user.md)
* [Var du vill distribuera](multi-factor-authentication-get-started.md)
