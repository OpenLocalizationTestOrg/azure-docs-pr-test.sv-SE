---
title: "Azure Active Directory Domain Services: Skapa eller välja ett virtuellt nätverk | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hjälp av den klassiska Azure-portalen"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 13ab1608-e3d8-40de-9f7b-9b5b42199af4
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.openlocfilehash: 457519b00b65b0157effe2d4aba033a1c99852e8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a><span data-ttu-id="9bfa5-103">Skapa eller välja ett virtuellt nätverk för Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="9bfa5-103">Create or select a virtual network for Azure Active Directory Domain Services</span></span>
## <a name="before-you-begin"></a><span data-ttu-id="9bfa5-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9bfa5-104">Before you begin</span></span>
<span data-ttu-id="9bfa5-105">Se [Nätverksrelaterade aspekter att tänka på med Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="9bfa5-105">Refer to [Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>

## <a name="task-2-create-an-azure-virtual-network"></a><span data-ttu-id="9bfa5-106">Uppgift 2: Skapa ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="9bfa5-106">Task 2: Create an Azure virtual network</span></span>
<span data-ttu-id="9bfa5-107">Nästa konfigurationsåtgärd är att skapa ett virtuellt nätverk för Azure och ett undernät i nätverket.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-107">The next configuration task is to create an Azure virtual network and a subnet within it.</span></span> <span data-ttu-id="9bfa5-108">Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="9bfa5-109">Du kan hoppa över det här steget om du har ett befintligt virtuellt nätverk som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-109">If you have an existing virtual network that you’d prefer to use, you can skip this step.</span></span>

> [!NOTE]
> <span data-ttu-id="9bfa5-110">Se till att det virtuella Azure-nätverk som du skapar eller väljer att använda med Azure Active Directory Domain Services tillhör en Azure-region som stöds av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-110">Make sure that the Azure virtual network you create or choose to use with Azure Active Directory Domain Services belongs to an Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="9bfa5-111">Se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/) för att se i vilka Azure-regioner som Azure Active Directory Domain Services är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-111">To ascertain the Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
>
><span data-ttu-id="9bfa5-112">Skriv ned namnet på det virtuella nätverket för att säkerställa att du väljer rätt virtuellt nätverk när du aktiverar Azure Active Directory Domain Services i ett efterföljande konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-112">Note the name of the virtual network to ensure that you select the right virtual network when you enable Azure Active Directory Domain Services in a subsequent configuration step.</span></span>


<span data-ttu-id="9bfa5-113">Om du vill skapa ett virtuellt Azure-nätverk i vilket du vill aktivera Azure Active Directory Domain Services, följer du nedanstående konfigurationsinstruktioner:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-113">To create an Azure virtual network in which you want to enable Azure Active Directory Domain Services, follow these configuration instructions:</span></span>

1. <span data-ttu-id="9bfa5-114">Gå till den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9bfa5-114">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9bfa5-115">Välj **Nätverk** i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-115">In the left pane, select **Networks**.</span></span>

    <span data-ttu-id="9bfa5-116">![Nätverksnod](./media/active-directory-domain-services-getting-started/networks-node.png)</span><span class="sxs-lookup"><span data-stu-id="9bfa5-116">![Networks node](./media/active-directory-domain-services-getting-started/networks-node.png)</span></span>  
    <span data-ttu-id="9bfa5-117">Fönstret **Virtuella nätverk** öppnas.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-117">The **Virtual Networks** window opens.</span></span>
3. <span data-ttu-id="9bfa5-118">Klicka på **Nytt** i åtgärdsrutan längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-118">In the task pane at the bottom of the window, click **New**.</span></span>

    ![Fönstret Virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. <span data-ttu-id="9bfa5-120">Klicka på **Nätverkstjänster** och välj sedan **Virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-120">Click **Network Services**, and then select **Virtual Network**.</span></span>

    ![Virtuellt nätverk – snabbregistrering](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. <span data-ttu-id="9bfa5-122">Klicka på **Snabbregistrering** för att skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-122">To create a virtual network, click **Quick Create**.</span></span>

6. <span data-ttu-id="9bfa5-123">Ange ett **Namn** för ditt virtuella nätverk och överväg att göra något av följande:</span><span class="sxs-lookup"><span data-stu-id="9bfa5-123">Specify a **Name** for your virtual network, and consider doing the following:</span></span>
    * <span data-ttu-id="9bfa5-124">Du kan välja att konfigurera **Adressutrymme** eller **Maximalt antal virtuella datorer** för det här nätverket.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-124">You can choose to configure **Address space** or **Maximum VM count** for this network.</span></span>
    * <span data-ttu-id="9bfa5-125">Du kan lämna inställningen för **DNS-server** som **Ingen** för tillfället.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-125">You can leave the **DNS server** setting as **None** for now.</span></span> <span data-ttu-id="9bfa5-126">Du kan uppdatera inställningen när du har aktiverat Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-126">You can update the setting after you enable Azure Active Directory Domain Services.</span></span>
7. <span data-ttu-id="9bfa5-127">Välj en Azure-region i den nedrullningsbara listan **Plats**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-127">In the **Location** drop-down list, select a supported Azure region.</span></span>  
    <span data-ttu-id="9bfa5-128">Se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/) för att se i vilka Azure-regioner som Azure Active Directory Domain Services är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-128">To ascertain the Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
8. <span data-ttu-id="9bfa5-129">Klicka på **Skapa ett virtuellt nätverk** för att skapa ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-129">To create your virtual network, click **Create a Virtual Network**.</span></span>

    ![Skapa ett virtuellt nätverk för Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. <span data-ttu-id="9bfa5-131">När det virtuella nätverket har skapats väljer du namn för det och klickar på fliken **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-131">After you've created a virtual network, select the name of the virtual network, and then click the **Configure** tab.</span></span>

    ![Skapa ett undernät](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. <span data-ttu-id="9bfa5-133">Under **adressutrymmen för virtuella nätverk** klickar du på **lägg till undernät** och anger sedan ett undernät med namnet **AaddsSubnet**.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-133">Under **virtual network address spaces**, click **add subnet**, and then specify a subnet with the name **AaddsSubnet**.</span></span>

    ![Skapa ett undernät för Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. <span data-ttu-id="9bfa5-135">Klicka på **Spara** för att skapa undernätet.</span><span class="sxs-lookup"><span data-stu-id="9bfa5-135">To create the subnet, click **Save**.</span></span>


## <a name="next-step"></a><span data-ttu-id="9bfa5-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bfa5-136">Next step</span></span>
[<span data-ttu-id="9bfa5-137">Uppgift 3: Aktivera Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="9bfa5-137">Task 3: enable Azure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-enableaadds.md)
