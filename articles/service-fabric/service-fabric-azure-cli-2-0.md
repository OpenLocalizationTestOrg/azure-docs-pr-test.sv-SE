---
title: "aaaGet igång med Azure Service Fabric och Azure CLI 2.0"
description: "Lär dig hur toouse hello Azure Service Fabric-kommandon modul i Azure CLI version 2.0. Lär dig hur tooconnect tooa klustret, och hur toomanage program."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ddbd0ef503dd3fff61494cc2cfa7c9a2e8d0a9a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-and-azure-cli-20"></a><span data-ttu-id="ced87-104">Service Fabric och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ced87-104">Azure Service Fabric and Azure CLI 2.0</span></span>

<span data-ttu-id="ced87-105">hello Azure kommandoradsverktyget (Azure CLI) version 2.0 innehåller kommandon toohelp som du hanterar Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="ced87-105">hello Azure command-line tool (Azure CLI) version 2.0 includes commands toohelp you manage Azure Service Fabric clusters.</span></span> <span data-ttu-id="ced87-106">Lär dig hur tooget igång med Azure CLI och Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ced87-106">Learn how tooget started with Azure CLI and Service Fabric.</span></span>

## <a name="install-azure-cli-20"></a><span data-ttu-id="ced87-107">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ced87-107">Install Azure CLI 2.0</span></span>

