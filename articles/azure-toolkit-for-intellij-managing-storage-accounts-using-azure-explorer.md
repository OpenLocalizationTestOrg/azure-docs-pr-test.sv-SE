---
title: "Hantera storage-konton med hjälp av Azure-Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig hur du hanterar Azure storage-konton med hjälp av Azure-Explorer för IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: a1b56cc2751fc43a1ad6917eca77eec460f26694
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-intellij"></a><span data-ttu-id="43c59-103">Hantera storage-konton med hjälp av Azure-Explorer för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="43c59-103">Manage storage accounts by using the Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="43c59-104">Azure Explorer, som är en del av Azure-verktygen för IntelliJ ger Java-utvecklare med en enkel att använda lösning för hantering av lagringskonton i Azure kontot från inuti IntelliJ integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="43c59-104">The Azure Explorer, which is part of the Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside the IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="43c59-105">Skapa ett lagringskonto i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="43c59-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="43c59-106">Om du vill skapa ett lagringskonto med hjälp av Azure-Explorer, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="43c59-106">To create a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="43c59-107">Logga in på ditt Azure-konto med hjälp av den [inloggning instruktioner för Azure-verktygen för Eclipse].</span><span class="sxs-lookup"><span data-stu-id="43c59-107">Sign in to your Azure account by using the [Sign-in instructions for the Azure Toolkit for Eclipse].</span></span> 

2. <span data-ttu-id="43c59-108">I den **Azure Explorer** , expanderar den **Azure** noden, högerklickar du på **Lagringskonton**, och klicka sedan på **skapa Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="43c59-108">In the **Azure Explorer** view, expand the **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![Skapa Lagringskontot kommando][CS01]

3. <span data-ttu-id="43c59-110">I den **skapa Lagringskonto** dialogrutan anger du följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="43c59-110">In the **Create Storage Account** dialog box, specify the following options:</span></span>

   ![Skapa nytt Lagringskonto dialogruta][CS02]

   * <span data-ttu-id="43c59-112">**Namnet**: Anger namnet för det nya lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="43c59-112">**Name**: Specifies the name for the new storage account.</span></span>

   * <span data-ttu-id="43c59-113">**Kontot kind**: Anger vilken typ av lagringskonto för att skapa (till exempel ”Blob storage”).</span><span class="sxs-lookup"><span data-stu-id="43c59-113">**Account kind**: Specifies the type of storage account to create (for example, "Blob storage").</span></span> <span data-ttu-id="43c59-114">Mer information finns i [om Azure storage-konton].</span><span class="sxs-lookup"><span data-stu-id="43c59-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="43c59-115">**Prestanda**: Anger vilken lagringskontotyp erbjudande ska användas från den valda utgivaren (till exempel ”bidrag”).</span><span class="sxs-lookup"><span data-stu-id="43c59-115">**Performance**: Specifies which storage account offering to use from the selected publisher (for example, "Premium").</span></span> <span data-ttu-id="43c59-116">Mer information finns i [skalbarhets- och prestandamål för Azure storage].</span><span class="sxs-lookup"><span data-stu-id="43c59-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="43c59-117">**Replikering**: Anger replikering för storage-konto (till exempel ”Zonredundant”).</span><span class="sxs-lookup"><span data-stu-id="43c59-117">**Replication**: Specifies the replication for the storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="43c59-118">Mer information finns i [Azure storage-replikering].</span><span class="sxs-lookup"><span data-stu-id="43c59-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="43c59-119">**Prenumerationen**: Anger den Azure-prenumeration som du vill använda för det nya lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="43c59-119">**Subscription**: Specifies the Azure subscription that you want to use for the new storage account.</span></span>

   * <span data-ttu-id="43c59-120">**Plats**: Anger platsen där ditt lagringskonto skapas (till exempel ”West US”).</span><span class="sxs-lookup"><span data-stu-id="43c59-120">**Location**: Specifies the location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="43c59-121">**Resursgruppen**: Anger resursgruppen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="43c59-121">**Resource Group**: Specifies the resource group for your virtual machine.</span></span> <span data-ttu-id="43c59-122">Välj något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="43c59-122">Select one of the following options:</span></span>
      * <span data-ttu-id="43c59-123">**Skapa en ny**: Anger att du vill skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="43c59-123">**Create new**: Specifies that you want to create a new resource group.</span></span>
      * <span data-ttu-id="43c59-124">**Använd befintlig**: Anger att du väljer från en lista över resursgrupper som är associerade med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="43c59-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="43c59-125">När du har angett alla föregående alternativ, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43c59-125">When you have specified all of the preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="43c59-126">Skapa en lagringsbehållare i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="43c59-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="43c59-127">Om du vill skapa en lagringsbehållare med hjälp av Azure-Explorer, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="43c59-127">To create a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="43c59-128">I vyn Azure Explorer högerklickar du på det lagringskonto där du vill skapa en behållare och klicka sedan på **skapa blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="43c59-128">In the Azure Explorer view, right-click the storage account where you want to create a container, and then click **Create blob container**.</span></span>

   ![Skapa blob-behållaren kommando][CC01]

