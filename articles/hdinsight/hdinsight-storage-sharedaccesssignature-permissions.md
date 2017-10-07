---
title: "aaaRestrict åtkomst med signaturer för delad åtkomst - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse signaturer för delad åtkomst toorestrict HDInsight åt toodata lagras i Azure storage BLOB."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: a34a2f8e52e47a15b09f09bc1fc67fc6159ec75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-shared-access-signatures-toorestrict-access-toodata-in-hdinsight"></a><span data-ttu-id="3f83a-103">Använd Azure Storage signaturer för delad åtkomst toorestrict åtkomst toodata i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-103">Use Azure Storage Shared Access Signatures toorestrict access toodata in HDInsight</span></span>

<span data-ttu-id="3f83a-104">HDInsight har fullständig åtkomst toodata i hello Azure Storage-konton som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-104">HDInsight has full access toodata in hello Azure Storage accounts associated with hello cluster.</span></span> <span data-ttu-id="3f83a-105">Du kan använda signaturer för delad åtkomst på hello blob-behållaren toorestrict åtkomst toohello data.</span><span class="sxs-lookup"><span data-stu-id="3f83a-105">You can use Shared Access Signatures on hello blob container toorestrict access toohello data.</span></span> <span data-ttu-id="3f83a-106">Till exempel tooprovide läsbehörighet toohello data.</span><span class="sxs-lookup"><span data-stu-id="3f83a-106">For example, tooprovide read-only access toohello data.</span></span> <span data-ttu-id="3f83a-107">Delad åtkomst signaturer (SAS) är en funktion i Azure storage-konton som du kan använda toolimit åtkomst toodata.</span><span class="sxs-lookup"><span data-stu-id="3f83a-107">Shared Access Signatures (SAS) are a feature of Azure storage accounts that allows you toolimit access toodata.</span></span> <span data-ttu-id="3f83a-108">Till exempel tillhandahåller skrivskyddad åtkomst toodata.</span><span class="sxs-lookup"><span data-stu-id="3f83a-108">For example, providing read-only access toodata.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f83a-109">Överväg att använda domänanslutna HDInsight för en lösning som använder Apache Ranger.</span><span class="sxs-lookup"><span data-stu-id="3f83a-109">For a solution using Apache Ranger, consider using domain-joined HDInsight.</span></span> <span data-ttu-id="3f83a-110">Mer information finns i hello [konfigurera domänanslutna HDInsight](hdinsight-domain-joined-configure.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3f83a-110">For more information, see hello [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) document.</span></span>

> [!WARNING]
> <span data-ttu-id="3f83a-111">HDInsight måste ha fullständig åtkomst toohello standard lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3f83a-111">HDInsight must have full access toohello default storage for hello cluster.</span></span>

## <a name="requirements"></a><span data-ttu-id="3f83a-112">Krav</span><span class="sxs-lookup"><span data-stu-id="3f83a-112">Requirements</span></span>

* <span data-ttu-id="3f83a-113">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f83a-113">An Azure subscription</span></span>
* <span data-ttu-id="3f83a-114">C# eller Python.</span><span class="sxs-lookup"><span data-stu-id="3f83a-114">C# or Python.</span></span> <span data-ttu-id="3f83a-115">C#-kodexempel anges som ett Visual Studio-lösning.</span><span class="sxs-lookup"><span data-stu-id="3f83a-115">C# example code is provided as a Visual Studio solution.</span></span>

  * <span data-ttu-id="3f83a-116">Visual Studio måste vara version 2013 eller 2015 2017</span><span class="sxs-lookup"><span data-stu-id="3f83a-116">Visual Studio must be version 2013, 2015, or 2017</span></span>
  * <span data-ttu-id="3f83a-117">Python måste vara version 2.7 eller högre</span><span class="sxs-lookup"><span data-stu-id="3f83a-117">Python must be version 2.7 or higher</span></span>

* <span data-ttu-id="3f83a-118">Ett Linux-baserade HDInsight-kluster eller [Azure PowerShell] [ powershell] -om du har ett befintligt Linux-baserade kluster, kan du använda Ambari tooadd en signatur för delad åtkomst toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="3f83a-118">A Linux-based HDInsight cluster OR [Azure PowerShell][powershell] - If you have an existing Linux-based cluster, you can use Ambari tooadd a Shared Access Signature toohello cluster.</span></span> <span data-ttu-id="3f83a-119">Om inte, kan du använda Azure PowerShell toocreate ett kluster och lägga till en signatur för delad åtkomst när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3f83a-119">If not, you can use Azure PowerShell toocreate a cluster and add a Shared Access Signature during cluster creation.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3f83a-120">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3f83a-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3f83a-121">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3f83a-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="3f83a-122">Hej exempel filer från [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="3f83a-122">hello example files from [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span></span> <span data-ttu-id="3f83a-123">Den här lagringsplatsen innehåller hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3f83a-123">This repository contains hello following items:</span></span>

  * <span data-ttu-id="3f83a-124">Ett Visual Studio-projekt som kan skapa en lagringsbehållare, lagrade principer och SAS för användning med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-124">A Visual Studio project that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="3f83a-125">Python-skriptet som kan skapa en lagringsbehållare, lagrade principer och SAS för användning med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-125">A Python script that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="3f83a-126">Ett PowerShell-skript som kan skapa ett HDInsight-kluster och konfigurera den toouse hello SAS.</span><span class="sxs-lookup"><span data-stu-id="3f83a-126">A PowerShell script that can create a HDInsight cluster and configure it toouse hello SAS.</span></span>

## <a name="shared-access-signatures"></a><span data-ttu-id="3f83a-127">Signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="3f83a-127">Shared Access Signatures</span></span>

<span data-ttu-id="3f83a-128">Det finns två typer av signaturer för delad åtkomst:</span><span class="sxs-lookup"><span data-stu-id="3f83a-128">There are two forms of Shared Access Signatures:</span></span>

* <span data-ttu-id="3f83a-129">Ad hoc-: hello starttid, förfallotiden och behörigheter för hello SAS har angetts för hello SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="3f83a-129">Ad hoc: hello start time, expiry time, and permissions for hello SAS are all specified on hello SAS URI.</span></span>

* <span data-ttu-id="3f83a-130">Lagras åtkomstprincip: en lagrade princip har definierats för en resurs-behållare, till exempel en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="3f83a-130">Stored access policy: A stored access policy is defined on a resource container, such as a blob container.</span></span> <span data-ttu-id="3f83a-131">En princip kan vara används toomanage begränsningarna för en eller flera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3f83a-131">A policy can be used toomanage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="3f83a-132">När du kopplar en SAS med en lagrad till hello SAS ärver begränsningarna hello - hello starttid, förfallotiden samt behörigheter - har definierats för hello lagras åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="3f83a-132">When you associate a SAS with a stored access policy, hello SAS inherits hello constraints - hello start time, expiry time, and permissions - defined for hello stored access policy.</span></span>

<span data-ttu-id="3f83a-133">hello skillnaden mellan hello två formulär är viktigt för ett key-scenario: återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="3f83a-133">hello difference between hello two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="3f83a-134">En SAS är en URL så att alla som erhåller hello SAS kan använda det, oavsett vem som har begärt det toobegin med.</span><span class="sxs-lookup"><span data-stu-id="3f83a-134">A SAS is a URL, so anyone who obtains hello SAS can use it, regardless of who requested it toobegin with.</span></span> <span data-ttu-id="3f83a-135">Om en SAS publiceras offentligt, kan den användas av alla i hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="3f83a-135">If a SAS is published publicly, it can be used by anyone in hello world.</span></span> <span data-ttu-id="3f83a-136">En SAS som distribueras är giltig förrän något av följande händer:</span><span class="sxs-lookup"><span data-stu-id="3f83a-136">A SAS that is distributed is valid until one of four things happens:</span></span>

1. <span data-ttu-id="3f83a-137">hello förfallotiden anges på hello SAS har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="3f83a-137">hello expiry time specified on hello SAS is reached.</span></span>

2. <span data-ttu-id="3f83a-138">hello förfallotiden har angetts på hello lagras åtkomstprincip som refereras av hello SAS har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="3f83a-138">hello expiry time specified on hello stored access policy referenced by hello SAS is reached.</span></span> <span data-ttu-id="3f83a-139">hello följande scenarier orsaka hello förfallotiden toobe uppnåtts:</span><span class="sxs-lookup"><span data-stu-id="3f83a-139">hello following scenarios cause hello expiry time toobe reached:</span></span>

    * <span data-ttu-id="3f83a-140">hello tidsintervall har gått ut.</span><span class="sxs-lookup"><span data-stu-id="3f83a-140">hello time interval has elapsed.</span></span>
    * <span data-ttu-id="3f83a-141">hello lagras åtkomstprincipen är ändrade toohave en förfallotiden i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="3f83a-141">hello stored access policy is modified toohave an expiry time in hello past.</span></span> <span data-ttu-id="3f83a-142">Ändra hello förfallotiden är enkelriktade toorevoke hello SAS.</span><span class="sxs-lookup"><span data-stu-id="3f83a-142">Changing hello expiry time is one way toorevoke hello SAS.</span></span>

3. <span data-ttu-id="3f83a-143">hello lagras åtkomstprincip som refereras av hello SAS tas bort, vilket är ett annat sätt toorevoke hello SAS.</span><span class="sxs-lookup"><span data-stu-id="3f83a-143">hello stored access policy referenced by hello SAS is deleted, which is another way toorevoke hello SAS.</span></span> <span data-ttu-id="3f83a-144">Om du återskapa hello lagras åtkomstprincip med samma namn, SAS-token för hello hello föregående principen gäller (om hello förfallotiden på hello SAS inte har passerats).</span><span class="sxs-lookup"><span data-stu-id="3f83a-144">If you recreate hello stored access policy with hello same name, all  SAS tokens for hello previous policy are valid (if hello expiry time on hello SAS has not passed).</span></span> <span data-ttu-id="3f83a-145">Om du avser toorevoke hello SAS vara säker på att toouse ett annat namn om du återskapa hello åtkomstprincip med en förfallotiden i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="3f83a-145">If you intend toorevoke hello SAS, be sure toouse a different name if you recreate hello access policy with an expiry time in hello future.</span></span>

4. <span data-ttu-id="3f83a-146">Hej kontonyckel som har använt toocreate hello SAS genereras.</span><span class="sxs-lookup"><span data-stu-id="3f83a-146">hello account key that was used toocreate hello SAS is regenerated.</span></span> <span data-ttu-id="3f83a-147">Återskapar hello nyckeln gör alla program som använder hello tidigare viktiga toofail autentisering.</span><span class="sxs-lookup"><span data-stu-id="3f83a-147">Regenerating hello key causes all applications that use hello previous key toofail authentication.</span></span> <span data-ttu-id="3f83a-148">Uppdatera alla komponenter toohello ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="3f83a-148">Update all components toohello new key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f83a-149">En signatur för delad åtkomst URI är associerad med hello konto nyckel används toocreate hello signatur och hello associerad lagrade åtkomstprincip (eventuella).</span><span class="sxs-lookup"><span data-stu-id="3f83a-149">A shared access signature URI is associated with hello account key used toocreate hello signature, and hello associated stored access policy (if any).</span></span> <span data-ttu-id="3f83a-150">Om inga lagrade åtkomstprincip anges är hello endast sätt toorevoke en signatur för delad åtkomst toochange hello kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="3f83a-150">If no stored access policy is specified, hello only way toorevoke a shared access signature is toochange hello account key.</span></span>

<span data-ttu-id="3f83a-151">Vi rekommenderar att du alltid använder lagrade åtkomstprinciper.</span><span class="sxs-lookup"><span data-stu-id="3f83a-151">We recommend that you always use stored access policies.</span></span> <span data-ttu-id="3f83a-152">När du använder lagrade principer, kan du återkalla signaturer eller utöka hello utgångsdatum efter behov.</span><span class="sxs-lookup"><span data-stu-id="3f83a-152">When using stored policies, you can either revoke signatures or extend hello expiry date as needed.</span></span> <span data-ttu-id="3f83a-153">hello stegen i det här dokumentet använder lagrade åtkomst principer toogenerate SAS.</span><span class="sxs-lookup"><span data-stu-id="3f83a-153">hello steps in this document use stored access policies toogenerate SAS.</span></span>

<span data-ttu-id="3f83a-154">Mer information om signaturer för delad åtkomst finns [förstå hello SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="3f83a-154">For more information on Shared Access Signatures, see [Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

### <a name="create-a-stored-policy-and-sas-using-c"></a><span data-ttu-id="3f83a-155">Skapa en princip för lagrade och SAS med hjälp av C\#</span><span class="sxs-lookup"><span data-stu-id="3f83a-155">Create a stored policy and SAS using C\#</span></span>

1. <span data-ttu-id="3f83a-156">Öppna hello lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f83a-156">Open hello solution in Visual Studio.</span></span>

2. <span data-ttu-id="3f83a-157">I Solution Explorer högerklickar du på hello **SASToken** projektet och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3f83a-157">In Solution Explorer, right-click on hello **SASToken** project and select **Properties**.</span></span>

3. <span data-ttu-id="3f83a-158">Välj **inställningar** och lägga till värden för följande poster hello:</span><span class="sxs-lookup"><span data-stu-id="3f83a-158">Select **Settings** and add values for hello following entries:</span></span>

   * <span data-ttu-id="3f83a-159">StorageConnectionString: hello anslutningssträngen för hello storage-konto som du vill toocreate lagrade politiska och SAS för.</span><span class="sxs-lookup"><span data-stu-id="3f83a-159">StorageConnectionString: hello connection string for hello storage account that you want toocreate a stored policy and SAS for.</span></span> <span data-ttu-id="3f83a-160">hello format bör vara `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` där `myaccount` är hello namnet på ditt lagringskonto och `mykey` är hello nyckel för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3f83a-160">hello format should be `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` where `myaccount` is hello name of your storage account and `mykey` is hello key for hello storage account.</span></span>

   * <span data-ttu-id="3f83a-161">ContainerName: hello behållare i hello storage-konto som du vill toorestrict åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="3f83a-161">ContainerName: hello container in hello storage account that you want toorestrict access to.</span></span>

   * <span data-ttu-id="3f83a-162">SASPolicyName: hello namnet toouse för hello lagras princip toocreate.</span><span class="sxs-lookup"><span data-stu-id="3f83a-162">SASPolicyName: hello name toouse for hello stored policy toocreate.</span></span>

   * <span data-ttu-id="3f83a-163">FileToUpload: hello sökvägen tooa fil som är överförda toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="3f83a-163">FileToUpload: hello path tooa file that is uploaded toohello container.</span></span>

4. <span data-ttu-id="3f83a-164">Kör hello-projektet.</span><span class="sxs-lookup"><span data-stu-id="3f83a-164">Run hello project.</span></span> <span data-ttu-id="3f83a-165">Information liknande toohello följande text som visas när hello SAS har skapats:</span><span class="sxs-lookup"><span data-stu-id="3f83a-165">Information similar toohello following text is displayed once hello SAS has been generated:</span></span>

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="3f83a-166">Spara princip hello SAS-token, lagringskontonamnet och behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="3f83a-166">Save hello SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="3f83a-167">Dessa värden används när du associerar hello storage-konto med ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-167">These values are used when associating hello storage account with your HDInsight cluster.</span></span>

### <a name="create-a-stored-policy-and-sas-using-python"></a><span data-ttu-id="3f83a-168">Skapa en princip för lagrade och SAS använder Python</span><span class="sxs-lookup"><span data-stu-id="3f83a-168">Create a stored policy and SAS using Python</span></span>

1. <span data-ttu-id="3f83a-169">Öppna hello SASToken.py filen och ändra hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="3f83a-169">Open hello SASToken.py file and change hello following values:</span></span>

   * <span data-ttu-id="3f83a-170">principen\_name: hello namnet toouse för hello lagras princip toocreate.</span><span class="sxs-lookup"><span data-stu-id="3f83a-170">policy\_name: hello name toouse for hello stored policy toocreate.</span></span>

   * <span data-ttu-id="3f83a-171">lagring\_konto\_name: hello namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3f83a-171">storage\_account\_name: hello name of your storage account.</span></span>

   * <span data-ttu-id="3f83a-172">lagring\_konto\_nyckel: hello nyckel för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3f83a-172">storage\_account\_key: hello key for hello storage account.</span></span>

   * <span data-ttu-id="3f83a-173">lagring\_behållare\_name: hello behållare i hello storage-konto som du vill toorestrict åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="3f83a-173">storage\_container\_name: hello container in hello storage account that you want toorestrict access to.</span></span>

   * <span data-ttu-id="3f83a-174">exempel\_filen\_sökväg: hello sökvägen tooa fil som är överförda toohello behållare.</span><span class="sxs-lookup"><span data-stu-id="3f83a-174">example\_file\_path: hello path tooa file that is uploaded toohello container.</span></span>

2. <span data-ttu-id="3f83a-175">Kör skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="3f83a-175">Run hello script.</span></span> <span data-ttu-id="3f83a-176">Hello SAS-token liknande toohello efter text när hello skriptet har slutförts visas:</span><span class="sxs-lookup"><span data-stu-id="3f83a-176">It displays hello SAS token similar toohello following text when hello script completes:</span></span>

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="3f83a-177">Spara princip hello SAS-token, lagringskontonamnet och behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="3f83a-177">Save hello SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="3f83a-178">Dessa värden används när du associerar hello storage-konto med ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-178">These values are used when associating hello storage account with your HDInsight cluster.</span></span>

## <a name="use-hello-sas-with-hdinsight"></a><span data-ttu-id="3f83a-179">Använda hello SAS med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-179">Use hello SAS with HDInsight</span></span>

<span data-ttu-id="3f83a-180">När du skapar ett HDInsight-kluster, måste du ange en primär storage-konto och du kan också ange ytterligare lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="3f83a-180">When creating an HDInsight cluster, you must specify a primary storage account and you can optionally specify additional storage accounts.</span></span> <span data-ttu-id="3f83a-181">Båda dessa metoder för att lägga till lagring kräver fullständig åtkomst toohello storage-konton och behållare som används.</span><span class="sxs-lookup"><span data-stu-id="3f83a-181">Both of these methods of adding storage require full access toohello storage accounts and containers that are used.</span></span>

<span data-ttu-id="3f83a-182">toouse en signatur för delad åtkomst toolimit åtkomst tooa behållare, lägga till en anpassad post toohello **core-plats** konfigurationen för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-182">toouse a Shared Access Signature toolimit access tooa container, add a custom entry toohello **core-site** configuration for hello cluster.</span></span>

* <span data-ttu-id="3f83a-183">För **Windows-baserade** eller **Linux-baserade** HDInsight-kluster, du kan lägga till hello-post när klustret skapas med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3f83a-183">For **Windows-based** or **Linux-based** HDInsight clusters, you can add hello entry during cluster creation using PowerShell.</span></span>
* <span data-ttu-id="3f83a-184">För **Linux-baserade** HDInsight-kluster, ändra hello konfiguration efter att skapa ett kluster med Ambari.</span><span class="sxs-lookup"><span data-stu-id="3f83a-184">For **Linux-based** HDInsight clusters, change hello configuration after cluster creation using Ambari.</span></span>

### <a name="create-a-cluster-that-uses-hello-sas"></a><span data-ttu-id="3f83a-185">Skapa ett kluster som använder hello SAS</span><span class="sxs-lookup"><span data-stu-id="3f83a-185">Create a cluster that uses hello SAS</span></span>

<span data-ttu-id="3f83a-186">Ett exempel på hur du skapar ett HDInsight-kluster som använder hello SAS ingår i hello `CreateCluster` för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3f83a-186">An example of creating an HDInsight cluster that uses hello SAS is included in hello `CreateCluster` directory of hello repository.</span></span> <span data-ttu-id="3f83a-187">toouse det, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f83a-187">toouse it, use hello following steps:</span></span>

1. <span data-ttu-id="3f83a-188">Öppna hello `CreateCluster\HDInsightSAS.ps1` filen i en textredigerare och ändra följande värden hello början av dokumentet hello hello.</span><span class="sxs-lookup"><span data-stu-id="3f83a-188">Open hello `CreateCluster\HDInsightSAS.ps1` file in a text editor and modify hello following values at hello beginning of hello document.</span></span>

    ```powershell
    # Replace 'mycluster' with hello name of hello cluster toobe created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with hello name of hello group toobe created
    $resourceGroupName = 'myresourcegroup'
    # Replace with hello Azure data center you want toohello cluster toolive in
    $location = 'North Europe'
    # Replace with hello name of hello default storage account toobe created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with hello name of hello SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with hello name of hello SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with hello SAS token generated earlier
    $SASToken = 'sastoken'
    # Set hello number of worker nodes in hello cluster
    $clusterSizeInNodes = 3
    ```

    <span data-ttu-id="3f83a-189">Till exempel ändra `'mycluster'` toohello namn hello klustret som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="3f83a-189">For example, change `'mycluster'` toohello name of hello cluster you want toocreate.</span></span> <span data-ttu-id="3f83a-190">hello SAS-värden måste matcha hello värden från hello föregående steg när du skapar ett lagringskonto och SAS-token.</span><span class="sxs-lookup"><span data-stu-id="3f83a-190">hello SAS values should match hello values from hello previous steps when creating a storage account and SAS token.</span></span>

    <span data-ttu-id="3f83a-191">När du har ändrat hello värden, spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3f83a-191">Once you have changed hello values, save hello file.</span></span>

2. <span data-ttu-id="3f83a-192">Öppna en ny Azure PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="3f83a-192">Open a new Azure PowerShell prompt.</span></span> <span data-ttu-id="3f83a-193">Om du inte känner till Azure PowerShell eller inte har installerat den, se [installera och konfigurera Azure PowerShell][powershell].</span><span class="sxs-lookup"><span data-stu-id="3f83a-193">If you are unfamiliar with Azure PowerShell, or have not installed it, see [Install and configure Azure PowerShell][powershell].</span></span>

1. <span data-ttu-id="3f83a-194">Använd hello efter kommandot tooauthenticate tooyour Azure-prenumeration från hello-prompten:</span><span class="sxs-lookup"><span data-stu-id="3f83a-194">From hello prompt, use hello following command tooauthenticate tooyour Azure subscription:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="3f83a-195">När du uppmanas logga in med hello-konto för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3f83a-195">When prompted, log in with hello account for your Azure subscription.</span></span>

    <span data-ttu-id="3f83a-196">Om ditt konto är kopplat till flera Azure-prenumerationer måste du eventuellt toouse `Select-AzureRmSubscription` tooselect hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="3f83a-196">If your account is associated with multiple Azure subscriptions, you may need toouse `Select-AzureRmSubscription` tooselect hello subscription you wish toouse.</span></span>

4. <span data-ttu-id="3f83a-197">Hello-Kommandotolken, ändra kataloger toohello `CreateCluster` katalog som innehåller hello HDInsightSAS.ps1 fil.</span><span class="sxs-lookup"><span data-stu-id="3f83a-197">From hello prompt, change directories toohello `CreateCluster` directory that contains hello HDInsightSAS.ps1 file.</span></span> <span data-ttu-id="3f83a-198">Använd följande kommandoskript toorun hello hello</span><span class="sxs-lookup"><span data-stu-id="3f83a-198">Then use hello following command toorun hello script</span></span>

    ```powershell
    .\HDInsightSAS.ps1
    ```

    <span data-ttu-id="3f83a-199">När hello skript körs loggas utdata toohello PowerShell-Kommandotolken eftersom den skapar hello resurs grupp och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="3f83a-199">As hello script runs, it logs output toohello PowerShell prompt as it creates hello resource group and storage accounts.</span></span> <span data-ttu-id="3f83a-200">Du kan ange tooenter hello HTTP användaren hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-200">You are prompted tooenter hello HTTP user for hello HDInsight cluster.</span></span> <span data-ttu-id="3f83a-201">Det här kontot är används toosecure HTTP/s åtkomst toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-201">This account is used toosecure HTTP/s access toohello cluster.</span></span>

    <span data-ttu-id="3f83a-202">Om du skapar ett Linux-baserade kluster, efterfrågas ett SSH-konto användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3f83a-202">If you are creating a Linux-based cluster, you are prompted for an SSH user account name and password.</span></span> <span data-ttu-id="3f83a-203">Det här kontot är används tooremotely logg i toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-203">This account is used tooremotely log in toohello cluster.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="3f83a-204">När du uppmanas att ange hello HTTP/s eller SSH-användarnamn och lösenord, måste du ange ett lösenord som uppfyller följande kriterier hello:</span><span class="sxs-lookup"><span data-stu-id="3f83a-204">When prompted for hello HTTP/s or SSH user name and password, you must provide a password that meets hello following criteria:</span></span>
   >
   > * <span data-ttu-id="3f83a-205">Måste vara minst 10 tecken</span><span class="sxs-lookup"><span data-stu-id="3f83a-205">Must be at least 10 characters in length</span></span>
   > * <span data-ttu-id="3f83a-206">Måste innehålla minst en siffra</span><span class="sxs-lookup"><span data-stu-id="3f83a-206">Must contain at least one digit</span></span>
   > * <span data-ttu-id="3f83a-207">Måste innehålla minst ett icke-alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="3f83a-207">Must contain at least one non-alphanumeric character</span></span>
   > * <span data-ttu-id="3f83a-208">Måste innehålla minst en versal eller gemen bokstav</span><span class="sxs-lookup"><span data-stu-id="3f83a-208">Must contain at least one upper or lower case letter</span></span>

<span data-ttu-id="3f83a-209">Det tar en stund innan det här skriptet toocomplete vanligtvis cirka 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="3f83a-209">It takes a while for this script toocomplete, usually around 15 minutes.</span></span> <span data-ttu-id="3f83a-210">När hello skriptet har slutförts utan fel har hello klustret skapats.</span><span class="sxs-lookup"><span data-stu-id="3f83a-210">When hello script completes without any errors, hello cluster has been created.</span></span>

### <a name="use-hello-sas-with-an-existing-cluster"></a><span data-ttu-id="3f83a-211">Använda hello SAS med ett befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="3f83a-211">Use hello SAS with an existing cluster</span></span>

<span data-ttu-id="3f83a-212">Om du har ett befintligt Linux-baserade kluster kan du lägga till hello SAS toohello **core-plats** konfiguration med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f83a-212">If you have an existing Linux-based cluster, you can add hello SAS toohello **core-site** configuration by using hello following steps:</span></span>

1. <span data-ttu-id="3f83a-213">Öppna hello Ambari-webbgränssnittet för klustret.</span><span class="sxs-lookup"><span data-stu-id="3f83a-213">Open hello Ambari web UI for your cluster.</span></span> <span data-ttu-id="3f83a-214">hello-adressen för den här sidan är https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="3f83a-214">hello address for this page is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="3f83a-215">När du uppmanas att autentisera toohello kluster med hjälp av namn på serveradministratör hello (admin) och lösenord som du använde när du skapar hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-215">When prompted, authenticate toohello cluster using hello admin name (admin) and password you used when creating hello cluster.</span></span>

2. <span data-ttu-id="3f83a-216">Hello vänster sida av hello Ambari-webbgränssnittet, Välj **HDFS** och välj sedan hello **konfigurationerna** fliken hello mitten av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="3f83a-216">From hello left side of hello Ambari web UI, select **HDFS** and then select hello **Configs** tab in hello middle of hello page.</span></span>

3. <span data-ttu-id="3f83a-217">Välj hello **Avancerat** , och Bläddra tills du hittar hello **anpassad core-plats** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3f83a-217">Select hello **Advanced** tab, and then scroll until you find hello **Custom core-site** section.</span></span>

4. <span data-ttu-id="3f83a-218">Expandera hello **anpassad core-plats** avsnittet, och sedan rullar toohello slut och välj hello **Lägg till egenskap...**  länk.</span><span class="sxs-lookup"><span data-stu-id="3f83a-218">Expand hello **Custom core-site** section, then scroll toohello end and select hello **Add property...** link.</span></span> <span data-ttu-id="3f83a-219">Använd hello följande värden för hello **nyckeln** och **värdet** fält:</span><span class="sxs-lookup"><span data-stu-id="3f83a-219">Use hello following values for hello **Key** and **Value** fields:</span></span>

   * <span data-ttu-id="3f83a-220">**Nyckeln**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="3f83a-220">**Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span></span>
   * <span data-ttu-id="3f83a-221">**Värdet**: hello SAS som returneras av hello C# eller Python-program som du tidigare körde</span><span class="sxs-lookup"><span data-stu-id="3f83a-221">**Value**: hello SAS returned by hello C# or Python application you ran previously</span></span>

     <span data-ttu-id="3f83a-222">Ersätt **CONTAINERNAME** med hello behållarens namn används med hello C# eller SAS-program.</span><span class="sxs-lookup"><span data-stu-id="3f83a-222">Replace **CONTAINERNAME** with hello container name you used with hello C# or SAS application.</span></span> <span data-ttu-id="3f83a-223">Ersätt **STORAGEACCOUNTNAME** med hello behållarens kontonamn som du använde.</span><span class="sxs-lookup"><span data-stu-id="3f83a-223">Replace **STORAGEACCOUNTNAME** with hello storage account name you used.</span></span>

5. <span data-ttu-id="3f83a-224">Klicka på hello **Lägg till** knappen toosave detta nyckel och värde och klicka sedan på hello **spara** knappen toosave hello konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="3f83a-224">Click hello **Add** button toosave this key and value, then click hello **Save** button toosave hello configuration changes.</span></span> <span data-ttu-id="3f83a-225">När du uppmanas, Lägg till en beskrivning av hello ändring (”lägger till SAS-lagringsåtkomst” till exempel) och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3f83a-225">When prompted, add a description of hello change ("adding SAS storage access" for example) and then click **Save**.</span></span>

    <span data-ttu-id="3f83a-226">Klicka på **OK** när hello ändringar har utförts.</span><span class="sxs-lookup"><span data-stu-id="3f83a-226">Click **OK** when hello changes have been completed.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="3f83a-227">Du måste starta om flera tjänster innan hello ändringen börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="3f83a-227">You must restart several services before hello change takes effect.</span></span>

6. <span data-ttu-id="3f83a-228">I hello Ambari-webbgränssnittet, väljer **HDFS** hello listan hello vänster och välj sedan **starta om alla** från hello **tjänståtgärder** listrutan på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="3f83a-228">In hello Ambari web UI, select **HDFS** from hello list on hello left, and then select **Restart All** from hello **Service Actions** drop down list on hello right.</span></span> <span data-ttu-id="3f83a-229">När du uppmanas, Välj **aktivera underhållsläge** och sedan väljer __Conform starta om alla ”.</span><span class="sxs-lookup"><span data-stu-id="3f83a-229">When prompted, select **Turn on maintenance mode** and then select __Conform Restart All".</span></span>

    <span data-ttu-id="3f83a-230">Upprepa processen för MapReduce2 och YARN.</span><span class="sxs-lookup"><span data-stu-id="3f83a-230">Repeat this process for MapReduce2 and YARN.</span></span>

7. <span data-ttu-id="3f83a-231">När hello tjänsterna har startats om, välja respektive och inaktivera underhållsläge från hello **tjänståtgärder** listrutan.</span><span class="sxs-lookup"><span data-stu-id="3f83a-231">Once hello services have restarted, select each one and disable maintenance mode from hello **Service Actions** drop down.</span></span>

## <a name="test-restricted-access"></a><span data-ttu-id="3f83a-232">Testa begränsad åtkomst</span><span class="sxs-lookup"><span data-stu-id="3f83a-232">Test restricted access</span></span>

<span data-ttu-id="3f83a-233">tooverify som du har begränsad åtkomst, användning hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="3f83a-233">tooverify that you have restricted access, use hello following methods:</span></span>

* <span data-ttu-id="3f83a-234">För **Windows-baserade** HDInsight-kluster, använda Fjärrskrivbord tooconnect toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-234">For **Windows-based** HDInsight clusters, use Remote Desktop tooconnect toohello cluster.</span></span> <span data-ttu-id="3f83a-235">Mer information finns i [ansluta tooHDInsight med](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="3f83a-235">For more information, see [Connect tooHDInsight using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

    <span data-ttu-id="3f83a-236">När du är ansluten, Använd hello **Hadoop kommandoradsverktyget** ikon på hello skrivbord tooopen en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="3f83a-236">Once connected, use hello **Hadoop Command-Line** icon on hello desktop tooopen a command prompt.</span></span>

* <span data-ttu-id="3f83a-237">För **Linux-baserade** HDInsight-kluster använder SSH tooconnect toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="3f83a-237">For **Linux-based** HDInsight clusters, use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="3f83a-238">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3f83a-238">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="3f83a-239">När du är ansluten toohello klustret Använd hello följande steg tooverify som du kan endast Läs- och objekt för hello SAS storage-konto:</span><span class="sxs-lookup"><span data-stu-id="3f83a-239">Once connected toohello cluster, use hello following steps tooverify that you can only read and list items on hello SAS storage account:</span></span>

1. <span data-ttu-id="3f83a-240">toolist hello innehållet i hello behållare, Använd hello följande kommando från hello prompten:</span><span class="sxs-lookup"><span data-stu-id="3f83a-240">toolist hello contents of hello container, use hello following command from hello prompt:</span></span> 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    <span data-ttu-id="3f83a-241">Ersätt **SASCONTAINER** med hello namnet hello-behållare som skapats för hello SAS storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3f83a-241">Replace **SASCONTAINER** with hello name of hello container created for hello SAS storage account.</span></span> <span data-ttu-id="3f83a-242">Ersätt **SASACCOUNTNAME** med hello namnet hello storage-konto som används för hello SAS.</span><span class="sxs-lookup"><span data-stu-id="3f83a-242">Replace **SASACCOUNTNAME** with hello name of hello storage account used for hello SAS.</span></span>

    <span data-ttu-id="3f83a-243">hello listan innehåller hello-fil som överförs när hello-behållaren och SAS skapades.</span><span class="sxs-lookup"><span data-stu-id="3f83a-243">hello list includes hello file uploaded when hello container and SAS were created.</span></span>

2. <span data-ttu-id="3f83a-244">Använd hello följande kommando tooverify att du kan läsa hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="3f83a-244">Use hello following command tooverify that you can read hello contents of hello file.</span></span> <span data-ttu-id="3f83a-245">Ersätt hello **SASCONTAINER** och **SASACCOUNTNAME** som hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="3f83a-245">Replace hello **SASCONTAINER** and **SASACCOUNTNAME** as in hello previous step.</span></span> <span data-ttu-id="3f83a-246">Ersätt **FILENAME** med hello namnet på hello-fil som visas i föregående hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="3f83a-246">Replace **FILENAME** with hello name of hello file displayed in hello previous command:</span></span>

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    <span data-ttu-id="3f83a-247">Det här kommandot visar hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="3f83a-247">This command lists hello contents of hello file.</span></span>

3. <span data-ttu-id="3f83a-248">Använd hello följande kommando lokalt filsystem för toodownload hello för filen toohello:</span><span class="sxs-lookup"><span data-stu-id="3f83a-248">Use hello following command toodownload hello file toohello local file system:</span></span>

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    <span data-ttu-id="3f83a-249">Det här kommandot hämtningar hello tooa lokal fil med namnet **testfile.txt**.</span><span class="sxs-lookup"><span data-stu-id="3f83a-249">This command downloads hello file tooa local file named **testfile.txt**.</span></span>

4. <span data-ttu-id="3f83a-250">Använd hello följande kommando tooupload hello lokal fil tooa ny fil med namnet **testupload.txt** på hello SAS-lagring:</span><span class="sxs-lookup"><span data-stu-id="3f83a-250">Use hello following command tooupload hello local file tooa new file named **testupload.txt** on hello SAS storage:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    <span data-ttu-id="3f83a-251">Du får ett meddelande liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="3f83a-251">You receive a message similar toohello following text:</span></span>

        put: java.io.IOException

    <span data-ttu-id="3f83a-252">Detta fel uppstår eftersom hello-lagringsplatsen är skrivskyddade + endast lista över tillåtna.</span><span class="sxs-lookup"><span data-stu-id="3f83a-252">This error occurs because hello storage location is read+list only.</span></span> <span data-ttu-id="3f83a-253">Använd hello följande kommando tooput hello data på hello standardlagring för hello-kluster, vilket är skrivbar:</span><span class="sxs-lookup"><span data-stu-id="3f83a-253">Use hello following command tooput hello data on hello default storage for hello cluster, which is writable:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    <span data-ttu-id="3f83a-254">Den här tiden kan ska hello åtgärden slutföras.</span><span class="sxs-lookup"><span data-stu-id="3f83a-254">This time, hello operation should complete successfully.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3f83a-255">Felsökning</span><span class="sxs-lookup"><span data-stu-id="3f83a-255">Troubleshooting</span></span>

### <a name="a-task-was-canceled"></a><span data-ttu-id="3f83a-256">En uppgift har avbrutits</span><span class="sxs-lookup"><span data-stu-id="3f83a-256">A task was canceled</span></span>

<span data-ttu-id="3f83a-257">**Symptom**: när du skapar ett kluster med hello PowerShell-skript får hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="3f83a-257">**Symptoms**: When creating a cluster using hello PowerShell script, you may receive hello following error message:</span></span>

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

<span data-ttu-id="3f83a-258">**Orsak**: det här felet kan inträffa om du använder ett lösenord för hello admin/http-användare för hello kluster, eller (för Linux-baserade kluster) hello SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="3f83a-258">**Cause**: This error can occur if you use a password for hello admin/HTTP user for hello cluster, or (for Linux-based clusters) hello SSH user.</span></span>

<span data-ttu-id="3f83a-259">**Lösning**: Använd ett lösenord som uppfyller följande kriterier hello:</span><span class="sxs-lookup"><span data-stu-id="3f83a-259">**Resolution**: Use a password that meets hello following criteria:</span></span>

* <span data-ttu-id="3f83a-260">Måste vara minst 10 tecken</span><span class="sxs-lookup"><span data-stu-id="3f83a-260">Must be at least 10 characters in length</span></span>
* <span data-ttu-id="3f83a-261">Måste innehålla minst en siffra</span><span class="sxs-lookup"><span data-stu-id="3f83a-261">Must contain at least one digit</span></span>
* <span data-ttu-id="3f83a-262">Måste innehålla minst ett icke-alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="3f83a-262">Must contain at least one non-alphanumeric character</span></span>
* <span data-ttu-id="3f83a-263">Måste innehålla minst en versal eller gemen bokstav</span><span class="sxs-lookup"><span data-stu-id="3f83a-263">Must contain at least one upper or lower case letter</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f83a-264">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f83a-264">Next steps</span></span>

<span data-ttu-id="3f83a-265">Nu när du har lärt dig hur tooadd lagring med begränsad åtkomst tooyour HDInsight-kluster Läs andra sätt toowork med data i klustret:</span><span class="sxs-lookup"><span data-stu-id="3f83a-265">Now that you have learned how tooadd limited-access storage tooyour HDInsight cluster, learn other ways toowork with data on your cluster:</span></span>

* [<span data-ttu-id="3f83a-266">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-266">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3f83a-267">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-267">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="3f83a-268">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3f83a-268">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
