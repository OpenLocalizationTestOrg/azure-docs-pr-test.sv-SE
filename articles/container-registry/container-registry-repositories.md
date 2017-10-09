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
# <a name="azure-container-registry-repositories"></a>Azure-behållaren registret databaser

Azure-behållaren registret kan toostore behållare bilder i databaser. Du kan ha grupper med bilder (eller version av avbildningar) i isolerade miljöer genom att lagra avbildningar i databaser. Du kan ange dessa databaser när du trycker på bilder tooyour registret.


## <a name="prerequisites"></a>Krav
* **Azure-behållarregister** – Skapa ett behållarregister i din Azure-prenumeration. Till exempel använda hello [Azure-portalen](container-registry-get-started-portal.md) eller hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).
* **Docker CLI** -tooset din lokala dator som en Docker-värden och åtkomst hello Docker CLI-kommandon, installera [Docker-motorn](https://docs.docker.com/engine/installation/).
* **Hämtar en bild** – hämta en bild från hello offentliga Docker hubb registret tagga den och push-installera den tooyour registret. Anvisningar om hur push och pull-avbildningar, se [Push Docker bild tooAzure privata registret](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-hello-portal"></a>Visa databaser i hello Portal

När du har pushas bilder tooyour behållare registret, kan du se en lista över hello databaser som är värd för hello bilder i hello Azure-portalen.

Om du har följt stegen hello i hello [Push Docker bild tooAzure privata registret](container-registry-get-started-docker-cli.md) artikeln du bör nu ha en Nginx-avbildning i behållaren-registret. Som en del av hello instruktioner, bör du har angett ett namnområde för hello avbildningen. Hello kommandot skickar hello NGinx toohello ”exempel”-avbildningslagringsplatsen i hello exemplet nedan:

```
docker push myregistry.azurecr.io/samples/nginx
```
 Azure Container Registry har stöd för namnområden för lagringsplatser på flera nivåer. Den här funktionen kan du toogroup samlingar med bilder relaterade tooa viss app eller en uppsättning appar toospecific utveckling eller operativa team. tooread mer om databaserna i behållaren register finns [privata Docker behållare register i Azure](container-registry-intro.md).

tooview hello behållaren registret databaser:

1. Logga in toohello Azure-portalen
2. På hello **Azure Container registret** bladet, Välj hello register som du vill tooinspect
3. I hello registret bladet, klickar du på **databaser** toosee en lista över alla hello-databaser och deras bilder
4. (Valfritt) Välj en viss bild toosee taggar

![Databaserna i hello-portalen](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Nästa steg
Nu när du vet hello grunderna är klar toostart med hjälp av registret! Till exempel börja distribuera behållaren bilder tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) klustret.
