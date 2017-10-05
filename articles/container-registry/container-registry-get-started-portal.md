---
title: "Skapa privat Docker-register – Azure Portal | Microsoft-dokument"
description: "Kom igång med att skapa och hantera privata Docker-behållarregister med Azure Portal"
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
ms.openlocfilehash: 7fbbb56d775ee96c9a44363a4e41d4fc3c630582
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-private-docker-container-registry-using-the-azure-portal"></a><span data-ttu-id="554b3-103">Skapa ett privat Docker-behållarregister med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="554b3-103">Create a private Docker container registry using the Azure portal</span></span>
<span data-ttu-id="554b3-104">Använd Azure Portal för att skapa ett behållarregister och hantera dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="554b3-104">Use the Azure portal to create a container registry and manage its settings.</span></span> <span data-ttu-id="554b3-105">Du kan också skapa och hantera behållarregister med hjälp av [Azure CLI 2.0-kommandona](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) eller via programmering med [REST-API:et](https://go.microsoft.com/fwlink/p/?linkid=834376) för Container Registry.</span><span class="sxs-lookup"><span data-stu-id="554b3-105">You can also create and manage container registries using the [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) or programmatically with the Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>

<span data-ttu-id="554b3-106">Bakgrund och koncept beskrivs i [översikten](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="554b3-106">For background and concepts, see [the overview](container-registry-intro.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="554b3-107">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="554b3-107">Create a container registry</span></span>
1. <span data-ttu-id="554b3-108">Klicka på **+ Nytt** på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="554b3-108">In the [Azure portal](https://portal.azure.com), click **+ New**.</span></span>
2. <span data-ttu-id="554b3-109">Sök efter **Azure Container Registry** på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="554b3-109">Search the marketplace for **Azure container registry**.</span></span>
3. <span data-ttu-id="554b3-110">Välj **Azure Container Registry**, med **Microsoft** som utgivare.</span><span class="sxs-lookup"><span data-stu-id="554b3-110">Select **Azure Container Registry**, with publisher **Microsoft**.</span></span>
    <span data-ttu-id="554b3-111">![Container Registry-tjänsten på Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span><span class="sxs-lookup"><span data-stu-id="554b3-111">![Container Registry service in Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span></span>
4. <span data-ttu-id="554b3-112">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="554b3-112">Click **Create**.</span></span> <span data-ttu-id="554b3-113">Bladet **Azure Container Registry** öppnas.</span><span class="sxs-lookup"><span data-stu-id="554b3-113">The **Azure Container Registry** blade appears.</span></span>

    ![Inställningar för behållarregister](./media/container-registry-get-started-portal/container-registry-settings.png)
5. <span data-ttu-id="554b3-115">Ange informationen nedan på bladet **Azure Container Registry**.</span><span class="sxs-lookup"><span data-stu-id="554b3-115">In the **Azure Container Registry** blade, enter the following information.</span></span> <span data-ttu-id="554b3-116">Klicka på **Skapa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="554b3-116">Click **Create** when you are done.</span></span>

    <span data-ttu-id="554b3-117">a.</span><span class="sxs-lookup"><span data-stu-id="554b3-117">a.</span></span> <span data-ttu-id="554b3-118">**Registernamn**: Ett globalt unikt domännamn på den översta nivån för ditt specifika register.</span><span class="sxs-lookup"><span data-stu-id="554b3-118">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="554b3-119">I det här exemplet är registernamnet *myRegistry01*, men du kan ersätta namnet med ett eget unikt namn.</span><span class="sxs-lookup"><span data-stu-id="554b3-119">In this example, the registry name is *myRegistry01*, but substitute a unique name of your own.</span></span> <span data-ttu-id="554b3-120">Namnet får bara innehålla bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="554b3-120">The name can contain only letters and numbers.</span></span>

    <span data-ttu-id="554b3-121">b.</span><span class="sxs-lookup"><span data-stu-id="554b3-121">b.</span></span> <span data-ttu-id="554b3-122">**Resursgrupp**: Välj en befintlig [resursgrupp](../azure-resource-manager/resource-group-overview.md#resource-groups) eller skriv namnet på en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="554b3-122">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span>

    <span data-ttu-id="554b3-123">c.</span><span class="sxs-lookup"><span data-stu-id="554b3-123">c.</span></span> <span data-ttu-id="554b3-124">**Plats**: Välj en plats för ett Azure-datacenter där tjänsten är [tillgänglig](https://azure.microsoft.com/regions/services/), t.ex. **USA, södra centrala**.</span><span class="sxs-lookup"><span data-stu-id="554b3-124">**Location**: Select an Azure datacenter location where the service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="554b3-125">d.</span><span class="sxs-lookup"><span data-stu-id="554b3-125">d.</span></span> <span data-ttu-id="554b3-126">**Administratörsanvändarnamn**: Om du vill ger du en administratörsanvändare åtkomst till registret.</span><span class="sxs-lookup"><span data-stu-id="554b3-126">**Admin user**: If you want, enable an admin user to access the registry.</span></span> <span data-ttu-id="554b3-127">Du kan ändra den här inställningen när du har skapat registret.</span><span class="sxs-lookup"><span data-stu-id="554b3-127">You can change this setting after creating the registry.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="554b3-128">Förutom att ge åtkomst genom ett administratörsanvändarkonto stöder behållarregister autentisering med Azure Active Directory-tjänstobjekt.</span><span class="sxs-lookup"><span data-stu-id="554b3-128">In addition to providing access through an admin user account, container registries support authentication backed by Azure Active Directory service principals.</span></span> <span data-ttu-id="554b3-129">Mer information och saker att tänka på finns i [Authenticate with a container registry](container-registry-authentication.md) (Autentisera med ett behållarregister).</span><span class="sxs-lookup"><span data-stu-id="554b3-129">For more information and considerations, see [Authenticate with a container registry](container-registry-authentication.md).</span></span>
      >

    <span data-ttu-id="554b3-130">e.</span><span class="sxs-lookup"><span data-stu-id="554b3-130">e.</span></span> <span data-ttu-id="554b3-131">**Lagringskonto**: Använd standardinställningen för att skapa ett [lagringskonto](../storage/common/storage-introduction.md), eller välj ett befintligt lagringskonto på samma plats.</span><span class="sxs-lookup"><span data-stu-id="554b3-131">**Storage account**: Use the default setting to create a [storage account](../storage/common/storage-introduction.md), or select an existing storage account in the same location.</span></span> <span data-ttu-id="554b3-132">Premium-lagring stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="554b3-132">Currently Premium Storage is not supported.</span></span>

## <a name="manage-registry-settings"></a><span data-ttu-id="554b3-133">Hantera registerinställningar</span><span class="sxs-lookup"><span data-stu-id="554b3-133">Manage registry settings</span></span>
<span data-ttu-id="554b3-134">När du har skapat registret kommer du åt registerinställningarna genom att först gå till bladet **Container Registries** (Behållarregister) på portalen.</span><span class="sxs-lookup"><span data-stu-id="554b3-134">After creating the registry, find the registry settings by starting at the **Container Registries** blade in the portal.</span></span> <span data-ttu-id="554b3-135">Du kan till exempel behöva inställningarna för att logga in i registret, eller så kanske du vill aktivera eller inaktivera administratörsanvändaren.</span><span class="sxs-lookup"><span data-stu-id="554b3-135">For example, you might need the settings to log in to your registry, or you might want to enable or disable the admin user.</span></span>

1. <span data-ttu-id="554b3-136">På bladet **Container Registries** (Behållarregister) klickar du på namnet på registret.</span><span class="sxs-lookup"><span data-stu-id="554b3-136">On the **Container Registries** blade, click the name of your registry.</span></span>

    ![Bladet för behållarregister](./media/container-registry-get-started-portal/container-registry-blade.png)
2. <span data-ttu-id="554b3-138">Om du vill hantera åtkomstinställningarna klickar du på **Åtkomstnyckel**.</span><span class="sxs-lookup"><span data-stu-id="554b3-138">To manage access settings, click **Access key**.</span></span>

    ![Åtkomst till behållarregister](./media/container-registry-get-started-portal/container-registry-access.png)
3. <span data-ttu-id="554b3-140">Notera följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="554b3-140">Note the following settings:</span></span>

   * <span data-ttu-id="554b3-141">**Inloggningsserver** –Det fullständigt kvalificerade namnet som du använder för att logga in i registret.</span><span class="sxs-lookup"><span data-stu-id="554b3-141">**Login server** - The fully qualified name you use to log in to the registry.</span></span> <span data-ttu-id="554b3-142">I det här exemplet är det `myregistry01.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="554b3-142">In this example, it is `myregistry01.azurecr.io`.</span></span>
   * <span data-ttu-id="554b3-143">**Administratörsanvändare** – Växla om du vill aktivera eller inaktivera administratörsanvändarkontot för registret.</span><span class="sxs-lookup"><span data-stu-id="554b3-143">**Admin user** - Toggle to enable or disable the registry's admin user account.</span></span>
   * <span data-ttu-id="554b3-144">**Användarnamn** och **Lösenord** –Autentiseringsuppgifterna för administratörsanvändarkontot (om det är aktiverat) som du kan använda för att logga in i registret.</span><span class="sxs-lookup"><span data-stu-id="554b3-144">**Username** and **Password** - The credentials of the admin user account (if enabled) you can use to log in to the registry.</span></span> <span data-ttu-id="554b3-145">Om du vill kan du återskapa lösenorden.</span><span class="sxs-lookup"><span data-stu-id="554b3-145">You can optionally regenerate the passwords.</span></span> <span data-ttu-id="554b3-146">Två lösenord skapas så att du kan upprätthålla anslutningar till registret genom att använda ett lösenord medan du återskapar det andra lösenordet.</span><span class="sxs-lookup"><span data-stu-id="554b3-146">Two passwords are created so that you can maintain connections to the registry by using one password while you regenerate the other password.</span></span> <span data-ttu-id="554b3-147">Om du vill autentisera med ett tjänstobjekt i stället läser du [Authenticate with a private Docker container registry](container-registry-authentication.md) (Autentisera med ett privat Docker-behållarregister).</span><span class="sxs-lookup"><span data-stu-id="554b3-147">To authenticate with a service principal instead, see [Authenticate with a private Docker container registry](container-registry-authentication.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="554b3-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="554b3-148">Next steps</span></span>
* [<span data-ttu-id="554b3-149">Skicka din första avbildning med hjälp av Docker CLI</span><span class="sxs-lookup"><span data-stu-id="554b3-149">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
