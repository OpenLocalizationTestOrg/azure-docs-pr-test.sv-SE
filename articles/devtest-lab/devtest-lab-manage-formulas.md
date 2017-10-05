---
title: "Hantera formler i Azure DevTest Labs för att skapa virtuella datorer | Microsoft Docs"
description: "Lär dig att uppdatera och ta bort Azure DevTest Labs formler"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfdab5def50158f9b764bbb1e50c2624cc6d5fb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a><span data-ttu-id="ac6ed-103">Hantera Azure DevTest Labs formler</span><span class="sxs-lookup"><span data-stu-id="ac6ed-103">Manage Azure DevTest Labs formulas</span></span>

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

<span data-ttu-id="ac6ed-104">Den här artikeln beskrivs hur du skapar en formel från en bas (anpassad avbildning, Marketplace-avbildning eller en annan metod) eller en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-104">This article illustrates how to create a formula from either a base (custom image, Marketplace image, or another formula) or an existing VM.</span></span> <span data-ttu-id="ac6ed-105">Den här artikeln hjälper dig att hantera befintliga formler också.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-105">This article also guides you through managing existing formulas.</span></span>

## <a name="create-a-formula"></a><span data-ttu-id="ac6ed-106">Skapa en formel</span><span class="sxs-lookup"><span data-stu-id="ac6ed-106">Create a formula</span></span>
<span data-ttu-id="ac6ed-107">Alla med DevTest Labs *användare* behörigheter är att skapa virtuella datorer med en formel som bas.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-107">Anyone with DevTest Labs *Users* permissions is able to create VMs using a formula as a base.</span></span> <span data-ttu-id="ac6ed-108">Det finns två sätt att skapa formler:</span><span class="sxs-lookup"><span data-stu-id="ac6ed-108">There are two ways to create formulas:</span></span> 

* <span data-ttu-id="ac6ed-109">Använd när du vill definiera alla av formeln egenskaper från en bas.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-109">From a base - Use when you want to define all the characteristics of the formula.</span></span>
* <span data-ttu-id="ac6ed-110">Använd när du vill skapa en formel baserat på inställningarna för en befintlig virtuell dator från en befintlig lab VM.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-110">From an existing lab VM - Use when you want to create a formula based on the settings of an existing VM.</span></span>

