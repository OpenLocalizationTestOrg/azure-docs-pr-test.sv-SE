---
title: "aaaManage virtuella datorer med hjälp av hello Azure Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig hur toomanage Azure virtuella datorer med hjälp av hello Azure Explorer för IntelliJ."
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
ms.openlocfilehash: a73dd4f73b311dd3413f6712e3b76c36ee464de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-by-using-hello-azure-explorer-for-intellij"></a><span data-ttu-id="a9a00-103">Hantera virtuella datorer med hjälp av hello Azure Explorer för IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a9a00-103">Manage virtual machines by using hello Azure Explorer for IntelliJ</span></span>

<span data-ttu-id="a9a00-104">hello Azure Explorer, som är en del av hello Azure Toolkit för IntelliJ ger Java-utvecklare med en enkel att använda lösning för hantering av virtuella datorer i Azure kontot från inuti hello IntelliJ integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="a9a00-104">hello Azure Explorer, which is part of hello Azure Toolkit for IntelliJ, provides Java developers with an easy-to-use solution for managing virtual machines in their Azure account from inside hello IntelliJ integrated development environment (IDE).</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-virtual-machine-in-intellij"></a><span data-ttu-id="a9a00-105">Skapa en virtuell dator i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a9a00-105">Create a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a9a00-106">toocreate en virtuell dator med hjälp av hello Azure Explorer hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9a00-106">toocreate a virtual machine by using hello Azure Explorer, do hello following:</span></span> 

1. <span data-ttu-id="a9a00-107">Logga in tooyour Azure-konto med hjälp av hello steg i hello [inloggning instruktioner för hello Azure Toolkit för IntelliJ] artikel.</span><span class="sxs-lookup"><span data-stu-id="a9a00-107">Sign in tooyour Azure account by using hello steps in hello [Sign-in instructions for hello Azure Toolkit for IntelliJ] article.</span></span>

2. <span data-ttu-id="a9a00-108">I hello **Azure Explorer** , expanderar hello **Azure** noden, högerklickar du på **virtuella datorer**, och klicka sedan på **skapa VM**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-108">In hello **Azure Explorer** view, expand hello **Azure** node, right-click **Virtual Machines**, and then click **Create VM**.</span></span> 

   <span data-ttu-id="a9a00-109">![hello skapa VM-kommando][CR01]</span><span class="sxs-lookup"><span data-stu-id="a9a00-109">![hello Create VM command][CR01]</span></span>  
    <span data-ttu-id="a9a00-110">Hej **Skapa ny virtuell dator** öppnas guiden.</span><span class="sxs-lookup"><span data-stu-id="a9a00-110">hello **Create new Virtual Machine** wizard opens.</span></span>

3. <span data-ttu-id="a9a00-111">I hello **Välj en prenumeration** fönstret, väljer din prenumeration och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-111">In hello **Choose a Subscription** window, select your subscription, and then click **Next**.</span></span> 

   ![hello Välj en prenumeration][CR02]

4. <span data-ttu-id="a9a00-113">I hello **Välj en virtuell datoravbildning** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a9a00-113">In hello **Select a Virtual Machine Image** window, enter hello following information:</span></span>

   * <span data-ttu-id="a9a00-114">**Plats**: Anger där den virtuella datorn skapas (till exempel *västra USA*).</span><span class="sxs-lookup"><span data-stu-id="a9a00-114">**Location**: Specifies where your virtual machine will be created (for example, *West US*).</span></span> 

   * <span data-ttu-id="a9a00-115">**Rekommenderat avbildningen**: Anger att du väljer en bild från en förkortad lista med vanliga bilder.</span><span class="sxs-lookup"><span data-stu-id="a9a00-115">**Recommended image**: Specifies that you will choose an image from an abbreviated list of commonly used images.</span></span>

   * <span data-ttu-id="a9a00-116">**Anpassad avbildning**: Anger att du väljer en anpassad avbildning genom att tillhandahålla hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a9a00-116">**Custom image**: Specifies that you will choose a custom image by providing hello following information:</span></span>

      * <span data-ttu-id="a9a00-117">**Publisher**: Anger hello utgivare som skapade hello-avbildning som ska användas för den virtuella datorn (till exempel *Microsoft*).</span><span class="sxs-lookup"><span data-stu-id="a9a00-117">**Publisher**: Specifies hello publisher that created hello image that you will use for your virtual machine (for example, *Microsoft*).</span></span>

      * <span data-ttu-id="a9a00-118">**Erbjuder**: Anger hello virtuella datorn erbjuder toouse från hello valda utgivare (till exempel *JDK*).</span><span class="sxs-lookup"><span data-stu-id="a9a00-118">**Offer**: Specifies hello virtual machine offering toouse from hello selected publisher (for example, *JDK*).</span></span>

      * <span data-ttu-id="a9a00-119">**SKU**: Anger hello lagerställeenheten SKU-enhet toouse från hello valda erbjudande (till exempel *JDK_8*).</span><span class="sxs-lookup"><span data-stu-id="a9a00-119">**Sku**: Specifies hello stockkeeping unit (SKU) toouse from hello selected offering (for example, *JDK_8*).</span></span>

      * <span data-ttu-id="a9a00-120">**Version #**: Anger vilken version av hello markerad SKU toouse.</span><span class="sxs-lookup"><span data-stu-id="a9a00-120">**Version #**: Specifies which version of hello selected SKU toouse.</span></span>

   ![hello Markera ett fönster för avbildning av virtuell dator][CR03]

