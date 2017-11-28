---
title: "Behörigheter i Azure Security Center | Microsoft Docs"
description: "Den här artikeln beskrivs hur Azure Security Center använder rollbaserad åtkomstkontroll för att tilldela behörigheter till användare och identifierar de tillåtna åtgärderna för varje roll."
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: 
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: 0aaa99dda44d2020afd3e841e84020eb4ff87a85
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="permissions-in-azure-security-center"></a><span data-ttu-id="e356b-103">Behörigheter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="e356b-103">Permissions in Azure Security Center</span></span>

<span data-ttu-id="e356b-104">I Azure Security Center tillämpas [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md), vilket innebär att det finns [förinställda roller](../active-directory/role-based-access-built-in-roles.md) som kan ges till användare, grupper och tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="e356b-104">Azure Security Center uses [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md), which provides [built-in roles](../active-directory/role-based-access-built-in-roles.md) that can be assigned to users, groups, and services in Azure.</span></span>

<span data-ttu-id="e356b-105">Security Center utvärderar konfigurationen av dina resurser för att identifiera säkerhetsproblem och säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="e356b-105">Security Center assesses the configuration of your resources to identify security issues and vulnerabilities.</span></span> <span data-ttu-id="e356b-106">I Security Center kan se du bara information relaterad till en resurs när du har tilldelats rollen ägare, deltagare eller läsare för prenumeration eller resursgrupp som en resurs tillhör.</span><span class="sxs-lookup"><span data-stu-id="e356b-106">In Security Center, you only see information related to a resource when you are assigned the role of Owner, Contributor, or Reader for the subscription or resource group that a resource belongs to.</span></span>

<span data-ttu-id="e356b-107">Förutom dessa roller finns två specifika roller i Security Center:</span><span class="sxs-lookup"><span data-stu-id="e356b-107">In addition to these roles, there are two specific Security Center roles:</span></span>

* <span data-ttu-id="e356b-108">**Säkerhet Reader**: en användare som tillhör den här rollen har rätt behörighet till Security Center.</span><span class="sxs-lookup"><span data-stu-id="e356b-108">**Security Reader**: A user that belongs to this role has viewing rights to Security Center.</span></span> <span data-ttu-id="e356b-109">Användaren kan visa rekommendationer, aviseringar, en säkerhetsprincip och säkerhetsstatus, men det går inte att göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="e356b-109">The user can view recommendations, alerts, a security policy, and security states, but cannot make changes.</span></span>
* <span data-ttu-id="e356b-110">**Säkerhetsadministratör**: en användare som tillhör den här rollen har samma rättigheter som läsaren säkerhet och kan också uppdatera säkerhetsprincipen och stänga aviseringar och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="e356b-110">**Security Administrator**: A user that belongs to this role has the same rights as the Security Reader and can also update the security policy and dismiss alerts and recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="e356b-111">Säkerhetsroller, säkerhet, läsare och säkerhetsadministratör, komma åt endast i Security Center.</span><span class="sxs-lookup"><span data-stu-id="e356b-111">The security roles, Security Reader and Security Administrator, have access only in Security Center.</span></span> <span data-ttu-id="e356b-112">Säkerhetsroller har inte åtkomst till andra service delar av Azure, till exempel lagring, webb & Mobile eller Sakernas Internet.</span><span class="sxs-lookup"><span data-stu-id="e356b-112">The security roles do not have access to other service areas of Azure such as Storage, Web & Mobile, or Internet of Things.</span></span>
>
>

## <a name="roles-and-allowed-actions"></a><span data-ttu-id="e356b-113">Roller och tillåtna åtgärder</span><span class="sxs-lookup"><span data-stu-id="e356b-113">Roles and allowed actions</span></span>

<span data-ttu-id="e356b-114">Följande tabell visar roller och tillåtna åtgärder i Security Center.</span><span class="sxs-lookup"><span data-stu-id="e356b-114">The following table displays roles and allowed actions in Security Center.</span></span> <span data-ttu-id="e356b-115">Ett X anger att åtgärden tillåts för rollen.</span><span class="sxs-lookup"><span data-stu-id="e356b-115">An X indicates that the action is allowed for that role.</span></span>

