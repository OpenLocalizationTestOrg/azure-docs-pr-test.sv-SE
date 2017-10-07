---
title: "medgivandetext för aaaUnexpected när du loggar in tooan program | Microsoft Docs"
description: "Hur tootroubleshoot när en användare ser en fråga om medgivande för ett program du har integrerat med Azure AD som du inte förväntar dig"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 32b7a5e6256faee0dcfe2e1644da3d3428812d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-tooan-application"></a>Oväntat medgivandetext när du loggar in tooan program

Många program som integreras med Azure Active Directory kräver behörighet toovarious resurser i ordning toorun. När resurserna är även integrerad med Azure Active Directory, medgivande framework behörigheter tooaccess begärs dem med hjälp av hello Azure AD. 

Detta resulterar i en fråga om medgivande visas hello första gången ett program används, vilket ofta är en gång. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Scenarier som användarna ser medgivande prompter

Fler meddelanden kan förväntas i olika scenarier:

* hello uppsättning behörigheter som krävs av programmet hello har ändrats.

* hello-användare som ursprungligen godkänt toohello programmet var inte en administratör och nu en annan (icke-administratörer) användare använder hello program för hello första gången.

* hello-användare som godkänt toohello programmet ursprungligen var en administratör, men de samtycker inte på uppdrag av hello hela organisationen.

* med hjälp av programmet hello [inkrementell och dynamiska medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest ytterligare behörighet efter medgivande beviljats från början. Det här används ofta när valfria funktioner i ett program ytterligare kräver behörigheter utöver de som krävs för grundläggande funktioner.

* Medgivande återkallades efter beviljas från början.

* hello utvecklare har konfigurerat hello programmet toorequire en fråga om medgivande varje gång den används (Obs: Detta är inte bästa praxis).

## <a name="next-steps"></a>Nästa steg

-   [Appar, behörigheter och medgivande i Azure Active Directory (v1.0 slutpunkten)](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Scope, behörigheter och medgivande i hello Azure Active Directory (v2.0-slutpunkten)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