5. <span data-ttu-id="a9a00-122">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-122">Click **Next**.</span></span> 

6. <span data-ttu-id="a9a00-123">I hello **grundläggande inställningar för virtuell dator** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a9a00-123">In hello **Virtual Machine Basic Settings** window, enter hello following information:</span></span>

   * <span data-ttu-id="a9a00-124">**Namn på virtuell dator**: Anger hello namn för din nya virtuella datorn som måste börja med en bokstav och innehålla bara bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="a9a00-124">**Virtual machine name**: Specifies hello name for your new virtual machine, which must start with a letter and contain only letters, numbers, and hyphens.</span></span>

   * <span data-ttu-id="a9a00-125">**Storlek**: Anger hello antalet kärnor och minne tooallocate för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9a00-125">**Size**: Specifies hello number of cores and memory tooallocate for your virtual machine.</span></span>

   * <span data-ttu-id="a9a00-126">**Användarnamnet**: Anger Hej administratör konto toocreate för att hantera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9a00-126">**User name**: Specifies hello administrator account toocreate for managing your virtual machine.</span></span>

   * <span data-ttu-id="a9a00-127">**Lösenordet** och **Bekräfta**: Anger hello lösenordet för ditt administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="a9a00-127">**Password** and **Confirm**: Specifies hello password for your administrator account.</span></span>

   ![hello grundläggande inställningar för virtuell dator fönster][CR04]

7. <span data-ttu-id="a9a00-129">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-129">Click **Next**.</span></span> 

