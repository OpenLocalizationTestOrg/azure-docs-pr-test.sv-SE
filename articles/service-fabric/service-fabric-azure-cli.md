---
title: "aaaGetting igång med Azure Service Fabric XPlat CLI"
description: "Komma igång med Azure Service Fabric XPlat CLI"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: c3ec8ff3-3b78-420c-a7ea-0c5e443fb10e
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: e4baa30536b4d8668d8efad301ed8210eb9c0335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-xplat-cli-toointeract-with-a-service-fabric-cluster"></a><span data-ttu-id="0ca6f-103">Hej XPlat CLI toointeract med ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="0ca6f-103">Using hello XPlat CLI toointeract with a Service Fabric cluster</span></span>

<span data-ttu-id="0ca6f-104">Du kan interagera med Service Fabric-kluster från Linux-datorer med hjälp av hello XPlat CLI på Linux.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-104">You can interact with Service Fabric cluster from Linux machines using hello XPlat CLI on Linux.</span></span>

<span data-ttu-id="0ca6f-105">hello första steget är att hämta hello senaste versionen av hello CLI från hello git rep och ange den i din sökväg med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-105">hello first step is get hello latest version of hello CLI from hello git rep and set it up in your path using hello following commands:</span></span>

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

<span data-ttu-id="0ca6f-106">Den stöder, du kan ange hello namn av hello kommandot tooobtain hello hjälp för kommandot för varje kommando.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-106">For each command, it supports, you can type hello name of hello command tooobtain hello help for that command.</span></span>
<span data-ttu-id="0ca6f-107">Automatisk komplettering stöds för hello-kommandon.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-107">Auto-completion is supported for hello commands.</span></span> <span data-ttu-id="0ca6f-108">Till exempel hello efter kommandot får du hjälp för alla kommandon som programmet hello.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-108">For example, hello following command gives you help for all hello application commands.</span></span> 

```sh
 azure servicefabric application 
```

<span data-ttu-id="0ca6f-109">Du kan filtrera hello hjälp tooa specifika kommandot ytterligare som följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-109">You can further filter hello help tooa specific command, as hello following example shows:</span></span>

```sh
 azure servicefabric application  create
```

<span data-ttu-id="0ca6f-110">tooenable automatisk komplettering i hello CLI kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-110">tooenable auto-completion in hello CLI, run hello following commands:</span></span>

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

<span data-ttu-id="0ca6f-111">Hej efter kommandona Anslut toohello klustret och visar hello av noderna i klustret hello:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-111">hello following commands connect toohello cluster and show you hello nodes in hello cluster:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

<span data-ttu-id="0ca6f-112">toouse namngivna parametrar och hitta vad de är, kan du skriva--hjälpa efter kommandot.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-112">toouse named parameters, and find what they are, you can type --help after a command.</span></span> <span data-ttu-id="0ca6f-113">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-113">For example:</span></span>

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

<span data-ttu-id="0ca6f-114">När du ansluter flera datorer tooa-kluster från en dator **som inte ingår i klustret hello**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-114">When connecting tooa multi-machine cluster from a machine **that is not part of hello cluster**, use hello following command:</span></span>

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

<span data-ttu-id="0ca6f-115">Ersätt hello PublicIPorFQDN taggen med hello verkliga IP eller FQDN efter behov.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-115">Replace hello PublicIPorFQDN tag with hello real IP or FQDN as appropriate.</span></span> <span data-ttu-id="0ca6f-116">När du ansluter flera datorer tooa-kluster från en dator **som ingår i klustret hello**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-116">When connecting tooa multi-machine cluster from a machine **that is part of hello cluster**, use hello following command:</span></span>

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

<span data-ttu-id="0ca6f-117">Du kan använda PowerShell eller CLI toointeract med Linux Service Fabric-kluster skapas med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-117">You can use PowerShell or CLI toointeract with your Linux Service Fabric Cluster created through hello Azure portal.</span></span>

> [!WARNING]
> <span data-ttu-id="0ca6f-118">Dessa kluster är inte säker, därför du kanske öppnar din som visas genom att lägga till hello offentliga IP-adressen i hello klustermanifestet.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-118">These clusters aren’t secure, thus, you may be opening up your one-box by adding hello public IP address in hello cluster manifest.</span></span>

## <a name="using-hello-xplat-cli-tooconnect-tooa-service-fabric-cluster"></a><span data-ttu-id="0ca6f-119">Med hjälp av hello XPlat CLI tooconnect tooa Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="0ca6f-119">Using hello XPlat CLI tooconnect tooa Service Fabric cluster</span></span>

