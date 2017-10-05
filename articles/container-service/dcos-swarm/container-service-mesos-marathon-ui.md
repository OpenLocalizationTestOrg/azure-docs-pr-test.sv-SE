---
title: "Hantera Azure DC/OS-kluster med Marathon-Gränssnittet | Microsoft Docs"
description: "Distribuera behållare till en klustertjänst i Azure Container Service med Marathons webbgränssnitt."
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
ms.openlocfilehash: b00088bb005519dc5d533433308c0e3e33c7f433
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a><span data-ttu-id="8b0fc-104">Hantera ett Azure Container Service DC/OS-kluster via webbgränssnittet för Marathon</span><span class="sxs-lookup"><span data-stu-id="8b0fc-104">Manage an Azure Container Service DC/OS cluster through the Marathon web UI</span></span>
<span data-ttu-id="8b0fc-105">DC/OS erbjuder en miljö för att distribuera och skala klustrade arbetsbelastningar samtidigt som den underliggande maskinvaran abstraheras.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-105">DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware.</span></span> <span data-ttu-id="8b0fc-106">Utöver DC/OS finns det ett ramverk som hanterar schemaläggning och beräkning av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-106">On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.</span></span>

<span data-ttu-id="8b0fc-107">Även om ramverk är tillgängliga för många populära arbetsbelastningar beskriver det här dokumentet hur du kommer igång med att distribuera behållare med Marathon.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-107">While frameworks are available for many popular workloads, this document describes how to get started deploying containers with Marathon.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="8b0fc-108">Krav</span><span class="sxs-lookup"><span data-stu-id="8b0fc-108">Prerequisites</span></span>
<span data-ttu-id="8b0fc-109">Innan du börjar med de här exemplen behöver du ett DC/OS-kluster som har konfigurerats i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-109">Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service.</span></span> <span data-ttu-id="8b0fc-110">Du måste också kunna fjärransluta till det här klustret.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-110">You also need to have remote connectivity to this cluster.</span></span> <span data-ttu-id="8b0fc-111">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="8b0fc-111">For more information on these items, see the following articles:</span></span>

* [<span data-ttu-id="8b0fc-112">Distribuera ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="8b0fc-112">Deploy an Azure Container Service cluster</span></span>](container-service-deployment.md)
* [<span data-ttu-id="8b0fc-113">Ansluta till ett Azure Container Service-kluster</span><span class="sxs-lookup"><span data-stu-id="8b0fc-113">Connect to an Azure Container Service cluster</span></span>](../container-service-connect.md)

> [!NOTE]
> <span data-ttu-id="8b0fc-114">Den här artikeln förutsätter att du använder tunneltrafik DC/OS-klustret via en lokal port 80.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-114">This article assumes you are tunneling to the DC/OS cluster through your local port 80.</span></span>
>

## <a name="explore-the-dcos-ui"></a><span data-ttu-id="8b0fc-115">Utforska gränssnittet för DC/OS</span><span class="sxs-lookup"><span data-stu-id="8b0fc-115">Explore the DC/OS UI</span></span>
<span data-ttu-id="8b0fc-116">Gå till http://localhost/ via en [etablerad](../container-service-connect.md) SSH-tunnel (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="8b0fc-116">With a Secure Shell (SSH) tunnel [established](../container-service-connect.md), browse to http://localhost/.</span></span> <span data-ttu-id="8b0fc-117">Då läses webbgränssnittet för DC/OS in och du kan se information om klustret, till exempel använda resurser, aktiva agenter och tjänster som körs.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-117">This loads the DC/OS web UI and shows information about the cluster, such as used resources, active agents, and running services.</span></span>

