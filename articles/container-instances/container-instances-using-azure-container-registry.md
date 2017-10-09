---
title: "aaaDeploy tooAzure Behållarinstanser från hello Azure Container registret | Azure-dokument"
description: "Distribuera tooAzure Behållarinstanser från hello Azure Container registret"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2667f91db8ed92a9ccc9ba722a2b1f5c5ea93886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-tooazure-container-instances-from-hello-azure-container-registry"></a><span data-ttu-id="80a97-103">Distribuera tooAzure Behållarinstanser från hello Azure Container registret</span><span class="sxs-lookup"><span data-stu-id="80a97-103">Deploy tooAzure Container Instances from hello Azure Container Registry</span></span>

<span data-ttu-id="80a97-104">hello Azure Container registret är en Azure-baserade, privat registret för Docker behållare bilder.</span><span class="sxs-lookup"><span data-stu-id="80a97-104">hello Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="80a97-105">Den här artikeln beskriver hur toodeploy behållare avbildningar lagras i hello Azure Container registret tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="80a97-105">This article covers how toodeploy container images stored in hello Azure Container Registry tooAzure Container Instances.</span></span>

## <a name="using-hello-azure-cli"></a><span data-ttu-id="80a97-106">Med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80a97-106">Using hello Azure CLI</span></span>

<span data-ttu-id="80a97-107">hello Azure CLI innehåller kommandon för att skapa och hantera behållare i Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="80a97-107">hello Azure CLI includes commands for creating and managing containers in Azure Container Instances.</span></span> <span data-ttu-id="80a97-108">Om du anger en privat avbildning i hello `create` kommandot, du kan också ange hello avbildningen registret lösenord tooauthenticate med hello behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="80a97-108">If you specify a private image in hello `create` command, you can also specify hello image registry password required tooauthenticate with hello container registry.</span></span>

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

<span data-ttu-id="80a97-109">Hej `create` kommandot också har stöd för att ange hello `registry-login-server` och `registry-username`.</span><span class="sxs-lookup"><span data-stu-id="80a97-109">hello `create` command also supports specifying hello `registry-login-server` and `registry-username`.</span></span> <span data-ttu-id="80a97-110">Hello inloggningsserver för hello Azure Container registret är dock alltid *registryname*. azurecr.io och hello Standardanvändarnamnet är *registryname*, så dessa värden är härledas från hello avbildningsnamn om inte uttryckligen anges.</span><span class="sxs-lookup"><span data-stu-id="80a97-110">However, hello login server for hello Azure Container Registry is always *registryname*.azurecr.io and hello default username is *registryname*, so these values are inferred from hello image name if not explicitly provided.</span></span>

## <a name="using-an-azure-resource-manager-template"></a><span data-ttu-id="80a97-111">Med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="80a97-111">Using an Azure Resource Manager template</span></span>

<span data-ttu-id="80a97-112">Du kan ange hello egenskaper för Azure-behållare registret i en Azure Resource Manager-mall genom att inkludera hello `imageRegistryCredentials` egenskap i hello definition av behållare för gruppen:</span><span class="sxs-lookup"><span data-stu-id="80a97-112">You can specify hello properties of your Azure Container Registry in an Azure Resource Manager template by including hello `imageRegistryCredentials` property in hello container group definition:</span></span>

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

<span data-ttu-id="80a97-113">lagra lösenordet behållaren registret direkt i mallen för hello tooavoid rekommenderar vi att du sparar den som en hemlighet i [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) och referens i hello mallen med hjälp av hello [integrering mellan hello Azure Resource Manager och Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="80a97-113">tooavoid storing your container registry password directly in hello template, we recommend that you store it as a secret in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) and reference it in hello template using hello [native integration between hello Azure Resource Manager and Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span>

## <a name="using-hello-azure-portal"></a><span data-ttu-id="80a97-114">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="80a97-114">Using hello Azure portal</span></span>

<span data-ttu-id="80a97-115">Om du underhåller behållaren bilder i hello Azure Container registret kan du enkelt skapa en behållare i Azure Container instanser som använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80a97-115">If you maintain container images in hello Azure Container Registry, you can easily create a container in Azure Container Instances using hello Azure portal.</span></span>

1. <span data-ttu-id="80a97-116">Navigera tooyour behållare registret i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80a97-116">In hello Azure portal, navigate tooyour container registry.</span></span>

2. <span data-ttu-id="80a97-117">Välj databaser.</span><span class="sxs-lookup"><span data-stu-id="80a97-117">Choose Repositories.</span></span>

    ![hello Azure Container register-menyn i hello Azure-portalen][acr-menu]

3. <span data-ttu-id="80a97-119">Välj hello-databasen som du vill toodeploy från.</span><span class="sxs-lookup"><span data-stu-id="80a97-119">Choose hello repository that you want toodeploy from.</span></span>

4. <span data-ttu-id="80a97-120">Högerklicka på hello taggen hello behållaren avbildning du vill ha toodeploy.</span><span class="sxs-lookup"><span data-stu-id="80a97-120">Right-click hello tag for hello container image you want toodeploy.</span></span>

    ![Snabbmenyn för att starta behållare med Azure Container instanser][acr-runinstance-contextmenu]

5. <span data-ttu-id="80a97-122">Ange ett namn för hello behållare och ett namn för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="80a97-122">Enter a name for hello container and a name for hello resource group.</span></span> <span data-ttu-id="80a97-123">Du kan också ändra hello standardvärden om du vill.</span><span class="sxs-lookup"><span data-stu-id="80a97-123">You can also change hello default values if you wish.</span></span>

    ![Skapa meny för Azure-Behållarinstanser][acr-create-deeplink]

6. <span data-ttu-id="80a97-125">När hello distributionen är klar kan du gå toohello behållargruppen från hello meddelanden fönstret toofind sin IP-adress och andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="80a97-125">Once hello deployment completes, you can navigate toohello container group from hello notifications pane toofind its IP address and other properties.</span></span>

    ![Detaljvyn för Azure-behållare instansgrupp behållare][aci-detailsview]

## <a name="next-steps"></a><span data-ttu-id="80a97-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80a97-127">Next steps</span></span>

<span data-ttu-id="80a97-128">Lär dig hur toobuild behållare, push-installera dem tooa privata behållare registret och distribuera dem tooAzure Behållarinstanser av [hello kursen](container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="80a97-128">Learn how toobuild containers, push them tooa private container registry, and deploy them tooAzure Container Instances by [completing hello tutorial](container-instances-tutorial-prepare-app.md).</span></span>

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