8. <span data-ttu-id="a9a00-130">I hello **associerade resurser** fönstret Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a9a00-130">In hello **Associated Resources** window, enter hello following information:</span></span>

   * <span data-ttu-id="a9a00-131">**Resursgruppen**: Anger hello resursgruppen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9a00-131">**Resource group**: Specifies hello resource group for your virtual machine.</span></span> <span data-ttu-id="a9a00-132">Välj något av följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="a9a00-132">Select one of hello following options:</span></span>
      * <span data-ttu-id="a9a00-133">**Skapa en ny**: Anger att du vill toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a9a00-133">**Create new**: Specifies that you want toocreate a new resource group.</span></span>
      * <span data-ttu-id="a9a00-134">**Använd befintlig**: Anger att du vill tooselect från en lista över resursgrupper som är associerade med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a9a00-134">**Use existing**: Specifies that you want tooselect from a list of resource groups that are associated with your Azure account.</span></span>

       ![hello associerade resurser fönster][CR07]

   * <span data-ttu-id="a9a00-136">**Lagringskontot**: Anger hello storage-konto toouse för att lagra den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9a00-136">**Storage account**: Specifies hello storage account toouse for storing your virtual machine.</span></span> <span data-ttu-id="a9a00-137">Du kan välja ett befintligt lagringskonto eller skapa ett nytt konto.</span><span class="sxs-lookup"><span data-stu-id="a9a00-137">You can choose an existing storage account or create a new account.</span></span> <span data-ttu-id="a9a00-138">Om du väljer **Skapa nytt**, visas följande dialogruta hello:</span><span class="sxs-lookup"><span data-stu-id="a9a00-138">If you choose **Create New**, hello following dialog box appears:</span></span>

      ![dialogrutan för hello skapa Storage-konto][CR05]

   * <span data-ttu-id="a9a00-140">**Virtuellt nätverk** och **undernät**: Anger hello virtuellt nätverk och undernät som den virtuella datorn ska ansluta till.</span><span class="sxs-lookup"><span data-stu-id="a9a00-140">**Virtual Network** and **Subnet**: Specifies hello virtual network and subnet that your virtual machine will connect to.</span></span> <span data-ttu-id="a9a00-141">Du kan använda ett befintligt nätverk och undernät eller skapa ett nytt nätverk och undernät.</span><span class="sxs-lookup"><span data-stu-id="a9a00-141">You can use an existing network and subnet, or you can create a new network and subnet.</span></span> <span data-ttu-id="a9a00-142">Om du väljer **Skapa nytt**, visas följande dialogruta hello:</span><span class="sxs-lookup"><span data-stu-id="a9a00-142">If you select **Create new**, hello following dialog box appears:</span></span>

      ![dialogrutan för hello skapa virtuella nätverk][CR06]

   * <span data-ttu-id="a9a00-144">**Offentliga IP-adressen**: Anger en externa IP-adress för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9a00-144">**Public IP address**: Specifies an external-facing IP address for your virtual machine.</span></span> <span data-ttu-id="a9a00-145">Du kan välja toocreate en ny IP-adress eller, om den virtuella datorn inte har en offentlig IP-adress, kan du välja **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-145">You can choose toocreate a new IP address or, if your virtual machine will not have a public IP address, you can select **(None)**.</span></span> 

   * <span data-ttu-id="a9a00-146">**Nätverkssäkerhetsgruppen**: Anger en valfri nätverk Brandvägg för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a9a00-146">**Network security group**: Specifies an optional networking firewall for your virtual machine.</span></span> <span data-ttu-id="a9a00-147">Du kan välja en befintlig brandvägg eller om den virtuella datorn inte kommer att använda en brandvägg för nätverk, kan du välja **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-147">You can select an existing firewall or, if your virtual machine will not use a network firewall, you can select **(None)**.</span></span> 

   * <span data-ttu-id="a9a00-148">**Tillgänglighetsuppsättningen**: Anger en valfri tillgänglighetsuppsättning som den virtuella datorn kan tillhöra.</span><span class="sxs-lookup"><span data-stu-id="a9a00-148">**Availability set**: Specifies an optional availability set that your virtual machine can belong to.</span></span> <span data-ttu-id="a9a00-149">Du kan välja en befintlig tillgänglighetsuppsättning, skapa en ny tillgänglighetsuppsättning eller, om den virtuella datorn inte kommer tillhör tooan tillgänglighetsuppsättning väljer **(ingen)**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-149">You can select an existing availability set, create a new availability set or, if your virtual machine will not belong tooan availability set, select **(None)**.</span></span>

9. <span data-ttu-id="a9a00-150">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-150">Click **Finish**.</span></span>  
    <span data-ttu-id="a9a00-151">Den nya virtuella datorn visas i hello Azure verktyget Explorer.</span><span class="sxs-lookup"><span data-stu-id="a9a00-151">Your new virtual machine appears in hello Azure Explorer tool window.</span></span> 

   ![Ny virtuell dator i hello Azure Utforskarvy][CR08]

## <a name="restart-a-virtual-machine-in-intellij"></a><span data-ttu-id="a9a00-153">Starta om en virtuell dator i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a9a00-153">Restart a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a9a00-154">toorestart en virtuell dator med hjälp av hello Azure Explorer i IntelliJ, hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9a00-154">toorestart a virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="a9a00-155">I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **starta om**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-155">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Restart**.</span></span>

   ![Hej kommandot för virtuella datorer starta om][RE01]

2. <span data-ttu-id="a9a00-157">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-157">In hello confirmation window, click **Yes**.</span></span> 

   ![hello starta om bekräftelsefönstret för virtuell dator][RE02]

## <a name="shut-down-a-virtual-machine-in-intellij"></a><span data-ttu-id="a9a00-159">Stänga av en virtuell dator i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a9a00-159">Shut down a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a9a00-160">tooshut ned en aktiv virtuell dator med hjälp av hello Azure Explorer i IntelliJ, hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9a00-160">tooshut down a running virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="a9a00-161">I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-161">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Shutdown**.</span></span>

   ![Hej avstängningskommandot för virtuella datorer][SH01]

2. <span data-ttu-id="a9a00-163">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-163">In hello confirmation window, click **Yes**.</span></span> 

   ![hello Stäng fönstret för bekräftelse av virtuell dator][SH02]

## <a name="delete-a-virtual-machine-in-intellij"></a><span data-ttu-id="a9a00-165">Ta bort en virtuell dator i IntelliJ</span><span class="sxs-lookup"><span data-stu-id="a9a00-165">Delete a virtual machine in IntelliJ</span></span>

