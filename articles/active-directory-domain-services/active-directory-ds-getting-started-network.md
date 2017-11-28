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
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7695dabb11df8d9dcfdac24996edf021af2e7f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="2ce78-103">Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="2ce78-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="2ce78-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2ce78-104">Before you begin</span></span>
<span data-ttu-id="2ce78-105">Se för[nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="2ce78-105">Refer too[Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>


## <a name="task-2-configure-network-settings"></a><span data-ttu-id="2ce78-106">Uppgift 2: konfigurera nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="2ce78-106">Task 2: configure network settings</span></span>
<span data-ttu-id="2ce78-107">hello nästa konfigurationsåtgärd är toocreate virtuellt Azure-nätverk och en dedikerad undernät i den.</span><span class="sxs-lookup"><span data-stu-id="2ce78-107">hello next configuration task is toocreate an Azure virtual network and a dedicated subnet within it.</span></span> <span data-ttu-id="2ce78-108">Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ce78-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="2ce78-109">Du kan också välja ett befintligt virtuellt nätverk och skapa hello dedikerad undernät i den.</span><span class="sxs-lookup"><span data-stu-id="2ce78-109">You may also pick an existing virtual network and create hello dedicated subnet within it.</span></span>

1. <span data-ttu-id="2ce78-110">Klicka på **för virtuella nätverk** tooselect ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ce78-110">Click **Virtual network** tooselect a virtual network.</span></span>
2. <span data-ttu-id="2ce78-111">På hello **Välj virtuellt nätverk** bladet visas alla befintliga virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ce78-111">On hello **Choose virtual network** blade, you see all existing virtual networks.</span></span> <span data-ttu-id="2ce78-112">Du ser endast virtuella nätverk för hello som tillhör toohello resursgruppen och Azure-plats som du har valt på hello **grunderna** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="2ce78-112">You see only hello virtual networks that belong toohello resource group and Azure location you have selected on hello **Basics** wizard page.</span></span>

3. <span data-ttu-id="2ce78-113">Välj hello virtuellt nätverk där Azure AD Domain Services ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="2ce78-113">Choose hello virtual network in which Azure AD Domain Services should be enabled.</span></span> <span data-ttu-id="2ce78-114">Klicka på **Skapa nytt**, om du föredrar toocreate ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ce78-114">Click **Create new**, if you prefer toocreate a new virtual network.</span></span> <span data-ttu-id="2ce78-115">Vi rekommenderar starkt att använda ett dedikerat undernät för Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="2ce78-115">We highly recommend using a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="2ce78-116">Om du väljer ett befintligt virtuellt nätverk [skapa ett dedikerat undernät använder hello tillägg för virtuella nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) och välj sedan det undernätet.</span><span class="sxs-lookup"><span data-stu-id="2ce78-116">If you pick an existing virtual network, [create a dedicated subnet using hello virtual networks extension](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) and then pick that subnet.</span></span> 

    ![Välj virtuella nätverk](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. <span data-ttu-id="2ce78-118">Klicka på **undernät** toopick hello dedikerad undernät i det här virtuella nätverket, inom vilka tooenable din nya hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="2ce78-118">Click **Subnet** toopick hello dedicated subnet in this virtual network, within which tooenable your new managed domain.</span></span> <span data-ttu-id="2ce78-119">I hello **skapa undernät** bladet, ange ett namn för hello undernätet och klickar på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="2ce78-119">In hello **Create subnet** blade, specify a name for hello subnet, and click **OK** when you're done.</span></span> <span data-ttu-id="2ce78-120">Till exempel skapa ett undernät med namnet hello DomainServices, vilket gör det enkelt för andra administratörer toounderstand vad distribueras inom hello undernät.</span><span class="sxs-lookup"><span data-stu-id="2ce78-120">For example, create a subnet with hello name 'DomainServices', making it easy for other administrators toounderstand what is deployed within hello subnet.</span></span>

    ![Välj undernät inom hello virtuellt nätverk](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > <span data-ttu-id="2ce78-122">**Riktlinjer för att välja ett undernät**</span><span class="sxs-lookup"><span data-stu-id="2ce78-122">**Guidelines for selecting a subnet**</span></span>
  > 1. <span data-ttu-id="2ce78-123">Använd en dedikerad undernät för Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="2ce78-123">Use a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="2ce78-124">Distribuera inte alla andra virtuella datorer toothis undernät.</span><span class="sxs-lookup"><span data-stu-id="2ce78-124">Do not deploy any other virtual machines toothis subnet.</span></span> <span data-ttu-id="2ce78-125">Den här konfigurationen kan du tooconfigure nätverkssäkerhetsgrupper (NSG: er) för dina arbetsbelastningar för virtuella datorer utan att avbryta din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="2ce78-125">This configuration enables you tooconfigure network security groups (NSGs) for your workloads/virtual machines without disrupting your managed domain.</span></span> <span data-ttu-id="2ce78-126">Mer information finns i [nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="2ce78-126">For details, see [networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>
  2. <span data-ttu-id="2ce78-127">Välj inte hello Gateway-undernät för att distribuera Azure AD Domain Services, eftersom den inte är en konfiguration som stöds.</span><span class="sxs-lookup"><span data-stu-id="2ce78-127">Do not select hello Gateway subnet for deploying Azure AD Domain Services, because it is not a supported configuration.</span></span>
  3. <span data-ttu-id="2ce78-128">Se till att hello undernät som du har valt har tillräckligt utrymme för tillgängliga adresser - minst 3-5 tillgängliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="2ce78-128">Ensure that hello subnet you've selected has sufficient available address space - at least 3-5 available IP addresses.</span></span>
  >

5. <span data-ttu-id="2ce78-129">När du är klar klickar du på **OK** toomove på toohello **administratörsgruppen** hello guiden.</span><span class="sxs-lookup"><span data-stu-id="2ce78-129">When you are done, click **OK** toomove on toohello **Administrator group** page of hello wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="2ce78-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ce78-130">Next step</span></span>
[<span data-ttu-id="2ce78-131">Uppgift 3: Konfigurera administrativ grupp och aktivera Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="2ce78-131">Task 3: configure administrative group and enable Azure AD Domain Services</span></span>](active-directory-ds-getting-started-admingroup.md)
