---
title: "aaaManage virtuella datorer med hjälp av hello Azure Explorer för Eclipse | Microsoft Docs"
description: "Lär dig hur toomanage Azure virtuella datorer med hjälp av hello Azure Explorer för Eclipse."
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
ms.openlocfilehash: aa83ec7546a0a8514842723b51ce8a5af81f98f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-eclipse"></a><span data-ttu-id="1d43d-103">Hantera virtuella datorer med hjälp av hello Azure Explorer för Eclipse</span><span class="sxs-lookup"><span data-stu-id="1d43d-103">Manage virtual machines by using hello Azure Explorer for Eclipse</span></span>

<span data-ttu-id="1d43d-104">hello Azure Explorer, som är en del av hello Azure Toolkit för Eclipse ger Java-utvecklare med en enkel att använda lösning för hantering av virtuella datorer i Azure kontot från inuti hello Eclipse integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="1d43d-104">hello Azure Explorer, which is part of hello Azure Toolkit for Eclipse, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside hello Eclipse integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d43d-105">Skapa en virtuell dator i Eclipse</span><span class="sxs-lookup"><span data-stu-id="1d43d-105">Create a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d43d-106">toocreate en virtuell dator med hjälp av hello Azure Explorer hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d43d-106">toocreate a virtual machine by using hello Azure Explorer, do hello following:</span></span>

1. <span data-ttu-id="1d43d-107">Logga in tooyour Azure-konto med hjälp av hello [inloggning anvisningar hello Azure Toolkit för Eclipse].</span><span class="sxs-lookup"><span data-stu-id="1d43d-107">Sign in tooyour Azure account by using hello [Sign-in instructions for hello Azure Toolkit for Eclipse].</span></span>

2. <span data-ttu-id="1d43d-108">I hello **Azure Explorer** , expanderar hello **Azure** noden, högerklickar du på **virtuella datorer**, och klicka sedan på **skapa VM**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span>

   <span data-ttu-id="1d43d-109">![hello skapa VM-kommando][CR01]</span><span class="sxs-lookup"><span data-stu-id="1d43d-109">![hello Create VM command][CR01]</span></span>  
   <span data-ttu-id="1d43d-110">Hej **Skapa ny virtuell dator** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="1d43d-110">hello **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="1d43d-111">I hello **Välj en prenumeration** fönstret, väljer din prenumeration och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-111">In hello **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span>

   ![hello Välj en prenumeration][CR02]

