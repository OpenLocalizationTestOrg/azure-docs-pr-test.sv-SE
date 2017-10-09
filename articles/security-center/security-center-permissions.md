---
title: aaaPermissions i Azure Security Center | Microsoft Docs
description: "Den här artikeln beskrivs hur Azure Security Center använder rollbaserad åtkomstkontroll tooassign behörigheter toousers och identifierar hello tillåtna åtgärder för varje roll."
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
ms.openlocfilehash: 03e16132dc3d951ef8ad9e86b9970b9e4d15c76b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-in-azure-security-center"></a><span data-ttu-id="57fda-103">Behörigheter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="57fda-103">Permissions in Azure Security Center</span></span>

<span data-ttu-id="57fda-104">Azure Security Center använder [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md), vilket ger [inbyggda roller](../active-directory/role-based-access-built-in-roles.md) som kan tilldelas toousers, grupper och tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="57fda-104">Azure Security Center uses [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md), which provides [built-in roles](../active-directory/role-based-access-built-in-roles.md) that can be assigned toousers, groups, and services in Azure.</span></span>

<span data-ttu-id="57fda-105">Security Center utvärderar hello konfigurationen av dina resurser tooidentify säkerhetsproblem och säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="57fda-105">Security Center assesses hello configuration of your resources tooidentify security issues and vulnerabilities.</span></span> <span data-ttu-id="57fda-106">I Security Center, du kan bara se information relaterade tooa resurs när du har tilldelats hello rollen ägare, deltagare eller läsare för hello prenumerationen eller resursen gruppen som en resurs tillhör.</span><span class="sxs-lookup"><span data-stu-id="57fda-106">In Security Center, you only see information related tooa resource when you are assigned hello role of Owner, Contributor, or Reader for hello subscription or resource group that a resource belongs to.</span></span>

<span data-ttu-id="57fda-107">Det finns två specifika roller i Security Center i tillägg toothese roller:</span><span class="sxs-lookup"><span data-stu-id="57fda-107">In addition toothese roles, there are two specific Security Center roles:</span></span>

* <span data-ttu-id="57fda-108">**Säkerhet Reader**: en användare som tillhör toothis roll har visa rättigheter tooSecurity Center.</span><span class="sxs-lookup"><span data-stu-id="57fda-108">**Security Reader**: A user that belongs toothis role has viewing rights tooSecurity Center.</span></span> <span data-ttu-id="57fda-109">hello användare kan visa rekommendationer, aviseringar, en säkerhetsprincip och säkerhetsstatus, men det går inte att göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="57fda-109">hello user can view recommendations, alerts, a security policy, and security states, but cannot make changes.</span></span>
* <span data-ttu-id="57fda-110">**Säkerhetsadministratör**: en användare som tillhör toothis roll har samma rättigheter som hello säkerhet Reader hello och de kan också uppdatera hello säkerhetsprincip och stänga aviseringar och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="57fda-110">**Security Administrator**: A user that belongs toothis role has hello same rights as hello Security Reader and can also update hello security policy and dismiss alerts and recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="57fda-111">hello säkerhetsroller, säkerhet, läsare och säkerhetsadministratör, komma åt endast i Security Center.</span><span class="sxs-lookup"><span data-stu-id="57fda-111">hello security roles, Security Reader and Security Administrator, have access only in Security Center.</span></span> <span data-ttu-id="57fda-112">hello säkerhetsroller har inte åtkomst tooother service områden i Azure, till exempel lagring, webb & Mobile eller Sakernas Internet.</span><span class="sxs-lookup"><span data-stu-id="57fda-112">hello security roles do not have access tooother service areas of Azure such as Storage, Web & Mobile, or Internet of Things.</span></span>
>
>

## <a name="roles-and-allowed-actions"></a><span data-ttu-id="57fda-113">Roller och tillåtna åtgärder</span><span class="sxs-lookup"><span data-stu-id="57fda-113">Roles and allowed actions</span></span>

