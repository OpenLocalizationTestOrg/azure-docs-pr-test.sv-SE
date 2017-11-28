---
title: "Azure Active Directory Domain Services: Skapa eller välja ett virtuellt nätverk | Microsoft Docs"
description: Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen
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
ms.openlocfilehash: 212c741b20e846742d94f70342c4263ce8984153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-or-select-a-virtual-network-for-azure-active-directory-domain-services"></a><span data-ttu-id="48699-103">Skapa eller välja ett virtuellt nätverk för Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="48699-103">Create or select a virtual network for Azure Active Directory Domain Services</span></span>
## <a name="before-you-begin"></a><span data-ttu-id="48699-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="48699-104">Before you begin</span></span>
<span data-ttu-id="48699-105">Se för[nätverk överväganden för Azure Active Directory Domain Services](active-directory-ds-networking.md).</span><span class="sxs-lookup"><span data-stu-id="48699-105">Refer too[Networking considerations for Azure Active Directory Domain Services](active-directory-ds-networking.md).</span></span>

## <a name="task-2-create-an-azure-virtual-network"></a><span data-ttu-id="48699-106">Uppgift 2: Skapa ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="48699-106">Task 2: Create an Azure virtual network</span></span>
<span data-ttu-id="48699-107">hello nästa konfigurationsåtgärd är toocreate virtuellt Azure-nätverk och ett undernät i den.</span><span class="sxs-lookup"><span data-stu-id="48699-107">hello next configuration task is toocreate an Azure virtual network and a subnet within it.</span></span> <span data-ttu-id="48699-108">Du aktiverar Azure Active Directory Domain Services i detta undernät inom ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="48699-108">You enable Azure Active Directory Domain Services in this subnet within your virtual network.</span></span> <span data-ttu-id="48699-109">Om du har ett befintligt virtuellt nätverk som du vill använda toouse kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="48699-109">If you have an existing virtual network that you’d prefer toouse, you can skip this step.</span></span>

> [!NOTE]
> <span data-ttu-id="48699-110">Se till att hello Azure-nätverket som du skapar eller väljer toouse med Azure Active Directory Domain Services tillhör tooan Azure-region som stöds av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="48699-110">Make sure that hello Azure virtual network you create or choose toouse with Azure Active Directory Domain Services belongs tooan Azure region that's supported by Azure Active Directory Domain Services.</span></span> <span data-ttu-id="48699-111">tooascertain hello Azure-regioner som Azure Active Directory Domain Services är tillgängligt, se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="48699-111">tooascertain hello Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
>
><span data-ttu-id="48699-112">Observera hello namnet på hello virtuellt nätverk tooensure att du väljer hello rätt virtuellt nätverk när du aktiverar Azure Active Directory Domain Services i ett efterföljande konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="48699-112">Note hello name of hello virtual network tooensure that you select hello right virtual network when you enable Azure Active Directory Domain Services in a subsequent configuration step.</span></span>


<span data-ttu-id="48699-113">toocreate virtuellt Azure-nätverk som du vill tooenable Azure Active Directory Domain Services, följer du dessa konfigurationsinstruktioner:</span><span class="sxs-lookup"><span data-stu-id="48699-113">toocreate an Azure virtual network in which you want tooenable Azure Active Directory Domain Services, follow these configuration instructions:</span></span>

1. <span data-ttu-id="48699-114">Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="48699-114">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="48699-115">I hello vänster och välj **nätverk**.</span><span class="sxs-lookup"><span data-stu-id="48699-115">In hello left pane, select **Networks**.</span></span>

    <span data-ttu-id="48699-116">![Nätverksnod](./media/active-directory-domain-services-getting-started/networks-node.png)</span><span class="sxs-lookup"><span data-stu-id="48699-116">![Networks node](./media/active-directory-domain-services-getting-started/networks-node.png)</span></span>  
    <span data-ttu-id="48699-117">Hej **virtuella nätverk** öppnas.</span><span class="sxs-lookup"><span data-stu-id="48699-117">hello **Virtual Networks** window opens.</span></span>
