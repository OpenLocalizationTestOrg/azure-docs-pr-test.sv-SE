---
title: "Tips om att använda Hadoop på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Hämta implementering tips för att använda kluster för Linux-baserat HDInsight (Hadoop) på en välbekant miljö för Linux körs i Azure-molnet."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 8c6ff4a6b8617cda9b12be060c7c7bed62cb3f44
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="information-about-using-hdinsight-on-linux"></a><span data-ttu-id="2ce48-103">Information om hur du använder HDInsight på Linux</span><span class="sxs-lookup"><span data-stu-id="2ce48-103">Information about using HDInsight on Linux</span></span>

<span data-ttu-id="2ce48-104">Azure HDInsight-kluster tillhandahåller Hadoop på en välbekant miljö för Linux, körs i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="2ce48-104">Azure HDInsight clusters provide Hadoop on a familiar Linux environment, running in the Azure cloud.</span></span> <span data-ttu-id="2ce48-105">För de flesta saker, bör det fungerar precis som andra Hadoop på Linux-installationen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-105">For most things, it should work exactly as any other Hadoop-on-Linux installation.</span></span> <span data-ttu-id="2ce48-106">Det här dokumentet visar specifika skillnader som du bör vara medveten om.</span><span class="sxs-lookup"><span data-stu-id="2ce48-106">This document calls out specific differences that you should be aware of.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2ce48-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="2ce48-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2ce48-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2ce48-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ce48-109">Krav</span><span class="sxs-lookup"><span data-stu-id="2ce48-109">Prerequisites</span></span>

<span data-ttu-id="2ce48-110">Många av stegen i det här dokumentet använder följande verktyg, som kan behöva installeras på datorn.</span><span class="sxs-lookup"><span data-stu-id="2ce48-110">Many of the steps in this document use the following utilities, which may need to be installed on your system.</span></span>

