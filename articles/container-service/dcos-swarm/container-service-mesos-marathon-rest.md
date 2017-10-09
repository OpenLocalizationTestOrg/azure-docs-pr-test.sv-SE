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
# <a name="dcos-container-management-through-hello-marathon-rest-api"></a><span data-ttu-id="69b50-104">DC/OS-hantering av behållare via hello Marathon REST API</span><span class="sxs-lookup"><span data-stu-id="69b50-104">DC/OS container management through hello Marathon REST API</span></span>
<span data-ttu-id="69b50-105">DC/OS erbjuder en miljö för att distribuera och skala klustrade arbetsbelastningar samtidigt abstrahera hello underliggande maskinvara.</span><span class="sxs-lookup"><span data-stu-id="69b50-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting hello underlying hardware.</span></span> <span data-ttu-id="69b50-106">Utöver DC/OS finns det ett ramverk som hanterar schemaläggning och beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="69b50-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span> <span data-ttu-id="69b50-107">Även om ramverk är tillgängliga för många populära arbetsbelastningar, hjälper det här dokumentet dig att börja skapa och skala distribution i behållare med hjälp av hello Marathon REST API.</span><span class="sxs-lookup"><span data-stu-id="69b50-107">Although frameworks are available for many popular workloads, this document gets you started creating and scaling container deployments by using hello Marathon REST API.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="69b50-108">Krav</span><span class="sxs-lookup"><span data-stu-id="69b50-108">Prerequisites</span></span>

<span data-ttu-id="69b50-109">Innan du börjar med de här exemplen behöver du ett DC/OS-kluster som har konfigurerats i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="69b50-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="69b50-110">Du måste också toohave fjärranslutningar toothis klustret.</span><span class="sxs-lookup"><span data-stu-id="69b50-110">You also need toohave remote connectivity toothis cluster.</span></span> <span data-ttu-id="69b50-111">Mer information om dessa objekt finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="69b50-111">For more information on these items, see hello following articles:</span></span>

* [<span data-ttu-id="69b50-112">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="69b50-112">Deploying an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="69b50-113">Ansluta tooan Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="69b50-113">Connecting tooan Azure Container Service cluster</span></span>](../container-service-connect.md)

## <a name="access-hello-dcos-apis"></a><span data-ttu-id="69b50-114">Komma åt hello DC/OS-API: er</span><span class="sxs-lookup"><span data-stu-id="69b50-114">Access hello DC/OS APIs</span></span>
<span data-ttu-id="69b50-115">När du är ansluten toohello Azure Container Service-kluster kan du komma åt hello DC/OS och relaterade REST API: er via http://localhost: Local-port.</span><span class="sxs-lookup"><span data-stu-id="69b50-115">After you are connected toohello Azure Container Service cluster, you can access hello DC/OS and related REST APIs through http://localhost:local-port.</span></span> <span data-ttu-id="69b50-116">hello exemplen i det här dokumentet förutsätter att du använder tunneltrafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="69b50-116">hello examples in this document assume that you are tunneling on port 80.</span></span> <span data-ttu-id="69b50-117">Till exempel hello Marathon slutpunkter kan nås på URI: er från och med `http://localhost/marathon/v2/`.</span><span class="sxs-lookup"><span data-stu-id="69b50-117">For example, hello Marathon endpoints can be reached at URIs beginning with `http://localhost/marathon/v2/`.</span></span> 

