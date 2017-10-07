---
title: "aaaGet igång med Azure Service Fabric CLI (sfctl)"
description: "Lär dig hur toouse hello Azure Service Fabric CLI. Lär dig hur tooconnect tooa klustret, och hur toomanage program."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: f76e8ff65bb38dfb63791da0a23e19b93b337f6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-command-line"></a><span data-ttu-id="0788b-104">Azure Service Fabric-kommandorad</span><span class="sxs-lookup"><span data-stu-id="0788b-104">Azure Service Fabric command line</span></span>

<span data-ttu-id="0788b-105">hello Azure Service Fabric CLI (sfctl) är ett kommandoradsverktyg för interagerar och hantera Azure Service Fabric-enheter.</span><span class="sxs-lookup"><span data-stu-id="0788b-105">hello Azure Service Fabric CLI (sfctl) is a command-line utility for interacting and managing Azure Service Fabric entities.</span></span> <span data-ttu-id="0788b-106">Sfctl kan användas med Windows- eller Linux-kluster.</span><span class="sxs-lookup"><span data-stu-id="0788b-106">Sfctl can be used with either Windows or Linux clusters.</span></span> <span data-ttu-id="0788b-107">Sfctl fungerar på alla plattformar där python stöds.</span><span class="sxs-lookup"><span data-stu-id="0788b-107">Sfctl runs on any platform where python is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0788b-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0788b-108">Prerequisites</span></span>

<span data-ttu-id="0788b-109">Tidigare tooinstallation, kontrollera att din miljö innehåller både python och pip-installeras.</span><span class="sxs-lookup"><span data-stu-id="0788b-109">Prior tooinstallation, make sure your environment has both python and pip installed.</span></span> <span data-ttu-id="0788b-110">Mer information, ta en titt på hello [pip quickstart dokumentationen](https://pip.pypa.io/en/latest/quickstart/), och det officiella [python installera dokumentationen](https://wiki.python.org/moin/BeginnersGuide/Download).</span><span class="sxs-lookup"><span data-stu-id="0788b-110">For more information, take a look at hello [pip quickstart documentation](https://pip.pypa.io/en/latest/quickstart/), and official [python install documentation](https://wiki.python.org/moin/BeginnersGuide/Download).</span></span>

<span data-ttu-id="0788b-111">När båda python 2.7 och 3,6 stöds rekommenderas toouse python 3,6.</span><span class="sxs-lookup"><span data-stu-id="0788b-111">While both python 2.7 and 3.6 are supported, it is recommended toouse python 3.6.</span></span>

## <a name="install"></a><span data-ttu-id="0788b-112">Installera</span><span class="sxs-lookup"><span data-stu-id="0788b-112">Install</span></span>

<span data-ttu-id="0788b-113">hello Azure Service Fabric CLI (sfctl) levereras som en python-paket.</span><span class="sxs-lookup"><span data-stu-id="0788b-113">hello Azure Service Fabric CLI (sfctl) is packaged as a python package.</span></span> <span data-ttu-id="0788b-114">tooinstall hello senaste versionen körs:</span><span class="sxs-lookup"><span data-stu-id="0788b-114">tooinstall hello latest version run:</span></span>

```bash
pip install sfctl
```

<span data-ttu-id="0788b-115">Efter installation kör `sfctl -h` tooget information om tillgängliga kommandon.</span><span class="sxs-lookup"><span data-stu-id="0788b-115">After installation, run `sfctl -h` tooget information about available commands.</span></span>

## <a name="cli-syntax"></a><span data-ttu-id="0788b-116">CLI-syntax</span><span class="sxs-lookup"><span data-stu-id="0788b-116">CLI syntax</span></span>

<span data-ttu-id="0788b-117">Kommandon har alltid prefixet `sfctl`.</span><span class="sxs-lookup"><span data-stu-id="0788b-117">Commands are prefixed always with `sfctl`.</span></span> <span data-ttu-id="0788b-118">För allmän information om alla kommandon som du kan använda, använd `sfctl -h`.</span><span class="sxs-lookup"><span data-stu-id="0788b-118">For general information about all commands you can use, use `sfctl -h`.</span></span> <span data-ttu-id="0788b-119">För hjälp med ett enda kommando, använd `sfctl <command> -h`.</span><span class="sxs-lookup"><span data-stu-id="0788b-119">For help with a single command, use `sfctl <command> -h`.</span></span>

<span data-ttu-id="0788b-120">Kommandon Följ en repeterbara struktur med hello målet för hello kommandot föregående hello verb eller åtgärd:</span><span class="sxs-lookup"><span data-stu-id="0788b-120">Commands follow a repeatable structure, with hello target of hello command preceding hello verb or action:</span></span>

```azurecli
sfctl <object> <action>
```

<span data-ttu-id="0788b-121">I det här exemplet `<object>` är hello mål för `<action>`.</span><span class="sxs-lookup"><span data-stu-id="0788b-121">In this example, `<object>` is hello target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="0788b-122">Välj ett kluster</span><span class="sxs-lookup"><span data-stu-id="0788b-122">Select a cluster</span></span>

<span data-ttu-id="0788b-123">Du måste välja en kluster-tooconnect till innan du utför några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0788b-123">Before you perform any operations, you must select a cluster tooconnect to.</span></span> <span data-ttu-id="0788b-124">Till exempel kör hello följande tooselect och ansluta toohello kluster med namnet hello `testcluster.com`.</span><span class="sxs-lookup"><span data-stu-id="0788b-124">For example, run hello following tooselect and connect toohello cluster with hello name `testcluster.com`.</span></span>

> [!WARNING]
> <span data-ttu-id="0788b-125">Använd inte oskyddade Service Fabric-kluster i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="0788b-125">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="0788b-126">Hej klusterslutpunkten måste föregås av `http` eller `https`.</span><span class="sxs-lookup"><span data-stu-id="0788b-126">hello cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="0788b-127">Det måste innehålla hello port för hello HTTP-gateway.</span><span class="sxs-lookup"><span data-stu-id="0788b-127">It must include hello port for hello HTTP gateway.</span></span> <span data-ttu-id="0788b-128">hello porten och adressen är hello samtidigt som hello Service Fabric Explorer-URL.</span><span class="sxs-lookup"><span data-stu-id="0788b-128">hello port and address are hello same as hello Service Fabric Explorer URL.</span></span>

<span data-ttu-id="0788b-129">För kluster som skyddas med ett certifikat kan du ange ett PEM-kodat certifikat.</span><span class="sxs-lookup"><span data-stu-id="0788b-129">For clusters that are secured with a certificate, you can specify a PEM encoded certificate.</span></span> <span data-ttu-id="0788b-130">hello certifikat kan anges som en enda fil eller ett certifikat och nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="0788b-130">hello certificate can be specified as a single file or cert and key pair.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="0788b-131">Mer information finns i [Anslut tooa säker Azure Service Fabric-kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="0788b-131">For more information, see [Connect tooa secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="basic-operations"></a><span data-ttu-id="0788b-132">Grundläggande åtgärder</span><span class="sxs-lookup"><span data-stu-id="0788b-132">Basic operations</span></span>

<span data-ttu-id="0788b-133">Klustrets anslutningsinformation bevaras mellan olika sfctl-sessioner.</span><span class="sxs-lookup"><span data-stu-id="0788b-133">Cluster connection information persists across multiple sfctl sessions.</span></span> <span data-ttu-id="0788b-134">När du har valt ett Service Fabric-kluster kan du köra ett Service Fabric-kommando på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0788b-134">After you select a Service Fabric cluster, you can run any Service Fabric command on hello cluster.</span></span>

<span data-ttu-id="0788b-135">Till exempel använda tooget hello hälsotillstånd för Service Fabric-kluster, hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0788b-135">For example, tooget hello Service Fabric cluster health state, use hello following command:</span></span>

```azurecli
sfctl cluster health
```

<span data-ttu-id="0788b-136">hello kommandot resulterar i hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="0788b-136">hello command results in hello following output:</span></span>

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="0788b-137">Felsökning och tips</span><span class="sxs-lookup"><span data-stu-id="0788b-137">Tips and troubleshooting</span></span>

<span data-ttu-id="0788b-138">Några förslag och tips för att lösa vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="0788b-138">Some suggestions and tips for solving common issues.</span></span>

### <a name="convert-a-certificate-from-pfx-toopem-format"></a><span data-ttu-id="0788b-139">Konvertera ett certifikat från tooPEM PFX-format</span><span class="sxs-lookup"><span data-stu-id="0788b-139">Convert a certificate from PFX tooPEM format</span></span>

<span data-ttu-id="0788b-140">hello Service Fabric CLI stöder klientens certifikat som PEM-filer (.pem tillägg).</span><span class="sxs-lookup"><span data-stu-id="0788b-140">hello Service Fabric CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="0788b-141">Om du använder PFX-filer från Windows, måste du konvertera dessa certifikat tooPEM format.</span><span class="sxs-lookup"><span data-stu-id="0788b-141">If you use PFX files from Windows, you must convert those certificates tooPEM format.</span></span> <span data-ttu-id="0788b-142">tooconvert en PFX-filen tooa PEM-fil, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0788b-142">tooconvert a PFX file tooa PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="0788b-143">Mer information finns i hello [OpenSSL dokumentationen](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="0788b-143">For more information, see hello [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="0788b-144">Anslutningsproblem</span><span class="sxs-lookup"><span data-stu-id="0788b-144">Connection issues</span></span>

<span data-ttu-id="0788b-145">Vissa åtgärder kan generera hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="0788b-145">Some operations might generate hello following message:</span></span>

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="0788b-146">Kontrollera att hello angett klusterslutpunkt är tillgänglig och lyssnar.</span><span class="sxs-lookup"><span data-stu-id="0788b-146">Verify that hello specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="0788b-147">Kontrollera också att hello Service Fabric Explorer Användargränssnittet är tillgänglig på som värd och port.</span><span class="sxs-lookup"><span data-stu-id="0788b-147">Also, verify that hello Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="0788b-148">tooupdate hello slutpunkt och Använd `sfctl cluster select`.</span><span class="sxs-lookup"><span data-stu-id="0788b-148">tooupdate hello endpoint, use `sfctl cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="0788b-149">Detaljerade loggar</span><span class="sxs-lookup"><span data-stu-id="0788b-149">Detailed logs</span></span>

<span data-ttu-id="0788b-150">Detaljerade loggar är ofta användbara när du felsöker eller rapporterar problem.</span><span class="sxs-lookup"><span data-stu-id="0788b-150">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="0788b-151">Det finns en global `--debug` flagga som ökar hello detaljnivå loggfiler.</span><span class="sxs-lookup"><span data-stu-id="0788b-151">There is a global `--debug` flag that increases hello verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="0788b-152">Hjälp och syntax för kommandon</span><span class="sxs-lookup"><span data-stu-id="0788b-152">Command help and syntax</span></span>

<span data-ttu-id="0788b-153">Hjälp med ett visst kommando eller en grupp med kommandon för att använda hello `-h` flaggan:</span><span class="sxs-lookup"><span data-stu-id="0788b-153">For help with a specific command or a group of commands, use hello `-h` flag:</span></span>

```azurecli
sfctl application -h
```

<span data-ttu-id="0788b-154">Ett annat exempel:</span><span class="sxs-lookup"><span data-stu-id="0788b-154">Another example:</span></span>

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a><span data-ttu-id="0788b-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0788b-155">Next steps</span></span>

* [<span data-ttu-id="0788b-156">Distribuera ett program med hello Azure Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="0788b-156">Deploy an application with hello Azure Service Fabric CLI</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="0788b-157">Kom igång med Service Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="0788b-157">Get started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