<span data-ttu-id="0ca6f-120">hello följande Azure CLI-kommandona beskrivs hur tooconnect tooa skydda klustret.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-120">hello following Azure CLI commands describe how tooconnect tooa secure cluster.</span></span> <span data-ttu-id="0ca6f-121">information om hello certifikat måste matcha ett certifikat på hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-121">hello certificate details must match a certificate on hello cluster nodes.</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

<span data-ttu-id="0ca6f-122">Om ditt certifikat har certifikatutfärdare (CA), måste tooadd hello--ca-certifikat-sökvägsparameter som hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-122">If your certificate has Certificate Authorities (CAs), you need tooadd hello --ca-cert-path parameter like hello following example:</span></span> 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

<span data-ttu-id="0ca6f-123">Om du har flera certifikatutfärdare kan du använda ett kommatecken som hello avgränsare.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-123">If you have multiple CAs, use a comma as hello delimiter.</span></span>

<span data-ttu-id="0ca6f-124">Om ditt eget namn i hello certifikatet inte matchar hello Anslutningens slutpunkt, kan du använda parametern hello `--strict-ssl-false` toobypass hello verifieringen enligt hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-124">If your Common Name in hello certificate does not match hello connection endpoint, you could use hello parameter `--strict-ssl-false` toobypass hello verification as shown in hello following command:</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

<span data-ttu-id="0ca6f-125">Om du vill att tooskip hello CA verifiering, kan du lägga till hello--avvisa obehörig false parametern enligt hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-125">If you would like tooskip hello CA verification, you could add hello --reject-unauthorized-false parameter as shown in hello following command:</span></span> 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

<span data-ttu-id="0ca6f-126">När du har anslutit bör du kunna toorun toointeract andra CLI-kommandon med hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-126">After you connect, you should be able toorun other CLI commands toointeract with hello cluster.</span></span>

## <a name="deploying-your-service-fabric-application"></a><span data-ttu-id="0ca6f-127">Distribuera programmet Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0ca6f-127">Deploying your Service Fabric application</span></span>

<span data-ttu-id="0ca6f-128">Kör följande kommandon toocopy, registrera och starta hello service fabric programmet hello:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-128">Execute hello following commands toocopy, register, and start hello service fabric application:</span></span>

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a><span data-ttu-id="0ca6f-129">Uppgradera ditt program</span><span class="sxs-lookup"><span data-stu-id="0ca6f-129">Upgrading your application</span></span>

<span data-ttu-id="0ca6f-130">hello-processen är liknande toohello [processer i Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span><span class="sxs-lookup"><span data-stu-id="0ca6f-130">hello process is similar toohello [process in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span></span>

<span data-ttu-id="0ca6f-131">Skapa, kopiera, registrera och skapa programmet från projektet rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-131">Build, copy, register, and create your application from project root directory.</span></span> <span data-ttu-id="0ca6f-132">Om din instans av programmet heter `fabric:/MySFApp`, och hello typ är MySFApp, hello kommandon blir som följer:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-132">If your application instance is named `fabric:/MySFApp`, and hello type is MySFApp, hello commands would be as follows:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

<span data-ttu-id="0ca6f-133">Se hello ändra tooyour programmet och återskapa hello ändrade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-133">Make hello change tooyour application and rebuild hello modified service.</span></span>  <span data-ttu-id="0ca6f-134">Uppdateringen hello ändrade tjänstens manifestfil (ServiceManifest.xml) med hello uppdaterade versioner hello Service (och koden eller Config eller Data efter behov).</span><span class="sxs-lookup"><span data-stu-id="0ca6f-134">Update hello modified service’s manifest file (ServiceManifest.xml) with hello updated versions for hello Service (and Code or Config or Data as appropriate).</span></span> <span data-ttu-id="0ca6f-135">Även ändra hello programmets manifest (ApplicationManifest.xml) med hello uppdatera versionsnumret för programmet hello och hello ändrade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-135">Also modify hello application’s manifest (ApplicationManifest.xml) with hello updated version number for hello application, and hello modified service.</span></span>  <span data-ttu-id="0ca6f-136">Nu kan kopiera och registrera ditt uppdaterade program med hjälp av hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-136">Now, copy and register your updated application using hello following commands:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

<span data-ttu-id="0ca6f-137">Du kan nu starta uppgradering av programmet hello med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-137">Now, you can start hello application upgrade with hello following command:</span></span>

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

<span data-ttu-id="0ca6f-138">Nu kan du övervaka uppgradering av programmet hello med SFX.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-138">You can now monitor hello application upgrade using SFX.</span></span> <span data-ttu-id="0ca6f-139">Om några minuter skulle hello-programmet har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-139">In a few minutes, hello application would have been updated.</span></span> <span data-ttu-id="0ca6f-140">Du kan också prova en uppdaterad app med ett fel och kontrollera hello automatisk återställning i service fabric.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-140">You can also try an updated app with an error and check hello auto rollback functionality in service fabric.</span></span>

## <a name="converting-from-pfx-toopem-and-vice-versa"></a><span data-ttu-id="0ca6f-141">Konvertera från PFX tooPEM och vice versa</span><span class="sxs-lookup"><span data-stu-id="0ca6f-141">Converting from PFX tooPEM and vice versa</span></span>

<span data-ttu-id="0ca6f-142">Du kan behöva tooinstall ett certifikat i din lokala dator (med Windows eller Linux) tooaccess säker kluster som kan finnas i en annan miljö.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-142">You might need tooinstall a certificate in your local machine (with Windows or Linux) tooaccess secure clusters that may be in a different environment.</span></span> <span data-ttu-id="0ca6f-143">Till exempel vid åtkomst till en skyddad Linux-kluster från en Windows-dator och vice versa kanske du måste tooconvert ditt certifikat från PFX tooPEM och vice versa.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-143">For example, while accessing a secured Linux cluster from a Windows machine and vice versa you may need tooconvert your certificate from PFX tooPEM and vice versa.</span></span> 

<span data-ttu-id="0ca6f-144">tooconvert från en PEM-filen tooa PFX-fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-144">tooconvert from a PEM file tooa PFX file, use hello following command:</span></span>

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

<span data-ttu-id="0ca6f-145">tooconvert från en PFX-filen tooa PEM-fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-145">tooconvert from a PFX file tooa PEM file, use hello following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="0ca6f-146">Se för[OpenSSL dokumentationen](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) mer information.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-146">Refer too[OpenSSL documentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) for details.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a><span data-ttu-id="0ca6f-147">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0ca6f-147">Troubleshooting</span></span>


### <a name="copying-of-hello-application-package-does-not-succeed"></a><span data-ttu-id="0ca6f-148">Kopiering av hello programpaket lyckas inte</span><span class="sxs-lookup"><span data-stu-id="0ca6f-148">Copying of hello application package does not succeed</span></span>

<span data-ttu-id="0ca6f-149">Kontrollera om `openssh` är installerad.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-149">Check if `openssh` is installed.</span></span> <span data-ttu-id="0ca6f-150">Ubuntu Desktop har inte installerats.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-150">By default, Ubuntu Desktop doesn't have it installed.</span></span> <span data-ttu-id="0ca6f-151">Installera den med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-151">Install it using hello following command:</span></span>

```sh
sudo apt-get install openssh-server openssh-client**
```

<span data-ttu-id="0ca6f-152">Om hello problemet kvarstår kan du prova att inaktivera PAM för ssh genom att ändra hello `sshd_config` fil med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-152">If hello problem persists, try disabling PAM for ssh by changing hello `sshd_config` file using hello following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd_config
#Change hello line with UsePAM toohello following: UsePAM no
sudo service sshd reload
```

<span data-ttu-id="0ca6f-153">Om hello problemet kvarstår kan du försöka ökande hello antal ssh sessioner genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="0ca6f-153">If hello problem still persists, try increasing hello number of ssh sessions by executing hello following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd\_config
# Add hello following toolines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

<span data-ttu-id="0ca6f-154">Använder nycklar för ssh-autentisering (som skillnad från toopasswords) ännu stöds inte (eftersom hello-plattformen använder ssh toocopy paket), så Använd lösenordsautentisering i stället.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-154">Using keys for ssh authentication (as opposed toopasswords) isn't yet supported (since hello platform uses ssh toocopy packages), so use password authentication instead.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ca6f-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ca6f-155">Next steps</span></span>

[<span data-ttu-id="0ca6f-156">Ställ in hello utvecklingsmiljö och distribuera ett Service Fabric application tooa Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="0ca6f-156">Set up hello development environment and deploy a Service Fabric application tooa Linux cluster.</span></span>](service-fabric-get-started-linux.md)

## <a name="related-articles"></a><span data-ttu-id="0ca6f-157">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="0ca6f-157">Related articles</span></span>

* [<span data-ttu-id="0ca6f-158">Kom igång med Service Fabric och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0ca6f-158">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
