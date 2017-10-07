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
# <a name="create-a-private-docker-container-registry-using-hello-azure-cli-20"></a>Skapa en privat Docker behållare registret med hjälp av hello Azure CLI 2.0
Använd kommandon i hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) toocreate ett register för behållaren och hantera dess inställningar från din Linux, Mac eller Windows-dator. Du kan också skapa och hantera behållare register med hello [Azure-portalen](container-registry-get-started-portal.md) eller programmässigt med hello behållaren registret [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).


* Bakgrund och begrepp finns [hello översikt](container-registry-intro.md)
* Mer information om behållaren registret CLI-kommandona (`az acr` kommandon), skicka hello `-h` parametern tooany kommando.


## <a name="prerequisites"></a>Krav
* **Azure CLI 2.0**: tooinstall och kom igång med hello CLI 2.0, se hello [Installationsinstruktioner](/cli/azure/install-azure-cli). Logga in tooyour Azure-prenumeration genom att köra `az login`. Mer information finns i [Kom igång med hello CLI 2.0](/cli/azure/get-started-with-azure-cli).
* **Resursgrupp**: Skapa en [resursgrupp](../azure-resource-manager/resource-group-overview.md#resource-groups) innan du skapar ett behållarregister eller använd en befintlig resursgrupp. Kontrollera att resursgruppen hello finns på en plats där hello behållaren Registry-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/). en resurs med toocreate hello CLI 2.0, finns [hello referens CLI 2.0](/cli/azure/group).
* **Lagringskontot** (valfritt): skapa en standard Azure [lagringskonto](../storage/common/storage-introduction.md) tooback hello behållaren registret i hello samma plats. Om du inte anger ett storage-konto när du skapar ett register med `az acr create`, hello kommando skapar en åt dig. en storage-konto med hjälp av toocreate Hej CLI 2.0, finns [hello referens CLI 2.0](/cli/azure/storage/account). Premium-lagring stöds inte för närvarande.
* **Tjänstens huvudnamn** (valfritt): när du skapar ett register med hello CLI som standard den har inte konfigurerats för åtkomst. Beroende på dina behov kan du tilldela en befintlig Azure Active Directory service principal tooa registret (du kan också skapa och tilldela en ny), eller aktivera hello registret admin-användarkonto. Avsnitten hello senare i den här artikeln. Läs mer om registeråtkomst [autentisera med hello behållaren registret](container-registry-authentication.md).

## <a name="create-a-container-registry"></a>Skapa ett behållarregister
Kör hello `az acr create` kommandot toocreate ett register för behållaren.

> [!TIP]
> När du skapar ett register anger du ett globalt unikt domännamn på den översta nivån, som endast innehåller bokstäver och siffror. hello registret i hello exempel heter `myRegistry1`, men i stället använda ett unikt namn för din egen.
>
>

Hej följande kommando använder hello minimal parametrar toocreate behållare registret `myRegistry1` i hello resursgruppen `myResourceGroup`, och använder hello *grundläggande* sku:

```azurecli
az acr create --name myRegistry1 --resource-group myResourceGroup --sku Basic
```

* `--storage-account-name` är valfritt. Om inte anges skapas ett lagringskonto med ett namn som består av hello registret namn och en tidsstämpel i hello angivna resursgruppen.

När hello register skapas är hello utdata liknande toohello följande:

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


Notera särskilt:

* `id`-ID för hello registernyckeln i din prenumeration som du behöver om du vill tooassign ett huvudnamn för tjänsten.
* `loginServer`-hello fullständigt kvalificerade namn som du anger för[logga in toohello registret](container-registry-authentication.md). I det här exemplet är hello namnet `myregistry1.exp.azurecr.io` (gemener).

## <a name="assign-a-service-principal"></a>Tilldela ett tjänstobjekt
Använd CLI 2.0 kommandon tooassign ett Azure Active Directory service principal tooa registret. hello tjänstens huvudnamn i dessa exempel tilldelas hello ägarrollen, men du kan tilldela [andra roller](../active-directory/role-based-access-control-configure.md) om du vill.

### <a name="create-a-service-principal-and-assign-access-toohello-registry"></a>Skapa ett huvudnamn för tjänsten och tilldela åtkomst toohello registret
I hello följande kommando, ett nytt huvudnamn för tjänsten tilldelas ägare roll åtkomst toohello registret identifierare godkändes med hello `--scopes` parameter. Ange ett starkt lösenord med hello `--password` parameter.

```azurecli
az ad sp create-for-rbac --scopes /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --password myPassword
```



### <a name="assign-an-existing-service-principal"></a>Tilldela ett befintligt tjänstobjekt
Om du redan har ett huvudnamn för tjänsten och vill tooassign den ägare roll åtkomst toohello registret, kör ett kommando liknande toohello följande exempel. Du skickar hello service principal app-ID med hjälp av hello `--assignee` parameter:

```azurecli
az role assignment create --scope /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry1 --role Owner --assignee myAppId
```



## <a name="manage-admin-credentials"></a>Hantera administratörsautentiseringsuppgifter
Ett administratörskonto skapas automatiskt för varje behållarregister och är inaktiverat som standard. Hej följande exempel visas `az acr` CLI-kommandon toomanage hello administratörsautentiseringsuppgifter för behållaren registret.

### <a name="obtain-admin-user-credentials"></a>Hämta administratörsautentiseringsuppgifter
```azurecli
az acr credential show -n myRegistry1
```

### <a name="enable-admin-user-for-an-existing-registry"></a>Aktivera en administratörsanvändare för ett befintligt register
```azurecli
az acr update -n myRegistry1 --admin-enabled true
```

### <a name="disable-admin-user-for-an-existing-registry"></a>Inaktivera en administratörsanvändare för ett befintligt register
```azurecli
az acr update -n myRegistry1 --admin-enabled false
```

## <a name="list-images-and-tags"></a>Visa en lista med avbildningar och taggar
Använd hello `az acr` CLI-kommandon tooquery hello bilder och taggar i en databas.

> [!NOTE]
> Behållaren registret stöder för närvarande inte hello `docker search` kommandot tooquery för avbildningar och etiketter.


### <a name="list-repositories"></a>Visa en lista över lagringsplatser
hello visar följande exempel hello databaser i ett register i JSON (JavaScript Object Notation)-format:

```azurecli
az acr repository list -n myRegistry1 -o json
```

### <a name="list-tags"></a>Visa en lista över taggar
hello följande exempel visar hello taggar på hello **exempel/nginx** databasen i JSON-format:

```azurecli
az acr repository show-tags -n myRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nästa steg
* [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md)