<span data-ttu-id="a9a00-166">toodelete en virtuell dator med hjälp av hello Azure Explorer i IntelliJ, hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9a00-166">toodelete a virtual machine by using hello Azure Explorer in IntelliJ, do hello following:</span></span>

1. <span data-ttu-id="a9a00-167">I hello **Azure Explorer** visa, högerklicka på hello virtuella datorn och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-167">In hello **Azure Explorer** view, right-click hello virtual machine, and then select **Delete**.</span></span>

   ![Hej Delete-kommandot för virtuella datorer][DE01]

2. <span data-ttu-id="a9a00-169">I fönstret för bekräftelse av hello klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="a9a00-169">In hello confirmation window, click **Yes**.</span></span> 

   ![hello ta bort fönstret för bekräftelse av virtuell dator][DE02]

## <a name="next-steps"></a><span data-ttu-id="a9a00-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9a00-171">Next steps</span></span>
<span data-ttu-id="a9a00-172">Mer information om Azure storlekar för virtuella datorer och priser finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a9a00-172">For more information about Azure virtual-machine sizes and pricing, see hello following resources:</span></span>

* <span data-ttu-id="a9a00-173">Azure storlekar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a9a00-173">Azure virtual-machine sizes</span></span>
  * <span data-ttu-id="a9a00-174">[Storlekar för virtuella Windows-datorer i Azure]</span><span class="sxs-lookup"><span data-stu-id="a9a00-174">[Sizes for Windows virtual machines in Azure]</span></span>
  * <span data-ttu-id="a9a00-175">[Storlekar för virtuella Linux-datorer i Azure]</span><span class="sxs-lookup"><span data-stu-id="a9a00-175">[Sizes for Linux virtual machines in Azure]</span></span>
* <span data-ttu-id="a9a00-176">Priser för Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="a9a00-176">Azure virtual-machine pricing</span></span>
  * <span data-ttu-id="a9a00-177">[Priser för Windows virtuell dator]</span><span class="sxs-lookup"><span data-stu-id="a9a00-177">[Windows virtual-machine pricing]</span></span>
  * <span data-ttu-id="a9a00-178">[Priser för Linux virtuella datorer]</span><span class="sxs-lookup"><span data-stu-id="a9a00-178">[Linux virtual-machine pricing]</span></span>

<span data-ttu-id="a9a00-179">Mer information om hello Azure-verktyg för Java IDEs finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a9a00-179">For more information about hello Azure Toolkits for Java IDEs, see hello following resources:</span></span>

* <span data-ttu-id="a9a00-180">[Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a9a00-180">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a9a00-181">[Vad är nytt i hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a9a00-181">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a9a00-182">[Installera hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a9a00-182">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a9a00-183">[Logga in anvisningar hello Azure Toolkit för Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a9a00-183">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a9a00-184">[Skapa en Hello World-webbapp för Azure i Eclipse]</span><span class="sxs-lookup"><span data-stu-id="a9a00-184">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="a9a00-185">[Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a9a00-185">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a9a00-186">[Vad är nytt i hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a9a00-186">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a9a00-187">[Installera hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a9a00-187">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a9a00-188">[inloggning instruktioner för hello Azure Toolkit för IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a9a00-188">[Sign-in instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a9a00-189">[Skapa en Hello World-webbapp för Azure i IntelliJ]</span><span class="sxs-lookup"><span data-stu-id="a9a00-189">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="a9a00-190">Mer information om hur du använder Azure med Java finns [Azure Java Developer Center] och [Java-verktyg för Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="a9a00-190">For more information about using Azure with Java, see [Azure Java Developer Center] and [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

[Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij.md
[Skapa en Hello World-webbapp för Azure i Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Skapa en Hello World-webbapp för Azure i IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installera hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installera hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Logga in anvisningar hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[inloggning instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Vad är nytt i hello Azure Toolkit för Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Vad är nytt i hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-verktyg för Visual Studio Team Services]: https://java.visualstudio.com/

[Storlekar för virtuella Windows-datorer i Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Storlekar för virtuella Linux-datorer i Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Priser för Windows virtuell dator]: /pricing/details/virtual-machines/windows/
[Priser för Linux virtuella datorer]: /pricing/details/virtual-machines/linux/


<!-- IMG List -->

[RE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE01.png
[RE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/RE02.png

[SH01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH01.png
[SH02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/SH02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/DE02.png

[CR01]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR03.png
[CR04]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR04.png
[CR05]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR05.png
[CR06]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR06.png
[CR07]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR07.png
[CR08]: ./media/azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer/CR08.png
