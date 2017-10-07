---
title: "aaaUnlicensed användningsrapporten | Microsoft Docs"
description: "Hej olicensierad användningsrapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92138f43-9528-4c8a-b834-66a47da476e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: c44d1756b4641d7ca88434017eedb6c5e2567cb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unlicensed-usage-report"></a>Olicensierad användningsrapporten
Hej olicensierad användningsrapporten kan du identifiera ej licensierade användare som använder betald Azure AD-funktioner. Det innebär att du toomake bättre användning av licenser som du har köpt och tooidentify som du vet när du eventuellt ytterligare licenser. 

hello rapporten visar aktiva användning av hello betald funktioner i hello senaste 30 dagarna. 

## <a name="report-structure"></a>Rapporten struktur
| Kolumnnamn | Beskrivning |
|:--- |:--- |
| Olicensierad användare |Namnet på hello användare |
| Funktion |hello funktionsnamnet. Till exempel: villkorlig åtkomst |
| Program som nås |hello namnet på hello-program som nås med hello-funktionen. Till exempel: Office 365 SharePoint Online |

> [!NOTE]
> Om ett användarkonto har tagits bort hello 'Olicensierad User' kolumnen fylls i med ett ID som 1003000090D8B285
> 
> 

## <a name="conditional-access-feature"></a>Funktionen för villkorlig åtkomst
Ej licensierade användare flaggas när de har åtkomst till en tjänst som har principen för villkorlig åtkomst tillämpas om de inte har en Azure AD Premium-licens. 

Detta gäller tooMFA / plats principer samt enheten principer som använder Intune.

## <a name="see-also"></a>Se även
* [Använda villkorlig åtkomst med Office 365 och andra Azure Active Directory anslutna appar](active-directory-conditional-access.md)
* [Komma igång med villkorlig åtkomst tooAzure AD](active-directory-conditional-access-azuread-connected-apps.md) 

