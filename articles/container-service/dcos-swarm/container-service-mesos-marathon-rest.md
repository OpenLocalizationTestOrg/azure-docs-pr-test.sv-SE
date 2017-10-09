---
title: aaaManage Azure DC/OS-kluster med Marathon REST API | Microsoft Docs
description: "Distribuera behållare tooan Azure Container Service DC/OS-klustret med hjälp av hello Marathon REST API."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: d926b9b90f5d4eda85a015d9ea0d96fea2c4b566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-container-management-through-hello-marathon-rest-api"></a>DC/OS-hantering av behållare via hello Marathon REST API
DC/OS erbjuder en miljö för att distribuera och skala klustrade arbetsbelastningar samtidigt abstrahera hello underliggande maskinvara. Utöver DC/OS finns det ett ramverk som hanterar schemaläggning och beräkning av arbetsbelastningar. Även om ramverk är tillgängliga för många populära arbetsbelastningar, hjälper det här dokumentet dig att börja skapa och skala distribution i behållare med hjälp av hello Marathon REST API. 

## <a name="prerequisites"></a>Krav

Innan du börjar med de här exemplen behöver du ett DC/OS-kluster som har konfigurerats i Azure Container Service. Du måste också toohave fjärranslutningar toothis klustret. Mer information om dessa objekt finns i hello följande artiklar:

* [Distribuera ett Azure Container Service-kluster](container-service-deployment.md)
* [Ansluta tooan Azure Container Service-kluster](../container-service-connect.md)

## <a name="access-hello-dcos-apis"></a>Komma åt hello DC/OS-API: er
När du är ansluten toohello Azure Container Service-kluster kan du komma åt hello DC/OS och relaterade REST API: er via http://localhost: Local-port. hello exemplen i det här dokumentet förutsätter att du använder tunneltrafik på port 80. Till exempel hello Marathon slutpunkter kan nås på URI: er från och med `http://localhost/marathon/v2/`. 

Mer information om hello olika API: er, se hello Mesosphere-dokumentationen för hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) och [Chronos API](https://mesos.github.io/chronos/docs/api.html), samt Apache-dokumentationen för hello [Mesos Scheduler API ](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Samla in information från DC/OS och Marathon
Innan du distribuerar behållare toohello DC/OS-klustret måste du samla in information om hello DC/OS-klustret, till exempel hello namn och statusen för hello DC/OS-agenterna. toodo fråga så hello `master/slaves` slutpunkten för hello DC/OS REST API. Om allt går bra returnerar hello frågan en lista över DC/OS-agenterna och flera egenskaper för varje.

```bash
curl http://localhost/mesos/master/slaves
```

Nu kan använda hello Marathon `/apps` endpoint toocheck för aktuella program distributioner toohello DC/OS-klustret. Om det här är ett nytt kluster visas en tom matris för appar.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Distribuera en Docker-formaterad behållare
Du kan distribuera Docker-formaterade behållare via hello Marathon REST API med hjälp av en JSON-fil som beskriver hello avsedda distributionen. hello distribueras följande exempel Nginx-behållaren tooa privata agent i hello kluster. 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

toodeploy Docker-formaterad behållare lagra hello JSON-fil i en tillgänglig plats. Nästa toodeploy hello behållare, kör följande kommando hello. Ange hello namn hello JSON-fil (`marathon.json` i det här exemplet).

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

hello-utdata är liknande toohello följande:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Om du frågar Marathon efter program nu visas den här nya programmet i hello utdata.

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-hello-container"></a>Nå hello behållare

Du kan kontrollera att hello Nginx körs i en behållare på en av hello privata agenter i hello kluster. toofind hello-värd och port där hello behållare körs frågar Marathon efter hello pågående aktiviteter: 

```bash
curl localhost/marathon/v2/tasks
```

Hitta hello värdet för `host` i hello utdata (en IP-adress för liknande`10.32.0.x`), och hello värdet för `ports`.


Nu göra en SSH terminal (inte en tunnel anslutning) toohello anslutningshanteringen FQDN hello-klustret. När du är ansluten, att Hej på begäran, ersätter hello korrekta värden för `host` och `ports`:

```bash
curl http://host:ports
```

Hej Nginx server-utdata är liknande toohello följande:

![Nginx från behållaren](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a>Skala behållarna
Du kan använda hello Marathon API tooscale ut eller skala in programdistributioner. Hello föregående exempel distribuerade du en instans av ett program. Nu kan vi skala detta ut toothree instanser av ett program. toodo så, skapa en JSON-fil med hjälp av hello följande JSON-text och lagra den på en tillgänglig plats.

```json
{ "instances": 3 }
```

Kör följande kommando tooscale ut hello programmet hello från anslutningens tunnel.

> [!NOTE]
> hello URI är http://localhost/marathon/v2/apps/ följt av hello-ID för hello programmet tooscale. Om du använder hello Nginx-exemplet som anges här, är hello URI: N http://localhost/marathon/v2/apps/nginx.
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Fråga slutligen hello Marathon-slutpunkten för program. Du ser nu att det finns tre Nginx-behållare.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Motsvarande PowerShell-kommandon
Du kan utföra samma åtgärder genom att använda PowerShell-kommandon på en Windows-dator.

toogather information om hello DC/OS-klustret, t.ex agentnamn och agentstatus kör hello följande kommando:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Du kan distribuera Docker-formaterad behållare via Marathon genom att använda en JSON-fil som beskriver hello avsedda distributionen. hello distribuerar följande exempel hello Nginx-behållaren, bindning hello DC/OS-agenten tooport 80 hello behållarens port 80.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

toodeploy Docker-formaterad behållare lagra hello JSON-fil i en tillgänglig plats. Nästa toodeploy hello behållare, kör följande kommando hello. Ange hello sökvägen toohello JSON-fil (`marathon.json` i det här exemplet).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Du kan också använda hello Marathon API tooscale ut eller skala in programdistributioner. Hello föregående exempel distribuerade du en instans av ett program. Nu kan vi skala detta ut toothree instanser av ett program. toodo så, skapa en JSON-fil med hjälp av hello följande JSON-text och lagra den på en tillgänglig plats.

```json
{ "instances": 3 }
```

Kör följande kommando tooscale ut hello programmet hello:

> [!NOTE]
> hello URI är http://localhost/marathon/v2/apps/ följt av hello-ID för hello programmet tooscale. Om du använder hello Nginx-exemplet som anges här, är hello URI: N http://localhost/marathon/v2/apps/nginx.
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Nästa steg
* [Läs mer om hello Mesos HTTP-slutpunkter](http://mesos.apache.org/documentation/latest/endpoints/)
* [Läs mer om hello Marathon REST API](https://mesosphere.github.io/marathon/docs/rest-api.html)

