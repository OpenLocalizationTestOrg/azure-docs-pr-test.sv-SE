---
title: aaaConsume en Azure hanterat program | Microsoft Docs
description: "Beskriver hur en kund skapar en Azure hanterade program från publicerade filer."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: b8510086eb05304c0e351a391b7e0cf34a467568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-internal-managed-application"></a>Använda en intern hanterad App

Du kan använda Azure [hanterade program](managed-application-overview.md) som är avsedda för medlemmar i din organisation. Du kan till exempel välja hanterade program från IT-avdelningen som säkerställa efterlevnad med organisationens normer. Dessa hanterade program är tillgängliga via hello Tjänstkatalog, inte hello Azure Marketplace.

Innan du fortsätter med den här artikeln, måste du ha ett hanterat program som är tillgängliga i hello tjänstkatalogen för din prenumeration. Om någon i din organisation redan inte har skapat ett hanterat program, se [publicera ett hanterat program för internt bruk](managed-application-publishing.md).

För närvarande kan du använda Azure CLI eller hello Azure portal tooconsume hanterade program.

## <a name="create-hello-managed-application-by-using-hello-portal"></a>Skapa hello hanterade program med hjälp av hello-portalen

toodeploy hanterade programmet hello-portalen så här:

1. Gå toohello Azure-portalen. Sök efter **Tjänstkatalogen hanterat program**.

   ![Tjänstkatalogen hanterade program](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Välj hello hanterade program som du vill toocreate hello listan över tillgängliga lösningar. Välj **Skapa**.

   ![Val av hanterade program](./media/managed-application-consumption/select-offer.png)

1. Ange värden för hello-parametrar som är nödvändiga tooprovision hello resurser. Välj **Väst centrala oss** för platsen. Välj **OK**.

   ![Parametrar för hanterade program](./media/managed-application-consumption/input-parameters.png)

1. hello mallen verifierar hello-värden som du angav. Om verifieringen lyckas, väljer **OK** toostart hello distribution.

   ![Validering av hanterade program](./media/managed-application-consumption/validation.png)

När hello distributionen är klar etableras hello lämpliga resurser som definierats i mallen för hello i hanterade hello resursgrupp som du angav.

## <a name="create-hello-managed-application-by-using-azure-cli"></a>Skapa hello hanterade program med hjälp av Azure CLI

Det finns två sätt toocreate ett hanterat program med hjälp av Azure CLI:

* Hello kommandot för att skapa hanterade program.
* Kommandot hello reguljära mallen distribution.

### <a name="use-hello-template-deployment-command"></a>Kommandot hello mall för distribution

Distribuera hello applianceMainTemplate.json fil som hello leverantör skapas.

Skapa sedan två resursgrupper. hello första resursgruppen är där hello hanterade programresursen har skapats: Microsoft.Solutions/appliances. hello andra resursgruppen innehåller alla hello-resurser som definierats i mainTemplate.json. Den här resursgruppen hanteras av hello ISV.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Använd `westcentralus` som hello plats hello resursgruppen.
>

toodeploy applianceMainTemplate.json i mainResourceGroup, Använd hello följande kommando:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Efter hello föregående mall körs, ombeds du ange hello värdena för hello-parametrar som definierats i mallen för hello. Dessutom toohello parametrar som behövs tooprovision resurser i en mall måste du två viktiga parametervärden:

- **managedResourceGroupId**: hello-ID för hello resurs som innehåller hello gruppresurser definieras i applianceMainTemplate.json. hello-ID är hello formatet `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. I föregående exempel hello, det är hello-ID för `managedResourceGroup`.
- **applianceDefinitionId**: hello-ID för hello hanterade definition programresursen. Det här värdet anges av hello ISV.

> [!NOTE]
> hello publisher måste ge åtkomst toohello resursgruppen som innehåller definitionen av hello hanterade program. hello definition resursen skapas i hello publisher prenumeration. Därför måste en användare, grupp eller programmet hello kunden klient läsbehörighet toothis resurs.

När hello distributionen är klar har du se hello hanterade program skapas i mainResourceGroup. Hej storageAccount resursen skapas i managedResourceGroup.

### <a name="use-hello-create-command"></a>Använd hello skapa kommando

Du kan använda hello `az managedapp create` kommandot toocreate ett hanterat program från hello hanteras programmets definition.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **installation-definition-Id**: hello resurs-ID för hello hanterade definition för program som skapats i hello föregående steg. tooobtain-ID, kör hello följande kommando:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Det här kommandot returnerar definition för hello hanterade program. Du måste hello värdet för hello ID-egenskap.

* **hanterade-rg-id**: hello namnet på hello resurs som innehåller hello gruppresurser definieras i applianceMainTemplate.json. Den här resursgruppen tillhör hello hanterade resursgruppen. Den hanteras av hello utgivare. Om det inte finns, skapas den åt dig.
* **resursgruppens namn**: hello resursgruppen där hello hanteras programresursen har skapats. Hej Microsoft.Solutions/appliance resursen finns i den här resursgruppen.
* **parametrarna**: hello parametrar som behövs för hello resurser som definierats i applianceMainTemplate.json.

## <a name="known-issues"></a>Kända problem

Den här förhandsversionen innehåller hello följande problem:

* En 500 Internt serverfel visas under hello skapandet av hello hanterade program. Om du stöter på problemet är sannolikt toobe tillfälligt. Försök igen om hello.
* Du behöver en ny resursgrupp hello hanterade resursgruppen. Om du använder en befintlig resursgrupp misslyckas hello distributionen.
* hello resursgruppen som innehåller hello Microsoft.Solutions/appliances resursen måste skapas i hello **westcentralus** plats.

## <a name="next-steps"></a>Nästa steg

* En introduktion toomanaged program, se [hanteras Programöversikt](managed-application-overview.md).
* Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).
* Information om publishing hanterade program toohello Azure Marketplace finns [Azure hanterade program i hello Marketplace](managed-application-author-marketplace.md).
* Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).