<span data-ttu-id="57fda-114">hello följande tabell visar roller och tillåtna åtgärder i Security Center.</span><span class="sxs-lookup"><span data-stu-id="57fda-114">hello following table displays roles and allowed actions in Security Center.</span></span> <span data-ttu-id="57fda-115">Ett X anger att hello-åtgärd tillåts för rollen.</span><span class="sxs-lookup"><span data-stu-id="57fda-115">An X indicates that hello action is allowed for that role.</span></span>

| <span data-ttu-id="57fda-116">Roll</span><span class="sxs-lookup"><span data-stu-id="57fda-116">Role</span></span> | <span data-ttu-id="57fda-117">Redigera säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="57fda-117">Edit security policy</span></span> | <span data-ttu-id="57fda-118">Tillämpa säkerhetsrekommendationer på en resurs</span><span class="sxs-lookup"><span data-stu-id="57fda-118">Apply security recommendations for a resource</span></span> | <span data-ttu-id="57fda-119">Avvisa aviseringar och rekommendationer</span><span class="sxs-lookup"><span data-stu-id="57fda-119">Dismiss alerts and recommendations</span></span> | <span data-ttu-id="57fda-120">Visa aviseringar och rekommendationer</span><span class="sxs-lookup"><span data-stu-id="57fda-120">View alerts and recommendations</span></span> |
|:--- |:---:|:---:|:---:|:---:|
| <span data-ttu-id="57fda-121">Prenumerationsägaren</span><span class="sxs-lookup"><span data-stu-id="57fda-121">Subscription Owner</span></span> | <span data-ttu-id="57fda-122">X</span><span class="sxs-lookup"><span data-stu-id="57fda-122">X</span></span> | <span data-ttu-id="57fda-123">X</span><span class="sxs-lookup"><span data-stu-id="57fda-123">X</span></span> | <span data-ttu-id="57fda-124">X</span><span class="sxs-lookup"><span data-stu-id="57fda-124">X</span></span> | <span data-ttu-id="57fda-125">X</span><span class="sxs-lookup"><span data-stu-id="57fda-125">X</span></span> |
| <span data-ttu-id="57fda-126">Deltagare i prenumeration</span><span class="sxs-lookup"><span data-stu-id="57fda-126">Subscription Contributor</span></span> | <span data-ttu-id="57fda-127">X</span><span class="sxs-lookup"><span data-stu-id="57fda-127">X</span></span> | <span data-ttu-id="57fda-128">X</span><span class="sxs-lookup"><span data-stu-id="57fda-128">X</span></span> | <span data-ttu-id="57fda-129">X</span><span class="sxs-lookup"><span data-stu-id="57fda-129">X</span></span> | <span data-ttu-id="57fda-130">X</span><span class="sxs-lookup"><span data-stu-id="57fda-130">X</span></span> |
| <span data-ttu-id="57fda-131">Ägaren av resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="57fda-131">Resource Group Owner</span></span> | -- | <span data-ttu-id="57fda-132">X</span><span class="sxs-lookup"><span data-stu-id="57fda-132">X</span></span> | -- | <span data-ttu-id="57fda-133">X</span><span class="sxs-lookup"><span data-stu-id="57fda-133">X</span></span> |
| <span data-ttu-id="57fda-134">Deltagare i resursgrupp</span><span class="sxs-lookup"><span data-stu-id="57fda-134">Resource Group Contributor</span></span> | -- | <span data-ttu-id="57fda-135">X</span><span class="sxs-lookup"><span data-stu-id="57fda-135">X</span></span> | -- | <span data-ttu-id="57fda-136">X</span><span class="sxs-lookup"><span data-stu-id="57fda-136">X</span></span> |
| <span data-ttu-id="57fda-137">Läsare</span><span class="sxs-lookup"><span data-stu-id="57fda-137">Reader</span></span> | -- | -- | -- | <span data-ttu-id="57fda-138">X</span><span class="sxs-lookup"><span data-stu-id="57fda-138">X</span></span> |
| <span data-ttu-id="57fda-139">Säkerhetsadministratör</span><span class="sxs-lookup"><span data-stu-id="57fda-139">Security Administrator</span></span> | <span data-ttu-id="57fda-140">X</span><span class="sxs-lookup"><span data-stu-id="57fda-140">X</span></span> | -- | <span data-ttu-id="57fda-141">X</span><span class="sxs-lookup"><span data-stu-id="57fda-141">X</span></span> | <span data-ttu-id="57fda-142">X</span><span class="sxs-lookup"><span data-stu-id="57fda-142">X</span></span> |
| <span data-ttu-id="57fda-143">Säkerhet läsare</span><span class="sxs-lookup"><span data-stu-id="57fda-143">Security Reader</span></span> | -- | -- | -- | <span data-ttu-id="57fda-144">X</span><span class="sxs-lookup"><span data-stu-id="57fda-144">X</span></span> |

