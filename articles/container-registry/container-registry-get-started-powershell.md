---
title: "aaaAzure behållaren registret databaser | Microsoft Docs"
description: "Hur toouse Azure Container registret databaser för Docker bilder"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: cristyg
ms.openlocfilehash: 448fb812f537c9502041ce5fb372b0681a9dac4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-powershell"></a><span data-ttu-id="988ec-103">Skapa en privat Docker behållare registret med hjälp av hello Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="988ec-103">Create a private Docker container registry using hello Azure PowerShell</span></span>
<span data-ttu-id="988ec-104">Använda kommandon i [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate ett register för behållaren och hantera dess inställningar från Windows-datorn.</span><span class="sxs-lookup"><span data-stu-id="988ec-104">Use commands in [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview) toocreate a container registry and manage its settings from your Windows computer.</span></span> <span data-ttu-id="988ec-105">Du kan också skapa och hantera behållare register med hello [Azure-portalen](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), eller programmässigt med hello behållaren registret [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="988ec-105">You can also create and manage container registries using hello [Azure portal](container-registry-get-started-portal.md), hello [Azure CLI](container-registry-get-started-azure-cli.md), or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="988ec-106">Bakgrund och begrepp finns [hello översikt](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="988ec-106">For background and concepts, see [hello overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="988ec-107">En fullständig lista över cmdlets som stöds finns i [Azure Container registret Management-Cmdlets](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span><span class="sxs-lookup"><span data-stu-id="988ec-107">For a full list of supported cmdlets, see [Azure Container Registry Management Cmdlets](https://docs.microsoft.com/en-us/powershell/module/azurerm.containerregistry/).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="988ec-108">Krav</span><span class="sxs-lookup"><span data-stu-id="988ec-108">Prerequisites</span></span>
* <span data-ttu-id="988ec-109">**Azure PowerShell**: tooinstall och kom igång med Azure PowerShell, se hello [Installationsinstruktioner](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="988ec-109">**Azure PowerShell**: tooinstall and get started with Azure PowerShell, see hello [installation instructions](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span> <span data-ttu-id="988ec-110">Logga in tooyour Azure-prenumeration genom att köra `Login-AzureRMAccount`.</span><span class="sxs-lookup"><span data-stu-id="988ec-110">Log in tooyour Azure subscription by running `Login-AzureRMAccount`.</span></span> <span data-ttu-id="988ec-111">Mer information finns i [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span><span class="sxs-lookup"><span data-stu-id="988ec-111">For more information, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/get-started-azurep).</span></span>
* <span data-ttu-id="988ec-112">**Resursgrupp**: Skapa en [resursgrupp](../azure-resource-manager/resource-group-overview.md#resource-groups) innan du skapar ett behållarregister eller använd en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="988ec-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="988ec-113">Kontrollera att resursgruppen hello finns på en plats där hello behållaren Registry-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="988ec-113">Make sure hello resource group is in a location where hello Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="988ec-114">toocreate en resursgrupp med hjälp av Azure PowerShell finns [hello PowerShell-referens](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span><span class="sxs-lookup"><span data-stu-id="988ec-114">toocreate a resource group using Azure PowerShell, see [hello PowerShell reference](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps#create-a-resource-group).</span></span>
* <span data-ttu-id="988ec-115">**Lagringskontot** (valfritt): skapa en standard Azure [lagringskonto](../storage/common/storage-introduction.md) tooback hello behållaren registret i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="988ec-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) tooback hello container registry in hello same location.</span></span> <span data-ttu-id="988ec-116">Om du inte anger ett storage-konto när du skapar ett register med `New-AzureRMContainerRegistry`, hello kommando skapar en åt dig.</span><span class="sxs-lookup"><span data-stu-id="988ec-116">If you don't specify a storage account when creating a registry with `New-AzureRMContainerRegistry`, hello command creates one for you.</span></span> <span data-ttu-id="988ec-117">toocreate en storage-konto med PowerShell, se [hello PowerShell-referens](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span><span class="sxs-lookup"><span data-stu-id="988ec-117">toocreate a storage account using PowerShell, see [hello PowerShell reference](https://docs.microsoft.com/en-us/powershell/module/azure/new-azurestorageaccount).</span></span> <span data-ttu-id="988ec-118">Premium-lagring stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="988ec-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="988ec-119">**Tjänstens huvudnamn** (valfritt): när du skapar ett register med PowerShell som standard den har inte konfigurerats för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="988ec-119">**Service principal** (optional): When you create a registry with PowerShell, by default it is not set up for access.</span></span> <span data-ttu-id="988ec-120">Beroende på dina behov kan du tilldela en befintlig Azure Active Directory service principal tooa registret eller skapa och tilldela en ny.</span><span class="sxs-lookup"><span data-stu-id="988ec-120">Depending on your needs, you can assign an existing Azure Active Directory service principal tooa registry or create and assign a new one.</span></span> <span data-ttu-id="988ec-121">Alternativt kan du aktivera hello registret admin-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="988ec-121">Alternatively, you can enable hello registry's admin user account.</span></span> <span data-ttu-id="988ec-122">Avsnitten hello senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="988ec-122">See hello sections later in this article.</span></span> <span data-ttu-id="988ec-123">Läs mer om registeråtkomst [autentisera med hello behållaren registret](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="988ec-123">For more information about registry access, see [Authenticate with hello container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="988ec-124">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="988ec-124">Create a container registry</span></span>
<span data-ttu-id="988ec-125">Kör hello `New-AzureRMContainerRegistry` kommandot toocreate ett register för behållaren.</span><span class="sxs-lookup"><span data-stu-id="988ec-125">Run hello `New-AzureRMContainerRegistry` command toocreate a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="988ec-126">När du skapar ett register anger du ett globalt unikt domännamn på den översta nivån, som endast innehåller bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="988ec-126">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="988ec-127">hello registret i hello exempel heter `MyRegistry`, men i stället använda ett unikt namn för din egen.</span><span class="sxs-lookup"><span data-stu-id="988ec-127">hello registry name in hello examples is `MyRegistry`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="988ec-128">Hej följande kommando använder hello minimal parametrar toocreate behållare registret `MyRegistry` i hello resursgruppen `MyResourceGroup` i hello södra centrala USA plats:</span><span class="sxs-lookup"><span data-stu-id="988ec-128">hello following command uses hello minimal parameters toocreate container registry `MyRegistry` in hello resource group `MyResourceGroup` in hello South Central US location:</span></span>

```PowerShell
$Registry = New-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

* <span data-ttu-id="988ec-129">`-StorageAccountName` är valfritt.</span><span class="sxs-lookup"><span data-stu-id="988ec-129">`-StorageAccountName` is optional.</span></span> <span data-ttu-id="988ec-130">Om inte anges skapas ett lagringskonto med ett namn som består av hello registret namn och en tidsstämpel i hello angivna resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="988ec-130">If not specified, a storage account is created with a name consisting of hello registry name and a timestamp in hello specified resource group.</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="988ec-131">Tilldela ett tjänstobjekt</span><span class="sxs-lookup"><span data-stu-id="988ec-131">Assign a service principal</span></span>
<span data-ttu-id="988ec-132">Använd PowerShell-kommandon tooassign ett Azure Active Directory [tjänstens huvudnamn](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registret.</span><span class="sxs-lookup"><span data-stu-id="988ec-132">Use PowerShell commands tooassign an Azure Active Directory [service principal](../azure-resource-manager/resource-group-authenticate-service-principal.md) tooa registry.</span></span> <span data-ttu-id="988ec-133">hello tjänstens huvudnamn i dessa exempel tilldelas hello ägarrollen, men du kan tilldela [andra roller](../active-directory/role-based-access-control-configure.md) om du vill.</span><span class="sxs-lookup"><span data-stu-id="988ec-133">hello service principal in these examples is assigned hello Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal"></a><span data-ttu-id="988ec-134">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="988ec-134">Create a service principal</span></span>
<span data-ttu-id="988ec-135">I följande kommando hello, skapas ett nytt huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="988ec-135">In hello following command, a new service principal is created.</span></span> <span data-ttu-id="988ec-136">Ange ett starkt lösenord med hello `-Password` parameter.</span><span class="sxs-lookup"><span data-stu-id="988ec-136">Specify a strong password with hello `-Password` parameter.</span></span>

```PowerShell
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName ApplicationDisplayName -Password "MyPassword"
```

### <a name="assign-a-new-or-existing-service-principal"></a><span data-ttu-id="988ec-137">Tilldela en ny eller befintlig tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="988ec-137">Assign a new or existing service principal</span></span>
<span data-ttu-id="988ec-138">Du kan tilldela en ny eller en befintlig service principal tooa registret.</span><span class="sxs-lookup"><span data-stu-id="988ec-138">You can assign a new or an existing service principal tooa registry.</span></span> <span data-ttu-id="988ec-139">tooassign den ägare roll åtkomst toohello registret, kör ett kommando liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="988ec-139">tooassign it Owner role access toohello registry, run a command similar toohello following example:</span></span>

```PowerShell
New-AzureRMRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Registry.Id
```

##<a name="sign-in-toohello-registry-with-hello-service-principal"></a><span data-ttu-id="988ec-140">Logga in toohello registret med hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="988ec-140">Sign in toohello registry with hello service principal</span></span>
<span data-ttu-id="988ec-141">När du tilldelar hello service principal toohello registret, kan du logga in med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="988ec-141">After assigning hello service principal toohello registry, you can sign in using hello following command:</span></span>

```PowerShell
docker login -u $ServicePrincipal.ApplicationId -p myPassword
```

## <a name="manage-admin-credentials"></a><span data-ttu-id="988ec-142">Hantera administratörsautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="988ec-142">Manage admin credentials</span></span>
<span data-ttu-id="988ec-143">Ett administratörskonto skapas automatiskt för varje behållarregister och är inaktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="988ec-143">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="988ec-144">hello följande exempel visar PowerShell-kommandon toomanage hello administratörsautentiseringsuppgifter för behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="988ec-144">hello following examples show PowerShell commands toomanage hello admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="988ec-145">Hämta administratörsautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="988ec-145">Obtain admin user credentials</span></span>
```PowerShell
Get-AzureRMContainerRegistryCredential -ResourceGroupName "MyResourceGroup" -Name "MyRegistry"
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="988ec-146">Aktivera en administratörsanvändare för ett befintligt register</span><span class="sxs-lookup"><span data-stu-id="988ec-146">Enable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -EnableAdminUser
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="988ec-147">Inaktivera en administratörsanvändare för ett befintligt register</span><span class="sxs-lookup"><span data-stu-id="988ec-147">Disable admin user for an existing registry</span></span>
```PowerShell
Update-AzureRMContainerRegistry -ResourceGroupName "MyResourceGroup" -Name "MyRegistry" -DisableAdminUser
```

## <a name="next-steps"></a><span data-ttu-id="988ec-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="988ec-148">Next steps</span></span>
* [<span data-ttu-id="988ec-149">Push-en avbildning med hjälp av hello Docker CLI</span><span class="sxs-lookup"><span data-stu-id="988ec-149">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
