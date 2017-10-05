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
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="1f474-103">Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="1f474-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="1f474-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1f474-104">Before you begin</span></span>
<span data-ttu-id="1f474-105">Se [Nätverksrelaterade aspekter att tänka på med Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="1f474-105">Refer to [Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>


## <a name="task-2-configure-network-settings"></a><span data-ttu-id="1f474-106">Uppgift 2: konfigurera nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="1f474-106">Task 2: configure network settings</span></span>
<span data-ttu-id="1f474-107">Nästa konfigurationsåtgärd är att skapa ett virtuellt Azure-nätverk och en dedikerad undernät i den.</span><span class="sxs-lookup"><span data-stu-id="1f474-107">The next configuration task is to create an Azure virtual network and a dedicated subnet within it.</span></span> <span data-ttu-id="1f474-108">Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f474-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="1f474-109">Du kan också välja ett befintligt virtuellt nätverk och skapa dedikerade undernätet i den.</span><span class="sxs-lookup"><span data-stu-id="1f474-109">You may also pick an existing virtual network and create the dedicated subnet within it.</span></span>

1. <span data-ttu-id="1f474-110">Klicka på **för virtuella nätverk** att välja ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f474-110">Click **Virtual network** to select a virtual network.</span></span>
2. <span data-ttu-id="1f474-111">På den **Välj virtuellt nätverk** bladet visas alla befintliga virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f474-111">On the **Choose virtual network** blade, you see all existing virtual networks.</span></span> <span data-ttu-id="1f474-112">Du ser bara de virtuella nätverk som tillhör resursgruppen och Azure-plats som du har valt på den **grunderna** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="1f474-112">You see only the virtual networks that belong to the resource group and Azure location you have selected on the **Basics** wizard page.</span></span>

3. <span data-ttu-id="1f474-113">Välj det virtuella nätverket som Azure AD Domain Services ska aktiveras.</span><span class="sxs-lookup"><span data-stu-id="1f474-113">Choose the virtual network in which Azure AD Domain Services should be enabled.</span></span> <span data-ttu-id="1f474-114">Klicka på **Skapa nytt**, om du vill skapa ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1f474-114">Click **Create new**, if you prefer to create a new virtual network.</span></span> <span data-ttu-id="1f474-115">Vi rekommenderar starkt att använda ett dedikerat undernät för Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="1f474-115">We highly recommend using a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="1f474-116">Om du väljer ett befintligt virtuellt nätverk [skapa ett dedikerat undernät med virtuella nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) och välj sedan det undernätet.</span><span class="sxs-lookup"><span data-stu-id="1f474-116">If you pick an existing virtual network, [create a dedicated subnet using the virtual networks extension](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) and then pick that subnet.</span></span> 

    ![Välj virtuella nätverk](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. <span data-ttu-id="1f474-118">Klicka på **undernät** att välja dedikerad undernät i det här virtuella nätverket att aktivera din nya hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="1f474-118">Click **Subnet** to pick the dedicated subnet in this virtual network, within which to enable your new managed domain.</span></span> <span data-ttu-id="1f474-119">I den **skapa undernät** bladet, anger ett namn för undernätet och klickar på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="1f474-119">In the **Create subnet** blade, specify a name for the subnet, and click **OK** when you're done.</span></span> <span data-ttu-id="1f474-120">Till exempel skapa ett undernät med namnet DomainServices, vilket gör det lättare för andra administratörer att förstå vad som har distribuerats i undernätet.</span><span class="sxs-lookup"><span data-stu-id="1f474-120">For example, create a subnet with the name 'DomainServices', making it easy for other administrators to understand what is deployed within the subnet.</span></span>

    ![Välj undernät i det virtuella nätverket](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > <span data-ttu-id="1f474-122">**Riktlinjer för att välja ett undernät**</span><span class="sxs-lookup"><span data-stu-id="1f474-122">**Guidelines for selecting a subnet**</span></span>
  > 1. <span data-ttu-id="1f474-123">Använd en dedikerad undernät för Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="1f474-123">Use a dedicated subnet for Azure AD Domain Services.</span></span> <span data-ttu-id="1f474-124">Distribuera inte eventuella andra virtuella datorer till det här undernätet.</span><span class="sxs-lookup"><span data-stu-id="1f474-124">Do not deploy any other virtual machines to this subnet.</span></span> <span data-ttu-id="1f474-125">Den här konfigurationen kan du konfigurera nätverkssäkerhetsgrupper (NSG: er) för dina arbetsbelastningar för virtuella datorer utan att avbryta din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="1f474-125">This configuration enables you to configure network security groups (NSGs) for your workloads/virtual machines without disrupting your managed domain.</span></span> <span data-ttu-id="1f474-126">Mer information finns i [nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="1f474-126">For details, see [networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>
  2. <span data-ttu-id="1f474-127">Välj inte Gateway-undernätet för att distribuera Azure AD Domain Services, eftersom den inte är en konfiguration som stöds.</span><span class="sxs-lookup"><span data-stu-id="1f474-127">Do not select the Gateway subnet for deploying Azure AD Domain Services, because it is not a supported configuration.</span></span>
  3. <span data-ttu-id="1f474-128">Kontrollera att det undernät som du har valt har tillräckligt utrymme för tillgängliga adresser - minst 3-5 tillgängliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="1f474-128">Ensure that the subnet you've selected has sufficient available address space - at least 3-5 available IP addresses.</span></span>
  >

5. <span data-ttu-id="1f474-129">När du är klar klickar du på **OK** att gå vidare till den **administratörsgruppen** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="1f474-129">When you are done, click **OK** to move on to the **Administrator group** page of the wizard.</span></span>


## <a name="next-step"></a><span data-ttu-id="1f474-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f474-130">Next step</span></span>
[<span data-ttu-id="1f474-131">Uppgift 3: Konfigurera administrativ grupp och aktivera Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="1f474-131">Task 3: configure administrative group and enable Azure AD Domain Services</span></span>](active-directory-ds-getting-started-admingroup.md)