<span data-ttu-id="ac6ed-111">Mer information om att lägga till användare och behörigheter finns [lägga till ägare och användare i Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-111">For more information about adding users and permissions, see [Add owners and users in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span></span>

### <a name="create-a-formula-from-a-base"></a><span data-ttu-id="ac6ed-112">Skapa en formel från en bas</span><span class="sxs-lookup"><span data-stu-id="ac6ed-112">Create a formula from a base</span></span>
<span data-ttu-id="ac6ed-113">Följande steg leder dig genom processen att skapa en formel från en anpassad avbildning, Marketplace-avbildning eller en annan formel.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-113">The following steps guide you through the process of creating a formula from a custom image, Marketplace image, or another formula.</span></span>

1. <span data-ttu-id="ac6ed-114">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-114">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

2. <span data-ttu-id="ac6ed-115">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-115">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>

3. <span data-ttu-id="ac6ed-116">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-116">From the list of labs, select the desired lab.</span></span>  

4. <span data-ttu-id="ac6ed-117">På den testmiljön bladet välj **formler (återanvändbara baser)**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-117">On the lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. <span data-ttu-id="ac6ed-119">På den **formler** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-119">On the **Formulas** blade, select **+ Add**.</span></span>
   
    ![Lägga till en formel](./media/devtest-lab-create-formulas/add-formula.png)

6. <span data-ttu-id="ac6ed-121">På den **väljer du en bas** bladet välj base (anpassad avbildning, Marketplace-avbildning eller metod) från vilken du vill skapa formeln.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-121">On the **Choose a base** blade, select the base (custom image, Marketplace image, or formula) from which you want to create the formula.</span></span>
   
    ![Grundläggande lista](./media/devtest-lab-create-formulas/base-list.png)

7. <span data-ttu-id="ac6ed-123">På den **skapa formeln** bladet anger du följande värden:</span><span class="sxs-lookup"><span data-stu-id="ac6ed-123">On the **Create formula** blade, specify the following values:</span></span>
   
    * <span data-ttu-id="ac6ed-124">**Formelnamnet** -ange ett namn för din formel.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-124">**Formula name** - Enter a name for your formula.</span></span> <span data-ttu-id="ac6ed-125">Det här värdet visas i listan över grundläggande bilder när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-125">This value is displayed in the list of base images when you create a VM.</span></span> <span data-ttu-id="ac6ed-126">Verifiera namnet på som du skriver du det och om det är inte giltig, ett meddelande som anger kraven för ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-126">The name is validated as you type it, and if not valid, a message indicates the requirements for a valid name.</span></span>
    * <span data-ttu-id="ac6ed-127">**Beskrivning** -ange en beskrivning för din formel.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-127">**Description** - Enter a meaningful description for your formula.</span></span> <span data-ttu-id="ac6ed-128">Det här värdet är tillgängligt formelns snabbmenyn när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-128">This value is available from the formula's context menu when you create a VM.</span></span>
    * <span data-ttu-id="ac6ed-129">**Användarnamnet** -ange ett användarnamn som har beviljats administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-129">**User name** - Enter a user name that is granted administrator privileges.</span></span>
    * <span data-ttu-id="ac6ed-130">**Lösenordet** – Skriv - eller välj i listrutan - ett värde som är associerade med den hemlighet (lösenord) som du vill använda för den angivna användaren.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-130">**Password** - Enter - or select from the dropdown - a value that is associated with the secret (password) that you want to use for the specified user.</span></span> <span data-ttu-id="ac6ed-131">Mer information om hemligheterna finns [Azure DevTest Labs: hemliga datorarkivet](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-131">For more information about the secrets, see [Azure DevTest Labs: Personal secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span></span>
    * <span data-ttu-id="ac6ed-132">**Typ av virtuell dator disk** – ange antingen Hårddisk (hårddiskenheten) eller SSD (SSD) som anger vilken lagringstyp som disk tillåts för de virtuella datorerna etableras med den här grundläggande bild.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-132">**Virtual machine disk type** - Specify either HDD (hard-disk drive) or SSD (solid-state drive) to indicate which storage disk type is allowed for the virtual machines provisioned using this base image.</span></span>
    * <span data-ttu-id="ac6ed-133">** Virtuella storlek ** – Välj något av de fördefinierade objekt som anger processorkärnor, ram-storlek och storleken på hårddisken på den virtuella datorn för att skapa.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-133">** Virtual machine size** - Select one of the predefined items that specify the processor cores, RAM size, and the hard drive size of the VM to create.</span></span> 
    * <span data-ttu-id="ac6ed-134">**Artefakter** – Välj för att öppna den **lägga till artefakter** bladet, där du kan välja och konfigurera artefakter som du vill lägga till basavbildningen.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-134">**Artifacts** - Select to open the **Add artifacts** blade, in which you select and configure the artifacts that you want to add to the base image.</span></span> <span data-ttu-id="ac6ed-135">Läs mer om artefakter [hantera VM artefakter i Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-135">For more information about artifacts, see [Manage VM artifacts in Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span></span>
    * <span data-ttu-id="ac6ed-136">**Avancerade inställningar** – Välj för att öppna den **Avancerat** bladet där du kan konfigurera följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="ac6ed-136">**Advanced settings** - Select to open the **Advanced** blade where you configure the following settings:</span></span>
        * <span data-ttu-id="ac6ed-137">**Virtuellt nätverk** -ange det önskade virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-137">**Virtual network** - Specify the desired virtual network.</span></span>
        * <span data-ttu-id="ac6ed-138">**Undernät** – ange det önskade undernätet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-138">**Subnet** - Specify the desired subnet.</span></span>    
        * <span data-ttu-id="ac6ed-139">**IP-adresskonfiguration** -ange om du vill Public, Private eller delade IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-139">**IP address configuration** - Specify if you want the Public, Private, or Shared IP addresses.</span></span> <span data-ttu-id="ac6ed-140">Mer information om delade IP-adresser finns [Förstå delade IP-adresser i Azure DevTest Labs](./devtest-lab-shared-ip.md).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-140">For more information about shared IP addresses, see [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md).</span></span>
        * <span data-ttu-id="ac6ed-141">**Se den här datorn claimable** -gör en dator ”claimable” innebär att den inte tilldelas ägarskap vid tidpunkten för skapandet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-141">**Make this machine claimable** - Making a machine "claimable" means that it will not be assigned ownership at the time of creation.</span></span> <span data-ttu-id="ac6ed-142">I stället kommer lab-användare att kunna bli ägare (”anspråk”) datorn i den testmiljön bladet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-142">Instead lab users will be able to take ownership ("claim") the machine in the lab's blade.</span></span>     
    * <span data-ttu-id="ac6ed-143">**Bild** -det här fältet visar namnet på basavbildningen du valde på föregående blad.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-143">**Image** - This field displays name of the base image you selected on the previous blade.</span></span> 
     
       ![Skapa formeln](./media/devtest-lab-create-formulas/create-formula.png)

8. <span data-ttu-id="ac6ed-145">Välj **skapa** att skapa formeln.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-145">Select **Create** to create the formula.</span></span>

9. <span data-ttu-id="ac6ed-146">När formeln har skapats kan den visas i listan på den **formler** bladet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-146">When the formula has been created, it displays in the list on the **Formulas** blade.</span></span>

### <a name="create-a-formula-from-a-vm"></a><span data-ttu-id="ac6ed-147">Skapa en formel från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ac6ed-147">Create a formula from a VM</span></span>
<span data-ttu-id="ac6ed-148">Följande steg leder dig genom processen att skapa en formel som baseras på en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-148">The following steps guide you through the process of creating a formula based on an existing VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="ac6ed-149">Om du vill skapa en formel från en virtuell dator måste den virtuella datorn har skapats efter den 30 mars 2016.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-149">To create a formula from a VM, the VM must have been created after March 30, 2016.</span></span> 
> 
> 

1. <span data-ttu-id="ac6ed-150">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-150">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="ac6ed-151">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-151">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="ac6ed-152">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-152">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="ac6ed-153">På testmiljön **översikt** bladet Välj den virtuella datorn från vilken du vill skapa formeln.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-153">On the lab's **Overview** blade, select the VM from which you wish to create the formula.</span></span>
   
    ![Labs virtuella datorer](./media/devtest-lab-create-formulas/my-vms.png)
5. <span data-ttu-id="ac6ed-155">På bladet för den virtuella datorn, väljer **skapa formeln (återanvändbara base)**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-155">On the VM's blade, select **Create formula (reusable base)**.</span></span>
   
    ![Skapa formeln](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. <span data-ttu-id="ac6ed-157">På den **skapa formeln** bladet anger du en **namn** och **beskrivning** för din nya formel.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-157">On the **Create formula** blade, enter a **Name** and **Description** for your new formula.</span></span>
   
    ![Skapa formeln bladet](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. <span data-ttu-id="ac6ed-159">Välj **OK** att skapa formeln.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-159">Select **OK** to create the formula.</span></span>

## <a name="modify-a-formula"></a><span data-ttu-id="ac6ed-160">Ändra en formel</span><span class="sxs-lookup"><span data-stu-id="ac6ed-160">Modify a formula</span></span>
<span data-ttu-id="ac6ed-161">Följ dessa steg om du vill ändra en formel:</span><span class="sxs-lookup"><span data-stu-id="ac6ed-161">To modify a formula, follow these steps:</span></span>

1. <span data-ttu-id="ac6ed-162">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-162">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="ac6ed-163">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-163">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="ac6ed-164">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-164">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="ac6ed-165">På den testmiljön bladet välj **formler (återanvändbara baser)**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-165">On the lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="ac6ed-167">På den **Lab formler** bladet Välj formeln som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-167">On the **Lab formulas** blade, select the formula you wish to modify.</span></span>
6. <span data-ttu-id="ac6ed-168">På den **uppdatera formel** bladet gör önskade ändringar och välj **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-168">On the **Update formula** blade, make the desired edits, and select **Update**.</span></span>

## <a name="delete-a-formula"></a><span data-ttu-id="ac6ed-169">Ta bort en formel</span><span class="sxs-lookup"><span data-stu-id="ac6ed-169">Delete a formula</span></span>
<span data-ttu-id="ac6ed-170">Följ dessa steg om du vill ta bort en formel:</span><span class="sxs-lookup"><span data-stu-id="ac6ed-170">To delete a formula, follow these steps:</span></span>

1. <span data-ttu-id="ac6ed-171">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-171">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="ac6ed-172">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-172">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="ac6ed-173">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-173">From the list of labs, select the desired lab.</span></span>  
4. <span data-ttu-id="ac6ed-174">Om labbet **inställningar** bladet väljer **formler**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-174">On the lab **Settings** blade, select **Formulas**.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="ac6ed-176">På den **Lab formler** bladet Välj tre punkter till höger om formeln som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-176">On the **Lab formulas** blade, select the ellipsis to the right of the formula you wish to delete.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. <span data-ttu-id="ac6ed-178">Markera på formelns snabbmenyn **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-178">On the formula's context menu, select **Delete**.</span></span>
   
    ![Formeln snabbmenyn](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. <span data-ttu-id="ac6ed-180">Välj **Ja** till dialogrutan för borttagning.</span><span class="sxs-lookup"><span data-stu-id="ac6ed-180">Select **Yes** to the deletion confirmation dialog.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="ac6ed-181">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="ac6ed-181">Related blog posts</span></span>
* [<span data-ttu-id="ac6ed-182">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="ac6ed-182">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="ac6ed-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac6ed-183">Next steps</span></span>
<span data-ttu-id="ac6ed-184">När du har skapat en formel för användning när du skapar en virtuell dator, nästa steg är att [lägga till en virtuell dator i labbet](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="ac6ed-184">Once you have created a formula for use when creating a VM, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

