---
title: "aaaManage storage-konton med hjälp av hello Azure Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig hur toomanage ditt Azure storage-konton med hjälp av hello Azure Explorer för IntelliJ."
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
ms.openlocfilehash: b094bdcd2fa2782cb12b67c96ac406fbe4c1aa3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-storage-accounts-by-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="ea106-103">Hantera storage-konton med hjälp av hello Azure Explorer för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="ea106-103">Manage storage accounts by using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="ea106-104">hello Azure Explorer, som är en del av hello Azure Toolkit för IntelliJ ger Java-utvecklare med en enkel att använda lösning för hantering av lagringskonton i Azure kontot från inuti hello IntelliJ integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="ea106-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing storage accounts in their Azure account from inside hello IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-intellij"></a><span data-ttu-id="ea106-105">Skapa ett lagringskonto i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="ea106-105">Create a storage account in IntelliJ</span></span>

<span data-ttu-id="ea106-106">toocreate ett lagringskonto med hjälp av hello Azure Explorer hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea106-106">toocreate a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="ea106-107">Logga in tooyour Azure-konto med hjälp av hello [inloggning anvisningar hello Azure Toolkit för Eclipse].</span><span class="sxs-lookup"><span data-stu-id="ea106-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span> 

2. <span data-ttu-id="ea106-108">I hello **Azure Explorer** , expanderar hello **Azure** noden, högerklickar du på **Lagringskonton**, och klicka sedan på **skapa Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="ea106-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Storage Accounts**, and then click **Create Storage Account**.</span></span>

   ![Skapa Lagringskontot kommando][CS01]

3. <span data-ttu-id="ea106-110">I hello **skapa Lagringskonto** dialogrutan Ange hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="ea106-110">In hello **Create Storage Account** dialog box, specify hello following options:</span></span>

   ![Skapa nytt Lagringskonto dialogruta][CS02]

   * <span data-ttu-id="ea106-112">**Namnet**: Anger hello namn för hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ea106-112">**Name**: Specifies hello name for hello new storage account.</span></span>

   * <span data-ttu-id="ea106-113">**Kontot kind**: Anger hello storage-konto toocreate (till exempel ”Blob storage”).</span><span class="sxs-lookup"><span data-stu-id="ea106-113">**Account kind**: Specifies hello type of storage account toocreate (for example, "Blob storage").</span></span> <span data-ttu-id="ea106-114">Mer information finns i [om Azure storage-konton].</span><span class="sxs-lookup"><span data-stu-id="ea106-114">For more information, see [About Azure storage accounts].</span></span> 

   * <span data-ttu-id="ea106-115">**Prestanda**: Anger vilken lagringskontotyp erbjuder toouse från hello valda utgivare (till exempel ”bidrag”).</span><span class="sxs-lookup"><span data-stu-id="ea106-115">**Performance**: Specifies which storage account offering toouse from hello selected publisher (for example, "Premium").</span></span> <span data-ttu-id="ea106-116">Mer information finns i [skalbarhets- och prestandamål för Azure storage].</span><span class="sxs-lookup"><span data-stu-id="ea106-116">For more information, see [Azure storage scalability and performance targets].</span></span> 

   * <span data-ttu-id="ea106-117">**Replikering**: Anger hello replikering för hello storage-konto (till exempel ”Zonredundant”).</span><span class="sxs-lookup"><span data-stu-id="ea106-117">**Replication**: Specifies hello replication for hello storage account (for example, "Zone-Redundant").</span></span> <span data-ttu-id="ea106-118">Mer information finns i [Azure storage-replikering].</span><span class="sxs-lookup"><span data-stu-id="ea106-118">For more information, see [Azure storage replication].</span></span> 

   * <span data-ttu-id="ea106-119">**Prenumerationen**: Anger hello Azure-prenumeration som du vill toouse för hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ea106-119">**Subscription**: Specifies hello Azure subscription that you want toouse for hello new storage account.</span></span>

   * <span data-ttu-id="ea106-120">**Plats**: Anger hello plats där ditt lagringskonto skapas (till exempel ”West US”).</span><span class="sxs-lookup"><span data-stu-id="ea106-120">**Location**: Specifies hello location where your storage account will be created (for example, "West US").</span></span>

   * <span data-ttu-id="ea106-121">**Resursgruppen**: Anger hello resursgruppen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ea106-121">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="ea106-122">Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="ea106-122">Select one of hello following options:</span></span>
      * <span data-ttu-id="ea106-123">**Skapa en ny**: Anger att du vill toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ea106-123">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="ea106-124">**Använd befintlig**: Anger att du väljer från en lista över resursgrupper som är associerade med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ea106-124">**Use existing**: Specifies that you will select from a list of resource groups that are associated with your Azure account.</span></span>

4. <span data-ttu-id="ea106-125">När du har angett alla hello föregående alternativ, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea106-125">When you have specified all of hello preceding options, click **OK**.</span></span>

## <a name="create-a-storage-container-in-intellij"></a><span data-ttu-id="ea106-126">Skapa en lagringsbehållare i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="ea106-126">Create a storage container in IntelliJ</span></span>

<span data-ttu-id="ea106-127">toocreate en lagringsbehållare med hjälp av hello Azure Explorer hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea106-127">toocreate a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="ea106-128">Högerklicka på hello storage-konto där du vill toocreate en behållare och klicka sedan på i hello Azure Utforskarvy **skapa blob-behållaren**.</span><span class="sxs-lookup"><span data-stu-id="ea106-128">In hello Azure Explorer view, right-click hello storage account where you want toocreate a container, and then click **Create blob container**.</span></span>

   ![Skapa blob-behållaren kommando][CC01]

