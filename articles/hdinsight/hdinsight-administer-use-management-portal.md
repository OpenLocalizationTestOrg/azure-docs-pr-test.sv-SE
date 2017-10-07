---
title: aaaManage Windows-baserade Hadoop-kluster i HDInsight med hello Azure-portalen | Microsoft Docs
description: "Lär dig hur tooadminister HDInsight Service. Skapa ett HDInsight-klustret, öppna hello interaktiva JavaScript-konsolen och öppna hello Hadoop kommandokonsolen."
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
ms.openlocfilehash: a237726b0e37a08005ce22e96581739e93edb050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a><span data-ttu-id="02f4f-104">Hantera Windows-baserade Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="02f4f-104">Manage Windows-based Hadoop clusters in HDInsight by using hello Azure portal</span></span>

<span data-ttu-id="02f4f-105">Med hjälp av hello [Azure-portalen][azure-portal], du kan skapa Windows-baserade Hadoop-kluster i Azure HDInsight, ändra Hadoop användarlösenord och aktivera Remote Desktop Protocol (RDP) så att du kan komma åt hello Hadoop kommandokonsolen på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-105">Using hello [Azure portal][azure-portal], you can create Windows-based Hadoop clusters in Azure HDInsight, change Hadoop user password, and enable Remote Desktop Protocol (RDP) so you can access hello Hadoop command console on hello cluster.</span></span>

<span data-ttu-id="02f4f-106">hello information i den här artikeln gäller bara tooWindow-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-106">hello information in this article only applies tooWindow-based HDInsight clusters.</span></span> <span data-ttu-id="02f4f-107">Information om hur du hanterar Linux-baserade kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-107">For information on managing Linux-based clusters, see [Manage Hadoop clusters in HDInsight by using hello Azure portal](hdinsight-administer-use-portal-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="02f4f-108">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="02f4f-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="02f4f-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="02f4f-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="02f4f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="02f4f-110">Prerequisites</span></span>

<span data-ttu-id="02f4f-111">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="02f4f-111">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="02f4f-112">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-112">**An Azure subscription**.</span></span> <span data-ttu-id="02f4f-113">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="02f4f-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="02f4f-114">**Azure Storage-konto** -ett HDInsight-kluster använder en Azure Blob storage-behållare som hello standardfilsystem.</span><span class="sxs-lookup"><span data-stu-id="02f4f-114">**Azure Storage account** - An HDInsight cluster uses an Azure Blob storage container as hello default file system.</span></span> <span data-ttu-id="02f4f-115">Mer information om hur Azure Blob storage ger en sömlös upplevelse med HDInsight-kluster finns [använda Azure Blob Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-115">For more information about how Azure Blob storage provides a seamless experience with HDInsight clusters, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="02f4f-116">Mer information om hur du skapar ett Azure Storage-konto finns [hur tooCreate ett Lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-116">For details on creating an Azure Storage account, see [How tooCreate a Storage Account](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="open-hello-portal"></a><span data-ttu-id="02f4f-117">Öppna hello Portal</span><span class="sxs-lookup"><span data-stu-id="02f4f-117">Open hello Portal</span></span>
1. <span data-ttu-id="02f4f-118">Logga in för[https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="02f4f-118">Sign in too[https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="02f4f-119">När du har öppnat hello-portalen kan du:</span><span class="sxs-lookup"><span data-stu-id="02f4f-119">After you open hello portal, you can:</span></span>

   * <span data-ttu-id="02f4f-120">Klicka på **ny** från hello vänstra menyn toocreate ett nytt kluster:</span><span class="sxs-lookup"><span data-stu-id="02f4f-120">Click **New** from hello left menu toocreate a new cluster:</span></span>

       ![ny knapp för HDInsight-kluster](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * <span data-ttu-id="02f4f-122">Klicka på **HDInsight-kluster** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="02f4f-122">Click **HDInsight Clusters** from hello left menu.</span></span>

       ![Azure portal HDInsight-kluster-knappen](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     <span data-ttu-id="02f4f-124">Om **HDInsight** inte visas i hello vänstra menyn klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-124">If **HDInsight** doesn't appear in hello left menu, click **Browse**.</span></span>

     ![Azure portal knappen Bläddra klustret.](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a><span data-ttu-id="02f4f-126">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="02f4f-126">Create clusters</span></span>
<span data-ttu-id="02f4f-127">Anvisningar hello skapas med hjälp av hello Portal finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-127">For hello creation instructions using hello Portal, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="02f4f-128">HDInsight fungerar med en bred Hadoop-komponenter.</span><span class="sxs-lookup"><span data-stu-id="02f4f-128">HDInsight works with a wide range of Hadoop components.</span></span> <span data-ttu-id="02f4f-129">Hello lista hello-komponenter som har verifierats, och som stöds finns i [vilken version av Hadoop finns i Azure HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-129">For hello list of hello components that have been verified and supported, see [What version of Hadoop is in Azure HDInsight](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="02f4f-130">Du kan anpassa HDInsight med något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="02f4f-130">You can customize HDInsight by using one of hello following options:</span></span>

* <span data-ttu-id="02f4f-131">Använd skriptåtgärder toorun anpassade skript som kan anpassa ett kluster tooeither ändrar klusterkonfigurationen eller installera anpassade komponenter, till exempel Giraph eller Solr.</span><span class="sxs-lookup"><span data-stu-id="02f4f-131">Use Script Action toorun custom scripts that can customize a cluster tooeither change cluster configuration or install custom components such as Giraph or Solr.</span></span> <span data-ttu-id="02f4f-132">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-132">For more information, see [Customize HDInsight cluster using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span>
* <span data-ttu-id="02f4f-133">Använd hello anpassning Klusterparametrar i hello HDInsight .NET SDK eller Azure PowerShell när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="02f4f-133">Use hello cluster customization parameters in hello HDInsight .NET SDK or Azure PowerShell during cluster creation.</span></span> <span data-ttu-id="02f4f-134">De här konfigurationsändringarna bevaras sedan via hello livstid hello kluster och påverkas inte av klustret nod reimages som Azure-plattformen utför med jämna mellanrum för underhåll.</span><span class="sxs-lookup"><span data-stu-id="02f4f-134">These configuration changes are then preserved through hello lifetime of hello cluster and are not affected by cluster node reimages that Azure platform periodically performs for maintenance.</span></span> <span data-ttu-id="02f4f-135">Mer information om hur du använder hello Klusterparametrar anpassning finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-135">For more information on using hello cluster customization parameters, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="02f4f-136">Vissa inbyggda Java-komponenter som Mahout och kaskad, kan köras på hello klustret som JAR-filer.</span><span class="sxs-lookup"><span data-stu-id="02f4f-136">Some native Java components, like Mahout and Cascading, can be run on hello cluster as JAR files.</span></span> <span data-ttu-id="02f4f-137">Dessa JAR-filer kan vara distribuerade tooAzure Blob storage och skicka tooHDInsight kluster via Hadoop jobbet skicka mekanismer.</span><span class="sxs-lookup"><span data-stu-id="02f4f-137">These JAR files can be distributed tooAzure Blob storage, and submitted tooHDInsight clusters through Hadoop job submission mechanisms.</span></span> <span data-ttu-id="02f4f-138">Mer information finns i [skicka Hadoop-jobb via programmering](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-138">For more information, see [Submit Hadoop jobs programmatically](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

  > [!NOTE]
  > <span data-ttu-id="02f4f-139">Om du har problem med distribution av JAR-filer tooHDInsight kluster eller anropa JAR-filer på HDInsight-kluster, kontakta [Microsoft-supporten](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="02f4f-139">If you have issues deploying JAR files tooHDInsight clusters or calling JAR files on HDInsight clusters, contact [Microsoft Support](https://azure.microsoft.com/support/options/).</span></span>
  >
  > <span data-ttu-id="02f4f-140">Sammanhängande stöds inte av HDInsight och är inte tillgänglig för Microsofts Support.</span><span class="sxs-lookup"><span data-stu-id="02f4f-140">Cascading is not supported by HDInsight, and is not eligible for Microsoft Support.</span></span> <span data-ttu-id="02f4f-141">Listor med stödda komponenter finns [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-141">For lists of supported components, see [What's new in hello cluster versions provided by HDInsight](hdinsight-component-versioning.md).</span></span>
  >
  >

<span data-ttu-id="02f4f-142">Anpassade programvara installeras på hello kluster med hjälp av anslutning till fjärrskrivbord stöds inte.</span><span class="sxs-lookup"><span data-stu-id="02f4f-142">Installation of custom software on hello cluster by using Remote Desktop Connection is not supported.</span></span> <span data-ttu-id="02f4f-143">Du bör inte lagra filer för hello enheter på hello huvudnod, eftersom de kommer att förloras om du behöver toore-skapa hello kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-143">You should avoid storing any files on hello drives of hello head node, as they will be lost if you need toore-create hello clusters.</span></span> <span data-ttu-id="02f4f-144">Vi rekommenderar att du lagrar filer på Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="02f4f-144">We recommend storing files on Azure Blob storage.</span></span> <span data-ttu-id="02f4f-145">BLOB storage är permanent.</span><span class="sxs-lookup"><span data-stu-id="02f4f-145">Blob storage is persistent.</span></span>

## <a name="list-and-show-clusters"></a><span data-ttu-id="02f4f-146">Listan och visa kluster</span><span class="sxs-lookup"><span data-stu-id="02f4f-146">List and show clusters</span></span>
1. <span data-ttu-id="02f4f-147">Logga in för[https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="02f4f-147">Sign in too[https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="02f4f-148">Klicka på **HDInsight-kluster** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="02f4f-148">Click **HDInsight Clusters** from hello left menu.</span></span>
3. <span data-ttu-id="02f4f-149">Klicka på hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-149">Click hello cluster name.</span></span> <span data-ttu-id="02f4f-150">Om hello klusterlista är långt kan du använda filter på hello hello överst.</span><span class="sxs-lookup"><span data-stu-id="02f4f-150">If hello cluster list is long, you can use filter on hello top of hello page.</span></span>
4. <span data-ttu-id="02f4f-151">Dubbelklicka på ett kluster från hello tooshow hello information.</span><span class="sxs-lookup"><span data-stu-id="02f4f-151">Double-click a cluster from hello list tooshow hello details.</span></span>

    <span data-ttu-id="02f4f-152">**Menyn och essentials**:</span><span class="sxs-lookup"><span data-stu-id="02f4f-152">**Menu and essentials**:</span></span>

    ![Azure portal HDInsight-kluster essentials](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * <span data-ttu-id="02f4f-154">toocustomize hello-menyn högerklickar du någonstans på hello-menyn och klicka sedan på **anpassa**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-154">toocustomize hello menu, right-click anywhere on hello menu, and then click **Customize**.</span></span>
   * <span data-ttu-id="02f4f-155">**Inställningar för** och **alla inställningar**: Visar hello **inställningar** bladet för hello-kluster, vilket gör att du tooaccess detaljerad konfigurationsinformation för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-155">**Settings** and **All Settings**: Displays hello **Settings** blade for hello cluster, which allows you tooaccess detailed configuration information for hello cluster.</span></span>
   * <span data-ttu-id="02f4f-156">**Instrumentpanelen**, **Klusterinstrumentpanel** och **URL: dessa är alla sätt tooaccess hello instrumentpanelen klustret, vilket är Ambari Web för Linux-baserade kluster. -**Secure Shell **: Visar hello instruktioner tooconnect toohello kluster med hjälp av SSH (Secure Shell)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="02f4f-156">**Dashboard**, **Cluster Dashboard** and **URL: These are all ways tooaccess hello cluster dashboard, which is Ambari Web for Linux-based clusters. -**Secure Shell**: Shows hello instructions tooconnect toohello cluster using Secure Shell (SSH) connection.</span></span>
   * <span data-ttu-id="02f4f-157">**Skala klustret**: tillåter toochange hello antalet arbetarnoder för det här klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-157">**Scale Cluster**: Allows you toochange hello number of worker nodes for this cluster.</span></span>
   * <span data-ttu-id="02f4f-158">**Ta bort**: tar bort hello klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-158">**Delete**: Deletes hello cluster.</span></span>
   * <span data-ttu-id="02f4f-159">**Snabbstart**: Visar information som hjälper dig att komma igång med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="02f4f-159">**Quickstart**: Displays information that will help you get started using HDInsight.</span></span>
   * <span data-ttu-id="02f4f-160">** Användare: Ger tooset behörigheter för *portal management* för detta kluster för andra användare på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-160">**Users: Allows you tooset permissions for *portal management* of this cluster for other users on your Azure subscription.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="02f4f-161">Detta *endast* påverkar åtkomst och behörighet toothis klustret i hello Azure-portalen och har ingen effekt på vem som kan ansluta tooor skicka jobb toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-161">This *only* affects access and permissions toothis cluster in hello Azure portal, and has no effect on who can connect tooor submit jobs toohello HDInsight cluster.</span></span>
     >
     >
   * <span data-ttu-id="02f4f-162">**Taggar**: etiketter kan du tooset nyckel/värde-par toodefine en anpassad taxonomi av dina molntjänster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-162">**Tags**: Tags allow you tooset key/value pairs toodefine a custom taxonomy of your cloud services.</span></span> <span data-ttu-id="02f4f-163">Du kan till exempel skapa en nyckel som heter **projekt**, och sedan använda ett värde som är gemensamma för alla tjänster som är associerad med ett visst projekt.</span><span class="sxs-lookup"><span data-stu-id="02f4f-163">For example, you may create a key named **project**, and then use a common value for all services associated with a specific project.</span></span>
   * <span data-ttu-id="02f4f-164">**Ambari Views**: länkar tooAmbari Web.</span><span class="sxs-lookup"><span data-stu-id="02f4f-164">**Ambari Views**: Links tooAmbari Web.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="02f4f-165">toomanage hello tjänster som tillhandahålls av hello HDInsight-kluster, måste du använda Ambari Web eller hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="02f4f-165">toomanage hello services provided by hello HDInsight cluster, you must use Ambari Web or hello Ambari REST API.</span></span> <span data-ttu-id="02f4f-166">Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-166">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
     >
     >

     <span data-ttu-id="02f4f-167">**Användning**:</span><span class="sxs-lookup"><span data-stu-id="02f4f-167">**Usage**:</span></span>

     ![Azure portal HDInsight-kluster användning](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. <span data-ttu-id="02f4f-169">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-169">Click **Settings**.</span></span>

    ![Azure portal HDInsight-kluster användning](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * <span data-ttu-id="02f4f-171">**Egenskaper för**: Visa egenskaper för hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-171">**Properties**: View hello cluster properties.</span></span>
   * <span data-ttu-id="02f4f-172">**Kluster-AAD-identitet**:</span><span class="sxs-lookup"><span data-stu-id="02f4f-172">**Cluster AAD Identity**:</span></span>
   * <span data-ttu-id="02f4f-173">**Azure Storage-nycklar**: Visa hello standardkontot för lagring och dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="02f4f-173">**Azure Storage Keys**: View hello default storage account and its key.</span></span> <span data-ttu-id="02f4f-174">Hej lagringskonto är konfigurationen under hello klusterskapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-174">hello storage account is configuration during hello cluster creation process.</span></span>
   * <span data-ttu-id="02f4f-175">**Klustret inloggningen**: ändra hello klustret HTTP-användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="02f4f-175">**Cluster Login**: Change hello cluster HTTP user name and password.</span></span>
   * <span data-ttu-id="02f4f-176">**Externa Metastores**: Visa hello Hive och Oozie metastores.</span><span class="sxs-lookup"><span data-stu-id="02f4f-176">**External Metastores**: View hello Hive and Oozie metastores.</span></span> <span data-ttu-id="02f4f-177">Hej metastores kan bara konfigureras när hello klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="02f4f-177">hello metastores can only be configured during hello cluster creation process.</span></span>
   * <span data-ttu-id="02f4f-178">**Skala klustret**: ökning och minskning hello antalet arbetarnoder i klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-178">**Scale Cluster**: Increase and decrease hello number of cluster worker nodes.</span></span>
   * <span data-ttu-id="02f4f-179">**Fjärrskrivbord**: aktivera och inaktivera åtkomst via fjärrskrivbord (RDP) och konfigurera hello RDP-användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-179">**Remote Desktop**: Enable and disable remote desktop (RDP) access, and configure hello RDP username.</span></span>  <span data-ttu-id="02f4f-180">hello RDP-användarnamnet måste skilja sig från hello HTTP-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="02f4f-180">hello RDP user name must be different from hello HTTP user name.</span></span>
   * <span data-ttu-id="02f4f-181">**Auktoriserad partner**:</span><span class="sxs-lookup"><span data-stu-id="02f4f-181">**Partner of Record**:</span></span>

     > [!NOTE]
     > <span data-ttu-id="02f4f-182">Det här är en generisk lista över tillgängliga inställningar. inte alla finnas för alla typer av klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-182">This is a generic list of available settings; not all of them will be present for all cluster types.</span></span>
     >
     >
6. <span data-ttu-id="02f4f-183">Klicka på **egenskaper**:</span><span class="sxs-lookup"><span data-stu-id="02f4f-183">Click **Properties**:</span></span>

    <span data-ttu-id="02f4f-184">Hej egenskapsavsnittet visar hello följande:</span><span class="sxs-lookup"><span data-stu-id="02f4f-184">hello properties section lists hello following:</span></span>

   * <span data-ttu-id="02f4f-185">**Hostname**: klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="02f4f-185">**Hostname**: Cluster name.</span></span>
   * <span data-ttu-id="02f4f-186">**Kluster-URL: en**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-186">**Cluster URL**.</span></span>
   * <span data-ttu-id="02f4f-187">**Status för**: inkludera avbröts, godkända ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, fungerar, kör, fel, ta bort, tas bort, för lång tid, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued ResizeQueued, ClusterCustomization</span><span class="sxs-lookup"><span data-stu-id="02f4f-187">**Status**: Include Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization</span></span>
   * <span data-ttu-id="02f4f-188">**Region**: Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="02f4f-188">**Region**: Azure location.</span></span> <span data-ttu-id="02f4f-189">En lista över Azure platser som stöds finns i hello **Region** nedrullningsbar listruta i [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="02f4f-189">For a list of supported Azure locations, see hello **Region** dropdown list box on [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>
   * <span data-ttu-id="02f4f-190">**Data som har skapats**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-190">**Data created**.</span></span>
   * <span data-ttu-id="02f4f-191">**Operativsystemet**: antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-191">**Operating system**: Either **Windows** or **Linux**.</span></span>
   * <span data-ttu-id="02f4f-192">**Typen**: Hadoop, HBase, Storm, Väck.</span><span class="sxs-lookup"><span data-stu-id="02f4f-192">**Type**: Hadoop, HBase, Storm, Spark.</span></span>
   * <span data-ttu-id="02f4f-193">**Version**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-193">**Version**.</span></span> <span data-ttu-id="02f4f-194">Se [HDInsight-versioner](hdinsight-component-versioning.md)</span><span class="sxs-lookup"><span data-stu-id="02f4f-194">See [HDInsight versions](hdinsight-component-versioning.md)</span></span>
   * <span data-ttu-id="02f4f-195">**Prenumerationen**: prenumerationsnamn.</span><span class="sxs-lookup"><span data-stu-id="02f4f-195">**Subscription**: Subscription name.</span></span>
   * <span data-ttu-id="02f4f-196">**Prenumerations-ID**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-196">**Subscription ID**.</span></span>
   * <span data-ttu-id="02f4f-197">**Primär datakälla**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-197">**Primary data source**.</span></span> <span data-ttu-id="02f4f-198">hello Azure Blob storage-konto som används som standard hello Hadoop-filsystem.</span><span class="sxs-lookup"><span data-stu-id="02f4f-198">hello Azure Blob storage account used as hello default Hadoop file system.</span></span>
   * <span data-ttu-id="02f4f-199">**Arbetsnoderna prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-199">**Worker nodes pricing tier**.</span></span>
   * <span data-ttu-id="02f4f-200">**Huvudnod prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-200">**Head node pricing tier**.</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="02f4f-201">Ta bort kluster</span><span class="sxs-lookup"><span data-stu-id="02f4f-201">Delete clusters</span></span>
<span data-ttu-id="02f4f-202">Ett kluster tas inte bort hello standardkontot för lagring eller eventuella länkade lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="02f4f-202">Delete a cluster will not delete hello default storage account or any linked storage accounts.</span></span> <span data-ttu-id="02f4f-203">Du kan återskapa hello kluster med hjälp av hello samma storage-konton och hello samma metastores.</span><span class="sxs-lookup"><span data-stu-id="02f4f-203">You can re-create hello cluster by using hello same storage accounts and hello same metastores.</span></span>

1. <span data-ttu-id="02f4f-204">Logga in toohello [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="02f4f-204">Sign in toohello [Portal][azure-portal].</span></span>
2. <span data-ttu-id="02f4f-205">Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-205">Click **Browse All** from hello left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="02f4f-206">Klicka på **ta bort** hello översta menyn och sedan hello anvisningar.</span><span class="sxs-lookup"><span data-stu-id="02f4f-206">Click **Delete** from hello top menu, and then follow hello instructions.</span></span>

<span data-ttu-id="02f4f-207">Se även [paus/stängs av kluster](#pauseshut-down-clusters).</span><span class="sxs-lookup"><span data-stu-id="02f4f-207">See also [Pause/shut down clusters](#pauseshut-down-clusters).</span></span>

## <a name="scale-clusters"></a><span data-ttu-id="02f4f-208">Skala kluster</span><span class="sxs-lookup"><span data-stu-id="02f4f-208">Scale clusters</span></span>
<span data-ttu-id="02f4f-209">hello klustret skalning funktionen kan du toochange hello antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan toore-skapa hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-209">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="02f4f-210">Endast kluster med HDInsight version 3.1.3 eller högre stöds.</span><span class="sxs-lookup"><span data-stu-id="02f4f-210">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="02f4f-211">Du kan kontrollera hello-egenskapssidan om du är osäker på hello version av klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-211">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="02f4f-212">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="02f4f-212">See [List and show clusters](#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="02f4f-213">hello konsekvenser av att ändra hello antalet datanoder för varje typ av kluster som stöds av HDInsight:</span><span class="sxs-lookup"><span data-stu-id="02f4f-213">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="02f4f-214">Hadoop</span><span class="sxs-lookup"><span data-stu-id="02f4f-214">Hadoop</span></span>

    <span data-ttu-id="02f4f-215">Sömlöst kan du öka hello antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb.</span><span class="sxs-lookup"><span data-stu-id="02f4f-215">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="02f4f-216">Nya jobb kan också skicka medan hello-åtgärd pågår.</span><span class="sxs-lookup"><span data-stu-id="02f4f-216">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="02f4f-217">Fel i en åtgärd för skalning hanteras korrekt så att hello klustret alltid lämnas i ett fungerande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="02f4f-217">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>

    <span data-ttu-id="02f4f-218">När ett Hadoop-kluster skalas genom att minska hello antalet datanoder hello tjänster i hello kluster har startats om.</span><span class="sxs-lookup"><span data-stu-id="02f4f-218">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="02f4f-219">Detta medför att alla körs och väntande jobb toofail på hello hello skalning åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="02f4f-219">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="02f4f-220">Du kan dock skicka hello jobb när hello åtgärden är klar.</span><span class="sxs-lookup"><span data-stu-id="02f4f-220">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="02f4f-221">HBase</span><span class="sxs-lookup"><span data-stu-id="02f4f-221">HBase</span></span>

    <span data-ttu-id="02f4f-222">Sömlöst kan du lägga till eller ta bort noder tooyour HBase-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="02f4f-222">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="02f4f-223">Regional servrar balanseras automatiskt inom några minuter efter att du avslutat hello skalning igen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-223">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="02f4f-224">Du kan också manuellt balansera hello regionala servrar genom att logga in hello headnode för klustret och köra hello följande kommandon från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="02f4f-224">However, you can also manually balance hello regional servers by logging into hello headnode of cluster and running hello following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    <span data-ttu-id="02f4f-225">Mer information om hur du använder hello HBase-gränssnittet finns i]</span><span class="sxs-lookup"><span data-stu-id="02f4f-225">For more information on using hello HBase shell, see []</span></span>
* <span data-ttu-id="02f4f-226">Storm</span><span class="sxs-lookup"><span data-stu-id="02f4f-226">Storm</span></span>

    <span data-ttu-id="02f4f-227">Sömlöst kan du lägga till eller ta bort data noder tooyour Storm-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="02f4f-227">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="02f4f-228">Men när en slutförd hello skalning åtgärden måste toorebalance hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="02f4f-228">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>

    <span data-ttu-id="02f4f-229">Ombalansering kan utföras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="02f4f-229">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="02f4f-230">Storm webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="02f4f-230">Storm web UI</span></span>
  * <span data-ttu-id="02f4f-231">Verktyget kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="02f4f-231">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="02f4f-232">Se toohello [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="02f4f-232">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="02f4f-233">hello Storm webbgränssnittet är tillgänglig på hello HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="02f4f-233">hello Storm web UI is available on hello HDInsight cluster:</span></span>

    ![HDInsight storm skala balansera](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    <span data-ttu-id="02f4f-235">Här är ett exempel hur toouse hello CLI kommandot toorebalance hello Storm-topologi:</span><span class="sxs-lookup"><span data-stu-id="02f4f-235">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="02f4f-236">**tooscale kluster**</span><span class="sxs-lookup"><span data-stu-id="02f4f-236">**tooscale clusters**</span></span>

1. <span data-ttu-id="02f4f-237">Logga in toohello [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="02f4f-237">Sign in toohello [Portal][azure-portal].</span></span>
2. <span data-ttu-id="02f4f-238">Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-238">Click **Browse All** from hello left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="02f4f-239">Klicka på **inställningar** hello översta menyn och klicka sedan på **kluster**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-239">Click **Settings** from hello top menu, and then click **Scale Cluster**.</span></span>
4. <span data-ttu-id="02f4f-240">Ange **numret för arbetarnoder**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-240">Enter **Number of Worker nodes**.</span></span> <span data-ttu-id="02f4f-241">hello varierar hello antalet klusternod mellan olika Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="02f4f-241">hello limit on hello number of cluster node varies among Azure subscriptions.</span></span> <span data-ttu-id="02f4f-242">Du kan kontakta faktureringssupporten tooincrease hello supportgränsen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-242">You can contact billing support tooincrease hello limit.</span></span>  <span data-ttu-id="02f4f-243">hello kostnadsinformation visar hello ändringar du har gjort toohello antalet noder.</span><span class="sxs-lookup"><span data-stu-id="02f4f-243">hello cost information will reflect hello changes you have made toohello number of nodes.</span></span>

    ![HDInsight Hadoop HBase, Storm Spark skala](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a><span data-ttu-id="02f4f-245">Pausa/stängs av kluster</span><span class="sxs-lookup"><span data-stu-id="02f4f-245">Pause/shut down clusters</span></span>
<span data-ttu-id="02f4f-246">De flesta Hadoop-jobb är batchjobb som bara ibland kördes.</span><span class="sxs-lookup"><span data-stu-id="02f4f-246">Most of Hadoop jobs are batch jobs that are only ran occasionally.</span></span> <span data-ttu-id="02f4f-247">Det finns stora perioder för de flesta Hadoop-kluster tid hello klustret inte används för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="02f4f-247">For most Hadoop clusters, there are large periods of time that hello cluster is not being used for processing.</span></span> <span data-ttu-id="02f4f-248">Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används.</span><span class="sxs-lookup"><span data-stu-id="02f4f-248">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span>
<span data-ttu-id="02f4f-249">Du debiteras också för ett HDInsight-kluster, även när det inte används.</span><span class="sxs-lookup"><span data-stu-id="02f4f-249">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="02f4f-250">Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används.</span><span class="sxs-lookup"><span data-stu-id="02f4f-250">Since hello charges for hello cluster are many times more than hello charges for storage, it makes economic sense toodelete clusters when they are not in use.</span></span>

<span data-ttu-id="02f4f-251">Det finns många sätt som du kan programmet hello processen:</span><span class="sxs-lookup"><span data-stu-id="02f4f-251">There are many ways you can program hello process:</span></span>

* <span data-ttu-id="02f4f-252">Användaren Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="02f4f-252">User Azure Data Factory.</span></span> <span data-ttu-id="02f4f-253">Se [länkad Azure HDInsight-tjänst](../data-factory/data-factory-compute-linked-services.md) och [transformera och analysera med hjälp av Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) för HDInsight på begäran och automatisk definierad länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-253">See [Azure HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md) and [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md) for on-demand and self-defined HDInsight linked services.</span></span>
* <span data-ttu-id="02f4f-254">Använda Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="02f4f-254">Use Azure PowerShell.</span></span>  <span data-ttu-id="02f4f-255">Se [analysera svarta fördröjning](hdinsight-analyze-flight-delay-data.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-255">See [Analyze flight delay data](hdinsight-analyze-flight-delay-data.md).</span></span>
* <span data-ttu-id="02f4f-256">Använda Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="02f4f-256">Use Azure CLI.</span></span> <span data-ttu-id="02f4f-257">Se [hantera HDInsight-kluster med hjälp av Azure CLI](hdinsight-administer-use-command-line.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-257">See [Manage HDInsight clusters using Azure CLI](hdinsight-administer-use-command-line.md).</span></span>
* <span data-ttu-id="02f4f-258">Använda HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="02f4f-258">Use HDInsight .NET SDK.</span></span> <span data-ttu-id="02f4f-259">Se [skicka Hadoop-jobb](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-259">See [Submit Hadoop jobs](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

<span data-ttu-id="02f4f-260">Hello information om priser, se [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="02f4f-260">For hello pricing information, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span> <span data-ttu-id="02f4f-261">toodelete ett kluster från hello-portalen finns [ta bort kluster](#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="02f4f-261">toodelete a cluster from hello Portal, see [Delete clusters](#delete-clusters)</span></span>

## <a name="change-cluster-username"></a><span data-ttu-id="02f4f-262">Ändra kluster användarnamn</span><span class="sxs-lookup"><span data-stu-id="02f4f-262">Change cluster username</span></span>
<span data-ttu-id="02f4f-263">Ett HDInsight-kluster kan ha två användarkonton.</span><span class="sxs-lookup"><span data-stu-id="02f4f-263">An HDInsight cluster can have two user accounts.</span></span> <span data-ttu-id="02f4f-264">hello användarkonto för HDInsight-kluster skapas under skapandeprocessen hello.</span><span class="sxs-lookup"><span data-stu-id="02f4f-264">hello HDInsight cluster user account is created during hello creation process.</span></span> <span data-ttu-id="02f4f-265">Du kan också skapa ett RDP-användarkonto för att komma åt hello klustret via RDP.</span><span class="sxs-lookup"><span data-stu-id="02f4f-265">You can also create an RDP user account for accessing hello cluster via RDP.</span></span> <span data-ttu-id="02f4f-266">Se [aktivera Fjärrskrivbord](#connect-to-hdinsight-clusters-by-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="02f4f-266">See [Enable remote desktop](#connect-to-hdinsight-clusters-by-using-rdp).</span></span>

<span data-ttu-id="02f4f-267">**toochange hello HDInsight-kluster, användarnamn och lösenord**</span><span class="sxs-lookup"><span data-stu-id="02f4f-267">**toochange hello HDInsight cluster user name and password**</span></span>

1. <span data-ttu-id="02f4f-268">Logga in toohello [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="02f4f-268">Sign in toohello [Portal][azure-portal].</span></span>
2. <span data-ttu-id="02f4f-269">Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-269">Click **Browse All** from hello left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="02f4f-270">Klicka på **inställningar** hello översta menyn och klicka sedan på **Klusterinloggning**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-270">Click **Settings** from hello top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="02f4f-271">Om **klusterinloggning** har aktiverats, måste du klicka på **inaktivera**, och klicka sedan på **aktivera** innan du kan ändra hello användarnamn och lösenord...</span><span class="sxs-lookup"><span data-stu-id="02f4f-271">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change hello username and password..</span></span>
5. <span data-ttu-id="02f4f-272">Ändra hello **klustret inloggningsnamnet** och/eller hello **klustret inloggningslösenordet**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-272">Change hello **Cluster Login Name** and/or hello **Cluster Login Password**, and then click **Save**.</span></span>

    ![HDInsight ändra klustret användarnamn lösenord http-användare](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a><span data-ttu-id="02f4f-274">Bevilja/återkalla åtkomst</span><span class="sxs-lookup"><span data-stu-id="02f4f-274">Grant/revoke access</span></span>
<span data-ttu-id="02f4f-275">HDInsight-kluster har hello efter http-webbtjänster (alla dessa tjänster har RESTful slutpunkter):</span><span class="sxs-lookup"><span data-stu-id="02f4f-275">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="02f4f-276">ODBC</span><span class="sxs-lookup"><span data-stu-id="02f4f-276">ODBC</span></span>
* <span data-ttu-id="02f4f-277">JDBC</span><span class="sxs-lookup"><span data-stu-id="02f4f-277">JDBC</span></span>
* <span data-ttu-id="02f4f-278">Ambari</span><span class="sxs-lookup"><span data-stu-id="02f4f-278">Ambari</span></span>
* <span data-ttu-id="02f4f-279">Oozie</span><span class="sxs-lookup"><span data-stu-id="02f4f-279">Oozie</span></span>
* <span data-ttu-id="02f4f-280">Templeton</span><span class="sxs-lookup"><span data-stu-id="02f4f-280">Templeton</span></span>

<span data-ttu-id="02f4f-281">Som standard beviljas dessa tjänster för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="02f4f-281">By default, these services are granted for access.</span></span> <span data-ttu-id="02f4f-282">Du kan återkalla/bevilja hello åtkomst från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-282">You can revoke/grant hello access from hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="02f4f-283">Genom att bevilja/återkalla hello åtkomst, återställs hello klustret användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="02f4f-283">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
>
>

<span data-ttu-id="02f4f-284">**toogrant/revoke HTTP services webbåtkomst**</span><span class="sxs-lookup"><span data-stu-id="02f4f-284">**toogrant/revoke HTTP web services access**</span></span>

1. <span data-ttu-id="02f4f-285">Logga in toohello [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="02f4f-285">Sign in toohello [Portal][azure-portal].</span></span>
2. <span data-ttu-id="02f4f-286">Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-286">Click **Browse All** from hello left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="02f4f-287">Klicka på **inställningar** hello översta menyn och klicka sedan på **Klusterinloggning**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-287">Click **Settings** from hello top menu, and then click **Cluster Login**.</span></span>
4. <span data-ttu-id="02f4f-288">Om **klusterinloggning** har aktiverats, måste du klicka på **inaktivera**, och klicka sedan på **aktivera** innan du kan ändra hello användarnamn och lösenord...</span><span class="sxs-lookup"><span data-stu-id="02f4f-288">If **Cluster login** has been enabled, you must click **Disable**, and then click **Enable** before you can change hello username and password..</span></span>
5. <span data-ttu-id="02f4f-289">För **klustret inloggning användarnamn** och **klustret inloggningslösenordet**, Skriv hello nytt användarnamn och lösenord (respektive) för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-289">For **Cluster Login Username** and **Cluster Login Password**, enter hello new user name and password (respectively) for hello cluster.</span></span>
6. <span data-ttu-id="02f4f-290">Klicka på **SPARA**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-290">Click **SAVE**.</span></span>

    ![HDInsight grand ta bort HTTP-tjänsten webbåtkomst](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="02f4f-292">Hitta hello standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="02f4f-292">Find hello default storage account</span></span>
<span data-ttu-id="02f4f-293">Varje HDInsight-kluster har en standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="02f4f-293">Each HDInsight cluster has a default storage account.</span></span> <span data-ttu-id="02f4f-294">Hej standardkontot för lagring och dess nycklar för ett kluster visas under **inställningar**/**egenskaper**/**Azure Lagringsnycklar**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-294">hello default storage account and its keys for a cluster appears under **Settings**/**Properties**/**Azure Storage Keys**.</span></span> <span data-ttu-id="02f4f-295">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="02f4f-295">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="find-hello-resource-group"></a><span data-ttu-id="02f4f-296">Hitta hello resursgruppen</span><span class="sxs-lookup"><span data-stu-id="02f4f-296">Find hello resource group</span></span>
<span data-ttu-id="02f4f-297">I läget för hello Azure Resource Manager skapas varje HDInsight-kluster med en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="02f4f-297">In hello Azure Resource Manager mode, each HDInsight cluster is created with an Azure resource group.</span></span> <span data-ttu-id="02f4f-298">hello Azure-resursgrupp som ett kluster tillhör tooappears i:</span><span class="sxs-lookup"><span data-stu-id="02f4f-298">hello Azure resource group that a cluster belongs tooappears in:</span></span>

* <span data-ttu-id="02f4f-299">Hej klusterlista har en **resursgruppen** kolumn.</span><span class="sxs-lookup"><span data-stu-id="02f4f-299">hello cluster list has a **Resource Group** column.</span></span>
* <span data-ttu-id="02f4f-300">Klustret **väsentliga** panelen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-300">Cluster **Essential** tile.</span></span>  

<span data-ttu-id="02f4f-301">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="02f4f-301">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="open-hdinsight-query-console"></a><span data-ttu-id="02f4f-302">Öppna konsolen för HDInsight-fråga</span><span class="sxs-lookup"><span data-stu-id="02f4f-302">Open HDInsight Query console</span></span>
<span data-ttu-id="02f4f-303">Hej HDInsight frågan konsolen innehåller hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="02f4f-303">hello HDInsight Query console includes hello following features:</span></span>

* <span data-ttu-id="02f4f-304">**Hive Editor**: A GUI Webbgränssnitt för att skicka Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="02f4f-304">**Hive Editor**: A GUI web interface for submitting Hive jobs.</span></span>  <span data-ttu-id="02f4f-305">Se [köra Hive-frågor med hello frågan konsolen](hdinsight-hadoop-use-hive-query-console.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-305">See [Run Hive queries using hello Query Console](hdinsight-hadoop-use-hive-query-console.md).</span></span>

    ![HDInsight portal hive-redigeraren](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* <span data-ttu-id="02f4f-307">**Jobbhistorik**: övervaka Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="02f4f-307">**Job history**: Monitor Hadoop jobs.</span></span>  

    ![HDInsight portal jobbhistorik](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    <span data-ttu-id="02f4f-309">Klicka på **Frågenamnet** tooshow hello information, inklusive jobbegenskaper, **Jobbfråga**, och ** Jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="02f4f-309">Click **Query Name** tooshow hello details including Job properties, **Job Query**, and **Job Output.</span></span> <span data-ttu-id="02f4f-310">Du kan också hämta både hello frågan och hello utdata tooyour arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-310">You can also download both hello query and hello output tooyour workstation.</span></span>
* <span data-ttu-id="02f4f-311">**Filen webbläsare**: Bläddra hello standardkontot för lagring och hello länkade lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="02f4f-311">**File Browser**: Browse hello default storage account and hello linked storage accounts.</span></span>

    ![HDInsight portal webbläsare filsökning](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    <span data-ttu-id="02f4f-313">På skärmbilden hello hello  **<Account>**  anger hello-objektet är ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="02f4f-313">On hello screenshot, hello **<Account>** type indicates hello item is an Azure storage account.</span></span>  <span data-ttu-id="02f4f-314">Klicka på hello konto namn toobrowse hello filer.</span><span class="sxs-lookup"><span data-stu-id="02f4f-314">Click hello account name toobrowse hello files.</span></span>
* <span data-ttu-id="02f4f-315">**Hadoop-Användargränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-315">**Hadoop UI**.</span></span>

    ![HDInsight Hadoop UI-portalen](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    <span data-ttu-id="02f4f-317">Från **Hadoop UI*, kan du bläddra bland filer och loggarna.</span><span class="sxs-lookup"><span data-stu-id="02f4f-317">From **Hadoop UI*, you can browse files, and check logs.</span></span>
* <span data-ttu-id="02f4f-318">**Yarn UI**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-318">**Yarn UI**.</span></span>

    ![HDInsight portal YARN-Användargränssnittet](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a><span data-ttu-id="02f4f-320">Köra Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="02f4f-320">Run Hive queries</span></span>
<span data-ttu-id="02f4f-321">tooran Hive-jobb från hello-portalen klickar du på **Hive-redigeraren** i hello HDInsight fråga-konsolen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-321">tooran Hive jobs from hello Portal, click **Hive Editor** in hello HDInsight Query console.</span></span> <span data-ttu-id="02f4f-322">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="02f4f-322">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-jobs"></a><span data-ttu-id="02f4f-323">Övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="02f4f-323">Monitor jobs</span></span>
<span data-ttu-id="02f4f-324">toomonitor jobb från hello-portalen klickar du på **jobbhistorik** i hello HDInsight fråga-konsolen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-324">toomonitor jobs from hello Portal, click **Job History** in hello HDInsight Query console.</span></span> <span data-ttu-id="02f4f-325">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="02f4f-325">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="browse-files"></a><span data-ttu-id="02f4f-326">Bläddra bland filer</span><span class="sxs-lookup"><span data-stu-id="02f4f-326">Browse files</span></span>
<span data-ttu-id="02f4f-327">toobrowse filer som lagras i hello standardkontot för lagring och hello länkade lagringskonton, klickar du på **Filhanteraren** i hello HDInsight fråga-konsolen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-327">toobrowse files stored in hello default storage account and hello linked storage accounts, click **File Browser** in hello HDInsight Query console.</span></span> <span data-ttu-id="02f4f-328">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="02f4f-328">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

<span data-ttu-id="02f4f-329">Du kan också använda hello **Bläddra hello filsystemet** utility från hello **Hadoop UI** i hello HDInsight-konsolen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-329">You can also use hello **Browse hello file system** utility from hello **Hadoop UI** in hello HDInsight console.</span></span>  <span data-ttu-id="02f4f-330">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="02f4f-330">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="monitor-cluster-usage"></a><span data-ttu-id="02f4f-331">Övervaka kluster användning</span><span class="sxs-lookup"><span data-stu-id="02f4f-331">Monitor cluster usage</span></span>
<span data-ttu-id="02f4f-332">Hej **användning** avsnitt i hello HDInsight-klusterbladet visar information om hello antal kärnor tillgängliga tooyour prenumerationen för användning med HDInsight, samt hello antal kärnor som allokerats toothis kluster och hur de tilldelat för hello noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="02f4f-332">hello **Usage** section of hello HDInsight cluster blade displays information about hello number of cores available tooyour subscription for use with HDInsight, as well as hello number of cores allocated toothis cluster and how they are allocated for hello nodes within this cluster.</span></span> <span data-ttu-id="02f4f-333">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="02f4f-333">See [List and show clusters](#list-and-show-clusters).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="02f4f-334">toomonitor hello tjänster som tillhandahålls av hello HDInsight-kluster, måste du använda Ambari Web eller hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="02f4f-334">toomonitor hello services provided by hello HDInsight cluster, you must use Ambari Web or hello Ambari REST API.</span></span> <span data-ttu-id="02f4f-335">Mer information om hur du använder Ambari finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="02f4f-335">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>
>
>

## <a name="open-hadoop-ui"></a><span data-ttu-id="02f4f-336">Öppna Användargränssnittet för Hadoop</span><span class="sxs-lookup"><span data-stu-id="02f4f-336">Open Hadoop UI</span></span>
<span data-ttu-id="02f4f-337">toomonitor hello klustret, bläddra hello filsystemet och kontrollera loggarna, klickar du på **Hadoop UI** i hello HDInsight fråga-konsolen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-337">toomonitor hello cluster, browse hello file system, and check logs, click **Hadoop UI** in hello HDInsight Query console.</span></span> <span data-ttu-id="02f4f-338">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="02f4f-338">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="open-yarn-ui"></a><span data-ttu-id="02f4f-339">Öppna Yarn UI</span><span class="sxs-lookup"><span data-stu-id="02f4f-339">Open Yarn UI</span></span>
<span data-ttu-id="02f4f-340">toouse Yarn-användargränssnittet, klickar du på **Yarn-Användargränssnittet** i hello HDInsight fråga-konsolen.</span><span class="sxs-lookup"><span data-stu-id="02f4f-340">toouse Yarn user interface, click **Yarn UI** in hello HDInsight Query console.</span></span> <span data-ttu-id="02f4f-341">Se [HDInsight fråga konsolen](#open-hdinsight-query-console).</span><span class="sxs-lookup"><span data-stu-id="02f4f-341">See [Open HDInsight Query console](#open-hdinsight-query-console).</span></span>

## <a name="connect-tooclusters-using-rdp"></a><span data-ttu-id="02f4f-342">Ansluta tooclusters med RDP</span><span class="sxs-lookup"><span data-stu-id="02f4f-342">Connect tooclusters using RDP</span></span>
<span data-ttu-id="02f4f-343">hello autentiseringsuppgifter för hello-kluster som du angav vid starten ge åtkomst toohello tjänster på hello kluster, men inte toohello själva klustret via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="02f4f-343">hello credentials for hello cluster that you provided at its creation give access toohello services on hello cluster, but not toohello cluster itself through Remote Desktop.</span></span> <span data-ttu-id="02f4f-344">Du kan aktivera åtkomst via Fjärrskrivbord när du konfigurerar ett kluster eller efter ett kluster har etablerats.</span><span class="sxs-lookup"><span data-stu-id="02f4f-344">You can turn on Remote Desktop access when you provision a cluster or after a cluster is provisioned.</span></span> <span data-ttu-id="02f4f-345">Hello instruktioner om hur du aktiverar Fjärrskrivbord när skapas finns [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="02f4f-345">For hello instructions about enabling Remote Desktop at creation, see [Create HDInsight cluster](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="02f4f-346">**tooenable fjärrskrivbord**</span><span class="sxs-lookup"><span data-stu-id="02f4f-346">**tooenable Remote Desktop**</span></span>

1. <span data-ttu-id="02f4f-347">Logga in toohello [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="02f4f-347">Sign in toohello [Portal][azure-portal].</span></span>
2. <span data-ttu-id="02f4f-348">Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-348">Click **Browse All** from hello left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="02f4f-349">Klicka på **inställningar** hello översta menyn och klicka sedan på **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-349">Click **Settings** from hello top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="02f4f-350">Ange **upphör att gälla på**, **användarnamnet för fjärrskrivbord** och **Remote Desktop lösenord**, och klicka sedan på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-350">Enter **Expires On**, **Remote Desktop Username** and **Remote Desktop Password**, and then click **Enable**.</span></span>

    ![HDInsight aktivera inaktivera Konfigurera fjärrskrivbord](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    <span data-ttu-id="02f4f-352">hello standardvärden för upphör att gälla på är en vecka.</span><span class="sxs-lookup"><span data-stu-id="02f4f-352">hello default values for Expires On is a week.</span></span>

   > [!NOTE]
   > <span data-ttu-id="02f4f-353">Du kan också använda hello HDInsight .NET SDK tooenable fjärrskrivbord på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-353">You can also use hello HDInsight .NET SDK tooenable Remote Desktop on a cluster.</span></span> <span data-ttu-id="02f4f-354">Använd hello **EnableRdp** metod på hello HDInsight klientobjekt i hello följande sätt: **klienten. EnableRdp (klusternamn, plats, ”rdpuser”, ”rdppassword”, DateTime.Now.AddDays(6))**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-354">Use hello **EnableRdp** method on hello HDInsight client object in hello following manner: **client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**.</span></span> <span data-ttu-id="02f4f-355">På liknande sätt toodisable fjärrskrivbord på hello klustret kan du använda **klienten. DisableRdp (klusternamn, platsen)**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-355">Similarly, toodisable Remote Desktop on hello cluster, you can use **client.DisableRdp(clustername, location)**.</span></span> <span data-ttu-id="02f4f-356">Mer information om dessa metoder finns [HDInsight .NET SDK referens](http://go.microsoft.com/fwlink/?LinkId=529017).</span><span class="sxs-lookup"><span data-stu-id="02f4f-356">For more information on these methods, see [HDInsight .NET SDK Reference](http://go.microsoft.com/fwlink/?LinkId=529017).</span></span> <span data-ttu-id="02f4f-357">Detta gäller endast för HDInsight-kluster som körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="02f4f-357">This is applicable only for HDInsight clusters running on Windows.</span></span>
   >
   >

<span data-ttu-id="02f4f-358">**tooconnect tooa kluster med hjälp av RDP**</span><span class="sxs-lookup"><span data-stu-id="02f4f-358">**tooconnect tooa cluster by using RDP**</span></span>

1. <span data-ttu-id="02f4f-359">Logga in toohello [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="02f4f-359">Sign in toohello [Portal][azure-portal].</span></span>
2. <span data-ttu-id="02f4f-360">Klicka på **Bläddra bland alla** hello vänstra menyn klickar du på **HDInsight-kluster**, klickar du på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="02f4f-360">Click **Browse All** from hello left menu, click **HDInsight Clusters**, click your cluster name.</span></span>
3. <span data-ttu-id="02f4f-361">Klicka på **inställningar** hello översta menyn och klicka sedan på **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-361">Click **Settings** from hello top menu, and then click **Remote Desktop**.</span></span>
4. <span data-ttu-id="02f4f-362">Klicka på **Anslut** och följer instruktionerna för hello.</span><span class="sxs-lookup"><span data-stu-id="02f4f-362">Click **Connect** and follow hello instructions.</span></span> <span data-ttu-id="02f4f-363">Om Connect inaktivera, måste du aktivera den först.</span><span class="sxs-lookup"><span data-stu-id="02f4f-363">If Connect is disable, you must enable it first.</span></span> <span data-ttu-id="02f4f-364">Kontrollera med hello fjärrskrivbord användarens användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="02f4f-364">Make sure using hello Remote Desktop user username and password.</span></span>  <span data-ttu-id="02f4f-365">Du kan inte använda hello klustret användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="02f4f-365">You can't use hello Cluster user credentials.</span></span>

## <a name="open-hadoop-command-line"></a><span data-ttu-id="02f4f-366">Öppna kommandoraden för Hadoop</span><span class="sxs-lookup"><span data-stu-id="02f4f-366">Open Hadoop command line</span></span>
<span data-ttu-id="02f4f-367">tooconnect toohello kluster med hjälp av fjärrskrivbord och Använd hello Hadoop-kommandoraden, måste du först ha aktiverat Fjärrskrivbord åtkomst toohello klustret enligt beskrivningen i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="02f4f-367">tooconnect toohello cluster by using Remote Desktop and use hello Hadoop command line, you must first have enabled Remote Desktop access toohello cluster as described in hello previous section.</span></span>

<span data-ttu-id="02f4f-368">**tooopen en kommandorad för Hadoop**</span><span class="sxs-lookup"><span data-stu-id="02f4f-368">**tooopen a Hadoop command line**</span></span>

1. <span data-ttu-id="02f4f-369">Ansluta toohello kluster med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="02f4f-369">Connect toohello cluster using Remote Desktop.</span></span>
2. <span data-ttu-id="02f4f-370">Dubbelklicka på skrivbordet hello **Hadoop kommandoraden**.</span><span class="sxs-lookup"><span data-stu-id="02f4f-370">From hello desktop, double-click **Hadoop Command Line**.</span></span>

    <span data-ttu-id="02f4f-371">![HDI. HadoopCommandLine][image-hadoopcommandline]</span><span class="sxs-lookup"><span data-stu-id="02f4f-371">![HDI.HadoopCommandLine][image-hadoopcommandline]</span></span>

    <span data-ttu-id="02f4f-372">Mer information om Hadoop-kommandon finns [Hadoop kommandon referens](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span><span class="sxs-lookup"><span data-stu-id="02f4f-372">For more information on Hadoop commands, see [Hadoop commands reference](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).</span></span>

<span data-ttu-id="02f4f-373">I föregående skärmbilden hello har hello mappnamn hello Hadoop versionsnummer inbäddade.</span><span class="sxs-lookup"><span data-stu-id="02f4f-373">In hello previous screenshot, hello folder name has hello Hadoop version number embedded.</span></span> <span data-ttu-id="02f4f-374">hello versionsnumret kan ändrade utifrån hello version av hello Hadoop-komponenter installeras på hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="02f4f-374">hello version number can changed based on hello version of hello Hadoop components installed on hello cluster.</span></span> <span data-ttu-id="02f4f-375">Du kan använda Hadoop-miljön variabler toorefer toothose mappar.</span><span class="sxs-lookup"><span data-stu-id="02f4f-375">You can use Hadoop environment variables toorefer toothose folders.</span></span> <span data-ttu-id="02f4f-376">Exempel:</span><span class="sxs-lookup"><span data-stu-id="02f4f-376">For example:</span></span>

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a><span data-ttu-id="02f4f-377">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02f4f-377">Next steps</span></span>
<span data-ttu-id="02f4f-378">I den här artikeln har du lärt dig hur toocreate ett HDInsight-kluster med hjälp av hello Portal och hur tooopen hello Hadoop-kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="02f4f-378">In this article, you have learned how toocreate an HDInsight cluster by using hello Portal, and how tooopen hello Hadoop command-line tool.</span></span> <span data-ttu-id="02f4f-379">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="02f4f-379">toolearn more, see hello following articles:</span></span>

* [<span data-ttu-id="02f4f-380">Administrera HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="02f4f-380">Administer HDInsight Using Azure PowerShell</span></span>](hdinsight-administer-use-powershell.md)
* [<span data-ttu-id="02f4f-381">Administrera HDInsight med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="02f4f-381">Administer HDInsight Using Azure CLI</span></span>](hdinsight-administer-use-command-line.md)
* [<span data-ttu-id="02f4f-382">Skapa HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="02f4f-382">Create HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="02f4f-383">Skicka Hadoop-jobb via programmering</span><span class="sxs-lookup"><span data-stu-id="02f4f-383">Submit Hadoop jobs programmatically</span></span>](hdinsight-submit-hadoop-jobs-programmatically.md)
* [<span data-ttu-id="02f4f-384">Komma igång med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="02f4f-384">Get Started with Azure HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="02f4f-385">Vilken version av Hadoop finns i Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="02f4f-385">What version of Hadoop is in Azure HDInsight?</span></span>](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop-kommandorad"
