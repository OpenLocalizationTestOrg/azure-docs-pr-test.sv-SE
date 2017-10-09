---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)"
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
ms.openlocfilehash: 79cbb21c4a50194f5ad8ca1a4a8493ee4a260a9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="6c32a-103">Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="6c32a-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>
<span data-ttu-id="6c32a-104">Den här artikeln visar hur tooenable Azure Active Directory Domain Services (Azure AD DS) med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c32a-104">This article shows how tooenable Azure Active Directory Domain Services (Azure AD DS) using hello Azure portal.</span></span>


<span data-ttu-id="6c32a-105">toolaunch hello **aktivera Azure AD Domain Services** guiden, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6c32a-105">toolaunch hello **Enable Azure AD Domain Services** wizard, complete hello following steps:</span></span>

1. <span data-ttu-id="6c32a-106">Gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6c32a-106">Go toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6c32a-107">Hello vänster, klicka på **ny**.</span><span class="sxs-lookup"><span data-stu-id="6c32a-107">In hello left pane, click on **New**.</span></span>
3. <span data-ttu-id="6c32a-108">I hello **ny** bladet, skriver **Domain Services** i hello sökfältet.</span><span class="sxs-lookup"><span data-stu-id="6c32a-108">In hello **New** blade, type **Domain Services** into hello search bar.</span></span>

    ![Sök efter domäntjänster](./media/getting-started/search-domain-services.png)

4. <span data-ttu-id="6c32a-110">Klicka på tooselect **Azure AD Domain Services** hello listan över sökförslag.</span><span class="sxs-lookup"><span data-stu-id="6c32a-110">Click tooselect **Azure AD Domain Services** from hello list of search suggestions.</span></span> <span data-ttu-id="6c32a-111">På hello **Azure AD Domain Services** bladet, klickar du på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="6c32a-111">On hello **Azure AD Domain Services** blade, click hello **Create** button.</span></span>

    ![Domain services-bladet](./media/getting-started/domain-services-blade.png)

5. <span data-ttu-id="6c32a-113">Hej **aktivera Azure AD Domain Services** starta guiden.</span><span class="sxs-lookup"><span data-stu-id="6c32a-113">hello **Enable Azure AD Domain Services** wizard is launched.</span></span>


## <a name="task-1-configure-basic-settings"></a><span data-ttu-id="6c32a-114">Uppgift 1: Konfigurera grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="6c32a-114">Task 1: configure basic settings</span></span>
<span data-ttu-id="6c32a-115">I hello **grunderna** sidan för hello guiden kan du ange hello DNS-domännamn för hello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="6c32a-115">In hello **Basics** page of hello wizard, you can specify hello DNS domain name for hello managed domain.</span></span> <span data-ttu-id="6c32a-116">Du kan också välja hello resursgruppen och Azure-plats toowhich hello hanterad domän som ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="6c32a-116">You can also choose hello resource group and Azure location toowhich hello managed domain should be deployed.</span></span>

![Konfigurera grunderna](./media/getting-started/domain-services-blade-basics.png)

1. <span data-ttu-id="6c32a-118">Välj hello **DNS-domännamn** för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="6c32a-118">Choose hello **DNS domain name** for your managed domain.</span></span>

   * <span data-ttu-id="6c32a-119">hello standarddomännamnet för hello katalogen (med en **. onmicrosoft.com** suffix) har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="6c32a-119">hello default domain name of hello directory (with a **.onmicrosoft.com** suffix) is specified by default.</span></span>

   * <span data-ttu-id="6c32a-120">Du kan också skriva i ett anpassat domännamn.</span><span class="sxs-lookup"><span data-stu-id="6c32a-120">You can also type in a custom domain name.</span></span> <span data-ttu-id="6c32a-121">I det här exemplet hello domännamn är *contoso100.com*.</span><span class="sxs-lookup"><span data-stu-id="6c32a-121">In this example, hello custom domain name is *contoso100.com*.</span></span>

     > [!WARNING]
     > <span data-ttu-id="6c32a-122">hello-prefixet för ett angivet domännamn (till exempel *contoso100* i hello *contoso100.com* domännamn) får innehålla högst 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="6c32a-122">hello prefix of your specified domain name (for example, *contoso100* in hello *contoso100.com* domain name) must contain 15 or fewer characters.</span></span> <span data-ttu-id="6c32a-123">Du kan inte skapa en hanterad domän med ett prefix som är längre än 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="6c32a-123">You cannot create a managed domain with a prefix longer than 15 characters.</span></span>
     >
     >

