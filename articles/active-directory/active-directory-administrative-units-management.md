---
title: "Förhandsgranskning för hantering av administrativa enheter i Azure Active Directory"
description: "Med hjälp av administrativa enheter för mer detaljerade delegeringen av behörigheter i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: e12a0aea8264b1ea67c26294ec5bbe9c404a171e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a><span data-ttu-id="05de8-103">Hantering av administrativa enheter i Azure AD - förhandsversion</span><span class="sxs-lookup"><span data-stu-id="05de8-103">Administrative units management in Azure AD - public preview</span></span>
<span data-ttu-id="05de8-104">Den här artikeln beskriver administrativa enheter – en ny Azure Active Directory-behållare för resurser som kan användas för att delegera administrativa behörigheter över delmängder av användare och tillämpa principer för att en del av användarna.</span><span class="sxs-lookup"><span data-stu-id="05de8-104">This article describes administrative units – a new Azure Active Directory container of resources that can be used for delegating administrative permissions over subsets of users and applying policies to a subset of users.</span></span> <span data-ttu-id="05de8-105">I Azure Active Directory aktivera administrativa enheter centrala administratörer att delegera behörigheter till regionala administratörer eller skapa princip på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="05de8-105">In Azure Active Directory, administrative units enable central administrators to delegate permissions to regional administrators or to set policy at a granular level.</span></span>

<span data-ttu-id="05de8-106">Detta är användbart i organisationer med oberoende avdelningar, till exempel en stor university som består av många autonoma skolorna (Business skola, ingenjörer skola och så vidare) som är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="05de8-106">This is useful in organizations with independent divisions, for example, a large university that is made up of many autonomous schools (Business school, Engineering school, and so on) which are independent from each other.</span></span> <span data-ttu-id="05de8-107">Sådana avdelningar har sina egna IT-administratörer som styr åtkomsten, hantera användare och ange principer för fördelningen.</span><span class="sxs-lookup"><span data-stu-id="05de8-107">Such divisions have their own IT administrators who control access, manage users, and set policies specifically for their division.</span></span> <span data-ttu-id="05de8-108">Central Administratörer vill kunna ge dessa dig administratörer behörigheter till användare i deras specifika avdelningar.</span><span class="sxs-lookup"><span data-stu-id="05de8-108">Central administrators want to be able grant these divisional administrators permissions over the users in their particular divisions.</span></span> <span data-ttu-id="05de8-109">Mer specifikt kan i det här exemplet administratör av en central, till exempel skapa en administrativ enhet för en viss skola (Business skolan) och fylla det med bara de skola företagsanvändarna.</span><span class="sxs-lookup"><span data-stu-id="05de8-109">More specifically, using this example, a central administrator can, for instance, create an administrative unit for a particular school (Business school) and populate it with only the Business school users.</span></span> <span data-ttu-id="05de8-110">En central administratör kan lägga till företag skola IT-personal till en roll som omfattas, med andra ord ge IT-personal Business skola behörigheter endast med administrativa affärsenhet skola.</span><span class="sxs-lookup"><span data-stu-id="05de8-110">Then a central administrator can add the Business school IT staff to a scoped role, in other words, grant the IT staff of Business school administrative permissions only over the Business school administrative unit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05de8-111">Du kan tilldela administrativa enhet omfång administratörsroller endast om du aktiverar Azure Active Directory Premium.</span><span class="sxs-lookup"><span data-stu-id="05de8-111">You can assign administrative unit-scoped admin roles only if you enable Azure Active Directory Premium.</span></span> <span data-ttu-id="05de8-112">Mer information finns i [komma igång med Azure AD Premium](active-directory-get-started-premium.md).</span><span class="sxs-lookup"><span data-stu-id="05de8-112">For more information, see [Getting started with Azure AD Premium](active-directory-get-started-premium.md).</span></span>
>


<span data-ttu-id="05de8-113">Från central administratörs synsätt är en administrativ enhet ett katalogobjekt som kan skapas och fylls med resurser.</span><span class="sxs-lookup"><span data-stu-id="05de8-113">From the central administrator’s point of view, an administrative unit is a directory object that can be created and populated with resources.</span></span> <span data-ttu-id="05de8-114">**Dessa resurser kan vara endast användare i den här förhandsversionen.**</span><span class="sxs-lookup"><span data-stu-id="05de8-114">**In this preview release, these resources can be only users.**</span></span> <span data-ttu-id="05de8-115">När skapas och konfigureras kan den administrativa enheten användas som omfattning för att begränsa den beviljade behörigheten endast över resurser som ingår i den administrativa enheten.</span><span class="sxs-lookup"><span data-stu-id="05de8-115">Once created and populated, the administrative unit can be used as a scope to restrict the granted permission only over resources contained in the administrative unit.</span></span>

## <a name="managing-administrative-units"></a><span data-ttu-id="05de8-116">Hantera administrativa enheter</span><span class="sxs-lookup"><span data-stu-id="05de8-116">Managing administrative units</span></span>
<span data-ttu-id="05de8-117">I den här förhandsversionen kan du skapa och hantera administrativa enheter med hjälp av Azure Active Directory-modulen för Windows PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="05de8-117">In this preview release, you can create and manage administrative units using the Azure Active Directory Module for Windows PowerShell cmdlets.</span></span> <span data-ttu-id="05de8-118">Mer information om hur du gör finns [arbeta med administrativa enheter](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span><span class="sxs-lookup"><span data-stu-id="05de8-118">To learn more about how to do that, see [Working with Administrative Units](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span></span>

<span data-ttu-id="05de8-119">Mer information om programvarukrav och installera Azure AD-modulen och information om Azure AD-modulen-cmdletar för att hantera administrativa enheter, inklusive syntax och beskrivningar av parametern exempel, se [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="05de8-119">For more information on software requirements and installing the Azure AD module, and for information on the Azure AD Module cmdlets for managing administrative units, including syntax, parameter descriptions, and examples, see [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span></span>

## <a name="next-steps"></a><span data-ttu-id="05de8-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05de8-120">Next steps</span></span>
[<span data-ttu-id="05de8-121">Azure Active Directory-versioner</span><span class="sxs-lookup"><span data-stu-id="05de8-121">Azure Active Directory editions</span></span>](active-directory-editions.md)