> [!NOTE]
> <span data-ttu-id="57fda-145">Vi rekommenderar att du ger hello minst Tillåtande rollen som behövs för användare toocomplete sina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="57fda-145">We recommend that you assign hello least permissive role needed for users toocomplete their tasks.</span></span> <span data-ttu-id="57fda-146">Till exempel tilldela hello Reader rollen toousers som bara behöver tooview information om hello säkerhetshälsa för en resurs, men inte vidta några åtgärder, till exempel tillämpa rekommendationer eller ändra principer.</span><span class="sxs-lookup"><span data-stu-id="57fda-146">For example, assign hello Reader role toousers who only need tooview information about hello security health of a resource but not take action, such as applying recommendations or editing policies.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="57fda-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57fda-147">Next steps</span></span>
<span data-ttu-id="57fda-148">Den här artikeln förklaras hur Security Center använder RBAC tooassign behörigheter toousers och identifieras hello tillåtna åtgärder för varje roll.</span><span class="sxs-lookup"><span data-stu-id="57fda-148">This article explained how Security Center uses RBAC tooassign permissions toousers and identified hello allowed actions for each role.</span></span> <span data-ttu-id="57fda-149">Nu när du är bekant med hello rolltilldelningar behövs toomonitor hello säkerhetsläget för din prenumeration, redigera IPSec-principer och tillämpa rekommendationer, Läs mer om hur du:</span><span class="sxs-lookup"><span data-stu-id="57fda-149">Now that you're familiar with hello role assignments needed toomonitor hello security state of your subscription, edit security policies, and apply recommendations, learn how to:</span></span>

- [<span data-ttu-id="57fda-150">Ange säkerhetsprinciper i Security Center</span><span class="sxs-lookup"><span data-stu-id="57fda-150">Set security policies in Security Center</span></span>](security-center-policies.md)
- [<span data-ttu-id="57fda-151">Hantera säkerhetsrekommendationer i Security Center</span><span class="sxs-lookup"><span data-stu-id="57fda-151">Manage security recommendations in Security Center</span></span>](security-center-recommendations.md)
- [<span data-ttu-id="57fda-152">Övervaka hello säkerhetshälsa i Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="57fda-152">Monitor hello security health of your Azure resources</span></span>](security-center-monitoring.md)
- [<span data-ttu-id="57fda-153">Hanterar och åtgärdar toosecurity aviseringar i Security Center</span><span class="sxs-lookup"><span data-stu-id="57fda-153">Manage and respond toosecurity alerts in Security Center</span></span>](security-center-managing-and-responding-alerts.md)
- [<span data-ttu-id="57fda-154">Övervaka partnerlösningar för säkerhet</span><span class="sxs-lookup"><span data-stu-id="57fda-154">Monitor partner security solutions</span></span>](security-center-partner-solutions.md)