| <span data-ttu-id="e356b-116">Roll</span><span class="sxs-lookup"><span data-stu-id="e356b-116">Role</span></span> | <span data-ttu-id="e356b-117">Redigera säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="e356b-117">Edit security policy</span></span> | <span data-ttu-id="e356b-118">Tillämpa säkerhetsrekommendationer på en resurs</span><span class="sxs-lookup"><span data-stu-id="e356b-118">Apply security recommendations for a resource</span></span> | <span data-ttu-id="e356b-119">Avvisa aviseringar och rekommendationer</span><span class="sxs-lookup"><span data-stu-id="e356b-119">Dismiss alerts and recommendations</span></span> | <span data-ttu-id="e356b-120">Visa aviseringar och rekommendationer</span><span class="sxs-lookup"><span data-stu-id="e356b-120">View alerts and recommendations</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="e356b-121">Prenumerationsägaren</span><span class="sxs-lookup"><span data-stu-id="e356b-121">Subscription Owner</span></span> | <span data-ttu-id="e356b-122">X</span><span class="sxs-lookup"><span data-stu-id="e356b-122">X</span></span> | <span data-ttu-id="e356b-123">X</span><span class="sxs-lookup"><span data-stu-id="e356b-123">X</span></span> | <span data-ttu-id="e356b-124">X</span><span class="sxs-lookup"><span data-stu-id="e356b-124">X</span></span> | <span data-ttu-id="e356b-125">X</span><span class="sxs-lookup"><span data-stu-id="e356b-125">X</span></span> |
| <span data-ttu-id="e356b-126">Deltagare i prenumeration</span><span class="sxs-lookup"><span data-stu-id="e356b-126">Subscription Contributor</span></span> | <span data-ttu-id="e356b-127">X</span><span class="sxs-lookup"><span data-stu-id="e356b-127">X</span></span> | <span data-ttu-id="e356b-128">X</span><span class="sxs-lookup"><span data-stu-id="e356b-128">X</span></span> | <span data-ttu-id="e356b-129">X</span><span class="sxs-lookup"><span data-stu-id="e356b-129">X</span></span> | <span data-ttu-id="e356b-130">X</span><span class="sxs-lookup"><span data-stu-id="e356b-130">X</span></span> |
| <span data-ttu-id="e356b-131">Ägaren av resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e356b-131">Resource Group Owner</span></span> | -- | <span data-ttu-id="e356b-132">X</span><span class="sxs-lookup"><span data-stu-id="e356b-132">X</span></span> | -- | <span data-ttu-id="e356b-133">X</span><span class="sxs-lookup"><span data-stu-id="e356b-133">X</span></span> |
| <span data-ttu-id="e356b-134">Deltagare i resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e356b-134">Resource Group Contributor</span></span> | -- | <span data-ttu-id="e356b-135">X</span><span class="sxs-lookup"><span data-stu-id="e356b-135">X</span></span> | -- | <span data-ttu-id="e356b-136">X</span><span class="sxs-lookup"><span data-stu-id="e356b-136">X</span></span> |
| <span data-ttu-id="e356b-137">Läsare</span><span class="sxs-lookup"><span data-stu-id="e356b-137">Reader</span></span> | -- | -- | -- | <span data-ttu-id="e356b-138">X</span><span class="sxs-lookup"><span data-stu-id="e356b-138">X</span></span> |
| <span data-ttu-id="e356b-139">Säkerhetsadministratör</span><span class="sxs-lookup"><span data-stu-id="e356b-139">Security Administrator</span></span> | <span data-ttu-id="e356b-140">X</span><span class="sxs-lookup"><span data-stu-id="e356b-140">X</span></span> | -- | <span data-ttu-id="e356b-141">X</span><span class="sxs-lookup"><span data-stu-id="e356b-141">X</span></span> | <span data-ttu-id="e356b-142">X</span><span class="sxs-lookup"><span data-stu-id="e356b-142">X</span></span> |
| <span data-ttu-id="e356b-143">Säkerhet läsare</span><span class="sxs-lookup"><span data-stu-id="e356b-143">Security Reader</span></span> | -- | -- | -- | <span data-ttu-id="e356b-144">X</span><span class="sxs-lookup"><span data-stu-id="e356b-144">X</span></span> |

> [!NOTE]
> <span data-ttu-id="e356b-145">Vi rekommenderar att du ger användarna den roll som precis ger dem den behörighet de behöver för att kunna utföra sina arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e356b-145">We recommend that you assign the least permissive role needed for users to complete their tasks.</span></span> <span data-ttu-id="e356b-146">Till exempel tilldela rollen Läsare för användare som behöver bara visa information om säkerhetshälsa för en resurs men inte vidta några åtgärder, till exempel tillämpa rekommendationer eller ändra principer.</span><span class="sxs-lookup"><span data-stu-id="e356b-146">For example, assign the Reader role to users who only need to view information about the security health of a resource but not take action, such as applying recommendations or editing policies.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="e356b-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e356b-147">Next steps</span></span>
<span data-ttu-id="e356b-148">Den här artikeln förklaras hur RBAC använder i Security Center för att tilldela behörigheter till användare och identifieras de tillåtna åtgärderna för varje roll.</span><span class="sxs-lookup"><span data-stu-id="e356b-148">This article explained how Security Center uses RBAC to assign permissions to users and identified the allowed actions for each role.</span></span> <span data-ttu-id="e356b-149">Nu när du är bekant med rolltilldelningar som behövs för att övervaka säkerhetsläget för din prenumeration, redigera IPSec-principer och tillämpa rekommendationer, Läs mer om hur du:</span><span class="sxs-lookup"><span data-stu-id="e356b-149">Now that you're familiar with the role assignments needed to monitor the security state of your subscription, edit security policies, and apply recommendations, learn how to:</span></span>

- [<span data-ttu-id="e356b-150">Ange säkerhetsprinciper i Security Center</span><span class="sxs-lookup"><span data-stu-id="e356b-150">Set security policies in Security Center</span></span>](security-center-policies.md)
- [<span data-ttu-id="e356b-151">Hantera säkerhetsrekommendationer i Security Center</span><span class="sxs-lookup"><span data-stu-id="e356b-151">Manage security recommendations in Security Center</span></span>](security-center-recommendations.md)
- [<span data-ttu-id="e356b-152">Övervaka säkerhetshälsa för Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="e356b-152">Monitor the security health of your Azure resources</span></span>](security-center-monitoring.md)
- [<span data-ttu-id="e356b-153">Hantera och besvara säkerhetsaviseringar i Security Center</span><span class="sxs-lookup"><span data-stu-id="e356b-153">Manage and respond to security alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
- [<span data-ttu-id="e356b-154">Övervaka partnerlösningar för säkerhet</span><span class="sxs-lookup"><span data-stu-id="e356b-154">Monitor partner security solutions</span></span>](security-center-partner-solutions.md)
