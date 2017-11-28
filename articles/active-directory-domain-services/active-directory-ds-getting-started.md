---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: maheshu
ms.openlocfilehash: 47507096a6245d4f1ba57a652ddf5167b3776ae9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="e22f1-103">Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="e22f1-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>
<span data-ttu-id="e22f1-104">Den här artikeln visar hur du aktiverar Azure Active Directory Domain Services (Azure AD DS) med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e22f1-104">This article shows how to enable Azure Active Directory Domain Services (Azure AD DS) using the Azure portal.</span></span>


<span data-ttu-id="e22f1-105">Att starta den **aktivera Azure AD Domain Services** guiden gör du följande:</span><span class="sxs-lookup"><span data-stu-id="e22f1-105">To launch the **Enable Azure AD Domain Services** wizard, complete the following steps:</span></span>

1. <span data-ttu-id="e22f1-106">Gå till [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e22f1-106">Go to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e22f1-107">I den vänstra rutan klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="e22f1-107">In the left pane, click on **New**.</span></span>
3. <span data-ttu-id="e22f1-108">I den **ny** bladet, skriver **Domain Services** i sökfältet.</span><span class="sxs-lookup"><span data-stu-id="e22f1-108">In the **New** blade, type **Domain Services** into the search bar.</span></span>

    ![Sök efter domäntjänster](./media/getting-started/search-domain-services.png)

4. <span data-ttu-id="e22f1-110">Välj **Azure AD Domain Services** från listan över sökförslag.</span><span class="sxs-lookup"><span data-stu-id="e22f1-110">Click to select **Azure AD Domain Services** from the list of search suggestions.</span></span> <span data-ttu-id="e22f1-111">På den **Azure AD Domain Services** bladet, klickar du på den **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="e22f1-111">On the **Azure AD Domain Services** blade, click the **Create** button.</span></span>

    ![Domain services-bladet](./media/getting-started/domain-services-blade.png)

5. <span data-ttu-id="e22f1-113">Den **aktivera Azure AD Domain Services** starta guiden.</span><span class="sxs-lookup"><span data-stu-id="e22f1-113">The **Enable Azure AD Domain Services** wizard is launched.</span></span>


## <a name="task-1-configure-basic-settings"></a><span data-ttu-id="e22f1-114">Uppgift 1: Konfigurera grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="e22f1-114">Task 1: configure basic settings</span></span>
<span data-ttu-id="e22f1-115">I den **grunderna** sidan i guiden kan du ange DNS-domännamnet för den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="e22f1-115">In the **Basics** page of the wizard, you can specify the DNS domain name for the managed domain.</span></span> <span data-ttu-id="e22f1-116">Du kan också välja resursgruppen och Azure-plats som den hanterade domänen ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="e22f1-116">You can also choose the resource group and Azure location to which the managed domain should be deployed.</span></span>

![Konfigurera grunderna](./media/getting-started/domain-services-blade-basics.png)

1. <span data-ttu-id="e22f1-118">Välj den **DNS-domännamn** för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="e22f1-118">Choose the **DNS domain name** for your managed domain.</span></span>

   * <span data-ttu-id="e22f1-119">Standarddomännamnet för katalogen (med en **. onmicrosoft.com** suffix) har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="e22f1-119">The default domain name of the directory (with a **.onmicrosoft.com** suffix) is specified by default.</span></span>

   * <span data-ttu-id="e22f1-120">Du kan också skriva i ett anpassat domännamn.</span><span class="sxs-lookup"><span data-stu-id="e22f1-120">You can also type in a custom domain name.</span></span> <span data-ttu-id="e22f1-121">I det här exemplet är det anpassade domännamnet *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="e22f1-121">In this example, the custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="e22f1-122">Prefixet för det angivna domännamnet (till exempel *contoso100* i domännamnet *contoso100.com*) kan innehålla upp till 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="e22f1-122">The prefix of your specified domain name (for example, *contoso100* in the *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="e22f1-123">Du kan inte skapa en hanterad domän med ett prefix som är längre än 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="e22f1-123">You cannot create a managed domain with a prefix longer than 15 characters.</span></span>
     >
     >

2. <span data-ttu-id="e22f1-124">Kontrollera att DNS-domännamnet som du har valt för den hanterade domänen inte redan finns i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="e22f1-124">Ensure that the DNS domain name you have chosen for the managed domain does not already exist in the virtual network.</span></span> <span data-ttu-id="e22f1-125">I synnerhet kontrollera om:</span><span class="sxs-lookup"><span data-stu-id="e22f1-125">Specifically, check whether:</span></span>

   * <span data-ttu-id="e22f1-126">Du redan har en domän med samma DNS-domännamn i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="e22f1-126">You already have a domain with the same DNS domain name on the virtual network.</span></span>

   * <span data-ttu-id="e22f1-127">Det virtuella nätverket där du planerar att aktivera den hanterade domänen har en VPN-anslutning med ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="e22f1-127">The virtual network where you plan to enable the managed domain has a VPN connection with your on-premises network.</span></span> <span data-ttu-id="e22f1-128">I det här scenariot, se till att du inte har en domän med samma DNS-domännamnet i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="e22f1-128">In this scenario, ensure you don't have a domain with the same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="e22f1-129">Du har en befintlig molntjänst med det namnet i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="e22f1-129">You have an existing cloud service with that name on the virtual network.</span></span>

3. <span data-ttu-id="e22f1-130">Välj den **typen av virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="e22f1-130">Choose the **type of virtual network**.</span></span> <span data-ttu-id="e22f1-131">Som standard den **Resource Manager** typ virtuella nätverk som väljs.</span><span class="sxs-lookup"><span data-stu-id="e22f1-131">By default, the **Resource Manager** virtual network type is selected.</span></span> <span data-ttu-id="e22f1-132">Vi rekommenderar att du använder den här typen av virtuellt nätverk för alla nyligen skapade hanterade domäner.</span><span class="sxs-lookup"><span data-stu-id="e22f1-132">We recommend using this type of virtual network for all newly created managed domains.</span></span>

4. <span data-ttu-id="e22f1-133">Välj Azure **prenumeration** i som du vill skapa den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="e22f1-133">Select the Azure **Subscription** in which you would like to create the managed domain.</span></span>

5. <span data-ttu-id="e22f1-134">Välj den **resursgruppen** till den hanterade domänen ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="e22f1-134">Select the **Resource group** to which the managed domain should belong.</span></span> <span data-ttu-id="e22f1-135">Du kan välja antingen den **Skapa nytt** eller **använda befintliga** alternativ för att välja en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e22f1-135">You can choose either the **Create new** or **Use existing** options to select the resource group.</span></span>

6. <span data-ttu-id="e22f1-136">Välj Azure **plats** i som den hanterade domänen ska skapas.</span><span class="sxs-lookup"><span data-stu-id="e22f1-136">Choose the Azure **Location** in which the managed domain should be created.</span></span> <span data-ttu-id="e22f1-137">På den **nätverk** sidan i guiden visas bara virtuella nätverk som hör till den plats som du har valt.</span><span class="sxs-lookup"><span data-stu-id="e22f1-137">On the **Network** page of the wizard, you see only virtual networks that belong to the location you have selected.</span></span>

7. <span data-ttu-id="e22f1-138">När du är klar klickar du på **OK** att gå vidare till den **nätverk** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="e22f1-138">When you are done, click **OK** to move on to the **Network** page of the wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="e22f1-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e22f1-139">Next step</span></span>
[<span data-ttu-id="e22f1-140">Uppgift 2: Konfigurera nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="e22f1-140">Task 2: configure network settings</span></span>](active-directory-ds-getting-started-network.md)
