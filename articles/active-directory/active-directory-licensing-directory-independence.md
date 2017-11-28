---
title: aaaCharacteristics av Azure Active Directory-klient intercaction | Microsoft Docs
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
ms.openlocfilehash: 57b677665c7cb4aee63f518c39d26754fe71a999
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a><span data-ttu-id="c5419-103">Förstå hur flera innehavare av Azure Active Directory interagera</span><span class="sxs-lookup"><span data-stu-id="c5419-103">Understand how multiple Azure Active Directory tenants interact</span></span>

<span data-ttu-id="c5419-104">I Azure Active Directory (Azure AD), varje klient är en helt oberoende resurser: en peer-dator som är logiskt oberoende av hello andra klienter som du hanterar.</span><span class="sxs-lookup"><span data-stu-id="c5419-104">In Azure Active Directory (Azure AD), each tenent is a fully independent resource: a peer that is logically independent from hello other tenants that you manage.</span></span> <span data-ttu-id="c5419-105">Det finns ingen överordnad-underordnad relation mellan klienter behålls.</span><span class="sxs-lookup"><span data-stu-id="c5419-105">There is no parent-child relationship between tenants.</span></span> <span data-ttu-id="c5419-106">Detta oberoende mellan klienter innehåller resursoberoende, administrativt oberoende och synkroniseringsoberoende.</span><span class="sxs-lookup"><span data-stu-id="c5419-106">This independence between tenants includes resource independence, administrative independence, and synchronization independence.</span></span>

## <a name="resource-independence"></a><span data-ttu-id="c5419-107">Resursoberoende</span><span class="sxs-lookup"><span data-stu-id="c5419-107">Resource independence</span></span>
* <span data-ttu-id="c5419-108">Om du skapar eller tar bort en resurs i en klient har ingen inverkan på alla resurser i en annan innehavare med hello partiella undantag av externa användare.</span><span class="sxs-lookup"><span data-stu-id="c5419-108">If you create or delete a resource in one tenant, it has no impact on any resource in another tenant, with hello partial exception of external users.</span></span> 
* <span data-ttu-id="c5419-109">Om du använder en av dina domännamn med en klient kan inte användas med andra innehavare.</span><span class="sxs-lookup"><span data-stu-id="c5419-109">If you use one of your domain names with one tenant, it cannot be used with any other tenant.</span></span>

## <a name="administrative-independence"></a><span data-ttu-id="c5419-110">Administrativt oberoende</span><span class="sxs-lookup"><span data-stu-id="c5419-110">Administrative independence</span></span>
<span data-ttu-id="c5419-111">Om en icke-administrativa användare av innehavaren ”Contoso” skapar en Testklient ”Test” sedan:</span><span class="sxs-lookup"><span data-stu-id="c5419-111">If a non-administrative user of tenant 'Contoso' creates a test tenant 'Test,' then:</span></span>

* <span data-ttu-id="c5419-112">Som standard läggs hello-användare som skapar en klient som en extern användare i den nya innehavaren och tilldelade hello rollen som global administratör i den klienten.</span><span class="sxs-lookup"><span data-stu-id="c5419-112">By default, hello user who creates a tenant is added as an external user in that new tenant, and assigned hello global administrator role in that tenant.</span></span>
* <span data-ttu-id="c5419-113">hello administratörer för ”Contoso”-klienten har ingen direkt administratörsbehörighet tootenant 'Test' om inte en administratör ”test” uttryckligen beviljar dem sådan behörighet.</span><span class="sxs-lookup"><span data-stu-id="c5419-113">hello administrators of tenant 'Contoso' have no direct administrative privileges tootenant 'Test,' unless an administrator of 'Test' specifically grants them these privileges.</span></span> <span data-ttu-id="c5419-114">”Contoso”-administratörerna kan dock styra åtkomst tootenant ”Test” om de styr hello användarkontot som skapade ”Test”.</span><span class="sxs-lookup"><span data-stu-id="c5419-114">However, administrators of 'Contoso' can control access tootenant 'Test' if they control hello user account that created 'Test.'</span></span>
* <span data-ttu-id="c5419-115">Om du lägger till/ta bort en administratörsroll för en användare i en klient, hello ändringen har inte påverkar hello administratörsroller som hello användare har i en annan klient.</span><span class="sxs-lookup"><span data-stu-id="c5419-115">If you add/remove an administrator role for a user in one tenant, hello change does not affect hello administrator roles that hello user has in another tenant.</span></span>

