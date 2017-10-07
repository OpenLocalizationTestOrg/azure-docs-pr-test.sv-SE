---
title: "aaaManage Azure DC/OS-kluster med Marathon-Gränssnittet | Microsoft Docs"
description: "Distribuera behållare tooan Azure Container Service-klustertjänsten med hello Marathons webbgränssnitt."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a90180e1b4763e6d2ddfa699ed4b7269f209f728
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-hello-marathon-web-ui"></a><span data-ttu-id="452db-104">Hantera en Azure Container Service DC/OS-klustret via hello Marathons webbgränssnitt</span><span class="sxs-lookup"><span data-stu-id="452db-104">Manage an Azure Container Service DC/OS cluster through hello Marathon web UI</span></span>
<span data-ttu-id="452db-105">DC/OS erbjuder en miljö för att distribuera och skala klustrade arbetsbelastningar samtidigt abstrahera hello underliggande maskinvara.</span><span class="sxs-lookup"><span data-stu-id="452db-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting hello underlying hardware.</span></span> <span data-ttu-id="452db-106">Utöver DC/OS finns det ett ramverk som hanterar schemaläggning och beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="452db-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span>

<span data-ttu-id="452db-107">Även om ramverk är tillgängliga för många populära arbetsbelastningar beskriver det här dokumentet hur tooget igång med att distribuera behållare med Marathon.</span><span class="sxs-lookup"><span data-stu-id="452db-107">While frameworks are available for many popular workloads, this document describes how tooget started deploying containers with Marathon.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="452db-108">Krav</span><span class="sxs-lookup"><span data-stu-id="452db-108">Prerequisites</span></span>
<span data-ttu-id="452db-109">Innan du börjar med de här exemplen behöver du ett DC/OS-kluster som har konfigurerats i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="452db-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="452db-110">Du måste också toohave fjärranslutningar toothis klustret.</span><span class="sxs-lookup"><span data-stu-id="452db-110">You also need toohave remote connectivity toothis cluster.</span></span> <span data-ttu-id="452db-111">Mer information om dessa objekt finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="452db-111">For more information on these items, see hello following articles:</span></span>

* [<span data-ttu-id="452db-112">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="452db-112">Deploy an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="452db-113">Ansluta tooan Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="452db-113">Connect tooan Azure Container Service cluster</span></span>](../container-service-connect.md)

> [!NOTE]
> <span data-ttu-id="452db-114">Den här artikeln förutsätter att du använder tunneltrafik toohello DC/OS-klustret via en lokal port 80.</span><span class="sxs-lookup"><span data-stu-id="452db-114">This article assumes you are tunneling toohello DC/OS cluster through your local port 80.</span></span>
>

## <a name="explore-hello-dcos-ui"></a><span data-ttu-id="452db-115">Utforska hello DC/OS-gränssnitt</span><span class="sxs-lookup"><span data-stu-id="452db-115">Explore hello DC/OS UI</span></span>
<span data-ttu-id="452db-116">Med en tunnel SSH (Secure Shell) [upprätta](../container-service-connect.md), bläddra toohttp://localhost/.</span><span class="sxs-lookup"><span data-stu-id="452db-116">With a Secure Shell (SSH) tunnel [established](../container-service-connect.md), browse toohttp://localhost/.</span></span> <span data-ttu-id="452db-117">Detta laddar hello DC/OS-webbgränssnittet och visar information om hello klustret, till exempel använda resurser, aktiva agenter och tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="452db-117">This loads hello DC/OS web UI and shows information about hello cluster, such as used resources, active agents, and running services.</span></span>

