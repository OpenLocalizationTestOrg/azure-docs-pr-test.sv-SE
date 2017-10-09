1. <span data-ttu-id="7fc27-101">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7fc27-101">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="7fc27-102">Från och med hello övre vänstra hörnet, klickar du på **New > Compute > Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="7fc27-102">Starting in hello upper left, click **New > Compute > Windows Server 2016 Datacenter**.</span></span>

    ![Navigera toohello Azure VM-avbildningar i hello-portalen](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. <span data-ttu-id="7fc27-104">Välj hello klassiska distributionsmodellen på hello Windows Server 2016 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="7fc27-104">On hello Windows Server 2016 Datacenter, select hello Classic deployment model.</span></span> <span data-ttu-id="7fc27-105">Klicka på Skapa.</span><span class="sxs-lookup"><span data-stu-id="7fc27-105">Click Create.</span></span>

    ![Skärmbild som visar hello Azure VM-avbildningar som är tillgänglig i hello-portalen](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a><span data-ttu-id="7fc27-107">1. Bladet Grundläggande inställningar</span><span class="sxs-lookup"><span data-stu-id="7fc27-107">1. Basics blade</span></span>

<span data-ttu-id="7fc27-108">hello grunderna bladet begär hello virtuella datorns administrativ information.</span><span class="sxs-lookup"><span data-stu-id="7fc27-108">hello Basics blade requests administrative information for hello virtual machine.</span></span>

1. <span data-ttu-id="7fc27-109">Ange en **namn** för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7fc27-109">Enter a **Name** for hello virtual machine.</span></span> <span data-ttu-id="7fc27-110">I exemplet hello _HeroVM_ är hello hello virtuella datorns namn.</span><span class="sxs-lookup"><span data-stu-id="7fc27-110">In hello example, _HeroVM_ is hello name of hello virtual machine.</span></span> <span data-ttu-id="7fc27-111">hello namn måste vara 1 och 15 tecken långt och det får inte innehålla specialtecken.</span><span class="sxs-lookup"><span data-stu-id="7fc27-111">hello name must be 1-15 characters long and it cannot contain special characters.</span></span>

2. <span data-ttu-id="7fc27-112">Ange en **användarnamn** och ett starkt **lösenord** som är toocreate används ett lokalt konto på hello VM.</span><span class="sxs-lookup"><span data-stu-id="7fc27-112">Enter a **User name** and a strong **Password** that are used toocreate a local account on hello VM.</span></span> <span data-ttu-id="7fc27-113">hello lokalt konto används toosign i tooand hantera hello VM.</span><span class="sxs-lookup"><span data-stu-id="7fc27-113">hello local account is used toosign in tooand manage hello VM.</span></span> <span data-ttu-id="7fc27-114">I exemplet hello _azureuser_ är hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="7fc27-114">In hello example, _azureuser_ is hello user name.</span></span>

 <span data-ttu-id="7fc27-115">hello lösenordet måste vara 8 och 123 tecken och uppfylla tre utanför hello fyra följande krav på komplexitet: en gemen, en versal, en siffra och ett specialtecken.</span><span class="sxs-lookup"><span data-stu-id="7fc27-115">hello password must be 8-123 characters long and meet three out of hello four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="7fc27-116">Läs mer om [krav för användarnamn och lösenord](../articles/virtual-machines/windows/faq.md).</span><span class="sxs-lookup"><span data-stu-id="7fc27-116">See more about [username and password requirements](../articles/virtual-machines/windows/faq.md).</span></span>

3. <span data-ttu-id="7fc27-117">Hej **prenumeration** är valfritt.</span><span class="sxs-lookup"><span data-stu-id="7fc27-117">hello **Subscription** is optional.</span></span> <span data-ttu-id="7fc27-118">En vanlig inställning är ”Betala per användning”.</span><span class="sxs-lookup"><span data-stu-id="7fc27-118">One common setting is "Pay-As-You-Go".</span></span>

4. <span data-ttu-id="7fc27-119">Välj en befintlig **resursgruppen** eller hello-typnamn för en ny.</span><span class="sxs-lookup"><span data-stu-id="7fc27-119">Select an existing **Resource group** or type hello name for a new one.</span></span> <span data-ttu-id="7fc27-120">I exemplet hello _HeroVMRG_ är hello namnet på hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7fc27-120">In hello example, _HeroVMRG_ is hello name of hello resource group.</span></span>

5. <span data-ttu-id="7fc27-121">Välj ett Azure-datacenter **plats** där du vill att hello VM toorun.</span><span class="sxs-lookup"><span data-stu-id="7fc27-121">Select an Azure datacenter **Location** where you want hello VM toorun.</span></span> <span data-ttu-id="7fc27-122">I exemplet hello **östra USA** är hello plats.</span><span class="sxs-lookup"><span data-stu-id="7fc27-122">In hello example, **East US** is hello location.</span></span>

6. <span data-ttu-id="7fc27-123">När du är klar klickar du på **nästa** toocontinue toohello nästa bladet.</span><span class="sxs-lookup"><span data-stu-id="7fc27-123">When you are done, click **Next** toocontinue toohello next blade.</span></span>

    ![Skärmbild som visar hello inställningar på bladet för hello grunderna för att konfigurera en Azure VM](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a><span data-ttu-id="7fc27-125">2. Bladet Storlek</span><span class="sxs-lookup"><span data-stu-id="7fc27-125">2. Size blade</span></span>

<span data-ttu-id="7fc27-126">hello storlek bladet identifierar hello konfigurationsinformation för hello VM och visar en lista över olika alternativ som innehåller OS, antal processorer, disktyp för lagring och uppskattade kostnaderna för månatlig användning.</span><span class="sxs-lookup"><span data-stu-id="7fc27-126">hello Size blade identifies hello configuration details of hello VM, and lists various choices that include OS, number of processors, disk storage type, and estimated monthly usage costs.</span></span>  

<span data-ttu-id="7fc27-127">Välj en VM-storlek och klicka sedan på **Välj** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="7fc27-127">Choose a VM size, and then click **Select** toocontinue.</span></span> <span data-ttu-id="7fc27-128">I det här exemplet _DS1_\__V2 Standard_ är hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="7fc27-128">In this example, _DS1_\__V2 Standard_ is hello VM size.</span></span>

  ![Skärmbild av hello storlek blad som visar hello Azure VM-storlekar som du kan välja](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a><span data-ttu-id="7fc27-130">3. Bladet Inställningar</span><span class="sxs-lookup"><span data-stu-id="7fc27-130">3. Settings blade</span></span>

<span data-ttu-id="7fc27-131">hello inställningsbladet begär alternativ för lagring och nätverk.</span><span class="sxs-lookup"><span data-stu-id="7fc27-131">hello Settings blade requests storage and network options.</span></span> <span data-ttu-id="7fc27-132">Du kan acceptera standardinställningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="7fc27-132">You can accept hello default settings.</span></span> <span data-ttu-id="7fc27-133">Azure skapar relevanta poster om det behövs.</span><span class="sxs-lookup"><span data-stu-id="7fc27-133">Azure creates appropriate entries where necessary.</span></span>

<span data-ttu-id="7fc27-134">Om du valde en VM-storlek som stöder det kan du prova Azure Premium Storage genom att välja Premium (SSD) i Disktyp.</span><span class="sxs-lookup"><span data-stu-id="7fc27-134">If you selected a virtual machine size that supports it, you can try Azure Premium Storage by selecting Premium (SSD) in Disk type.</span></span>

<span data-ttu-id="7fc27-135">När du har gjort önskade ändringar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7fc27-135">When you're done making changes, click **OK**.</span></span>

## <a name="4-summary-blade"></a><span data-ttu-id="7fc27-136">4. Bladet Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7fc27-136">4. Summary blade</span></span>

<span data-ttu-id="7fc27-137">hello sammanfattning bladet visar hello inställningarna i hello föregående blad.</span><span class="sxs-lookup"><span data-stu-id="7fc27-137">hello Summary blade lists hello settings specified in hello previous blades.</span></span> <span data-ttu-id="7fc27-138">Klicka på **OK** när du är klar toomake hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="7fc27-138">Click **OK** when you're ready toomake hello image.</span></span>

 ![Översikt över bladet rapporter med angivna inställningarna för hello virtuell dator](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

<span data-ttu-id="7fc27-140">När hello virtuell dator skapas hello portal visar hello ny virtuell dator under **alla resurser**, och visar en panel för hello virtuell dator på hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7fc27-140">After hello virtual machine is created, hello portal lists hello new virtual machine under **All resources**, and displays a tile of hello virtual machine on hello dashboard.</span></span> <span data-ttu-id="7fc27-141">hello motsvarande cloud service och storage-konto också skapas och visas.</span><span class="sxs-lookup"><span data-stu-id="7fc27-141">hello corresponding cloud service and storage account also are created and listed.</span></span> <span data-ttu-id="7fc27-142">Både hello virtuella datorn och Molntjänsten startas automatiskt och deras status visas som **kör**.</span><span class="sxs-lookup"><span data-stu-id="7fc27-142">Both hello virtual machine and cloud service are started automatically and their status is listed as **Running**.</span></span>

 ![Konfigurera VM-agenten och hello slutpunkter för hello virtuell dator](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