## <a name="synchronization-independence"></a><span data-ttu-id="c5419-116">Synkroniseringsoberoende</span><span class="sxs-lookup"><span data-stu-id="c5419-116">Synchronization independence</span></span>
<span data-ttu-id="c5419-117">Du kan konfigurera varje Azure AD-klient oberoende tooget synkronisera data från en enda instans av antingen:</span><span class="sxs-lookup"><span data-stu-id="c5419-117">You can configure each Azure AD tenant independently tooget data synchronized from a single instance of either:</span></span>

* <span data-ttu-id="c5419-118">hello Azure AD Connect-verktyget, toosynchronize data med en enda AD-skog.</span><span class="sxs-lookup"><span data-stu-id="c5419-118">hello Azure AD Connect tool, toosynchronize data with a single AD forest.</span></span>
* <span data-ttu-id="c5419-119">hello Azure Active-klient Connector för Forefront Identity Manager, toosynchronize data med en eller flera lokala skogar och/eller Azure-AD-datakällor.</span><span class="sxs-lookup"><span data-stu-id="c5419-119">hello Azure Active tenant Connector for Forefront Identity Manager, toosynchronize data with one or more on-premises forests, and/or non-Azure AD data sources.</span></span>

## <a name="add-an-azure-ad-tenant"></a><span data-ttu-id="c5419-120">Lägg till en Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="c5419-120">Add an Azure AD tenant</span></span>
<span data-ttu-id="c5419-121">tooadd en Azure AD-klient i hello Azure-portalen, logga in för[hello Azure-portalen](https://portal.azure.com) med ett konto som är global administratör för Azure AD och hello vänster, Välj **ny**.</span><span class="sxs-lookup"><span data-stu-id="c5419-121">tooadd an Azure AD tenant in hello Azure portal, sign in too[hello Azure portal](https://portal.azure.com) with an account that is an Azure AD global administrator, and, on hello left, select **New**.</span></span>

> [!NOTE]
> <span data-ttu-id="c5419-122">Till skillnad från andra Azure-resurser är klienterna inte underordnade resurser till en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c5419-122">Unlike other Azure resources, your tenants are not child resources of an Azure subscription.</span></span> <span data-ttu-id="c5419-123">Om din Azure-prenumeration avbryts eller upphört att gälla, kan du fortfarande komma åt din klientdata med hjälp av Azure PowerShell, hello Azure Graph API eller hello Office 365 Admin Center.</span><span class="sxs-lookup"><span data-stu-id="c5419-123">If your Azure subscription is canceled or expired, you can still access your tenant data using Azure PowerShell, hello Azure Graph API, or hello Office 365 Admin Center.</span></span> <span data-ttu-id="c5419-124">Du kan även associera en annan prenumeration med hello innehavaren.</span><span class="sxs-lookup"><span data-stu-id="c5419-124">You can also associate another subscription with hello tenant.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="c5419-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5419-125">Next steps</span></span>
<span data-ttu-id="c5419-126">En översikt av Azure AD licensproblem och bästa praxis, se [vad är Azure Active-klient licensiering?](active-directory-licensing-whatis-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c5419-126">For a broad overview of Azure AD licensing issues and best practices, see [What is Azure Active tenant licensing?](active-directory-licensing-whatis-azure-portal.md)</span></span>
