---
title: 'Azure Active Directory B2C: Hot management | Microsoft Docs'
description: "Läs mer om identifiering och minskning tekniker för denial of service-attacker och lösenord attacker i Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 60bc0cc392b332cc4e9741ddb97dfa58e68ed420
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Threat management

Threat management innehåller planering för skydd mot attacker mot ditt system och nätverk. Denial of service-attacker kan göra resurser tillgängliga toointended användare. Lösenord attacker lead toounauthorized åtkomst tooresources. Azure Active Directory B2C (Azure AD B2C) har inbyggda funktioner som kan hjälpa dig att skydda dina data mot dessa hot på flera olika sätt.

## <a name="denial-of-service-attacks"></a>Denial of service-attacker

Azure AD B2C använder tekniker för identifiering och lösning som SYN cookies och hastighet och anslutning gränser tooprotect underliggande resurser mot denial of service-attacker.

## <a name="password-attacks"></a>Lösenord attacker

Azure AD B2C har också minskning tekniker för lösenord attacker. Lösning innehåller lösenord brute force-attacker och lösenord för ordlisteattacker. Lösenord som anges av användarna är obligatoriska toobe rimligen komplexa. Med hjälp av olika signaler analyserar Azure AD B2C hello integriteten för begäranden. Azure AD B2C är utformad toointelligently skilja avsedda användarna från hackare och botnät. Azure AD B2C ger en strategi för avancerade toolock konton baserat på hello-lösenord som angetts i hello sannolikheten för en attack.

Mer information finns i hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
