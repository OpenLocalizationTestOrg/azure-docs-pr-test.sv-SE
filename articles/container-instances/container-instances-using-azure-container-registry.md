---
title: "Distribuera till Azure-Behållarinstanser från Azure-behållaren registret | Azure-dokument"
description: "Distribuera till Azure-Behållarinstanser från registret Azure-behållaren"
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
ms.openlocfilehash: aa1c4ea379c10dff246e2f924a345f9fa444aa64
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-to-azure-container-instances-from-the-azure-container-registry"></a><span data-ttu-id="28bb0-103">Distribuera till Azure-Behållarinstanser från registret Azure-behållaren</span><span class="sxs-lookup"><span data-stu-id="28bb0-103">Deploy to Azure Container Instances from the Azure Container Registry</span></span>

<span data-ttu-id="28bb0-104">Azure Container registret är en Azure-baserade, privat registret för Docker behållare bilder.</span><span class="sxs-lookup"><span data-stu-id="28bb0-104">The Azure Container Registry is an Azure-based, private registry, for Docker container images.</span></span> <span data-ttu-id="28bb0-105">Den här artikeln beskriver hur du distribuerar behållare bilderna som lagras i registret för Azure-behållare till Behållarinstanser som Azure.</span><span class="sxs-lookup"><span data-stu-id="28bb0-105">This article covers how to deploy container images stored in the Azure Container Registry to Azure Container Instances.</span></span>

## <a name="using-the-azure-cli"></a><span data-ttu-id="28bb0-106">Använda Azure CLI</span><span class="sxs-lookup"><span data-stu-id="28bb0-106">Using the Azure CLI</span></span>

<span data-ttu-id="28bb0-107">Azure CLI innehåller kommandon för att skapa och hantera behållare i Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="28bb0-107">The Azure CLI includes commands for creating and managing containers in Azure Container Instances.</span></span> <span data-ttu-id="28bb0-108">Om du anger en privat bilden i den `create` kommando, du kan också ange avbildningen registret lösenord som krävs för att autentisera med behållaren registret.</span><span class="sxs-lookup"><span data-stu-id="28bb0-108">If you specify a private image in the `create` command, you can also specify the image registry password required to authenticate with the container registry.</span></span>

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

<span data-ttu-id="28bb0-109">Den `create` kommandot stöder även att ange den `registry-login-server` och `registry-username`.</span><span class="sxs-lookup"><span data-stu-id="28bb0-109">The `create` command also supports specifying the `registry-login-server` and `registry-username`.</span></span> <span data-ttu-id="28bb0-110">Inloggningsserver för Azure-behållare registret är dock alltid *registryname*. azurecr.io och Standardanvändarnamnet är *registryname*, så dessa värden är om den inte att härleda från avbildningens namn uttryckligen anges.</span><span class="sxs-lookup"><span data-stu-id="28bb0-110">However, the login server for the Azure Container Registry is always *registryname*.azurecr.io and the default username is *registryname*, so these values are inferred from the image name if not explicitly provided.</span></span>

## <a name="using-an-azure-resource-manager-template"></a><span data-ttu-id="28bb0-111">Med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="28bb0-111">Using an Azure Resource Manager template</span></span>

<span data-ttu-id="28bb0-112">Du kan ange egenskaper för Azure-behållare registret i en Azure Resource Manager-mall genom att inkludera den `imageRegistryCredentials` egenskap i Gruppdefinition behållare:</span><span class="sxs-lookup"><span data-stu-id="28bb0-112">You can specify the properties of your Azure Container Registry in an Azure Resource Manager template by including the `imageRegistryCredentials` property in the container group definition:</span></span>

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

<span data-ttu-id="28bb0-113">För att undvika att lagra lösenordet behållaren registret direkt i mallen, rekommenderar vi att du sparar den som en hemlighet i [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) och referera till den i mallen med hjälp av [integrering mellan Azure Resource Manager och Nyckelvalv](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span><span class="sxs-lookup"><span data-stu-id="28bb0-113">To avoid storing your container registry password directly in the template, we recommend that you store it as a secret in [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) and reference it in the template using the [native integration between the Azure Resource Manager and Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).</span></span>

## <a name="using-the-azure-portal"></a><span data-ttu-id="28bb0-114">Använda Azure Portal</span><span class="sxs-lookup"><span data-stu-id="28bb0-114">Using the Azure portal</span></span>

<span data-ttu-id="28bb0-115">Om du underhåller behållaren avbildningar i Azure Container registret kan du enkelt skapa en behållare i Azure Container instanser med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="28bb0-115">If you maintain container images in the Azure Container Registry, you can easily create a container in Azure Container Instances using the Azure portal.</span></span>

1. <span data-ttu-id="28bb0-116">Navigera till behållaren registret i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="28bb0-116">In the Azure portal, navigate to your container registry.</span></span>

2. <span data-ttu-id="28bb0-117">Välj databaser.</span><span class="sxs-lookup"><span data-stu-id="28bb0-117">Choose Repositories.</span></span>

    ![Azure Container register-menyn i Azure-portalen][acr-menu]

3. <span data-ttu-id="28bb0-119">Välj databasen som du vill distribuera från.</span><span class="sxs-lookup"><span data-stu-id="28bb0-119">Choose the repository that you want to deploy from.</span></span>

4. <span data-ttu-id="28bb0-120">Högerklicka på taggen för den behållare bilden som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="28bb0-120">Right-click the tag for the container image you want to deploy.</span></span>

    ![Snabbmenyn för att starta behållare med Azure Container instanser][acr-runinstance-contextmenu]

5. <span data-ttu-id="28bb0-122">Ange ett namn för behållaren och ett namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="28bb0-122">Enter a name for the container and a name for the resource group.</span></span> <span data-ttu-id="28bb0-123">Du kan också ändra standardvärden om du vill.</span><span class="sxs-lookup"><span data-stu-id="28bb0-123">You can also change the default values if you wish.</span></span>

    ![Skapa meny för Azure-Behållarinstanser][acr-create-deeplink]

6. <span data-ttu-id="28bb0-125">När distributionen är klar kan du gå till behållargruppen från fönstret meddelanden för att hitta sin IP-adress och andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="28bb0-125">Once the deployment completes, you can navigate to the container group from the notifications pane to find its IP address and other properties.</span></span>

    ![Detaljvyn för Azure-behållare instansgrupp behållare][aci-detailsview]

## <a name="next-steps"></a><span data-ttu-id="28bb0-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28bb0-127">Next steps</span></span>

<span data-ttu-id="28bb0-128">Lär dig att bygga behållare, push-installera dem till ett privat behållaren register och distribuera dem till Azure-Behållarinstanser av [igenom kursen](container-instances-tutorial-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="28bb0-128">Learn how to build containers, push them to a private container registry, and deploy them to Azure Container Instances by [completing the tutorial](container-instances-tutorial-prepare-app.md).</span></span>

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
