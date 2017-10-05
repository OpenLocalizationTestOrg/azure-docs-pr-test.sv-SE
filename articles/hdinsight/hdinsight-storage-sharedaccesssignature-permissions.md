---
title: "Begränsa åtkomst med signaturer för delad åtkomst - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder signaturer för delad åtkomst för att begränsa HDInsight åtkomst till data som lagras i Azure storage BLOB."
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
ms.openlocfilehash: 2e4b1a307fae06c0639d93b9804c6f0f703d5900
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a><span data-ttu-id="53297-103">Använd Azure Storage signaturer för delad åtkomst för att begränsa åtkomsten till data i HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-103">Use Azure Storage Shared Access Signatures to restrict access to data in HDInsight</span></span>

<span data-ttu-id="53297-104">HDInsight har fullständig åtkomst till data i Azure Storage-konton som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-104">HDInsight has full access to data in the Azure Storage accounts associated with the cluster.</span></span> <span data-ttu-id="53297-105">Du kan använda signaturer för delad åtkomst på blob-behållaren för att begränsa åtkomsten till data.</span><span class="sxs-lookup"><span data-stu-id="53297-105">You can use Shared Access Signatures on the blob container to restrict access to the data.</span></span> <span data-ttu-id="53297-106">Till exempel för att ge skrivskyddad åtkomst till data.</span><span class="sxs-lookup"><span data-stu-id="53297-106">For example, to provide read-only access to the data.</span></span> <span data-ttu-id="53297-107">Delad åtkomst signaturer (SAS) är en funktion i Azure storage-konton som gör att du kan begränsa åtkomsten till data.</span><span class="sxs-lookup"><span data-stu-id="53297-107">Shared Access Signatures (SAS) are a feature of Azure storage accounts that allows you to limit access to data.</span></span> <span data-ttu-id="53297-108">Till exempel ger skrivskyddad åtkomst till data.</span><span class="sxs-lookup"><span data-stu-id="53297-108">For example, providing read-only access to data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53297-109">Överväg att använda domänanslutna HDInsight för en lösning som använder Apache Ranger.</span><span class="sxs-lookup"><span data-stu-id="53297-109">For a solution using Apache Ranger, consider using domain-joined HDInsight.</span></span> <span data-ttu-id="53297-110">Mer information finns i [konfigurera domänanslutna HDInsight](hdinsight-domain-joined-configure.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="53297-110">For more information, see the [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md) document.</span></span>

> [!WARNING]
> <span data-ttu-id="53297-111">HDInsight måste ha fullständig åtkomst till standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-111">HDInsight must have full access to the default storage for the cluster.</span></span>

## <a name="requirements"></a><span data-ttu-id="53297-112">Krav</span><span class="sxs-lookup"><span data-stu-id="53297-112">Requirements</span></span>

* <span data-ttu-id="53297-113">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="53297-113">An Azure subscription</span></span>
* <span data-ttu-id="53297-114">C# eller Python.</span><span class="sxs-lookup"><span data-stu-id="53297-114">C# or Python.</span></span> <span data-ttu-id="53297-115">C#-kodexempel anges som ett Visual Studio-lösning.</span><span class="sxs-lookup"><span data-stu-id="53297-115">C# example code is provided as a Visual Studio solution.</span></span>

  * <span data-ttu-id="53297-116">Visual Studio måste vara version 2013 eller 2015 2017</span><span class="sxs-lookup"><span data-stu-id="53297-116">Visual Studio must be version 2013, 2015, or 2017</span></span>
  * <span data-ttu-id="53297-117">Python måste vara version 2.7 eller högre</span><span class="sxs-lookup"><span data-stu-id="53297-117">Python must be version 2.7 or higher</span></span>

* <span data-ttu-id="53297-118">Ett Linux-baserade HDInsight-kluster eller [Azure PowerShell] [ powershell] -om du har ett befintligt Linux-baserade kluster kan du använda Ambari att lägga till en signatur för delad åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-118">A Linux-based HDInsight cluster OR [Azure PowerShell][powershell] - If you have an existing Linux-based cluster, you can use Ambari to add a Shared Access Signature to the cluster.</span></span> <span data-ttu-id="53297-119">Om inte, du kan använda Azure PowerShell för att skapa ett kluster och lägga till en signatur för delad åtkomst när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="53297-119">If not, you can use Azure PowerShell to create a cluster and add a Shared Access Signature during cluster creation.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="53297-120">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="53297-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="53297-121">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="53297-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="53297-122">I exempel-filer från [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="53297-122">The example files from [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).</span></span> <span data-ttu-id="53297-123">Den här lagringsplatsen innehåller följande objekt:</span><span class="sxs-lookup"><span data-stu-id="53297-123">This repository contains the following items:</span></span>

  * <span data-ttu-id="53297-124">Ett Visual Studio-projekt som kan skapa en lagringsbehållare, lagrade principer och SAS för användning med HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-124">A Visual Studio project that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="53297-125">Python-skriptet som kan skapa en lagringsbehållare, lagrade principer och SAS för användning med HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-125">A Python script that can create a storage container, stored policy, and SAS for use with HDInsight</span></span>
  * <span data-ttu-id="53297-126">Ett PowerShell-skript som kan skapa ett HDInsight-kluster och konfigurera den att använda SAS.</span><span class="sxs-lookup"><span data-stu-id="53297-126">A PowerShell script that can create a HDInsight cluster and configure it to use the SAS.</span></span>

## <a name="shared-access-signatures"></a><span data-ttu-id="53297-127">Signaturer för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="53297-127">Shared Access Signatures</span></span>

<span data-ttu-id="53297-128">Det finns två typer av signaturer för delad åtkomst:</span><span class="sxs-lookup"><span data-stu-id="53297-128">There are two forms of Shared Access Signatures:</span></span>

* <span data-ttu-id="53297-129">Ad hoc-: starttid, förfallotiden och behörigheter för SAS har angetts för SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="53297-129">Ad hoc: The start time, expiry time, and permissions for the SAS are all specified on the SAS URI.</span></span>

* <span data-ttu-id="53297-130">Lagras åtkomstprincip: en lagrade princip har definierats för en resurs-behållare, till exempel en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="53297-130">Stored access policy: A stored access policy is defined on a resource container, such as a blob container.</span></span> <span data-ttu-id="53297-131">En princip kan användas för att hantera begränsningar för en eller flera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="53297-131">A policy can be used to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="53297-132">När du kopplar en SAS med en lagrad till ärver SAS begränsningarna - starttid, förfallotiden och behörigheter - har definierats för den lagrade åtkomstprincipen.</span><span class="sxs-lookup"><span data-stu-id="53297-132">When you associate a SAS with a stored access policy, the SAS inherits the constraints - the start time, expiry time, and permissions - defined for the stored access policy.</span></span>

<span data-ttu-id="53297-133">Skillnaden mellan de två formulär är viktigt för ett key-scenario: återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="53297-133">The difference between the two forms is important for one key scenario: revocation.</span></span> <span data-ttu-id="53297-134">En SAS är en URL så att alla som erhåller SAS kan använda det, oavsett vem som har begärt det börja med.</span><span class="sxs-lookup"><span data-stu-id="53297-134">A SAS is a URL, so anyone who obtains the SAS can use it, regardless of who requested it to begin with.</span></span> <span data-ttu-id="53297-135">Om en SAS publiceras offentligt, kan den användas av vem som helst i världen.</span><span class="sxs-lookup"><span data-stu-id="53297-135">If a SAS is published publicly, it can be used by anyone in the world.</span></span> <span data-ttu-id="53297-136">En SAS som distribueras är giltig förrän något av följande händer:</span><span class="sxs-lookup"><span data-stu-id="53297-136">A SAS that is distributed is valid until one of four things happens:</span></span>

1. <span data-ttu-id="53297-137">Förfallotiden som anges på SAS har nåtts.</span><span class="sxs-lookup"><span data-stu-id="53297-137">The expiry time specified on the SAS is reached.</span></span>

2. <span data-ttu-id="53297-138">Förfallotiden som angetts på den lagrade åtkomstprincip som refereras av SAS har nåtts.</span><span class="sxs-lookup"><span data-stu-id="53297-138">The expiry time specified on the stored access policy referenced by the SAS is reached.</span></span> <span data-ttu-id="53297-139">Följande scenarier orsaka förfallotiden att nå:</span><span class="sxs-lookup"><span data-stu-id="53297-139">The following scenarios cause the expiry time to be reached:</span></span>

    * <span data-ttu-id="53297-140">Tidsintervallet har gått ut.</span><span class="sxs-lookup"><span data-stu-id="53297-140">The time interval has elapsed.</span></span>
    * <span data-ttu-id="53297-141">Den lagrade åtkomstprincipen ändras till att ha en förfallotiden tidigare.</span><span class="sxs-lookup"><span data-stu-id="53297-141">The stored access policy is modified to have an expiry time in the past.</span></span> <span data-ttu-id="53297-142">Förfallotiden är ett sätt att återkalla SAS.</span><span class="sxs-lookup"><span data-stu-id="53297-142">Changing the expiry time is one way to revoke the SAS.</span></span>

3. <span data-ttu-id="53297-143">Lagrade åtkomstprincipen som refereras av SAS tas bort, vilket är ett annat sätt att återkalla SAS.</span><span class="sxs-lookup"><span data-stu-id="53297-143">The stored access policy referenced by the SAS is deleted, which is another way to revoke the SAS.</span></span> <span data-ttu-id="53297-144">Om du återskapa den lagrade åtkomstprincipen med samma namn, gäller alla SAS-token för den föregående principen (förfallotiden på SAS inte har passerats).</span><span class="sxs-lookup"><span data-stu-id="53297-144">If you recreate the stored access policy with the same name, all  SAS tokens for the previous policy are valid (if the expiry time on the SAS has not passed).</span></span> <span data-ttu-id="53297-145">Om du vill återkalla SAS måste du använda ett annat namn om du återskapa åtkomstprincipen med ett förfallodatum i framtiden.</span><span class="sxs-lookup"><span data-stu-id="53297-145">If you intend to revoke the SAS, be sure to use a different name if you recreate the access policy with an expiry time in the future.</span></span>

4. <span data-ttu-id="53297-146">Nyckeln för kontot som används för att skapa SAS genereras.</span><span class="sxs-lookup"><span data-stu-id="53297-146">The account key that was used to create the SAS is regenerated.</span></span> <span data-ttu-id="53297-147">Återskapa nyckeln gör att alla program som använder den tidigare nyckeln Avbryt autentisering.</span><span class="sxs-lookup"><span data-stu-id="53297-147">Regenerating the key causes all applications that use the previous key to fail authentication.</span></span> <span data-ttu-id="53297-148">Uppdatera alla komponenter till den nya nyckeln.</span><span class="sxs-lookup"><span data-stu-id="53297-148">Update all components to the new key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53297-149">En signatur för delad åtkomst URI som är associerad med kontonyckel som används för att skapa signaturen och den associerade lagras åtkomstprincip (eventuella).</span><span class="sxs-lookup"><span data-stu-id="53297-149">A shared access signature URI is associated with the account key used to create the signature, and the associated stored access policy (if any).</span></span> <span data-ttu-id="53297-150">Om inga lagrade åtkomstprincip anges är det enda sättet att återkalla en signatur för delad åtkomst att ändra nyckeln för kontot.</span><span class="sxs-lookup"><span data-stu-id="53297-150">If no stored access policy is specified, the only way to revoke a shared access signature is to change the account key.</span></span>

<span data-ttu-id="53297-151">Vi rekommenderar att du alltid använder lagrade åtkomstprinciper.</span><span class="sxs-lookup"><span data-stu-id="53297-151">We recommend that you always use stored access policies.</span></span> <span data-ttu-id="53297-152">När du använder lagrade principer, kan du återkalla signaturer eller utöka utgångsdatum efter behov.</span><span class="sxs-lookup"><span data-stu-id="53297-152">When using stored policies, you can either revoke signatures or extend the expiry date as needed.</span></span> <span data-ttu-id="53297-153">Stegen i det här dokumentet används lagras åtkomstprinciper generera SAS.</span><span class="sxs-lookup"><span data-stu-id="53297-153">The steps in this document use stored access policies to generate SAS.</span></span>

<span data-ttu-id="53297-154">Mer information om signaturer för delad åtkomst finns [förstå SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="53297-154">For more information on Shared Access Signatures, see [Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

### <a name="create-a-stored-policy-and-sas-using-c"></a><span data-ttu-id="53297-155">Skapa en princip för lagrade och SAS med hjälp av C\#</span><span class="sxs-lookup"><span data-stu-id="53297-155">Create a stored policy and SAS using C\#</span></span>

1. <span data-ttu-id="53297-156">Öppna lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53297-156">Open the solution in Visual Studio.</span></span>

2. <span data-ttu-id="53297-157">I Solution Explorer högerklickar du på den **SASToken** projektet och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="53297-157">In Solution Explorer, right-click on the **SASToken** project and select **Properties**.</span></span>

3. <span data-ttu-id="53297-158">Välj **inställningar** och lägga till värden för följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="53297-158">Select **Settings** and add values for the following entries:</span></span>

   * <span data-ttu-id="53297-159">StorageConnectionString: Anslutningssträngen för lagringskontot som du vill skapa en princip för lagrade och SAS för.</span><span class="sxs-lookup"><span data-stu-id="53297-159">StorageConnectionString: The connection string for the storage account that you want to create a stored policy and SAS for.</span></span> <span data-ttu-id="53297-160">Formatet ska vara `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` där `myaccount` är namnet på ditt lagringskonto och `mykey` är nyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="53297-160">The format should be `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` where `myaccount` is the name of your storage account and `mykey` is the key for the storage account.</span></span>

   * <span data-ttu-id="53297-161">ContainerName: Behållare i storage-konto som du vill begränsa åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="53297-161">ContainerName: The container in the storage account that you want to restrict access to.</span></span>

   * <span data-ttu-id="53297-162">SASPolicyName: Namnet att använda lagrade principen för att skapa.</span><span class="sxs-lookup"><span data-stu-id="53297-162">SASPolicyName: The name to use for the stored policy to create.</span></span>

   * <span data-ttu-id="53297-163">FileToUpload: Sökvägen till en fil som överförs till behållaren.</span><span class="sxs-lookup"><span data-stu-id="53297-163">FileToUpload: The path to a file that is uploaded to the container.</span></span>

4. <span data-ttu-id="53297-164">Kör projektet.</span><span class="sxs-lookup"><span data-stu-id="53297-164">Run the project.</span></span> <span data-ttu-id="53297-165">När SAS har skapats visas information som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="53297-165">Information similar to the following text is displayed once the SAS has been generated:</span></span>

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="53297-166">Spara SAS-token för principen, lagringskontonamnet och behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="53297-166">Save the SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="53297-167">Dessa värden används när du associerar storage-konto med ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="53297-167">These values are used when associating the storage account with your HDInsight cluster.</span></span>

### <a name="create-a-stored-policy-and-sas-using-python"></a><span data-ttu-id="53297-168">Skapa en princip för lagrade och SAS använder Python</span><span class="sxs-lookup"><span data-stu-id="53297-168">Create a stored policy and SAS using Python</span></span>

1. <span data-ttu-id="53297-169">Öppna filen SASToken.py och ändra följande värden:</span><span class="sxs-lookup"><span data-stu-id="53297-169">Open the SASToken.py file and change the following values:</span></span>

   * <span data-ttu-id="53297-170">principen\_namn: namnet som ska använda för att principen för att skapa.</span><span class="sxs-lookup"><span data-stu-id="53297-170">policy\_name: The name to use for the stored policy to create.</span></span>

   * <span data-ttu-id="53297-171">lagring\_konto\_name: namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="53297-171">storage\_account\_name: The name of your storage account.</span></span>

   * <span data-ttu-id="53297-172">lagring\_konto\_nyckel: nyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="53297-172">storage\_account\_key: The key for the storage account.</span></span>

   * <span data-ttu-id="53297-173">lagring\_behållare\_name: storage-konto som du vill begränsa åtkomst till behållaren.</span><span class="sxs-lookup"><span data-stu-id="53297-173">storage\_container\_name: The container in the storage account that you want to restrict access to.</span></span>

   * <span data-ttu-id="53297-174">exempel\_filen\_sökväg: sökväg till en fil som överförs till behållaren.</span><span class="sxs-lookup"><span data-stu-id="53297-174">example\_file\_path: The path to a file that is uploaded to the container.</span></span>

2. <span data-ttu-id="53297-175">Kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="53297-175">Run the script.</span></span> <span data-ttu-id="53297-176">När skriptet har slutförts visas SAS-token som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="53297-176">It displays the SAS token similar to the following text when the script completes:</span></span>

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    <span data-ttu-id="53297-177">Spara SAS-token för principen, lagringskontonamnet och behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="53297-177">Save the SAS policy token, storage account name, and container name.</span></span> <span data-ttu-id="53297-178">Dessa värden används när du associerar storage-konto med ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="53297-178">These values are used when associating the storage account with your HDInsight cluster.</span></span>

## <a name="use-the-sas-with-hdinsight"></a><span data-ttu-id="53297-179">Använd SAS med HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-179">Use the SAS with HDInsight</span></span>

<span data-ttu-id="53297-180">När du skapar ett HDInsight-kluster, måste du ange en primär storage-konto och du kan också ange ytterligare lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="53297-180">When creating an HDInsight cluster, you must specify a primary storage account and you can optionally specify additional storage accounts.</span></span> <span data-ttu-id="53297-181">Båda dessa metoder för att lägga till lagring kräver fullständig åtkomst till storage-konton och behållare som används.</span><span class="sxs-lookup"><span data-stu-id="53297-181">Both of these methods of adding storage require full access to the storage accounts and containers that are used.</span></span>

<span data-ttu-id="53297-182">Om du vill använda en signatur för delad åtkomst för att begränsa åtkomsten till en behållare, lägger du till en anpassad post i **core-plats** konfiguration för klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-182">To use a Shared Access Signature to limit access to a container, add a custom entry to the **core-site** configuration for the cluster.</span></span>

* <span data-ttu-id="53297-183">För **Windows-baserade** eller **Linux-baserade** HDInsight-kluster, kan du lägga till posten när klustret skapas med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="53297-183">For **Windows-based** or **Linux-based** HDInsight clusters, you can add the entry during cluster creation using PowerShell.</span></span>
* <span data-ttu-id="53297-184">För **Linux-baserade** HDInsight-kluster, ändra konfigurationen efter att skapa ett kluster med Ambari.</span><span class="sxs-lookup"><span data-stu-id="53297-184">For **Linux-based** HDInsight clusters, change the configuration after cluster creation using Ambari.</span></span>

### <a name="create-a-cluster-that-uses-the-sas"></a><span data-ttu-id="53297-185">Skapa ett kluster som använder SAS</span><span class="sxs-lookup"><span data-stu-id="53297-185">Create a cluster that uses the SAS</span></span>

<span data-ttu-id="53297-186">Ett exempel på hur du skapar ett HDInsight-kluster som använder SAS ingår i den `CreateCluster` för databasen.</span><span class="sxs-lookup"><span data-stu-id="53297-186">An example of creating an HDInsight cluster that uses the SAS is included in the `CreateCluster` directory of the repository.</span></span> <span data-ttu-id="53297-187">Använd följande steg för att använda den:</span><span class="sxs-lookup"><span data-stu-id="53297-187">To use it, use the following steps:</span></span>

1. <span data-ttu-id="53297-188">Öppna den `CreateCluster\HDInsightSAS.ps1` filen i en textredigerare och ändra följande värden i början av dokumentet.</span><span class="sxs-lookup"><span data-stu-id="53297-188">Open the `CreateCluster\HDInsightSAS.ps1` file in a text editor and modify the following values at the beginning of the document.</span></span>

    ```powershell
    # Replace 'mycluster' with the name of the cluster to be created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with the name of the group to be created
    $resourceGroupName = 'myresourcegroup'
    # Replace with the Azure data center you want to the cluster to live in
    $location = 'North Europe'
    # Replace with the name of the default storage account to be created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with the name of the SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with the name of the SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with the SAS token generated earlier
    $SASToken = 'sastoken'
    # Set the number of worker nodes in the cluster
    $clusterSizeInNodes = 3
    ```

    <span data-ttu-id="53297-189">Till exempel ändra `'mycluster'` till namnet på det kluster du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="53297-189">For example, change `'mycluster'` to the name of the cluster you want to create.</span></span> <span data-ttu-id="53297-190">SAS-värden ska matcha värden från föregående steg när du skapar ett lagringskonto och SAS-token.</span><span class="sxs-lookup"><span data-stu-id="53297-190">The SAS values should match the values from the previous steps when creating a storage account and SAS token.</span></span>

    <span data-ttu-id="53297-191">När du har ändrat värdena, spara filen.</span><span class="sxs-lookup"><span data-stu-id="53297-191">Once you have changed the values, save the file.</span></span>

2. <span data-ttu-id="53297-192">Öppna en ny Azure PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="53297-192">Open a new Azure PowerShell prompt.</span></span> <span data-ttu-id="53297-193">Om du inte känner till Azure PowerShell eller inte har installerat den, se [installera och konfigurera Azure PowerShell][powershell].</span><span class="sxs-lookup"><span data-stu-id="53297-193">If you are unfamiliar with Azure PowerShell, or have not installed it, see [Install and configure Azure PowerShell][powershell].</span></span>

1. <span data-ttu-id="53297-194">Använder du följande kommando för autentisering till din Azure-prenumeration från Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="53297-194">From the prompt, use the following command to authenticate to your Azure subscription:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="53297-195">När du uppmanas logga in med kontot för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="53297-195">When prompted, log in with the account for your Azure subscription.</span></span>

    <span data-ttu-id="53297-196">Om ditt konto är kopplat till flera Azure-prenumerationer, kan du behöva använda `Select-AzureRmSubscription` till väljer du den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="53297-196">If your account is associated with multiple Azure subscriptions, you may need to use `Select-AzureRmSubscription` to select the subscription you wish to use.</span></span>

4. <span data-ttu-id="53297-197">Från Kommandotolken, ändra kataloger till den `CreateCluster` katalog som innehåller filen HDInsightSAS.ps1.</span><span class="sxs-lookup"><span data-stu-id="53297-197">From the prompt, change directories to the `CreateCluster` directory that contains the HDInsightSAS.ps1 file.</span></span> <span data-ttu-id="53297-198">Sedan använder du följande kommando för att köra skriptet</span><span class="sxs-lookup"><span data-stu-id="53297-198">Then use the following command to run the script</span></span>

    ```powershell
    .\HDInsightSAS.ps1
    ```

    <span data-ttu-id="53297-199">När skriptet körs, loggas utdata till PowerShell-Kommandotolken eftersom den skapar resursen grupp och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="53297-199">As the script runs, it logs output to the PowerShell prompt as it creates the resource group and storage accounts.</span></span> <span data-ttu-id="53297-200">Du uppmanas att ange HTTP-användare för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="53297-200">You are prompted to enter the HTTP user for the HDInsight cluster.</span></span> <span data-ttu-id="53297-201">Det här kontot används för att skydda HTTP/s-åtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-201">This account is used to secure HTTP/s access to the cluster.</span></span>

    <span data-ttu-id="53297-202">Om du skapar ett Linux-baserade kluster, efterfrågas ett SSH-konto användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="53297-202">If you are creating a Linux-based cluster, you are prompted for an SSH user account name and password.</span></span> <span data-ttu-id="53297-203">Det här kontot används för att logga in via en fjärranslutning till klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-203">This account is used to remotely log in to the cluster.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="53297-204">När du uppmanas att ange HTTP/s eller SSH-användarnamn och lösenord, måste du ange ett lösenord som uppfyller följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="53297-204">When prompted for the HTTP/s or SSH user name and password, you must provide a password that meets the following criteria:</span></span>
   >
   > * <span data-ttu-id="53297-205">Måste vara minst 10 tecken</span><span class="sxs-lookup"><span data-stu-id="53297-205">Must be at least 10 characters in length</span></span>
   > * <span data-ttu-id="53297-206">Måste innehålla minst en siffra</span><span class="sxs-lookup"><span data-stu-id="53297-206">Must contain at least one digit</span></span>
   > * <span data-ttu-id="53297-207">Måste innehålla minst ett icke-alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="53297-207">Must contain at least one non-alphanumeric character</span></span>
   > * <span data-ttu-id="53297-208">Måste innehålla minst en versal eller gemen bokstav</span><span class="sxs-lookup"><span data-stu-id="53297-208">Must contain at least one upper or lower case letter</span></span>

<span data-ttu-id="53297-209">Det tar en stund innan det här skriptet att slutföra, vanligtvis cirka 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="53297-209">It takes a while for this script to complete, usually around 15 minutes.</span></span> <span data-ttu-id="53297-210">När skriptet har slutförts utan fel, har klustret skapats.</span><span class="sxs-lookup"><span data-stu-id="53297-210">When the script completes without any errors, the cluster has been created.</span></span>

### <a name="use-the-sas-with-an-existing-cluster"></a><span data-ttu-id="53297-211">Använd SAS med ett befintligt kluster</span><span class="sxs-lookup"><span data-stu-id="53297-211">Use the SAS with an existing cluster</span></span>

<span data-ttu-id="53297-212">Om du har ett befintligt Linux-baserade kluster kan du lägga till SA till den **core-plats** konfiguration med hjälp av följande steg:</span><span class="sxs-lookup"><span data-stu-id="53297-212">If you have an existing Linux-based cluster, you can add the SAS to the **core-site** configuration by using the following steps:</span></span>

1. <span data-ttu-id="53297-213">Öppna Ambari webbgränssnittet för klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-213">Open the Ambari web UI for your cluster.</span></span> <span data-ttu-id="53297-214">Adressen för den här sidan är https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="53297-214">The address for this page is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="53297-215">När du uppmanas, autentisera för klustret med hjälp av admin-namnet (admin) och lösenord som du använde när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-215">When prompted, authenticate to the cluster using the admin name (admin) and password you used when creating the cluster.</span></span>

2. <span data-ttu-id="53297-216">Vänster sida av Ambari-webbgränssnittet, Välj **HDFS** och välj sedan den **konfigurationerna** fliken i mitten på sidan.</span><span class="sxs-lookup"><span data-stu-id="53297-216">From the left side of the Ambari web UI, select **HDFS** and then select the **Configs** tab in the middle of the page.</span></span>

3. <span data-ttu-id="53297-217">Välj den **Avancerat** , och Bläddra tills du hittar den **anpassad core-plats** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="53297-217">Select the **Advanced** tab, and then scroll until you find the **Custom core-site** section.</span></span>

4. <span data-ttu-id="53297-218">Expandera den **anpassad core-plats** avsnitt och rulla till slutet och välj sedan den **Lägg till egenskap...**  länk.</span><span class="sxs-lookup"><span data-stu-id="53297-218">Expand the **Custom core-site** section, then scroll to the end and select the **Add property...** link.</span></span> <span data-ttu-id="53297-219">Använd följande värden för den **nyckeln** och **värdet** fält:</span><span class="sxs-lookup"><span data-stu-id="53297-219">Use the following values for the **Key** and **Value** fields:</span></span>

   * <span data-ttu-id="53297-220">**Nyckeln**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span><span class="sxs-lookup"><span data-stu-id="53297-220">**Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net</span></span>
   * <span data-ttu-id="53297-221">**Värdet**: den SAS som returneras av C# eller Python-programmet som du tidigare körde</span><span class="sxs-lookup"><span data-stu-id="53297-221">**Value**: The SAS returned by the C# or Python application you ran previously</span></span>

     <span data-ttu-id="53297-222">Ersätt **CONTAINERNAME** med behållarens namn används med C# eller SAS-programmet.</span><span class="sxs-lookup"><span data-stu-id="53297-222">Replace **CONTAINERNAME** with the container name you used with the C# or SAS application.</span></span> <span data-ttu-id="53297-223">Ersätt **STORAGEACCOUNTNAME** med lagringskontots namn du använde.</span><span class="sxs-lookup"><span data-stu-id="53297-223">Replace **STORAGEACCOUNTNAME** with the storage account name you used.</span></span>

5. <span data-ttu-id="53297-224">Klicka på den **Lägg till** knappen om du vill spara den här nyckeln och värdet och klicka sedan på den **spara** för att spara ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="53297-224">Click the **Add** button to save this key and value, then click the **Save** button to save the configuration changes.</span></span> <span data-ttu-id="53297-225">När du uppmanas, lägga till en beskrivning av ändringen (”lägger till SAS-lagringsåtkomst” till exempel) och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="53297-225">When prompted, add a description of the change ("adding SAS storage access" for example) and then click **Save**.</span></span>

    <span data-ttu-id="53297-226">Klicka på **OK** när ändringarna har utförts.</span><span class="sxs-lookup"><span data-stu-id="53297-226">Click **OK** when the changes have been completed.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="53297-227">Du måste starta om flera tjänster innan ändringen börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="53297-227">You must restart several services before the change takes effect.</span></span>

6. <span data-ttu-id="53297-228">Markera i Ambari webbgränssnittet **HDFS** i listan till vänster och välj sedan **starta om alla** från den **tjänståtgärder** listrutan till höger.</span><span class="sxs-lookup"><span data-stu-id="53297-228">In the Ambari web UI, select **HDFS** from the list on the left, and then select **Restart All** from the **Service Actions** drop down list on the right.</span></span> <span data-ttu-id="53297-229">När du uppmanas, Välj **aktivera underhållsläge** och sedan väljer __Conform starta om alla ”.</span><span class="sxs-lookup"><span data-stu-id="53297-229">When prompted, select **Turn on maintenance mode** and then select __Conform Restart All".</span></span>

    <span data-ttu-id="53297-230">Upprepa processen för MapReduce2 och YARN.</span><span class="sxs-lookup"><span data-stu-id="53297-230">Repeat this process for MapReduce2 and YARN.</span></span>

7. <span data-ttu-id="53297-231">När tjänsten har startats om, välja respektive och inaktivera underhållsläge från den **tjänståtgärder** listrutan.</span><span class="sxs-lookup"><span data-stu-id="53297-231">Once the services have restarted, select each one and disable maintenance mode from the **Service Actions** drop down.</span></span>

## <a name="test-restricted-access"></a><span data-ttu-id="53297-232">Testa begränsad åtkomst</span><span class="sxs-lookup"><span data-stu-id="53297-232">Test restricted access</span></span>

<span data-ttu-id="53297-233">Kontrollera att du har begränsad åtkomst genom att använda följande metoder:</span><span class="sxs-lookup"><span data-stu-id="53297-233">To verify that you have restricted access, use the following methods:</span></span>

* <span data-ttu-id="53297-234">För **Windows-baserade** HDInsight-kluster, använda Fjärrskrivbord för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-234">For **Windows-based** HDInsight clusters, use Remote Desktop to connect to the cluster.</span></span> <span data-ttu-id="53297-235">Mer information finns i [Anslut till HDInsight med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="53297-235">For more information, see [Connect to HDInsight using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>

    <span data-ttu-id="53297-236">När du är ansluten, Använd den **Hadoop kommandoradsverktyget** ikon på skrivbordet för att öppna en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="53297-236">Once connected, use the **Hadoop Command-Line** icon on the desktop to open a command prompt.</span></span>

* <span data-ttu-id="53297-237">För **Linux-baserade** HDInsight-kluster, använda SSH för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="53297-237">For **Linux-based** HDInsight clusters, use SSH to connect to the cluster.</span></span> <span data-ttu-id="53297-238">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="53297-238">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="53297-239">När du är ansluten till klustret, Använd följande steg för att verifiera att du kan endast Läs- och objekten på lagringskontot SAS:</span><span class="sxs-lookup"><span data-stu-id="53297-239">Once connected to the cluster, use the following steps to verify that you can only read and list items on the SAS storage account:</span></span>

1. <span data-ttu-id="53297-240">Om du vill visa innehållet i behållaren, använder du följande kommando i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="53297-240">To list the contents of the container, use the following command from the prompt:</span></span> 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    <span data-ttu-id="53297-241">Ersätt **SASCONTAINER** med namnet på den behållare som skapats för SAS-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="53297-241">Replace **SASCONTAINER** with the name of the container created for the SAS storage account.</span></span> <span data-ttu-id="53297-242">Ersätt **SASACCOUNTNAME** med namnet på lagringskontot används för SAS.</span><span class="sxs-lookup"><span data-stu-id="53297-242">Replace **SASACCOUNTNAME** with the name of the storage account used for the SAS.</span></span>

    <span data-ttu-id="53297-243">Listan innehåller filen när behållare och SAS skapades.</span><span class="sxs-lookup"><span data-stu-id="53297-243">The list includes the file uploaded when the container and SAS were created.</span></span>

2. <span data-ttu-id="53297-244">Använd följande kommando för att kontrollera att du kan läsa innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="53297-244">Use the following command to verify that you can read the contents of the file.</span></span> <span data-ttu-id="53297-245">Ersätt den **SASCONTAINER** och **SASACCOUNTNAME** som i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="53297-245">Replace the **SASCONTAINER** and **SASACCOUNTNAME** as in the previous step.</span></span> <span data-ttu-id="53297-246">Ersätt **FILENAME** med namnet på den fil som visas i föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="53297-246">Replace **FILENAME** with the name of the file displayed in the previous command:</span></span>

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    <span data-ttu-id="53297-247">Det här kommandot visar innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="53297-247">This command lists the contents of the file.</span></span>

3. <span data-ttu-id="53297-248">Använd följande kommando för att hämta filen till det lokala filsystemet:</span><span class="sxs-lookup"><span data-stu-id="53297-248">Use the following command to download the file to the local file system:</span></span>

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    <span data-ttu-id="53297-249">Det här kommandot laddar ned filen till en lokal fil med namnet **testfile.txt**.</span><span class="sxs-lookup"><span data-stu-id="53297-249">This command downloads the file to a local file named **testfile.txt**.</span></span>

4. <span data-ttu-id="53297-250">Använd följande kommando för att överföra den lokala filen till en ny fil med namnet **testupload.txt** på SAS-lagring:</span><span class="sxs-lookup"><span data-stu-id="53297-250">Use the following command to upload the local file to a new file named **testupload.txt** on the SAS storage:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    <span data-ttu-id="53297-251">Du får ett meddelande som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="53297-251">You receive a message similar to the following text:</span></span>

        put: java.io.IOException

    <span data-ttu-id="53297-252">Detta fel uppstår eftersom lagringsplatsen är skrivskyddade + endast lista över tillåtna.</span><span class="sxs-lookup"><span data-stu-id="53297-252">This error occurs because the storage location is read+list only.</span></span> <span data-ttu-id="53297-253">Använd följande kommando för att placera data på lagringsutrymme som standard för klustret, som är skrivbara:</span><span class="sxs-lookup"><span data-stu-id="53297-253">Use the following command to put the data on the default storage for the cluster, which is writable:</span></span>

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    <span data-ttu-id="53297-254">Den här gången ska åtgärden slutföras.</span><span class="sxs-lookup"><span data-stu-id="53297-254">This time, the operation should complete successfully.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="53297-255">Felsökning</span><span class="sxs-lookup"><span data-stu-id="53297-255">Troubleshooting</span></span>

### <a name="a-task-was-canceled"></a><span data-ttu-id="53297-256">En uppgift har avbrutits</span><span class="sxs-lookup"><span data-stu-id="53297-256">A task was canceled</span></span>

<span data-ttu-id="53297-257">**Symptom**: när du skapar ett kluster med PowerShell-skript får följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="53297-257">**Symptoms**: When creating a cluster using the PowerShell script, you may receive the following error message:</span></span>

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

<span data-ttu-id="53297-258">**Orsak**: det här felet kan inträffa om du använder ett lösenord för admin/http-användaren för klustret, eller (för Linux-baserade kluster) för SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="53297-258">**Cause**: This error can occur if you use a password for the admin/HTTP user for the cluster, or (for Linux-based clusters) the SSH user.</span></span>

<span data-ttu-id="53297-259">**Lösning**: Använd ett lösenord som uppfyller följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="53297-259">**Resolution**: Use a password that meets the following criteria:</span></span>

* <span data-ttu-id="53297-260">Måste vara minst 10 tecken</span><span class="sxs-lookup"><span data-stu-id="53297-260">Must be at least 10 characters in length</span></span>
* <span data-ttu-id="53297-261">Måste innehålla minst en siffra</span><span class="sxs-lookup"><span data-stu-id="53297-261">Must contain at least one digit</span></span>
* <span data-ttu-id="53297-262">Måste innehålla minst ett icke-alfanumeriska tecken</span><span class="sxs-lookup"><span data-stu-id="53297-262">Must contain at least one non-alphanumeric character</span></span>
* <span data-ttu-id="53297-263">Måste innehålla minst en versal eller gemen bokstav</span><span class="sxs-lookup"><span data-stu-id="53297-263">Must contain at least one upper or lower case letter</span></span>

## <a name="next-steps"></a><span data-ttu-id="53297-264">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53297-264">Next steps</span></span>

<span data-ttu-id="53297-265">Nu när du har lärt dig hur du lägger till lagring med begränsad åtkomst till ditt HDInsight-kluster, lär du dig andra sätt att arbeta med data på ditt kluster:</span><span class="sxs-lookup"><span data-stu-id="53297-265">Now that you have learned how to add limited-access storage to your HDInsight cluster, learn other ways to work with data on your cluster:</span></span>

* [<span data-ttu-id="53297-266">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-266">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="53297-267">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-267">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="53297-268">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="53297-268">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs
