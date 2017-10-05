---
title: "Hantera Hadoop-kluster i HDInsight med hjälp av Azure-portalen | Microsoft Docs"
description: "Lär dig mer om att skapa och hantera HDInsight-kluster med Azure-portalen."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: c9cb631aef71f72457c3517d02566a56919f82bc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a><span data-ttu-id="90bb1-103">Hantera Hadoop-kluster i HDInsight med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="90bb1-103">Manage Hadoop clusters in HDInsight by using the Azure portal</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="90bb1-104">Med hjälp av den [Azure-portalen][azure-portal], kan du hantera Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="90bb1-104">Using the [Azure portal][azure-portal], you can manage Hadoop clusters in Azure HDInsight.</span></span> <span data-ttu-id="90bb1-105">Använd flikväljaren för information om hur du hanterar Hadoop-kluster i HDInsight med andra verktyg.</span><span class="sxs-lookup"><span data-stu-id="90bb1-105">Use the tab selector for information on managing Hadoop clusters in HDInsight using other tools.</span></span>

<span data-ttu-id="90bb1-106">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="90bb1-106">**Prerequisites**</span></span>

<span data-ttu-id="90bb1-107">Innan du börjar den här artikeln, måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="90bb1-107">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="90bb1-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-108">**An Azure subscription**.</span></span> <span data-ttu-id="90bb1-109">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="90bb1-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="open-the-portal"></a><span data-ttu-id="90bb1-110">Öppna portalen</span><span class="sxs-lookup"><span data-stu-id="90bb1-110">Open the portal</span></span>
1. <span data-ttu-id="90bb1-111">Logga in på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90bb1-111">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="90bb1-112">När du öppnar portalen kan du:</span><span class="sxs-lookup"><span data-stu-id="90bb1-112">After you open the portal, you can:</span></span>

   * <span data-ttu-id="90bb1-113">Klicka på **ny** i den vänstra menyn för att skapa ett nytt kluster:</span><span class="sxs-lookup"><span data-stu-id="90bb1-113">Click **New** from the left menu to create a new cluster:</span></span>

       ![ny knapp för HDInsight-kluster](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * <span data-ttu-id="90bb1-115">Klicka på **HDInsight-kluster** i den vänstra menyn för att visa befintliga kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-115">Click **HDInsight Clusters** from the left menu to list the existing clusters</span></span>

       ![Azure portal HDInsight-kluster-knappen](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       <span data-ttu-id="90bb1-117">Om du inte ser HDInsight-kluster, klickar du på **fler tjänster** längst ned i listan och klicka sedan på **HDInsight-kluster** under den **Intelligence + analys** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="90bb1-117">If you don't see HDInsight cluster, click **More services** on the bottom of the list, and then click **HDInsight clusters** under the **Intelligence + Analytics** section.</span></span>


## <a name="create-clusters"></a><span data-ttu-id="90bb1-118">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-118">Create clusters</span></span>
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="90bb1-119">HDInsight fungerar med en bred Hadoop-komponenter.</span><span class="sxs-lookup"><span data-stu-id="90bb1-119">HDInsight works with a wide range of Hadoop components.</span></span> <span data-ttu-id="90bb1-120">Lista över de komponenter som har verifierats, och som stöds finns i [vilken version av Hadoop finns i Azure HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-120">For the list of the components that have been verified and supported, see [What version of Hadoop is in Azure HDInsight](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="90bb1-121">För allmän klustret skapas information, se [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-121">For the general cluster creation information, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

### <a name="access-control-requirements"></a><span data-ttu-id="90bb1-122">Åtkomstkontrollkrav</span><span class="sxs-lookup"><span data-stu-id="90bb1-122">Access control requirements</span></span>

<span data-ttu-id="90bb1-123">När du skapar ett HDInsight-kluster måste du ange en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="90bb1-123">You must specify an Azure subscription when you create an HDInsight cluster.</span></span> <span data-ttu-id="90bb1-124">Det här klustret kan skapas i en ny Azure resursgrupp eller en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="90bb1-124">This cluster can be created in either a new Azure Resource group or an existing Resource group.</span></span> <span data-ttu-id="90bb1-125">Du kan använda följande steg för att verifiera din behörighet för att skapa HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="90bb1-125">You can use the following steps to verify your permissions for creating HDInsight clusters:</span></span>

- <span data-ttu-id="90bb1-126">Att använda en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="90bb1-126">To use an existing resource group.</span></span>

    1. <span data-ttu-id="90bb1-127">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90bb1-127">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
    2. <span data-ttu-id="90bb1-128">Klicka på **resursgrupper** i den vänstra menyn att lista resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="90bb1-128">Click **Resource groups** from the left menu to list the resource groups.</span></span>
    3. <span data-ttu-id="90bb1-129">Klicka på den resursgrupp som du vill använda för att skapa ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-129">Click the resource group you want to use for creating your HDInsight cluster.</span></span>
    4. <span data-ttu-id="90bb1-130">Klicka på **åtkomstkontroll (IAM)**, och kontrollera att du (eller en grupp som du tillhör) har minst deltagare åtkomst till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-130">Click **Access control (IAM)**, and verify that you (or a group that you belong to) have at least the Contributor access to the resource group.</span></span>

- <span data-ttu-id="90bb1-131">Skapa en ny resursgrupp</span><span class="sxs-lookup"><span data-stu-id="90bb1-131">To create a new resource group</span></span>

    1. <span data-ttu-id="90bb1-132">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90bb1-132">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
    2. <span data-ttu-id="90bb1-133">Klicka på **prenumeration** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-133">Click **Subscription** from the left menu.</span></span> <span data-ttu-id="90bb1-134">Den har en gul nyckelikonen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-134">It has a yellow key icon.</span></span> <span data-ttu-id="90bb1-135">Du bör se en lista över prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="90bb1-135">You shall see a list of subscriptions.</span></span>
    3. <span data-ttu-id="90bb1-136">Klicka på den prenumeration som du använder för att skapa kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-136">Click the subscription that you use to create clusters.</span></span> 
    4. <span data-ttu-id="90bb1-137">Klicka på **min behörighet**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-137">Click **My permissions**.</span></span>  <span data-ttu-id="90bb1-138">Det visar dina [rollen](../active-directory/role-based-access-control-what-is.md#built-in-roles) för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-138">It shows your [role](../active-directory/role-based-access-control-what-is.md#built-in-roles) on the subscription.</span></span> <span data-ttu-id="90bb1-139">Du behöver minst deltagare behörighet att skapa HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-139">You need at least Contributor access to create HDInsight cluster.</span></span>

<span data-ttu-id="90bb1-140">Om du får felet NoRegisteredProviderFound eller MissingSubscriptionRegistration felet, se [felsöka vanliga Azure-distribution med Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-140">If you receive the NoRegisteredProviderFound error or the MissingSubscriptionRegistration error, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).</span></span>

## <a name="list-and-show-clusters"></a><span data-ttu-id="90bb1-141">Listan och visa kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-141">List and show clusters</span></span>
1. <span data-ttu-id="90bb1-142">Logga in på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90bb1-142">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="90bb1-143">Klicka på **HDInsight-kluster** i den vänstra menyn för att visa befintliga kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-143">Click **HDInsight Clusters** from the left menu to list the existing clusters.</span></span>
3. <span data-ttu-id="90bb1-144">Klicka på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-144">Click the cluster name.</span></span> <span data-ttu-id="90bb1-145">Om klustret är långt, kan du använda filter överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="90bb1-145">If the cluster list is long, you can use filter on the top of the page.</span></span>
4. <span data-ttu-id="90bb1-146">Klicka på ett kluster från listan som ska visas på översiktssidan:</span><span class="sxs-lookup"><span data-stu-id="90bb1-146">Click a cluster from the list to see the overview page:</span></span>

    ![Azure portal HDInsight-kluster essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * <span data-ttu-id="90bb1-148">**Instrumentpanelen**: öppnar instrumentpanelen i klustret som är Ambari Web för Linux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-148">**Dashboard**: Opens the cluster dashboard, which is Ambari Web for Linux-based clusters.</span></span>
    * <span data-ttu-id="90bb1-149">**Secure Shell**: Visar instruktionerna för att ansluta till klustret med SSH (Secure Shell)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="90bb1-149">**Secure Shell**: Shows the instructions to connect to the cluster using Secure Shell (SSH) connection.</span></span>
    * <span data-ttu-id="90bb1-150">**Skala klustret**: du kan ändra antalet arbetarnoder för det här klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-150">**Scale Cluster**: Allows you to change the number of worker nodes for this cluster.</span></span>
    * <span data-ttu-id="90bb1-151">**Ta bort**: tar bort klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-151">**Delete**: Deletes the cluster.</span></span>
    * <span data-ttu-id="90bb1-152">**Aktivitetsloggar**: visa och fråga aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="90bb1-152">**Activity logs**: Show and query activity logs.</span></span>
    * <span data-ttu-id="90bb1-153">**Åtkomstkontroll (IAM)**: Använd rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="90bb1-153">**Access control (IAM)**: Use role assignments.</span></span>  <span data-ttu-id="90bb1-154">Se [använda rolltilldelningar för att hantera åtkomst till resurserna i Azure-prenumeration](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-154">See [Use role assignments to manage access to your Azure subscription resources](../active-directory/role-based-access-control-configure.md).</span></span>
    * <span data-ttu-id="90bb1-155">**Taggar**: Om du vill ange nyckel/värde-par för att definiera en anpassad taxonomi av dina molntjänster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-155">**Tags**: Allows you to set key/value pairs to define a custom taxonomy of your cloud services.</span></span> <span data-ttu-id="90bb1-156">Du kan till exempel skapa en nyckel som heter **projekt**, och sedan använda ett värde som är gemensamma för alla tjänster som är associerad med ett visst projekt.</span><span class="sxs-lookup"><span data-stu-id="90bb1-156">For example, you may create a key named **project**, and then use a common value for all services associated with a specific project.</span></span>
    * <span data-ttu-id="90bb1-157">**Diagnostisera och lösa problem**: visa felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="90bb1-157">**Diagnose and solve problems**: Display troubleshooting information.</span></span>
    * <span data-ttu-id="90bb1-158">**Låser**: Lägg till lås för att förhindra att klustret som ändras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="90bb1-158">**Locks**: Add lock to prevent the cluster being modified or deleted.</span></span>
    * <span data-ttu-id="90bb1-159">**Automatiseringsskriptet**: visa och exportera Azure Resource Manager-mall för klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-159">**Automation script**: Display and export the Azure Resource Manager template for the cluster.</span></span> <span data-ttu-id="90bb1-160">Du kan för närvarande bara exportera beroende Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="90bb1-160">Currently, you can only export the dependent Azure storage account.</span></span> <span data-ttu-id="90bb1-161">Se [skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Azure Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-161">See [Create Linux-based Hadoop clusters in HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md).</span></span>
    * <span data-ttu-id="90bb1-162">**Snabbstart**: Visar information som hjälper dig att komma igång med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="90bb1-162">**Quick Start**:  Displays information that helps you get started using HDInsight.</span></span>
    * <span data-ttu-id="90bb1-163">**Verktyg för HDInsight**: hjälpinformation för HDInsight relaterade verktyg.</span><span class="sxs-lookup"><span data-stu-id="90bb1-163">**Tools for HDInsight**: Help information for HDInsight related tools.</span></span>
    * <span data-ttu-id="90bb1-164">**Klustret inloggningen**: Visa inloggningsinformation för klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-164">**Cluster Login**: Display the cluster login information.</span></span>
    * <span data-ttu-id="90bb1-165">**Prenumerationen Core användning**: Visa Använd och tillgänglig kärnor för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-165">**Subscription Core Usage**: Display the used and available cores for your subscription.</span></span>
    * <span data-ttu-id="90bb1-166">**Skala klustret**: öka och minska antalet arbetarnoder i klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-166">**Scale Cluster**: Increase and decrease the number of cluster worker nodes.</span></span> <span data-ttu-id="90bb1-167">Se[skala kluster](hdinsight-administer-use-management-portal.md#scale-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-167">See[Scale clusters](hdinsight-administer-use-management-portal.md#scale-clusters).</span></span>
    * <span data-ttu-id="90bb1-168">**Secure Shell**: Visar instruktionerna för att ansluta till klustret med SSH (Secure Shell)-anslutning.</span><span class="sxs-lookup"><span data-stu-id="90bb1-168">**Secure Shell**: Shows the instructions to connect to the cluster using Secure Shell (SSH) connection.</span></span> <span data-ttu-id="90bb1-169">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="90bb1-169">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
    * <span data-ttu-id="90bb1-170">**HDInsight-partnern**: Lägg till/ta bort aktuell HDInsight-Partner.</span><span class="sxs-lookup"><span data-stu-id="90bb1-170">**HDInsight Partner**: Add/remove the current HDInsight Partner.</span></span>
    * <span data-ttu-id="90bb1-171">**Externa Metastores**: Visa Hive och Oozie metastores.</span><span class="sxs-lookup"><span data-stu-id="90bb1-171">**External Metastores**: View the Hive and Oozie metastores.</span></span> <span data-ttu-id="90bb1-172">Metastores kan bara konfigureras när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="90bb1-172">The metastores can only be configured during the cluster creation process.</span></span> <span data-ttu-id="90bb1-173">Se [använda Hive/Oozie metastore](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).</span><span class="sxs-lookup"><span data-stu-id="90bb1-173">See [use Hive/Oozie metastore](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).</span></span>
    * <span data-ttu-id="90bb1-174">**Script åtgärder**: köra Bash-skript på klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-174">**Script Actions**: Run Bash scripts on the cluster.</span></span> <span data-ttu-id="90bb1-175">Se [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-175">See [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    * <span data-ttu-id="90bb1-176">**Program**: Lägg till/ta bort HDInsight-program.</span><span class="sxs-lookup"><span data-stu-id="90bb1-176">**Applications**: Add/remove HDInsight applications.</span></span>  <span data-ttu-id="90bb1-177">Se [installera anpassade HDInsight-program](hdinsight-apps-install-custom-applications.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-177">See [Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md).</span></span>
    * <span data-ttu-id="90bb1-178">**Egenskaper för**: Visa egenskaper för klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-178">**Properties**: View the cluster properties.</span></span>
    * <span data-ttu-id="90bb1-179">**Storage-konton**: Visa storage-konton och nycklarna.</span><span class="sxs-lookup"><span data-stu-id="90bb1-179">**Storage accounts**: View the storage accounts and the keys.</span></span> <span data-ttu-id="90bb1-180">Storage-konton har konfigurerats när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="90bb1-180">The storage accounts are configured during the cluster creation process.</span></span>
    * <span data-ttu-id="90bb1-181">**Kluster-AAD-identitet**:</span><span class="sxs-lookup"><span data-stu-id="90bb1-181">**Cluster AAD Identity**:</span></span>
    * <span data-ttu-id="90bb1-182">**Ny supportbegäran**: du kan skapa ett supportärende Microsoft support.</span><span class="sxs-lookup"><span data-stu-id="90bb1-182">**New support request**: Allows you to create a support ticket with Microsoft support.</span></span>
    
6. <span data-ttu-id="90bb1-183">Klicka på **egenskaper**:</span><span class="sxs-lookup"><span data-stu-id="90bb1-183">Click **Properties**:</span></span>

    <span data-ttu-id="90bb1-184">Egenskaperna är:</span><span class="sxs-lookup"><span data-stu-id="90bb1-184">The properties are:</span></span>

   * <span data-ttu-id="90bb1-185">**Hostname**: klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-185">**Hostname**: Cluster name.</span></span>
   * <span data-ttu-id="90bb1-186">**Kluster-URL: en**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-186">**Cluster URL**.</span></span> <span data-ttu-id="90bb1-187">URL till Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-187">The URL for the Ambari web interface.</span></span>
   * <span data-ttu-id="90bb1-188">**Status för**: inkludera avbröts, godkända ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, fungerar, kör, fel, ta bort, tas bort, för lång tid, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued ResizeQueued, ClusterCustomization</span><span class="sxs-lookup"><span data-stu-id="90bb1-188">**Status**: Include Aborted, Accepted, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Operational, Running, Error, Deleting, Deleted, Timedout, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization</span></span>
   * <span data-ttu-id="90bb1-189">**Region**: Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="90bb1-189">**Region**: Azure location.</span></span> <span data-ttu-id="90bb1-190">En lista över Azure platser som stöds finns i **Region** nedrullningsbar listruta i [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="90bb1-190">For a list of supported Azure locations, see the **Region** dropdown list box on [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>
   * <span data-ttu-id="90bb1-191">**Skapandedatum**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-191">**Date created**.</span></span>
   * <span data-ttu-id="90bb1-192">**Operativsystemet**: antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-192">**Operating system**: Either **Windows** or **Linux**.</span></span>
   * <span data-ttu-id="90bb1-193">**Typen**: Hadoop, HBase, Storm, Väck.</span><span class="sxs-lookup"><span data-stu-id="90bb1-193">**Type**: Hadoop, HBase, Storm, Spark.</span></span>
   * <span data-ttu-id="90bb1-194">**Version**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-194">**Version**.</span></span> <span data-ttu-id="90bb1-195">Se [HDInsight-versioner](hdinsight-component-versioning.md)</span><span class="sxs-lookup"><span data-stu-id="90bb1-195">See [HDInsight versions](hdinsight-component-versioning.md)</span></span>
   * <span data-ttu-id="90bb1-196">**Prenumerationen**: prenumerationsnamn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-196">**Subscription**: Subscription name.</span></span>
   * <span data-ttu-id="90bb1-197">**Standarddatakälla**: standardfilsystem för klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-197">**Default data source**: The default cluster file system.</span></span>
   * <span data-ttu-id="90bb1-198">**Worker noder storlek**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-198">**Worker nodes size**.</span></span>
   * <span data-ttu-id="90bb1-199">**Gå nodstorlek**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-199">**Head node size**.</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="90bb1-200">Ta bort kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-200">Delete clusters</span></span>
<span data-ttu-id="90bb1-201">Tar bort ett kluster tar inte bort standardkontot för lagring eller eventuella länkade lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="90bb1-201">Deleting a cluster does not delete the default storage account or any linked storage accounts.</span></span> <span data-ttu-id="90bb1-202">Du kan återskapa klustret med hjälp av samma lagringskonton och samma metastores.</span><span class="sxs-lookup"><span data-stu-id="90bb1-202">You can re-create the cluster by using the same storage accounts and the same metastores.</span></span> <span data-ttu-id="90bb1-203">Det rekommenderas att använda en ny standardbehållaren när du skapar klustret igen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-203">It is recommended to use a new default Blob container when you re-create the cluster.</span></span>

1. <span data-ttu-id="90bb1-204">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="90bb1-204">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="90bb1-205">Klicka på **HDInsight-kluster** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-205">Click **HDInsight Clusters** from the left menu.</span></span> <span data-ttu-id="90bb1-206">Om du inte ser **HDInsight-kluster**, klickar du på **fler tjänster** första.</span><span class="sxs-lookup"><span data-stu-id="90bb1-206">If you don't see **HDInsight Clusters**, click **More services** first.</span></span>
3. <span data-ttu-id="90bb1-207">Klicka på det kluster som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="90bb1-207">Click the cluster that you want to delete.</span></span>
4. <span data-ttu-id="90bb1-208">Klicka på **ta bort** på den översta menyn och följ sedan instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="90bb1-208">Click **Delete** from the top menu, and then follow the instructions.</span></span>

<span data-ttu-id="90bb1-209">Se även [paus/stängs av kluster](#pauseshut-down-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-209">See also [Pause/shut down clusters](#pauseshut-down-clusters).</span></span>

## <a name="add-additional-storage-accounts"></a><span data-ttu-id="90bb1-210">Lägga till fler lagringskonton</span><span class="sxs-lookup"><span data-stu-id="90bb1-210">Add additional storage accounts</span></span>

<span data-ttu-id="90bb1-211">Du kan lägga till ytterligare Azure Storage-konton och Azure Data Lake Store-konton när ett kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="90bb1-211">You can add additional Azure Storage accounts and Azure Data Lake Store accounts after a cluster is created.</span></span> <span data-ttu-id="90bb1-212">Mer information finns i [Add additional storage accounts to HDInsight](./hdinsight-hadoop-add-storage.md) (Lägga till fler lagringskonton till HDInsight).</span><span class="sxs-lookup"><span data-stu-id="90bb1-212">For more information, see [Add additional storage accounts to HDInsight](./hdinsight-hadoop-add-storage.md).</span></span>

## <a name="scale-clusters"></a><span data-ttu-id="90bb1-213">Skala kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-213">Scale clusters</span></span>
<span data-ttu-id="90bb1-214">Klustret skalning funktionen kan du ändra antalet arbetarnoder som används av ett kluster som körs i Azure HDInsight utan att behöva återskapa klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-214">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="90bb1-215">Endast kluster med HDInsight version 3.1.3 eller högre stöds.</span><span class="sxs-lookup"><span data-stu-id="90bb1-215">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="90bb1-216">Om du är osäker på vilken version av klustret kan kontrollera du egenskapssidan.</span><span class="sxs-lookup"><span data-stu-id="90bb1-216">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="90bb1-217">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-217">See [List and show clusters](#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="90bb1-218">Konsekvenser av att ändra antalet datanoder som för varje typ av kluster som stöds av HDInsight:</span><span class="sxs-lookup"><span data-stu-id="90bb1-218">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="90bb1-219">Hadoop</span><span class="sxs-lookup"><span data-stu-id="90bb1-219">Hadoop</span></span>

    <span data-ttu-id="90bb1-220">Sömlöst kan du öka antalet arbetarnoder i ett Hadoop-kluster som körs utan att påverka alla väntande eller körs jobb.</span><span class="sxs-lookup"><span data-stu-id="90bb1-220">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="90bb1-221">Också du kan skicka nya jobb medan åtgärden pågår.</span><span class="sxs-lookup"><span data-stu-id="90bb1-221">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="90bb1-222">Fel i en åtgärd för skalning hanteras korrekt så att klustret alltid kvar i ett fungerande tillstånd.</span><span class="sxs-lookup"><span data-stu-id="90bb1-222">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>

    <span data-ttu-id="90bb1-223">När ett Hadoop-kluster skalas ned genom att minska antalet datanoder som, en del av tjänsterna i klustret har startats om.</span><span class="sxs-lookup"><span data-stu-id="90bb1-223">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="90bb1-224">Detta medför alla körs och väntande jobb misslyckas vid skalning åtgärden slutfördes.</span><span class="sxs-lookup"><span data-stu-id="90bb1-224">This behavior causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="90bb1-225">Du kan dock skicka jobb när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="90bb1-225">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="90bb1-226">HBase</span><span class="sxs-lookup"><span data-stu-id="90bb1-226">HBase</span></span>

    <span data-ttu-id="90bb1-227">Du kan sömlöst Lägg till eller ta bort noder till HBase-kluster, medan den körs.</span><span class="sxs-lookup"><span data-stu-id="90bb1-227">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="90bb1-228">Regional servrar balanseras automatiskt inom några minuter för att slutföra åtgärden. skalning.</span><span class="sxs-lookup"><span data-stu-id="90bb1-228">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="90bb1-229">Du kan också manuellt balansera regionala servrar genom att logga in på headnode i klustret och köra följande kommandon från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="90bb1-229">However, you can also manually balance the regional servers by logging in to the headnode of cluster and running the following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    <span data-ttu-id="90bb1-230">Mer information om hur du använder HBase-gränssnittet finns i]</span><span class="sxs-lookup"><span data-stu-id="90bb1-230">For more information on using the HBase shell, see []</span></span>
* <span data-ttu-id="90bb1-231">Storm</span><span class="sxs-lookup"><span data-stu-id="90bb1-231">Storm</span></span>

    <span data-ttu-id="90bb1-232">Du kan sömlöst lägga till eller ta bort datanoder till ditt Storm-kluster medan den körs.</span><span class="sxs-lookup"><span data-stu-id="90bb1-232">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="90bb1-233">Men efter en lyckad skalning åtgärden har slutförts, behöver du ombalansera topologin.</span><span class="sxs-lookup"><span data-stu-id="90bb1-233">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>

    <span data-ttu-id="90bb1-234">Ombalansering kan utföras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="90bb1-234">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="90bb1-235">Storm webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="90bb1-235">Storm web UI</span></span>
  * <span data-ttu-id="90bb1-236">Verktyget kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="90bb1-236">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="90bb1-237">Referera till den [Apache Storm dokumentationen](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) för mer information.</span><span class="sxs-lookup"><span data-stu-id="90bb1-237">Refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="90bb1-238">Storm webbgränssnittet är tillgänglig på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="90bb1-238">The Storm web UI is available on the HDInsight cluster:</span></span>

    ![HDInsight Storm skala balansera](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    <span data-ttu-id="90bb1-240">Här är ett exempel så använder du kommandot CLI för att balansera Storm-topologi:</span><span class="sxs-lookup"><span data-stu-id="90bb1-240">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="90bb1-241">**Att skala kluster**</span><span class="sxs-lookup"><span data-stu-id="90bb1-241">**To scale clusters**</span></span>

1. <span data-ttu-id="90bb1-242">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="90bb1-242">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="90bb1-243">Klicka på **HDInsight-kluster** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-243">Click **HDInsight Clusters** from the left menu.</span></span>
3. <span data-ttu-id="90bb1-244">Klicka på det kluster som du vill skala.</span><span class="sxs-lookup"><span data-stu-id="90bb1-244">Click the cluster you want to scale.</span></span>
3. <span data-ttu-id="90bb1-245">Klicka på **skala klustret**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-245">Click **Scale Cluster**.</span></span>
4. <span data-ttu-id="90bb1-246">Ange **numret för arbetarnoder**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-246">Enter **Number of Worker nodes**.</span></span> <span data-ttu-id="90bb1-247">Gränsen för antalet klusternoder varierar mellan olika Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="90bb1-247">The limit on the number of cluster nodes varies among Azure subscriptions.</span></span> <span data-ttu-id="90bb1-248">Du kan kontakta faktureringssupporten om du vill öka gränsvärdet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-248">You can contact billing support to increase the limit.</span></span>  <span data-ttu-id="90bb1-249">Information om kostnaden avspeglar ändringar i antalet noder.</span><span class="sxs-lookup"><span data-stu-id="90bb1-249">The cost information reflects the changes you have made to the number of nodes.</span></span>

    ![HDInsight hadoop hbase storm spark skala](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a><span data-ttu-id="90bb1-251">Pausa/stängs av kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-251">Pause/shut down clusters</span></span>

<span data-ttu-id="90bb1-252">De flesta Hadoop-jobb är batchjobb som endast körs ibland.</span><span class="sxs-lookup"><span data-stu-id="90bb1-252">Most of Hadoop jobs are batch jobs that are only run occasionally.</span></span> <span data-ttu-id="90bb1-253">För de flesta Hadoop-kluster finns stora tidsperioder som klustret inte används för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="90bb1-253">For most Hadoop clusters, there are large periods of time that the cluster is not being used for processing.</span></span> <span data-ttu-id="90bb1-254">Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används.</span><span class="sxs-lookup"><span data-stu-id="90bb1-254">With HDInsight, your data is stored in Azure Storage, so you can safely delete a cluster when it is not in use.</span></span>
<span data-ttu-id="90bb1-255">Du debiteras också för ett HDInsight-kluster, även när det inte används.</span><span class="sxs-lookup"><span data-stu-id="90bb1-255">You are also charged for an HDInsight cluster, even when it is not in use.</span></span> <span data-ttu-id="90bb1-256">Eftersom avgifterna för klustret är flera gånger större än avgifterna för lagring är det ekonomiskt sett bra att ta bort kluster när de inte används.</span><span class="sxs-lookup"><span data-stu-id="90bb1-256">Since the charges for the cluster are many times more than the charges for storage, it makes economic sense to delete clusters when they are not in use.</span></span>

<span data-ttu-id="90bb1-257">Det finns många sätt kan du programmerar processen:</span><span class="sxs-lookup"><span data-stu-id="90bb1-257">There are many ways you can program the process:</span></span>

* <span data-ttu-id="90bb1-258">Användaren Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="90bb1-258">User Azure Data Factory.</span></span> <span data-ttu-id="90bb1-259">Se [skapa på begäran Linux-baserade Hadoop-kluster i HDInsight med hjälp av Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) länkade tjänster för att skapa HDInsight på begäran.</span><span class="sxs-lookup"><span data-stu-id="90bb1-259">See [Create on-demand Linux-based Hadoop clusters in HDInsight using Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) for creating on-demand HDInsight linked services.</span></span>
* <span data-ttu-id="90bb1-260">Använda Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="90bb1-260">Use Azure PowerShell.</span></span>  <span data-ttu-id="90bb1-261">Se [analysera svarta fördröjning](hdinsight-analyze-flight-delay-data.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-261">See [Analyze flight delay data](hdinsight-analyze-flight-delay-data.md).</span></span>
* <span data-ttu-id="90bb1-262">Använda Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="90bb1-262">Use Azure CLI.</span></span> <span data-ttu-id="90bb1-263">Se [hantera HDInsight-kluster med hjälp av Azure CLI](hdinsight-administer-use-command-line.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-263">See [Manage HDInsight clusters using Azure CLI](hdinsight-administer-use-command-line.md).</span></span>
* <span data-ttu-id="90bb1-264">Använda HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="90bb1-264">Use HDInsight .NET SDK.</span></span> <span data-ttu-id="90bb1-265">Se [skicka Hadoop-jobb](hdinsight-submit-hadoop-jobs-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-265">See [Submit Hadoop jobs](hdinsight-submit-hadoop-jobs-programmatically.md).</span></span>

<span data-ttu-id="90bb1-266">Prisinformation, se [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="90bb1-266">For the pricing information, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span> <span data-ttu-id="90bb1-267">Om du vill ta bort ett kluster från portalen finns [ta bort kluster](#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="90bb1-267">To delete a cluster from the Portal, see [Delete clusters](#delete-clusters)</span></span>


## <a name="upgrade-clusters"></a><span data-ttu-id="90bb1-268">Uppgradera kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-268">Upgrade clusters</span></span>

<span data-ttu-id="90bb1-269">Se [uppgradera HDInsight-kluster till en nyare version](./hdinsight-upgrade-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-269">See [Upgrade HDInsight cluster to a newer version](./hdinsight-upgrade-cluster.md).</span></span>

## <a name="change-passwords"></a><span data-ttu-id="90bb1-270">Ändra lösenord</span><span class="sxs-lookup"><span data-stu-id="90bb1-270">Change passwords</span></span>
<span data-ttu-id="90bb1-271">Ett HDInsight-kluster kan ha två användarkonton.</span><span class="sxs-lookup"><span data-stu-id="90bb1-271">An HDInsight cluster can have two user accounts.</span></span> <span data-ttu-id="90bb1-272">HDInsight-kluster användarkonto (kallas även</span><span class="sxs-lookup"><span data-stu-id="90bb1-272">The HDInsight cluster user account (A.K.A.</span></span> <span data-ttu-id="90bb1-273">HTTP-användarkontot) och SSH-användarkontot skapas under skapandeprocessen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-273">HTTP user account) and the SSH user account are created during the creation process.</span></span> <span data-ttu-id="90bb1-274">Du kan använda Ambari-webbgränssnittet för att ändra klustret användarens användarnamn och lösenord och skriptåtgärder ändra SSH-användarkontot</span><span class="sxs-lookup"><span data-stu-id="90bb1-274">You can use the Ambari web UI to change the cluster user account username and password, and script actions to change the SSH user account</span></span>

### <a name="change-the-cluster-user-password"></a><span data-ttu-id="90bb1-275">Ändra användarlösenord kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-275">Change the cluster user password</span></span>
<span data-ttu-id="90bb1-276">Du kan använda Ambari-Webbgränssnittet för att ändra användarlösenord klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-276">You can use the Ambari Web UI to change the Cluster user password.</span></span> <span data-ttu-id="90bb1-277">Du måste använda det befintliga klustret användarnamn och lösenord för att logga in till Ambari.</span><span class="sxs-lookup"><span data-stu-id="90bb1-277">To log in to Ambari, you must use the existing cluster username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="90bb1-278">Ändra kluster-användarlösenord (admin) kan det leda till att skriptet åtgärder har körts för det här klustret slutar fungera.</span><span class="sxs-lookup"><span data-stu-id="90bb1-278">Changing the cluster user (admin) password may cause script actions ran against this cluster to fail.</span></span> <span data-ttu-id="90bb1-279">Om du har några beständiga skriptåtgärder arbetsnoder som mål, misslyckas dessa skript när du lägger till noder i klustret via ändra storlek på åtgärder.</span><span class="sxs-lookup"><span data-stu-id="90bb1-279">If you have any persisted script actions that target worker nodes, these scripts may fail when you add nodes to the cluster through resize operations.</span></span> <span data-ttu-id="90bb1-280">Mer information om skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="90bb1-280">For more information on script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
>
>

1. <span data-ttu-id="90bb1-281">Logga in på Ambari-Webbgränssnittet användarens autentiseringsuppgifter för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-281">Sign in to the Ambari Web UI using the HDInsight cluster user credentials.</span></span> <span data-ttu-id="90bb1-282">Standardanvändarnamnet är **admin**. URL-Adressen är **https://&lt;HDInsight-klustrets namn > azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-282">The default username is **admin**. The URL is **https://&lt;HDInsight Cluster Name>azurehdinsight.net**.</span></span>
2. <span data-ttu-id="90bb1-283">Klicka på **Admin** på den översta menyn och klicka sedan på ”Hantera Ambari”.</span><span class="sxs-lookup"><span data-stu-id="90bb1-283">Click **Admin** from the top menu, and then click "Manage Ambari".</span></span>
3. <span data-ttu-id="90bb1-284">I den vänstra menyn klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-284">From the left menu, click **Users**.</span></span>
4. <span data-ttu-id="90bb1-285">Klicka på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-285">Click **Admin**.</span></span>
5. <span data-ttu-id="90bb1-286">Klicka på **ändra lösenord**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-286">Click **Change Password**.</span></span>

<span data-ttu-id="90bb1-287">Ambari och ändrar lösenordet på alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-287">Ambari then changes the password on all nodes in the cluster.</span></span>

### <a name="change-the-ssh-user-password"></a><span data-ttu-id="90bb1-288">Ändra lösenord för SSH-användare</span><span class="sxs-lookup"><span data-stu-id="90bb1-288">Change the SSH user password</span></span>
1. <span data-ttu-id="90bb1-289">Med hjälp av en textredigerare, spara följande text som en fil med namnet **changepassword.sh**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-289">Using a text editor, save the following text as a file named **changepassword.sh**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="90bb1-290">Du måste använda ett redigeringsprogram som använder LF som rad avslutas.</span><span class="sxs-lookup"><span data-stu-id="90bb1-290">You must use an editor that uses LF as the line ending.</span></span> <span data-ttu-id="90bb1-291">Om CRLF används, fungerar inte skriptet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-291">If the editor uses CRLF, then the script does not work.</span></span>
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. <span data-ttu-id="90bb1-292">Överföra filen till en lagringsplats som kan nås från HDInsight med hjälp av en HTTP- eller HTTPS-adress.</span><span class="sxs-lookup"><span data-stu-id="90bb1-292">Upload the file to a storage location that can be accessed from HDInsight using an HTTP or HTTPS address.</span></span> <span data-ttu-id="90bb1-293">Till exempel lagra en gemensam fil, t.ex OneDrive eller Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="90bb1-293">For example, a public file store such as OneDrive or Azure Blob storage.</span></span> <span data-ttu-id="90bb1-294">Spara URI: N (HTTP eller HTTPS-adress) i filen eftersom den här URI krävs i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="90bb1-294">Save the URI (HTTP or HTTPS address) to the file, as this URI is needed in the next step.</span></span>
3. <span data-ttu-id="90bb1-295">Azure-portalen klickar du på **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-295">From the Azure portal, click **HDInsight Clusters**.</span></span>
4. <span data-ttu-id="90bb1-296">Klicka på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-296">Click your HDInsight cluster.</span></span>
4. <span data-ttu-id="90bb1-297">Klicka på **skript åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-297">Click **Script Actions**.</span></span>
4. <span data-ttu-id="90bb1-298">Från den **skriptåtgärder** bladet väljer **skicka nya**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-298">From the **Script Actions** blade, select **Submit New**.</span></span> <span data-ttu-id="90bb1-299">När den **skicka skriptåtgärden** bladet visas anger du följande information:</span><span class="sxs-lookup"><span data-stu-id="90bb1-299">When the **Submit script action** blade appears, enter the following information:</span></span>

   | <span data-ttu-id="90bb1-300">Fält</span><span class="sxs-lookup"><span data-stu-id="90bb1-300">Field</span></span> | <span data-ttu-id="90bb1-301">Värde</span><span class="sxs-lookup"><span data-stu-id="90bb1-301">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="90bb1-302">Namn</span><span class="sxs-lookup"><span data-stu-id="90bb1-302">Name</span></span> |<span data-ttu-id="90bb1-303">Ändra ssh lösenord</span><span class="sxs-lookup"><span data-stu-id="90bb1-303">Change ssh password</span></span> |
   | <span data-ttu-id="90bb1-304">Bash-skript URI</span><span class="sxs-lookup"><span data-stu-id="90bb1-304">Bash script URI</span></span> |<span data-ttu-id="90bb1-305">URI: N till filen changepassword.sh</span><span class="sxs-lookup"><span data-stu-id="90bb1-305">The URI to the changepassword.sh file</span></span> |
   | <span data-ttu-id="90bb1-306">Noder (Head, Worker, Nimbus, chef, Zookeeper osv.)</span><span class="sxs-lookup"><span data-stu-id="90bb1-306">Nodes (Head, Worker, Nimbus, Supervisor, Zookeeper, etc.)</span></span> |<span data-ttu-id="90bb1-307">✓ för alla nodtyper som anges</span><span class="sxs-lookup"><span data-stu-id="90bb1-307">✓ for all node types listed</span></span> |
   | <span data-ttu-id="90bb1-308">Parametrar</span><span class="sxs-lookup"><span data-stu-id="90bb1-308">Parameters</span></span> |<span data-ttu-id="90bb1-309">Ange SSH-användarnamn och det nya lösenordet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-309">Enter the SSH user name and then the new password.</span></span> <span data-ttu-id="90bb1-310">Det bör finnas ett blanksteg mellan användarnamnet och lösenordet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-310">There should be one space between the user name and the password.</span></span> |
   | <span data-ttu-id="90bb1-311">Spara den här skriptåtgärden...</span><span class="sxs-lookup"><span data-stu-id="90bb1-311">Persist this script action ...</span></span> |<span data-ttu-id="90bb1-312">Lämna det här fältet är avmarkerat.</span><span class="sxs-lookup"><span data-stu-id="90bb1-312">Leave this field unchecked.</span></span> |
5. <span data-ttu-id="90bb1-313">Välj **skapa** tillämpa skriptet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-313">Select **Create** to apply the script.</span></span> <span data-ttu-id="90bb1-314">När skriptet har slutförts, är du kunna ansluta till klustret med SSH med det nya lösenordet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-314">Once the script finishes, you are able to connect to the cluster using SSH with the new password.</span></span>

## <a name="grantrevoke-access"></a><span data-ttu-id="90bb1-315">Bevilja/återkalla åtkomst</span><span class="sxs-lookup"><span data-stu-id="90bb1-315">Grant/revoke access</span></span>
<span data-ttu-id="90bb1-316">HDInsight-kluster har följande HTTP-webbtjänster (alla dessa tjänster har RESTful slutpunkter):</span><span class="sxs-lookup"><span data-stu-id="90bb1-316">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="90bb1-317">ODBC</span><span class="sxs-lookup"><span data-stu-id="90bb1-317">ODBC</span></span>
* <span data-ttu-id="90bb1-318">JDBC</span><span class="sxs-lookup"><span data-stu-id="90bb1-318">JDBC</span></span>
* <span data-ttu-id="90bb1-319">Ambari</span><span class="sxs-lookup"><span data-stu-id="90bb1-319">Ambari</span></span>
* <span data-ttu-id="90bb1-320">Oozie</span><span class="sxs-lookup"><span data-stu-id="90bb1-320">Oozie</span></span>
* <span data-ttu-id="90bb1-321">Templeton</span><span class="sxs-lookup"><span data-stu-id="90bb1-321">Templeton</span></span>

<span data-ttu-id="90bb1-322">Som standard beviljas dessa tjänster för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="90bb1-322">By default, these services are granted for access.</span></span> <span data-ttu-id="90bb1-323">Du kan återkalla/bevilja åtkomst med hjälp av [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) och [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).</span><span class="sxs-lookup"><span data-stu-id="90bb1-323">You can revoke/grant the access using [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) and [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).</span></span>

## <a name="find-the-subscription-id"></a><span data-ttu-id="90bb1-324">Hitta prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="90bb1-324">Find the subscription ID</span></span>

<span data-ttu-id="90bb1-325">**Hitta din Azure-prenumeration ID: N**</span><span class="sxs-lookup"><span data-stu-id="90bb1-325">**To find your Azure subscription IDs**</span></span>

1. <span data-ttu-id="90bb1-326">Logga in på den [Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="90bb1-326">Sign in to the [Portal][azure-portal].</span></span>
2. <span data-ttu-id="90bb1-327">Klicka på **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-327">Click **Subscriptions**.</span></span> <span data-ttu-id="90bb1-328">Varje prenumeration har ett namn och ett ID.</span><span class="sxs-lookup"><span data-stu-id="90bb1-328">Each subscription has a name and an ID.</span></span>

<span data-ttu-id="90bb1-329">Varje kluster är kopplad till en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="90bb1-329">Each cluster is tied to an Azure subscription.</span></span> <span data-ttu-id="90bb1-330">Prenumerationen ID visas i klustret **väsentliga** panelen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-330">The subscription ID is shown on the cluster **Essential** tile.</span></span> <span data-ttu-id="90bb1-331">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-331">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="find-the-resource-group"></a><span data-ttu-id="90bb1-332">Hitta resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-332">Find the resource group</span></span>
<span data-ttu-id="90bb1-333">I läget Azure Resource Manager skapas varje HDInsight-kluster med en Azure Resource Manager-grupp.</span><span class="sxs-lookup"><span data-stu-id="90bb1-333">In the Azure Resource Manager mode, each HDInsight cluster is created with an Azure Resource Manager group.</span></span> <span data-ttu-id="90bb1-334">Resource Manager-gruppen som tillhör ett kluster visas i:</span><span class="sxs-lookup"><span data-stu-id="90bb1-334">The Resource Manager group that a cluster belongs to appears in:</span></span>

* <span data-ttu-id="90bb1-335">Listan över kluster har en **resursgruppen** kolumn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-335">The cluster list has a **Resource Group** column.</span></span>
* <span data-ttu-id="90bb1-336">Klustret **väsentliga** panelen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-336">Cluster **Essential** tile.</span></span>  

<span data-ttu-id="90bb1-337">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-337">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="find-the-default-storage-account"></a><span data-ttu-id="90bb1-338">Hitta standardkontot för lagring</span><span class="sxs-lookup"><span data-stu-id="90bb1-338">Find the default storage account</span></span>
<span data-ttu-id="90bb1-339">Varje HDInsight-kluster har en standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="90bb1-339">Each HDInsight cluster has a default storage account.</span></span> <span data-ttu-id="90bb1-340">Standardkontot för lagring och dess nycklar för ett kluster visas under **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-340">The default storage account and its keys for a cluster appears under **Storage Accounts**.</span></span> <span data-ttu-id="90bb1-341">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-341">See [List and show clusters](#list-and-show-clusters).</span></span>

## <a name="run-hive-queries"></a><span data-ttu-id="90bb1-342">Köra Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="90bb1-342">Run Hive queries</span></span>
<span data-ttu-id="90bb1-343">Du kan inte köra Hive-jobb direkt från Azure-portalen, men du kan använda Hive-vy på Ambari-Webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-343">You cannot run Hive job directly from the Azure portal, but you can use the Hive View on Ambari Web UI.</span></span>

<span data-ttu-id="90bb1-344">**Köra Hive-frågor med Ambari Hive-vy**</span><span class="sxs-lookup"><span data-stu-id="90bb1-344">**To run Hive queries using Ambari Hive View**</span></span>

1. <span data-ttu-id="90bb1-345">Logga in på Ambari-Webbgränssnittet användarens autentiseringsuppgifter för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-345">Sign in to the Ambari Web UI using the HDInsight cluster user credentials.</span></span> <span data-ttu-id="90bb1-346">Standardanvändarnamnet är **admin**. URL-Adressen är **https://&lt;HDInsight-klustrets namn > azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-346">The default username is **admin**. The URL is **https://&lt;HDInsight Cluster Name>azurehdinsight.net**.</span></span>
2. <span data-ttu-id="90bb1-347">Öppna Hive-vy som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="90bb1-347">Open Hive View as shown in the following screenshot:</span></span>  

    ![HDInsight hive-vyn](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. <span data-ttu-id="90bb1-349">Klicka på **frågan** på den översta menyn.</span><span class="sxs-lookup"><span data-stu-id="90bb1-349">Click **Query** from the top menu.</span></span>
4. <span data-ttu-id="90bb1-350">Ange en Hive-fråga i **frågeredigeraren**, och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="90bb1-350">Enter a Hive query in **Query Editor**, and then click **Execute**.</span></span>

## <a name="monitor-jobs"></a><span data-ttu-id="90bb1-351">Övervaka jobb</span><span class="sxs-lookup"><span data-stu-id="90bb1-351">Monitor jobs</span></span>
<span data-ttu-id="90bb1-352">Se [hantera HDInsight-kluster med Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md#monitoring).</span><span class="sxs-lookup"><span data-stu-id="90bb1-352">See [Manage HDInsight clusters by using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md#monitoring).</span></span>

## <a name="browse-files"></a><span data-ttu-id="90bb1-353">Bläddra bland filer</span><span class="sxs-lookup"><span data-stu-id="90bb1-353">Browse files</span></span>
<span data-ttu-id="90bb1-354">Du kan bläddra innehållet i standardbehållaren med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-354">Using the Azure portal, you can browse the content of the default container.</span></span>

1. <span data-ttu-id="90bb1-355">Logga in på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90bb1-355">Sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="90bb1-356">Klicka på **HDInsight-kluster** i den vänstra menyn för att visa befintliga kluster.</span><span class="sxs-lookup"><span data-stu-id="90bb1-356">Click **HDInsight Clusters** from the left menu to list the existing clusters.</span></span>
3. <span data-ttu-id="90bb1-357">Klicka på klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="90bb1-357">Click the cluster name.</span></span> <span data-ttu-id="90bb1-358">Om klustret är långt, kan du använda filter överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="90bb1-358">If the cluster list is long, you can use filter on the top of the page.</span></span>
4. <span data-ttu-id="90bb1-359">Klicka på **Lagringskonton** i den vänstra menyn i klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-359">Click **Storage Accounts** from the cluster left menu.</span></span>
5. <span data-ttu-id="90bb1-360">Klicka på ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="90bb1-360">Click a Storage account.</span></span>
7. <span data-ttu-id="90bb1-361">Klicka på den **Blobbar** panelen.</span><span class="sxs-lookup"><span data-stu-id="90bb1-361">Click the **Blobs** tile.</span></span>
8. <span data-ttu-id="90bb1-362">Klicka på standardnamnet för behållaren.</span><span class="sxs-lookup"><span data-stu-id="90bb1-362">Click the default container name.</span></span>

## <a name="monitor-cluster-usage"></a><span data-ttu-id="90bb1-363">Övervaka kluster användning</span><span class="sxs-lookup"><span data-stu-id="90bb1-363">Monitor cluster usage</span></span>
<span data-ttu-id="90bb1-364">Den **användning** på HDInsight-klusterbladet visar information om antal kärnor som är tillgängliga för din prenumeration för användning med HDInsight, samt antal kärnor som allokerats till detta kluster och hur de är allokerade för alla noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="90bb1-364">The **Usage** section of the HDInsight cluster blade displays information about the number of cores available to your subscription for use with HDInsight, as well as the number of cores allocated to this cluster and how they are allocated for the nodes within this cluster.</span></span> <span data-ttu-id="90bb1-365">Se [listan och visa](#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="90bb1-365">See [List and show clusters](#list-and-show-clusters).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90bb1-366">För att övervaka tjänster som tillhandahålls av HDInsight-kluster, måste du använda Ambari Web eller Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="90bb1-366">To monitor the services provided by the HDInsight cluster, you must use Ambari Web or the Ambari REST API.</span></span> <span data-ttu-id="90bb1-367">Mer information om hur du använder Ambari finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="90bb1-367">For more information on using Ambari, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>
>
>

## <a name="connect-to-a-cluster"></a><span data-ttu-id="90bb1-368">Ansluta till ett kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-368">Connect to a cluster</span></span>

* [<span data-ttu-id="90bb1-369">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="90bb1-369">Use Hive with HDInsight</span></span>](hdinsight-hadoop-use-hive-ambari-view.md)
* [<span data-ttu-id="90bb1-370">Använda SSH med HDInsight</span><span class="sxs-lookup"><span data-stu-id="90bb1-370">Use SSH with HDInsight</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a><span data-ttu-id="90bb1-371">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="90bb1-371">Next steps</span></span>
<span data-ttu-id="90bb1-372">I den här artikeln har du lärt dig vissa grundläggande administative funktioner.</span><span class="sxs-lookup"><span data-stu-id="90bb1-372">In this article, you have learned some basic administative functions.</span></span> <span data-ttu-id="90bb1-373">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="90bb1-373">To learn more, see the following articles:</span></span>

* [<span data-ttu-id="90bb1-374">Administrera HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="90bb1-374">Administer HDInsight Using Azure PowerShell</span></span>](hdinsight-administer-use-powershell.md)
* [<span data-ttu-id="90bb1-375">Administrera HDInsight med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="90bb1-375">Administer HDInsight Using Azure CLI</span></span>](hdinsight-administer-use-command-line.md)
* [<span data-ttu-id="90bb1-376">Skapa HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="90bb1-376">Create HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="90bb1-377">Använda Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="90bb1-377">Use Hive in HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="90bb1-378">Använda Pig i HDInsight</span><span class="sxs-lookup"><span data-stu-id="90bb1-378">Use Pig in HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="90bb1-379">Använda Sqoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="90bb1-379">Use Sqoop in HDInsight</span></span>](hdinsight-use-sqoop.md)
* [<span data-ttu-id="90bb1-380">Komma igång med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="90bb1-380">Get Started with Azure HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="90bb1-381">Vilken version av Hadoop finns i Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="90bb1-381">What version of Hadoop is in Azure HDInsight?</span></span>](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop-kommandorad"
