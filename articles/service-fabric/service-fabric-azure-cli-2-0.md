---
title: "aaaGet igång med Azure Service Fabric och Azure CLI 2.0"
description: "Lär dig hur toouse hello Azure Service Fabric-kommandon modul i Azure CLI version 2.0. Lär dig hur tooconnect tooa klustret, och hur toomanage program."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ddbd0ef503dd3fff61494cc2cfa7c9a2e8d0a9a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-and-azure-cli-20"></a>Service Fabric och Azure CLI 2.0

hello Azure kommandoradsverktyget (Azure CLI) version 2.0 innehåller kommandon toohelp som du hanterar Azure Service Fabric-kluster. Lär dig hur tooget igång med Azure CLI och Service Fabric.

## <a name="install-azure-cli-20"></a>Installera Azure CLI 2.0

Du kan använda Azure CLI 2.0 kommandon toointeract med och hantera Service Fabric-kluster. tooget hello senaste versionen av Azure CLI, följ hello [Azure CLI 2.0 standardinstallation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

Mer information finns i hello [översikt över Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Azure CLI-syntax

Alla Service Fabric-kommandon har prefixet `az sf` i Azure CLI. Allmän information om hello-kommandon som du kan använda använda `az sf -h`. För hjälp med ett enda kommando, använd `az sf <command> -h`.

Service Fabric-kommandon i CLI följer detta namngivningsmönster:

```azurecli
az sf <object> <action>
```

`<object>`är hello mål för `<action>`.

## <a name="select-a-cluster"></a>Välj ett kluster

Du måste välja en kluster-tooconnect till innan du utför några åtgärder. Ett exempel finns i hello följande kod. hello koden ansluter tooan oskyddade kluster.

> [!WARNING]
> Använd inte oskyddade Service Fabric-kluster i produktionsmiljöer.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Hej klusterslutpunkten måste föregås av `http` eller `https`. Det måste innehålla hello port för hello HTTP-gateway. hello porten och adressen är hello samtidigt som hello Service Fabric Explorer-URL.

Du kan antingen använda okrypterade .pem-filer, eller .crt- och .key-filer, för kluster som är säkrade med ett certifikat. Exempel:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Mer information finns i [Anslut tooa säker Azure Service Fabric-kluster](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Hej `select` kommando inte fungerar på alla begäranden innan den returnerar. tooverify att du har angett ett kluster korrekt, använder du ett kommando som `az sf cluster health`. Kontrollera att hello kommando inte returnerar några fel.

## <a name="basic-operations"></a>Grundläggande åtgärder

Klustrets anslutningsinformation bevaras mellan olika Azure CLI-sessioner. När du har valt ett Service Fabric-kluster kan du köra ett Service Fabric-kommando på hello klustret.

Till exempel använda tooget hello hälsotillstånd för Service Fabric-kluster, hello följande kommando:

```azurecli
az sf cluster health
```

hello kommandot resulterar i hello följande utdata (förutsatt att JSON-utdata har angetts i konfigurationen för hello Azure CLI):

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a>Felsökning och tips

Du kan hitta hello efter information som är användbar om du stöter på problem när du använder Service Fabric-kommandon i Azure CLI.

### <a name="convert-a-certificate-from-pfx-toopem-format"></a>Konvertera ett certifikat från tooPEM PFX-format

Azure CLI har stöd för certifikat på klientsidan i form av PEM-filer (.pem-filtillägg). Om du använder PFX-filer från Windows, måste du konvertera dessa certifikat tooPEM format. tooconvert en PFX-filen tooa PEM-fil, Använd hello följande kommando:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Mer information finns i hello [OpenSSL dokumentationen](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Anslutningsproblem

Vissa åtgärder kan generera hello följande meddelande:

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

Kontrollera att hello angett klusterslutpunkt är tillgänglig och lyssnar. Kontrollera också att hello Service Fabric Explorer Användargränssnittet är tillgänglig på som värd och port. tooupdate hello slutpunkt och Använd `az sf cluster select`.

### <a name="detailed-logs"></a>Detaljerade loggar

Detaljerade loggar är ofta användbara när du felsöker eller rapporterar problem. Azure CLI erbjuder en global `--debug` flagga som ökar hello detaljnivå loggfiler.

### <a name="command-help-and-syntax"></a>Hjälp och syntax för kommandon

Service Fabric-kommandon Följ hello samma konventioner som Azure CLI. Hjälp med ett visst kommando eller en grupp med kommandon för att använda hello `-h` flaggan:

```azurecli
az sf application -h
```

Här är ett annat exempel:

```azurecli
az sf application create -h
```
