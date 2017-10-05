---
title: "Distribuera din första app till molnet Foundry på Microsoft Azure | Microsoft Docs"
description: "Distribuera program till molnet Foundry på Azure"
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
ms.openlocfilehash: b617127fc0a3f8dcae293e356ea669edcfa5deff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a><span data-ttu-id="5b158-103">Distribuera din första app till molnet Foundry på Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="5b158-103">Deploy your first app to Cloud Foundry on Microsoft Azure</span></span>

<span data-ttu-id="5b158-104">[Molnet Foundry](http://cloudfoundry.org) är en programplattform för populära öppen källkod tillgängliga på Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5b158-104">[Cloud Foundry](http://cloudfoundry.org) is a popular open-source application platform available on Microsoft Azure.</span></span> <span data-ttu-id="5b158-105">I den här artikeln visar vi hur du distribuerar och hanterar ett program på molnet Foundry i en Azure-miljö.</span><span class="sxs-lookup"><span data-stu-id="5b158-105">In this article, we show how to deploy and manage an application on Cloud Foundry in an Azure environment.</span></span>

## <a name="create-a-cloud-foundry-environment"></a><span data-ttu-id="5b158-106">Skapa en moln Foundry-miljö</span><span class="sxs-lookup"><span data-stu-id="5b158-106">Create a Cloud Foundry environment</span></span>

<span data-ttu-id="5b158-107">Det finns flera alternativ för att skapa en miljö med molnet Foundry i Azure:</span><span class="sxs-lookup"><span data-stu-id="5b158-107">There are several options for creating a Cloud Foundry environment on Azure:</span></span>

- <span data-ttu-id="5b158-108">Använd den [spela en central molnet Foundry erbjudande] [ pcf-azuremarketplace] i Azure Marketplace för att skapa en standard miljö som innehåller PCF Ops Manager och Azure Service Broker.</span><span class="sxs-lookup"><span data-stu-id="5b158-108">Use the [Pivotal Cloud Foundry offer][pcf-azuremarketplace] in the Azure Marketplace to create a standard environment that includes PCF Ops Manager and the Azure Service Broker.</span></span> <span data-ttu-id="5b158-109">Du kan hitta [fullständiga] [ pcf-azuremarketplace-pivotaldocs] för att distribuera marketplace erbjuder i spela en central-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="5b158-109">You can find [complete instructions][pcf-azuremarketplace-pivotaldocs] for deploying the marketplace offer in the Pivotal documentation.</span></span>
- <span data-ttu-id="5b158-110">Skapa en anpassad miljö genom [manuellt distribuera spela en central molnet Foundry][pcf-custom].</span><span class="sxs-lookup"><span data-stu-id="5b158-110">Create a customized environment by [deploying Pivotal Cloud Foundry manually][pcf-custom].</span></span>
- <span data-ttu-id="5b158-111">[Distribuera paket för öppen källkod molnet Foundry direkt] [ oss-cf-bosh] genom att ställa in en [BOSH](http://bosh.io) chef, en virtuell dator som samordnar distributionen av molnet Foundry-miljö.</span><span class="sxs-lookup"><span data-stu-id="5b158-111">[Deploy the open-source Cloud Foundry packages directly][oss-cf-bosh] by setting up a [BOSH](http://bosh.io) director, a VM that coordinates the deployment of the Cloud Foundry environment.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="5b158-112">Om du distribuerar PCF från Azure Marketplace, notera SYSTEMDOMAINURL och administratörsautentiseringsuppgifter för att komma åt hanteraren spela en central appar som beskrivs i distributionsguiden marketplace.</span><span class="sxs-lookup"><span data-stu-id="5b158-112">If you are deploying PCF from the Azure Marketplace, make a note of the SYSTEMDOMAINURL and the admin credentials required to access the Pivotal Apps Manager, both of which are described in the marketplace deployment guide.</span></span> <span data-ttu-id="5b158-113">De behövs för att slutföra den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="5b158-113">They are needed to complete this tutorial.</span></span> <span data-ttu-id="5b158-114">Marketplace-distributioner är SYSTEMDOMAINURL i formuläret https://system. *ip-adress*. cf.pcfazure.com.</span><span class="sxs-lookup"><span data-stu-id="5b158-114">For marketplace deployments, the SYSTEMDOMAINURL is in the form https://system.*ip-address*.cf.pcfazure.com.</span></span>

## <a name="connect-to-the-cloud-controller"></a><span data-ttu-id="5b158-115">Ansluta till molnet-styrenhet</span><span class="sxs-lookup"><span data-stu-id="5b158-115">Connect to the Cloud Controller</span></span>

<span data-ttu-id="5b158-116">Molnet domänkontrollanten är den primära startpunkten till en molnet Foundry miljö för att distribuera och hantera program.</span><span class="sxs-lookup"><span data-stu-id="5b158-116">The Cloud Controller is the primary entry point to a Cloud Foundry environment for deploying and managing applications.</span></span> <span data-ttu-id="5b158-117">Core molnet Controller API (CCAPI) är en REST-API, men den är tillgänglig via olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="5b158-117">The core Cloud Controller API (CCAPI) is a REST API, but it is accessible through various tools.</span></span> <span data-ttu-id="5b158-118">I det här fallet vi interagerar med det via den [moln Foundry CLI][cf-cli].</span><span class="sxs-lookup"><span data-stu-id="5b158-118">In this case, we interact with it through the [Cloud Foundry CLI][cf-cli].</span></span> <span data-ttu-id="5b158-119">Du kan installera CLI på Linux, MacOS eller Windows, men om du inte vill installera den på alla, den är tillgänglig förinstallerat i den [Azure Cloud Shell][cloudshell-docs].</span><span class="sxs-lookup"><span data-stu-id="5b158-119">You can install the CLI on Linux, MacOS, or Windows, but if you'd prefer not to install it at all, it is available pre-installed in the [Azure Cloud Shell][cloudshell-docs].</span></span>

<span data-ttu-id="5b158-120">För att logga in lägga `api` till SYSTEMDOMAINURL som du fick från marketplace-distribution.</span><span class="sxs-lookup"><span data-stu-id="5b158-120">To log in, prepend `api` to the SYSTEMDOMAINURL that you obtained from the marketplace deployment.</span></span> <span data-ttu-id="5b158-121">Eftersom standard-distributionen använder ett självsignerat certifikat, bör du också inkludera den `skip-ssl-validation` växla.</span><span class="sxs-lookup"><span data-stu-id="5b158-121">Since the default deployment uses a self-signed certificate, you should also include the `skip-ssl-validation` switch.</span></span>

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

<span data-ttu-id="5b158-122">Du uppmanas att logga in på molnet-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="5b158-122">You are prompted to log in to the Cloud Controller.</span></span> <span data-ttu-id="5b158-123">Använd administratörsautentiseringsuppgifter för kontot som du har köpt från marketplace-distributionsstegen.</span><span class="sxs-lookup"><span data-stu-id="5b158-123">Use the admin account credentials that you acquired from the marketplace deployment steps.</span></span>

<span data-ttu-id="5b158-124">Molnet Foundry ger *organisationer* och *blanksteg* som namnområden att isolera team och miljöer i en delad distribution.</span><span class="sxs-lookup"><span data-stu-id="5b158-124">Cloud Foundry provides *orgs* and *spaces* as namespaces to isolate the teams and environments within a shared deployment.</span></span> <span data-ttu-id="5b158-125">PCF marketplace distributionen omfattar standard *system* org och en uppsättning blanksteg som innehåller de grundläggande komponenterna som tjänsten autoskalning och Azure service broker.</span><span class="sxs-lookup"><span data-stu-id="5b158-125">The PCF marketplace deployment includes the default *system* org and a set of spaces created to contain the base components, like the autoscaling service and the Azure service broker.</span></span> <span data-ttu-id="5b158-126">Nu välja de *system* utrymme.</span><span class="sxs-lookup"><span data-stu-id="5b158-126">For now, choose the *system* space.</span></span>


## <a name="create-an-org-and-space"></a><span data-ttu-id="5b158-127">Skapa en organisations och utrymme</span><span class="sxs-lookup"><span data-stu-id="5b158-127">Create an org and space</span></span>

<span data-ttu-id="5b158-128">Om du skriver `cf apps`, visas en uppsättning systemprogram som har distribuerats i området system inom organisationen system.</span><span class="sxs-lookup"><span data-stu-id="5b158-128">If you type `cf apps`, you see a set of system applications that have been deployed in the system space within the system org.</span></span> 

<span data-ttu-id="5b158-129">Du bör tänka på *system* org som reserverats för systemprogram, så du måste skapa en organisations och utrymme för våra exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5b158-129">You should keep the *system* org reserved for system applications, so create an org and space to house our sample application.</span></span>

```bash
cf create-org myorg
cf create-space dev -o myorg
```

<span data-ttu-id="5b158-130">Använd Målkommandot för att växla till den nya org och utrymme:</span><span class="sxs-lookup"><span data-stu-id="5b158-130">Use the target command to switch to the new org and space:</span></span>

```bash
cf target -o testorg -s dev
```

<span data-ttu-id="5b158-131">Nu när du distribuerar ett program kan skapas det automatiskt i nya org och blanksteg.</span><span class="sxs-lookup"><span data-stu-id="5b158-131">Now, when you deploy an application, it is automatically created in the new org and space.</span></span> <span data-ttu-id="5b158-132">För att bekräfta att det finns för närvarande inga appar i det nya org/utrymmet, Skriv `cf apps` igen.</span><span class="sxs-lookup"><span data-stu-id="5b158-132">To confirm that there are currently no apps in the new org/space, type `cf apps` again.</span></span>

> [!NOTE] 
> <span data-ttu-id="5b158-133">Mer information om organisationer blanksteg och hur de kan användas för rollbaserad åtkomstkontroll (RBAC) finns på [moln Foundry dokumentationen][cf-orgs-spaces-docs].</span><span class="sxs-lookup"><span data-stu-id="5b158-133">For more information about orgs and spaces and how they can be used for role-based access control (RBAC), see the [Cloud Foundry documentation][cf-orgs-spaces-docs].</span></span>

## <a name="deploy-an-application"></a><span data-ttu-id="5b158-134">Distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="5b158-134">Deploy an application</span></span>

<span data-ttu-id="5b158-135">Vi använder ett exempelprogram för molntjänster Foundry kallas Hello källan moln, vilket är skriven i Java och baserat på de [Vårversionen Framework](http://spring.io) och [Vårversionen Start](http://projects.spring.io/spring-boot/).</span><span class="sxs-lookup"><span data-stu-id="5b158-135">Let's use a sample Cloud Foundry application called Hello Spring Cloud, which is written in Java and based on the [Spring Framework](http://spring.io) and [Spring Boot](http://projects.spring.io/spring-boot/).</span></span>

### <a name="clone-the-hello-spring-cloud-repository"></a><span data-ttu-id="5b158-136">Klona lagringsplatsen Hello källan moln</span><span class="sxs-lookup"><span data-stu-id="5b158-136">Clone the Hello Spring Cloud repository</span></span>

<span data-ttu-id="5b158-137">Exempelprogrammet Hello källan molnet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="5b158-137">The Hello Spring Cloud sample application is available on GitHub.</span></span> <span data-ttu-id="5b158-138">Klona den i din miljö och ändra till den nya katalogen:</span><span class="sxs-lookup"><span data-stu-id="5b158-138">Clone it to your environment and change into the new directory:</span></span>

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a><span data-ttu-id="5b158-139">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="5b158-139">Build the application</span></span>

<span data-ttu-id="5b158-140">Skapa en app med hjälp av [Apache Maven](http://maven.apache.org).</span><span class="sxs-lookup"><span data-stu-id="5b158-140">Build the app using [Apache Maven](http://maven.apache.org).</span></span>

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a><span data-ttu-id="5b158-141">Distribuera programmet med cf push</span><span class="sxs-lookup"><span data-stu-id="5b158-141">Deploy the application with cf push</span></span>

<span data-ttu-id="5b158-142">Du kan distribuera de flesta program till molnet Foundry med hjälp av den `push` kommando:</span><span class="sxs-lookup"><span data-stu-id="5b158-142">You can deploy most applications to Cloud Foundry using the `push` command:</span></span>

```bash
cf push
```

<span data-ttu-id="5b158-143">När du *push* ett program, molnet Foundry identifierar typ av program (i det här fallet en Java-app) och identifierar dess beroenden (i det här fallet källan framework).</span><span class="sxs-lookup"><span data-stu-id="5b158-143">When you *push* an application, Cloud Foundry detects the type of application (in this case, a Java app) and identifies its dependencies (in this case, the Spring framework).</span></span> <span data-ttu-id="5b158-144">Den sedan paket allt som krävs för att köra din kod i en fristående behållaren avbildningen som kallas en *droplet*.</span><span class="sxs-lookup"><span data-stu-id="5b158-144">It then packages everything required to run your code into a standalone container image, known as a *droplet*.</span></span> <span data-ttu-id="5b158-145">Slutligen molnet Foundry schemalägger program på en av de tillgängliga datorerna i din miljö och skapar en URL där du kan nå den, som är tillgängliga i resultatet av kommandot.</span><span class="sxs-lookup"><span data-stu-id="5b158-145">Finally, Cloud Foundry schedules the application on one of the available machines in your environment and creates a URL where you can reach it, which is available in the output of the command.</span></span>

![Utdata från cf push-kommando][cf-push-output]

<span data-ttu-id="5b158-147">Öppna den angivna Webbadressen i webbläsaren om du vill se programmet hello källan moln:</span><span class="sxs-lookup"><span data-stu-id="5b158-147">To see the hello-spring-cloud application, open the provided URL in your browser:</span></span>

![Standardgränssnitt för Hello källan moln][hello-spring-cloud-basic]

> [!NOTE] 
> <span data-ttu-id="5b158-149">Mer information om vad som händer under `cf push`, se [hur program mellanlagras] [ cf-push-docs] i molnet Foundry-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="5b158-149">To learn more about what happens during `cf push`, see [How Applications Are Staged][cf-push-docs] in the Cloud Foundry documentation.</span></span>

## <a name="view-application-logs"></a><span data-ttu-id="5b158-150">Visa programloggar</span><span class="sxs-lookup"><span data-stu-id="5b158-150">View application logs</span></span>

<span data-ttu-id="5b158-151">Du kan använda molnet Foundry CLI för att visa loggar för ett program med namnet:</span><span class="sxs-lookup"><span data-stu-id="5b158-151">You can use the Cloud Foundry CLI to view logs for an application by its name:</span></span>

```bash
cf logs hello-spring-cloud
```

<span data-ttu-id="5b158-152">Som standard loggar kommandot använder *pilslut*, vilket visar nya loggar som de är skrivna.</span><span class="sxs-lookup"><span data-stu-id="5b158-152">By default, the logs command uses *tail*, which shows new logs as they are written.</span></span> <span data-ttu-id="5b158-153">Uppdatera hello-källan-cloud-app i webbläsaren om du vill se nya loggar visas.</span><span class="sxs-lookup"><span data-stu-id="5b158-153">To see new logs appear, refresh the hello-spring-cloud app in the browser.</span></span>

<span data-ttu-id="5b158-154">Lägg till om du vill visa loggar som redan har skrivits av `recent` växel:</span><span class="sxs-lookup"><span data-stu-id="5b158-154">To view logs that have already been written, add the `recent` switch:</span></span>

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a><span data-ttu-id="5b158-155">Skala programmet</span><span class="sxs-lookup"><span data-stu-id="5b158-155">Scale the application</span></span>

<span data-ttu-id="5b158-156">Som standard `cf push` skapar endast en instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="5b158-156">By default, `cf push` only creates a single instance of your application.</span></span> <span data-ttu-id="5b158-157">För att säkerställa hög tillgänglighet och aktivera skala ut för högre genomströmning, vill du vanligtvis köra fler än en instans av dina program.</span><span class="sxs-lookup"><span data-stu-id="5b158-157">To ensure high availability and enable scale out for higher throughput, you generally want to run more than one instance of your applications.</span></span> <span data-ttu-id="5b158-158">Du kan enkelt skala ut redan distribuerade program med hjälp av den `scale` kommando:</span><span class="sxs-lookup"><span data-stu-id="5b158-158">You can easily scale out already deployed applications using the `scale` command:</span></span>

```bash
cf scale -i 2 hello-spring-cloud
```

<span data-ttu-id="5b158-159">Kör den `cf app` kommandot på programmet visar att molnet Foundry skapar en annan instans av programmet.</span><span class="sxs-lookup"><span data-stu-id="5b158-159">Running the `cf app` command on the application shows that Cloud Foundry is creating another instance of the application.</span></span> <span data-ttu-id="5b158-160">När programmet har börjat startar automatiskt moln Foundry belastningsutjämning trafik till den.</span><span class="sxs-lookup"><span data-stu-id="5b158-160">Once the application has started, Cloud Foundry automatically starts load balancing traffic to it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5b158-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b158-161">Next steps</span></span>

- <span data-ttu-id="5b158-162">[Läs i dokumentationen för molnet Foundry][cloudfoundry-docs]</span><span class="sxs-lookup"><span data-stu-id="5b158-162">[Read the Cloud Foundry documentation][cloudfoundry-docs]</span></span>
- <span data-ttu-id="5b158-163">[Konfigurera Visual Studio Team Services plugin-programmet för molnet Foundry][vsts-plugin]</span><span class="sxs-lookup"><span data-stu-id="5b158-163">[Set up the Visual Studio Team Services plugin for Cloud Foundry][vsts-plugin]</span></span>
- <span data-ttu-id="5b158-164">[Konfigurera Microsoft Log Analytics munstycket för molnet Foundry][loganalytics-nozzle]</span><span class="sxs-lookup"><span data-stu-id="5b158-164">[Configure the Microsoft Log Analytics Nozzle for Cloud Foundry][loganalytics-nozzle]</span></span>

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