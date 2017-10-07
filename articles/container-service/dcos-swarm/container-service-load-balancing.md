---
title: "aaaLoad saldo behållare i Azure DC/OS-kluster | Microsoft Docs"
description: "Belastningen över flera behållare i ett Azure Container Service DC/OS-kluster."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, Micro-tjänster, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 2249cb06880cdb7e9a3aa94c0750c6a27316d349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Läsa in saldo behållare i ett Azure Container Service DC/OS-kluster
I den här artikeln förklarar vi hur toocreate en intern belastningsutjämnare i DC/OS hanteras Azure Container Service med Marathon-LB. Den här konfigurationen kan du tooscale dina program vågrätt. Du kan också tootake nytta av hello offentliga och privata agent kluster genom att placera din belastningsutjämnare på hello offentliga och programmet behållarna på hello privata klustret. I den här kursen har du:

> [!div class="checklist"]
> * Konfigurera en belastningsutjämnare för Marathon
> * Distribuera ett program med hello belastningsutjämnare
> * Konfigurera och Azure belastningsutjämnare

Du behöver en ACS-DC /-OS klustret toocomplete hello stegen i den här kursen. Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.

Den här kursen kräver hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Översikt för belastningsutjämning

Det finns två lager för belastningsutjämning i ett Azure Container Service DC/OS-kluster: 

**Azure belastningsutjämnare** ger offentliga startpunkterna (hello viktiga att slutanvändare åtkomst till). Ett Azure LB tillhandahålls automatiskt med Azure Container Service och är som standard konfigurerade tooexpose port 80 och 443 8080.

**Hej Marathon belastningsutjämnare (marathon-lb)** vägar inkommande begäranden toocontainer instanser som dessa tjänstbegäranden. Som vi skalanpassar hello behållarna som tillhandahåller våra webbtjänsten hello marathon-lb anpassas efter dynamiskt. Denna belastningsutjämning har inte angetts som standard i Container Service, men det är enkelt tooinstall.

## <a name="configure-marathon-load-balancer"></a>Konfigurera Marathon belastningsutjämnare

Marathon belastningsutjämnare automatiskt dynamiskt baserat på hello-behållare som du har distribuerat. Det är också flexibel toohello förlust av en behållare eller agent - om detta inträffar, Apache Mesos startar hello behållaren någon annanstans och marathon-lb anpassas efter dina behov.

Hello kör följande kommando tooinstall hello marathon belastningsutjämnaren på hello offentlig agent klustret.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Distribuera program för Utjämning av nätverksbelastning

Nu när vi har hello marathon-lb-paketet kan vi distribuera en behållare för program som vi vill tooload saldo. 

Först hämta hello FQDN för hello offentligt exponeras agenter.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Skapa sedan en fil med namnet *hello web.json* och kopiera i hello efter innehållet. Hej `HAPROXY_0_VHOST` etikett måste toobe uppdateras med hello FQDN för hello DC/OS-agenterna. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Använda hello DC/OS CLI toorun hello programmet. Som standard distribuerar Marathon hello hello programobjekt toohello privata kluster. Detta innebär att hello ovan distribution är endast tillgänglig via din belastningsutjämnare, som vanligtvis är hello önskat beteende.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Bläddra toohello FQDN för hello klustret tooview belastningsutjämnade Agentprogrammet när hello programmet har distribuerats.

![Bild av belastningsutjämnade program](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Konfigurera Azure belastningsutjämnare

Som standard exponerar Azure Load Balancer portarna 80, 8080 och 443. Om du använder en av de här tre portarna (som vi gör i hello ovanstående exempel) och det inte finns något du behöver toodo. Du bör vara kan toohit agent läsa in belastningsutjämnings FQDN och varje gång du uppdaterar du tryck något av de tre webbservrarna resursallokering överskrids. 

Om du använder en annan port, behöver du tooadd en resursallokering regel och en avsökning på hello belastningsutjämnare för hello-porten som du använde. Du kan göra detta från hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), med hello kommandon `azure network lb rule create` och `azure network lb probe create`.

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig om belastningsutjämning i ACS med både hello Marathon och Azure belastningen belastningsutjämnare inklusive hello följande åtgärder:

> [!div class="checklist"]
> * Konfigurera en belastningsutjämnare för Marathon
> * Distribuera ett program med hello belastningsutjämnare
> * Konfigurera och Azure belastningsutjämnare

Avancera toohello nästa självstudiekurs toolearn om integreringen med Azure storage med DC/OS i Azure.

> [!div class="nextstepaction"]
> [Montera Azure-filresursen i DC/OS-klustret](container-service-dcos-fileshare.md)