---
title: Sipi Testfilen | Microsoft Docs
description: Testa filberoenden toocheck ReadyForTest
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
ms.openlocfilehash: afd3dc94dfb30926b316256fb06a768a391004f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sipi-test-file"></a>Sipi Testfilen

Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter. När du är klar registreras ditt program för användning i hello Azure B2C-klient.

## <a name="prerequisites"></a>Krav

toobuild ett program som accepterar konsumenten registrering och inloggning, måste du först tooregister hello program med en Azure Active Directory B2C-klient. Skaffa en egen klient genom att använda hello steg som beskrivs i [skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).

Program som har skapats från hello Azure AD B2C-bladet i hello Azure-portalen måste hanteras från hello samma plats. Om du redigerar hello B2C-program med hjälp av PowerShell eller en annan portal, blir stöds inte och fungerar inte med Azure AD B2C. Mer information finns i hello [fel appar](#faulted-apps) avsnitt. 

## <a name="navigate-toob2c-settings"></a>Navigera tooB2C inställningar

Logga in toohello [Azure-portalen](https://portal.azure.com/) som hello Global administratör för hello B2C-klienten. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

Välj nästa steg baserat på hello programtyp som du registrerar:
