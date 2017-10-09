---
title: "aaaDeploy behållare med Helm i Azure Kubernetes | Microsoft Docs"
description: "Använd hello Helm paketering verktyget toodeploy behållare på ett Kubernetes kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: c7bd780afe00084ebe4e3a14873e1e340a29d144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-helm-toodeploy-containers-on-a-kubernetes-cluster"></a>Använd Helm toodeploy behållare på ett Kubernetes kluster 

[Helm](https://github.com/kubernetes/helm/) är en öppen källkod paketering verktyg som hjälper dig att installera och hantera hello livscykeln för Kubernetes program. Liknande tooLinux paketet chefer som lgh get och Yum, Helm är används toomanage Kubernetes diagram som är paket med förkonfigurerade Kubernetes resurser. Den här artikeln visar hur toowork med Helm på ett Kubernetes kluster distribueras i Azure Container Service.

Helm består av två komponenter: 
* Hej **Helm CLI** är en klient som körs på datorn lokalt eller i molnet hello  

* **Rorkulten** är en server som körs på hello Kubernetes klustret och hanterar hello livscykeln för Kubernetes-program 
 
## <a name="prerequisites"></a>Krav

* [Skapa ett kluster med Kubernetes](container-service-kubernetes-walkthrough.md) i Azure Container Service

* [Installera och konfigurera `kubectl` ](../container-service-connect.md) på en lokal dator

* [Installera Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) på en lokal dator

## <a name="helm-basics"></a>Helm grunderna 

tooview information om hello Kubernetes klustret att du installerar rorkulten och distribuerar program för att ange hello följande kommando:

```bash
kubectl cluster-info 
```
![kubectl klusterinformation](./media/container-service-kubernetes-helm/clusterinfo.png)
 
När du har installerat Helm installera rorkulten på Kubernetes klustret genom att skriva följande kommando hello:

```bash
helm init --upgrade
```
När den är klar kan du se utdata som liknar hello följande:

![Rorkulten installation](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
tooview alla hello Helm diagram tillgängliga i hello-lagringsplatsen, typen hello följande kommando:

```bash 
helm search 
```

Du kan se utdata som liknar hello följande:

![Helm sökning](./media/container-service-kubernetes-helm/helm-search.png)
 
tooupdate hello diagram tooget hello senaste versioner, skriver du:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Distribuera ett Nginx ingång controller diagram 
 
toodeploy en Nginx ingång controller diagram, skriver du ett enda kommando:

```bash
helm install stable/nginx-ingress 
```
![Distribuera ingång-styrenhet](./media/container-service-kubernetes-helm/nginx-ingress.png)

Om du anger `kubectl get svc` tooview alla tjänster som körs på hello klustret kan du se att en IP-adress har tilldelats toohello ingång domänkontrollant. (Under hello tilldelning pågår visas `<pending>`. Det tar några minuter toocomplete.) 

Navigera toohello värdet för hello externa IP-adress toosee hello Nginx backend körs när hello IP-adress har tilldelats. 
 
![Ingång IP-adress](./media/container-service-kubernetes-helm/ingress-ip-address.png)


toosee en lista över diagram installeras på klustret, typ:

```bash
helm list 
```

Du kan förkorta hello kommandot för`helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Distribuera ett MariaDB diagram och klient

Nu distribuera MariaDB diagram och en MariaDB klienten tooconnect toohello databas.

toodeploy hello MariaDB diagram, typen hello följande kommando:

```bash
helm install --name v1 stable/mariadb
```

där `--name` är en tagg som används för versioner.

> [!TIP]
> Om hello distributionen misslyckas kör `helm repo update` och försök igen.
>
 
 
tooview alla hello diagram som har distribuerats på klustret, typ:

```bash 
helm list
```
 
tooview skriver du alla distributioner som körs på klustret:

```bash
kubectl get deployments 
``` 
 
 
Slutligen toorun baljor tooaccess hello klienten, skriver du:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
tooconnect toohello klienten, typen hello följande kommando, där du ersätter `v1-mariadb` med hello namn för din distribution:

```bash
sudo mysql –h v1-mariadb
```
 
 
Du kan nu använda standard toocreate databaser för SQL-kommandon, tabeller osv. Till exempel `Create DATABASE testdb1;` skapar en tom databas. 
 
 
 
## <a name="next-steps"></a>Nästa steg

* Mer information om hur du hanterar Kubernetes diagram finns hello [Helm dokumentationen](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