2. <span data-ttu-id="ea106-130">I hello **skapa blob-behållaren** dialogrutan Ange hello namn för din behållaren och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea106-130">In hello **Create blob container** dialog box, specify hello name for your container, and then click **OK**.</span></span> <span data-ttu-id="ea106-131">Mer information om namngivning av behållare för lagring finns [namnge och referera till behållare, blobbar och metadata].</span><span class="sxs-lookup"><span data-stu-id="ea106-131">For more information about naming storage containers, see [Naming and referencing containers, blobs, and metadata].</span></span>

   ![Skapa lagring dialogruta för behållaren][CC02]

## <a name="delete-a-storage-container-in-intellij"></a><span data-ttu-id="ea106-133">Ta bort en lagringsbehållaren i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="ea106-133">Delete a storage container in IntelliJ</span></span>

<span data-ttu-id="ea106-134">toodelete en lagringsbehållare med hjälp av hello Azure Explorer hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea106-134">toodelete a storage container by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="ea106-135">Högerklicka på hello lagringsbehållaren i hello Azure Utforskarvy, och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ea106-135">In hello Azure Explorer view, right-click hello storage container, and then click **Delete**.</span></span>

   ![Ta bort lagring behållaren kommando][DC01]

2. <span data-ttu-id="ea106-137">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ea106-137">In hello confirmation window, click **Yes**.</span></span>

   ![Ta bort bekräftelsefönstret behållare för lagring][DC02]

## <a name="delete-a-storage-account-in-intellij"></a><span data-ttu-id="ea106-139">Ta bort ett lagringskonto i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="ea106-139">Delete a storage account in IntelliJ</span></span>

<span data-ttu-id="ea106-140">toodelete ett lagringskonto med hjälp av hello Azure Explorer hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea106-140">toodelete a storage account by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="ea106-141">I hello **Azure Explorer** visa, högerklicka på hello storage-konto och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ea106-141">In hello **Azure Explorer** view, right-click hello storage account, and then select **Delete**.</span></span>

   ![Ta bort storage-konto-menyn][DS01]

2. <span data-ttu-id="ea106-143">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ea106-143">In hello confirmation window, click **Yes**.</span></span>

   ![Ta bort bekräftelsefönstret för storage-konto][DS02]

## <a name="next-steps"></a><span data-ttu-id="ea106-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea106-145">Next steps</span></span>
<span data-ttu-id="ea106-146">Mer information om Azure storage-konton, storlek och prissättning finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="ea106-146">For more information about Azure storage accounts, sizes, and pricing, see hello following resources:</span></span>

* <span data-ttu-id="ea106-147">[Introduktion tooMicrosoft Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="ea106-147">[Introduction tooMicrosoft Azure Storage]</span></span>
* <span data-ttu-id="ea106-148">[om Azure storage-konton]</span><span class="sxs-lookup"><span data-stu-id="ea106-148">[About Azure storage accounts]</span></span>
* <span data-ttu-id="ea106-149">Azure lagringskonto storlekar</span><span class="sxs-lookup"><span data-stu-id="ea106-149">Azure storage-account sizes</span></span>
  * <span data-ttu-id="ea106-150">[Storlekar för Windows storage-konton i Azure]</span><span class="sxs-lookup"><span data-stu-id="ea106-150">[Sizes for Windows storage accounts in Azure]</span></span>
  * <span data-ttu-id="ea106-151">[Storlekar för Linux storage-konton i Azure]</span><span class="sxs-lookup"><span data-stu-id="ea106-151">[Sizes for Linux storage accounts in Azure]</span></span>
* <span data-ttu-id="ea106-152">Priser för Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="ea106-152">Azure storage-account pricing</span></span>
  * <span data-ttu-id="ea106-153">[Priser för Windows storage-konto]</span><span class="sxs-lookup"><span data-stu-id="ea106-153">[Windows storage-account pricing]</span></span>
  * <span data-ttu-id="ea106-154">[Priser för Linux-lagringskonto]</span><span class="sxs-lookup"><span data-stu-id="ea106-154">[Linux storage-account pricing]</span></span>

<span data-ttu-id="ea106-155">Mer information om Azure-verktyg för Java IDEs finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="ea106-155">For more information about Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="ea106-156">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ea106-156">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ea106-157">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ea106-157">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ea106-158">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ea106-158">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ea106-159">[inloggning anvisningar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ea106-159">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="ea106-160">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="ea106-160">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="ea106-161">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ea106-161">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ea106-162">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ea106-162">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ea106-163">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ea106-163">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ea106-164">[Logga in instruktioner för hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ea106-164">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="ea106-165">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="ea106-165">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="ea106-166">Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="ea106-166">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[inloggning anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Logga in instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

[Introduktion tooMicrosoft Azure Storage]: /azure/storage/storage-introduction
[om Azure storage-konton]: /azure/storage/storage-create-storage-account
[Azure storage-replikering]: /azure/storage/storage-redundancy
[Azure storage skalbarhets- och prestandamål]: /azure/storage/storage-scalability-targets
[namnge och referera till behållare, blobbar och metadata]: http://go.microsoft.com/fwlink/?LinkId=255555

[Storlekar för Windows storage-konton i Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Storlekar för Linux storage-konton i Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Priser för Windows storage-konto]: /pricing/details/virtual-machines/windows/
[Priser för Linux-lagringskonto]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png