<span data-ttu-id="69b50-118">Mer information om hello olika API: er, se hello Mesosphere-dokumentationen för hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) och [Chronos API](https://mesos.github.io/chronos/docs/api.html), samt Apache-dokumentationen för hello [Mesos Scheduler API ](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span><span class="sxs-lookup"><span data-stu-id="69b50-118">For more information on hello various APIs, see hello Mesosphere documentation for hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) and the [Chronos API](https://mesos.github.io/chronos/docs/api.html), and the Apache documentation for hello [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span></span>

## <a name="gather-information-from-dcos-and-marathon"></a><span data-ttu-id="69b50-119">Samla in information från DC/OS och Marathon</span><span class="sxs-lookup"><span data-stu-id="69b50-119">Gather information from DC/OS and Marathon</span></span>
<span data-ttu-id="69b50-120">Innan du distribuerar behållare toohello DC/OS-klustret måste du samla in information om hello DC/OS-klustret, till exempel hello namn och statusen för hello DC/OS-agenterna.</span><span class="sxs-lookup"><span data-stu-id="69b50-120">Before you deploy containers toohello DC/OS cluster, gather some information about hello DC/OS cluster, such as hello names and status of hello DC/OS agents.</span></span> <span data-ttu-id="69b50-121">toodo fråga så hello `master/slaves` slutpunkten för hello DC/OS REST API.</span><span class="sxs-lookup"><span data-stu-id="69b50-121">toodo so, query hello `master/slaves` endpoint of hello DC/OS REST API.</span></span> <span data-ttu-id="69b50-122">Om allt går bra returnerar hello frågan en lista över DC/OS-agenterna och flera egenskaper för varje.</span><span class="sxs-lookup"><span data-stu-id="69b50-122">If everything goes well, hello query returns a list of DC/OS agents and several properties for each.</span></span>

```bash
curl http://localhost/mesos/master/slaves
```

<span data-ttu-id="69b50-123">Nu kan använda hello Marathon `/apps` endpoint toocheck för aktuella program distributioner toohello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="69b50-123">Now, use hello Marathon `/apps` endpoint toocheck for current application deployments toohello DC/OS cluster.</span></span> <span data-ttu-id="69b50-124">Om det här är ett nytt kluster visas en tom matris för appar.</span><span class="sxs-lookup"><span data-stu-id="69b50-124">If this is a new cluster, you see an empty array for apps.</span></span>

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="69b50-125">Distribuera en Docker-formaterad behållare</span><span class="sxs-lookup"><span data-stu-id="69b50-125">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="69b50-126">Du kan distribuera Docker-formaterade behållare via hello Marathon REST API med hjälp av en JSON-fil som beskriver hello avsedda distributionen.</span><span class="sxs-lookup"><span data-stu-id="69b50-126">You deploy Docker-formatted containers through hello Marathon REST API by using a JSON file that describes hello intended deployment.</span></span> <span data-ttu-id="69b50-127">hello distribueras följande exempel Nginx-behållaren tooa privata agent i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="69b50-127">hello following sample deploys an Nginx container tooa private agent in hello cluster.</span></span> 

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

<span data-ttu-id="69b50-128">toodeploy Docker-formaterad behållare lagra hello JSON-fil i en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="69b50-128">toodeploy a Docker-formatted container, store hello JSON file in an accessible location.</span></span> <span data-ttu-id="69b50-129">Nästa toodeploy hello behållare, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="69b50-129">Next, toodeploy hello container, run hello following command.</span></span> <span data-ttu-id="69b50-130">Ange hello namn hello JSON-fil (`marathon.json` i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="69b50-130">Specify hello name of hello JSON file (`marathon.json` in this example).</span></span>

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

<span data-ttu-id="69b50-131">hello-utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="69b50-131">hello output is similar toohello following:</span></span>

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

<span data-ttu-id="69b50-132">Om du frågar Marathon efter program nu visas den här nya programmet i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="69b50-132">Now, if you query Marathon for applications, this new application appears in hello output.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-hello-container"></a><span data-ttu-id="69b50-133">Nå hello behållare</span><span class="sxs-lookup"><span data-stu-id="69b50-133">Reach hello container</span></span>

<span data-ttu-id="69b50-134">Du kan kontrollera att hello Nginx körs i en behållare på en av hello privata agenter i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="69b50-134">You can verify that hello Nginx is running in a container on one of hello private agents in hello cluster.</span></span> <span data-ttu-id="69b50-135">toofind hello-värd och port där hello behållare körs frågar Marathon efter hello pågående aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="69b50-135">toofind hello host and port where hello container is running, query Marathon for hello running tasks:</span></span> 

```bash
curl localhost/marathon/v2/tasks
```

<span data-ttu-id="69b50-136">Hitta hello värdet för `host` i hello utdata (en IP-adress för liknande`10.32.0.x`), och hello värdet för `ports`.</span><span class="sxs-lookup"><span data-stu-id="69b50-136">Find hello value of `host` in hello output (an IP address similar too`10.32.0.x`), and hello value of `ports`.</span></span>


<span data-ttu-id="69b50-137">Nu göra en SSH terminal (inte en tunnel anslutning) toohello anslutningshanteringen FQDN hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="69b50-137">Now make an SSH terminal connection (not a tunneled connection) toohello management FQDN of hello cluster.</span></span> <span data-ttu-id="69b50-138">När du är ansluten, att Hej på begäran, ersätter hello korrekta värden för `host` och `ports`:</span><span class="sxs-lookup"><span data-stu-id="69b50-138">Once connected, make hello following request, substituting hello correct values of `host` and `ports`:</span></span>

```bash
curl http://host:ports
```

<span data-ttu-id="69b50-139">Hej Nginx server-utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="69b50-139">hello Nginx server output is similar toohello following:</span></span>

![Nginx från behållaren](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a><span data-ttu-id="69b50-141">Skala behållarna</span><span class="sxs-lookup"><span data-stu-id="69b50-141">Scale your containers</span></span>
<span data-ttu-id="69b50-142">Du kan använda hello Marathon API tooscale ut eller skala in programdistributioner.</span><span class="sxs-lookup"><span data-stu-id="69b50-142">You can use hello Marathon API tooscale out or scale in application deployments.</span></span> <span data-ttu-id="69b50-143">Hello föregående exempel distribuerade du en instans av ett program.</span><span class="sxs-lookup"><span data-stu-id="69b50-143">In hello previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="69b50-144">Nu kan vi skala detta ut toothree instanser av ett program.</span><span class="sxs-lookup"><span data-stu-id="69b50-144">Let's scale this out toothree instances of an application.</span></span> <span data-ttu-id="69b50-145">toodo så, skapa en JSON-fil med hjälp av hello följande JSON-text och lagra den på en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="69b50-145">toodo so, create a JSON file by using hello following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="69b50-146">Kör följande kommando tooscale ut hello programmet hello från anslutningens tunnel.</span><span class="sxs-lookup"><span data-stu-id="69b50-146">From your tunneled connection, run hello following command tooscale out hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="69b50-147">hello URI är http://localhost/marathon/v2/apps/ följt av hello-ID för hello programmet tooscale.</span><span class="sxs-lookup"><span data-stu-id="69b50-147">hello URI is http://localhost/marathon/v2/apps/ followed by hello ID of hello application tooscale.</span></span> <span data-ttu-id="69b50-148">Om du använder hello Nginx-exemplet som anges här, är hello URI: N http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="69b50-148">If you are using hello Nginx sample that is provided here, hello URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

<span data-ttu-id="69b50-149">Fråga slutligen hello Marathon-slutpunkten för program.</span><span class="sxs-lookup"><span data-stu-id="69b50-149">Finally, query hello Marathon endpoint for applications.</span></span> <span data-ttu-id="69b50-150">Du ser nu att det finns tre Nginx-behållare.</span><span class="sxs-lookup"><span data-stu-id="69b50-150">You see that there are now three Nginx containers.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a><span data-ttu-id="69b50-151">Motsvarande PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="69b50-151">Equivalent PowerShell commands</span></span>
<span data-ttu-id="69b50-152">Du kan utföra samma åtgärder genom att använda PowerShell-kommandon på en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="69b50-152">You can perform these same actions by using PowerShell commands on a Windows system.</span></span>

<span data-ttu-id="69b50-153">toogather information om hello DC/OS-klustret, t.ex agentnamn och agentstatus kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="69b50-153">toogather information about hello DC/OS cluster, such as agent names and agent status, run hello following command:</span></span>

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

<span data-ttu-id="69b50-154">Du kan distribuera Docker-formaterad behållare via Marathon genom att använda en JSON-fil som beskriver hello avsedda distributionen.</span><span class="sxs-lookup"><span data-stu-id="69b50-154">You deploy Docker-formatted containers through Marathon by using a JSON file that describes hello intended deployment.</span></span> <span data-ttu-id="69b50-155">hello distribuerar följande exempel hello Nginx-behållaren, bindning hello DC/OS-agenten tooport 80 hello behållarens port 80.</span><span class="sxs-lookup"><span data-stu-id="69b50-155">hello following sample deploys hello Nginx container, binding port 80 of hello DC/OS agent tooport 80 of hello container.</span></span>

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

<span data-ttu-id="69b50-156">toodeploy Docker-formaterad behållare lagra hello JSON-fil i en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="69b50-156">toodeploy a Docker-formatted container, store hello JSON file in an accessible location.</span></span> <span data-ttu-id="69b50-157">Nästa toodeploy hello behållare, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="69b50-157">Next, toodeploy hello container, run hello following command.</span></span> <span data-ttu-id="69b50-158">Ange hello sökvägen toohello JSON-fil (`marathon.json` i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="69b50-158">Specify hello path toohello JSON file (`marathon.json` in this example).</span></span>

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

<span data-ttu-id="69b50-159">Du kan också använda hello Marathon API tooscale ut eller skala in programdistributioner.</span><span class="sxs-lookup"><span data-stu-id="69b50-159">You can also use hello Marathon API tooscale out or scale in application deployments.</span></span> <span data-ttu-id="69b50-160">Hello föregående exempel distribuerade du en instans av ett program.</span><span class="sxs-lookup"><span data-stu-id="69b50-160">In hello previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="69b50-161">Nu kan vi skala detta ut toothree instanser av ett program.</span><span class="sxs-lookup"><span data-stu-id="69b50-161">Let's scale this out toothree instances of an application.</span></span> <span data-ttu-id="69b50-162">toodo så, skapa en JSON-fil med hjälp av hello följande JSON-text och lagra den på en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="69b50-162">toodo so, create a JSON file by using hello following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="69b50-163">Kör följande kommando tooscale ut hello programmet hello:</span><span class="sxs-lookup"><span data-stu-id="69b50-163">Run hello following command tooscale out hello application:</span></span>

> [!NOTE]
> <span data-ttu-id="69b50-164">hello URI är http://localhost/marathon/v2/apps/ följt av hello-ID för hello programmet tooscale.</span><span class="sxs-lookup"><span data-stu-id="69b50-164">hello URI is http://localhost/marathon/v2/apps/ followed by hello ID of hello application tooscale.</span></span> <span data-ttu-id="69b50-165">Om du använder hello Nginx-exemplet som anges här, är hello URI: N http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="69b50-165">If you are using hello Nginx sample provided here, hello URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a><span data-ttu-id="69b50-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69b50-166">Next steps</span></span>
* [<span data-ttu-id="69b50-167">Läs mer om hello Mesos HTTP-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="69b50-167">Read more about hello Mesos HTTP endpoints</span></span>](http://mesos.apache.org/documentation/latest/endpoints/)
* [<span data-ttu-id="69b50-168">Läs mer om hello Marathon REST API</span><span class="sxs-lookup"><span data-stu-id="69b50-168">Read more about hello Marathon REST API</span></span>](https://mesosphere.github.io/marathon/docs/rest-api.html)