![DC/OS-gränssnitt:](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-the-marathon-ui"></a><span data-ttu-id="8b0fc-119">Utforska Marathon-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="8b0fc-119">Explore the Marathon UI</span></span>
<span data-ttu-id="8b0fc-120">Om du vill visa Gränssnittet i Marathon, gå till http://localhost/marathon.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-120">To see the Marathon UI, browse to http://localhost/marathon.</span></span> <span data-ttu-id="8b0fc-121">Från den här skärmbilden kan du starta en ny behållare eller ett annat program på DC/OS-klustret för Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-121">From this screen, you can start a new container or another application on the Azure Container Service DC/OS cluster.</span></span> <span data-ttu-id="8b0fc-122">Du kan även se information om att köra behållare och program.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-122">You can also see information about running containers and applications.</span></span>  

![Gränssnittet i Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a><span data-ttu-id="8b0fc-124">Distribuera en Docker-formaterad behållare</span><span class="sxs-lookup"><span data-stu-id="8b0fc-124">Deploy a Docker-formatted container</span></span>
<span data-ttu-id="8b0fc-125">Om du vill distribuera en ny behållare med hjälp av Marathon klickar du på **Skapa program** och anger följande information på flikarna i formuläret:</span><span class="sxs-lookup"><span data-stu-id="8b0fc-125">To deploy a new container by using Marathon, click **Create Application**, and enter the following information into the form tabs:</span></span>

| <span data-ttu-id="8b0fc-126">Fält</span><span class="sxs-lookup"><span data-stu-id="8b0fc-126">Field</span></span> | <span data-ttu-id="8b0fc-127">Värde</span><span class="sxs-lookup"><span data-stu-id="8b0fc-127">Value</span></span> |
| --- | --- |
| <span data-ttu-id="8b0fc-128">ID</span><span class="sxs-lookup"><span data-stu-id="8b0fc-128">ID</span></span> |<span data-ttu-id="8b0fc-129">nginx</span><span class="sxs-lookup"><span data-stu-id="8b0fc-129">nginx</span></span> |
| <span data-ttu-id="8b0fc-130">Minne</span><span class="sxs-lookup"><span data-stu-id="8b0fc-130">Memory</span></span> | <span data-ttu-id="8b0fc-131">32</span><span class="sxs-lookup"><span data-stu-id="8b0fc-131">32</span></span> |
| <span data-ttu-id="8b0fc-132">Bild</span><span class="sxs-lookup"><span data-stu-id="8b0fc-132">Image</span></span> |<span data-ttu-id="8b0fc-133">nginx</span><span class="sxs-lookup"><span data-stu-id="8b0fc-133">nginx</span></span> |
| <span data-ttu-id="8b0fc-134">Nätverk</span><span class="sxs-lookup"><span data-stu-id="8b0fc-134">Network</span></span> |<span data-ttu-id="8b0fc-135">Bryggad</span><span class="sxs-lookup"><span data-stu-id="8b0fc-135">Bridged</span></span> |
| <span data-ttu-id="8b0fc-136">Värdport</span><span class="sxs-lookup"><span data-stu-id="8b0fc-136">Host Port</span></span> |<span data-ttu-id="8b0fc-137">80</span><span class="sxs-lookup"><span data-stu-id="8b0fc-137">80</span></span> |
| <span data-ttu-id="8b0fc-138">Protokoll</span><span class="sxs-lookup"><span data-stu-id="8b0fc-138">Protocol</span></span> |<span data-ttu-id="8b0fc-139">TCP</span><span class="sxs-lookup"><span data-stu-id="8b0fc-139">TCP</span></span> |

![Nytt programgränssnitt – allmänt](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nytt programgränssnitt – Dockerbehållare](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nytt programgränssnitt – portar och identifiering av tjänst](./media/container-service-mesos-marathon-ui/dcos6.png)

<span data-ttu-id="8b0fc-143">Om du vill mappa behållarporten statiskt till en port på agenten måste du använda JSON-läget.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-143">If you want to statically map the container port to a port on the agent, you need to use JSON Mode.</span></span> <span data-ttu-id="8b0fc-144">För att göra det växlar du guiden för nya program till **JSON-läge** med hjälp av växlingsknappen.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-144">To do so, switch the New Application wizard to **JSON Mode** by using the toggle.</span></span> <span data-ttu-id="8b0fc-145">Ange sedan följande inställning under avsnittet `portMappings` i programdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-145">Then enter the following setting under the `portMappings` section of the application definition.</span></span> <span data-ttu-id="8b0fc-146">Det här exemplet binder behållarens port 80 till port 80 på DC/OS-agenten.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-146">This example binds port 80 of the container to port 80 of the DC/OS agent.</span></span> <span data-ttu-id="8b0fc-147">När du har gjort den här ändringen kan du växla ur guiden ur JSON-läget.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-147">You can switch this wizard out of JSON Mode after you make this change.</span></span>

```none
"hostPort": 80,
```

![Nytt programgränssnitt – port 80-exempel](./media/container-service-mesos-marathon-ui/dcos13.png)

<span data-ttu-id="8b0fc-149">Om du vill aktivera hälsokontroller anger du en sökväg på fliken **Hälsokontroller**.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-149">If you want to enable health checks, set a path on the **Health Checks** tab.</span></span>

![Nytt programanvändargränssnitt – hälsokontroller](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

<span data-ttu-id="8b0fc-151">DC/OS-klustret distribueras med privata och offentliga agenter.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-151">The DC/OS cluster is deployed with set of private and public agents.</span></span> <span data-ttu-id="8b0fc-152">För att klustret ska kunna komma åt program från Internet måste du distribuera programmen till en offentlig agent.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-152">For the cluster to be able to access applications from the Internet, you need to deploy the applications to a public agent.</span></span> <span data-ttu-id="8b0fc-153">För att göra det väljer du fliken **Valfritt** i guiden Nytt program och anger **slave_public** för **Accepterade resursroller**.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-153">To do so, select the **Optional** tab of the New Application wizard and enter **slave_public** for the **Accepted Resource Roles**.</span></span>

<span data-ttu-id="8b0fc-154">Klicka på **Skapa program**.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-154">Then click **Create Application**.</span></span>

![Nytt programgränssnitt – inställning av offentlig agent](./media/container-service-mesos-marathon-ui/dcos14.png)

<span data-ttu-id="8b0fc-156">Tillbaka på huvudsidan för Marathon kan du se distributionsstatusen för behållaren.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-156">Back on the Marathon main page, you can see the deployment status for the container.</span></span> <span data-ttu-id="8b0fc-157">Inledningsvis visas statusen **Distribuerar**.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-157">Initially you see a status of **Deploying**.</span></span> <span data-ttu-id="8b0fc-158">När distributionen är klar ändras statusen till **Kör**.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-158">After a successful deployment, the status changes to **Running**.</span></span>

![Marathon-huvudsidans gränssnitt 0 behållarens distributionsstatus](./media/container-service-mesos-marathon-ui/dcos7.png)

<span data-ttu-id="8b0fc-160">När du växlar tillbaka till webbgränssnittet för DC/OS (http://localhost/) ser du att en aktivitet (i det här fallet en Docker-formaterad behållare) körs i DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-160">When you switch back to the DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on the DC/OS cluster.</span></span>

![DC/OS-webbgränssnitt – aktivitet som körs på klustret](./media/container-service-mesos-marathon-ui/dcos8.png)

<span data-ttu-id="8b0fc-162">Du kan visa klusternoden som uppgiften körs på genom att klicka på fliken **Noder**.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-162">To see the cluster node that the task is running on, click the **Nodes** tab.</span></span>

![DC/OS-webbgränssnitt – klusternod](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-the-container"></a><span data-ttu-id="8b0fc-164">Nå behållaren</span><span class="sxs-lookup"><span data-stu-id="8b0fc-164">Reach the container</span></span>

<span data-ttu-id="8b0fc-165">Programmet körs i detta exempel på en offentlig agent-nod.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-165">In this example, the application is running on a public agent node.</span></span> <span data-ttu-id="8b0fc-166">Du når programmet från internet genom att bläddra till agenten klustrets FQDN-namn: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, där:</span><span class="sxs-lookup"><span data-stu-id="8b0fc-166">You reach the application from the internet by browsing to the agent FQDN of the cluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, where:</span></span>

* <span data-ttu-id="8b0fc-167">**DNSPREFIX** är det DNS-prefix som du angav när du distribuerade klustret.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-167">**DNSPREFIX** is the DNS prefix that you provided when you deployed the cluster.</span></span>
* <span data-ttu-id="8b0fc-168">**REGION** är den region där resursgruppen finns.</span><span class="sxs-lookup"><span data-stu-id="8b0fc-168">**REGION** is the region in which your resource group is located.</span></span>

    ![Nginx från Internet](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a><span data-ttu-id="8b0fc-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b0fc-170">Next steps</span></span>
* [<span data-ttu-id="8b0fc-171">Arbeta med API för DC/OS och Marathon API</span><span class="sxs-lookup"><span data-stu-id="8b0fc-171">Work with DC/OS and the Marathon API</span></span>](container-service-mesos-marathon-rest.md)

* <span data-ttu-id="8b0fc-172">Ingående om Azure Container Service med Mesos</span><span class="sxs-lookup"><span data-stu-id="8b0fc-172">Deep dive on the Azure Container Service with Mesos</span></span>

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
