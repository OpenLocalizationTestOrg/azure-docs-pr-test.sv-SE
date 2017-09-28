---
title: Sipi Testfilen | Microsoft Docs
description: "Testa filen för att kontrollera ReadyForTest beroenden"
services: active-directory-b2c
documentationcenter: 
author: Sipi
manager: Sipi
editor: Sipi
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f68
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: Sipi
ms.openlocfilehash: 871d58818dcbaee5f7a5f07c19e2297ec6459a6f
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2017
---
# <a name="sipi-test-file"></a>Sipi Testfilen

Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter. När du är färdig är programmet klart att användas i Azure B2C-klientorganisationen.

## <a name="prerequisites"></a>Krav

Om du vill skapa ett program som accepterar registrering och inloggning av konsumenter måste du först registrera programmet med en Azure Active Directory B2C-klient. Skaffa en egen klient genom att följa stegen i [Skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).

Program som har skapats från Azure AD B2C-bladet i Azure Portal måste hanteras från samma plats. Om du redigerar B2C-program med hjälp av PowerShell eller någon annan portal, stöds de inte och fungerar inte med Azure AD B2C. Mer information finns i avsnittet [felaktiga appar](#faulted-apps). 

## <a name="navigate-to-b2c-settings"></a>Gå till B2C-inställningar

Logga in på [Azure Portal](https://portal.azure.com/) som global administratör för B2C-klientorganisationen. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

Välj nästa steg baserat på vilken typ av program du registrerar:
