---
title: "aaaAuthenticate med en Azure-behållaren register | Microsoft Docs"
description: "Hur toolog i tooan Azure-behållaren registret med hjälp av en Azure Active Directory service principal eller ett administratörskonto"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0b0462e8432b2567689debca322e2426baa7fa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a><span data-ttu-id="3c3a7-103">Autentisera med ett privat Docker behållare register</span><span class="sxs-lookup"><span data-stu-id="3c3a7-103">Authenticate with a private Docker container registry</span></span>
<span data-ttu-id="3c3a7-104">toowork med behållaren bilder i en Azure-behållaren registret du logga in med hello `docker login` kommando.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-104">toowork with container images in an Azure container registry, you log in using hello `docker login` command.</span></span> <span data-ttu-id="3c3a7-105">Du kan logga in med antingen en  **[Azure Active Directory-tjänstens huvudnamn](../active-directory/active-directory-application-objects.md)**  eller registret-specifika **administratörskonto**.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-105">You can log in using either an **[Azure Active Directory service principal](../active-directory/active-directory-application-objects.md)** or a registry-specific **admin account**.</span></span> <span data-ttu-id="3c3a7-106">Den här artikeln innehåller mer information om dessa identiteter.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-106">This article provides more detail about these identities.</span></span>



## <a name="service-principal"></a><span data-ttu-id="3c3a7-107">Tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="3c3a7-107">Service principal</span></span>

