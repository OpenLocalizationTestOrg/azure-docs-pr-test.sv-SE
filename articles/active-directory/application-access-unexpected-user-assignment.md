---
title: "aaaHow tooAssign användare tooapplications | Microsoft Docs"
description: "Förstå hur användare tilldelas tooan program i din klientorganisation"
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
ms.openlocfilehash: 4df60c7d723140d0d1bbd6ba8b34aa4e762d1138
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooassign-users-tooapplications"></a>Hur tooassign användare tooapplications

Den här artikeln hjälper dig toounderstand hur användare tilldelas tooan program i din klient.

## <a name="how-do-users-get-assigned-tooan-application-in-azure-ad"></a>Hur användare tilldelas tooan program i Azure AD?

För en användare tooaccess ett program, måste de först tilldelas tooit på något sätt. Tilldelning kan utföras av en administratör, en business ombud eller ibland hello användare själva. Nedan beskrivs hello sätt som användare kan tilldelas tooapplications:

1.  En administratör [tilldelas en användare](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) toohello program direkt

2.  En administratör [tilldelar en grupp](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) hello användaren är medlem i toohello program, inklusive:

  * En grupp som synkroniserades från lokala

  * En statisk säkerhetsgrupp som skapas i hello moln

  * En [dynamiska säkerhetsgrupp](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) skapas i hello moln

  * En Office 365-grupp som skapades i hello moln

  * Hej [alla användare](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grupp

3.  En administratör aktiverar [Self-service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow som en användare tooadd ett program som använder hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Lägg till App** funktionen **utan business godkännande**

4.  En administratör aktiverar [Self-service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow som en användare tooadd ett program som använder hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Lägg till App** funktionen, men endast w **: te tidigare godkännande från en vald uppsättning företag godkännare**

5.  En administratör aktiverar [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow användaren-toojoin en grupp som tilldelas ett program för**utan business godkännande**

6.  En administratör aktiverar [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow användaren-toojoin en grupp som ett program är tilldelad till, men endast **med föregående godkännande från en vald uppsättning företag godkännare**

7.  En administratör kopplar en användare med licens tooa direkt för ett första program tillverkare som [Microsoft Office 365](http://products.office.com/)

8.  En administratör tilldelas en licensgrupp tooa som hello användaren är medlem i tooa första partsprogram som [Microsoft Office 365](http://products.office.com/)

9.  En [administratören godkänner tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe som används av alla användare och sedan en användare loggar in toohello program

10. En användare [godkänner tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) sig genom att logga in toohello program

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
