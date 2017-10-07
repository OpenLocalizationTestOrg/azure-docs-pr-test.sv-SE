---
title: "aaaGet igång med Azure Service Fabric CLI (sfctl)"
description: "Lär dig hur toouse hello Azure Service Fabric CLI. Lär dig hur tooconnect tooa klustret, och hur toomanage program."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: f76e8ff65bb38dfb63791da0a23e19b93b337f6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-command-line"></a>Azure Service Fabric-kommandorad

hello Azure Service Fabric CLI (sfctl) är ett kommandoradsverktyg för interagerar och hantera Azure Service Fabric-enheter. Sfctl kan användas med Windows- eller Linux-kluster. Sfctl fungerar på alla plattformar där python stöds.

## <a name="prerequisites"></a>Krav

Tidigare tooinstallation, kontrollera att din miljö innehåller både python och pip-installeras. Mer information, ta en titt på hello [pip quickstart dokumentationen](https://pip.pypa.io/en/latest/quickstart/), och det officiella [python installera dokumentationen](https://wiki.python.org/moin/BeginnersGuide/Download).

När båda python 2.7 och 3,6 stöds rekommenderas toouse python 3,6.

## <a name="install"></a>Installera

hello Azure Service Fabric CLI (sfctl) levereras som en python-paket. tooinstall hello senaste versionen körs:

```bash
pip install sfctl
```

Efter installation kör `sfctl -h` tooget information om tillgängliga kommandon.

## <a name="cli-syntax"></a>CLI-syntax

Kommandon har alltid prefixet `sfctl`. För allmän information om alla kommandon som du kan använda, använd `sfctl -h`. För hjälp med ett enda kommando, använd `sfctl <command> -h`.

Kommandon Följ en repeterbara struktur med hello målet för hello kommandot föregående hello verb eller åtgärd:

```azurecli
sfctl <object> <action>
```

I det här exemplet `<object>` är hello mål för `<action>`.

## <a name="select-a-cluster"></a>Välj ett kluster

Du måste välja en kluster-tooconnect till innan du utför några åtgärder. Till exempel kör hello följande tooselect och ansluta toohello kluster med namnet hello `testcluster.com`.

> [!WARNING]
> Använd inte oskyddade Service Fabric-kluster i produktionsmiljöer.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Hej klusterslutpunkten måste föregås av `http` eller `https`. Det måste innehålla hello port för hello HTTP-gateway. hello porten och adressen är hello samtidigt som hello Service Fabric Explorer-URL.

För kluster som skyddas med ett certifikat kan du ange ett PEM-kodat certifikat. hello certifikat kan anges som en enda fil eller ett certifikat och nyckelpar.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Mer information finns i [Anslut tooa säker Azure Service Fabric-kluster](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Grundläggande åtgärder

Klustrets anslutningsinformation bevaras mellan olika sfctl-sessioner. När du har valt ett Service Fabric-kluster kan du köra ett Service Fabric-kommando på hello klustret.

Till exempel använda tooget hello hälsotillstånd för Service Fabric-kluster, hello följande kommando:

```azurecli
sfctl cluster health
```

hello kommandot resulterar i hello följande utdata:

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

Några förslag och tips för att lösa vanliga problem.

### <a name="convert-a-certificate-from-pfx-toopem-format"></a>Konvertera ett certifikat från tooPEM PFX-format

hello Service Fabric CLI stöder klientens certifikat som PEM-filer (.pem tillägg). Om du använder PFX-filer från Windows, måste du konvertera dessa certifikat tooPEM format. tooconvert en PFX-filen tooa PEM-fil, använder du följande kommando:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Mer information finns i hello [OpenSSL dokumentationen](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Anslutningsproblem

Vissa åtgärder kan generera hello följande meddelande:

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

Kontrollera att hello angett klusterslutpunkt är tillgänglig och lyssnar. Kontrollera också att hello Service Fabric Explorer Användargränssnittet är tillgänglig på som värd och port. tooupdate hello slutpunkt och Använd `sfctl cluster select`.

### <a name="detailed-logs"></a>Detaljerade loggar

Detaljerade loggar är ofta användbara när du felsöker eller rapporterar problem. Det finns en global `--debug` flagga som ökar hello detaljnivå loggfiler.

### <a name="command-help-and-syntax"></a>Hjälp och syntax för kommandon

Hjälp med ett visst kommando eller en grupp med kommandon för att använda hello `-h` flaggan:

```azurecli
sfctl application -h
```

Ett annat exempel:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Nästa steg

* [Distribuera ett program med hello Azure Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
* [Kom igång med Service Fabric på Linux](service-fabric-get-started-linux.md)