<span data-ttu-id="3c3a7-108">Du kan [tilldela ett huvudnamn för tjänsten](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registret och använda den för grundläggande Docker-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-108">You can [assign a service principal](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registry and use it for basic Docker authentication.</span></span> <span data-ttu-id="3c3a7-109">Du bör använda ett huvudnamn för tjänsten för de flesta scenarier.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-109">Using a service principal is recommended for most scenarios.</span></span> <span data-ttu-id="3c3a7-110">Ange hello app-ID och lösenord för hello service principal toohello `docker login` kommandot som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="3c3a7-110">Provide hello app ID and password of hello service principal toohello `docker login` command, as shown in hello following example:</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="3c3a7-111">När du loggade in cachelagrar Docker hello autentiseringsuppgifter, så du inte behöver tooremember hello app-ID.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-111">Once logged in, Docker caches hello credentials, so you don't need tooremember hello app ID.</span></span>

> [!TIP]
> <span data-ttu-id="3c3a7-112">Om du vill kan du återskapa hello lösenordet för ett huvudnamn för tjänsten genom att köra hello `az ad sp reset-credentials` kommando.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-112">If you want, you can regenerate hello password of a service principal by running hello `az ad sp reset-credentials` command.</span></span>
>


<span data-ttu-id="3c3a7-113">Tjänstens huvudnamn Tillåt [rollbaserad åtkomst](../active-directory/role-based-access-control-configure.md) tooa registret.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-113">Service principals allow [role-based access](../active-directory/role-based-access-control-configure.md) tooa registry.</span></span> <span data-ttu-id="3c3a7-114">Tillgängliga roller är:</span><span class="sxs-lookup"><span data-stu-id="3c3a7-114">Available roles are:</span></span>
  * <span data-ttu-id="3c3a7-115">Läsare (pull endast åtkomst).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-115">Reader (pull only access).</span></span>
  * <span data-ttu-id="3c3a7-116">Deltagare (pull och push).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-116">Contributor (pull and push).</span></span>
  * <span data-ttu-id="3c3a7-117">Ägaren (pull-, push- och tilldela roller tooother användare).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-117">Owner (pull, push, and assign roles tooother users).</span></span>

<span data-ttu-id="3c3a7-118">Anonym åtkomst är inte tillgänglig på Azure-behållare register.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-118">Anonymous access is not available on Azure Container Registries.</span></span> <span data-ttu-id="3c3a7-119">Du kan använda för offentliga bilder [Docker-hubb](https://docs.docker.com/docker-hub/).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-119">For public images you can use [Docker Hub](https://docs.docker.com/docker-hub/).</span></span>

<span data-ttu-id="3c3a7-120">Du kan tilldela flera tjänstens huvudnamn tooa registret, vilket innebär att du toodefine åtkomst för olika användare eller program.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-120">You can assign multiple service principals tooa registry, which allows you toodefine access for different users or applications.</span></span> <span data-ttu-id="3c3a7-121">Tjänstens huvudnamn också aktivera ”fjärradministrerade” anslutning tooa registret i utvecklare eller DevOps scenarier, till exempel hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="3c3a7-121">Service principals also enable "headless" connectivity tooa registry in developer or DevOps scenarios such as hello following examples:</span></span>

  * <span data-ttu-id="3c3a7-122">Distribution i behållare från en DC/OS, Docker Swarm och Kubernetes i registret tooorchestration system.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-122">Container deployments from a registry tooorchestration systems including DC/OS, Docker Swarm and Kubernetes.</span></span> <span data-ttu-id="3c3a7-123">Du kan också dra behållaren register toorelated Azure-tjänster som [Behållartjänsten](../container-service/index.yml), [Apptjänst](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), med mera.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-123">You can also pull container registries toorelated Azure services such as [Container Service](../container-service/index.yml), [App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), and others.</span></span>

  * <span data-ttu-id="3c3a7-124">Kontinuerlig integrering och distribution lösningar (till exempel Visual Studio Team Services eller Jenkins) som bygger avbildningar för behållaren och push-installera dem tooa registret.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-124">Continuous integration and deployment solutions (such as Visual Studio Team Services or Jenkins) that build container images and push them tooa registry.</span></span>





## <a name="admin-account"></a><span data-ttu-id="3c3a7-125">Administratörskonto</span><span class="sxs-lookup"><span data-stu-id="3c3a7-125">Admin account</span></span>
<span data-ttu-id="3c3a7-126">Med varje register som du skapar skapas ett administratörskonto automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-126">With each registry you create, an admin account gets created automatically.</span></span> <span data-ttu-id="3c3a7-127">Hello-kontot är inaktiverat som standard, men du kan aktivera och hantera hello autentiseringsuppgifter, till exempel via hello [portal](container-registry-get-started-portal.md#manage-registry-settings) eller med hjälp av hello [Azure CLI 2.0 kommandon](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-127">By default hello account is disabled, but you can enable it and manage hello credentials, for example through hello [portal](container-registry-get-started-portal.md#manage-registry-settings) or using hello [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span></span> <span data-ttu-id="3c3a7-128">Varje administratörskontot tillhandahålls med två lösenord som genereras.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-128">Each admin account is provided with two passwords, both of which can be regenerated.</span></span> <span data-ttu-id="3c3a7-129">hello två lösenord kan du toomaintain anslutningar toohello registret med hjälp av ett lösenord när du återskapar hello andra lösenord.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-129">hello two passwords allow you toomaintain connections toohello registry by using one password while you regenerate hello other password.</span></span> <span data-ttu-id="3c3a7-130">Om hello-kontot är aktiverat du överföra hello användarnamn och antingen lösenord toohello `docker login` kommandot för grundläggande autentisering toohello registret.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-130">If hello account is enabled, you can pass hello user name and either password toohello `docker login` command for basic authentication toohello registry.</span></span> <span data-ttu-id="3c3a7-131">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3c3a7-131">For example:</span></span>

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> <span data-ttu-id="3c3a7-132">hello administratörskonto är utformad för en enskild användare tooaccess hello registret, främst för testning.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-132">hello admin account is designed for a single user tooaccess hello registry, mainly for test purposes.</span></span> <span data-ttu-id="3c3a7-133">Det rekommenderas inte tooshare hello administratörsautentiseringsuppgifter konto bland andra användare.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-133">It is not recommended tooshare hello admin account credentials among other users.</span></span> <span data-ttu-id="3c3a7-134">Alla användare som visas som en enskild användare toohello registret.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-134">All users appear as a single user toohello registry.</span></span> <span data-ttu-id="3c3a7-135">Ändra eller inaktivera det här kontot inaktiverar du registret åtkomst för alla användare som använder hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3c3a7-135">Changing or disabling this account disables registry access for all users who use hello credentials.</span></span>
>


### <a name="next-steps"></a><span data-ttu-id="3c3a7-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c3a7-136">Next steps</span></span>
* <span data-ttu-id="3c3a7-137">[Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-137">[Push your first image using hello Docker CLI](container-registry-get-started-docker-cli.md).</span></span>
* <span data-ttu-id="3c3a7-138">Mer information om autentisering i hello behållaren registret preview finns hello [blogginlägget](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span><span class="sxs-lookup"><span data-stu-id="3c3a7-138">For more information about authentication in hello Container Registry preview, see hello [blog post](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span></span>
