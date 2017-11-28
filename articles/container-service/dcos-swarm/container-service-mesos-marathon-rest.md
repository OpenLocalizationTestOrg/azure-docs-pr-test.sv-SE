---
title: Hantera Azure DC/OS-kluster med Marathon REST API | Microsoft Docs
description: "Distribuera behållare till ett Azure Container Service DC/OS-kluster med Marathon REST API."
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
ms.openlocfilehash: 65f8e0170fa7b89162e811a1d5dd58775fd20d7b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="dcos-container-management-through-the-marathon-rest-api"></a><span data-ttu-id="a97a9-104">DC/OS-hantering av behållare via Marathon REST API</span><span class="sxs-lookup"><span data-stu-id="a97a9-104">DC/OS container management through the Marathon REST API</span></span>
<span data-ttu-id="a97a9-105">DC/OS erbjuder en miljö för att distribuera och skala klustrade arbetsbelastningar samtidigt som den underliggande maskinvaran abstraheras.</span><span class="sxs-lookup"><span data-stu-id="a97a9-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware.</span></span> <span data-ttu-id="a97a9-106">Utöver DC/OS finns det ett ramverk som hanterar schemaläggning och beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="a97a9-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span> <span data-ttu-id="a97a9-107">Även om ramverk är tillgängliga för många populära arbetsbelastningar, hjälper det här dokumentet dig att börja skapa och skala distribution i behållare med Marathon REST API.</span><span class="sxs-lookup"><span data-stu-id="a97a9-107">Although frameworks are available for many popular workloads, this document gets you started creating and scaling container deployments by using the Marathon REST API.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a97a9-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a97a9-108">Prerequisites</span></span>

<span data-ttu-id="a97a9-109">Innan du börjar med de här exemplen behöver du ett DC/OS-kluster som har konfigurerats i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="a97a9-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="a97a9-110">Du måste också kunna fjärransluta till det här klustret.</span><span class="sxs-lookup"><span data-stu-id="a97a9-110">You also need to have remote connectivity to this cluster.</span></span> <span data-ttu-id="a97a9-111">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="a97a9-111">For more information on these items, see the following articles:</span></span>

* [<span data-ttu-id="a97a9-112">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="a97a9-112">Deploying an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="a97a9-113">Ansluta till ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="a97a9-113">Connecting to an Azure Container Service cluster</span></span>](../container-service-connect.md)

## <a name="access-the-dcos-apis"></a><span data-ttu-id="a97a9-114">DC/OS-API: er</span><span class="sxs-lookup"><span data-stu-id="a97a9-114">Access the DC/OS APIs</span></span>
<span data-ttu-id="a97a9-115">När du har anslutit till Azure Container Service-klustret kan du komma åt DC/OS och relaterade REST API:er via http://localhost:local-port.</span><span class="sxs-lookup"><span data-stu-id="a97a9-115">After you are connected to the Azure Container Service cluster, you can access the DC/OS and related REST APIs through http://localhost:local-port.</span></span> <span data-ttu-id="a97a9-116">Exemplen i det här dokumentet förutsätter att du använder tunneltrafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="a97a9-116">The examples in this document assume that you are tunneling on port 80.</span></span> <span data-ttu-id="a97a9-117">Till exempel Marathon-slutpunkter kan nås på URI: er från och med `http://localhost/marathon/v2/`.</span><span class="sxs-lookup"><span data-stu-id="a97a9-117">For example, the Marathon endpoints can be reached at URIs beginning with `http://localhost/marathon/v2/`.</span></span> 

