---
title: aaaManage formler i Azure DevTest Labs toocreate VMs | Microsoft Docs
description: "Lär dig hur tooupdate och ta bort Azure DevTest Labs formler"
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
ms.openlocfilehash: 855debe46f3b70cc45ea5d55869663b64e225124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-devtest-labs-formulas"></a><span data-ttu-id="7acf1-103">Hantera Azure DevTest Labs formler</span><span class="sxs-lookup"><span data-stu-id="7acf1-103">Manage Azure DevTest Labs formulas</span></span>

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

<span data-ttu-id="7acf1-104">Den här artikeln beskrivs hur toocreate en formel från en bas (anpassad avbildning, Marketplace-avbildning eller en annan metod) eller en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7acf1-104">This article illustrates how toocreate a formula from either a base (custom image, Marketplace image, or another formula) or an existing VM.</span></span> <span data-ttu-id="7acf1-105">Den här artikeln hjälper dig att hantera befintliga formler också.</span><span class="sxs-lookup"><span data-stu-id="7acf1-105">This article also guides you through managing existing formulas.</span></span>

## <a name="create-a-formula"></a><span data-ttu-id="7acf1-106">Skapa en formel</span><span class="sxs-lookup"><span data-stu-id="7acf1-106">Create a formula</span></span>
<span data-ttu-id="7acf1-107">Alla med DevTest Labs *användare* behörigheter är kan toocreate virtuella datorer med en formel som bas.</span><span class="sxs-lookup"><span data-stu-id="7acf1-107">Anyone with DevTest Labs *Users* permissions is able toocreate VMs using a formula as a base.</span></span> <span data-ttu-id="7acf1-108">Det finns två sätt toocreate formler:</span><span class="sxs-lookup"><span data-stu-id="7acf1-108">There are two ways toocreate formulas:</span></span> 

* <span data-ttu-id="7acf1-109">Använd när du vill toodefine alla hello av hello formeln egenskaper från en bas.</span><span class="sxs-lookup"><span data-stu-id="7acf1-109">From a base - Use when you want toodefine all hello characteristics of hello formula.</span></span>
* <span data-ttu-id="7acf1-110">Använd när du vill toocreate en formel baserat på hello inställningarna för en befintlig virtuell dator från en befintlig lab VM.</span><span class="sxs-lookup"><span data-stu-id="7acf1-110">From an existing lab VM - Use when you want toocreate a formula based on hello settings of an existing VM.</span></span>

