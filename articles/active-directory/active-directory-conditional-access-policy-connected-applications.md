---
title: "principer för aaaConfigure Azure Active Directory enhetsbaserad villkorlig åtkomst | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure Active Directory enhetsbaserad villkorliga åtkomstprinciper."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc5923b8ccee4335442c17ef63b2ee6f098e104e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Konfigurera principer för Azure Active Directory enhetsbaserad villkorlig åtkomst

Med [villkorlig åtkomst i Azure Active Directory (AD Azure)](active-directory-conditional-access-azure-portal.md), du kan finjustera hur behöriga användare kan komma åt dina resurser. Till exempel begränsa hello åtkomst toocertain resurser tootrusted enheter. En princip för villkorlig åtkomst som kräver en betrodd enhet kallas även för principen för enhetsbaserad villkorlig åtkomst.

Det här avsnittet ger information om hur tooconfigure enhetsbaserad villkorliga åtkomstprinciper för Azure AD-anslutna program. 


## <a name="before-you-begin"></a>Innan du börjar

Enhetsbaserad villkorlig åtkomst ties **villkorlig åtkomst i Azure AD** och **Azure AD-enhetshantering tillsammans**. Om du inte är bekant med något av dessa områden ännu, bör du läsa hello följande ämnen, först:

- **[Villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)**  -det här avsnittet ger dig en översikt över villkorlig åtkomst och hello relaterade termer.

- **[Introduktion toodevice management i Azure Active Directory](device-management-introduction.md)**  -det här avsnittet ger en översikt av hello olika alternativ du har tooconnect enheter med Azure AD. 


## <a name="trusted-devices"></a>Betrodda enheter

I en mobile första, molnet först värld kan Azure Active Directory enkel inloggning toodevices, appar och tjänster från var som helst. För vissa kanske resurser i din miljö, beviljar åtkomst toohello rätt användare inte tillräckligt bra. Dessutom toohello rätt användare, du kan även kräva en betrodd enhet toobe används tooaccess en resurs. I din miljö kan du definiera en betrodd enhet är baserad på hello följande komponenter:

- Hej [enhetsplattformar](active-directory-conditional-access-azure-portal.md#device-platforms) på en enhet
- Om en enhet är kompatibel
- Om en enhet är ansluten till domänen 

Hej [enhetsplattformar](active-directory-conditional-access-azure-portal.md#device-platforms) kännetecknas av hello-operativsystemet som körs på enheten. Du kan begränsa åtkomst toocertain resurser toospecific enhetsplattformar i principen för enhetsbaserad villkorlig åtkomst.



Du kan kräva betrodda enheter toobe som markerats som kompatibel i en princip med enhetsbaserad villkorlig åtkomst.

![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Enheter kan markeras som kompatibla i hello katalog genom att:

- Intune 
- Ett system från tredje part mobila enheter som kan integreras med Azure AD  

Endast enheter som är anslutna tooAzure AD kan markeras som kompatibel. tooconnect enhet-tooAzure Active Directory har hello följande alternativ: 

- Azure AD som registrerade
- Azure AD ansluten
- Azure AD-hybridlösning ansluten

    ![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Om du har en lokal Active Directory (AD) storleken kan överväga du att enheter som inte är anslutna tooAzure AD men kopplade tooyour AD toobe betrodda.

![Molnappar](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Nästa steg

Innan du konfigurerar en princip för enhetsbaserad villkorlig åtkomst i din miljö bör du ta en titt på hello [bästa praxis för villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-best-practices.md).