<span data-ttu-id="a97a9-118">Mer information om de olika API:erna finns i Mesosphere-dokumentationen för [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) och [Chronos API](https://mesos.github.io/chronos/docs/api.html), samt Apache-dokumentationen för [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span><span class="sxs-lookup"><span data-stu-id="a97a9-118">For more information on the various APIs, see the Mesosphere documentation for the [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) and the [Chronos API](https://mesos.github.io/chronos/docs/api.html), and the Apache documentation for the [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).</span></span>

## <a name="gather-information-from-dcos-and-marathon"></a><span data-ttu-id="a97a9-119">Samla in information från DC/OS och Marathon</span><span class="sxs-lookup"><span data-stu-id="a97a9-119">Gather information from DC/OS and Marathon</span></span>
<span data-ttu-id="a97a9-120">Samla in information om DC/OS-klustret, till exempel namn och status för DC/OS-agenter innan du distribuerar behållare i DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="a97a9-120">Before you deploy containers to the DC/OS cluster, gather some information about the DC/OS cluster, such as the names and status of the DC/OS agents.</span></span> <span data-ttu-id="a97a9-121">Det gör du genom att fråga `master/slaves`-slutpunkten för DC/OS REST API.</span><span class="sxs-lookup"><span data-stu-id="a97a9-121">To do so, query the `master/slaves` endpoint of the DC/OS REST API.</span></span> <span data-ttu-id="a97a9-122">Om allt går bra returnerar frågan en lista över DC/OS-agenter och flera egenskaper för varje agent.</span><span class="sxs-lookup"><span data-stu-id="a97a9-122">If everything goes well, the query returns a list of DC/OS agents and several properties for each.</span></span>

```bash
curl http://localhost/mesos/master/slaves
```

<span data-ttu-id="a97a9-123">Nu kan du använda Marathon-slutpunkten `/apps` för att kontrollera om det finns aktuella programdistributioner i DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="a97a9-123">Now, use the Marathon `/apps` endpoint to check for current application deployments to the DC/OS cluster.</span></span> <span data-ttu-id="a97a9-124">Om det här är ett nytt kluster visas en tom matris för appar.</span><span class="sxs-lookup"><span data-stu-id="a97a9-124">If this is a new cluster, you see an empty array for apps.</span></span>

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="a97a9-125">Distribuera en Docker-formaterad behållare</span><span class="sxs-lookup"><span data-stu-id="a97a9-125">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="a97a9-126">Du kan distribuera Docker-formaterade behållare via Marathon REST API med hjälp av en JSON-fil som beskriver den avsedda distributionen.</span><span class="sxs-lookup"><span data-stu-id="a97a9-126">You deploy Docker-formatted containers through the Marathon REST API by using a JSON file that describes the intended deployment.</span></span> <span data-ttu-id="a97a9-127">I följande exempel distribueras Nginx-behållaren till en privat agent i klustret.</span><span class="sxs-lookup"><span data-stu-id="a97a9-127">The following sample deploys an Nginx container to a private agent in the cluster.</span></span> 

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

<span data-ttu-id="a97a9-128">Distribuera en Docker-formaterad behållare genom att lagra JSON-filen på en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="a97a9-128">To deploy a Docker-formatted container, store the JSON file in an accessible location.</span></span> <span data-ttu-id="a97a9-129">Kör följande kommando för att distribuera behållaren.</span><span class="sxs-lookup"><span data-stu-id="a97a9-129">Next, to deploy the container, run the following command.</span></span> <span data-ttu-id="a97a9-130">Ange namnet på JSON-fil (`marathon.json` i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="a97a9-130">Specify the name of the JSON file (`marathon.json` in this example).</span></span>

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

<span data-ttu-id="a97a9-131">De utdata som genereras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="a97a9-131">The output is similar to the following:</span></span>

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

<span data-ttu-id="a97a9-132">Om du nu frågar Marathon efter program visas det här nya programmet i resultaten.</span><span class="sxs-lookup"><span data-stu-id="a97a9-132">Now, if you query Marathon for applications, this new application appears in the output.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-the-container"></a><span data-ttu-id="a97a9-133">Nå behållaren</span><span class="sxs-lookup"><span data-stu-id="a97a9-133">Reach the container</span></span>

<span data-ttu-id="a97a9-134">Du kan kontrollera att Nginx körs i en behållare på en av de privata agenterna i klustret.</span><span class="sxs-lookup"><span data-stu-id="a97a9-134">You can verify that the Nginx is running in a container on one of the private agents in the cluster.</span></span> <span data-ttu-id="a97a9-135">Fråga Marathon efter aktiviteter som körs för att hitta värden och den port där behållaren körs:</span><span class="sxs-lookup"><span data-stu-id="a97a9-135">To find the host and port where the container is running, query Marathon for the running tasks:</span></span> 

```bash
curl localhost/marathon/v2/tasks
```

<span data-ttu-id="a97a9-136">Hitta värdet för `host` i utdata (en IP-adress som liknar `10.32.0.x`), och värdet för `ports`.</span><span class="sxs-lookup"><span data-stu-id="a97a9-136">Find the value of `host` in the output (an IP address similar to `10.32.0.x`), and the value of `ports`.</span></span>


<span data-ttu-id="a97a9-137">Nu göra en terminal SSH-anslutning (inte en tunnel anslutning) för management FQDN i klustret.</span><span class="sxs-lookup"><span data-stu-id="a97a9-137">Now make an SSH terminal connection (not a tunneled connection) to the management FQDN of the cluster.</span></span> <span data-ttu-id="a97a9-138">När du är ansluten, se följande begäran, ersätter de korrekta värdena för `host` och `ports`:</span><span class="sxs-lookup"><span data-stu-id="a97a9-138">Once connected, make the following request, substituting the correct values of `host` and `ports`:</span></span>

```bash
curl http://host:ports
```

<span data-ttu-id="a97a9-139">Nginx server-utdata liknar följande:</span><span class="sxs-lookup"><span data-stu-id="a97a9-139">The Nginx server output is similar to the following:</span></span>

![Nginx från behållaren](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a><span data-ttu-id="a97a9-141">Skala behållarna</span><span class="sxs-lookup"><span data-stu-id="a97a9-141">Scale your containers</span></span>
<span data-ttu-id="a97a9-142">Du kan använda Marathon API för att skala ut eller skala in programdistributioner.</span><span class="sxs-lookup"><span data-stu-id="a97a9-142">You can use the Marathon API to scale out or scale in application deployments.</span></span> <span data-ttu-id="a97a9-143">I föregående exempel distribuerade du en instans av ett program.</span><span class="sxs-lookup"><span data-stu-id="a97a9-143">In the previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="a97a9-144">Nu kan vi skala ut det här till tre instanser av ett program.</span><span class="sxs-lookup"><span data-stu-id="a97a9-144">Let's scale this out to three instances of an application.</span></span> <span data-ttu-id="a97a9-145">Det gör du genom att skapa en JSON-fil med nedanstående JSON-text och lagra den på en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="a97a9-145">To do so, create a JSON file by using the following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="a97a9-146">Kör följande kommando för att skala ut programmet från anslutningens tunnel.</span><span class="sxs-lookup"><span data-stu-id="a97a9-146">From your tunneled connection, run the following command to scale out the application.</span></span>

> [!NOTE]
> <span data-ttu-id="a97a9-147">URI:n är http://localhost/marathon/v2/apps/ och följs av ID:t för programmet som ska skalas.</span><span class="sxs-lookup"><span data-stu-id="a97a9-147">The URI is http://localhost/marathon/v2/apps/ followed by the ID of the application to scale.</span></span> <span data-ttu-id="a97a9-148">Om du använder Nginx-exemplet som anges här blir URI:n http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="a97a9-148">If you are using the Nginx sample that is provided here, the URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

<span data-ttu-id="a97a9-149">Fråga slutligen Marathon-slutpunkten efter program.</span><span class="sxs-lookup"><span data-stu-id="a97a9-149">Finally, query the Marathon endpoint for applications.</span></span> <span data-ttu-id="a97a9-150">Du ser nu att det finns tre Nginx-behållare.</span><span class="sxs-lookup"><span data-stu-id="a97a9-150">You see that there are now three Nginx containers.</span></span>

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a><span data-ttu-id="a97a9-151">Motsvarande PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="a97a9-151">Equivalent PowerShell commands</span></span>
<span data-ttu-id="a97a9-152">Du kan utföra samma åtgärder genom att använda PowerShell-kommandon på en Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="a97a9-152">You can perform these same actions by using PowerShell commands on a Windows system.</span></span>

<span data-ttu-id="a97a9-153">Kör följande kommando för att samla in information om DC/OS-klustret, t.ex agentnamn och agentstatus:</span><span class="sxs-lookup"><span data-stu-id="a97a9-153">To gather information about the DC/OS cluster, such as agent names and agent status, run the following command:</span></span>

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

<span data-ttu-id="a97a9-154">Du kan distribuera Docker-formaterade behållare via Marathon genom att använda en JSON-fil som beskriver den avsedda distributionen.</span><span class="sxs-lookup"><span data-stu-id="a97a9-154">You deploy Docker-formatted containers through Marathon by using a JSON file that describes the intended deployment.</span></span> <span data-ttu-id="a97a9-155">I följande exempel distribueras Nginx-behållaren, vilket binder port 80 på DC/OS-agenten till port 80 på behållaren.</span><span class="sxs-lookup"><span data-stu-id="a97a9-155">The following sample deploys the Nginx container, binding port 80 of the DC/OS agent to port 80 of the container.</span></span>

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

<span data-ttu-id="a97a9-156">Distribuera en Docker-formaterad behållare genom att lagra JSON-filen på en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="a97a9-156">To deploy a Docker-formatted container, store the JSON file in an accessible location.</span></span> <span data-ttu-id="a97a9-157">Kör följande kommando för att distribuera behållaren.</span><span class="sxs-lookup"><span data-stu-id="a97a9-157">Next, to deploy the container, run the following command.</span></span> <span data-ttu-id="a97a9-158">Ange sökvägen till JSON-fil (`marathon.json` i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="a97a9-158">Specify the path to the JSON file (`marathon.json` in this example).</span></span>

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

<span data-ttu-id="a97a9-159">Du kan även använda Marathon API för att skala ut eller skala in programdistributioner.</span><span class="sxs-lookup"><span data-stu-id="a97a9-159">You can also use the Marathon API to scale out or scale in application deployments.</span></span> <span data-ttu-id="a97a9-160">I föregående exempel distribuerade du en instans av ett program.</span><span class="sxs-lookup"><span data-stu-id="a97a9-160">In the previous example, you deployed one instance of an application.</span></span> <span data-ttu-id="a97a9-161">Nu kan vi skala ut det här till tre instanser av ett program.</span><span class="sxs-lookup"><span data-stu-id="a97a9-161">Let's scale this out to three instances of an application.</span></span> <span data-ttu-id="a97a9-162">Det gör du genom att skapa en JSON-fil med nedanstående JSON-text och lagra den på en tillgänglig plats.</span><span class="sxs-lookup"><span data-stu-id="a97a9-162">To do so, create a JSON file by using the following JSON text, and store it in an accessible location.</span></span>

```json
{ "instances": 3 }
```

<span data-ttu-id="a97a9-163">Kör följande kommando för att skala ut programmet:</span><span class="sxs-lookup"><span data-stu-id="a97a9-163">Run the following command to scale out the application:</span></span>

> [!NOTE]
> <span data-ttu-id="a97a9-164">URI:n är http://localhost/marathon/v2/apps/ och följs av ID:t för programmet som ska skalas.</span><span class="sxs-lookup"><span data-stu-id="a97a9-164">The URI is http://localhost/marathon/v2/apps/ followed by the ID of the application to scale.</span></span> <span data-ttu-id="a97a9-165">Om du använder Nginx-exemplet som anges här blir URI:n http://localhost/marathon/v2/apps/nginx.</span><span class="sxs-lookup"><span data-stu-id="a97a9-165">If you are using the Nginx sample provided here, the URI would be http://localhost/marathon/v2/apps/nginx.</span></span>
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a><span data-ttu-id="a97a9-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a97a9-166">Next steps</span></span>
* [<span data-ttu-id="a97a9-167">Läs mer om Mesos HTTP-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="a97a9-167">Read more about the Mesos HTTP endpoints</span></span>](http://mesos.apache.org/documentation/latest/endpoints/)
* [<span data-ttu-id="a97a9-168">Läs mer om Marathon REST API</span><span class="sxs-lookup"><span data-stu-id="a97a9-168">Read more about the Marathon REST API</span></span>](https://mesosphere.github.io/marathon/docs/rest-api.html)

