---
title: "Hantera Windows-baserade Hadoop-kluster i HDInsight med hjälp av Azure portal | Microsoft Docs"
description: "Lär dig hur du administrerar HDInsight Service. Skapa ett HDInsight-kluster, öppna interaktiva JavaScript-konsolen och öppna konsolen Hadoop-kommando."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: f69fa4f838b22ccbb25186c08cac9744bb31c6d1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a><span data-ttu-id="3a9a7-104">Hantera Windows-baserade Hadoop-kluster i HDInsight med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="3a9a7-104">Manage Windows-based Hadoop clusters in HDInsight by using the Azure portal</span></span>

<span data-ttu-id="3a9a7-105">Med hjälp av den [Azure-portalen][azure-portal], kan du skapa Windows-baserade Hadoop-kluster i Azure HDInsight, ändra Hadoop användarlösenord och aktivera Remote Desktop Protocol (RDP) så att du kan använda kommandot Hadoop -konsolen på klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-105">Using the [Azure portal][azure-portal], you can create Windows-based Hadoop clusters in Azure HDInsight, change Hadoop user password, and enable Remote Desktop Protocol (RDP) so you can access the Hadoop command console on the cluster.</span></span>

<span data-ttu-id="3a9a7-106">Informationen i den här artikeln gäller bara för Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-106">The information in this article only applies to Window-based HDInsight clusters.</span></span> <span data-ttu-id="3a9a7-107">Information om hur du hanterar Linux-baserade kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av Azure portal](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-107">For information on managing Linux-based clusters, see [Manage Hadoop clusters in HDInsight by using the Azure portal](hdinsight-administer-use-portal-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a9a7-108">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3a9a7-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="3a9a7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3a9a7-110">Prerequisites</span></span>

<span data-ttu-id="3a9a7-111">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-111">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="3a9a7-112">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-112">**An Azure subscription**.</span></span> <span data-ttu-id="3a9a7-113">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="3a9a7-114">**Azure Storage-konto** -ett HDInsight-kluster använder en Azure Blob storage-behållare som filsystemet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-114">**Azure Storage account** - An HDInsight cluster uses an Azure Blob storage container as the default file system.</span></span> <span data-ttu-id="3a9a7-115">Mer information om hur Azure Blob storage ger en sömlös upplevelse med HDInsight-kluster finns [använda Azure Blob Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-115">For more information about how Azure Blob storage provides a seamless experience with HDInsight clusters, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="3a9a7-116">Mer information om hur du skapar ett Azure Storage-konto finns [hur du skapar ett Lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-116">For details on creating an Azure Storage account, see [How to Create a Storage Account](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="open-the-portal"></a><span data-ttu-id="3a9a7-117">Öppna portalen</span><span class="sxs-lookup"><span data-stu-id="3a9a7-117">Open the Portal</span></span>
1. <span data-ttu-id="3a9a7-118">Logga in på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-118">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3a9a7-119">När du öppnar portalen kan du:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-119">After you open the portal, you can:</span></span>

   * <span data-ttu-id="3a9a7-120">Klicka på **ny** i den vänstra menyn för att skapa ett nytt kluster:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-120">Click **New** from the left menu to create a new cluster:</span></span>

       ![ny knapp för HDInsight-kluster](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * <span data-ttu-id="3a9a7-122">Klicka på **HDInsight-kluster** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-122">Click **HDInsight Clusters** from the left menu.</span></span>

       ![Azure portal HDInsight-kluster-knappen](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     <span data-ttu-id="3a9a7-124">Om **HDInsight** inte visas i den vänstra menyn klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-124">If **HDInsight** doesn't appear in the left menu, click **Browse**.</span></span>

     ![Azure portal knappen Bläddra klustret.](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a><span data-ttu-id="3a9a7-126">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="3a9a7-126">Create clusters</span></span>
<span data-ttu-id="3a9a7-127">Skapa-instruktioner med hjälp av portalen finns i [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-127">For the creation instructions using the Portal, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="3a9a7-128">HDInsight fungerar med en bred Hadoop-komponenter.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-128">HDInsight works with a wide range of Hadoop components.</span></span> <span data-ttu-id="3a9a7-129">Lista över de komponenter som har verifierats, och som stöds finns i [vilken version av Hadoop finns i Azure HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-129">For the list of the components that have been verified and supported, see [What version of Hadoop is in Azure HDInsight](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="3a9a7-130">Du kan anpassa HDInsight med hjälp av något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-130">You can customize HDInsight by using one of the following options:</span></span>

* <span data-ttu-id="3a9a7-131">Använd skriptåtgärder om du vill köra anpassade skript som kan anpassa ett kluster om du vill ändra klusterkonfigurationen eller installera anpassade komponenter, till exempel Giraph eller Solr.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-131">Use Script Action to run custom scripts that can customize a cluster to either change cluster configuration or install custom components such as Giraph or Solr.</span></span> <span data-ttu-id="3a9a7-132">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-132">For more information, see [Customize HDInsight cluster using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span>
* <span data-ttu-id="3a9a7-133">Använd anpassning Klusterparametrar i Azure PowerShell eller HDInsight .NET SDK när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-133">Use the cluster customization parameters in the HDInsight .NET SDK or Azure PowerShell during cluster creation.</span></span> <span data-ttu-id="3a9a7-134">Ändringarna sparas sedan via livslängden för klustret och påverkas inte av klustret nod reimages som Azure-plattformen utför med jämna mellanrum för underhåll.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-134">These configuration changes are then preserved through the lifetime of the cluster and are not affected by cluster node reimages that Azure platform periodically performs for maintenance.</span></span> <span data-ttu-id="3a9a7-135">Mer information om hur du använder anpassning Klusterparametrar finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-135">For more information on using the cluster customization parameters, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="3a9a7-136">Vissa inbyggda Java-komponenter som Mahout och kaskad, kan köras i klustret som JAR-filer.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-136">Some native Java components, like Mahout and Cascading, can be run on the cluster as JAR files.</span></span> <span data-ttu-id="3a9a7-137">Dessa JAR-filer kan distribueras till Azure Blob storage och skickas till HDInsight-kluster med Hadoop-jobbet skicka mekanismer.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-137">These JAR files can be distributed to Azure Blob storage, and submitted to HDInsight clusters through Hadoop job submission mechanisms.</span></span> <span data-ttu-id="3a9a7-138">Mer information finns i [skicka Hadoop-jobb via programmering](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-138">For more information, see [Submit Hadoop jobs programmatically](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

  > [!NOTE]
  > <span data-ttu-id="3a9a7-139">Om du har problem med distribution av JAR-filer till HDInsight-kluster eller anropa JAR-filer på HDInsight-kluster, kontakta [Microsoft-supporten](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-139">If you have issues deploying JAR files to HDInsight clusters or calling JAR files on HDInsight clusters, contact [Microsoft Support](https://azure.microsoft.com/support/options/).</span></span>
  >
  > <span data-ttu-id="3a9a7-140">Sammanhängande stöds inte av HDInsight och är inte tillgänglig för Microsofts Support.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-140">Cascading is not supported by HDInsight, and is not eligible for Microsoft Support.</span></span> <span data-ttu-id="3a9a7-141">Listor med stödda komponenter finns [vad är nytt i klusterversioner som tillhandahålls av HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-141">For lists of supported components, see [What's new in the cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
  >
  >

<span data-ttu-id="3a9a7-142">Installation av anpassade program i klustret med hjälp av anslutning till fjärrskrivbord stöds inte.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-142">Installation of custom software on the cluster by using Remote Desktop Connection is not supported.</span></span> <span data-ttu-id="3a9a7-143">Du bör inte lagra filer för enheter på huvudnoden, eftersom de kommer att förloras om du behöver skapa nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-143">You should avoid storing any files on the drives of the head node, as they will be lost if you need to re-create the clusters.</span></span> <span data-ttu-id="3a9a7-144">Vi rekommenderar att du lagrar filer på Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-144">We recommend storing files on Azure Blob storage.</span></span> <span data-ttu-id="3a9a7-145">BLOB storage är permanent.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-145">Blob storage is persistent.</span></span>

## <a name="list-and-show-clusters"></a><span data-ttu-id="3a9a7-146">Listan och visa kluster</span><span class="sxs-lookup"><span data-stu-id="3a9a7-146">List and show clusters</span></span>
1. <span data-ttu-id="3a9a7-147">Logga in på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-147">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3a9a7-148">Klicka på **HDInsight-kluster** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-148">Click **HDInsight Clusters** from the left menu.</span></span>
3. <span data-ttu-id="3a9a7-149">Klicka på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-149">Click the cluster name.</span></span> <span data-ttu-id="3a9a7-150">Om klustret är långt, kan du använda filter överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-150">If the cluster list is long, you can use filter on the top of the page.</span></span>
4. <span data-ttu-id="3a9a7-151">Dubbelklicka på ett kluster från listan om du vill visa detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-151">Double-click a cluster from the list to show the details.</span></span>

    <span data-ttu-id="3a9a7-152">**Menyn och essentials**:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-152">**Menu and essentials**:</span></span>

    ![Azure portal HDInsight-kluster essentials](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * <span data-ttu-id="3a9a7-154">Om du vill anpassa menyn högerklickar du någonstans på menyn och klicka sedan på **anpassa**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-154">To customize the menu, right-click anywhere on the menu, and then click **Customize**.</span></span>
   * <span data-ttu-id="3a9a7-155">**Inställningar för** och **alla inställningar**: Visar den **inställningar** bladet för klustret, där du kan komma åt detaljerad konfigurationsinformation för klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-155">**Settings** and **All Settings**: Displays the **Settings** blade for the cluster, which allows you to access detailed configuration information for the cluster.</span></span>
   * <span data-ttu-id="3a9a7-156">**Instrumentpanelen**, **Klusterinstrumentpanel** och **URL: dessa är alla sätt att komma åt instrumentpanelen kluster, vilket är Ambari Web för Linux-baserade kluster. -**Secure Shell **: Visar instruktionerna för att ansluta till klustret med SSH (Secure Shell)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-156">**Dashboard**, **Cluster Dashboard** and **URL: These are all ways to access the cluster dashboard, which is Ambari Web for Linux-based clusters. -**Secure Shell**: Shows the instructions to connect to the cluster using Secure Shell (SSH) connection.</span></span>
   * <span data-ttu-id="3a9a7-157">**Skala klustret**: du kan ändra antalet arbetarnoder för det här klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-157">**Scale Cluster**: Allows you to change the number of worker nodes for this cluster.</span></span>
   * <span data-ttu-id="3a9a7-158">**Ta bort**: tar bort klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-158">**Delete**: Deletes the cluster.</span></span>
   * <span data-ttu-id="3a9a7-159">**Snabbstart**: Visar information som hjälper dig att komma igång med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-159">**Quickstart**: Displays information that will help you get started using HDInsight.</span></span>
   * <span data-ttu-id="3a9a7-160">** Användare: Om du vill ange behörigheter för *portal management* för detta kluster för andra användare på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-160">**Users: Allows you to set permissions for *portal management* of this cluster for other users on your Azure subscription.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="3a9a7-161">Detta *endast* påverkar åtkomst och behörighet till detta kluster i Azure-portalen och har ingen effekt på vem som kan ansluta till eller skicka jobb till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-161">This *only* affects access and permissions to this cluster in the Azure portal, and has no effect on who can connect to or submit jobs to the HDInsight cluster.</span></span>
     >
     >
   * <span data-ttu-id="3a9a7-162">**Taggar**: taggar kan du ange nyckel/värde-par för att definiera en anpassad taxonomi av dina molntjänster.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-162">**Tags**: Tags allow you to set key/value pairs to define a custom taxonomy of your cloud services.</span></span> <span data-ttu-id="3a9a7-163">Du kan till exempel skapa en nyckel som heter **projekt**, och sedan använda ett värde som är gemensamma för alla tjänster som är associerad med ett visst projekt.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-163">For example, you may create a key named **project**, and then use a common value for all services associated with a specific project.</span></span>
   * <span data-ttu-id="3a9a7-164">**Ambari Views**: länkar till Ambari Web.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-164">**Ambari Views**: Links to Ambari Web.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="3a9a7-165">För att hantera tjänster som tillhandahålls av HDInsight-kluster, måste du använda Ambari Web eller Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-165">To manage the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="3a9a7-166">Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-166">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
     >
     >

     <span data-ttu-id="3a9a7-167">**Användning**:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-167">**Usage**:</span></span>

     ![Azure portal HDInsight-kluster användning](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. <span data-ttu-id="3a9a7-169">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-169">Click **Settings**.</span></span>

    ![Azure portal HDInsight-kluster användning](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * <span data-ttu-id="3a9a7-171">**Egenskaper för**: Visa egenskaper för klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-171">**Properties**: View the cluster properties.</span></span>
   * <span data-ttu-id="3a9a7-172">**Kluster-AAD-identitet**:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-172">**Cluster AAD Identity**:</span></span>
   * <span data-ttu-id="3a9a7-173">**Azure Storage-nycklar**: Visa standardkontot för lagring och dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-173">**Azure Storage Keys**: View the default storage account and its key.</span></span> <span data-ttu-id="3a9a7-174">Lagringskontot är konfiguration när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-174">The storage account is configuration during the cluster creation process.</span></span>
   * <span data-ttu-id="3a9a7-175">**Klustret inloggningen**: ändra klustret HTTP-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-175">**Cluster Login**: Change the cluster HTTP user name and password.</span></span>
   * <span data-ttu-id="3a9a7-176">**Externa Metastores**: Visa Hive och Oozie metastores.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-176">**External Metastores**: View the Hive and Oozie metastores.</span></span> <span data-ttu-id="3a9a7-177">Metastores kan bara konfigureras när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-177">The metastores can only be configured during the cluster creation process.</span></span>
   * <span data-ttu-id="3a9a7-178">**Skala klustret**: öka och minska antalet arbetarnoder i klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-178">**Scale Cluster**: Increase and decrease the number of cluster worker nodes.</span></span>
   * <span data-ttu-id="3a9a7-179">**Fjärrskrivbord**: aktivera och inaktivera åtkomst via fjärrskrivbord (RDP) och konfigurera RDP-användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-179">**Remote Desktop**: Enable and disable remote desktop (RDP) access, and configure the RDP username.</span></span>  <span data-ttu-id="3a9a7-180">RDP-användarnamnet måste vara samma som HTTP-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-180">The RDP user name must be different from the HTTP user name.</span></span>
   * <span data-ttu-id="3a9a7-181">**Auktoriserad partner**:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-181">**Partner of Record**:</span></span>

     > [!NOTE]
     > <span data-ttu-id="3a9a7-182">Det här är en generisk lista över tillgängliga inställningar. inte alla finnas för alla typer av klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-182">This is a generic list of available settings; not all of them will be present for all cluster types.</span></span>
     >
     >
6. <span data-ttu-id="3a9a7-183">Klicka på **egenskaper**:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-183">Click **Properties**:</span></span>

    <span data-ttu-id="3a9a7-184">Egenskapsavsnittet innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-184">The properties section lists the following:</span></span>

   * <span data-ttu-id="3a9a7-185">**Hostname**: klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-185">**Hostname**: Cluster name.</span></span>
   * <span data-ttu-id="3a9a7-186">**Kluster-URL: en**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-186">**Cluster URL**.</span></span>
   * <span data-ttu-id="3a9a7-187">**Status för**: inkludera avbröts, godkända ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, fungerar, kör, fel, ta bort, tas bort, för lång tid, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued ResizeQueued, ClusterCustomization</span><span class="sxs-lookup"><span data-stu-id="3a9a7-187">**Status**: Include Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization</span></span>
   * <span data-ttu-id="3a9a7-188">**Region**: Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-188">**Region**: Azure location.</span></span> <span data-ttu-id="3a9a7-189">En lista över Azure platser som stöds finns i **Region** nedrullningsbar listruta i [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-189">For a list of supported Azure locations, see the **Region** dropdown list box on [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>
   * <span data-ttu-id="3a9a7-190">**Data som har skapats**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-190">**Data created**.</span></span>
   * <span data-ttu-id="3a9a7-191">**Operativsystemet**: antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-191">**Operating system**: Either **Windows** or **Linux**.</span></span>
   * <span data-ttu-id="3a9a7-192">**Typen**: Hadoop, HBase, Storm, Väck.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-192">**Type**: Hadoop, HBase, Storm, Spark.</span></span>
   * <span data-ttu-id="3a9a7-193">**Version**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-193">**Version**.</span></span> <span data-ttu-id="3a9a7-194">Se [HDInsight-versioner](hdinsight-component-versioning.md)</span><span class="sxs-lookup"><span data-stu-id="3a9a7-194">See [HDInsight versions](hdinsight-component-versioning.md)</span></span>
   * <span data-ttu-id="3a9a7-195">**Prenumerationen**: prenumerationsnamn.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-195">**Subscription**: Subscription name.</span></span>
   * <span data-ttu-id="3a9a7-196">**Prenumerations-ID**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-196">**Subscription ID**.</span></span>
   * <span data-ttu-id="3a9a7-197">**Primär datakälla**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-197">**Primary data source**.</span></span> <span data-ttu-id="3a9a7-198">Azure Blob storage-konto som används som standard Hadoop-filsystem.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-198">The Azure Blob storage account used as the default Hadoop file system.</span></span>
   * <span data-ttu-id="3a9a7-199">**Arbetsnoderna prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-199">**Worker nodes pricing tier**.</span></span>
   * <span data-ttu-id="3a9a7-200">**Huvudnod prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-200">**Head node pricing tier**.</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="3a9a7-201">Ta bort kluster</span><span class="sxs-lookup"><span data-stu-id="3a9a7-201">Delete clusters</span></span>
<span data-ttu-id="3a9a7-202">Ett kluster tas inte bort standardkontot för lagring eller eventuella länkade lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-202">Delete a cluster will not delete the default storage account or any linked storage accounts.</span></span> <span data-ttu-id="3a9a7-203">Du kan återskapa klustret med hjälp av samma lagringskonton och samma metastores.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-203">You can re-create the cluster by using the same storage accounts and the same metastores.</span></span>

1. <span data-ttu-id="3a9a7-204">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="3a9a7-204">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="3a9a7-205">Klicka på **Bläddra bland alla** från den vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-205">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="3a9a7-206">Klicka på **ta bort** på den översta menyn och följ sedan instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-206">Click **Delete** from the top menu, and then follow the instructions.</span></span>

<span data-ttu-id="3a9a7-207">Se även [paus/stängs av kluster](#pauseshut-down-clusters).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-207">See also [Pause/shut down clusters](#pauseshut-down-clusters).</span></span>

## <a name="scale-clusters"></a><span data-ttu-id="3a9a7-208">Skala kluster</span><span class="sxs-lookup"><span data-stu-id="3a9a7-208">Scale clusters</span></span>
<span data-ttu-id="3a9a7-209">Klustret skalning funktionen kan du ändra antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan att behöva återskapa klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-209">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="3a9a7-210">Endast kluster med HDInsight version 3.1.3 eller högre stöds.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-210">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="3a9a7-211">Om du är osäker på vilken version av klustret kan kontrollera du egenskapssidan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-211">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="3a9a7-212">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-212">See [List and show clusters](#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="3a9a7-213">Konsekvenser av att ändra antalet datanoder som för varje typ av kluster som stöds av HDInsight:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-213">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="3a9a7-214">Hadoop</span><span class="sxs-lookup"><span data-stu-id="3a9a7-214">Hadoop</span></span>

    <span data-ttu-id="3a9a7-215">Sömlöst kan du öka antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-215">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="3a9a7-216">Också du kan skicka nya jobb medan åtgärden pågår.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-216">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="3a9a7-217">Fel i en åtgärd för skalning hanteras korrekt så att klustret alltid kvar i ett fungerande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-217">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>

    <span data-ttu-id="3a9a7-218">När ett Hadoop-kluster skalas ned genom att minska antalet datanoder som, en del av tjänsterna i klustret har startats om.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-218">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="3a9a7-219">Detta medför att alla körs och väntande jobb misslyckas vid skalning åtgärden slutfördes.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-219">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="3a9a7-220">Du kan dock skicka jobb när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-220">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="3a9a7-221">HBase</span><span class="sxs-lookup"><span data-stu-id="3a9a7-221">HBase</span></span>

    <span data-ttu-id="3a9a7-222">Du kan sömlöst Lägg till eller ta bort noder till HBase-kluster, medan den körs.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-222">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="3a9a7-223">Regional servrar balanseras automatiskt inom några minuter för att slutföra åtgärden. skalning.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-223">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="3a9a7-224">Du kan också manuellt balansera regionala servrar genom att logga in på headnode i klustret och köra följande kommandon från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-224">However, you can also manually balance the regional servers by logging into the headnode of cluster and running the following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    <span data-ttu-id="3a9a7-225">Mer information om hur du använder HBase-gränssnittet finns i]</span><span class="sxs-lookup"><span data-stu-id="3a9a7-225">For more information on using the HBase shell, see []</span></span>
* <span data-ttu-id="3a9a7-226">Storm</span><span class="sxs-lookup"><span data-stu-id="3a9a7-226">Storm</span></span>

    <span data-ttu-id="3a9a7-227">Du kan sömlöst lägga till eller ta bort datanoder till ditt Storm-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-227">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="3a9a7-228">Men efter en lyckad skalning åtgärden har slutförts, behöver du ombalansera topologin.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-228">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>

    <span data-ttu-id="3a9a7-229">Ombalansering kan utföras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-229">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="3a9a7-230">Storm webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="3a9a7-230">Storm web UI</span></span>
  * <span data-ttu-id="3a9a7-231">Verktyget kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="3a9a7-231">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="3a9a7-232">Mer information finns i [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-232">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="3a9a7-233">Storm webbgränssnittet är tillgänglig på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-233">The Storm web UI is available on the HDInsight cluster:</span></span>

    ![HDInsight storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    <span data-ttu-id="3a9a7-235">Här är ett exempel så använder du kommandot CLI för att balansera Storm-topologi:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-235">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="3a9a7-236">**Att skala kluster**</span><span class="sxs-lookup"><span data-stu-id="3a9a7-236">**To scale clusters**</span></span>

1. <span data-ttu-id="3a9a7-237">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="3a9a7-237">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="3a9a7-238">Klicka på **Bläddra bland alla** från den vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-238">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="3a9a7-239">Klicka på **inställningar** från den översta menyn och klicka sedan på **kluster**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-239">Click **Settings** from the top menu, and then click **Scale Cluster**.</span></span>
4. <span data-ttu-id="3a9a7-240">Ange **numret för arbetarnoder**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-240">Enter **Number of Worker nodes**.</span></span> <span data-ttu-id="3a9a7-241">Gränsen för antalet klusternod varierar mellan olika Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-241">The limit on the number of cluster node varies among Azure subscriptions.</span></span> <span data-ttu-id="3a9a7-242">Du kan kontakta faktureringssupporten om du vill öka gränsvärdet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-242">You can contact billing support to increase the limit.</span></span>  <span data-ttu-id="3a9a7-243">Information om kostnaden återspeglas ändringar i antalet noder.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-243">The cost information will reflect the changes you have made to the number of nodes.</span></span>

    ![HDInsight Hadoop HBase, Storm Spark skala](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a><span data-ttu-id="3a9a7-245">Pausa/stängs av kluster</span><span class="sxs-lookup"><span data-stu-id="3a9a7-245">Pause/shut down clusters</span></span>
<span data-ttu-id="3a9a7-246">De flesta Hadoop-jobb är batchjobb som bara ibland kördes.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-246">Most of Hadoop jobs are batch jobs that are only ran occasionally.</span></span> <span data-ttu-id="3a9a7-247">För de flesta Hadoop-kluster finns stora tidsperioder som klustret inte används för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-247">For most Hadoop clusters, there are large periods of time that the cluster is not being used for processing.</span></span> <span data-ttu-id="3a9a7-248">Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-248">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span>
<span data-ttu-id="3a9a7-249">Du debiteras också för ett HDInsight-kluster, även när det inte används.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-249">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="3a9a7-250">Eftersom avgifterna för klustret är flera gånger större än avgifterna för lagring är det ekonomiskt sett bra att ta bort kluster när de inte används.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-250">Since the charges for the cluster are many times more than the charges for storage, it makes economic sense to delete clusters when they are not in use.</span></span>

<span data-ttu-id="3a9a7-251">Det finns många sätt kan du programmerar processen:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-251">There are many ways you can program the process:</span></span>

* <span data-ttu-id="3a9a7-252">Användaren Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-252">User Azure Data Factory.</span></span> <span data-ttu-id="3a9a7-253">Se [länkad Azure HDInsight-tjänst](../data-factory/data-factory-compute-linked-services.md) och [transformera och analysera med hjälp av Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) för HDInsight på begäran och automatisk definierad länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-253">See [Azure HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md) and [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) for on-demand and self-defined HDInsight linked services.</span></span>
* <span data-ttu-id="3a9a7-254">Använda Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-254">Use Azure PowerShell.</span></span>  <span data-ttu-id="3a9a7-255">Se [analysera svarta fördröjning](hdinsight-analyze-flight-delay-data.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-255">See [Analyze flight delay data](hdinsight-analyze-flight-delay-data.md).</span></span>
* <span data-ttu-id="3a9a7-256">Använda Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-256">Use Azure CLI.</span></span> <span data-ttu-id="3a9a7-257">Se [hantera HDInsight-kluster med hjälp av Azure CLI](hdinsight-administer-use-command-line.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-257">See [Manage HDInsight clusters using Azure CLI](hdinsight-administer-use-command-line.md).</span></span>
* <span data-ttu-id="3a9a7-258">Använda HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-258">Use HDInsight .NET SDK.</span></span> <span data-ttu-id="3a9a7-259">Se [skicka Hadoop-jobb](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-259">See [Submit Hadoop jobs](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

<span data-ttu-id="3a9a7-260">Prisinformation, se [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-260">For the pricing information, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span> <span data-ttu-id="3a9a7-261">Om du vill ta bort ett kluster från portalen finns [ta bort kluster](#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="3a9a7-261">To delete a cluster from the Portal, see [Delete clusters](#delete-clusters)</span></span>

## <a name="change-cluster-username"></a><span data-ttu-id="3a9a7-262">Ändra kluster användarnamn</span><span class="sxs-lookup"><span data-stu-id="3a9a7-262">Change cluster username</span></span>
<span data-ttu-id="3a9a7-263">Ett HDInsight-kluster kan ha två användarkonton.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-263">An HDInsight cluster can have two user accounts.</span></span> <span data-ttu-id="3a9a7-264">Användarkonto för HDInsight-kluster skapas under skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-264">The HDInsight cluster user account is created during the creation process.</span></span> <span data-ttu-id="3a9a7-265">Du kan också skapa ett användarkonto för RDP för åtkomst till klustret via RDP.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-265">You can also create an RDP user account for accessing the cluster via RDP.</span></span> <span data-ttu-id="3a9a7-266">Se [aktivera Fjärrskrivbord](#connect-to-hdinsight-clusters-by-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-266">See [Enable remote desktop](#connect-to-hdinsight-clusters-by-using-rdp).</span></span>

<span data-ttu-id="3a9a7-267">**Ändra HDInsight-kluster-användarnamn och lösenord**</span><span class="sxs-lookup"><span data-stu-id="3a9a7-267">**To change the HDInsight cluster user name and password**</span></span>

1. <span data-ttu-id="3a9a7-268">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="3a9a7-268">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="3a9a7-269">Klicka på **Bläddra bland alla** från den vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-269">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="3a9a7-270">Klicka på **inställningar** från den översta menyn och klicka sedan på **Klusterinloggning**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-270">Click **Settings** from the top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="3a9a7-271">Om **klusterinloggning** har aktiverats, måste du klicka på **inaktivera**, och klicka sedan på **aktivera** innan du kan ändra det användarnamn och lösenord...</span><span class="sxs-lookup"><span data-stu-id="3a9a7-271">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change the username and password..</span></span>
5. <span data-ttu-id="3a9a7-272">Ändra den **klustret inloggningsnamnet** och/eller den **klustret inloggningslösenordet**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-272">Change the **Cluster Login Name** and/or the **Cluster Login Password**, and then click **Save**.</span></span>

    ![HDInsight ändra klustret användarnamn lösenord http-användare](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a><span data-ttu-id="3a9a7-274">Bevilja/återkalla åtkomst</span><span class="sxs-lookup"><span data-stu-id="3a9a7-274">Grant/revoke access</span></span>
<span data-ttu-id="3a9a7-275">HDInsight-kluster har följande HTTP-webbtjänster (alla dessa tjänster har RESTful slutpunkter):</span><span class="sxs-lookup"><span data-stu-id="3a9a7-275">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="3a9a7-276">ODBC</span><span class="sxs-lookup"><span data-stu-id="3a9a7-276">ODBC</span></span>
* <span data-ttu-id="3a9a7-277">JDBC</span><span class="sxs-lookup"><span data-stu-id="3a9a7-277">JDBC</span></span>
* <span data-ttu-id="3a9a7-278">Ambari</span><span class="sxs-lookup"><span data-stu-id="3a9a7-278">Ambari</span></span>
* <span data-ttu-id="3a9a7-279">Oozie</span><span class="sxs-lookup"><span data-stu-id="3a9a7-279">Oozie</span></span>
* <span data-ttu-id="3a9a7-280">Templeton</span><span class="sxs-lookup"><span data-stu-id="3a9a7-280">Templeton</span></span>

<span data-ttu-id="3a9a7-281">Som standard beviljas dessa tjänster för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-281">By default, these services are granted for access.</span></span> <span data-ttu-id="3a9a7-282">Du kan återkalla/bevilja åtkomst från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-282">You can revoke/grant the access from the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="3a9a7-283">Genom att bevilja/återkalla åtkomst, återställs klustret användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-283">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
>
>

<span data-ttu-id="3a9a7-284">**Att bevilja/återkalla åtkomst HTTP services webbåtkomst**</span><span class="sxs-lookup"><span data-stu-id="3a9a7-284">**To grant/revoke HTTP web services access**</span></span>

1. <span data-ttu-id="3a9a7-285">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="3a9a7-285">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="3a9a7-286">Klicka på **Bläddra bland alla** från den vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-286">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="3a9a7-287">Klicka på **inställningar** från den översta menyn och klicka sedan på **Klusterinloggning**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-287">Click **Settings** from the top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="3a9a7-288">Om **klusterinloggning** har aktiverats, måste du klicka på **inaktivera**, och klicka sedan på **aktivera** innan du kan ändra det användarnamn och lösenord...</span><span class="sxs-lookup"><span data-stu-id="3a9a7-288">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change the username and password..</span></span>
5. <span data-ttu-id="3a9a7-289">För **klustret inloggning användarnamn** och **klustret inloggningslösenordet**, ange nytt användarnamn och lösenord (respektive) för klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-289">For **Cluster Login Username** and **Cluster Login Password**, enter the new user name and password (respectively) for the cluster.</span></span>
6. <span data-ttu-id="3a9a7-290">Klicka på **SPARA**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-290">Click **SAVE**.</span></span>

    ![HDInsight grand ta bort HTTP-tjänsten webbåtkomst](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-the-default-storage-account"></a><span data-ttu-id="3a9a7-292">Hitta standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="3a9a7-292">Find the default storage account</span></span>
<span data-ttu-id="3a9a7-293">Varje HDInsight-kluster har en standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-293">Each HDInsight cluster has a default storage account.</span></span> <span data-ttu-id="3a9a7-294">Standardkontot för lagring och dess nycklar för ett kluster visas under **inställningar**/**egenskaper**/**Azure Lagringsnycklar**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-294">The default storage account and its keys for a cluster appears under **Settings**/**Properties**/**Azure Storage Keys**.</span></span> <span data-ttu-id="3a9a7-295">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-295">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="find-the-resource-group"></a><span data-ttu-id="3a9a7-296">Hitta resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-296">Find the resource group</span></span>
<span data-ttu-id="3a9a7-297">I läget Azure Resource Manager skapas varje HDInsight-kluster med en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-297">In the Azure Resource Manager mode, each HDInsight cluster is created with an Azure resource group.</span></span> <span data-ttu-id="3a9a7-298">Azure-resursgrupp som tillhör ett kluster visas i:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-298">The Azure resource group that a cluster belongs to appears in:</span></span>

* <span data-ttu-id="3a9a7-299">Listan över kluster har en **resursgruppen** kolumn.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-299">The cluster list has a **Resource Group** column.</span></span>
* <span data-ttu-id="3a9a7-300">Klustret **väsentliga** panelen.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-300">Cluster **Essential** tile.</span></span>  

<span data-ttu-id="3a9a7-301">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-301">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="open-hdinsight-query-console"></a><span data-ttu-id="3a9a7-302">Öppna konsolen för HDInsight-fråga</span><span class="sxs-lookup"><span data-stu-id="3a9a7-302">Open HDInsight Query console</span></span>
<span data-ttu-id="3a9a7-303">Konsolen HDInsight frågan innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-303">The HDInsight Query console includes the following features:</span></span>

* <span data-ttu-id="3a9a7-304">**Hive Editor**: A GUI Webbgränssnitt för att skicka Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-304">**Hive Editor**: A GUI web interface for submitting Hive jobs.</span></span>  <span data-ttu-id="3a9a7-305">Se [köra Hive-frågor via konsolen frågan](hdinsight-hadoop-use-hive-query-console.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-305">See [Run Hive queries using the Query Console](hdinsight-hadoop-use-hive-query-console.md).</span></span>

    ![HDInsight portal hive-redigeraren](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* <span data-ttu-id="3a9a7-307">**Jobbhistorik**: övervaka Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-307">**Job history**: Monitor Hadoop jobs.</span></span>  

    ![HDInsight portal jobbhistorik](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    <span data-ttu-id="3a9a7-309">Klicka på **Frågenamnet** att visa information, inklusive jobbegenskaper, **Jobbfråga**, och ** Jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-309">Click **Query Name** to show the details including Job properties, **Job Query**, and **Job Output.</span></span> <span data-ttu-id="3a9a7-310">Du kan också hämta både frågan och utdata till din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-310">You can also download both the query and the output to your workstation.</span></span>
* <span data-ttu-id="3a9a7-311">**Filen webbläsare**: Bläddra standardkontot för lagring och länkade storage-konton.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-311">**File Browser**: Browse the default storage account and the linked storage accounts.</span></span>

    ![HDInsight portal webbläsare filsökning](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    <span data-ttu-id="3a9a7-313">På skärmbilden, den  **<Account>**  anger objektet är ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-313">On the screenshot, the **<Account>** type indicates the item is an Azure storage account.</span></span>  <span data-ttu-id="3a9a7-314">Klicka på namnet på kontot för att bläddra till filerna.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-314">Click the account name to browse the files.</span></span>
* <span data-ttu-id="3a9a7-315">**Hadoop-Användargränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-315">**Hadoop UI**.</span></span>

    ![HDInsight Hadoop UI-portalen](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    <span data-ttu-id="3a9a7-317">Från **Hadoop UI*, kan du bläddra bland filer och loggarna.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-317">From **Hadoop UI*, you can browse files, and check logs.</span></span>
* <span data-ttu-id="3a9a7-318">**Yarn UI**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-318">**Yarn UI**.</span></span>

    ![HDInsight portal YARN-Användargränssnittet](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a><span data-ttu-id="3a9a7-320">Köra Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="3a9a7-320">Run Hive queries</span></span>
<span data-ttu-id="3a9a7-321">Till körde Hive-jobb från portalen klickar du på **Hive-redigeraren** i konsolen HDInsight frågan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-321">To ran Hive jobs from the Portal, click **Hive Editor** in the HDInsight Query console.</span></span> <span data-ttu-id="3a9a7-322">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-322">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-jobs"></a><span data-ttu-id="3a9a7-323">Övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="3a9a7-323">Monitor jobs</span></span>
<span data-ttu-id="3a9a7-324">Om du vill övervaka jobb från portalen klickar du på **jobbhistorik** i konsolen HDInsight frågan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-324">To monitor jobs from the Portal, click **Job History** in the HDInsight Query console.</span></span> <span data-ttu-id="3a9a7-325">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-325">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="browse-files"></a><span data-ttu-id="3a9a7-326">Bläddra bland filer</span><span class="sxs-lookup"><span data-stu-id="3a9a7-326">Browse files</span></span>
<span data-ttu-id="3a9a7-327">Om du vill bläddra bland filer som lagras i standardkontot för lagring och länkade storage-konton, klickar du på **Filhanteraren** i konsolen HDInsight frågan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-327">To browse files stored in the default storage account and the linked storage accounts, click **File Browser** in the HDInsight Query console.</span></span> <span data-ttu-id="3a9a7-328">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-328">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

<span data-ttu-id="3a9a7-329">Du kan också använda den **Bläddra filsystemet** utility från den **Hadoop UI** i HDInsight-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-329">You can also use the **Browse the file system** utility from the **Hadoop UI** in the HDInsight console.</span></span>  <span data-ttu-id="3a9a7-330">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-330">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-cluster-usage"></a><span data-ttu-id="3a9a7-331">Övervaka kluster användning</span><span class="sxs-lookup"><span data-stu-id="3a9a7-331">Monitor cluster usage</span></span>
<span data-ttu-id="3a9a7-332">Den **användning** på HDInsight-klusterbladet visar information om antal kärnor som är tillgängliga för din prenumeration för användning med HDInsight, samt antal kärnor som allokerats till detta kluster och hur de är allokerade för alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-332">The **Usage** section of the HDInsight cluster blade displays information about the number of cores available to your subscription for use with HDInsight, as well as the number of cores allocated to this cluster and how they are allocated for the nodes within this cluster.</span></span> <span data-ttu-id="3a9a7-333">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-333">See [List and show clusters](#list-and-show-clusters).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a9a7-334">För att övervaka tjänster som tillhandahålls av HDInsight-kluster, måste du använda Ambari Web eller Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-334">To monitor the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="3a9a7-335">Mer information om hur du använder Ambari finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="3a9a7-335">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>
>
>

## <a name="open-hadoop-ui"></a><span data-ttu-id="3a9a7-336">Öppna Användargränssnittet för Hadoop</span><span class="sxs-lookup"><span data-stu-id="3a9a7-336">Open Hadoop UI</span></span>
<span data-ttu-id="3a9a7-337">Om du vill övervaka klustret Bläddra i filsystemet och loggarna klickar du på **Hadoop UI** i konsolen HDInsight frågan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-337">To monitor the cluster, browse the file system, and check logs, click **Hadoop UI** in the HDInsight Query console.</span></span> <span data-ttu-id="3a9a7-338">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-338">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="open-yarn-ui"></a><span data-ttu-id="3a9a7-339">Öppna Yarn UI</span><span class="sxs-lookup"><span data-stu-id="3a9a7-339">Open Yarn UI</span></span>
<span data-ttu-id="3a9a7-340">Om du vill använda Yarn-användargränssnittet **Yarn-Användargränssnittet** i konsolen HDInsight frågan.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-340">To use Yarn user interface, click **Yarn UI** in the HDInsight Query console.</span></span> <span data-ttu-id="3a9a7-341">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-341">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="connect-to-clusters-using-rdp"></a><span data-ttu-id="3a9a7-342">Anslut till kluster med RDP</span><span class="sxs-lookup"><span data-stu-id="3a9a7-342">Connect to clusters using RDP</span></span>
<span data-ttu-id="3a9a7-343">Autentiseringsuppgifterna för det kluster som du har angett på skapades ge åtkomst till tjänsterna i klustret, men inte själva klustret via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-343">The credentials for the cluster that you provided at its creation give access to the services on the cluster, but not to the cluster itself through Remote Desktop.</span></span> <span data-ttu-id="3a9a7-344">Du kan aktivera åtkomst via Fjärrskrivbord när du konfigurerar ett kluster eller efter ett kluster har etablerats.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-344">You can turn on Remote Desktop access when you provision a cluster or after a cluster is provisioned.</span></span> <span data-ttu-id="3a9a7-345">Anvisningar om hur du aktiverar fjärrskrivbord på Skapa finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-345">For the instructions about enabling Remote Desktop at creation, see [Create HDInsight cluster](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="3a9a7-346">**Aktivera Fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="3a9a7-346">**To enable Remote Desktop**</span></span>

1. <span data-ttu-id="3a9a7-347">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="3a9a7-347">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="3a9a7-348">Klicka på **Bläddra bland alla** från den vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-348">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="3a9a7-349">Klicka på **inställningar** från den översta menyn och klicka sedan på **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-349">Click **Settings** from the top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="3a9a7-350">Ange **upphör att gälla på**, **användarnamnet för fjärrskrivbord** och **Remote Desktop lösenord**, och klicka sedan på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-350">Enter **Expires On**, **Remote Desktop Username** and **Remote Desktop Password**, and then click **Enable**.</span></span>

    ![HDInsight aktivera inaktivera Konfigurera fjärrskrivbord](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    <span data-ttu-id="3a9a7-352">Standardvärden för upphör att gälla på är en vecka.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-352">The default values for Expires On is a week.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3a9a7-353">Du kan också använda HDInsight .NET SDK för att aktivera Fjärrskrivbord på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-353">You can also use the HDInsight .NET SDK to enable Remote Desktop on a cluster.</span></span> <span data-ttu-id="3a9a7-354">Använd den **EnableRdp** metod på HDInsight klient-objekt på följande sätt: **klienten. EnableRdp (klusternamn, plats, ”rdpuser”, ”rdppassword”, DateTime.Now.AddDays(6))**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-354">Use the **EnableRdp** method on the HDInsight client object in the following manner: **client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**.</span></span> <span data-ttu-id="3a9a7-355">På samma sätt för att inaktivera Fjärrskrivbord på klustret, du kan använda **klienten. DisableRdp (klusternamn, platsen)**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-355">Similarly, to disable Remote Desktop on the cluster, you can use **client.DisableRdp(clustername, location)**.</span></span> <span data-ttu-id="3a9a7-356">Mer information om dessa metoder finns [HDInsight .NET SDK referens](http://go.microsoft.com/fwlink/?LinkId=529017).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-356">For more information on these methods, see [HDInsight .NET SDK Reference](http://go.microsoft.com/fwlink/?LinkId=529017).</span></span> <span data-ttu-id="3a9a7-357">Detta gäller endast för HDInsight-kluster som körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-357">This is applicable only for HDInsight clusters running on Windows.</span></span>
   >
   >

<span data-ttu-id="3a9a7-358">**Att ansluta till ett kluster med RDP**</span><span class="sxs-lookup"><span data-stu-id="3a9a7-358">**To connect to a cluster by using RDP**</span></span>

1. <span data-ttu-id="3a9a7-359">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="3a9a7-359">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="3a9a7-360">Klicka på **Bläddra bland alla** från den vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-360">Click **Browse All** from the left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="3a9a7-361">Klicka på **inställningar** från den översta menyn och klicka sedan på **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-361">Click **Settings** from the top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="3a9a7-362">Klicka på **Anslut** och följ instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-362">Click **Connect** and follow the instructions.</span></span> <span data-ttu-id="3a9a7-363">Om Connect inaktivera, måste du aktivera den först.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-363">If Connect is disable, you must enable it first.</span></span> <span data-ttu-id="3a9a7-364">Kontrollera med hjälp av fjärrskrivbord användarens användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-364">Make sure using the Remote Desktop user username and password.</span></span>  <span data-ttu-id="3a9a7-365">Du kan inte använda autentiseringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-365">You can't use the Cluster user credentials.</span></span>

## <a name="open-hadoop-command-line"></a><span data-ttu-id="3a9a7-366">Öppna kommandoraden för Hadoop</span><span class="sxs-lookup"><span data-stu-id="3a9a7-366">Open Hadoop command line</span></span>
<span data-ttu-id="3a9a7-367">Om du vill ansluta till klustret med hjälp av fjärrskrivbord och använda Hadoop-kommandoraden, måste du först ha aktiverat Fjärrskrivbord åtkomst till klustret enligt beskrivningen i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-367">To connect to the cluster by using Remote Desktop and use the Hadoop command line, you must first have enabled Remote Desktop access to the cluster as described in the previous section.</span></span>

<span data-ttu-id="3a9a7-368">**Öppna en kommandorad för Hadoop**</span><span class="sxs-lookup"><span data-stu-id="3a9a7-368">**To open a Hadoop command line**</span></span>

1. <span data-ttu-id="3a9a7-369">Anslut till klustret med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-369">Connect to the cluster using Remote Desktop.</span></span>
2. <span data-ttu-id="3a9a7-370">Från skrivbordet, dubbelklickar du på **Hadoop kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-370">From the desktop, double-click **Hadoop Command Line**.</span></span>

    <span data-ttu-id="3a9a7-371">![HDI. HadoopCommandLine][image-hadoopcommandline]</span><span class="sxs-lookup"><span data-stu-id="3a9a7-371">![HDI.HadoopCommandLine][image-hadoopcommandline]</span></span>

    <span data-ttu-id="3a9a7-372">Mer information om Hadoop-kommandon finns [Hadoop kommandon referens](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span><span class="sxs-lookup"><span data-stu-id="3a9a7-372">For more information on Hadoop commands, see [Hadoop commands reference](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span></span>

<span data-ttu-id="3a9a7-373">Mappnamnet har versionsnumret Hadoop inbäddade i föregående skärmbild.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-373">In the previous screenshot, the folder name has the Hadoop version number embedded.</span></span> <span data-ttu-id="3a9a7-374">Versionsnumret kan ändras beroende på vilken version av Hadoop-komponenter som installeras i klustret.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-374">The version number can changed based on the version of the Hadoop components installed on the cluster.</span></span> <span data-ttu-id="3a9a7-375">Du kan använda Hadoop miljövariabler för att referera till dessa mappar.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-375">You can use Hadoop environment variables to refer to those folders.</span></span> <span data-ttu-id="3a9a7-376">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-376">For example:</span></span>

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a><span data-ttu-id="3a9a7-377">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a9a7-377">Next steps</span></span>
<span data-ttu-id="3a9a7-378">I den här artikeln har du lärt dig hur du skapar ett HDInsight-kluster med hjälp av portalen och hur du öppnar kommandoradsverktyget Hadoop.</span><span class="sxs-lookup"><span data-stu-id="3a9a7-378">In this article, you have learned how to create an HDInsight cluster by using the Portal, and how to open the Hadoop command-line tool.</span></span> <span data-ttu-id="3a9a7-379">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="3a9a7-379">To learn more, see the following articles:</span></span>

* [<span data-ttu-id="3a9a7-380">Administrera HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3a9a7-380">Administer HDInsight Using Azure PowerShell</span></span>](hdinsight-administer-use-powershell.md)
* [<span data-ttu-id="3a9a7-381">Administrera HDInsight med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3a9a7-381">Administer HDInsight Using Azure CLI</span></span>](hdinsight-administer-use-command-line.md)
* [<span data-ttu-id="3a9a7-382">Skapa HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="3a9a7-382">Create HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="3a9a7-383">Skicka Hadoop-jobb via programmering</span><span class="sxs-lookup"><span data-stu-id="3a9a7-383">Submit Hadoop jobs programmatically</span></span>](hdinsight-submit-hadoop-jobs-programmatically.md)
* [<span data-ttu-id="3a9a7-384">Komma igång med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="3a9a7-384">Get Started with Azure HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="3a9a7-385">Vilken version av Hadoop finns i Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="3a9a7-385">What version of Hadoop is in Azure HDInsight?</span></span>](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop-kommandorad"