<span data-ttu-id="ced87-108">Du kan använda Azure CLI 2.0 kommandon toointeract med och hantera Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="ced87-108">You can use Azure CLI 2.0 commands toointeract with and manage Service Fabric clusters.</span></span> <span data-ttu-id="ced87-109">tooget hello senaste versionen av Azure CLI, följ hello [Azure CLI 2.0 standardinstallation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ced87-109">tooget hello latest version of Azure CLI, follow hello [Azure CLI 2.0 standard installation process](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="ced87-110">Mer information finns i hello [översikt över Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ced87-110">For more information, see hello [Azure CLI 2.0 overview](https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="azure-cli-syntax"></a><span data-ttu-id="ced87-111">Azure CLI-syntax</span><span class="sxs-lookup"><span data-stu-id="ced87-111">Azure CLI syntax</span></span>

<span data-ttu-id="ced87-112">Alla Service Fabric-kommandon har prefixet `az sf` i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ced87-112">In Azure CLI, all Service Fabric commands are prefixed with `az sf`.</span></span> <span data-ttu-id="ced87-113">Allmän information om hello-kommandon som du kan använda använda `az sf -h`.</span><span class="sxs-lookup"><span data-stu-id="ced87-113">For general information about hello commands you can use, use `az sf -h`.</span></span> <span data-ttu-id="ced87-114">För hjälp med ett enda kommando, använd `az sf <command> -h`.</span><span class="sxs-lookup"><span data-stu-id="ced87-114">For help with a single command, use `az sf <command> -h`.</span></span>

<span data-ttu-id="ced87-115">Service Fabric-kommandon i CLI följer detta namngivningsmönster:</span><span class="sxs-lookup"><span data-stu-id="ced87-115">Service Fabric commands in Azure CLI follow this naming pattern:</span></span>

```azurecli
az sf <object> <action>
```

<span data-ttu-id="ced87-116">`<object>`är hello mål för `<action>`.</span><span class="sxs-lookup"><span data-stu-id="ced87-116">`<object>` is hello target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="ced87-117">Välj ett kluster</span><span class="sxs-lookup"><span data-stu-id="ced87-117">Select a cluster</span></span>

<span data-ttu-id="ced87-118">Du måste välja en kluster-tooconnect till innan du utför några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ced87-118">Before you perform any operations, you must select a cluster tooconnect to.</span></span> <span data-ttu-id="ced87-119">Ett exempel finns i hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="ced87-119">For an example, see hello following code.</span></span> <span data-ttu-id="ced87-120">hello koden ansluter tooan oskyddade kluster.</span><span class="sxs-lookup"><span data-stu-id="ced87-120">hello code connects tooan unsecured cluster.</span></span>

> [!WARNING]
> <span data-ttu-id="ced87-121">Använd inte oskyddade Service Fabric-kluster i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="ced87-121">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="ced87-122">Hej klusterslutpunkten måste föregås av `http` eller `https`.</span><span class="sxs-lookup"><span data-stu-id="ced87-122">hello cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="ced87-123">Det måste innehålla hello port för hello HTTP-gateway.</span><span class="sxs-lookup"><span data-stu-id="ced87-123">It must include hello port for hello HTTP gateway.</span></span> <span data-ttu-id="ced87-124">hello porten och adressen är hello samtidigt som hello Service Fabric Explorer-URL.</span><span class="sxs-lookup"><span data-stu-id="ced87-124">hello port and address are hello same as hello Service Fabric Explorer URL.</span></span>

<span data-ttu-id="ced87-125">Du kan antingen använda okrypterade .pem-filer, eller .crt- och .key-filer, för kluster som är säkrade med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="ced87-125">For clusters that are secured with a certificate, you can use either unencrypted .pem files, or .crt and .key files.</span></span> <span data-ttu-id="ced87-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ced87-126">For example:</span></span>

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="ced87-127">Mer information finns i [Anslut tooa säker Azure Service Fabric-kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="ced87-127">For more information, see [Connect tooa secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

> [!NOTE]
> <span data-ttu-id="ced87-128">Hej `select` kommando inte fungerar på alla begäranden innan den returnerar.</span><span class="sxs-lookup"><span data-stu-id="ced87-128">hello `select` command doesn't act on any requests before it returns.</span></span> <span data-ttu-id="ced87-129">tooverify att du har angett ett kluster korrekt, använder du ett kommando som `az sf cluster health`.</span><span class="sxs-lookup"><span data-stu-id="ced87-129">tooverify that you've specified a cluster correctly, use a command like `az sf cluster health`.</span></span> <span data-ttu-id="ced87-130">Kontrollera att hello kommando inte returnerar några fel.</span><span class="sxs-lookup"><span data-stu-id="ced87-130">Verify that hello command doesn't return any errors.</span></span>

## <a name="basic-operations"></a><span data-ttu-id="ced87-131">Grundläggande åtgärder</span><span class="sxs-lookup"><span data-stu-id="ced87-131">Basic operations</span></span>

<span data-ttu-id="ced87-132">Klustrets anslutningsinformation bevaras mellan olika Azure CLI-sessioner.</span><span class="sxs-lookup"><span data-stu-id="ced87-132">Cluster connection information persists across multiple Azure CLI sessions.</span></span> <span data-ttu-id="ced87-133">När du har valt ett Service Fabric-kluster kan du köra ett Service Fabric-kommando på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ced87-133">After you select a Service Fabric cluster, you can run any Service Fabric command on hello cluster.</span></span>

<span data-ttu-id="ced87-134">Till exempel använda tooget hello hälsotillstånd för Service Fabric-kluster, hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ced87-134">For example, tooget hello Service Fabric cluster health state, use hello following command:</span></span>

```azurecli
az sf cluster health
```

<span data-ttu-id="ced87-135">hello kommandot resulterar i hello följande utdata (förutsatt att JSON-utdata har angetts i konfigurationen för hello Azure CLI):</span><span class="sxs-lookup"><span data-stu-id="ced87-135">hello command results in hello following output (assuming that JSON output is specified in hello Azure CLI configuration):</span></span>

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

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="ced87-136">Felsökning och tips</span><span class="sxs-lookup"><span data-stu-id="ced87-136">Tips and troubleshooting</span></span>

<span data-ttu-id="ced87-137">Du kan hitta hello efter information som är användbar om du stöter på problem när du använder Service Fabric-kommandon i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ced87-137">You might find hello following information helpful if you run into issues while using Service Fabric commands in Azure CLI.</span></span>

### <a name="convert-a-certificate-from-pfx-toopem-format"></a><span data-ttu-id="ced87-138">Konvertera ett certifikat från tooPEM PFX-format</span><span class="sxs-lookup"><span data-stu-id="ced87-138">Convert a certificate from PFX tooPEM format</span></span>

<span data-ttu-id="ced87-139">Azure CLI har stöd för certifikat på klientsidan i form av PEM-filer (.pem-filtillägg).</span><span class="sxs-lookup"><span data-stu-id="ced87-139">Azure CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="ced87-140">Om du använder PFX-filer från Windows, måste du konvertera dessa certifikat tooPEM format.</span><span class="sxs-lookup"><span data-stu-id="ced87-140">If you use PFX files from Windows, you must convert those certificates tooPEM format.</span></span> <span data-ttu-id="ced87-141">tooconvert en PFX-filen tooa PEM-fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ced87-141">tooconvert a PFX file tooa PEM file, use hello following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="ced87-142">Mer information finns i hello [OpenSSL dokumentationen](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="ced87-142">For more information, see hello [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="ced87-143">Anslutningsproblem</span><span class="sxs-lookup"><span data-stu-id="ced87-143">Connection issues</span></span>

<span data-ttu-id="ced87-144">Vissa åtgärder kan generera hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="ced87-144">Some operations might generate hello following message:</span></span>

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="ced87-145">Kontrollera att hello angett klusterslutpunkt är tillgänglig och lyssnar.</span><span class="sxs-lookup"><span data-stu-id="ced87-145">Verify that hello specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="ced87-146">Kontrollera också att hello Service Fabric Explorer Användargränssnittet är tillgänglig på som värd och port.</span><span class="sxs-lookup"><span data-stu-id="ced87-146">Also, verify that hello Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="ced87-147">tooupdate hello slutpunkt och Använd `az sf cluster select`.</span><span class="sxs-lookup"><span data-stu-id="ced87-147">tooupdate hello endpoint, use `az sf cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="ced87-148">Detaljerade loggar</span><span class="sxs-lookup"><span data-stu-id="ced87-148">Detailed logs</span></span>

<span data-ttu-id="ced87-149">Detaljerade loggar är ofta användbara när du felsöker eller rapporterar problem.</span><span class="sxs-lookup"><span data-stu-id="ced87-149">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="ced87-150">Azure CLI erbjuder en global `--debug` flagga som ökar hello detaljnivå loggfiler.</span><span class="sxs-lookup"><span data-stu-id="ced87-150">Azure CLI offers a global `--debug` flag that increases hello verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="ced87-151">Hjälp och syntax för kommandon</span><span class="sxs-lookup"><span data-stu-id="ced87-151">Command help and syntax</span></span>

<span data-ttu-id="ced87-152">Service Fabric-kommandon Följ hello samma konventioner som Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ced87-152">Service Fabric commands follow hello same conventions as Azure CLI.</span></span> <span data-ttu-id="ced87-153">Hjälp med ett visst kommando eller en grupp med kommandon för att använda hello `-h` flaggan:</span><span class="sxs-lookup"><span data-stu-id="ced87-153">For help with a specific command or a group of commands, use hello `-h` flag:</span></span>

```azurecli
az sf application -h
```

<span data-ttu-id="ced87-154">Här är ett annat exempel:</span><span class="sxs-lookup"><span data-stu-id="ced87-154">Here's another example:</span></span>

```azurecli
az sf application create -h
```