![DC/OS-gränssnitt:](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-hello-marathon-ui"></a><span data-ttu-id="452db-119">Utforska hello Marathon-Gränssnittet</span><span class="sxs-lookup"><span data-stu-id="452db-119">Explore hello Marathon UI</span></span>
<span data-ttu-id="452db-120">toosee hello Användargränssnittet för Marathon Bläddra toohttp://localhost/marathon.</span><span class="sxs-lookup"><span data-stu-id="452db-120">toosee hello Marathon UI, browse toohttp://localhost/marathon.</span></span> <span data-ttu-id="452db-121">Från den här skärmbilden kan starta du en ny behållare eller ett annat program på hello Azure Container Service DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="452db-121">From this screen, you can start a new container or another application on hello Azure Container Service DC/OS cluster.</span></span> <span data-ttu-id="452db-122">Du kan även se information om att köra behållare och program.</span><span class="sxs-lookup"><span data-stu-id="452db-122">You can also see information about running containers and applications.</span></span>  

![Gränssnittet i Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="452db-124">Distribuera en Docker-formaterad behållare</span><span class="sxs-lookup"><span data-stu-id="452db-124">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="452db-125">toodeploy en ny behållare med Marathon, klickar du på **skapa program**, och ange följande information i hello formuläret flikar hello:</span><span class="sxs-lookup"><span data-stu-id="452db-125">toodeploy a new container by using Marathon, click **Create Application**, and enter hello following information into hello form tabs:</span></span>

| <span data-ttu-id="452db-126">Fält</span><span class="sxs-lookup"><span data-stu-id="452db-126">Field</span></span> | <span data-ttu-id="452db-127">Värde</span><span class="sxs-lookup"><span data-stu-id="452db-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="452db-128">ID</span><span class="sxs-lookup"><span data-stu-id="452db-128">ID</span></span> |<span data-ttu-id="452db-129">nginx</span><span class="sxs-lookup"><span data-stu-id="452db-129">nginx</span></span> |
| <span data-ttu-id="452db-130">Minne</span><span class="sxs-lookup"><span data-stu-id="452db-130">Memory</span></span> | <span data-ttu-id="452db-131">32</span><span class="sxs-lookup"><span data-stu-id="452db-131">32</span></span> |
| <span data-ttu-id="452db-132">Bild</span><span class="sxs-lookup"><span data-stu-id="452db-132">Image</span></span> |<span data-ttu-id="452db-133">nginx</span><span class="sxs-lookup"><span data-stu-id="452db-133">nginx</span></span> |
| <span data-ttu-id="452db-134">Nätverk</span><span class="sxs-lookup"><span data-stu-id="452db-134">Network</span></span> |<span data-ttu-id="452db-135">Bryggad</span><span class="sxs-lookup"><span data-stu-id="452db-135">Bridged</span></span> |
| <span data-ttu-id="452db-136">Värdport</span><span class="sxs-lookup"><span data-stu-id="452db-136">Host Port</span></span> |<span data-ttu-id="452db-137">80</span><span class="sxs-lookup"><span data-stu-id="452db-137">80</span></span> |
| <span data-ttu-id="452db-138">Protokoll</span><span class="sxs-lookup"><span data-stu-id="452db-138">Protocol</span></span> |<span data-ttu-id="452db-139">TCP</span><span class="sxs-lookup"><span data-stu-id="452db-139">TCP</span></span> |

![Nytt programgränssnitt – allmänt](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nytt programgränssnitt – Dockerbehållare](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nytt programgränssnitt – portar och identifiering av tjänst](./media/container-service-mesos-marathon-ui/dcos6.png)

<span data-ttu-id="452db-143">Om du vill toostatically mappa hello behållaren port tooa port på hello-agenten måste toouse JSON-läget.</span><span class="sxs-lookup"><span data-stu-id="452db-143">If you want toostatically map hello container port tooa port on hello agent, you need toouse JSON Mode.</span></span> <span data-ttu-id="452db-144">toodo så växla hello guiden för nya program för**JSON-läget** med hjälp av hello växla.</span><span class="sxs-lookup"><span data-stu-id="452db-144">toodo so, switch hello New Application wizard too**JSON Mode** by using hello toggle.</span></span> <span data-ttu-id="452db-145">Ange följande inställningen under hello hello `portMappings` avsnitt i hello programmets definition.</span><span class="sxs-lookup"><span data-stu-id="452db-145">Then enter hello following setting under hello `portMappings` section of hello application definition.</span></span> <span data-ttu-id="452db-146">Det här exemplet Binder port 80 på hello behållaren tooport 80 av hello DC/OS-agenten.</span><span class="sxs-lookup"><span data-stu-id="452db-146">This example binds port 80 of hello container tooport 80 of hello DC/OS agent.</span></span> <span data-ttu-id="452db-147">När du har gjort den här ändringen kan du växla ur guiden ur JSON-läget.</span><span class="sxs-lookup"><span data-stu-id="452db-147">You can switch this wizard out of JSON Mode after you make this change.</span></span>

```none
"hostPort": 80,
```

![Nytt programgränssnitt – port 80-exempel](./media/container-service-mesos-marathon-ui/dcos13.png)

<span data-ttu-id="452db-149">Om du vill tooenable hälsokontroller, ange en sökväg på hello **hälsa kontrollerar** fliken.</span><span class="sxs-lookup"><span data-stu-id="452db-149">If you want tooenable health checks, set a path on hello **Health Checks** tab.</span></span>

![Nytt programanvändargränssnitt – hälsokontroller](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

<span data-ttu-id="452db-151">hello DC/OS-klustret distribueras med privata och offentliga agenter.</span><span class="sxs-lookup"><span data-stu-id="452db-151">hello DC/OS cluster is deployed with set of private and public agents.</span></span> <span data-ttu-id="452db-152">För hello klustret toobe kan tooaccess program från hello Internet behöver du toodeploy hello program tooa offentlig agent.</span><span class="sxs-lookup"><span data-stu-id="452db-152">For hello cluster toobe able tooaccess applications from hello Internet, you need toodeploy hello applications tooa public agent.</span></span> <span data-ttu-id="452db-153">Välj toodo därför hello **valfritt** fliken hello nytt program guiden och ange **slave_public** för hello **accepterade resursroller**.</span><span class="sxs-lookup"><span data-stu-id="452db-153">toodo so, select hello **Optional** tab of hello New Application wizard and enter **slave_public** for hello **Accepted Resource Roles**.</span></span>

<span data-ttu-id="452db-154">Klicka på **Skapa program**.</span><span class="sxs-lookup"><span data-stu-id="452db-154">Then click **Create Application**.</span></span>

![Nytt programgränssnitt – inställning av offentlig agent](./media/container-service-mesos-marathon-ui/dcos14.png)

<span data-ttu-id="452db-156">Tillbaka på hello huvudsidan för Marathon ser du hello Distributionsstatus för hello behållare.</span><span class="sxs-lookup"><span data-stu-id="452db-156">Back on hello Marathon main page, you can see hello deployment status for hello container.</span></span> <span data-ttu-id="452db-157">Inledningsvis visas statusen **Distribuerar**.</span><span class="sxs-lookup"><span data-stu-id="452db-157">Initially you see a status of **Deploying**.</span></span> <span data-ttu-id="452db-158">Efter en lyckad distribution hello status ändras för**kör**.</span><span class="sxs-lookup"><span data-stu-id="452db-158">After a successful deployment, hello status changes too**Running**.</span></span>

![Marathon-huvudsidans gränssnitt 0 behållarens distributionsstatus](./media/container-service-mesos-marathon-ui/dcos7.png)

<span data-ttu-id="452db-160">När du växlar tillbaka toohello DC/OS-webbgränssnittet (http://localhost/) ser du att en aktivitet (i det här fallet en Docker-formaterad behållare) körs på hello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="452db-160">When you switch back toohello DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on hello DC/OS cluster.</span></span>

![DC/OS-webbgränssnitt – aktivitet som körs på klustret hello](./media/container-service-mesos-marathon-ui/dcos8.png)

<span data-ttu-id="452db-162">toosee hello nod i klustret som hello aktiviteten körs på, klicka på hello **noder** fliken.</span><span class="sxs-lookup"><span data-stu-id="452db-162">toosee hello cluster node that hello task is running on, click hello **Nodes** tab.</span></span>

![DC/OS-webbgränssnitt – klusternod](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-hello-container"></a><span data-ttu-id="452db-164">Nå hello behållare</span><span class="sxs-lookup"><span data-stu-id="452db-164">Reach hello container</span></span>

<span data-ttu-id="452db-165">I det här exemplet körs hello programmet på en offentlig agent-nod.</span><span class="sxs-lookup"><span data-stu-id="452db-165">In this example, hello application is running on a public agent node.</span></span> <span data-ttu-id="452db-166">Du når programmet hello från hello internet genom att bläddra toohello agent FQDN för hello kluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, där:</span><span class="sxs-lookup"><span data-stu-id="452db-166">You reach hello application from hello internet by browsing toohello agent FQDN of hello cluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, where:</span></span>

* <span data-ttu-id="452db-167">**DNSPREFIX** är hello DNS-prefix som du angav när du har distribuerat hello klustret.</span><span class="sxs-lookup"><span data-stu-id="452db-167">**DNSPREFIX** is hello DNS prefix that you provided when you deployed hello cluster.</span></span>
* <span data-ttu-id="452db-168">**REGION** hello region där resursgruppen finns.</span><span class="sxs-lookup"><span data-stu-id="452db-168">**REGION** is hello region in which your resource group is located.</span></span>

    ![Nginx från Internet](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a><span data-ttu-id="452db-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="452db-170">Next steps</span></span>
* [<span data-ttu-id="452db-171">Arbeta med DC/OS och Marathon API hello</span><span class="sxs-lookup"><span data-stu-id="452db-171">Work with DC/OS and hello Marathon API</span></span>](container-service-mesos-marathon-rest.md)

* <span data-ttu-id="452db-172">Ingående om hello Azure Container Service med Mesos</span><span class="sxs-lookup"><span data-stu-id="452db-172">Deep dive on hello Azure Container Service with Mesos</span></span>

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
