---
title: "Azure-behållaren registret databaser | Microsoft Docs"
description: "Hur du använder Azure-behållare registret databaser för Docker bilder"
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
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 06b809c31cecef1714f60d04657eb74c611be8cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="8de23-103">Azure-behållaren registret databaser</span><span class="sxs-lookup"><span data-stu-id="8de23-103">Azure container registry repositories</span></span>

<span data-ttu-id="8de23-104">Azure-behållaren registret kan du lagra avbildningar för behållare i databaser.</span><span class="sxs-lookup"><span data-stu-id="8de23-104">Azure container registry allows you to store container images in repositories.</span></span> <span data-ttu-id="8de23-105">Du kan ha grupper med bilder (eller version av avbildningar) i isolerade miljöer genom att lagra avbildningar i databaser.</span><span class="sxs-lookup"><span data-stu-id="8de23-105">By storing images in repositories, you can have groups of images (or version of images) in isolated environments.</span></span> <span data-ttu-id="8de23-106">Du kan ange dessa databaser när du trycker på bilder i registret.</span><span class="sxs-lookup"><span data-stu-id="8de23-106">You can specify these repositories when you push images to your registry.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="8de23-107">Krav</span><span class="sxs-lookup"><span data-stu-id="8de23-107">Prerequisites</span></span>
* <span data-ttu-id="8de23-108">**Azure-behållarregister** – Skapa ett behållarregister i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8de23-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="8de23-109">Använd till exempel [Azure-portalen](container-registry-get-started-portal.md) eller [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-109">For example, use the [Azure portal](container-registry-get-started-portal.md) or the [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="8de23-110">**Docker CLI** – Om du vill konfigurera den lokala datorn som en Docker-värd och komma åt Docker CLI-kommandona installerar du [Docker-motorn](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="8de23-110">**Docker CLI** - To set up your local computer as a Docker host and access the Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>
* <span data-ttu-id="8de23-111">**Hämtar en bild** - dra en bild från offentliga Docker hubb registret tagga den och push i registret.</span><span class="sxs-lookup"><span data-stu-id="8de23-111">**Pull an image** - Pull an image from the public Docker Hub registry, tag it, and push it to your registry.</span></span> <span data-ttu-id="8de23-112">Anvisningar om hur push och pull-avbildningar, se [Push Docker avbildningen till Azure privata registret](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-112">For guidance on how push and pull images, see [Push Docker image to Azure private registry](container-registry-get-started-docker-cli.md).</span></span>


## <a name="viewing-repositories-in-the-portal"></a><span data-ttu-id="8de23-113">Visa databaser i portalen</span><span class="sxs-lookup"><span data-stu-id="8de23-113">Viewing repositories in the Portal</span></span>

<span data-ttu-id="8de23-114">När du har pushas avbildningar till behållaren registret, kan du se en lista över databaser som är värd för avbildningar i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8de23-114">Once you have pushed images to your container registry, you can see a list of the repositories hosting the images in the Azure portal.</span></span>

<span data-ttu-id="8de23-115">Om du har följt stegen i den [Push Docker avbildningen till Azure privata registret](container-registry-get-started-docker-cli.md) artikeln du bör nu ha en Nginx-avbildning i behållaren-registret.</span><span class="sxs-lookup"><span data-stu-id="8de23-115">If you followed the steps in the [Push Docker image to Azure private registry](container-registry-get-started-docker-cli.md) article, you should now have a Nginx image in your container registry.</span></span> <span data-ttu-id="8de23-116">Som en del av instruktionerna, bör du har angett ett namnområde för avbildningen.</span><span class="sxs-lookup"><span data-stu-id="8de23-116">As part of the instructions, you should have specified a namespace for the image.</span></span> <span data-ttu-id="8de23-117">I exemplet nedan skickar kommandot NGinx-avbildningen till databasen ”exempel”:</span><span class="sxs-lookup"><span data-stu-id="8de23-117">In the example below, the command pushes the NGinx image to the "samples" repository:</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```
 <span data-ttu-id="8de23-118">Azure Container Registry har stöd för namnområden för lagringsplatser på flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="8de23-118">Azure Container Registry supports multilevel repository namespaces.</span></span> <span data-ttu-id="8de23-119">Den här funktionen gör att du kan gruppera samlingar med avbildningar relaterade till en viss app, eller en samling appar för specifika utvecklingsgrupper eller operativa team.</span><span class="sxs-lookup"><span data-stu-id="8de23-119">This feature enables you to group collections of images related to a specific app, or a collection of apps to specific development or operational teams.</span></span> <span data-ttu-id="8de23-120">Du kan läsa mer om databaserna i behållaren register finns [privata Docker behållare register i Azure](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="8de23-120">To read more about repositories in container registries, see [Private Docker container registries in Azure](container-registry-intro.md).</span></span>

<span data-ttu-id="8de23-121">Visa behållaren registret databaser:</span><span class="sxs-lookup"><span data-stu-id="8de23-121">To view the container registry repositories:</span></span>

1. <span data-ttu-id="8de23-122">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8de23-122">Log in to the Azure portal</span></span>
2. <span data-ttu-id="8de23-123">På den **Azure Container registret** bladet Välj registret som du vill granska</span><span class="sxs-lookup"><span data-stu-id="8de23-123">On the **Azure Container Registry** blade, select the registry you wish to inspect</span></span>
3. <span data-ttu-id="8de23-124">I registret-bladet klickar du på **databaser** att se en lista över alla databaser och deras bilder</span><span class="sxs-lookup"><span data-stu-id="8de23-124">In the registry blade, click **Repositories** to see a list of all the repositories and their images</span></span>
4. <span data-ttu-id="8de23-125">(Valfritt) Välj en viss bild att se taggar</span><span class="sxs-lookup"><span data-stu-id="8de23-125">(Optional) Select a specific image to see tags</span></span>

![Databaserna i portalen](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a><span data-ttu-id="8de23-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8de23-127">Next steps</span></span>
<span data-ttu-id="8de23-128">Nu när du känner till grunderna är du redo att börja använda registret!</span><span class="sxs-lookup"><span data-stu-id="8de23-128">Now that you know the basics, you are ready to start using your registry!</span></span> <span data-ttu-id="8de23-129">Du kan till exempel börja distribuera behållaravbildningar till ett [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/)-kluster.</span><span class="sxs-lookup"><span data-stu-id="8de23-129">For example, start deploying container images to an [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
