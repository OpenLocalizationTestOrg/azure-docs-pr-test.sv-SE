---
title: "Egenskaperna för Azure Active Directory-klient intercaction | Microsoft Docs"
description: "Hantera dina Azure Active-klient-klienter genom att förstå klienterna som helt oberoende resurser"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: d25d2c731034d0785bbd404ec693c4c41d913d01
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a><span data-ttu-id="86bca-103">Förstå hur flera innehavare av Azure Active Directory interagera</span><span class="sxs-lookup"><span data-stu-id="86bca-103">Understand how multiple Azure Active Directory tenants interact</span></span>

<span data-ttu-id="86bca-104">I Azure Active Directory (Azure AD), varje klient är en helt oberoende resurser: en peer-dator som är logiskt oberoende av andra klienter som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="86bca-104">In Azure Active Directory (Azure AD), each tenent is a fully independent resource: a peer that is logically independent from the other tenants that you manage.</span></span> <span data-ttu-id="86bca-105">Det finns ingen överordnad-underordnad relation mellan klienter behålls.</span><span class="sxs-lookup"><span data-stu-id="86bca-105">There is no parent-child relationship between tenants.</span></span> <span data-ttu-id="86bca-106">Detta oberoende mellan klienter innehåller resursoberoende, administrativt oberoende och synkroniseringsoberoende.</span><span class="sxs-lookup"><span data-stu-id="86bca-106">This independence between tenants includes resource independence, administrative independence, and synchronization independence.</span></span>

## <a name="resource-independence"></a><span data-ttu-id="86bca-107">Resursoberoende</span><span class="sxs-lookup"><span data-stu-id="86bca-107">Resource independence</span></span>
* <span data-ttu-id="86bca-108">Om du skapar eller tar bort en resurs i en klient har ingen inverkan på alla resurser i en annan innehavare, delvis med undantag av externa användare.</span><span class="sxs-lookup"><span data-stu-id="86bca-108">If you create or delete a resource in one tenant, it has no impact on any resource in another tenant, with the partial exception of external users.</span></span> 
* <span data-ttu-id="86bca-109">Om du använder en av dina domännamn med en klient kan inte användas med andra innehavare.</span><span class="sxs-lookup"><span data-stu-id="86bca-109">If you use one of your domain names with one tenant, it cannot be used with any other tenant.</span></span>

## <a name="administrative-independence"></a><span data-ttu-id="86bca-110">Administrativt oberoende</span><span class="sxs-lookup"><span data-stu-id="86bca-110">Administrative independence</span></span>
<span data-ttu-id="86bca-111">Om en icke-administrativa användare av innehavaren ”Contoso” skapar en Testklient ”Test” sedan:</span><span class="sxs-lookup"><span data-stu-id="86bca-111">If a non-administrative user of tenant 'Contoso' creates a test tenant 'Test,' then:</span></span>

* <span data-ttu-id="86bca-112">Som standard är den användare som skapar en klient läggas till som en extern användare i den nya klienten och tilldelats rollen global administratör i den klienten.</span><span class="sxs-lookup"><span data-stu-id="86bca-112">By default, the user who creates a tenant is added as an external user in that new tenant, and assigned the global administrator role in that tenant.</span></span>
* <span data-ttu-id="86bca-113">Administratörer av innehavaren ”Contoso” har ingen direkt administratörsbehörighet till klient 'Test' om inte en administratör ”test” uttryckligen beviljar dem sådan behörighet.</span><span class="sxs-lookup"><span data-stu-id="86bca-113">The administrators of tenant 'Contoso' have no direct administrative privileges to tenant 'Test,' unless an administrator of 'Test' specifically grants them these privileges.</span></span> <span data-ttu-id="86bca-114">Dock kan ”Contoso”-administratörerna styra åtkomsten till klient ”Test” om de kontrollera att användarkontot som skapade ”Test”.</span><span class="sxs-lookup"><span data-stu-id="86bca-114">However, administrators of 'Contoso' can control access to tenant 'Test' if they control the user account that created 'Test.'</span></span>
* <span data-ttu-id="86bca-115">Om du lägger till/ta bort en administratörsroll för en användare i en klient, påverkar inte ändringen administratörsroller som användaren har i en annan klient.</span><span class="sxs-lookup"><span data-stu-id="86bca-115">If you add/remove an administrator role for a user in one tenant, the change does not affect the administrator roles that the user has in another tenant.</span></span>

## <a name="synchronization-independence"></a><span data-ttu-id="86bca-116">Synkroniseringsoberoende</span><span class="sxs-lookup"><span data-stu-id="86bca-116">Synchronization independence</span></span>
<span data-ttu-id="86bca-117">Du kan konfigurera varje Azure AD-klient separat om du vill synkronisera data från en enda instans av antingen:</span><span class="sxs-lookup"><span data-stu-id="86bca-117">You can configure each Azure AD tenant independently to get data synchronized from a single instance of either:</span></span>

* <span data-ttu-id="86bca-118">Verktyget Azure AD Connect att synkronisera data med en enda AD-skog.</span><span class="sxs-lookup"><span data-stu-id="86bca-118">The Azure AD Connect tool, to synchronize data with a single AD forest.</span></span>
* <span data-ttu-id="86bca-119">Azure Active innehavaren Connector för Forefront Identity Manager för att synkronisera data med en eller flera lokala skogar och/eller Azure-AD-datakällor.</span><span class="sxs-lookup"><span data-stu-id="86bca-119">The Azure Active tenant Connector for Forefront Identity Manager, to synchronize data with one or more on-premises forests, and/or non-Azure AD data sources.</span></span>

## <a name="add-an-azure-ad-tenant"></a><span data-ttu-id="86bca-120">Lägg till en Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="86bca-120">Add an Azure AD tenant</span></span>
<span data-ttu-id="86bca-121">Om du vill lägga till en Azure AD-klient i Azure-portalen, logga in på [Azure-portalen](https://portal.azure.com) med ett konto som är global administratör för Azure AD och till vänster, Välj **ny**.</span><span class="sxs-lookup"><span data-stu-id="86bca-121">To add an Azure AD tenant in the Azure portal, sign in to [the Azure portal](https://portal.azure.com) with an account that is an Azure AD global administrator, and, on the left, select **New**.</span></span>

> [!NOTE]
> <span data-ttu-id="86bca-122">Till skillnad från andra Azure-resurser är klienterna inte underordnade resurser till en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="86bca-122">Unlike other Azure resources, your tenants are not child resources of an Azure subscription.</span></span> <span data-ttu-id="86bca-123">Om din Azure-prenumeration avbryts eller upphört att gälla, kan du fortfarande komma åt din klientdata med hjälp av Azure PowerShell, Azure Graph API eller Office 365 Admin Center.</span><span class="sxs-lookup"><span data-stu-id="86bca-123">If your Azure subscription is canceled or expired, you can still access your tenant data using Azure PowerShell, the Azure Graph API, or the Office 365 Admin Center.</span></span> <span data-ttu-id="86bca-124">Du kan även associera en annan prenumeration med klienten.</span><span class="sxs-lookup"><span data-stu-id="86bca-124">You can also associate another subscription with the tenant.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="86bca-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86bca-125">Next steps</span></span>
<span data-ttu-id="86bca-126">En översikt av Azure AD licensproblem och bästa praxis, se [vad är Azure Active-klient licensiering?](active-directory-licensing-whatis-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="86bca-126">For a broad overview of Azure AD licensing issues and best practices, see [What is Azure Active tenant licensing?](active-directory-licensing-whatis-azure-portal.md)</span></span>
