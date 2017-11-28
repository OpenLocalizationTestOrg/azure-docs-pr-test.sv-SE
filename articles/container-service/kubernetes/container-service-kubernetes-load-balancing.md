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
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="8abf4-104">Läsa in saldo behållare i ett Kubernetes kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="8abf4-104">Load balance containers in a Kubernetes cluster in Azure Container Service</span></span> 
<span data-ttu-id="8abf4-105">Den här artikeln introducerar belastningsutjämning i ett Kubernetes kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="8abf4-105">This article introduces load balancing in a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="8abf4-106">Belastningsutjämning tillhandahåller en externt tillgänglig IP-adress för hello service och distribuerar trafik mellan hello skida körs i agenten virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8abf4-106">Load balancing provides an externally accessible IP address for hello service and distributes network traffic among hello pods running in agent VMs.</span></span>

<span data-ttu-id="8abf4-107">Du kan ställa in en Kubernetes service toouse [Azure belastningsutjämnare](../../load-balancer/load-balancer-overview.md) toomanage extern (TCP) nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="8abf4-107">You can set up a Kubernetes service toouse [Azure Load Balancer](../../load-balancer/load-balancer-overview.md) toomanage external network (TCP) traffic.</span></span> <span data-ttu-id="8abf4-108">Läsa in belastningsutjämning och routning av HTTP eller HTTPS-trafik eller mer avancerade scenarier är möjliga med ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8abf4-108">With additional configuration, load balancing and routing of HTTP or HTTPS traffic or more advanced scenarios are possible.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8abf4-109">Krav</span><span class="sxs-lookup"><span data-stu-id="8abf4-109">Prerequisites</span></span>
* <span data-ttu-id="8abf4-110">[Distribuera ett kluster Kubernetes](container-service-kubernetes-walkthrough.md) i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="8abf4-110">[Deploy a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>
* <span data-ttu-id="8abf4-111">[Ansluta klienten](../container-service-connect.md) tooyour kluster</span><span class="sxs-lookup"><span data-stu-id="8abf4-111">[Connect your client](../container-service-connect.md) tooyour cluster</span></span>

## <a name="azure-load-balancer"></a><span data-ttu-id="8abf4-112">Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="8abf4-112">Azure load balancer</span></span>

<span data-ttu-id="8abf4-113">Som standard innehåller ett Kubernetes kluster som distribueras i Azure Container Service en Azure belastningsutjämnare mot Internet för hello agent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8abf4-113">By default, a Kubernetes cluster deployed in Azure Container Service includes an Internet-facing Azure load balancer for hello agent VMs.</span></span> <span data-ttu-id="8abf4-114">(En separat belastningsutjämningsresurs har konfigurerats för hello master virtuella datorer.) Azure belastningsutjämnare är Layer 4 belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="8abf4-114">(A separate load balancer resource is configured for hello master VMs.) Azure load balancer is a Layer 4 load balancer.</span></span> <span data-ttu-id="8abf4-115">Hello belastningsutjämnaren stöder för närvarande endast TCP-trafik i Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="8abf4-115">Currently, hello load balancer only supports TCP traffic in Kubernetes.</span></span>

<span data-ttu-id="8abf4-116">När du skapar en Kubernetes-tjänst kan konfigurera du hello Azure belastningen belastningsutjämnaren tooallow toohello tjänsten automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8abf4-116">When creating a Kubernetes service, you can automatically configure hello Azure load balancer tooallow access toohello service.</span></span> <span data-ttu-id="8abf4-117">tooconfigure hello belastningsutjämnare, ange hello tjänst `type` för`LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-117">tooconfigure hello load balancer, set hello service `type` too`LoadBalancer`.</span></span> <span data-ttu-id="8abf4-118">hello belastningsutjämnaren skapar en regel toomap en offentlig IP-adress och portnummer för inkommande trafik toohello privata IP-adresser och portnummer för hello skida i agenten virtuella datorer (och vice versa för svarstrafik).</span><span class="sxs-lookup"><span data-stu-id="8abf4-118">hello load balancer creates a rule toomap a public IP address and port number of incoming service traffic toohello private IP addresses and port numbers of hello pods in agent VMs (and vice versa for response traffic).</span></span> 

 <span data-ttu-id="8abf4-119">Följande är två exempel visar hur tooset hello Kubernetes service `type` för`LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-119">Following are two examples showing how tooset hello Kubernetes service `type` too`LoadBalancer`.</span></span> <span data-ttu-id="8abf4-120">(När du försöker hello exempel, ta bort hello distributioner om du inte längre behöver dem..)</span><span class="sxs-lookup"><span data-stu-id="8abf4-120">(After trying hello examples, delete hello deployments if you no longer need them.)</span></span>

### <a name="example-use-hello-kubectl-expose-command"></a><span data-ttu-id="8abf4-121">Exempel: Använd hello `kubectl expose` kommando</span><span class="sxs-lookup"><span data-stu-id="8abf4-121">Example: Use hello `kubectl expose` command</span></span> 
<span data-ttu-id="8abf4-122">Hej [Kubernetes genomgången](container-service-kubernetes-walkthrough.md) innehåller ett exempel på hur tooexpose en tjänst med hello `kubectl expose` kommandot och dess `--type=LoadBalancer` flaggan.</span><span class="sxs-lookup"><span data-stu-id="8abf4-122">hello [Kubernetes walkthrough](container-service-kubernetes-walkthrough.md) includes an example of how tooexpose a service with hello `kubectl expose` command and its `--type=LoadBalancer` flag.</span></span> <span data-ttu-id="8abf4-123">Här är hello steg:</span><span class="sxs-lookup"><span data-stu-id="8abf4-123">Here are hello steps :</span></span>

1. <span data-ttu-id="8abf4-124">Starta en ny distribution i behållare.</span><span class="sxs-lookup"><span data-stu-id="8abf4-124">Start a new container deployment.</span></span> <span data-ttu-id="8abf4-125">Hej exempelvis följande kommando startar en ny distribution kallas `mynginx`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-125">For example, hello following command starts a new deployment called `mynginx`.</span></span> <span data-ttu-id="8abf4-126">hello-distribution består av tre behållare baserat på hello Docker bild för hello Nginx-webbserver.</span><span class="sxs-lookup"><span data-stu-id="8abf4-126">hello deployment consists of three containers based on hello Docker image for hello Nginx web server.</span></span>

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. <span data-ttu-id="8abf4-127">Kontrollera att hello behållare körs.</span><span class="sxs-lookup"><span data-stu-id="8abf4-127">Verify that hello containers are running.</span></span> <span data-ttu-id="8abf4-128">Om du fråga efter hello behållare med till exempel `kubectl get pods`, visas utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="8abf4-128">For example, if you query for hello containers with `kubectl get pods`, you see output similar toohello following:</span></span>

    ![Hämta Nginx-behållare](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. <span data-ttu-id="8abf4-130">tooconfigure hello belastningen belastningsutjämnaren tooaccept externa trafiken toohello distribution, kör `kubectl expose` med `--type=LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-130">tooconfigure hello load balancer tooaccept external traffic toohello deployment, run `kubectl expose` with `--type=LoadBalancer`.</span></span> <span data-ttu-id="8abf4-131">hello visar följande kommando hello Nginx-server på port 80:</span><span class="sxs-lookup"><span data-stu-id="8abf4-131">hello following command exposes hello Nginx server on port 80:</span></span>

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. <span data-ttu-id="8abf4-132">Typen `kubectl get svc` toosee hello tjänsttillstånd hello hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8abf4-132">Type `kubectl get svc` toosee hello state of hello services in hello cluster.</span></span> <span data-ttu-id="8abf4-133">Medan hello belastningsutjämnaren konfigurerar hello regeln, hello `EXTERNAL-IP` av hello tjänsten visas som `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-133">While hello load balancer configures hello rule, hello `EXTERNAL-IP` of hello service appears as `<pending>`.</span></span> <span data-ttu-id="8abf4-134">Efter några minuter konfigureras hello extern IP-adress:</span><span class="sxs-lookup"><span data-stu-id="8abf4-134">After a few minutes, hello external IP address is configured:</span></span> 

    ![Konfigurera Azure belastningsutjämnare](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. <span data-ttu-id="8abf4-136">Kontrollera att du kan komma åt hello-tjänsten på hello extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8abf4-136">Verify that you can access hello service at hello external IP address.</span></span> <span data-ttu-id="8abf4-137">Till exempel öppna en webbläsare toohello IP webbadress visas.</span><span class="sxs-lookup"><span data-stu-id="8abf4-137">For example, open a web browser toohello IP address shown.</span></span> <span data-ttu-id="8abf4-138">hello webbläsaren visar hello Nginx webbserver som körs i ett av hello behållare.</span><span class="sxs-lookup"><span data-stu-id="8abf4-138">hello browser shows hello Nginx web server running in one of hello containers.</span></span> <span data-ttu-id="8abf4-139">Eller kör hello `curl` eller `wget` kommando.</span><span class="sxs-lookup"><span data-stu-id="8abf4-139">Or, run hello `curl` or `wget` command.</span></span> <span data-ttu-id="8abf4-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8abf4-140">For example:</span></span>

    ```
    curl 13.82.93.130
    ```

    <span data-ttu-id="8abf4-141">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="8abf4-141">You should see output similar to:</span></span>

    ![Åtkomst Nginx med curl](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. <span data-ttu-id="8abf4-143">toosee hello konfigurationen av hello Azure belastningsutjämnare, gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8abf4-143">toosee hello configuration of hello Azure load balancer, go toohello [Azure portal](https://portal.azure.com).</span></span>

7. <span data-ttu-id="8abf4-144">Bläddra efter hello resursgrupp för ditt behållartjänstklustret och välj hello belastningsutjämnaren för hello agent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="8abf4-144">Browse for hello resource group for your container service cluster, and select hello load balancer for hello agent VMs.</span></span> <span data-ttu-id="8abf4-145">Namnet ska vara hello samma som hello behållartjänsten.</span><span class="sxs-lookup"><span data-stu-id="8abf4-145">Its name should be hello same as hello container service.</span></span> <span data-ttu-id="8abf4-146">(Inte välja hello belastningsutjämnaren för hello överordnade noder, hello en vars namn innehåller **master-lb**.)</span><span class="sxs-lookup"><span data-stu-id="8abf4-146">(Don't choose hello load balancer for hello master nodes, hello one whose name includes **master-lb**.)</span></span> 

    ![Belastningsutjämnare i resursgruppen.](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. <span data-ttu-id="8abf4-148">toosee hello information om konfiguration av belastningsutjämning hello, klickar du på **belastningsutjämningsregler** och hello namnet på hello-regel som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="8abf4-148">toosee hello details of hello load balancer configuration, click **Load balancing rules** and hello name of hello rule that was configured.</span></span>

    ![Belastningsutjämningsreglerna](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-hello-service-configuration-file"></a><span data-ttu-id="8abf4-150">Exempel: Ange `type: LoadBalancer` i konfigurationsfilen för hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="8abf4-150">Example: Specify `type: LoadBalancer` in hello service configuration file</span></span>

<span data-ttu-id="8abf4-151">Om du distribuerar en Kubernetes behållare app från en YAML eller JSON [tjänstkonfigurationsfilen](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), ange en extern belastningsutjämnare genom att lägga till hello följande rad toohello service specifikationen:</span><span class="sxs-lookup"><span data-stu-id="8abf4-151">If you deploy a Kubernetes container app from a YAML or JSON [service configuration file](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), specify an external load balancer by adding hello following line toohello service specification:</span></span>

```YAML
 "type": "LoadBalancer"
``` 



<span data-ttu-id="8abf4-152">hello följande använda hello Kubernetes [gästbok exempel](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook).</span><span class="sxs-lookup"><span data-stu-id="8abf4-152">hello following steps use hello Kubernetes [Guestbook example](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook).</span></span> <span data-ttu-id="8abf4-153">Det här exemplet är ett webbprogram med flera nivåer baserat på Redis- och PHP Docker-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="8abf4-153">This example is a multi-tier web app based on  Redis and PHP Docker images.</span></span> <span data-ttu-id="8abf4-154">Du kan ange i hello tjänstkonfigurationsfilen hello klientdel PHP servern använder hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="8abf4-154">You can specify in hello service configuration file that hello frontend PHP server uses hello Azure load balancer.</span></span>

1. <span data-ttu-id="8abf4-155">Hämta hello filen `guestbook-all-in-one.yaml` från [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one).</span><span class="sxs-lookup"><span data-stu-id="8abf4-155">Download hello file `guestbook-all-in-one.yaml` from [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one).</span></span> 
2. <span data-ttu-id="8abf4-156">Bläddra efter hello `spec` för hello `frontend` service.</span><span class="sxs-lookup"><span data-stu-id="8abf4-156">Browse for hello `spec` for hello `frontend` service.</span></span>
3. <span data-ttu-id="8abf4-157">Ta bort kommentarerna hello rad `type: LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-157">Uncomment hello line `type: LoadBalancer`.</span></span>

    ![Belastningsfördelning i tjänstekonfigurationen](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. <span data-ttu-id="8abf4-159">Spara filen med hello och distribuera hello app genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8abf4-159">Save hello file, and deploy hello app by running hello following command:</span></span>

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. <span data-ttu-id="8abf4-160">Typen `kubectl get svc` toosee hello tjänsttillstånd hello hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8abf4-160">Type `kubectl get svc` toosee hello state of hello services in hello cluster.</span></span> <span data-ttu-id="8abf4-161">Medan hello belastningsutjämnaren konfigurerar hello regeln, hello `EXTERNAL-IP` av hello `frontend` tjänsten visas som `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-161">While hello load balancer configures hello rule, hello `EXTERNAL-IP` of hello `frontend` service appears as `<pending>`.</span></span> <span data-ttu-id="8abf4-162">Efter några minuter konfigureras hello extern IP-adress:</span><span class="sxs-lookup"><span data-stu-id="8abf4-162">After a few minutes, hello external IP address is configured:</span></span> 

    ![Konfigurera Azure belastningsutjämnare](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. <span data-ttu-id="8abf4-164">Kontrollera att du kan komma åt hello-tjänsten på hello extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8abf4-164">Verify that you can access hello service at hello external IP address.</span></span> <span data-ttu-id="8abf4-165">Du kan exempelvis öppna en web webbläsare toohello externa IP-adress hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8abf4-165">For example, you can open a web browser toohello external IP address of hello service.</span></span>

    ![Åtkomst gästbok externt](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    <span data-ttu-id="8abf4-167">Du kan lägga till gästbok poster.</span><span class="sxs-lookup"><span data-stu-id="8abf4-167">You can add guestbook entries.</span></span>

7. <span data-ttu-id="8abf4-168">toosee hello konfigurationen av hello Azure belastningsutjämnare, bläddra efter hello belastningsutjämningsresurs för hello-kluster i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8abf4-168">toosee hello configuration of hello Azure load balancer, browse for hello load balancer resource for hello cluster in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8abf4-169">Se hello stegen i föregående exempel för hello.</span><span class="sxs-lookup"><span data-stu-id="8abf4-169">See hello steps in hello previous example.</span></span>

### <a name="considerations"></a><span data-ttu-id="8abf4-170">Överväganden</span><span class="sxs-lookup"><span data-stu-id="8abf4-170">Considerations</span></span>

* <span data-ttu-id="8abf4-171">Skapa regel för belastningsutjämnare hello sker asynkront och information om hello etablerats belastningsutjämnaren har publicerats i hello service `status.loadBalancer` fältet.</span><span class="sxs-lookup"><span data-stu-id="8abf4-171">Creation of hello load balancer rule happens asynchronously, and information about hello provisioned balancer is published in hello service’s `status.loadBalancer` field.</span></span>
* <span data-ttu-id="8abf4-172">Varje tjänst tilldelas automatiskt sin egen virtuella IP-adressen i hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="8abf4-172">Every service is automatically assigned its own virtual IP address in hello load balancer.</span></span>
* <span data-ttu-id="8abf4-173">Om du vill tooreach hello belastningsutjämnaren med ett DNS-namn, arbeta med din domän service provider toocreate ett DNS-namn för hello regeln IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8abf4-173">If you want tooreach hello load balancer by a DNS name, work with your domain service provider toocreate a DNS name for hello rule's IP address.</span></span>

## <a name="http-or-https-traffic"></a><span data-ttu-id="8abf4-174">HTTP eller HTTPS-trafik</span><span class="sxs-lookup"><span data-stu-id="8abf4-174">HTTP or HTTPS traffic</span></span>

<span data-ttu-id="8abf4-175">tooload saldo HTTP eller HTTPS-trafik toocontainer webbappar och hantera certifikat för transport layer security (TLS), kan du använda hello Kubernetes [ingång](https://kubernetes.io/docs/user-guide/ingress/) resurs.</span><span class="sxs-lookup"><span data-stu-id="8abf4-175">tooload balance HTTP or HTTPS traffic toocontainer web apps and manage certificates for transport layer security (TLS), you can use hello Kubernetes [Ingress](https://kubernetes.io/docs/user-guide/ingress/) resource.</span></span> <span data-ttu-id="8abf4-176">Ett meddelande om ingångs är en uppsättning regler som tillåter inkommande anslutningar tooreach hello klustertjänsterna.</span><span class="sxs-lookup"><span data-stu-id="8abf4-176">An Ingress is a collection of rules that allow inbound connections tooreach hello cluster services.</span></span> <span data-ttu-id="8abf4-177">För en resurs toowork ingång hello Kubernetes klustret måste ha en [ingång controller](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) körs.</span><span class="sxs-lookup"><span data-stu-id="8abf4-177">For an Ingress resource toowork, hello Kubernetes cluster must have an [Ingress controller](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) running.</span></span>

<span data-ttu-id="8abf4-178">Azure Container Service implementerar inte en Kubernetes ingång domänkontrollant automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8abf4-178">Azure Container Service does not implement a Kubernetes Ingress controller automatically.</span></span> <span data-ttu-id="8abf4-179">Det finns flera implementeringar av domänkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="8abf4-179">Several controller implementations are available.</span></span> <span data-ttu-id="8abf4-180">För närvarande hello [Nginx ingång controller](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) rekommenderas tooconfigure ingång regler och belastningsutjämning HTTP och HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="8abf4-180">Currently, hello [Nginx Ingress controller](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) is recommended tooconfigure Ingress rules and load balance HTTP and HTTPS traffic.</span></span> 

<span data-ttu-id="8abf4-181">Mer information finns i hello [Nginx ingång controller dokumentationen](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).</span><span class="sxs-lookup"><span data-stu-id="8abf4-181">For more information, see hello [Nginx Ingress controller documentation](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8abf4-182">När du använder hello Nginx ingång domänkontrollant i Azure Container Service kan du visa hello distributionen av domänkontrollanter som en tjänst med `type: LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="8abf4-182">When using hello Nginx Ingress controller in Azure Container Service, you must expose hello controller deployment as a service with `type: LoadBalancer`.</span></span> <span data-ttu-id="8abf4-183">Detta konfigurerar hello Azure belastningen belastningsutjämnaren tooroute trafik toohello domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="8abf4-183">This configures hello Azure load balancer tooroute traffic toohello controller.</span></span> <span data-ttu-id="8abf4-184">Mer information finns i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8abf4-184">For more information, see hello previous section.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8abf4-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8abf4-185">Next steps</span></span>

* <span data-ttu-id="8abf4-186">Se hello [Kubernetes LoadBalancer-dokumentation](https://kubernetes.io/docs/user-guide/load-balancer/)</span><span class="sxs-lookup"><span data-stu-id="8abf4-186">See hello [Kubernetes LoadBalancer documentation](https://kubernetes.io/docs/user-guide/load-balancer/)</span></span>
* <span data-ttu-id="8abf4-187">Lär dig mer om [Kubernetes meddelanden om ingångs- och Ingångsanspråk domänkontrollanter](https://kubernetes.io/docs/user-guide/ingress/)</span><span class="sxs-lookup"><span data-stu-id="8abf4-187">Learn more about [Kubernetes Ingress and Ingress controllers](https://kubernetes.io/docs/user-guide/ingress/)</span></span>
* <span data-ttu-id="8abf4-188">Se [Kubernetes exempel](https://github.com/kubernetes/kubernetes/tree/master/examples)</span><span class="sxs-lookup"><span data-stu-id="8abf4-188">See [Kubernetes examples](https://github.com/kubernetes/kubernetes/tree/master/examples)</span></span>