2. <span data-ttu-id="6c32a-124">Se till att hello DNS-domännamn som du har valt för hello hanterade domänen inte redan finns i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c32a-124">Ensure that hello DNS domain name you have chosen for hello managed domain does not already exist in hello virtual network.</span></span> <span data-ttu-id="6c32a-125">I synnerhet kontrollera om:</span><span class="sxs-lookup"><span data-stu-id="6c32a-125">Specifically, check whether:</span></span>

   * <span data-ttu-id="6c32a-126">Du redan har en domän med hello samma DNS-domännamn på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c32a-126">You already have a domain with hello same DNS domain name on hello virtual network.</span></span>

   * <span data-ttu-id="6c32a-127">hello virtuellt nätverk där du planerar att tooenable hello hanterade domänen har en VPN-anslutning med ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c32a-127">hello virtual network where you plan tooenable hello managed domain has a VPN connection with your on-premises network.</span></span> <span data-ttu-id="6c32a-128">I det här scenariot, se till att du inte har en domän med hello samma DNS-namnet på ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c32a-128">In this scenario, ensure you don't have a domain with hello same DNS domain name on your on-premises network.</span></span>

   * <span data-ttu-id="6c32a-129">Du har en befintlig molntjänst med det namnet på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="6c32a-129">You have an existing cloud service with that name on hello virtual network.</span></span>

3. <span data-ttu-id="6c32a-130">Välj hello **typen av virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="6c32a-130">Choose hello **type of virtual network**.</span></span> <span data-ttu-id="6c32a-131">Som standard hello **Resource Manager** typ virtuella nätverk som väljs.</span><span class="sxs-lookup"><span data-stu-id="6c32a-131">By default, hello **Resource Manager** virtual network type is selected.</span></span> <span data-ttu-id="6c32a-132">Vi rekommenderar att du använder den här typen av virtuellt nätverk för alla nyligen skapade hanterade domäner.</span><span class="sxs-lookup"><span data-stu-id="6c32a-132">We recommend using this type of virtual network for all newly created managed domains.</span></span>

4. <span data-ttu-id="6c32a-133">Välj hello Azure **prenumeration** som du vill att toocreate hello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="6c32a-133">Select hello Azure **Subscription** in which you would like toocreate hello managed domain.</span></span>

5. <span data-ttu-id="6c32a-134">Välj hello **resursgruppen** toowhich hello hanterad domän ska tillhöra.</span><span class="sxs-lookup"><span data-stu-id="6c32a-134">Select hello **Resource group** toowhich hello managed domain should belong.</span></span> <span data-ttu-id="6c32a-135">Du kan välja antingen hello **Skapa nytt** eller **använda befintliga** alternativ tooselect hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6c32a-135">You can choose either hello **Create new** or **Use existing** options tooselect hello resource group.</span></span>

6. <span data-ttu-id="6c32a-136">Välj hello Azure **plats** i vilka hello hanterad domän ska skapas.</span><span class="sxs-lookup"><span data-stu-id="6c32a-136">Choose hello Azure **Location** in which hello managed domain should be created.</span></span> <span data-ttu-id="6c32a-137">På hello **nätverk** sidan hello guiden kan du se endast virtuella nätverk som tillhör toohello plats som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6c32a-137">On hello **Network** page of hello wizard, you see only virtual networks that belong toohello location you have selected.</span></span>

7. <span data-ttu-id="6c32a-138">När du är klar klickar du på **OK** toomove på toohello **nätverk** hello guiden.</span><span class="sxs-lookup"><span data-stu-id="6c32a-138">When you are done, click **OK** toomove on toohello **Network** page of hello wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="6c32a-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c32a-139">Next step</span></span>
[<span data-ttu-id="6c32a-140">Uppgift 2: Konfigurera nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="6c32a-140">Task 2: configure network settings</span></span>](active-directory-ds-getting-started-network.md)
