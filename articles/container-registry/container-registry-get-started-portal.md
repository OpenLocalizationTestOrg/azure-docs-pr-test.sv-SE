---
title: aaaCreate privata Docker register - Azure-portalen | Microsoft Docs
description: "Börja skapa och hantera privata Docker behållare register med hello Azure-portalen"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40f3ce44fea26e5fbeca865c9da6df55c2df9511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-portal"></a><span data-ttu-id="b3b69-103">Skapa en privat Docker behållare registret med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b3b69-103">Create a private Docker container registry using hello Azure portal</span></span>
<span data-ttu-id="b3b69-104">Använd hello Azure portal toocreate ett register för behållaren och hantera dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b69-104">Use hello Azure portal toocreate a container registry and manage its settings.</span></span> <span data-ttu-id="b3b69-105">Du kan också skapa och hantera behållare register med hello [Azure CLI 2.0 kommandon](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) eller programmässigt med hello behållaren registret [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="b3b69-105">You can also create and manage container registries using hello [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>

<span data-ttu-id="b3b69-106">Bakgrund och begrepp finns [hello översikt](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="b3b69-106">For background and concepts, see [hello overview](container-registry-intro.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="b3b69-107">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="b3b69-107">Create a container registry</span></span>
1. <span data-ttu-id="b3b69-108">I hello [Azure-portalen](https://portal.azure.com), klickar du på **+ ny**.</span><span class="sxs-lookup"><span data-stu-id="b3b69-108">In hello [Azure portal](https://portal.azure.com), click **+ New**.</span></span>
2. <span data-ttu-id="b3b69-109">Sök hello marketplace för **Azure-behållaren registret**.</span><span class="sxs-lookup"><span data-stu-id="b3b69-109">Search hello marketplace for **Azure container registry**.</span></span>
3. <span data-ttu-id="b3b69-110">Välj **Azure Container Registry**, med **Microsoft** som utgivare.</span><span class="sxs-lookup"><span data-stu-id="b3b69-110">Select **Azure Container Registry**, with publisher **Microsoft**.</span></span>
    <span data-ttu-id="b3b69-111">![Container Registry-tjänsten på Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span><span class="sxs-lookup"><span data-stu-id="b3b69-111">![Container Registry service in Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span></span>
4. <span data-ttu-id="b3b69-112">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b3b69-112">Click **Create**.</span></span> <span data-ttu-id="b3b69-113">Hej **Azure Container registret** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="b3b69-113">hello **Azure Container Registry** blade appears.</span></span>

    ![Inställningar för behållarregister](./media/container-registry-get-started-portal/container-registry-settings.png)
5. <span data-ttu-id="b3b69-115">I hello **Azure Container registret** bladet ange hello följande information.</span><span class="sxs-lookup"><span data-stu-id="b3b69-115">In hello **Azure Container Registry** blade, enter hello following information.</span></span> <span data-ttu-id="b3b69-116">Klicka på **Skapa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="b3b69-116">Click **Create** when you are done.</span></span>

    <span data-ttu-id="b3b69-117">a.</span><span class="sxs-lookup"><span data-stu-id="b3b69-117">a.</span></span> <span data-ttu-id="b3b69-118">**Registernamn**: Ett globalt unikt domännamn på den översta nivån för ditt specifika register.</span><span class="sxs-lookup"><span data-stu-id="b3b69-118">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="b3b69-119">I det här exemplet är hello registret namnet *myRegistry01*, men i stället använda ett unikt namn för din egen.</span><span class="sxs-lookup"><span data-stu-id="b3b69-119">In this example, hello registry name is *myRegistry01*, but substitute a unique name of your own.</span></span> <span data-ttu-id="b3b69-120">hello namn får bara innehålla bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="b3b69-120">hello name can contain only letters and numbers.</span></span>

    <span data-ttu-id="b3b69-121">b.</span><span class="sxs-lookup"><span data-stu-id="b3b69-121">b.</span></span> <span data-ttu-id="b3b69-122">**Resursgruppen**: Välj en befintlig [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny.</span><span class="sxs-lookup"><span data-stu-id="b3b69-122">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span>

    <span data-ttu-id="b3b69-123">c.</span><span class="sxs-lookup"><span data-stu-id="b3b69-123">c.</span></span> <span data-ttu-id="b3b69-124">**Plats**: Välj en plats för Azure-datacenter där hello-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/), som **södra centrala USA**.</span><span class="sxs-lookup"><span data-stu-id="b3b69-124">**Location**: Select an Azure datacenter location where hello service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="b3b69-125">d.</span><span class="sxs-lookup"><span data-stu-id="b3b69-125">d.</span></span> <span data-ttu-id="b3b69-126">**Administratören**: Om du vill aktivera en admin tooaccess hello registerinställningar.</span><span class="sxs-lookup"><span data-stu-id="b3b69-126">**Admin user**: If you want, enable an admin user tooaccess hello registry.</span></span> <span data-ttu-id="b3b69-127">Du kan ändra den här inställningen när du har skapat hello registret.</span><span class="sxs-lookup"><span data-stu-id="b3b69-127">You can change this setting after creating hello registry.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="b3b69-128">Dessutom tooproviding komma åt via ett administratörskonto för användaren, behållare register stöder autentisering backas upp av Azure Active Directory-tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="b3b69-128">In addition tooproviding access through an admin user account, container registries support authentication backed by Azure Active Directory service principals.</span></span> <span data-ttu-id="b3b69-129">Mer information och saker att tänka på finns i [Authenticate with a container registry](container-registry-authentication.md) (Autentisera med ett behållarregister).</span><span class="sxs-lookup"><span data-stu-id="b3b69-129">For more information and considerations, see [Authenticate with a container registry](container-registry-authentication.md).</span></span>
      >

    <span data-ttu-id="b3b69-130">e.</span><span class="sxs-lookup"><span data-stu-id="b3b69-130">e.</span></span> <span data-ttu-id="b3b69-131">**Lagringskontot**: Använd hello standard inställningen toocreate en [lagringskonto](../storage/common/storage-introduction.md), eller välj ett befintligt lagringskonto i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="b3b69-131">**Storage account**: Use hello default setting toocreate a [storage account](../storage/common/storage-introduction.md), or select an existing storage account in hello same location.</span></span> <span data-ttu-id="b3b69-132">Premium-lagring stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="b3b69-132">Currently Premium Storage is not supported.</span></span>

## <a name="manage-registry-settings"></a><span data-ttu-id="b3b69-133">Hantera registerinställningar</span><span class="sxs-lookup"><span data-stu-id="b3b69-133">Manage registry settings</span></span>
<span data-ttu-id="b3b69-134">När du har skapat hello registret, hitta hello registerinställningar genom att starta vid hello **behållare register** bladet i hello portal.</span><span class="sxs-lookup"><span data-stu-id="b3b69-134">After creating hello registry, find hello registry settings by starting at hello **Container Registries** blade in hello portal.</span></span> <span data-ttu-id="b3b69-135">Till exempel kanske du måste hello inställningar toolog i tooyour registret eller du kanske vill tooenable eller inaktivera hello administratörsanvändare.</span><span class="sxs-lookup"><span data-stu-id="b3b69-135">For example, you might need hello settings toolog in tooyour registry, or you might want tooenable or disable hello admin user.</span></span>

1. <span data-ttu-id="b3b69-136">På hello **behållare register** bladet, klickar du på hello namn i registret.</span><span class="sxs-lookup"><span data-stu-id="b3b69-136">On hello **Container Registries** blade, click hello name of your registry.</span></span>

    ![Bladet för behållarregister](./media/container-registry-get-started-portal/container-registry-blade.png)
2. <span data-ttu-id="b3b69-138">inställningar för åtkomst av toomanage, klickar du på **åtkomstnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="b3b69-138">toomanage access settings, click **Access key**.</span></span>

    ![Åtkomst till behållarregister](./media/container-registry-get-started-portal/container-registry-access.png)
3. <span data-ttu-id="b3b69-140">Observera hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="b3b69-140">Note hello following settings:</span></span>

   * <span data-ttu-id="b3b69-141">**Inloggningsserver** -hello fullständigt kvalificerade namn som du använder toolog i toohello registret.</span><span class="sxs-lookup"><span data-stu-id="b3b69-141">**Login server** - hello fully qualified name you use toolog in toohello registry.</span></span> <span data-ttu-id="b3b69-142">I det här exemplet är det `myregistry01.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="b3b69-142">In this example, it is `myregistry01.azurecr.io`.</span></span>
   * <span data-ttu-id="b3b69-143">**Administratörsanvändare** – växla tooenable eller inaktivera hello registret admin-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="b3b69-143">**Admin user** - Toggle tooenable or disable hello registry's admin user account.</span></span>
   * <span data-ttu-id="b3b69-144">**Användarnamnet** och **lösenord** -hello autentiseringsuppgifter för användarkontot för Hej administratör (om aktiverat) du kan använda toolog i toohello registret.</span><span class="sxs-lookup"><span data-stu-id="b3b69-144">**Username** and **Password** - hello credentials of hello admin user account (if enabled) you can use toolog in toohello registry.</span></span> <span data-ttu-id="b3b69-145">Alternativt kan du återskapa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="b3b69-145">You can optionally regenerate hello passwords.</span></span> <span data-ttu-id="b3b69-146">Två lösenord skapas så att du kan upprätthålla anslutningar toohello registret med hjälp av ett lösenord när du återskapar hello andra lösenord.</span><span class="sxs-lookup"><span data-stu-id="b3b69-146">Two passwords are created so that you can maintain connections toohello registry by using one password while you regenerate hello other password.</span></span> <span data-ttu-id="b3b69-147">tooauthenticate med ett huvudnamn för tjänsten finns i stället [autentisera med ett privat Docker behållare register](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="b3b69-147">tooauthenticate with a service principal instead, see [Authenticate with a private Docker container registry](container-registry-authentication.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3b69-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b3b69-148">Next steps</span></span>
* [<span data-ttu-id="b3b69-149">Push-en avbildning med hjälp av hello Docker CLI</span><span class="sxs-lookup"><span data-stu-id="b3b69-149">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