<span data-ttu-id="7acf1-111">Mer information om att lägga till användare och behörigheter finns [lägga till ägare och användare i Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="7acf1-111">For more information about adding users and permissions, see [Add owners and users in Azure DevTest Labs](./devtest-lab-add-devtest-user.md).</span></span>

### <a name="create-a-formula-from-a-base"></a><span data-ttu-id="7acf1-112">Skapa en formel från en bas</span><span class="sxs-lookup"><span data-stu-id="7acf1-112">Create a formula from a base</span></span>
<span data-ttu-id="7acf1-113">hello följande steg leder dig igenom hello processen att skapa en formel från en anpassad avbildning, Marketplace-avbildning eller en annan formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-113">hello following steps guide you through hello process of creating a formula from a custom image, Marketplace image, or another formula.</span></span>

1. <span data-ttu-id="7acf1-114">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7acf1-114">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

2. <span data-ttu-id="7acf1-115">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7acf1-115">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>

3. <span data-ttu-id="7acf1-116">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="7acf1-116">From hello list of labs, select hello desired lab.</span></span>  

4. <span data-ttu-id="7acf1-117">På bladet hello lab väljer **formler (återanvändbara baser)**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-117">On hello lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. <span data-ttu-id="7acf1-119">På hello **formler** bladet väljer **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-119">On hello **Formulas** blade, select **+ Add**.</span></span>
   
    ![Lägga till en formel](./media/devtest-lab-create-formulas/add-formula.png)

6. <span data-ttu-id="7acf1-121">På hello **väljer du en bas** bladet välj hello base (anpassad avbildning, Marketplace-avbildning eller metod) som du vill toocreate hello formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-121">On hello **Choose a base** blade, select hello base (custom image, Marketplace image, or formula) from which you want toocreate hello formula.</span></span>
   
    ![Grundläggande lista](./media/devtest-lab-create-formulas/base-list.png)

7. <span data-ttu-id="7acf1-123">På hello **skapa formeln** bladet ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="7acf1-123">On hello **Create formula** blade, specify hello following values:</span></span>
   
    * <span data-ttu-id="7acf1-124">**Formelnamnet** -ange ett namn för din formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-124">**Formula name** - Enter a name for your formula.</span></span> <span data-ttu-id="7acf1-125">Det här värdet visas i hello lista över grundläggande bilder när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7acf1-125">This value is displayed in hello list of base images when you create a VM.</span></span> <span data-ttu-id="7acf1-126">verifiera hello namnet som du skriver du det och om det är inte giltig, ett meddelande visar hello kraven för ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="7acf1-126">hello name is validated as you type it, and if not valid, a message indicates hello requirements for a valid name.</span></span>
    * <span data-ttu-id="7acf1-127">**Beskrivning** -ange en beskrivning för din formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-127">**Description** - Enter a meaningful description for your formula.</span></span> <span data-ttu-id="7acf1-128">Det här värdet är tillgängligt hello formeln snabbmenyn när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7acf1-128">This value is available from hello formula's context menu when you create a VM.</span></span>
    * <span data-ttu-id="7acf1-129">**Användarnamnet** -ange ett användarnamn som har beviljats administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="7acf1-129">**User name** - Enter a user name that is granted administrator privileges.</span></span>
    * <span data-ttu-id="7acf1-130">**Lösenordet** – ange - eller välj hello listrutan - ett-värde som är associerad med hello hemlighet (lösenord) som du vill toouse för hello angiven användare.</span><span class="sxs-lookup"><span data-stu-id="7acf1-130">**Password** - Enter - or select from hello dropdown - a value that is associated with hello secret (password) that you want toouse for hello specified user.</span></span> <span data-ttu-id="7acf1-131">Läs mer om hello hemligheter [Azure DevTest Labs: hemliga datorarkivet](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span><span class="sxs-lookup"><span data-stu-id="7acf1-131">For more information about hello secrets, see [Azure DevTest Labs: Personal secret store](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store/).</span></span>
    * <span data-ttu-id="7acf1-132">**Typ av virtuell dator disk** – ange antingen Hårddisk (hårddiskenheten) eller SSD (SSD) tooindicate vilka lagring disktyp tillåts för hello virtuella datorer som etablerats med hjälp av den här grundläggande bild.</span><span class="sxs-lookup"><span data-stu-id="7acf1-132">**Virtual machine disk type** - Specify either HDD (hard-disk drive) or SSD (solid-state drive) tooindicate which storage disk type is allowed for hello virtual machines provisioned using this base image.</span></span>
    * <span data-ttu-id="7acf1-133">** Virtuella storlek ** – Välj något av hello fördefinierade objekt som anger hello processorkärnor, ram-storlek och hello hårddiskstorlek av hello VM toocreate.</span><span class="sxs-lookup"><span data-stu-id="7acf1-133">** Virtual machine size** - Select one of hello predefined items that specify hello processor cores, RAM size, and hello hard drive size of hello VM toocreate.</span></span> 
    * <span data-ttu-id="7acf1-134">**Artefakter** -väljer tooopen hello **lägga till artefakter** bladet, där du kan välja och konfigurera hello artefakter som du vill tooadd toohello basavbildning.</span><span class="sxs-lookup"><span data-stu-id="7acf1-134">**Artifacts** - Select tooopen hello **Add artifacts** blade, in which you select and configure hello artifacts that you want tooadd toohello base image.</span></span> <span data-ttu-id="7acf1-135">Läs mer om artefakter [hantera VM artefakter i Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="7acf1-135">For more information about artifacts, see [Manage VM artifacts in Azure DevTest Labs](./devtest-lab-add-vm-with-artifacts.md).</span></span>
    * <span data-ttu-id="7acf1-136">**Avancerade inställningar** -väljer tooopen hello **Avancerat** bladet där du konfigurerar hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="7acf1-136">**Advanced settings** - Select tooopen hello **Advanced** blade where you configure hello following settings:</span></span>
        * <span data-ttu-id="7acf1-137">**Virtuellt nätverk** -ange hello önskas virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7acf1-137">**Virtual network** - Specify hello desired virtual network.</span></span>
        * <span data-ttu-id="7acf1-138">**Undernät** -ange hello önskad undernät.</span><span class="sxs-lookup"><span data-stu-id="7acf1-138">**Subnet** - Specify hello desired subnet.</span></span>    
        * <span data-ttu-id="7acf1-139">**IP-adresskonfiguration** -ange om du vill hello Public, Private eller delade IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="7acf1-139">**IP address configuration** - Specify if you want hello Public, Private, or Shared IP addresses.</span></span> <span data-ttu-id="7acf1-140">Mer information om delade IP-adresser finns [Förstå delade IP-adresser i Azure DevTest Labs](./devtest-lab-shared-ip.md).</span><span class="sxs-lookup"><span data-stu-id="7acf1-140">For more information about shared IP addresses, see [Understand shared IP addresses in Azure DevTest Labs](./devtest-lab-shared-ip.md).</span></span>
        * <span data-ttu-id="7acf1-141">**Se den här datorn claimable** -gör en dator ”claimable” innebär att den inte tilldelas ägarskap för närvarande hello skapas.</span><span class="sxs-lookup"><span data-stu-id="7acf1-141">**Make this machine claimable** - Making a machine "claimable" means that it will not be assigned ownership at hello time of creation.</span></span> <span data-ttu-id="7acf1-142">I stället aktiveras lab kan tootake ägarskap (”anspråk”) hello datorn hello labb-bladet.</span><span class="sxs-lookup"><span data-stu-id="7acf1-142">Instead lab users will be able tootake ownership ("claim") hello machine in hello lab's blade.</span></span>     
    * <span data-ttu-id="7acf1-143">**Bild** -fältet visar namnet på hello basavbildning som du har valt på hello föregående blad.</span><span class="sxs-lookup"><span data-stu-id="7acf1-143">**Image** - This field displays name of hello base image you selected on hello previous blade.</span></span> 
     
       ![Skapa formeln](./media/devtest-lab-create-formulas/create-formula.png)

8. <span data-ttu-id="7acf1-145">Välj **skapa** toocreate hello formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-145">Select **Create** toocreate hello formula.</span></span>

9. <span data-ttu-id="7acf1-146">När hello formeln har skapats visas i listan hello på hello **formler** bladet.</span><span class="sxs-lookup"><span data-stu-id="7acf1-146">When hello formula has been created, it displays in hello list on hello **Formulas** blade.</span></span>

### <a name="create-a-formula-from-a-vm"></a><span data-ttu-id="7acf1-147">Skapa en formel från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="7acf1-147">Create a formula from a VM</span></span>
<span data-ttu-id="7acf1-148">hello leder följande steg dig igenom hello processen att skapa en formel som baseras på en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7acf1-148">hello following steps guide you through hello process of creating a formula based on an existing VM.</span></span> 

> [!NOTE]
> <span data-ttu-id="7acf1-149">toocreate en formel från en virtuell dator, hello VM måste ha skapats efter den 30 mars 2016.</span><span class="sxs-lookup"><span data-stu-id="7acf1-149">toocreate a formula from a VM, hello VM must have been created after March 30, 2016.</span></span> 
> 
> 

1. <span data-ttu-id="7acf1-150">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7acf1-150">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="7acf1-151">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7acf1-151">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="7acf1-152">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="7acf1-152">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="7acf1-153">På hello lab **översikt** bladet välj hello VM som du vill toocreate hello formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-153">On hello lab's **Overview** blade, select hello VM from which you wish toocreate hello formula.</span></span>
   
    ![Labs virtuella datorer](./media/devtest-lab-create-formulas/my-vms.png)
5. <span data-ttu-id="7acf1-155">På bladet hello VM väljer **skapa formeln (återanvändbara base)**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-155">On hello VM's blade, select **Create formula (reusable base)**.</span></span>
   
    ![Skapa formeln](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. <span data-ttu-id="7acf1-157">På hello **skapa formeln** bladet anger du en **namn** och **beskrivning** för din nya formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-157">On hello **Create formula** blade, enter a **Name** and **Description** for your new formula.</span></span>
   
    ![Skapa formeln bladet](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. <span data-ttu-id="7acf1-159">Välj **OK** toocreate hello formel.</span><span class="sxs-lookup"><span data-stu-id="7acf1-159">Select **OK** toocreate hello formula.</span></span>

## <a name="modify-a-formula"></a><span data-ttu-id="7acf1-160">Ändra en formel</span><span class="sxs-lookup"><span data-stu-id="7acf1-160">Modify a formula</span></span>
<span data-ttu-id="7acf1-161">toomodify en formel, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="7acf1-161">toomodify a formula, follow these steps:</span></span>

1. <span data-ttu-id="7acf1-162">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7acf1-162">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="7acf1-163">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7acf1-163">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="7acf1-164">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="7acf1-164">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="7acf1-165">På bladet hello lab väljer **formler (återanvändbara baser)**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-165">On hello lab's blade, select **Formulas (reusable bases)**.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="7acf1-167">På hello **Lab formler** bladet, Välj hello formeln som du vill toomodify.</span><span class="sxs-lookup"><span data-stu-id="7acf1-167">On hello **Lab formulas** blade, select hello formula you wish toomodify.</span></span>
6. <span data-ttu-id="7acf1-168">På hello **uppdatera formel** bladet hello vill redigera och välj **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-168">On hello **Update formula** blade, make hello desired edits, and select **Update**.</span></span>

## <a name="delete-a-formula"></a><span data-ttu-id="7acf1-169">Ta bort en formel</span><span class="sxs-lookup"><span data-stu-id="7acf1-169">Delete a formula</span></span>
<span data-ttu-id="7acf1-170">toodelete en formel, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="7acf1-170">toodelete a formula, follow these steps:</span></span>

1. <span data-ttu-id="7acf1-171">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7acf1-171">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="7acf1-172">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7acf1-172">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="7acf1-173">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="7acf1-173">From hello list of labs, select hello desired lab.</span></span>  
4. <span data-ttu-id="7acf1-174">På hello lab **inställningar** bladet väljer **formler**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-174">On hello lab **Settings** blade, select **Formulas**.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. <span data-ttu-id="7acf1-176">På hello **Lab formler** bladet, Välj hello ellips toohello höger i hello formeln gärna toodelete.</span><span class="sxs-lookup"><span data-stu-id="7acf1-176">On hello **Lab formulas** blade, select hello ellipsis toohello right of hello formula you wish toodelete.</span></span>
   
    ![Formeln menyn](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. <span data-ttu-id="7acf1-178">På snabbmenyn för hello formel, väljer **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7acf1-178">On hello formula's context menu, select **Delete**.</span></span>
   
    ![Formeln snabbmenyn](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. <span data-ttu-id="7acf1-180">Välj **Ja** toohello bekräftelsedialogruta för borttagning.</span><span class="sxs-lookup"><span data-stu-id="7acf1-180">Select **Yes** toohello deletion confirmation dialog.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a><span data-ttu-id="7acf1-181">Relaterade blogginlägg</span><span class="sxs-lookup"><span data-stu-id="7acf1-181">Related blog posts</span></span>
* [<span data-ttu-id="7acf1-182">Anpassade avbildningar eller formler?</span><span class="sxs-lookup"><span data-stu-id="7acf1-182">Custom images or formulas?</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a><span data-ttu-id="7acf1-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7acf1-183">Next steps</span></span>
<span data-ttu-id="7acf1-184">När du har skapat en formel för användning när du skapar en virtuell dator, hello nästa steg är för[lägga till ett labb för VM-tooyour](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="7acf1-184">Once you have created a formula for use when creating a VM, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