3. <span data-ttu-id="48699-118">Hello i åtgärdsfönstret längst ned hello hello-fönstret klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="48699-118">In hello task pane at hello bottom of hello window, click **New**.</span></span>

    ![Fönstret Virtuellt nätverk](./media/active-directory-domain-services-getting-started/virtual-networks.png)
4. <span data-ttu-id="48699-120">Klicka på **Nätverkstjänster** och välj sedan **Virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="48699-120">Click **Network Services**, and then select **Virtual Network**.</span></span>

    ![Virtuellt nätverk – snabbregistrering](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)
5. <span data-ttu-id="48699-122">toocreate ett virtuellt nätverk, klickar du på **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="48699-122">toocreate a virtual network, click **Quick Create**.</span></span>

6. <span data-ttu-id="48699-123">Ange en **namn** för din virtuella nätverks- och bör du göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="48699-123">Specify a **Name** for your virtual network, and consider doing hello following:</span></span>
    * <span data-ttu-id="48699-124">Du kan välja tooconfigure **adressutrymmet** eller **maximalt antal Virtuella datorer** för det här nätverket.</span><span class="sxs-lookup"><span data-stu-id="48699-124">You can choose tooconfigure **Address space** or **Maximum VM count** for this network.</span></span>
    * <span data-ttu-id="48699-125">Du kan lämna hello **DNS-server** som **ingen** just nu.</span><span class="sxs-lookup"><span data-stu-id="48699-125">You can leave hello **DNS server** setting as **None** for now.</span></span> <span data-ttu-id="48699-126">Du kan uppdatera hello-inställningen när du har aktiverat Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="48699-126">You can update hello setting after you enable Azure Active Directory Domain Services.</span></span>
7. <span data-ttu-id="48699-127">I hello **plats** listrutan väljer du en Azure-regionen stöds.</span><span class="sxs-lookup"><span data-stu-id="48699-127">In hello **Location** drop-down list, select a supported Azure region.</span></span>  
    <span data-ttu-id="48699-128">tooascertain hello Azure-regioner som Azure Active Directory Domain Services är tillgängligt, se [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/).</span><span class="sxs-lookup"><span data-stu-id="48699-128">tooascertain hello Azure regions in which Azure Active Directory Domain Services is available, see [Azure services by region](https://azure.microsoft.com/regions/#services/).</span></span>
8. <span data-ttu-id="48699-129">toocreate ditt virtuella nätverk, klickar du på **skapa ett virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="48699-129">toocreate your virtual network, click **Create a Virtual Network**.</span></span>

    ![Skapa ett virtuellt nätverk för Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet.png)
9. <span data-ttu-id="48699-131">När du har skapat ett virtuellt nätverk, Välj hello hello virtuella nätverk och klicka sedan på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="48699-131">After you've created a virtual network, select hello name of hello virtual network, and then click hello **Configure** tab.</span></span>

    ![Skapa ett undernät](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)
10. <span data-ttu-id="48699-133">Under **virtuellt nätverk adressutrymmen**, klickar du på **Lägg till undernät**, och ange ett undernät med namnet hello **AaddsSubnet**.</span><span class="sxs-lookup"><span data-stu-id="48699-133">Under **virtual network address spaces**, click **add subnet**, and then specify a subnet with hello name **AaddsSubnet**.</span></span>

    ![Skapa ett undernät för Azure Active Directory Domain Services](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)

11. <span data-ttu-id="48699-135">toocreate Hej undernät, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="48699-135">toocreate hello subnet, click **Save**.</span></span>


## <a name="next-step"></a><span data-ttu-id="48699-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48699-136">Next step</span></span>
[<span data-ttu-id="48699-137">Uppgift 3: Aktivera Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="48699-137">Task 3: enable Azure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-enableaadds.md)
