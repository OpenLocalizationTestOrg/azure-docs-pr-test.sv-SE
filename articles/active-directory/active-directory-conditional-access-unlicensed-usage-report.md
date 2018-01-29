---
title: "Olicensierad användningsrapporten | Microsoft Docs"
description: "Olicensierad användning rapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: 8bcd814ee7b7bfc158e62ad26e6cc23781f5352d
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/16/2018
---
# <a name="unlicensed-usage-report"></a>Olicensierad användningsrapporten
Olicensierad användning rapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner. Detta gör att du kan göra bättre användning av licenser som du har köpt och för att identifiera du vet när du eventuellt ytterligare licenser. 

Rapporten visar aktiva användning av funktionerna för betalda under de senaste 30 dagarna. 

## <a name="report-structure"></a>Rapporten struktur
| Kolumnnamn | Beskrivning |
|:--- |:--- |
| Olicensierad användare |Användarens namn |
| Funktion |Funktionsnamnet. Till exempel: villkorlig åtkomst |
| Program som nås |Namnet på det program som nås med funktionen. Till exempel: Office 365 SharePoint Online |

> [!NOTE]
> Om ett användarkonto har tagits bort kolumnen 'Olicensierad User' fylls i med ett ID som 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Funktionen för villkorlig åtkomst
Ej licensierade användare flaggas när de har åtkomst till en tjänst som har principen för villkorlig åtkomst tillämpas om de inte har en Azure AD Premium-licens. 

Detta gäller för MFA / plats principer samt enheten principer som använder Intune.

## <a name="see-also"></a>Se också
* [Använda villkorlig åtkomst med Office 365 och andra Azure Active Directory anslutna appar](active-directory-conditional-access-azure-portal.md)
* [Komma igång med villkorlig åtkomst till Azure AD](active-directory-conditional-access-azure-portal-get-started.md) 

