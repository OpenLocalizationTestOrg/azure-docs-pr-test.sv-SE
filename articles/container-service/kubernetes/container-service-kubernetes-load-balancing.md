---
title: "aaaLoad balansera Kubernetes behållare i Azure | Microsoft Docs"
description: "Anslut externt och belastningen över flera behållare i ett Kubernetes kluster i Azure Container Service."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, Micro-tjänster, Kubernetes, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8073c8d3a015a53a532c326749571cb2582e1bac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a>Läsa in saldo behållare i ett Kubernetes kluster i Azure Container Service 
Den här artikeln introducerar belastningsutjämning i ett Kubernetes kluster i Azure Container Service. Belastningsutjämning tillhandahåller en externt tillgänglig IP-adress för hello service och distribuerar trafik mellan hello skida körs i agenten virtuella datorer.

Du kan ställa in en Kubernetes service toouse [Azure belastningsutjämnare](../../load-balancer/load-balancer-overview.md) toomanage extern (TCP) nätverkstrafik. Läsa in belastningsutjämning och routning av HTTP eller HTTPS-trafik eller mer avancerade scenarier är möjliga med ytterligare konfiguration.

## <a name="prerequisites"></a>Krav
* [Distribuera ett kluster Kubernetes](container-service-kubernetes-walkthrough.md) i Azure Container Service
* [Ansluta klienten](../container-service-connect.md) tooyour kluster

## <a name="azure-load-balancer"></a>Azure belastningsutjämnare

Som standard innehåller ett Kubernetes kluster som distribueras i Azure Container Service en Azure belastningsutjämnare mot Internet för hello agent virtuella datorer. (En separat belastningsutjämningsresurs har konfigurerats för hello master virtuella datorer.) Azure belastningsutjämnare är Layer 4 belastningsutjämning. Hello belastningsutjämnaren stöder för närvarande endast TCP-trafik i Kubernetes.

När du skapar en Kubernetes-tjänst kan konfigurera du hello Azure belastningen belastningsutjämnaren tooallow toohello tjänsten automatiskt. tooconfigure hello belastningsutjämnare, ange hello tjänst `type` för`LoadBalancer`. hello belastningsutjämnaren skapar en regel toomap en offentlig IP-adress och portnummer för inkommande trafik toohello privata IP-adresser och portnummer för hello skida i agenten virtuella datorer (och vice versa för svarstrafik). 

 Följande är två exempel visar hur tooset hello Kubernetes service `type` för`LoadBalancer`. (När du försöker hello exempel, ta bort hello distributioner om du inte längre behöver dem..)

### <a name="example-use-hello-kubectl-expose-command"></a>Exempel: Använd hello `kubectl expose` kommando 
Hej [Kubernetes genomgången](container-service-kubernetes-walkthrough.md) innehåller ett exempel på hur tooexpose en tjänst med hello `kubectl expose` kommandot och dess `--type=LoadBalancer` flaggan. Här är hello steg:

1. Starta en ny distribution i behållare. Hej exempelvis följande kommando startar en ny distribution kallas `mynginx`. hello-distribution består av tre behållare baserat på hello Docker bild för hello Nginx-webbserver.

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. Kontrollera att hello behållare körs. Om du fråga efter hello behållare med till exempel `kubectl get pods`, visas utdata liknande toohello följande:

    ![Hämta Nginx-behållare](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. tooconfigure hello belastningen belastningsutjämnaren tooaccept externa trafiken toohello distribution, kör `kubectl expose` med `--type=LoadBalancer`. hello visar följande kommando hello Nginx-server på port 80:

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. Typen `kubectl get svc` toosee hello tjänsttillstånd hello hello klustret. Medan hello belastningsutjämnaren konfigurerar hello regeln, hello `EXTERNAL-IP` av hello tjänsten visas som `<pending>`. Efter några minuter konfigureras hello extern IP-adress: 

    ![Konfigurera Azure belastningsutjämnare](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. Kontrollera att du kan komma åt hello-tjänsten på hello extern IP-adress. Till exempel öppna en webbläsare toohello IP webbadress visas. hello webbläsaren visar hello Nginx webbserver som körs i ett av hello behållare. Eller kör hello `curl` eller `wget` kommando. Exempel:

    ```
    curl 13.82.93.130
    ```

    Du bör se utdata som liknar följande:

    ![Åtkomst Nginx med curl](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. toosee hello konfigurationen av hello Azure belastningsutjämnare, gå toohello [Azure-portalen](https://portal.azure.com).

7. Bläddra efter hello resursgrupp för ditt behållartjänstklustret och välj hello belastningsutjämnaren för hello agent virtuella datorer. Namnet ska vara hello samma som hello behållartjänsten. (Inte välja hello belastningsutjämnaren för hello överordnade noder, hello en vars namn innehåller **master-lb**.) 

    ![Belastningsutjämnare i resursgruppen.](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. toosee hello information om konfiguration av belastningsutjämning hello, klickar du på **belastningsutjämningsregler** och hello namnet på hello-regel som har konfigurerats.

    ![Belastningsutjämningsreglerna](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-hello-service-configuration-file"></a>Exempel: Ange `type: LoadBalancer` i konfigurationsfilen för hello-tjänsten

Om du distribuerar en Kubernetes behållare app från en YAML eller JSON [tjänstkonfigurationsfilen](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), ange en extern belastningsutjämnare genom att lägga till hello följande rad toohello service specifikationen:

```YAML
 "type": "LoadBalancer"
``` 



hello följande använda hello Kubernetes [gästbok exempel](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook). Det här exemplet är ett webbprogram med flera nivåer baserat på Redis- och PHP Docker-avbildningar. Du kan ange i hello tjänstkonfigurationsfilen hello klientdel PHP servern använder hello Azure belastningsutjämnare.

1. Hämta hello filen `guestbook-all-in-one.yaml` från [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one). 
2. Bläddra efter hello `spec` för hello `frontend` service.
3. Ta bort kommentarerna hello rad `type: LoadBalancer`.

    ![Belastningsfördelning i tjänstekonfigurationen](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. Spara filen med hello och distribuera hello app genom att köra följande kommando hello:

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. Typen `kubectl get svc` toosee hello tjänsttillstånd hello hello klustret. Medan hello belastningsutjämnaren konfigurerar hello regeln, hello `EXTERNAL-IP` av hello `frontend` tjänsten visas som `<pending>`. Efter några minuter konfigureras hello extern IP-adress: 

    ![Konfigurera Azure belastningsutjämnare](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. Kontrollera att du kan komma åt hello-tjänsten på hello extern IP-adress. Du kan exempelvis öppna en web webbläsare toohello externa IP-adress hello-tjänsten.

    ![Åtkomst gästbok externt](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    Du kan lägga till gästbok poster.

7. toosee hello konfigurationen av hello Azure belastningsutjämnare, bläddra efter hello belastningsutjämningsresurs för hello-kluster i hello [Azure-portalen](https://portal.azure.com). Se hello stegen i föregående exempel för hello.

### <a name="considerations"></a>Överväganden

* Skapa regel för belastningsutjämnare hello sker asynkront och information om hello etablerats belastningsutjämnaren har publicerats i hello service `status.loadBalancer` fältet.
* Varje tjänst tilldelas automatiskt sin egen virtuella IP-adressen i hello belastningsutjämnaren.
* Om du vill tooreach hello belastningsutjämnaren med ett DNS-namn, arbeta med din domän service provider toocreate ett DNS-namn för hello regeln IP-adress.

## <a name="http-or-https-traffic"></a>HTTP eller HTTPS-trafik

tooload saldo HTTP eller HTTPS-trafik toocontainer webbappar och hantera certifikat för transport layer security (TLS), kan du använda hello Kubernetes [ingång](https://kubernetes.io/docs/user-guide/ingress/) resurs. Ett meddelande om ingångs är en uppsättning regler som tillåter inkommande anslutningar tooreach hello klustertjänsterna. För en resurs toowork ingång hello Kubernetes klustret måste ha en [ingång controller](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) körs.

Azure Container Service implementerar inte en Kubernetes ingång domänkontrollant automatiskt. Det finns flera implementeringar av domänkontrollanten. För närvarande hello [Nginx ingång controller](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) rekommenderas tooconfigure ingång regler och belastningsutjämning HTTP och HTTPS-trafik. 

Mer information finns i hello [Nginx ingång controller dokumentationen](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).

> [!IMPORTANT]
> När du använder hello Nginx ingång domänkontrollant i Azure Container Service kan du visa hello distributionen av domänkontrollanter som en tjänst med `type: LoadBalancer`. Detta konfigurerar hello Azure belastningen belastningsutjämnaren tooroute trafik toohello domänkontrollant. Mer information finns i hello föregående avsnitt.


## <a name="next-steps"></a>Nästa steg

* Se hello [Kubernetes LoadBalancer-dokumentation](https://kubernetes.io/docs/user-guide/load-balancer/)
* Lär dig mer om [Kubernetes meddelanden om ingångs- och Ingångsanspråk domänkontrollanter](https://kubernetes.io/docs/user-guide/ingress/)
* Se [Kubernetes exempel](https://github.com/kubernetes/kubernetes/tree/master/examples)

