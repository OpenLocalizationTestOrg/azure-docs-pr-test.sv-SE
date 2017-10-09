---
title: "aaaManage Azure Service Fabric-program med hjälp av Azure Service Fabric CLI"
description: "Lär dig hur toodeploy och ta bort program från en Azure Service Fabric-kluster med hjälp av Azure Service Fabric CLI"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: d9f98cee1d70f71a2aab68ff556956619910e4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a>Hantera ett Azure Service Fabric-program med hjälp av Azure Service Fabric CLI

Lär dig hur toocreate och ta bort program som körs i ett Azure Service Fabric-kluster.

## <a name="prerequisites"></a>Krav

* Installera Service Fabric CLI. Välj sedan Service Fabric-klustret. Mer information finns i [komma igång med Service Fabric CLI](service-fabric-cli.md).

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

toodeploy ett nytt program fullständig hello följande uppgifter:

### <a name="upload-a-new-application-package-toohello-image-store"></a>Ladda upp en ny paketet toohello bilden appbutik

Ladda upp hello programmet paketet toohello Service Fabric avbildningsarkivet innan du skapar ett program.

Till exempel om ditt programpaket i hello `app_package_dir` directory, Använd hello följande kommandon tooupload hello directory:

```azurecli
sfctl application upload --path ~/app_package_dir
```

För stora programpaket kan du ange hello `--show-progress` alternativet toodisplay hello fortskrider hello överföringen.

### <a name="provision-hello-application-type"></a>Etablera hello programtyp

Etablera hello programmet när hello överföringen är klar. tooprovision hello program, Använd hello följande kommando:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

Hej värdet för `application-type-build-path` är hello namnet på hello katalog där du laddade upp ditt programpaket.

### <a name="create-an-application-from-an-application-type"></a>Skapa ett program från en typ av program

När du etablerar hello program kan använda hello följande kommando tooname och skapa programmet:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`är hello namn som du vill toouse för hello programinstansen. Du kan få ytterligare parametrar från tidigare etablerade programmanifestet.

hello programmets namn måste börja med hello prefixet `fabric:/`.

### <a name="create-services-for-hello-new-application"></a>Skapa tjänster för hello nytt program

När du har skapat ett program kan skapa tjänster från hello program. I följande exempel hello, skapar vi en ny tjänst för tillståndslösa från våra program. hello-tjänster som du kan skapa från ett program som har definierats i en tjänstmanifestet i hello tidigare etablerade programpaketet.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Kontrollera programdistribution och hälsa

tooverify allt är felfri, använder du följande kommandon för health hello:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

tooverify att hello-tjänsten är felfri, Använd liknande kommandon tooretrieve hello hälsotillståndet för både hello-tjänsten och programmet:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

Felfri tjänster och program har en `HealthState` värdet för `Ok`.

## <a name="remove-an-existing-application"></a>Ta bort ett befintligt program

tooremove ett program, fullständig hello följande uppgifter:

### <a name="delete-hello-application"></a>Ta bort programmet hello

toodelete hello program, Använd hello följande kommando:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a>Avetablera hello programtyp

När du har tagit bort programmet hello avetablera du hello programtyp om du inte längre behöver. toounprovision hello programtyp, Använd hello följande kommando:

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

hello typen namn och typ versionen måste matcha hello namn och version i hello tidigare etablerade programmanifestet.

### <a name="delete-hello-application-package"></a>Ta bort hello programpaket

När du har avetablerade hello programtyp, kan du ta bort hello programpaket från avbildningsarkivet hello om du inte längre behöver. Om du tar bort programpaket kan frigöra utrymme på hårddisken. 

toodelete hello programpaket från avbildningsarkivet hello, Använd hello följande kommando:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path`måste vara hello namnet på hello-katalog som du laddade upp när du skapade programmet hello.

## <a name="upgrade-application"></a>Uppgradera program

När du har skapat programmet, du kan upprepa hello samma uppsättning steg tooprovision en andra versioner av programmet. Du kan sedan övergång toorunning hello andra versionen av programmet hello med en uppgradering av Service Fabric-programmet. Mer information finns i dokumentationen för hello på [Service Fabric programuppgraderingar](service-fabric-application-upgrade.md).

tooperform en uppgradering första etablera hello nästa version av hello använder hello samma kommandon som tidigare:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

Det rekommenderas sedan tooperform en övervakade automatisk uppgradering starta hello uppgraderingen genom att köra följande kommando hello:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Uppgraderingar åsidosätta befintliga parametrar med oavsett set anges. Programmet parametrar ska skickas som argument toohello uppgradera kommando, om det behövs. Parametrar för programmet ska vara kodad som ett JSON-objekt.

tooretrieve parametrar anges tidigare kan du använda hello `sfctl application info` kommando.

När en uppgradering av programmet pågår hello status kan hämtas med hjälp av den `sfctl application upgrade-status` kommando.

Slutligen, om en uppgradering pågår och behöver toobe avbrytas, kan du använda hello `sfctl application upgrade-rollback` tooroll tillbaka hello uppgraderingen.

## <a name="next-steps"></a>Nästa steg

* [Grunderna i Service Fabric CLI](service-fabric-cli.md)
* [Komma igång med Service Fabric på Linux](service-fabric-get-started-linux.md)
* [Starta en uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md)
