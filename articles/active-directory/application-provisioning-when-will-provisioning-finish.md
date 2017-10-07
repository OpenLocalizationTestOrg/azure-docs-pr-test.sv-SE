---
title: "aaaUser etablering tooan Azure AD-galleriet program är ta timmar eller mer | Microsoft Docs"
description: "Hur toofind reda på varför etablering tooyour programmet kan tar längre tid än väntat"
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
ms.openlocfilehash: 0658f041724c91ddd1997cc7759393b46680f13a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-provisioning-tooan-azure-ad-gallery-application-is-taking-hours-or-more"></a>Användaretablering tooan Azure AD-galleriet program är ta timmar eller mer

När du först aktiverar automatisk etablering för ett program, kan hello inledande synkronisering ta mellan 20 minuter tooseveral timmar, beroende på hello storleken på hello Azure AD-katalog och hello antalet användare i omfånget för etablering. 

Efterföljande synkroniseringar efter hello inledande synkronisering vara snabbare som hello Etablerar tjänsten lagrar vattenstämplar som representerar hello tillståndet för båda systemen efter hello inledande synkronisering, förbättra prestanda för efterföljande synkronisering.

## <a name="how-tooimprove-provisioning-performance"></a>Hur tooimprove etablering prestanda

Om hello inledande synkronisering tar flera timmar, finns det en sak som du kan göra tooimprove prestanda:

-   **Målgrupp Användarfilter.** Målgrupp filter kan du toofine finjustera hello data som hello etablering service utdrag ur Azure AD genom att filtrera bort användare baserat på specifika attributvärden. Mer information om Omfångsfilter finns [attributbaserad programmet etablering med Omfångsfilter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

## <a name="next-steps"></a>Nästa steg
[Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](active-directory-saas-app-provisioning.md)

