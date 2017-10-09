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
# <a name="deploy-tooazure-container-instances-from-hello-azure-container-registry"></a>Distribuera tooAzure Behållarinstanser från hello Azure Container registret

hello Azure Container registret är en Azure-baserade, privat registret för Docker behållare bilder. Den här artikeln beskriver hur toodeploy behållare avbildningar lagras i hello Azure Container registret tooAzure Behållarinstanser.

## <a name="using-hello-azure-cli"></a>Med hjälp av hello Azure CLI

hello Azure CLI innehåller kommandon för att skapa och hantera behållare i Azure Container instanser. Om du anger en privat avbildning i hello `create` kommandot, du kan också ange hello avbildningen registret lösenord tooauthenticate med hello behållaren registret.

```azurecli-interactive
az container create --name myprivatecontainer --image mycontainerregistry.azurecr.io/mycontainerimage:v1 --registry-password myRegistryPassword --resource-group myresourcegroup
```

Hej `create` kommandot också har stöd för att ange hello `registry-login-server` och `registry-username`. Hello inloggningsserver för hello Azure Container registret är dock alltid *registryname*. azurecr.io och hello Standardanvändarnamnet är *registryname*, så dessa värden är härledas från hello avbildningsnamn om inte uttryckligen anges.

## <a name="using-an-azure-resource-manager-template"></a>Med en Azure Resource Manager-mall

Du kan ange hello egenskaper för Azure-behållare registret i en Azure Resource Manager-mall genom att inkludera hello `imageRegistryCredentials` egenskap i hello definition av behållare för gruppen:

```json
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

lagra lösenordet behållaren registret direkt i mallen för hello tooavoid rekommenderar vi att du sparar den som en hemlighet i [Azure Key Vault](../key-vault/key-vault-manage-with-cli2.md) och referens i hello mallen med hjälp av hello [integrering mellan hello Azure Resource Manager och Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

## <a name="using-hello-azure-portal"></a>Med hjälp av hello Azure-portalen

Om du underhåller behållaren bilder i hello Azure Container registret kan du enkelt skapa en behållare i Azure Container instanser som använder hello Azure-portalen.

1. Navigera tooyour behållare registret i hello Azure-portalen.

2. Välj databaser.

    ![hello Azure Container register-menyn i hello Azure-portalen][acr-menu]

3. Välj hello-databasen som du vill toodeploy från.

4. Högerklicka på hello taggen hello behållaren avbildning du vill ha toodeploy.

    ![Snabbmenyn för att starta behållare med Azure Container instanser][acr-runinstance-contextmenu]

5. Ange ett namn för hello behållare och ett namn för hello resursgrupp. Du kan också ändra hello standardvärden om du vill.

    ![Skapa meny för Azure-Behållarinstanser][acr-create-deeplink]

6. När hello distributionen är klar kan du gå toohello behållargruppen från hello meddelanden fönstret toofind sin IP-adress och andra egenskaper.

    ![Detaljvyn för Azure-behållare instansgrupp behållare][aci-detailsview]

## <a name="next-steps"></a>Nästa steg

Lär dig hur toobuild behållare, push-installera dem tooa privata behållare registret och distribuera dem tooAzure Behållarinstanser av [hello kursen](container-instances-tutorial-prepare-app.md).

<!-- IMAGES -->
[acr-menu]: ./media/container-instances-using-azure-container-registry/acr-menu.png

[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png

[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