* <span data-ttu-id="2ce48-111">[cURL](https://curl.haxx.se/) – används för att kommunicera med webbtjänster</span><span class="sxs-lookup"><span data-stu-id="2ce48-111">[cURL](https://curl.haxx.se/) - used to communicate with web-based services</span></span>
* <span data-ttu-id="2ce48-112">[jq](https://stedolan.github.io/jq/) – används för att parsa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="2ce48-112">[jq](https://stedolan.github.io/jq/) - used to parse JSON documents</span></span>
* <span data-ttu-id="2ce48-113">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (förhandsvisning) – används för att hantera Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="2ce48-113">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (preview) - used to remotely manage Azure services</span></span>

## <a name="users"></a><span data-ttu-id="2ce48-114">Användare</span><span class="sxs-lookup"><span data-stu-id="2ce48-114">Users</span></span>

<span data-ttu-id="2ce48-115">Om inte [domänanslutna](hdinsight-domain-joined-introduction.md), HDInsight ska betraktas som en **enanvändarläge** system.</span><span class="sxs-lookup"><span data-stu-id="2ce48-115">Unless [domain-joined](hdinsight-domain-joined-introduction.md), HDInsight should be considered a **single-user** system.</span></span> <span data-ttu-id="2ce48-116">En SSH-användarkontot skapas med i klustret, med administratörsbehörighet för nivån.</span><span class="sxs-lookup"><span data-stu-id="2ce48-116">A single SSH user account is created with the cluster, with administrator level permissions.</span></span> <span data-ttu-id="2ce48-117">Ytterligare SSH-konton kan skapas, men de har också administratörsåtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-117">Additional SSH accounts can be created, but they also have administrator access to the cluster.</span></span>

<span data-ttu-id="2ce48-118">Domänanslutna HDInsight har stöd för flera användare och mer detaljerade inställningar för behörighet och roll.</span><span class="sxs-lookup"><span data-stu-id="2ce48-118">Domain-joined HDInsight supports multiple users and more granular permission and role settings.</span></span> <span data-ttu-id="2ce48-119">Mer information finns i [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).</span><span class="sxs-lookup"><span data-stu-id="2ce48-119">For more information, see [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md).</span></span>

## <a name="domain-names"></a><span data-ttu-id="2ce48-120">Domännamn</span><span class="sxs-lookup"><span data-stu-id="2ce48-120">Domain names</span></span>

<span data-ttu-id="2ce48-121">Det fullständigt kvalificerade domännamnet (FQDN) ska användas vid anslutning till klustret från internet är  **&lt;klusternamn >. azurehdinsight.net** eller (SSH)  **&lt;klusternamn-ssh >. azurehdinsight.NET**.</span><span class="sxs-lookup"><span data-stu-id="2ce48-121">The fully qualified domain name (FQDN) to use when connecting to the cluster from the internet is **&lt;clustername>.azurehdinsight.net** or (for SSH only) **&lt;clustername-ssh>.azurehdinsight.net**.</span></span>

<span data-ttu-id="2ce48-122">Varje nod i klustret har internt, ett namn som tilldelas under klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-122">Internally, each node in the cluster has a name that is assigned during cluster configuration.</span></span> <span data-ttu-id="2ce48-123">Du hittar klusternamn den **värdar** sida på Ambari-Webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2ce48-123">To find the cluster names, see the **Hosts** page on the Ambari Web UI.</span></span> <span data-ttu-id="2ce48-124">Du kan också använda följande för att returnera en lista över värdar från Ambari REST API:</span><span class="sxs-lookup"><span data-stu-id="2ce48-124">You can also use the following to return a list of hosts from the Ambari REST API:</span></span>

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

<span data-ttu-id="2ce48-125">Ersätt **lösenord** med lösenordet för administratörskontot och **KLUSTERNAMN** med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-125">Replace **PASSWORD** with the password of the admin account, and **CLUSTERNAME** with the name of your cluster.</span></span> <span data-ttu-id="2ce48-126">Det här kommandot returnerar ett JSON-dokument som innehåller en lista över värdar i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-126">This command returns a JSON document that contains a list of the hosts in the cluster.</span></span> <span data-ttu-id="2ce48-127">Jq används för att extrahera den `host_name` elementvärde för varje värd.</span><span class="sxs-lookup"><span data-stu-id="2ce48-127">Jq is used to extract the `host_name` element value for each host.</span></span>

<span data-ttu-id="2ce48-128">Om du behöver att hitta namnet på noden för en specifik tjänst kan fråga du Ambari för den komponenten.</span><span class="sxs-lookup"><span data-stu-id="2ce48-128">If you need to find the name of the node for a specific service, you can query Ambari for that component.</span></span> <span data-ttu-id="2ce48-129">Till exempel för att hitta värdar för noden HDFS namn, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2ce48-129">For example, to find the hosts for the HDFS name node, use the following command:</span></span>

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

<span data-ttu-id="2ce48-130">Det här kommandot returnerar ett JSON-dokument som beskriver tjänsten och sedan jq hämtar endast den `host_name` värde för värdarna.</span><span class="sxs-lookup"><span data-stu-id="2ce48-130">This command returns a JSON document describing the service, and then jq pulls out only the `host_name` value for the hosts.</span></span>

## <a name="remote-access-to-services"></a><span data-ttu-id="2ce48-131">Fjärråtkomst till tjänster</span><span class="sxs-lookup"><span data-stu-id="2ce48-131">Remote access to services</span></span>

* <span data-ttu-id="2ce48-132">**Ambari (webb)** -https://&lt;klusternamn >. azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="2ce48-132">**Ambari (web)** - https://&lt;clustername>.azurehdinsight.net</span></span>

    <span data-ttu-id="2ce48-133">Autentisera med hjälp av kluster-administratör och lösenordet och sedan logga in på Ambari.</span><span class="sxs-lookup"><span data-stu-id="2ce48-133">Authenticate by using the cluster administrator user and password, and then log in to Ambari.</span></span>

    <span data-ttu-id="2ce48-134">Autentisering är klartext - alltid använda HTTPS för att säkerställa att anslutningen är säker.</span><span class="sxs-lookup"><span data-stu-id="2ce48-134">Authentication is plaintext - always use HTTPS to help ensure that the connection is secure.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2ce48-135">Vissa av användargränssnitt som är tillgängliga via Ambari åt noder med hjälp av ett internt domännamn.</span><span class="sxs-lookup"><span data-stu-id="2ce48-135">Some of the web UIs available through Ambari access nodes using an internal domain name.</span></span> <span data-ttu-id="2ce48-136">Interna domännamn är inte offentligt tillgänglig via internet.</span><span class="sxs-lookup"><span data-stu-id="2ce48-136">Internal domain names are not publicly accessible over the internet.</span></span> <span data-ttu-id="2ce48-137">Felmeddelandet ”Det gick inte att hitta” fel vid försök att komma åt vissa funktioner via Internet.</span><span class="sxs-lookup"><span data-stu-id="2ce48-137">You may receive "server not found" errors when trying to access some features over the Internet.</span></span>
    >
    > <span data-ttu-id="2ce48-138">Om du vill använda den fullständiga funktionaliteten hos Ambari-webbgränssnittet, använda en SSH-tunnel till proxy webbtrafik till klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="2ce48-138">To use the full functionality of the Ambari web UI, use an SSH tunnel to proxy web traffic to the cluster head node.</span></span> <span data-ttu-id="2ce48-139">Se [Använd SSH-tunnlar för att komma åt Ambari-webbgränssnittet, resurshanteraren, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="2ce48-139">See [Use SSH Tunneling to access Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

* <span data-ttu-id="2ce48-140">**Ambari (REST)** -https://&lt;klusternamn >.azurehdinsight.net/ambari</span><span class="sxs-lookup"><span data-stu-id="2ce48-140">**Ambari (REST)** - https://&lt;clustername>.azurehdinsight.net/ambari</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ce48-141">Autentisera med hjälp av kluster-administratör och lösenord.</span><span class="sxs-lookup"><span data-stu-id="2ce48-141">Authenticate by using the cluster administrator user and password.</span></span>
    >
    > <span data-ttu-id="2ce48-142">Autentisering är klartext - alltid använda HTTPS för att säkerställa att anslutningen är säker.</span><span class="sxs-lookup"><span data-stu-id="2ce48-142">Authentication is plaintext - always use HTTPS to help ensure that the connection is secure.</span></span>

* <span data-ttu-id="2ce48-143">**WebHCat (Templeton)** -https://&lt;klusternamn >.azurehdinsight.net/templeton</span><span class="sxs-lookup"><span data-stu-id="2ce48-143">**WebHCat (Templeton)** - https://&lt;clustername>.azurehdinsight.net/templeton</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ce48-144">Autentisera med hjälp av kluster-administratör och lösenord.</span><span class="sxs-lookup"><span data-stu-id="2ce48-144">Authenticate by using the cluster administrator user and password.</span></span>
    >
    > <span data-ttu-id="2ce48-145">Autentisering är klartext - alltid använda HTTPS för att säkerställa att anslutningen är säker.</span><span class="sxs-lookup"><span data-stu-id="2ce48-145">Authentication is plaintext - always use HTTPS to help ensure that the connection is secure.</span></span>

* <span data-ttu-id="2ce48-146">**SSH** - &lt;klusternamn >-ssh.azurehdinsight.net på port 22 eller 23.</span><span class="sxs-lookup"><span data-stu-id="2ce48-146">**SSH** - &lt;clustername>-ssh.azurehdinsight.net on port 22 or 23.</span></span> <span data-ttu-id="2ce48-147">Att ansluta till den primära headnode 23 används för att ansluta till sekundärt används port 22.</span><span class="sxs-lookup"><span data-stu-id="2ce48-147">Port 22 is used to connect to the primary headnode, while 23 is used to connect to the secondary.</span></span> <span data-ttu-id="2ce48-148">Mer information om huvudnoderna finns i [Tillgänglighet och tillförlitlighet för Hadoop-kluster i HDInsight](hdinsight-high-availability-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2ce48-148">For more information on the head nodes, see [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ce48-149">Du kan bara komma åt klustrets huvudnoder via SSH från en klientdator.</span><span class="sxs-lookup"><span data-stu-id="2ce48-149">You can only access the cluster head nodes through SSH from a client machine.</span></span> <span data-ttu-id="2ce48-150">När du är ansluten, du kan sedan komma åt arbetsnoderna med hjälp av SSH från en headnode.</span><span class="sxs-lookup"><span data-stu-id="2ce48-150">Once connected, you can then access the worker nodes by using SSH from a headnode.</span></span>

## <a name="file-locations"></a><span data-ttu-id="2ce48-151">Sökvägar</span><span class="sxs-lookup"><span data-stu-id="2ce48-151">File locations</span></span>

<span data-ttu-id="2ce48-152">Hadoop-relaterade filer kan hittas på klusternoder på `/usr/hdp`.</span><span class="sxs-lookup"><span data-stu-id="2ce48-152">Hadoop-related files can be found on the cluster nodes at `/usr/hdp`.</span></span> <span data-ttu-id="2ce48-153">Den här katalogen innehåller följande undermappar:</span><span class="sxs-lookup"><span data-stu-id="2ce48-153">This directory contains the following subdirectories:</span></span>

* <span data-ttu-id="2ce48-154">**2.2.4.9-1**: katalognamnet är versionen av Hortonworks Data Platform som används av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2ce48-154">**2.2.4.9-1**: The directory name is the version of the Hortonworks Data Platform used by HDInsight.</span></span> <span data-ttu-id="2ce48-155">Numret på ditt kluster kan vara annorlunda än den som anges här.</span><span class="sxs-lookup"><span data-stu-id="2ce48-155">The number on your cluster may be different than the one listed here.</span></span>
* <span data-ttu-id="2ce48-156">**aktuella**: den här katalogen innehåller länkar till underkataloger på den **2.2.4.9-1** directory.</span><span class="sxs-lookup"><span data-stu-id="2ce48-156">**current**: This directory contains links to subdirectories under the **2.2.4.9-1** directory.</span></span> <span data-ttu-id="2ce48-157">Denna katalog finns så att du inte behöver komma ihåg versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-157">This directory exists so that you don't have to remember the version number.</span></span>

<span data-ttu-id="2ce48-158">Exempeldata och JAR-filer kan hittas på Hadoop Distributed File System på `/example` och`/HdiSamples`</span><span class="sxs-lookup"><span data-stu-id="2ce48-158">Example data and JAR files can be found on Hadoop Distributed File System at `/example` and `/HdiSamples`</span></span>

## <a name="hdfs-azure-storage-and-data-lake-store"></a><span data-ttu-id="2ce48-159">HDFS och Azure Storage Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2ce48-159">HDFS, Azure Storage, and Data Lake Store</span></span>

<span data-ttu-id="2ce48-160">I de flesta Hadoop distributioner stöds HDFS av lokal lagring på datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-160">In most Hadoop distributions, HDFS is backed by local storage on the machines in the cluster.</span></span> <span data-ttu-id="2ce48-161">Använder lokal lagring kan vara kostsamma för en molnbaserad lösning där du debiteras timvis eller per minut för beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="2ce48-161">Using local storage can be costly for a cloud-based solution where you are charged hourly or by minute for compute resources.</span></span>

<span data-ttu-id="2ce48-162">HDInsight använder antingen blobbar i Azure Storage eller Azure Data Lake Store som standard Arkiv.</span><span class="sxs-lookup"><span data-stu-id="2ce48-162">HDInsight uses either blobs in Azure Storage or Azure Data Lake Store as the default store.</span></span> <span data-ttu-id="2ce48-163">Dessa tjänster ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2ce48-163">These services provide the following benefits:</span></span>

* <span data-ttu-id="2ce48-164">Billig långsiktig lagring</span><span class="sxs-lookup"><span data-stu-id="2ce48-164">Cheap long-term storage</span></span>
* <span data-ttu-id="2ce48-165">Åtkomst från externa tjänster, till exempel webbplatser, filen överför/hämta verktyg, SDK: er med olika språk och webbläsare</span><span class="sxs-lookup"><span data-stu-id="2ce48-165">Accessibility from external services such as websites, file upload/download utilities, various language SDKs, and web browsers</span></span>

> [!WARNING]
> <span data-ttu-id="2ce48-166">HDInsight stöder endast __allmänna__ Azure Storage-konton.</span><span class="sxs-lookup"><span data-stu-id="2ce48-166">HDInsight only supports __General-purpose__ Azure Storage accounts.</span></span> <span data-ttu-id="2ce48-167">För närvarande stöder inte den __Blob storage__ kontotyp.</span><span class="sxs-lookup"><span data-stu-id="2ce48-167">It does not currently support the __Blob storage__ account type.</span></span>

<span data-ttu-id="2ce48-168">Ett Azure Storage-konto kan innehålla upp till 4,75 TB, även om enskilda blobbar (eller filer från ett HDInsight-perspektiv) kan du bara gå upp till 195 GB.</span><span class="sxs-lookup"><span data-stu-id="2ce48-168">An Azure Storage account can hold up to 4.75 TB, though individual blobs (or files from an HDInsight perspective) can only go up to 195 GB.</span></span> <span data-ttu-id="2ce48-169">Azure Data Lake Store kan växa dynamiskt för att rymma BILJONTALS filer med enskilda filer som är större än en petabyte.</span><span class="sxs-lookup"><span data-stu-id="2ce48-169">Azure Data Lake Store can grow dynamically to hold trillions of files, with individual files greater than a petabyte.</span></span> <span data-ttu-id="2ce48-170">Mer information finns i [förstå blobbar](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) och [Datasjölager](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="2ce48-170">For more information, see [Understanding blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) and [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span>

<span data-ttu-id="2ce48-171">När du använder antingen Azure Storage eller Azure Data Lake Store kan du inte vidta några särskilda åtgärder från HDInsight för att komma åt data.</span><span class="sxs-lookup"><span data-stu-id="2ce48-171">When using either Azure Storage or Data Lake Store, you don't have to do anything special from HDInsight to access the data.</span></span> <span data-ttu-id="2ce48-172">Till exempel följande kommando visar filer i den `/example/data` mappen oavsett om den är lagrad på Azure Storage eller Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="2ce48-172">For example, the following command lists files in the `/example/data` folder regardless of whether it is stored on Azure Storage or Data Lake Store:</span></span>

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a><span data-ttu-id="2ce48-173">URI: N och schema</span><span class="sxs-lookup"><span data-stu-id="2ce48-173">URI and scheme</span></span>

<span data-ttu-id="2ce48-174">Vissa kommandon kan kräva att ange schemat som en del av URI vid åtkomst till en fil.</span><span class="sxs-lookup"><span data-stu-id="2ce48-174">Some commands may require you to specify the scheme as part of the URI when accessing a file.</span></span> <span data-ttu-id="2ce48-175">Till exempel måste Storm-HDFS-komponent du ange schemat.</span><span class="sxs-lookup"><span data-stu-id="2ce48-175">For example, the Storm-HDFS component requires you to specify the scheme.</span></span> <span data-ttu-id="2ce48-176">När du använder icke-förvalt lagring (storage lagts till som ”ytterligare” lagringsutrymme i klustret), måste du alltid använda schemat som en del av URI: N.</span><span class="sxs-lookup"><span data-stu-id="2ce48-176">When using non-default storage (storage added as "additional" storage to the cluster), you must always use the scheme as part of the URI.</span></span>

<span data-ttu-id="2ce48-177">När du använder __Azure Storage__, Använd en av de följande URI-scheman:</span><span class="sxs-lookup"><span data-stu-id="2ce48-177">When using __Azure Storage__, use one of the following URI schemes:</span></span>

* <span data-ttu-id="2ce48-178">`wasb:///`: Åtkomst standardlagring använder okrypterad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2ce48-178">`wasb:///`: Access default storage using unencrypted communication.</span></span>

* <span data-ttu-id="2ce48-179">`wasbs:///`: Standard lagring med krypterad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2ce48-179">`wasbs:///`: Access default storage using encrypted communication.</span></span>  <span data-ttu-id="2ce48-180">Wasbs-scheman stöds endast från HDInsight version 3,6 och senare.</span><span class="sxs-lookup"><span data-stu-id="2ce48-180">The wasbs scheme is supported only from HDInsight version 3.6 onwards.</span></span>

* <span data-ttu-id="2ce48-181">`wasb://<container-name>@<account-name>.blob.core.windows.net/`: Används vid kommunikation med ett icke-förvalt storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2ce48-181">`wasb://<container-name>@<account-name>.blob.core.windows.net/`: Used when communicating with a non-default storage account.</span></span> <span data-ttu-id="2ce48-182">Till exempel om du har ett konto för ytterligare lagringsutrymme eller när åtkomst till data lagras i ett offentligt tillgänglig storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2ce48-182">For example, when you have an additional storage account or when accessing data stored in a publicly accessible storage account.</span></span>

<span data-ttu-id="2ce48-183">När du använder __Datasjölager__, Använd en av de följande URI-scheman:</span><span class="sxs-lookup"><span data-stu-id="2ce48-183">When using __Data Lake Store__, use one of the following URI schemes:</span></span>

* <span data-ttu-id="2ce48-184">`adl:///`: Komma åt standard Data Lake Store för klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-184">`adl:///`: Access the default Data Lake Store for the cluster.</span></span>

* <span data-ttu-id="2ce48-185">`adl://<storage-name>.azuredatalakestore.net/`: Används vid kommunikation med en icke-förvalt Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2ce48-185">`adl://<storage-name>.azuredatalakestore.net/`: Used when communicating with a non-default Data Lake Store.</span></span> <span data-ttu-id="2ce48-186">Används också för att komma åt data utanför rotkatalogen för din HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2ce48-186">Also used to access data outside the root directory of your HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2ce48-187">När du använder Data Lake Store som standardlagringsplatsen för HDInsight, måste du ange en sökväg i arkivet för att använda som rot för HDInsight-lagringen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-187">When using Data Lake Store as the default store for HDInsight, you must specify a path within the store to use as the root of HDInsight storage.</span></span> <span data-ttu-id="2ce48-188">Standardsökvägen är `/clusters/<cluster-name>/`.</span><span class="sxs-lookup"><span data-stu-id="2ce48-188">The default path is `/clusters/<cluster-name>/`.</span></span>
>
> <span data-ttu-id="2ce48-189">När du använder `/` eller `adl:///` komma åt data kan du bara komma åt data som lagras i roten (till exempel `/clusters/<cluster-name>/`) i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-189">When using `/` or `adl:///` to access data, you can only access data stored in the root (for example, `/clusters/<cluster-name>/`) of the cluster.</span></span> <span data-ttu-id="2ce48-190">Komma åt data var som helst i arkivet kan använda den `adl://<storage-name>.azuredatalakestore.net/` format.</span><span class="sxs-lookup"><span data-stu-id="2ce48-190">To access data anywhere in the store, use the `adl://<storage-name>.azuredatalakestore.net/` format.</span></span>

### <a name="what-storage-is-the-cluster-using"></a><span data-ttu-id="2ce48-191">Vilka lagring klustret använder</span><span class="sxs-lookup"><span data-stu-id="2ce48-191">What storage is the cluster using</span></span>

<span data-ttu-id="2ce48-192">Du kan använda Ambari för att hämta standardkonfigurationen för lagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-192">You can use Ambari to retrieve the default storage configuration for the cluster.</span></span> <span data-ttu-id="2ce48-193">Använd följande kommando för att hämta konfigurationsinformation för HDFS med curl och filtrera den med hjälp av [jq](https://stedolan.github.io/jq/):</span><span class="sxs-lookup"><span data-stu-id="2ce48-193">Use the following command to retrieve HDFS configuration information using curl, and filter it using [jq](https://stedolan.github.io/jq/):</span></span>

```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> <span data-ttu-id="2ce48-194">Detta returnerar den första konfigurationen tillämpas på servern (`service_config_version=1`), som innehåller den här informationen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-194">This returns the first configuration applied to the server (`service_config_version=1`), which contains this information.</span></span> <span data-ttu-id="2ce48-195">Du kan behöva lista alla versioner för konfigurationen hittar det senaste.</span><span class="sxs-lookup"><span data-stu-id="2ce48-195">You may need to list all configuration versions to find the latest one.</span></span>

<span data-ttu-id="2ce48-196">Det här kommandot returnerar ett värde som liknar följande URI: er:</span><span class="sxs-lookup"><span data-stu-id="2ce48-196">This command returns a value similar to the following URIs:</span></span>

* <span data-ttu-id="2ce48-197">`wasb://<container-name>@<account-name>.blob.core.windows.net`Om du använder ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2ce48-197">`wasb://<container-name>@<account-name>.blob.core.windows.net` if using an Azure Storage account.</span></span>

    <span data-ttu-id="2ce48-198">Kontonamnet är namnet på Azure Storage-konto när behållarens namn är blob-behållaren som är roten till klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-198">The account name is the name of the Azure Storage account, while the container name is the blob container that is the root of the cluster storage.</span></span>

* <span data-ttu-id="2ce48-199">`adl://home`Om du använder Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2ce48-199">`adl://home` if using Azure Data Lake Store.</span></span> <span data-ttu-id="2ce48-200">Använd följande REST-anrop för att få Data Lake Store-namn:</span><span class="sxs-lookup"><span data-stu-id="2ce48-200">To get the Data Lake Store name, use the following REST call:</span></span>

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    <span data-ttu-id="2ce48-201">Det här kommandot returnerar följande värdnamn: `<data-lake-store-account-name>.azuredatalakestore.net`.</span><span class="sxs-lookup"><span data-stu-id="2ce48-201">This command returns the following host name: `<data-lake-store-account-name>.azuredatalakestore.net`.</span></span>

    <span data-ttu-id="2ce48-202">Använd följande REST-anrop för att hämta katalogen i arkivet som är rot för HDInsight:</span><span class="sxs-lookup"><span data-stu-id="2ce48-202">To get the directory within the store that is the root for HDInsight, use the following REST call:</span></span>

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    <span data-ttu-id="2ce48-203">Det här kommandot returnerar en sökväg liknar följande sökväg: `/clusters/<hdinsight-cluster-name>/`.</span><span class="sxs-lookup"><span data-stu-id="2ce48-203">This command returns a path similar to the following path: `/clusters/<hdinsight-cluster-name>/`.</span></span>

<span data-ttu-id="2ce48-204">Du kan också hitta i informationen med Azure-portalen med hjälp av följande steg:</span><span class="sxs-lookup"><span data-stu-id="2ce48-204">You can also find the storage information using the Azure portal by using the following steps:</span></span>

1. <span data-ttu-id="2ce48-205">I den [Azure-portalen](https://portal.azure.com/), Välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2ce48-205">In the [Azure portal](https://portal.azure.com/), select your HDInsight cluster.</span></span>

2. <span data-ttu-id="2ce48-206">Från den **egenskaper** väljer **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="2ce48-206">From the **Properties** section, select **Storage Accounts**.</span></span> <span data-ttu-id="2ce48-207">Storage-informationen för klustret visas.</span><span class="sxs-lookup"><span data-stu-id="2ce48-207">The storage information for the cluster is displayed.</span></span>

### <a name="how-do-i-access-files-from-outside-hdinsight"></a><span data-ttu-id="2ce48-208">Hur kommer jag åt filer från utanför HDInsight</span><span class="sxs-lookup"><span data-stu-id="2ce48-208">How do I access files from outside HDInsight</span></span>

<span data-ttu-id="2ce48-209">Det finns olika sätt att få åtkomst till data utanför HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-209">There are a various ways to access data from outside the HDInsight cluster.</span></span> <span data-ttu-id="2ce48-210">Följande är några länkar till verktyg och SDK: er som kan användas för att arbeta med dina data:</span><span class="sxs-lookup"><span data-stu-id="2ce48-210">The following are a few links to utilities and SDKs that can be used to work with your data:</span></span>

<span data-ttu-id="2ce48-211">Om du använder __Azure Storage__, se följande länkar för olika sätt att du kan komma åt dina data:</span><span class="sxs-lookup"><span data-stu-id="2ce48-211">If using __Azure Storage__, see the following links for ways that you can access your data:</span></span>

* <span data-ttu-id="2ce48-212">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): kommandoradsgränssnittet kommandon för att arbeta med Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce48-212">[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): Command-Line interface commands for working with Azure.</span></span> <span data-ttu-id="2ce48-213">När du har installerat, Använd den `az storage` kommandot för hjälp med att använda lagring, eller `az storage blob` för blob-fil.</span><span class="sxs-lookup"><span data-stu-id="2ce48-213">After installing, use the `az storage` command for help on using storage, or `az storage blob` for blob-specific commands.</span></span>
* <span data-ttu-id="2ce48-214">[blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): en python-skript för att arbeta med blobbar i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2ce48-214">[blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): A python script for working with blobs in Azure Storage.</span></span>
* <span data-ttu-id="2ce48-215">Olika SDK:</span><span class="sxs-lookup"><span data-stu-id="2ce48-215">Various SDKs:</span></span>

    * [<span data-ttu-id="2ce48-216">Java</span><span class="sxs-lookup"><span data-stu-id="2ce48-216">Java</span></span>](https://github.com/Azure/azure-sdk-for-java)
    * [<span data-ttu-id="2ce48-217">Node.js</span><span class="sxs-lookup"><span data-stu-id="2ce48-217">Node.js</span></span>](https://github.com/Azure/azure-sdk-for-node)
    * [<span data-ttu-id="2ce48-218">PHP</span><span class="sxs-lookup"><span data-stu-id="2ce48-218">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php)
    * [<span data-ttu-id="2ce48-219">Python</span><span class="sxs-lookup"><span data-stu-id="2ce48-219">Python</span></span>](https://github.com/Azure/azure-sdk-for-python)
    * [<span data-ttu-id="2ce48-220">Ruby</span><span class="sxs-lookup"><span data-stu-id="2ce48-220">Ruby</span></span>](https://github.com/Azure/azure-sdk-for-ruby)
    * [<span data-ttu-id="2ce48-221">.NET</span><span class="sxs-lookup"><span data-stu-id="2ce48-221">.NET</span></span>](https://github.com/Azure/azure-sdk-for-net)
    * [<span data-ttu-id="2ce48-222">Storage REST API</span><span class="sxs-lookup"><span data-stu-id="2ce48-222">Storage REST API</span></span>](https://msdn.microsoft.com/library/azure/dd135733.aspx)

<span data-ttu-id="2ce48-223">Om du använder __Azure Data Lake Store__, se följande länkar för olika sätt att du kan komma åt dina data:</span><span class="sxs-lookup"><span data-stu-id="2ce48-223">If using __Azure Data Lake Store__, see the following links for ways that you can access your data:</span></span>

* [<span data-ttu-id="2ce48-224">Webbläsare</span><span class="sxs-lookup"><span data-stu-id="2ce48-224">Web browser</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* [<span data-ttu-id="2ce48-225">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ce48-225">PowerShell</span></span>](../data-lake-store/data-lake-store-get-started-powershell.md)
* [<span data-ttu-id="2ce48-226">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2ce48-226">Azure CLI 2.0</span></span>](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [<span data-ttu-id="2ce48-227">WebHDFS REST API</span><span class="sxs-lookup"><span data-stu-id="2ce48-227">WebHDFS REST API</span></span>](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [<span data-ttu-id="2ce48-228">Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ce48-228">Data Lake Tools for Visual Studio</span></span>](https://www.microsoft.com/download/details.aspx?id=49504)
* [<span data-ttu-id="2ce48-229">.NET</span><span class="sxs-lookup"><span data-stu-id="2ce48-229">.NET</span></span>](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [<span data-ttu-id="2ce48-230">Java</span><span class="sxs-lookup"><span data-stu-id="2ce48-230">Java</span></span>](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [<span data-ttu-id="2ce48-231">Python</span><span class="sxs-lookup"><span data-stu-id="2ce48-231">Python</span></span>](../data-lake-store/data-lake-store-get-started-python.md)

## <span data-ttu-id="2ce48-232"><a name="scaling"></a>Skalning av klustret</span><span class="sxs-lookup"><span data-stu-id="2ce48-232"><a name="scaling"></a>Scaling your cluster</span></span>

<span data-ttu-id="2ce48-233">Klustret skalning funktionen kan du ändra antalet datanoder som används av ett kluster dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="2ce48-233">The cluster scaling feature allows you to dynamically change the number of data nodes used by a cluster.</span></span> <span data-ttu-id="2ce48-234">Du kan utföra skalning åtgärder när andra jobb eller processer som körs på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="2ce48-234">You can perform scaling operations while other jobs or processes are running on a cluster.</span></span>

<span data-ttu-id="2ce48-235">Olika klustertyper påverkas av skalning enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2ce48-235">The different cluster types are affected by scaling as follows:</span></span>

* <span data-ttu-id="2ce48-236">**Hadoop**: vid skalning av antalet noder i ett kluster, en del av tjänsterna i klustret har startats om.</span><span class="sxs-lookup"><span data-stu-id="2ce48-236">**Hadoop**: When scaling down the number of nodes in a cluster, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="2ce48-237">Skalning åtgärder kan orsaka jobb körs eller väntar misslyckas vid skalning åtgärden slutfördes.</span><span class="sxs-lookup"><span data-stu-id="2ce48-237">Scaling operations can cause jobs running or pending to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="2ce48-238">Du kan skicka jobb när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2ce48-238">You can resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="2ce48-239">**HBase**: regionala servrar balanseras automatiskt inom några minuter efter skalning åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2ce48-239">**HBase**: Regional servers are automatically balanced within a few minutes after completion of the scaling operation.</span></span> <span data-ttu-id="2ce48-240">Om du vill utjämna manuellt regionala servrar, Använd följande steg:</span><span class="sxs-lookup"><span data-stu-id="2ce48-240">To manually balance regional servers, use the following steps:</span></span>

    1. <span data-ttu-id="2ce48-241">Anslut till HDInsight-klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="2ce48-241">Connect to the HDInsight cluster using SSH.</span></span> <span data-ttu-id="2ce48-242">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="2ce48-242">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

    2. <span data-ttu-id="2ce48-243">Använd följande för att starta HBase-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="2ce48-243">Use the following to start the HBase shell:</span></span>

            hbase shell

    3. <span data-ttu-id="2ce48-244">När HBase-gränssnittet har lästs in, Använd följande för att balansera regionala servrar manuellt:</span><span class="sxs-lookup"><span data-stu-id="2ce48-244">Once the HBase shell has loaded, use the following to manually balance the regional servers:</span></span>

            balancer

* <span data-ttu-id="2ce48-245">**Storm**: du bör balansera alla Storm-topologier som körs när en skalning åtgärden har utförts.</span><span class="sxs-lookup"><span data-stu-id="2ce48-245">**Storm**: You should rebalance any running Storm topologies after a scaling operation has been performed.</span></span> <span data-ttu-id="2ce48-246">Ombalansering gör att topologin kan justera parallellitet inställningar baserat på det nya antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-246">Rebalancing allows the topology to readjust parallelism settings based on the new number of nodes in the cluster.</span></span> <span data-ttu-id="2ce48-247">För att balansera om topologier som körs, använder du något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="2ce48-247">To rebalance running topologies, use one of the following options:</span></span>

    * <span data-ttu-id="2ce48-248">**SSH**: ansluta till servern och använder du följande kommando för att balansera en topologi:</span><span class="sxs-lookup"><span data-stu-id="2ce48-248">**SSH**: Connect to the server and use the following command to rebalance a topology:</span></span>

            storm rebalance TOPOLOGYNAME

        <span data-ttu-id="2ce48-249">Du kan också ange parametrar för att åsidosätta parallellitet tipsen ursprungligen tillhandahålls av topologin.</span><span class="sxs-lookup"><span data-stu-id="2ce48-249">You can also specify parameters to override the parallelism hints originally provided by the topology.</span></span> <span data-ttu-id="2ce48-250">Till exempel `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` konfigurerar topologi 5 arbetsprocesser, 3 Utförarna för komponenten blå kanal och 10 Utförarna för komponenten gul bulten.</span><span class="sxs-lookup"><span data-stu-id="2ce48-250">For example, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` reconfigures the topology to 5 worker processes, 3 executors for the blue-spout component, and 10 executors for the yellow-bolt component.</span></span>

    * <span data-ttu-id="2ce48-251">**Storm-Användargränssnittet**: Använd följande steg för att balansera en topologi med hjälp av Storm-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2ce48-251">**Storm UI**: Use the following steps to rebalance a topology using the Storm UI.</span></span>

        1. <span data-ttu-id="2ce48-252">Öppna **https://CLUSTERNAME.azurehdinsight.net/stormui** i webbläsaren, där KLUSTERNAMN är namnet på Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="2ce48-252">Open **https://CLUSTERNAME.azurehdinsight.net/stormui** in your web browser, where CLUSTERNAME is the name of your Storm cluster.</span></span> <span data-ttu-id="2ce48-253">Om du uppmanas ange HDInsight-kluster administratör (admin) namnet och lösenordet du angav när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-253">If prompted, enter the HDInsight cluster administrator (admin) name and password you specified when creating the cluster.</span></span>
        2. <span data-ttu-id="2ce48-254">Välj den topologi som du inte vill att balansera och välj sedan den **balansera** knappen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-254">Select the topology you wish to rebalance, then select the **Rebalance** button.</span></span> <span data-ttu-id="2ce48-255">Ange fördröjningen innan balansera åtgärden utförs.</span><span class="sxs-lookup"><span data-stu-id="2ce48-255">Enter the delay before the rebalance operation is performed.</span></span>

<span data-ttu-id="2ce48-256">Specifik information om att skala HDInsight-kluster, se:</span><span class="sxs-lookup"><span data-stu-id="2ce48-256">For specific information on scaling your HDInsight cluster, see:</span></span>

* [<span data-ttu-id="2ce48-257">Hantera Hadoop-kluster i HDInsight med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="2ce48-257">Manage Hadoop clusters in HDInsight by using the Azure portal</span></span>](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [<span data-ttu-id="2ce48-258">Hantera Hadoop-kluster i HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ce48-258">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a><span data-ttu-id="2ce48-259">Hur installerar jag Hue (eller andra Hadoop-komponenter)?</span><span class="sxs-lookup"><span data-stu-id="2ce48-259">How do I install Hue (or other Hadoop component)?</span></span>

<span data-ttu-id="2ce48-260">HDInsight är en hanterad tjänst.</span><span class="sxs-lookup"><span data-stu-id="2ce48-260">HDInsight is a managed service.</span></span> <span data-ttu-id="2ce48-261">Om Azure upptäcker ett problem med klustret, kan det ta bort noden misslyckas och skapa en nod för att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="2ce48-261">If Azure detects a problem with the cluster, it may delete the failing node and create a node to replace it.</span></span> <span data-ttu-id="2ce48-262">Du kan manuellt installera saker på klustret, är inte beständiga när den här åtgärden genomförs.</span><span class="sxs-lookup"><span data-stu-id="2ce48-262">If you manually install things on the cluster, they are not persisted when this operation occurs.</span></span> <span data-ttu-id="2ce48-263">Använd i stället [HDInsight-skriptåtgärder](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="2ce48-263">Instead, use [HDInsight Script Actions](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="2ce48-264">En skriptåtgärd kan användas för att göra följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="2ce48-264">A script action can be used to make the following changes:</span></span>

* <span data-ttu-id="2ce48-265">Installera och konfigurera en tjänst eller en webbplats till exempel Spark eller Hue.</span><span class="sxs-lookup"><span data-stu-id="2ce48-265">Install and configure a service or web site such as Spark or Hue.</span></span>
* <span data-ttu-id="2ce48-266">Installera och konfigurera en komponent som kräver konfigurationsändringar på flera noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-266">Install and configure a component that requires configuration changes on multiple nodes in the cluster.</span></span> <span data-ttu-id="2ce48-267">Till exempel en obligatorisk miljövariabel, skapa en loggningskatalog eller skapa en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="2ce48-267">For example, a required environment variable, creating of a logging directory, or creation of a configuration file.</span></span>

<span data-ttu-id="2ce48-268">Skriptåtgärder är Bash-skript.</span><span class="sxs-lookup"><span data-stu-id="2ce48-268">Script Actions are Bash scripts.</span></span> <span data-ttu-id="2ce48-269">Skript körs under klusteretablering och kan användas för att installera och konfigurera ytterligare komponenter i klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-269">The scripts run during cluster provisioning, and can be used to install and configure additional components on the cluster.</span></span> <span data-ttu-id="2ce48-270">Exempelskript tillhandahålls för att installera följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="2ce48-270">Example scripts are provided for installing the following components:</span></span>

* [<span data-ttu-id="2ce48-271">Hue</span><span class="sxs-lookup"><span data-stu-id="2ce48-271">Hue</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="2ce48-272">Giraph</span><span class="sxs-lookup"><span data-stu-id="2ce48-272">Giraph</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="2ce48-273">Solr</span><span class="sxs-lookup"><span data-stu-id="2ce48-273">Solr</span></span>](hdinsight-hadoop-solr-install-linux.md)

<span data-ttu-id="2ce48-274">Information om hur du utvecklar dina egna skriptåtgärder finns i [Utveckling av skriptåtgärder med HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2ce48-274">For information on developing your own Script Actions, see [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>

### <a name="jar-files"></a><span data-ttu-id="2ce48-275">JAR-filer</span><span class="sxs-lookup"><span data-stu-id="2ce48-275">Jar files</span></span>

<span data-ttu-id="2ce48-276">Vissa Hadoop-teknik finns i självständiga jar-filer som innehåller funktioner som används som en del av ett MapReduce-jobb, eller från inuti Pig- eller Hive.</span><span class="sxs-lookup"><span data-stu-id="2ce48-276">Some Hadoop technologies are provided in self-contained jar files that contain functions used as part of a MapReduce job, or from inside Pig or Hive.</span></span> <span data-ttu-id="2ce48-277">När dessa kan installeras med hjälp av skriptåtgärder, kan de ofta kräver inte några inställningar och överföras till klustret när du har etablerat och användas direkt.</span><span class="sxs-lookup"><span data-stu-id="2ce48-277">While these can be installed using Script Actions, they often don't require any setup and can be uploaded to the cluster after provisioning and used directly.</span></span> <span data-ttu-id="2ce48-278">Om du vill kontrollera att komponenten FRISKRIVNINGEN återställning av avbildning i klustret kan du lagra jar-filen i standardlagring för klustret (WASB eller ADL).</span><span class="sxs-lookup"><span data-stu-id="2ce48-278">If you want to make sure the component survives reimaging of the cluster, you can store the jar file in the default storage for your cluster (WASB or ADL).</span></span>

<span data-ttu-id="2ce48-279">Om du vill använda den senaste versionen av till exempel [DataFu](http://datafu.incubator.apache.org/), du kan hämta en jar som innehåller projektet och överföra den till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="2ce48-279">For example, if you want to use the latest version of [DataFu](http://datafu.incubator.apache.org/), you can download a jar containing the project and upload it to the HDInsight cluster.</span></span> <span data-ttu-id="2ce48-280">Följ sedan DataFu-dokumentationen om hur du använder från Pig eller Hive.</span><span class="sxs-lookup"><span data-stu-id="2ce48-280">Then follow the DataFu documentation on how to use it from Pig or Hive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2ce48-281">Vissa komponenter som är fristående jar-filer med HDInsight, men är inte i sökvägen.</span><span class="sxs-lookup"><span data-stu-id="2ce48-281">Some components that are standalone jar files are provided with HDInsight, but are not in the path.</span></span> <span data-ttu-id="2ce48-282">Om du letar efter en viss komponent använder du följande för att söka efter den i klustret:</span><span class="sxs-lookup"><span data-stu-id="2ce48-282">If you are looking for a specific component, you can use the follow to search for it on your cluster:</span></span>
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> <span data-ttu-id="2ce48-283">Det här kommandot returnerar sökvägen till några matchande jar-filer.</span><span class="sxs-lookup"><span data-stu-id="2ce48-283">This command returns the path of any matching jar files.</span></span>

<span data-ttu-id="2ce48-284">Överför den version som du behöver och använda i dina jobb för att använda en annan version av en komponent.</span><span class="sxs-lookup"><span data-stu-id="2ce48-284">To use a different version of a component, upload the version you need and use it in your jobs.</span></span>

> [!WARNING]
> <span data-ttu-id="2ce48-285">Komponenter som ingår i HDInsight-kluster stöds fullt ut och Microsoft-supporten hjälper att isolera och lösa problem relaterade till komponenterna.</span><span class="sxs-lookup"><span data-stu-id="2ce48-285">Components provided with the HDInsight cluster are fully supported and Microsoft Support helps to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="2ce48-286">Anpassade komponenter få kommersiellt rimliga stöd för att hjälpa dig att felsöka problemet ytterligare.</span><span class="sxs-lookup"><span data-stu-id="2ce48-286">Custom components receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="2ce48-287">Detta kan resultera i att lösa problemet eller där du uppmanas att engagera tillgängliga kanaler för öppen källkod där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="2ce48-287">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="2ce48-288">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="2ce48-288">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="2ce48-289">Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="2ce48-289">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ce48-290">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ce48-290">Next steps</span></span>

* [<span data-ttu-id="2ce48-291">Migrera från Windows-baserade HDInsight till Linux-baserade</span><span class="sxs-lookup"><span data-stu-id="2ce48-291">Migrate from Windows-based HDInsight to Linux-based</span></span>](hdinsight-migrate-from-windows-to-linux.md)
* [<span data-ttu-id="2ce48-292">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="2ce48-292">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="2ce48-293">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="2ce48-293">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="2ce48-294">Använda MapReduce-jobb med HDInsight</span><span class="sxs-lookup"><span data-stu-id="2ce48-294">Use MapReduce jobs with HDInsight</span></span>](hdinsight-use-mapreduce.md)
