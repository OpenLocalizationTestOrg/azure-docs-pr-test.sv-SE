---
title: "aaaAdministrative enheter management Förhandsgranska i Azure Active Directory"
description: "Med hjälp av administrativa enheter för mer detaljerade delegeringen av behörigheter i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: ee2c7beb6f9f6292bbf3cdeab00801ac066ae0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Hantering av administrativa enheter i Azure AD - förhandsversion
Den här artikeln beskriver administrativa enheter – en ny Azure Active Directory-behållare för resurser som kan användas för att delegera administrativa behörigheter över delmängder av användare och tillämpa principer tooa underuppsättning med användare. I Azure Active Directory aktivera administrativa enheter centrala administratörer toodelegate behörigheter tooregional administratörer eller tooset princip på en detaljerad nivå.

Detta är användbart i organisationer med oberoende avdelningar, till exempel en stor university som består av många autonoma skolorna (Business skola, ingenjörer skola och så vidare) som är oberoende av varandra. Sådana avdelningar har sina egna IT-administratörer som styr åtkomsten, hantera användare och ange principer för fördelningen. Central Administratörer vill toobe kan bevilja behörigheterna dig administratörer via hello användare i deras specifika avdelningar. Mer specifikt kan i det här exemplet administratör av en central, till exempel skapa en administrativ enhet för en viss skola (Business skolan) och fylla det med bara hello skola företagsanvändare. Sedan en central administratör kan lägga till hello Business skola IT-personal tooa omfång roll, med andra ord bevilja hello IT-personal Business skola behörigheter endast med hello skola administrativa affärsenhet.

> [!IMPORTANT]
> Du kan tilldela administrativa enhet omfång administratörsroller endast om du aktiverar Azure Active Directory Premium. Mer information finns i [komma igång med Azure AD Premium](active-directory-get-started-premium.md).
>


Från hello den centrala administratörs synsätt är en administrativ enhet ett katalogobjekt som kan skapas och fylls med resurser. **Dessa resurser kan vara endast användare i den här förhandsversionen.** När skapas och konfigureras kan hello administrativa enhet användas som ett omfång toorestrict hello behörighet endast över resurser som ingår i hello administrativa enhet.

## <a name="managing-administrative-units"></a>Hantera administrativa enheter
I den här förhandsversionen kan du skapa och hantera administrativa enheter med hello Azure Active Directory-modulen för Windows PowerShell-cmdlets. Mer om hur toolearn toodo som finns i [arbeta med administrativa enheter](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Mer information om programvarukrav och installera hello Azure AD-modulen och information om hello Azure AD-modulen cmdletar för att hantera administrativa enheter, inklusive syntax och beskrivningar av parametern exemplen finns [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Nästa steg
[Azure Active Directory-versioner](active-directory-editions.md)
