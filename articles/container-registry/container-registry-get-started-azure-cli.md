---
title: "aaaCreate privata Docker behållare registret - Azure CLI | Microsoft Docs"
description: "Börja skapa och hantera privata Docker behållare register med hello Azure CLI 2.0"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0d876a70b71a5e1bd564fbc9198f693dfe8a347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-cli-20"></a><span data-ttu-id="f07f6-103">Skapa en privat Docker behållare registret med hjälp av hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f07f6-103">Create a private Docker container registry using hello Azure CLI 2.0</span></span>
<span data-ttu-id="f07f6-104">Använd kommandon i hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate ett register för behållaren och hantera dess inställningar från din Linux, Mac eller Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="f07f6-104">Use commands in hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate a container registry and manage its settings from your Linux, Mac, or Windows computer.</span></span> <span data-ttu-id="f07f6-105">Du kan också skapa och hantera behållare register med hello [Azure-portalen](container-registry-get-started-portal.md) eller programmässigt med hello behållaren registret [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="f07f6-105">You can also create and manage container registries using hello [Azure portal](container-registry-get-started-portal.md) or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>


* <span data-ttu-id="f07f6-106">Bakgrund och begrepp finns [hello översikt](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="f07f6-106">For background and concepts, see [hello overview](container-registry-intro.md)</span></span>
* <span data-ttu-id="f07f6-107">Mer information om behållaren registret CLI-kommandona (`az acr` kommandon), skicka hello `-h` parametern tooany kommando.</span><span class="sxs-lookup"><span data-stu-id="f07f6-107">For help on Container Registry CLI commands (`az acr` commands), pass hello `-h` parameter tooany command.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f07f6-108">Krav</span><span class="sxs-lookup"><span data-stu-id="f07f6-108">Prerequisites</span></span>
* <span data-ttu-id="f07f6-109">**Azure CLI 2.0**: tooinstall och kom igång med hello CLI 2.0, se hello [Installationsinstruktioner](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f07f6-109">**Azure CLI 2.0**: tooinstall and get started with hello CLI 2.0, see hello [installation instructions](/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="f07f6-110">Logga in tooyour Azure-prenumeration genom att köra `az login`.</span><span class="sxs-lookup"><span data-stu-id="f07f6-110">Log in tooyour Azure subscription by running `az login`.</span></span> <span data-ttu-id="f07f6-111">Mer information finns i [Kom igång med hello CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f07f6-111">For more information, see [Get started with hello CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>
* <span data-ttu-id="f07f6-112">**Resursgrupp**: Skapa en [resursgrupp](../azure-resource-manager/resource-group-overview.md#resource-groups) innan du skapar ett behållarregister eller använd en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f07f6-112">**Resource group**: Create a [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) before creating a container registry, or use an existing resource group.</span></span> <span data-ttu-id="f07f6-113">Kontrollera att resursgruppen hello finns på en plats där hello behållaren Registry-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="f07f6-113">Make sure hello resource group is in a location where hello Container Registry service is [available](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="f07f6-114">en resurs med toocreate hello CLI 2.0, finns [hello referens CLI 2.0](/cli/azure/group).</span><span class="sxs-lookup"><span data-stu-id="f07f6-114">toocreate a resource group using hello CLI 2.0, see [hello CLI 2.0 reference](/cli/azure/group).</span></span>
* <span data-ttu-id="f07f6-115">**Lagringskontot** (valfritt): skapa en standard Azure [lagringskonto](../storage/common/storage-introduction.md) tooback hello behållaren registret i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="f07f6-115">**Storage account** (optional): Create a standard Azure [storage account](../storage/common/storage-introduction.md) tooback hello container registry in hello same location.</span></span> <span data-ttu-id="f07f6-116">Om du inte anger ett storage-konto när du skapar ett register med `az acr create`, hello kommando skapar en åt dig.</span><span class="sxs-lookup"><span data-stu-id="f07f6-116">If you don't specify a storage account when creating a registry with `az acr create`, hello command creates one for you.</span></span> <span data-ttu-id="f07f6-117">en storage-konto med hjälp av toocreate Hej CLI 2.0, finns [hello referens CLI 2.0](/cli/azure/storage/account).</span><span class="sxs-lookup"><span data-stu-id="f07f6-117">toocreate a storage account using hello CLI 2.0, see [hello CLI 2.0 reference](/cli/azure/storage/account).</span></span> <span data-ttu-id="f07f6-118">Premium-lagring stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="f07f6-118">Currently Premium Storage is not supported.</span></span>
* <span data-ttu-id="f07f6-119">**Tjänstens huvudnamn** (valfritt): när du skapar ett register med hello CLI som standard den har inte konfigurerats för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f07f6-119">**Service principal** (optional): When you create a registry with hello CLI, by default it is not set up for access.</span></span> <span data-ttu-id="f07f6-120">Beroende på dina behov kan du tilldela en befintlig Azure Active Directory service principal tooa registret (du kan också skapa och tilldela en ny), eller aktivera hello registret admin-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="f07f6-120">Depending on your needs, you can assign an existing Azure Active Directory service principal tooa registry (or create and assign a new one), or enable hello registry's admin user account.</span></span> <span data-ttu-id="f07f6-121">Avsnitten hello senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f07f6-121">See hello sections later in this article.</span></span> <span data-ttu-id="f07f6-122">Läs mer om registeråtkomst [autentisera med hello behållaren registret](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f07f6-122">For more information about registry access, see [Authenticate with hello container registry](container-registry-authentication.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="f07f6-123">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="f07f6-123">Create a container registry</span></span>
<span data-ttu-id="f07f6-124">Kör hello `az acr create` kommandot toocreate ett register för behållaren.</span><span class="sxs-lookup"><span data-stu-id="f07f6-124">Run hello `az acr create` command toocreate a container registry.</span></span>

> [!TIP]
> <span data-ttu-id="f07f6-125">När du skapar ett register anger du ett globalt unikt domännamn på den översta nivån, som endast innehåller bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="f07f6-125">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span> <span data-ttu-id="f07f6-126">hello registret i hello exempel heter `myRegistry1`, men i stället använda ett unikt namn för din egen.</span><span class="sxs-lookup"><span data-stu-id="f07f6-126">hello registry name in hello examples is `myRegistry1`, but substitute a unique name of your own.</span></span>
>
>

<span data-ttu-id="f07f6-127">Hej följande kommando använder hello minimal parametrar toocreate behållare registret `myRegistry1` i hello resursgruppen `myResourceGroup`, och använder hello *grundläggande* sku:</span><span class="sxs-lookup"><span data-stu-id="f07f6-127">hello following command uses hello minimal parameters toocreate container registry `myRegistry1` in hello resource group `myResourceGroup`, and using hello *Basic* sku:</span></span>

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* <span data-ttu-id="f07f6-128">`--storage-account-name` är valfritt.</span><span class="sxs-lookup"><span data-stu-id="f07f6-128">`--storage-account-name` is optional.</span></span> <span data-ttu-id="f07f6-129">Om inte anges skapas ett lagringskonto med ett namn som består av hello registret namn och en tidsstämpel i hello angivna resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f07f6-129">If not specified, a storage account is created with a name consisting of hello registry name and a timestamp in hello specified resource group.</span></span>

<span data-ttu-id="f07f6-130">När hello register skapas är hello utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="f07f6-130">When hello registry is created, hello output is similar toohello following:</span></span>

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T18:36:29.124842+00:00",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry
/registries/myRegistry1",
  "location": "southcentralus",
  "loginServer": "myregistry1.azurecr.io",
  "name": "myRegistry1",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myregistry123456789"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}

```


<span data-ttu-id="f07f6-131">Notera särskilt:</span><span class="sxs-lookup"><span data-stu-id="f07f6-131">Take special note:</span></span>

* <span data-ttu-id="f07f6-132">`id`-ID för hello registernyckeln i din prenumeration som du behöver om du vill tooassign ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f07f6-132">`id` - Identifier for hello registry in your subscription, which you need if you want tooassign a service principal.</span></span>
* <span data-ttu-id="f07f6-133">`loginServer`-hello fullständigt kvalificerade namn som du anger för[logga in toohello registret](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f07f6-133">`loginServer` - hello fully qualified name you specify too[log in toohello registry](container-registry-authentication.md).</span></span> <span data-ttu-id="f07f6-134">I det här exemplet är hello namnet `myregistry1.exp.azurecr.io` (gemener).</span><span class="sxs-lookup"><span data-stu-id="f07f6-134">In this example, hello name is `myregistry1.exp.azurecr.io` (all lowercase).</span></span>

## <a name="assign-a-service-principal"></a><span data-ttu-id="f07f6-135">Tilldela ett tjänstobjekt</span><span class="sxs-lookup"><span data-stu-id="f07f6-135">Assign a service principal</span></span>
<span data-ttu-id="f07f6-136">Använd CLI 2.0 kommandon tooassign ett Azure Active Directory service principal tooa registret.</span><span class="sxs-lookup"><span data-stu-id="f07f6-136">Use CLI 2.0 commands tooassign an Azure Active Directory service principal tooa registry.</span></span> <span data-ttu-id="f07f6-137">hello tjänstens huvudnamn i dessa exempel tilldelas hello ägarrollen, men du kan tilldela [andra roller](../active-directory/role-based-access-control-configure.md) om du vill.</span><span class="sxs-lookup"><span data-stu-id="f07f6-137">hello service principal in these examples is assigned hello Owner role, but you can assign [other roles](../active-directory/role-based-access-control-configure.md) if you want.</span></span>

### <a name="create-a-service-principal-and-assign-access-toohello-registry"></a><span data-ttu-id="f07f6-138">Skapa ett huvudnamn för tjänsten och tilldela åtkomst toohello registret</span><span class="sxs-lookup"><span data-stu-id="f07f6-138">Create a service principal and assign access toohello registry</span></span>
<span data-ttu-id="f07f6-139">I hello följande kommando, ett nytt huvudnamn för tjänsten tilldelas ägare roll åtkomst toohello registret identifierare godkändes med hello `--scopes` parameter.</span><span class="sxs-lookup"><span data-stu-id="f07f6-139">In hello following command, a new service principal is assigned Owner role access toohello registry identifier passed with hello `--scopes` parameter.</span></span> <span data-ttu-id="f07f6-140">Ange ett starkt lösenord med hello `--password` parameter.</span><span class="sxs-lookup"><span data-stu-id="f07f6-140">Specify a strong password with hello `--password` parameter.</span></span>

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a><span data-ttu-id="f07f6-141">Tilldela ett befintligt tjänstobjekt</span><span class="sxs-lookup"><span data-stu-id="f07f6-141">Assign an existing service principal</span></span>
<span data-ttu-id="f07f6-142">Om du redan har ett huvudnamn för tjänsten och vill tooassign den ägare roll åtkomst toohello registret, kör ett kommando liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="f07f6-142">If you already have a service principal and want tooassign it Owner role access toohello registry, run a command similar toohello following example.</span></span> <span data-ttu-id="f07f6-143">Du skickar hello service principal app-ID med hjälp av hello `--assignee` parameter:</span><span class="sxs-lookup"><span data-stu-id="f07f6-143">You pass hello service principal app ID using hello `--assignee` parameter:</span></span>

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a><span data-ttu-id="f07f6-144">Hantera administratörsautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="f07f6-144">Manage admin credentials</span></span>
<span data-ttu-id="f07f6-145">Ett administratörskonto skapas automatiskt för varje behållarregister och är inaktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="f07f6-145">An admin account is automatically created for each container registry and is disabled by default.</span></span> <span data-ttu-id="f07f6-146">Hej följande exempel visas `az acr` CLI-kommandon toomanage hello administratörsautentiseringsuppgifter för behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="f07f6-146">hello following examples show `az acr` CLI commands toomanage hello admin credentials for your container registry.</span></span>

### <a name="obtain-admin-user-credentials"></a><span data-ttu-id="f07f6-147">Hämta administratörsautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="f07f6-147">Obtain admin user credentials</span></span>
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a><span data-ttu-id="f07f6-148">Aktivera en administratörsanvändare för ett befintligt register</span><span class="sxs-lookup"><span data-stu-id="f07f6-148">Enable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a><span data-ttu-id="f07f6-149">Inaktivera en administratörsanvändare för ett befintligt register</span><span class="sxs-lookup"><span data-stu-id="f07f6-149">Disable admin user for an existing registry</span></span>
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a><span data-ttu-id="f07f6-150">Visa en lista med avbildningar och taggar</span><span class="sxs-lookup"><span data-stu-id="f07f6-150">List images and tags</span></span>
<span data-ttu-id="f07f6-151">Använd hello `az acr` CLI-kommandon tooquery hello bilder och taggar i en databas.</span><span class="sxs-lookup"><span data-stu-id="f07f6-151">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="f07f6-152">Behållaren registret stöder för närvarande inte hello `docker search` kommandot tooquery för avbildningar och etiketter.</span><span class="sxs-lookup"><span data-stu-id="f07f6-152">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>


### <a name="list-repositories"></a><span data-ttu-id="f07f6-153">Visa en lista över lagringsplatser</span><span class="sxs-lookup"><span data-stu-id="f07f6-153">List repositories</span></span>
<span data-ttu-id="f07f6-154">hello visar följande exempel hello databaser i ett register i JSON (JavaScript Object Notation)-format:</span><span class="sxs-lookup"><span data-stu-id="f07f6-154">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="f07f6-155">Visa en lista över taggar</span><span class="sxs-lookup"><span data-stu-id="f07f6-155">List tags</span></span>
<span data-ttu-id="f07f6-156">hello följande exempel visar hello taggar på hello **exempel/nginx** databasen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="f07f6-156">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="f07f6-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f07f6-157">Next steps</span></span>
* [<span data-ttu-id="f07f6-158">Push-en avbildning med hjälp av hello Docker CLI</span><span class="sxs-lookup"><span data-stu-id="f07f6-158">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
