---
title: "Kom igång med Azure Service Fabric och Azure CLI 2.0"
description: "Lär dig hur du använder kommandomodulen Azure Service Fabric i Azure CLI version 2.0. Lär dig hur du ansluter till ett kluster och hanterar program."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ee3302b984ca2f5509755dc17b0a5fd06ace0afe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-service-fabric-and-azure-cli-20"></a><span data-ttu-id="b5483-104">Service Fabric och Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b5483-104">Azure Service Fabric and Azure CLI 2.0</span></span>

<span data-ttu-id="b5483-105">Azure kommandoradsverktyget (Azure CLI) version 2.0 innehåller kommandon för att hjälpa dig att hantera Azure Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="b5483-105">The Azure command-line tool (Azure CLI) version 2.0 includes commands to help you manage Azure Service Fabric clusters.</span></span> <span data-ttu-id="b5483-106">Kom igång med Azure Service Fabric och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b5483-106">Learn how to get started with Azure CLI and Service Fabric.</span></span>

## <a name="install-azure-cli-20"></a><span data-ttu-id="b5483-107">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b5483-107">Install Azure CLI 2.0</span></span>

<span data-ttu-id="b5483-108">Azure CLI 2.0 innehåller nu kommandon för att interagera med och hantera Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="b5483-108">You can use Azure CLI 2.0 commands to interact with and manage Service Fabric clusters.</span></span> <span data-ttu-id="b5483-109">För att få den senaste versionen av Azure CLI följer du standardanvisningarna för [Azure CLI 2.0-installation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b5483-109">To get the latest version of Azure CLI, follow the [Azure CLI 2.0 standard installation process](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="b5483-110">Mer information finns i [Översikt över Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b5483-110">For more information, see the [Azure CLI 2.0 overview](https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="azure-cli-syntax"></a><span data-ttu-id="b5483-111">Azure CLI-syntax</span><span class="sxs-lookup"><span data-stu-id="b5483-111">Azure CLI syntax</span></span>

<span data-ttu-id="b5483-112">Alla Service Fabric-kommandon har prefixet `az sf` i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b5483-112">In Azure CLI, all Service Fabric commands are prefixed with `az sf`.</span></span> <span data-ttu-id="b5483-113">För allmän information om de kommandon som du kan använda, använd `az sf -h`.</span><span class="sxs-lookup"><span data-stu-id="b5483-113">For general information about the commands you can use, use `az sf -h`.</span></span> <span data-ttu-id="b5483-114">För hjälp med ett enda kommando, använd `az sf <command> -h`.</span><span class="sxs-lookup"><span data-stu-id="b5483-114">For help with a single command, use `az sf <command> -h`.</span></span>

<span data-ttu-id="b5483-115">Service Fabric-kommandon i CLI följer detta namngivningsmönster:</span><span class="sxs-lookup"><span data-stu-id="b5483-115">Service Fabric commands in Azure CLI follow this naming pattern:</span></span>

```azurecli
az sf <object> <action>
```

<span data-ttu-id="b5483-116">`<object>` är målet för `<action>`.</span><span class="sxs-lookup"><span data-stu-id="b5483-116">`<object>` is the target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="b5483-117">Välj ett kluster</span><span class="sxs-lookup"><span data-stu-id="b5483-117">Select a cluster</span></span>

<span data-ttu-id="b5483-118">Innan du kan utföra några åtgärder måste du välja ett kluster att ansluta till.</span><span class="sxs-lookup"><span data-stu-id="b5483-118">Before you perform any operations, you must select a cluster to connect to.</span></span> <span data-ttu-id="b5483-119">Följande kod är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="b5483-119">For an example, see the following code.</span></span> <span data-ttu-id="b5483-120">Koden ansluter till ett oskyddat kluster.</span><span class="sxs-lookup"><span data-stu-id="b5483-120">The code connects to an unsecured cluster.</span></span>

> [!WARNING]
> <span data-ttu-id="b5483-121">Använd inte oskyddade Service Fabric-kluster i produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="b5483-121">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="b5483-122">Klusterslutpunkten måste föregås av `http` eller `https`.</span><span class="sxs-lookup"><span data-stu-id="b5483-122">The cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="b5483-123">Det måste innehålla porten för HTTP-gateway.</span><span class="sxs-lookup"><span data-stu-id="b5483-123">It must include the port for the HTTP gateway.</span></span> <span data-ttu-id="b5483-124">Den här porten och adressen är samma som webbadressen för Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="b5483-124">The port and address are the same as the Service Fabric Explorer URL.</span></span>

<span data-ttu-id="b5483-125">Du kan antingen använda okrypterade .pem-filer, eller .crt- och .key-filer, för kluster som är säkrade med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="b5483-125">For clusters that are secured with a certificate, you can use either unencrypted .pem files, or .crt and .key files.</span></span> <span data-ttu-id="b5483-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b5483-126">For example:</span></span>

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="b5483-127">Mer information finns i [Ansluta till ett Azure Service Fabric-kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="b5483-127">For more information, see [Connect to a secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b5483-128">Kommandot `select` inverkar inte på anrop innan den returnerar.</span><span class="sxs-lookup"><span data-stu-id="b5483-128">The `select` command doesn't act on any requests before it returns.</span></span> <span data-ttu-id="b5483-129">Om du vill kontrollera att du har angett ett kluster korrekt använder du ett kommando som `az sf cluster health`.</span><span class="sxs-lookup"><span data-stu-id="b5483-129">To verify that you've specified a cluster correctly, use a command like `az sf cluster health`.</span></span> <span data-ttu-id="b5483-130">Kontrollera att kommandot inte returnerar några fel.</span><span class="sxs-lookup"><span data-stu-id="b5483-130">Verify that the command doesn't return any errors.</span></span>

## <a name="basic-operations"></a><span data-ttu-id="b5483-131">Grundläggande åtgärder</span><span class="sxs-lookup"><span data-stu-id="b5483-131">Basic operations</span></span>

<span data-ttu-id="b5483-132">Klustrets anslutningsinformation bevaras mellan olika Azure CLI-sessioner.</span><span class="sxs-lookup"><span data-stu-id="b5483-132">Cluster connection information persists across multiple Azure CLI sessions.</span></span> <span data-ttu-id="b5483-133">När du har valt ett Service Fabric-kluster kan du köra alla Service Fabric-kommandon på klustret.</span><span class="sxs-lookup"><span data-stu-id="b5483-133">After you select a Service Fabric cluster, you can run any Service Fabric command on the cluster.</span></span>

<span data-ttu-id="b5483-134">Om du till exempel vill se hälsotillståndet för Service Fabric-klustret kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5483-134">For example, to get the Service Fabric cluster health state, use the following command:</span></span>

```azurecli
az sf cluster health
```

<span data-ttu-id="b5483-135">Kommandot resulterar i följande utdata (förutsatt att JSON-utdata har angetts i konfigurationen av Azure CLI):</span><span class="sxs-lookup"><span data-stu-id="b5483-135">The command results in the following output (assuming that JSON output is specified in the Azure CLI configuration):</span></span>

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

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="b5483-136">Felsökning och tips</span><span class="sxs-lookup"><span data-stu-id="b5483-136">Tips and troubleshooting</span></span>

<span data-ttu-id="b5483-137">Följande information kan vara till hjälp om du stöter på problem när du använder Service Fabric-kommandon i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b5483-137">You might find the following information helpful if you run into issues while using Service Fabric commands in Azure CLI.</span></span>

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a><span data-ttu-id="b5483-138">Konvertera ett certifikat från PFX till PEM</span><span class="sxs-lookup"><span data-stu-id="b5483-138">Convert a certificate from PFX to PEM format</span></span>

<span data-ttu-id="b5483-139">Azure CLI har stöd för certifikat på klientsidan i form av PEM-filer (.pem-filtillägg).</span><span class="sxs-lookup"><span data-stu-id="b5483-139">Azure CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="b5483-140">Om du använder PFX-filer från Windows måste du konvertera dessa certifikat till PEM-format.</span><span class="sxs-lookup"><span data-stu-id="b5483-140">If you use PFX files from Windows, you must convert those certificates to PEM format.</span></span> <span data-ttu-id="b5483-141">Om du vill konvertera en PFX-fil till en PEM-fil använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b5483-141">To convert a PFX file to a PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="b5483-142">Mer information finns i [OpenSSL-dokumentationen](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="b5483-142">For more information, see the [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="b5483-143">Anslutningsproblem</span><span class="sxs-lookup"><span data-stu-id="b5483-143">Connection issues</span></span>

<span data-ttu-id="b5483-144">Vissa åtgärder kan generera följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="b5483-144">Some operations might generate the following message:</span></span>

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="b5483-145">Kontrollera att den angivna kluster-slutpunkten är tillgänglig och lyssnar.</span><span class="sxs-lookup"><span data-stu-id="b5483-145">Verify that the specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="b5483-146">Kontrollera också att gränssnittet för Service Fabric Explorer kan nås på denna värd och port.</span><span class="sxs-lookup"><span data-stu-id="b5483-146">Also, verify that the Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="b5483-147">Använd `az sf cluster select` till att uppdatera slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b5483-147">To update the endpoint, use `az sf cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="b5483-148">Detaljerade loggar</span><span class="sxs-lookup"><span data-stu-id="b5483-148">Detailed logs</span></span>

<span data-ttu-id="b5483-149">Detaljerade loggar är ofta användbara när du felsöker eller rapporterar problem.</span><span class="sxs-lookup"><span data-stu-id="b5483-149">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="b5483-150">Azure CLI innehåller en global `--debug`-flagga som ökar loggfilernas detaljnivå.</span><span class="sxs-lookup"><span data-stu-id="b5483-150">Azure CLI offers a global `--debug` flag that increases the verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="b5483-151">Hjälp och syntax för kommandon</span><span class="sxs-lookup"><span data-stu-id="b5483-151">Command help and syntax</span></span>

<span data-ttu-id="b5483-152">Service Fabric-kommandona följer samma regler som i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b5483-152">Service Fabric commands follow the same conventions as Azure CLI.</span></span> <span data-ttu-id="b5483-153">Hjälp med ett visst kommando eller en grupp med kommandon, använder du flaggan `-h`:</span><span class="sxs-lookup"><span data-stu-id="b5483-153">For help with a specific command or a group of commands, use the `-h` flag:</span></span>

```azurecli
az sf application -h
```

<span data-ttu-id="b5483-154">Här är ett annat exempel:</span><span class="sxs-lookup"><span data-stu-id="b5483-154">Here's another example:</span></span>

```azurecli
az sf application create -h
```
