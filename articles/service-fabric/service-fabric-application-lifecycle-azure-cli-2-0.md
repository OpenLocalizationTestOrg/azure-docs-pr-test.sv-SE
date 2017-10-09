---
title: "aaaManage Azure Service Fabric-program med hjälp av Azure CLI 2.0"
description: "Lär dig hur toodeploy och ta bort program från en Azure Service Fabric-kluster med hjälp av Azure CLI 2.0."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ae1ba19513978b0f95ffb65d5f1f7a21ed5f2894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a>Hantera ett Azure Service Fabric-program med hjälp av Azure CLI 2.0

Lär dig hur toocreate och ta bort program som körs i ett Azure Service Fabric-kluster.

## <a name="prerequisites"></a>Krav

* Installera Azure CLI 2.0. Välj sedan Service Fabric-klustret. Mer information finns i [Kom igång med Azure CLI 2.0](service-fabric-azure-cli-2-0.md).

* Har ett Service Fabric application package redo toobe distribueras. Mer information om hur tooauthor och paketet program kan läsa om hello [Service Fabric programmodell](service-fabric-application-model.md).

## <a name="overview"></a>Översikt

toodeploy ett nytt program slutföra de här stegen:

1. Ladda upp ett paket toohello Service Fabric avbildningen programarkiv.
2. Etablera en typ av program.
3. Ange och skapa ett program.
4. Ange och skapa tjänster.

tooremove ett befintligt program slutföra de här stegen:

1. Ta bort hello-programmet.
2. Avetablera hello associerade programtyp.
3. Ta bort hello image store innehåll.

## <a name="deploy-a-new-application"></a>Distribuera ett nytt program

toodeploy ett nytt program fullständig hello följande uppgifter.

### <a name="upload-a-new-application-package-toohello-image-store"></a>Ladda upp en ny paketet toohello bilden appbutik

Ladda upp hello programmet paketet toohello Service Fabric avbildningsarkivet innan du skapar ett program. 

Till exempel om ditt programpaket i hello `app_package_dir` directory, Använd hello följande kommandon tooupload hello directory:

```azurecli
az sf application upload --path ~/app_package_dir
```

För stora programpaket kan du ange hello `--show-progress` alternativet toodisplay hello fortskrider hello överföringen.

### <a name="provision-hello-application-type"></a>Etablera hello programtyp

Etablera hello programmet när hello överföringen är klar. tooprovision hello program, Använd hello följande kommando:

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

Hej värdet för `application-type-build-path` är hello namnet på hello katalog där du laddade upp ditt programpaket.

### <a name="create-an-application-from-an-application-type"></a>Skapa ett program från en typ av program

När du etablerar hello program kan använda hello följande kommando tooname och skapa programmet:

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`är hello namn som du vill toouse för hello programinstansen. Du kan få ytterligare parametrar från hello tidigare etablerade programmanifestet.

hello programmets namn måste börja med hello prefixet `fabric:/`.

### <a name="create-services-for-hello-new-application"></a>Skapa tjänster för hello nytt program

När du har skapat ett program kan skapa tjänster från hello program. I följande exempel hello, skapar vi en ny tjänst för tillståndslösa från våra program. hello-tjänster som du kan skapa från ett program som har definierats i en tjänstmanifestet i hello tidigare etablerade programpaketet.

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Kontrollera programdistribution och hälsa

tooverify att ett program och tjänsten har distribuerats, kontrollera att programmet hello och tjänsten visas:

```azurecli
az sf application list
az sf service list --application-list TestApp
```

tooverify att hello-tjänsten är felfri, Använd liknande kommandon tooretrieve hello hälsotillståndet för både hello-tjänst och hello program:

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

Felfri tjänster och program har en `HealthState` värdet för `Ok`.

## <a name="remove-an-existing-application"></a>Ta bort ett befintligt program

tooremove ett program, fullständig hello följande uppgifter.

### <a name="delete-hello-application"></a>Ta bort programmet hello

toodelete hello program, Använd hello följande kommando:

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a>Avetablera hello programtyp

När du har tagit bort programmet hello avetablera du hello programtyp om du inte längre behöver. toounprovision hello programtyp, Använd hello följande kommando:

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

hello typen namn och typ versionen måste matcha hello namn och version i hello tidigare etablerade programmanifestet.

### <a name="delete-hello-application-package"></a>Ta bort hello programpaket

När du har avetablerade hello programtyp, kan du ta bort hello programpaket från avbildningsarkivet hello om du inte längre behöver. Om du tar bort programpaket kan frigöra utrymme på hårddisken. 

toodelete hello programpaket från avbildningsarkivet hello, Använd hello följande kommando:

```azurecli
az sf application package-delete --content-path app_package_dir
```

`content-path`måste vara hello namnet på hello-katalog som du laddade upp när du skapade programmet hello.

## <a name="related-articles"></a>Relaterade artiklar

* [Kom igång med Service Fabric och Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Kom igång med Service Fabric XPlat CLI](service-fabric-azure-cli.md)
