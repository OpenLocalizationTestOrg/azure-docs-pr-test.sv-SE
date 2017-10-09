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
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 108622c565e41777fbb1fc9da9a01168abc7a7fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="135e6-103">Azure-behållaren registret databaser</span><span class="sxs-lookup"><span data-stu-id="135e6-103">Azure container registry repositories</span></span>

<span data-ttu-id="135e6-104">Azure-behållaren registret kan toostore behållare bilder i databaser.</span><span class="sxs-lookup"><span data-stu-id="135e6-104">Azure container registry allows you toostore container images in repositories.</span></span> <span data-ttu-id="135e6-105">Du kan ha grupper med bilder (eller version av avbildningar) i isolerade miljöer genom att lagra avbildningar i databaser.</span><span class="sxs-lookup"><span data-stu-id="135e6-105">By storing images in repositories, you can have groups of images (or version of images) in isolated environments.</span></span> <span data-ttu-id="135e6-106">Du kan ange dessa databaser när du trycker på bilder tooyour registret.</span><span class="sxs-lookup"><span data-stu-id="135e6-106">You can specify these repositories when you push images tooyour registry.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="135e6-107">Krav</span><span class="sxs-lookup"><span data-stu-id="135e6-107">Prerequisites</span></span>
* <span data-ttu-id="135e6-108">**Azure-behållarregister** – Skapa ett behållarregister i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="135e6-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="135e6-109">Till exempel använda hello [Azure-portalen](container-registry-get-started-portal.md) eller hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="135e6-109">For example, use hello [Azure portal](container-registry-get-started-portal.md) or hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="135e6-110">**Docker CLI** -tooset din lokala dator som en Docker-värden och åtkomst hello Docker CLI-kommandon, installera [Docker-motorn](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="135e6-110">**Docker CLI** - tooset up your local computer as a Docker host and access hello Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>
* <span data-ttu-id="135e6-111">**Hämtar en bild** – hämta en bild från hello offentliga Docker hubb registret tagga den och push-installera den tooyour registret.</span><span class="sxs-lookup"><span data-stu-id="135e6-111">**Pull an image** - Pull an image from hello public Docker Hub registry, tag it, and push it tooyour registry.</span></span> <span data-ttu-id="135e6-112">Anvisningar om hur push och pull-avbildningar, se [Push Docker bild tooAzure privata registret](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="135e6-112">For guidance on how push and pull images, see [Push Docker image tooAzure private registry](container-registry-get-started-docker-cli.md).</span></span>


## <a name="viewing-repositories-in-hello-portal"></a><span data-ttu-id="135e6-113">Visa databaser i hello Portal</span><span class="sxs-lookup"><span data-stu-id="135e6-113">Viewing repositories in hello Portal</span></span>

<span data-ttu-id="135e6-114">När du har pushas bilder tooyour behållare registret, kan du se en lista över hello databaser som är värd för hello bilder i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="135e6-114">Once you have pushed images tooyour container registry, you can see a list of hello repositories hosting hello images in hello Azure portal.</span></span>

<span data-ttu-id="135e6-115">Om du har följt stegen hello i hello [Push Docker bild tooAzure privata registret](container-registry-get-started-docker-cli.md) artikeln du bör nu ha en Nginx-avbildning i behållaren-registret.</span><span class="sxs-lookup"><span data-stu-id="135e6-115">If you followed hello steps in hello [Push Docker image tooAzure private registry](container-registry-get-started-docker-cli.md) article, you should now have a Nginx image in your container registry.</span></span> <span data-ttu-id="135e6-116">Som en del av hello instruktioner, bör du har angett ett namnområde för hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="135e6-116">As part of hello instructions, you should have specified a namespace for hello image.</span></span> <span data-ttu-id="135e6-117">Hello kommandot skickar hello NGinx toohello ”exempel”-avbildningslagringsplatsen i hello exemplet nedan:</span><span class="sxs-lookup"><span data-stu-id="135e6-117">In hello example below, hello command pushes hello NGinx image toohello "samples" repository:</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```
 <span data-ttu-id="135e6-118">Azure Container Registry har stöd för namnområden för lagringsplatser på flera nivåer.</span><span class="sxs-lookup"><span data-stu-id="135e6-118">Azure Container Registry supports multilevel repository namespaces.</span></span> <span data-ttu-id="135e6-119">Den här funktionen kan du toogroup samlingar med bilder relaterade tooa viss app eller en uppsättning appar toospecific utveckling eller operativa team.</span><span class="sxs-lookup"><span data-stu-id="135e6-119">This feature enables you toogroup collections of images related tooa specific app, or a collection of apps toospecific development or operational teams.</span></span> <span data-ttu-id="135e6-120">tooread mer om databaserna i behållaren register finns [privata Docker behållare register i Azure](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="135e6-120">tooread more about repositories in container registries, see [Private Docker container registries in Azure](container-registry-intro.md).</span></span>

<span data-ttu-id="135e6-121">tooview hello behållaren registret databaser:</span><span class="sxs-lookup"><span data-stu-id="135e6-121">tooview hello container registry repositories:</span></span>

1. <span data-ttu-id="135e6-122">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="135e6-122">Log in toohello Azure portal</span></span>
2. <span data-ttu-id="135e6-123">På hello **Azure Container registret** bladet, Välj hello register som du vill tooinspect</span><span class="sxs-lookup"><span data-stu-id="135e6-123">On hello **Azure Container Registry** blade, select hello registry you wish tooinspect</span></span>
3. <span data-ttu-id="135e6-124">I hello registret bladet, klickar du på **databaser** toosee en lista över alla hello-databaser och deras bilder</span><span class="sxs-lookup"><span data-stu-id="135e6-124">In hello registry blade, click **Repositories** toosee a list of all hello repositories and their images</span></span>
4. <span data-ttu-id="135e6-125">(Valfritt) Välj en viss bild toosee taggar</span><span class="sxs-lookup"><span data-stu-id="135e6-125">(Optional) Select a specific image toosee tags</span></span>

![Databaserna i hello-portalen](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a><span data-ttu-id="135e6-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="135e6-127">Next steps</span></span>
<span data-ttu-id="135e6-128">Nu när du vet hello grunderna är klar toostart med hjälp av registret!</span><span class="sxs-lookup"><span data-stu-id="135e6-128">Now that you know hello basics, you are ready toostart using your registry!</span></span> <span data-ttu-id="135e6-129">Till exempel börja distribuera behållaren bilder tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) klustret.</span><span class="sxs-lookup"><span data-stu-id="135e6-129">For example, start deploying container images tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