4. <span data-ttu-id="1d43d-113">I hello **Välj en virtuell datoravbildning** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1d43d-113">In hello **Select a Virtual Machine Image** window, enter hello following information:</span></span>

   * <span data-ttu-id="1d43d-114">**Plats**: Anger där den virtuella datorn skapas (till exempel *västra USA*).</span><span class="sxs-lookup"><span data-stu-id="1d43d-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span>

   * <span data-ttu-id="1d43d-115">**Publisher**: Anger hello utgivare som skapade hello avbildningen ska du använda toocreate till den virtuella datorn (till exempel *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="1d43d-115">**Publisher**: Specifies hello publisher that created hello image you'll use toocreate your virtual machine (for example, *Microsoft*).</span></span>

   * <span data-ttu-id="1d43d-116">**Erbjuder**: Anger hello virtuella datorn erbjuder toouse från hello valda utgivare (till exempel *JDK*).</span><span class="sxs-lookup"><span data-stu-id="1d43d-116">**Offer**: Specifies hello virtual machine offering toouse from hello selected publisher (for example, *JDK*).</span></span>

   * <span data-ttu-id="1d43d-117">**SKU**: Anger hello lagerställeenheten SKU-enhet toouse från hello valda erbjudande (till exempel *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="1d43d-117">**Sku**: Specifies hello stockkeeping unit (SKU) toouse from hello selected offering (for example, *JDK_8*).</span></span>

   * <span data-ttu-id="1d43d-118">**Version #**: Anger vilken version av hello markerad SKU toouse.</span><span class="sxs-lookup"><span data-stu-id="1d43d-118">**Version #**: Specifies which version of hello selected SKU toouse.</span></span>

    ![hello Markera ett fönster för avbildning av virtuell dator][CR03]

5. <span data-ttu-id="1d43d-120">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-120">Click **Next**.</span></span>

6. <span data-ttu-id="1d43d-121">I hello **grundläggande inställningar för virtuell dator** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1d43d-121">In hello **Virtual Machine Basic Settings** window, enter hello following information:</span></span>

   * <span data-ttu-id="1d43d-122">**Namn på virtuell dator**: Anger hello namn för din nya virtuella datorn som måste börja med en bokstav och innehålla bara bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="1d43d-122">**Virtual Machine Name**: Specifies hello name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="1d43d-123">**Storlek**: Anger hello antalet kärnor och minne tooallocate för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d43d-123">**Size**: Specifies hello number of cores and memory tooallocate for your virtual machine.</span></span>

   * <span data-ttu-id="1d43d-124">**Användarnamnet**: Anger Hej administratör konto toocreate för att hantera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d43d-124">**User name**: Specifies hello administrator account toocreate for managing your virtual machine.</span></span>

   * <span data-ttu-id="1d43d-125">**Lösenordet** och **Bekräfta**: Anger hello lösenordet för ditt administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="1d43d-125">**Password** and **Confirm**: Specifies hello password for your administrator account.</span></span>

    ![hello grundläggande inställningar för virtuell dator fönster][CR04]

7. <span data-ttu-id="1d43d-127">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-127">Click **Next**.</span></span>

8. <span data-ttu-id="1d43d-128">I hello **Skapa nytt Lagringskonto** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1d43d-128">In hello **Create New Storage Account** window, enter hello following information:</span></span>

   * <span data-ttu-id="1d43d-129">**Resursgruppen**: Anger hello resursgruppen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d43d-129">**Resource Group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="1d43d-130">Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="1d43d-130">Select one of hello following options:</span></span>
      * <span data-ttu-id="1d43d-131">**Skapa en ny**: Anger att du vill toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1d43d-131">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="1d43d-132">**Använd befintlig**: Anger att du vill tooselect en resursgrupp är redan kopplat till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1d43d-132">**Use existing**: Specifies that you want tooselect a resource group that is already associated with your Azure account.</span></span>

      ![dialogrutan Skapa nytt Lagringskonto för hello][CR05]

   * <span data-ttu-id="1d43d-134">**Lagringskontot**: Anger hello storage-konto toouse för att lagra den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d43d-134">**Storage account**: Specifies hello storage account toouse for storing your virtual machine.</span></span> <span data-ttu-id="1d43d-135">Du kan använda ett befintligt lagringskonto eller skapa ett nytt konto.</span><span class="sxs-lookup"><span data-stu-id="1d43d-135">You can use an existing storage account or create a new account.</span></span>

   * <span data-ttu-id="1d43d-136">**Virtuellt nätverk** och **undernät**: Anger hello virtuellt nätverk och undernät som den virtuella datorn ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="1d43d-136">**Virtual Network** and **Subnet**: Specifies hello virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="1d43d-137">Du kan använda ett befintligt nätverk och undernät eller skapa ett nytt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="1d43d-137">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="1d43d-138">Om du väljer **Skapa nytt**, visas följande dialogruta hello:</span><span class="sxs-lookup"><span data-stu-id="1d43d-138">If you select **Create new**, hello following dialog box is displayed:</span></span>

      ![dialogrutan för hello Skapa nytt virtuellt nätverk][CR06]

9. <span data-ttu-id="1d43d-140">I hello **associerade resurser** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1d43d-140">In hello **Associated Resources** window, enter hello following information:</span></span>

   * <span data-ttu-id="1d43d-141">**Offentliga IP-adressen**: Anger en externa IP-adress för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d43d-141">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="1d43d-142">Du kan välja toocreate en ny IP-adress eller, om den virtuella datorn inte har en offentlig IP-adress, kan du välja **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-142">You can choose toocreate a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span>

   * <span data-ttu-id="1d43d-143">**Nätverkssäkerhetsgruppen**: Anger en valfri nätverk Brandvägg för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1d43d-143">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="1d43d-144">Du kan välja en befintlig brandvägg eller om den virtuella datorn inte kommer att använda en brandvägg för nätverk, kan du välja **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-144">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span>

   * <span data-ttu-id="1d43d-145">**Tillgänglighetsuppsättningen**: Anger en valfri tillgänglighetsuppsättning som den virtuella datorn kan tillhöra.</span><span class="sxs-lookup"><span data-stu-id="1d43d-145">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="1d43d-146">Du kan välja en befintlig tillgänglighetsuppsättning eller skapa en ny tillgänglighetsuppsättning eller om den virtuella datorn inte kommer tillhör tillgänglighetsuppsättningen tooan, kan du välja **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-146">You can select an existing availability set or create a new availability set or, if your virtual machine will not belong tooan availability set, you can select **(None)**.</span></span>

   ![hello associerade resurser fönster][CR07]

9. <span data-ttu-id="1d43d-148">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-148">Click **Finish**.</span></span>  
  <span data-ttu-id="1d43d-149">Den nya virtuella datorn visas i hello Azure verktyget Explorer.</span><span class="sxs-lookup"><span data-stu-id="1d43d-149">Your new virtual machine is displayed in hello Azure Explorer tool window.</span></span>

   ![Ny virtuell dator][CR08]

## <a name="restart-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d43d-151">Starta om en virtuell dator i Eclipse</span><span class="sxs-lookup"><span data-stu-id="1d43d-151">Restart a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d43d-152">toorestart en virtuell dator med hjälp av hello Azure Explorer i Eclipse hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d43d-152">toorestart a virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="1d43d-153">I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **starta om**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-153">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Restart**.</span></span>

   ![Hej kommandot för virtuella datorer starta om][RE01]

2. <span data-ttu-id="1d43d-155">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-155">In hello confirmation window, click **Yes**.</span></span>

   ![hello omstart bekräftelsefönstret][RE02]

## <a name="shut-down-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d43d-157">Stänga av en virtuell dator i Eclipse</span><span class="sxs-lookup"><span data-stu-id="1d43d-157">Shut down a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d43d-158">tooshut ned en aktiv virtuell dator med hjälp av hello Azure Explorer i Eclipse hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d43d-158">tooshut down a running virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="1d43d-159">I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-159">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Shutdown**.</span></span>

   ![Hej avstängningskommandot för virtuella datorer][SH01]

2. <span data-ttu-id="1d43d-161">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-161">In hello confirmation window, click **Yes**.</span></span>

   ![hello bekräftelsefönstret för virtuella datorer avstängning][SH02]

## <a name="delete-a-virtual-machine-in-eclipse"></a><span data-ttu-id="1d43d-163">Ta bort en virtuell dator i Eclipse</span><span class="sxs-lookup"><span data-stu-id="1d43d-163">Delete a virtual machine in Eclipse</span></span>

<span data-ttu-id="1d43d-164">toodelete en virtuell dator med hjälp av hello Azure Explorer i Eclipse hello följande:</span><span class="sxs-lookup"><span data-stu-id="1d43d-164">toodelete a virtual machine by using hello Azure Explorer in Eclipse, do hello following:</span></span>

1. <span data-ttu-id="1d43d-165">I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-165">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Delete**.</span></span>

   ![Hej Delete-kommandot för virtuella datorer][DE01]

2. <span data-ttu-id="1d43d-167">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="1d43d-167">In hello confirmation window, click **Yes**.</span></span>

   ![hello bekräftelsefönstret för borttagning av virtuella datorer][DE02]

## <a name="next-steps"></a><span data-ttu-id="1d43d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d43d-169">Next steps</span></span>
<span data-ttu-id="1d43d-170">Mer information om Azure storlekar för virtuella datorer och priser finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="1d43d-170">For more information about Azure virtual-machine sizes and pricing, see hello following resources:</span></span>

* <span data-ttu-id="1d43d-171">Azure storlekar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1d43d-171">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="1d43d-172">[Storlekar för virtuella Windows-datorer i Azure]</span><span class="sxs-lookup"><span data-stu-id="1d43d-172">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="1d43d-173">[Storlekar för virtuella Linux-datorer i Azure]</span><span class="sxs-lookup"><span data-stu-id="1d43d-173">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="1d43d-174">Priser för Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1d43d-174">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="1d43d-175">[Priser för Windows virtuell dator]</span><span class="sxs-lookup"><span data-stu-id="1d43d-175">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="1d43d-176">[Priser för Linux virtuella datorer]</span><span class="sxs-lookup"><span data-stu-id="1d43d-176">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="1d43d-177">Mer information om hello Azure-verktyg för Java IDEs finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="1d43d-177">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="1d43d-178">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="1d43d-178">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1d43d-179">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="1d43d-179">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1d43d-180">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="1d43d-180">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1d43d-181">[inloggning anvisningar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="1d43d-181">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1d43d-182">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="1d43d-182">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="1d43d-183">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="1d43d-183">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1d43d-184">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="1d43d-184">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1d43d-185">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="1d43d-185">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1d43d-186">[Logga in instruktioner för hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="1d43d-186">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1d43d-187">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="1d43d-187">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="1d43d-188">Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="1d43d-188">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

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

[Storlekar för virtuella Windows-datorer i Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Storlekar för virtuella Linux-datorer i Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Priser för Windows virtuell dator]: /pricing/details/virtual-machines/windows/
[Priser för Linux virtuella datorer]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer/CR08.png