2. <span data-ttu-id="43c59-130">I den **skapa blob-behållaren** dialogrutan, ange namnet på din behållare klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43c59-130">In the **Create blob container** dialog box, specify the name for your container, and then click **OK**.</span></span> <span data-ttu-id="43c59-131">Mer information om namngivning av behållare för lagring finns [namnge och referera till behållare, blobbar och metadata].</span><span class="sxs-lookup"><span data-stu-id="43c59-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Skapa lagring dialogruta för behållaren][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="43c59-133">Ta bort en lagringsbehållaren i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="43c59-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="43c59-134">Ta bort en lagringsbehållare med hjälp av Azure-Explorer genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="43c59-134">To delete a storage container by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="43c59-135">Högerklicka på lagringsbehållare för i vyn Azure Explorer och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="43c59-135">In the Azure Explorer view, right-click the storage container, and then click **Delete**.</span></span>

   ![Ta bort lagring behållaren kommando][DC01]

2. <span data-ttu-id="43c59-137">Klicka på i bekräftelsefönstret **Ja**.</span><span class="sxs-lookup"><span data-stu-id="43c59-137">In the confirmation window, click **Yes**.</span></span>

   ![Ta bort bekräftelsefönstret behållare för lagring][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="43c59-139">Ta bort ett lagringskonto i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="43c59-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="43c59-140">Om du vill ta bort ett lagringskonto med hjälp av Azure-Explorer gör du följande:</span><span class="sxs-lookup"><span data-stu-id="43c59-140">To delete a storage account by using the Azure Explorer, do the following:</span></span>

1. <span data-ttu-id="43c59-141">I den **Azure Explorer** visa, högerklicka på lagringskontot och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="43c59-141">In the **Azure Explorer** view, right-click the storage account, and then select **Delete**.</span></span>

   ![Ta bort storage-konto-menyn][DS01]

2. <span data-ttu-id="43c59-143">Klicka på i bekräftelsefönstret **Ja**.</span><span class="sxs-lookup"><span data-stu-id="43c59-143">In the confirmation window, click **Yes**.</span></span>

   ![Ta bort bekräftelsefönstret för storage-konto][DS02]

## <a name="next-steps"></a><span data-ttu-id="43c59-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43c59-145">Next steps</span></span>
<span data-ttu-id="43c59-146">Mer information om Azure storage-konton finns storlek och prissättning, i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="43c59-146">For more information about Azure storage accounts, sizes, and pricing, see the following resources:</span></span>

* <span data-ttu-id="43c59-147">[Introduktion till Microsoft Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="43c59-147">[Introduction to Microsoft Azure Storage]</span></span>
* <span data-ttu-id="43c59-148">[om Azure storage-konton]</span><span class="sxs-lookup"><span data-stu-id="43c59-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="43c59-149">Azure lagringskonto storlekar</span><span class="sxs-lookup"><span data-stu-id="43c59-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="43c59-150">[Storlekar för Windows storage-konton i Azure]</span><span class="sxs-lookup"><span data-stu-id="43c59-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="43c59-151">[Storlekar för Linux storage-konton i Azure]</span><span class="sxs-lookup"><span data-stu-id="43c59-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="43c59-152">Priser för Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="43c59-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="43c59-153">[Priser för Windows storage-konto]</span><span class="sxs-lookup"><span data-stu-id="43c59-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="43c59-154">[Priser för Linux-lagringskonto]</span><span class="sxs-lookup"><span data-stu-id="43c59-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="43c59-155">Mer information om Azure-verktyg för Java IDEs finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="43c59-155">For more information about Azure Toolkits for Java IDEs, see the following resources:</span></span>

* <span data-ttu-id="43c59-156">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="43c59-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="43c59-157">[Vad är nytt i Azure-verktygen för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="43c59-157">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="43c59-158">[Installera Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="43c59-158">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="43c59-159">[inloggning instruktioner för Azure-verktygen för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="43c59-159">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="43c59-160">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="43c59-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="43c59-161">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="43c59-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="43c59-162">[Vad är nytt i Azure-verktygen för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="43c59-162">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="43c59-163">[Installera Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="43c59-163">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="43c59-164">[Logga in instruktioner för Azure-Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="43c59-164">[Sign-in instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="43c59-165">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="43c59-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="43c59-166">Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="43c59-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="43c59-167">[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="43c59-167">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="43c59-168">[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="43c59-168">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="43c59-169">[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="43c59-169">[Create a Hello World web app for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="43c59-170">[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="43c59-170">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="43c59-171">[Installera Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="43c59-171">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="43c59-172">[Installera Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="43c59-172">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="43c59-173">[inloggning instruktioner för Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="43c59-173">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="43c59-174">[Logga in instruktioner för Azure-Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="43c59-174">[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="43c59-175">[Vad är nytt i Azure-verktygen för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="43c59-175">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="43c59-176">[Vad är nytt i Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="43c59-176">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="43c59-177">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="43c59-177">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="43c59-178">[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="43c59-178">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

<span data-ttu-id="43c59-179">[Introduktion till Microsoft Azure Storage]: /azure/storage/storage-introduction</span><span class="sxs-lookup"><span data-stu-id="43c59-179">[Introduction to Microsoft Azure Storage]: /azure/storage/storage-introduction</span></span>
<span data-ttu-id="43c59-180">[om Azure storage-konton]: /azure/storage/storage-create-storage-account</span><span class="sxs-lookup"><span data-stu-id="43c59-180">[About Azure storage accounts]: /azure/storage/storage-create-storage-account</span></span>
<span data-ttu-id="43c59-181">[Azure storage-replikering]: /azure/storage/storage-redundancy</span><span class="sxs-lookup"><span data-stu-id="43c59-181">[Azure storage replication]: /azure/storage/storage-redundancy</span></span>
<span data-ttu-id="43c59-182">[Azure storage skalbarhets- och prestandamål]: /azure/storage/storage-scalability-targets</span><span class="sxs-lookup"><span data-stu-id="43c59-182">[Azure storage scalability and Performance Targets]: /azure/storage/storage-scalability-targets</span></span>
<span data-ttu-id="43c59-183">[namnge och referera till behållare, blobbar och metadata]: http://go.microsoft.com/fwlink/?LinkId=255555</span><span class="sxs-lookup"><span data-stu-id="43c59-183">[Naming and referencing containers, blobs, and metadata]: http://go.microsoft.com/fwlink/?LinkId=255555</span></span>

<span data-ttu-id="43c59-184">[Storlekar för Windows storage-konton i Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span><span class="sxs-lookup"><span data-stu-id="43c59-184">[Sizes for Windows storage accounts in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes</span></span>
<span data-ttu-id="43c59-185">[Storlekar för Linux storage-konton i Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span><span class="sxs-lookup"><span data-stu-id="43c59-185">[Sizes for Linux storage accounts in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes</span></span>
<span data-ttu-id="43c59-186">[Priser för Windows storage-konto]: /pricing/details/virtual-machines/windows/</span><span class="sxs-lookup"><span data-stu-id="43c59-186">[Windows storage-account pricing]: /pricing/details/virtual-machines/windows/</span></span>
<span data-ttu-id="43c59-187">[Priser för Linux-lagringskonto]: /pricing/details/virtual-machines/linux/</span><span class="sxs-lookup"><span data-stu-id="43c59-187">[Linux storage-account pricing]: /pricing/details/virtual-machines/linux/</span></span>

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
