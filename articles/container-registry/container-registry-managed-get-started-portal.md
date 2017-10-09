---
title: aaaCreate privata Docker register - Azure-portalen | Microsoft Docs
description: "Börja skapa och hantera privata Docker behållare register med hello Azure-portalen"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: cf3ce0dcf3036d0e9cd1eaf01721deccb00248d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-portal"></a>Skapa en hanterad behållaren registret med hjälp av hello Azure-portalen

Azure Container Registry är en hanterad Docker-behållarregistertjänst som används för att lagra privata Docker-behållaravbildningar. Den här guiden information hur du skapar en hanterad Azure Container registret-instans med hello Azure-portalen.

Hanterade Azure-behållarregister finns endast som förhandsversion och är inte tillgängliga i alla regioner.

## <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in toohello Azure-portalen på http://portal.azure.com.

## <a name="create-a-container-registry"></a>Skapa ett behållarregister

1. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2. Sök hello marketplace för **Azure-behållaren registret** och markera den.

3. Klicka på **skapa** som öppnas hello ACR skapa blad.

    ![Inställningar för behållarregister](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. I hello **Azure Container registret** bladet ange hello följande information. Klicka på **Skapa** när du är klar.

    a. **Registernamn**: Ett globalt unikt domännamn på den översta nivån för ditt specifika register. I det här exemplet är hello registret namnet *myAzureContainerRegistry1*, men i stället använda ett unikt namn för din egen. hello namn får bara innehålla bokstäver och siffror.

    b. **Resursgruppen**: Välj en befintlig [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny.

    c. **Plats**: Välj en plats för Azure-datacenter där hello-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/), som **södra centrala USA**.

    d. **Administratören**: Om du vill aktivera en admin tooaccess hello registerinställningar. Du kan ändra den här inställningen när du har skapat hello registret.

    e. **Använd hanterade registret**: väljer du Ja toohave ACR automatiskt hantera hello registret lagring, använder webhooks och Använd AAD-autentisering.

    f. **Prisnivå**: Välj en prisnivå. Här visas mer information om ACR-priser.

## <a name="log-in-tooacr-instance"></a>Logga in tooACR instans

Innan push-installation och dra behållaren bilder, måste du logga in toohello ACR-instans. 

toodo så Använd hello Azure CLI 2.0. Först om det behövs, logga in på Azure med hjälp av hello [az inloggningen](/cli/azure/#login) kommando. 

```azurecli
az login
```

Använd sedan hello [az acr inloggning](/cli/azure/acr#login) kommandot toolog i toohello Azure Container registret.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a>Använda Azure Container Registry

### <a name="list-container-images"></a>Visa lista över behållaravbildningar

Använd hello `az acr` CLI-kommandon tooquery hello bilder och taggar i en databas.

> [!NOTE]
> Behållaren registret stöder för närvarande inte hello `docker search` kommandot tooquery för avbildningar och etiketter.

### <a name="list-repositories"></a>Visa en lista över lagringsplatser

hello visar följande exempel hello databaser i ett register i JSON (JavaScript Object Notation)-format:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Visa en lista över taggar

hello följande exempel visar hello taggar på hello **exempel/nginx** databasen i JSON-format:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nästa steg

Du har skapat en hanterad Azure Container registret-instans med hello Azure-portalen i den här snabbstartsguide.

> [!div class="nextstepaction"]
> [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md)
