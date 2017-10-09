---
title: "aaaDeploy din första app tooCloud Foundry på Microsoft Azure | Microsoft Docs"
description: "Distribuera ett program tooCloud Foundry på Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 8fa04a58-56ad-4e6c-bef4-d02c80d4b60f
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 878da38f6eabe32a339f02aa0ead811d6e5af9a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-first-app-toocloud-foundry-on-microsoft-azure"></a><span data-ttu-id="f8a20-103">Distribuera din första app tooCloud Foundry på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f8a20-103">Deploy your first app tooCloud Foundry on Microsoft Azure</span></span>

<span data-ttu-id="f8a20-104">[Molnet Foundry](http://cloudfoundry.org) är en programplattform för populära öppen källkod tillgängliga på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8a20-104">[Cloud Foundry](http://cloudfoundry.org) is a popular open-source application platform available on Microsoft Azure.</span></span> <span data-ttu-id="f8a20-105">I den här artikeln visar vi hur toodeploy och hantera ett program på molnet Foundry i en Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="f8a20-105">In this article, we show how toodeploy and manage an application on Cloud Foundry in an Azure environment.</span></span>

## <a name="create-a-cloud-foundry-environment"></a><span data-ttu-id="f8a20-106">Skapa en moln Foundry-miljö</span><span class="sxs-lookup"><span data-stu-id="f8a20-106">Create a Cloud Foundry environment</span></span>

<span data-ttu-id="f8a20-107">Det finns flera alternativ för att skapa en miljö med molnet Foundry i Azure:</span><span class="sxs-lookup"><span data-stu-id="f8a20-107">There are several options for creating a Cloud Foundry environment on Azure:</span></span>

- <span data-ttu-id="f8a20-108">Använd hello [spela en central molnet Foundry erbjudande] [ pcf-azuremarketplace] i hello Azure Marketplace toocreate en standard miljö som innehåller PCF Ops Manager och hello Azure Service Broker.</span><span class="sxs-lookup"><span data-stu-id="f8a20-108">Use hello [Pivotal Cloud Foundry offer][pcf-azuremarketplace] in hello Azure Marketplace toocreate a standard environment that includes PCF Ops Manager and hello Azure Service Broker.</span></span> <span data-ttu-id="f8a20-109">Du kan hitta [fullständiga] [ pcf-azuremarketplace-pivotaldocs] för att distribuera hello marketplace erbjuder i hello spela en central dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f8a20-109">You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] for deploying hello marketplace offer in hello Pivotal documentation.</span></span>
- <span data-ttu-id="f8a20-110">Skapa en anpassad miljö genom [manuellt distribuera spela en central molnet Foundry][pcf-custom].</span><span class="sxs-lookup"><span data-stu-id="f8a20-110">Create a customized environment by [deploying Pivotal Cloud Foundry manually][pcf-custom].</span></span>
- <span data-ttu-id="f8a20-111">[Distribuera hello öppen källkod molnet Foundry paket direkt] [ oss-cf-bosh] genom att ställa in en [BOSH](http://bosh.io) chef, en virtuell dator som samordnar hello distribution av hello molnet Foundry miljö.</span><span class="sxs-lookup"><span data-stu-id="f8a20-111">[Deploy hello open-source Cloud Foundry packages directly][oss-cf-bosh] by setting up a [BOSH](http://bosh.io) director, a VM that coordinates hello deployment of hello Cloud Foundry environment.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="f8a20-112">Om du distribuerar PCF från hello Azure Marketplace anteckna hello SYSTEMDOMAINURL och hello administratörsautentiseringsuppgifter krävs tooaccess hello spela en central appar Manager, som beskrivs i distributionsguiden för hello marketplace.</span><span class="sxs-lookup"><span data-stu-id="f8a20-112">If you are deploying PCF from hello Azure Marketplace, make a note of hello SYSTEMDOMAINURL and hello admin credentials required tooaccess hello Pivotal Apps Manager, both of which are described in hello marketplace deployment guide.</span></span> <span data-ttu-id="f8a20-113">De är nödvändiga toocomplete den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f8a20-113">They are needed toocomplete this tutorial.</span></span> <span data-ttu-id="f8a20-114">Hej SYSTEMDOMAINURL är i hello formuläret https://system för marketplace-distributioner. *ip-adress*. cf.pcfazure.com.</span><span class="sxs-lookup"><span data-stu-id="f8a20-114">For marketplace deployments, hello SYSTEMDOMAINURL is in hello form https://system.*ip-address*.cf.pcfazure.com.</span></span>

## <a name="connect-toohello-cloud-controller"></a><span data-ttu-id="f8a20-115">Ansluta toohello moln-styrenhet</span><span class="sxs-lookup"><span data-stu-id="f8a20-115">Connect toohello Cloud Controller</span></span>

<span data-ttu-id="f8a20-116">hello molnet domänkontrollant är hello primära post punkt tooa moln Foundry miljö för att distribuera och hantera program.</span><span class="sxs-lookup"><span data-stu-id="f8a20-116">hello Cloud Controller is hello primary entry point tooa Cloud Foundry environment for deploying and managing applications.</span></span> <span data-ttu-id="f8a20-117">hello core molnet Controller API (CCAPI) är en REST-API, men den är tillgänglig via olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="f8a20-117">hello core Cloud Controller API (CCAPI) is a REST API, but it is accessible through various tools.</span></span> <span data-ttu-id="f8a20-118">I det här fallet vi interagerar med det via hello [moln Foundry CLI][cf-cli].</span><span class="sxs-lookup"><span data-stu-id="f8a20-118">In this case, we interact with it through hello [Cloud Foundry CLI][cf-cli].</span></span> <span data-ttu-id="f8a20-119">Du kan installera hello CLI på Linux, MacOS eller Windows, men om du föredrar att inte tooinstall den, den är tillgänglig förinstallerat i hello [Azure Cloud Shell][cloudshell-docs].</span><span class="sxs-lookup"><span data-stu-id="f8a20-119">You can install hello CLI on Linux, MacOS, or Windows, but if you'd prefer not tooinstall it at all, it is available pre-installed in hello [Azure Cloud Shell][cloudshell-docs].</span></span>

<span data-ttu-id="f8a20-120">toolog, lägga `api` toohello SYSTEMDOMAINURL som du fick från hello marketplace-distributionen.</span><span class="sxs-lookup"><span data-stu-id="f8a20-120">toolog in, prepend `api` toohello SYSTEMDOMAINURL that you obtained from hello marketplace deployment.</span></span> <span data-ttu-id="f8a20-121">Eftersom hello standard distributionen använder ett självsignerat certifikat, bör du också inkludera hello `skip-ssl-validation` växla.</span><span class="sxs-lookup"><span data-stu-id="f8a20-121">Since hello default deployment uses a self-signed certificate, you should also include hello `skip-ssl-validation` switch.</span></span>

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

<span data-ttu-id="f8a20-122">Du kan ange toolog i toohello moln-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="f8a20-122">You are prompted toolog in toohello Cloud Controller.</span></span> <span data-ttu-id="f8a20-123">Använd hello admin kontoautentiseringsuppgifter som du har köpt från hello marketplace-distributionsstegen.</span><span class="sxs-lookup"><span data-stu-id="f8a20-123">Use hello admin account credentials that you acquired from hello marketplace deployment steps.</span></span>

<span data-ttu-id="f8a20-124">Molnet Foundry ger *organisationer* och *blanksteg* som namnområden tooisolate hello team och miljöer i en delad distribution.</span><span class="sxs-lookup"><span data-stu-id="f8a20-124">Cloud Foundry provides *orgs* and *spaces* as namespaces tooisolate hello teams and environments within a shared deployment.</span></span> <span data-ttu-id="f8a20-125">Hej PCF marketplace distribution innehåller hello standard *system* org och en uppsättning blanksteg skapade toocontain hello baskomponenterna som hello autoskalning service och hello Azure service broker.</span><span class="sxs-lookup"><span data-stu-id="f8a20-125">hello PCF marketplace deployment includes hello default *system* org and a set of spaces created toocontain hello base components, like hello autoscaling service and hello Azure service broker.</span></span> <span data-ttu-id="f8a20-126">Välj hello just nu *system* utrymme.</span><span class="sxs-lookup"><span data-stu-id="f8a20-126">For now, choose hello *system* space.</span></span>


## <a name="create-an-org-and-space"></a><span data-ttu-id="f8a20-127">Skapa en organisations och utrymme</span><span class="sxs-lookup"><span data-stu-id="f8a20-127">Create an org and space</span></span>

<span data-ttu-id="f8a20-128">Om du skriver `cf apps`, visas en uppsättning systemprogram som har distribuerats i hello systemutrymme i hello system organisationen.</span><span class="sxs-lookup"><span data-stu-id="f8a20-128">If you type `cf apps`, you see a set of system applications that have been deployed in hello system space within hello system org.</span></span> 

<span data-ttu-id="f8a20-129">Du bör ha hello *system* org som reserverats för systemprogram, så du måste skapa en organisations och utrymme toohouse våra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f8a20-129">You should keep hello *system* org reserved for system applications, so create an org and space toohouse our sample application.</span></span>

```bash
cf create-org myorg
cf create-space dev -o myorg
```

<span data-ttu-id="f8a20-130">Använd hello mål kommandot tooswitch toohello nya org och utrymme:</span><span class="sxs-lookup"><span data-stu-id="f8a20-130">Use hello target command tooswitch toohello new org and space:</span></span>

```bash
cf target -o testorg -s dev
```

<span data-ttu-id="f8a20-131">Nu när du distribuerar ett program kan skapas det automatiskt i hello nya org och blanksteg.</span><span class="sxs-lookup"><span data-stu-id="f8a20-131">Now, when you deploy an application, it is automatically created in hello new org and space.</span></span> <span data-ttu-id="f8a20-132">tooconfirm som det finns för närvarande inga appar i hello nya org/utrymme, Skriv `cf apps` igen.</span><span class="sxs-lookup"><span data-stu-id="f8a20-132">tooconfirm that there are currently no apps in hello new org/space, type `cf apps` again.</span></span>

> [!NOTE] 
> <span data-ttu-id="f8a20-133">Mer information om organisationer blanksteg och hur de kan användas för rollbaserad åtkomstkontroll (RBAC) finns hello [moln Foundry dokumentationen][cf-orgs-spaces-docs].</span><span class="sxs-lookup"><span data-stu-id="f8a20-133">For more information about orgs and spaces and how they can be used for role-based access control (RBAC), see hello [Cloud Foundry documentation][cf-orgs-spaces-docs].</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="f8a20-134">Distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="f8a20-134">Deploy an application</span></span>

<span data-ttu-id="f8a20-135">Nu ska vi använda ett exempelprogram för molntjänster Foundry kallas Hello källan moln, vilket är skriven i Java och baserat på hello [Vårversionen Framework](http://spring.io) och [Vårversionen Start](http://projects.spring.io/spring-boot/).</span><span class="sxs-lookup"><span data-stu-id="f8a20-135">Let's use a sample Cloud Foundry application called Hello Spring Cloud, which is written in Java and based on hello [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).</span></span>

### <a name="clone-hello-hello-spring-cloud-repository"></a><span data-ttu-id="f8a20-136">Klona hello Hello källan molnet databasen</span><span class="sxs-lookup"><span data-stu-id="f8a20-136">Clone hello Hello Spring Cloud repository</span></span>

<span data-ttu-id="f8a20-137">hello Hello källan molnet exempelprogrammet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="f8a20-137">hello Hello Spring Cloud sample application is available on GitHub.</span></span> <span data-ttu-id="f8a20-138">Klona den tooyour miljö och ändra till nya hello-katalogen:</span><span class="sxs-lookup"><span data-stu-id="f8a20-138">Clone it tooyour environment and change into hello new directory:</span></span>

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-hello-application"></a><span data-ttu-id="f8a20-139">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="f8a20-139">Build hello application</span></span>

<span data-ttu-id="f8a20-140">Build hello appen med [Apache Maven](http://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="f8a20-140">Build hello app using [Apache Maven](http://maven.apache.org).</span></span>

```bash
mvn clean package
```

### <a name="deploy-hello-application-with-cf-push"></a><span data-ttu-id="f8a20-141">Distribuera hello program med cf push</span><span class="sxs-lookup"><span data-stu-id="f8a20-141">Deploy hello application with cf push</span></span>

<span data-ttu-id="f8a20-142">Du kan distribuera de flesta program tooCloud Foundry med hello `push` kommando:</span><span class="sxs-lookup"><span data-stu-id="f8a20-142">You can deploy most applications tooCloud Foundry using hello `push` command:</span></span>

```bash
cf push
```

<span data-ttu-id="f8a20-143">När du *push* ett program, molnet Foundry identifierar hello typ av program (i det här fallet en Java-app) och identifierar dess beroenden (i det här fallet hello källan framework).</span><span class="sxs-lookup"><span data-stu-id="f8a20-143">When you *push* an application, Cloud Foundry detects hello type of application (in this case, a Java app) and identifies its dependencies (in this case, hello Spring framework).</span></span> <span data-ttu-id="f8a20-144">Den sedan paket allt krävs toorun din kod i en fristående behållaren avbildning som kallas en *droplet*.</span><span class="sxs-lookup"><span data-stu-id="f8a20-144">It then packages everything required toorun your code into a standalone container image, known as a *droplet*.</span></span> <span data-ttu-id="f8a20-145">Slutligen molnet Foundry scheman hello program på en av hello tillgängliga datorer i din miljö och skapar en URL där du kan nå den, som finns i hello hello kommandots utdata.</span><span class="sxs-lookup"><span data-stu-id="f8a20-145">Finally, Cloud Foundry schedules hello application on one of hello available machines in your environment and creates a URL where you can reach it, which is available in hello output of hello command.</span></span>

![Utdata från cf push-kommando][cf-push-output]

<span data-ttu-id="f8a20-147">toosee hello hello källan moln program, öppna hello angivna URL: en i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="f8a20-147">toosee hello hello-spring-cloud application, open hello provided URL in your browser:</span></span>

![Standardgränssnitt för Hello källan moln][hello-spring-cloud-basic]

> [!NOTE] 
> <span data-ttu-id="f8a20-149">Mer om vad som händer under toolearn `cf push`, finns [hur program mellanlagras] [ cf-push-docs] i hello molnet Foundry-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="f8a20-149">toolearn more about what happens during `cf push`, see [How Applications Are Staged][cf-push-docs] in hello Cloud Foundry documentation.</span></span>

## <a name="view-application-logs"></a><span data-ttu-id="f8a20-150">Visa programloggar</span><span class="sxs-lookup"><span data-stu-id="f8a20-150">View application logs</span></span>

<span data-ttu-id="f8a20-151">Du kan använda hello molnet Foundry CLI tooview loggar för ett program med namnet:</span><span class="sxs-lookup"><span data-stu-id="f8a20-151">You can use hello Cloud Foundry CLI tooview logs for an application by its name:</span></span>

```bash
cf logs hello-spring-cloud
```

<span data-ttu-id="f8a20-152">Som standard loggas hello kommando använder *pilslut*, vilket visar nya loggar som de är skrivna.</span><span class="sxs-lookup"><span data-stu-id="f8a20-152">By default, hello logs command uses *tail*, which shows new logs as they are written.</span></span> <span data-ttu-id="f8a20-153">toosee nya loggar visas uppdatera hello hello-källan-cloud app i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f8a20-153">toosee new logs appear, refresh hello hello-spring-cloud app in hello browser.</span></span>

<span data-ttu-id="f8a20-154">tooview loggar som redan har skrivits, lägga till hello `recent` växel:</span><span class="sxs-lookup"><span data-stu-id="f8a20-154">tooview logs that have already been written, add hello `recent` switch:</span></span>

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-hello-application"></a><span data-ttu-id="f8a20-155">Skala hello program</span><span class="sxs-lookup"><span data-stu-id="f8a20-155">Scale hello application</span></span>

<span data-ttu-id="f8a20-156">Som standard `cf push` skapar endast en instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="f8a20-156">By default, `cf push` only creates a single instance of your application.</span></span> <span data-ttu-id="f8a20-157">tooensure hög tillgänglighet och aktivera skala ut för högre genomströmning, vanligtvis du toorun mer än en instans av dina program.</span><span class="sxs-lookup"><span data-stu-id="f8a20-157">tooensure high availability and enable scale out for higher throughput, you generally want toorun more than one instance of your applications.</span></span> <span data-ttu-id="f8a20-158">Du kan enkelt skala ut redan distribuerade program med hjälp av hello `scale` kommando:</span><span class="sxs-lookup"><span data-stu-id="f8a20-158">You can easily scale out already deployed applications using hello `scale` command:</span></span>

```bash
cf scale -i 2 hello-spring-cloud
```

<span data-ttu-id="f8a20-159">Körs hello `cf app` kommandot på hello program visar att molnet Foundry skapar en annan instans av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f8a20-159">Running hello `cf app` command on hello application shows that Cloud Foundry is creating another instance of hello application.</span></span> <span data-ttu-id="f8a20-160">När programmet hello har börjat startar automatiskt moln Foundry trafik tooit för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="f8a20-160">Once hello application has started, Cloud Foundry automatically starts load balancing traffic tooit.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f8a20-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8a20-161">Next steps</span></span>

- <span data-ttu-id="f8a20-162">[Läs hello molnet Foundry-dokumentation][cloudfoundry-docs]</span><span class="sxs-lookup"><span data-stu-id="f8a20-162">[Read hello Cloud Foundry documentation][cloudfoundry-docs]</span></span>
- <span data-ttu-id="f8a20-163">[Konfigurera hello Visual Studio Team Services plugin-programmet för molnet Foundry][vsts-plugin]</span><span class="sxs-lookup"><span data-stu-id="f8a20-163">[Set up hello Visual Studio Team Services plugin for Cloud Foundry][vsts-plugin]</span></span>
- <span data-ttu-id="f8a20-164">[Konfigurera hello Microsoft Log Analytics munstycket för molnet Foundry][loganalytics-nozzle]</span><span class="sxs-lookup"><span data-stu-id="f8a20-164">[Configure hello Microsoft Log Analytics Nozzle for Cloud Foundry][loganalytics-nozzle]</span></span>

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: http://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png