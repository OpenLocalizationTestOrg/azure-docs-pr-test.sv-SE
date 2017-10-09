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
# <a name="how-tooassign-users-tooapplications"></a><span data-ttu-id="79072-103">Hur tooassign användare tooapplications</span><span class="sxs-lookup"><span data-stu-id="79072-103">How tooassign users tooapplications</span></span>

<span data-ttu-id="79072-104">Den här artikeln hjälper dig toounderstand hur användare tilldelas tooan program i din klient.</span><span class="sxs-lookup"><span data-stu-id="79072-104">This article help you toounderstand how users get assigned tooan application in your tenant.</span></span>

## <a name="how-do-users-get-assigned-tooan-application-in-azure-ad"></a><span data-ttu-id="79072-105">Hur användare tilldelas tooan program i Azure AD?</span><span class="sxs-lookup"><span data-stu-id="79072-105">How do users get assigned tooan application in Azure AD?</span></span>

<span data-ttu-id="79072-106">För en användare tooaccess ett program, måste de först tilldelas tooit på något sätt.</span><span class="sxs-lookup"><span data-stu-id="79072-106">For a user tooaccess an application, they must first be assigned tooit in some way.</span></span> <span data-ttu-id="79072-107">Tilldelning kan utföras av en administratör, en business ombud eller ibland hello användare själva.</span><span class="sxs-lookup"><span data-stu-id="79072-107">Assignment can be performed by an administrator, a business delegate, or sometimes, hello user themselves.</span></span> <span data-ttu-id="79072-108">Nedan beskrivs hello sätt som användare kan tilldelas tooapplications:</span><span class="sxs-lookup"><span data-stu-id="79072-108">Below describes hello ways users can get assigned tooapplications:</span></span>

1.  <span data-ttu-id="79072-109">En administratör [tilldelas en användare](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) toohello program direkt</span><span class="sxs-lookup"><span data-stu-id="79072-109">An administrator [assigns a user](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) toohello application directly</span></span>

2.  <span data-ttu-id="79072-110">En administratör [tilldelar en grupp](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) hello användaren är medlem i toohello program, inklusive:</span><span class="sxs-lookup"><span data-stu-id="79072-110">An administrator [assigns a group](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) that hello user is a member of toohello application, including:</span></span>

  * <span data-ttu-id="79072-111">En grupp som synkroniserades från lokala</span><span class="sxs-lookup"><span data-stu-id="79072-111">A group that was synchronized from on-premises</span></span>

  * <span data-ttu-id="79072-112">En statisk säkerhetsgrupp som skapas i hello moln</span><span class="sxs-lookup"><span data-stu-id="79072-112">A static security group created in hello cloud</span></span>

  * <span data-ttu-id="79072-113">En [dynamiska säkerhetsgrupp](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) skapas i hello moln</span><span class="sxs-lookup"><span data-stu-id="79072-113">A [dynamic security group](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) created in hello cloud</span></span>

  * <span data-ttu-id="79072-114">En Office 365-grupp som skapades i hello moln</span><span class="sxs-lookup"><span data-stu-id="79072-114">An Office 365 group created in hello cloud</span></span>

  * <span data-ttu-id="79072-115">Hej [alla användare](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) grupp</span><span class="sxs-lookup"><span data-stu-id="79072-115">hello [All Users](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups) group</span></span>

3.  <span data-ttu-id="79072-116">En administratör aktiverar [Self-service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow som en användare tooadd ett program som använder hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Lägg till App** funktionen **utan business godkännande**</span><span class="sxs-lookup"><span data-stu-id="79072-116">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow a user tooadd an application using hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature **without business approval**</span></span>

4.  <span data-ttu-id="79072-117">En administratör aktiverar [Self-service programåtkomst](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow som en användare tooadd ett program som använder hello [programmet åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Lägg till App** funktionen, men endast w **: te tidigare godkännande från en vald uppsättning företag godkännare**</span><span class="sxs-lookup"><span data-stu-id="79072-117">An administrator enables [Self-service Application Access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooallow a user tooadd an application using hello [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Add App** feature, but only w**ith prior approval from a selected set of business approvers**</span></span>

5.  <span data-ttu-id="79072-118">En administratör aktiverar [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow användaren-toojoin en grupp som tilldelas ett program för**utan business godkännande**</span><span class="sxs-lookup"><span data-stu-id="79072-118">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow a user toojoin a group that an application is assigned too**without business approval**</span></span>

6.  <span data-ttu-id="79072-119">En administratör aktiverar [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow användaren-toojoin en grupp som ett program är tilldelad till, men endast **med föregående godkännande från en vald uppsättning företag godkännare**</span><span class="sxs-lookup"><span data-stu-id="79072-119">An administrator enables [Self-service Group Management](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) tooallow a user toojoin a group that an application is assigned to, but only **with prior approval from a selected set of business approvers**</span></span>

7.  <span data-ttu-id="79072-120">En administratör kopplar en användare med licens tooa direkt för ett första program tillverkare som [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="79072-120">An administrator assigns a license tooa user directly for a first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

8.  <span data-ttu-id="79072-121">En administratör tilldelas en licensgrupp tooa som hello användaren är medlem i tooa första partsprogram som [Microsoft Office 365](http://products.office.com/)</span><span class="sxs-lookup"><span data-stu-id="79072-121">An administrator assigns a license tooa group that hello user is a member of tooa first party application, like [Microsoft Office 365](http://products.office.com/)</span></span>

9.  <span data-ttu-id="79072-122">En [administratören godkänner tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe som används av alla användare och sedan en användare loggar in toohello program</span><span class="sxs-lookup"><span data-stu-id="79072-122">An [administrator consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) toobe used by all users and then a user signs in toohello application</span></span>

10. <span data-ttu-id="79072-123">En användare [godkänner tooan programmet](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) sig genom att logga in toohello program</span><span class="sxs-lookup"><span data-stu-id="79072-123">A user [consents tooan application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) themselves by signing in toohello application</span></span>

## <a name="next-steps"></a><span data-ttu-id="79072-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79072-124">Next steps</span></span>
[<span data-ttu-id="79072-125">Hantera program med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79072-125">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)
