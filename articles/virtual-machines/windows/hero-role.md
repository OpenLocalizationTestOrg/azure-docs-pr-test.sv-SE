---
title: "aaaInstall IIS på din första virtuella Windows-dator | Microsoft Docs"
description: "Experimentera med först virtuella Windows-dator genom att installera IIS och öppna port 80 med hello Azure-portalen."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6334ea45-db6b-4e08-abb5-1f98927ebc34
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cynthn
ms.openlocfilehash: 7cfed6197df058c4569d111ee88961da7c6fe0b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a><span data-ttu-id="fb50e-103">Experimentera med att installera en roll på Windows VM</span><span class="sxs-lookup"><span data-stu-id="fb50e-103">Experiment with installing a role on your Windows VM</span></span>
<span data-ttu-id="fb50e-104">När du har din första virtuella dator (VM) in och körs, kan du gå vidare tooinstalling program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="fb50e-104">Once you have your first virtual machine (VM) up and running, you can move on tooinstalling software and services.</span></span> <span data-ttu-id="fb50e-105">Den här självstudiekursen kommer vi toouse Serverhanteraren på hello Windows Server VM tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="fb50e-105">For this tutorial, we are going toouse Server Manager on hello Windows Server VM tooinstall IIS.</span></span> <span data-ttu-id="fb50e-106">Sedan skapar vi en Nätverkssäkerhetsgrupp (NSG) med hjälp av hello Azure portal tooopen port 80 tooIIS trafik.</span><span class="sxs-lookup"><span data-stu-id="fb50e-106">Then, we will create a Network Security Group (NSG) using hello Azure portal tooopen port 80 tooIIS traffic.</span></span> 

<span data-ttu-id="fb50e-107">Om du inte har skapat din första virtuella dator du bör gå tillbaka för[skapa din första Windows virtuell dator i hello Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du fortsätter med den här kursen.</span><span class="sxs-lookup"><span data-stu-id="fb50e-107">If you haven't already created your first VM, you should go back too[Create your first Windows virtual machine in hello Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before continuing with this tutorial.</span></span>

## <a name="make-sure-hello-vm-is-running"></a><span data-ttu-id="fb50e-108">Se till att hello virtuella datorn körs</span><span class="sxs-lookup"><span data-stu-id="fb50e-108">Make sure hello VM is running</span></span>
1. <span data-ttu-id="fb50e-109">Öppna hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fb50e-109">Open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fb50e-110">Hej hubbmenyn, klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-110">On hello hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="fb50e-111">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="fb50e-111">Select hello virtual machine from hello list.</span></span>
3. <span data-ttu-id="fb50e-112">Om hello status är **Stoppad (Deallocated)**, klicka på hello **starta** hello-knappen **Essentials** bladet för hello VM.</span><span class="sxs-lookup"><span data-stu-id="fb50e-112">If hello status is **Stopped (Deallocated)**, click hello **Start** button on hello **Essentials** blade of hello VM.</span></span> <span data-ttu-id="fb50e-113">Om hello status är **kör**, kan du flytta på toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="fb50e-113">If hello status is **Running**, you can move on toohello next step.</span></span>

## <a name="connect-toohello-virtual-machine-and-sign-in"></a><span data-ttu-id="fb50e-114">Ansluta toohello virtuella datorn och logga in</span><span class="sxs-lookup"><span data-stu-id="fb50e-114">Connect toohello virtual machine and sign in</span></span>
1. <span data-ttu-id="fb50e-115">Hej hubbmenyn, klicka på **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-115">On hello hub menu, click **Virtual Machines**.</span></span> <span data-ttu-id="fb50e-116">Välj hello virtuella hello-listan.</span><span class="sxs-lookup"><span data-stu-id="fb50e-116">Select hello virtual machine from hello list.</span></span>
2. <span data-ttu-id="fb50e-117">Klicka på hello bladet för den virtuella datorn hello **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-117">On hello blade for hello virtual machine, click **Connect**.</span></span> <span data-ttu-id="fb50e-118">Detta skapar och laddar ned en fil med Remote Desktop Protocol (RDP-fil) som fungerar som en genväg tooconnect tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="fb50e-118">This creates and downloads a Remote Desktop Protocol file (.rdp file) that is like a shortcut tooconnect tooyour machine.</span></span> <span data-ttu-id="fb50e-119">Du kanske vill toosave hello filen tooyour skrivbordet för enkel åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fb50e-119">You might want toosave hello file tooyour desktop for easy access.</span></span> <span data-ttu-id="fb50e-120">**Öppna** den här filen tooconnect tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="fb50e-120">**Open** this file tooconnect tooyour VM.</span></span>
   
    ![Skärmbild av hello Azure portal som visar hur tooconnect tooyour VM](./media/hero-role/connect.png)
3. <span data-ttu-id="fb50e-122">Du får en varning om att hello RDP-filen kommer från en okänd utgivare.</span><span class="sxs-lookup"><span data-stu-id="fb50e-122">You get a warning that hello .rdp is from an unknown publisher.</span></span> <span data-ttu-id="fb50e-123">Detta är normalt.</span><span class="sxs-lookup"><span data-stu-id="fb50e-123">This is normal.</span></span> <span data-ttu-id="fb50e-124">På hello fjärrskrivbordet i fönstret **Anslut** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="fb50e-124">In hello Remote Desktop window, click **Connect** toocontinue.</span></span>
   
    ![Skärmbild med ett varning som meddelar att utgivaren är okänd](./media/hero-role/rdp-warn.png)
4. <span data-ttu-id="fb50e-126">I fönstret säkerhet för Windows hello hello typen hello användarnamn och lösenord för lokala hello-kontot som du skapade när du skapade VM.</span><span class="sxs-lookup"><span data-stu-id="fb50e-126">In hello Windows Security window, type hello username and password for hello local account that you created when you created hello VM.</span></span> <span data-ttu-id="fb50e-127">hello användarnamnet anges som *vmname*&#92; *användarnamnet*, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-127">hello username is entered as *vmname*&#92;*username*, then click **OK**.</span></span>
   
    ![Skärmbild av att ange hello VM-namn, användarnamn och lösenord](./media/hero-role/credentials.png)
5. <span data-ttu-id="fb50e-129">Du får en varning hello certifikatet kan inte verifieras.</span><span class="sxs-lookup"><span data-stu-id="fb50e-129">You get a warning that hello certificate cannot be verified.</span></span> <span data-ttu-id="fb50e-130">Detta är normalt.</span><span class="sxs-lookup"><span data-stu-id="fb50e-130">This is normal.</span></span> <span data-ttu-id="fb50e-131">Klicka på **Ja** tooverify hello hello virtuella datorns identitet och slutföra inloggning.</span><span class="sxs-lookup"><span data-stu-id="fb50e-131">Click **Yes** tooverify hello identity of hello virtual machine and finish logging on.</span></span>
   
   ![Skärmbild som visar ett meddelande möts verifierar hello identitet hello VM](./media/hero-role/cert-warning.png)

<span data-ttu-id="fb50e-133">Om du kör i tootrouble när du försöker tooconnect, se [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-datorn](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fb50e-133">If you run in tootrouble when you try tooconnect, see [Troubleshoot Remote Desktop connections tooa Windows-based Azure Virtual Machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="install-iis-on-your-vm"></a><span data-ttu-id="fb50e-134">Installera IIS på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="fb50e-134">Install IIS on your VM</span></span>
<span data-ttu-id="fb50e-135">Nu när du är inloggad i toohello VM, kommer vi installera en serverroll så att du kan experimentera mer.</span><span class="sxs-lookup"><span data-stu-id="fb50e-135">Now that you are logged in toohello VM, we will install a server role so that you can experiment more.</span></span>

1. <span data-ttu-id="fb50e-136">Öppna **Serverhanteraren** om den inte redan är öppen.</span><span class="sxs-lookup"><span data-stu-id="fb50e-136">Open **Server Manager** if it isn't already open.</span></span> <span data-ttu-id="fb50e-137">Klicka på hello **starta** -menyn och klicka sedan på **Serverhanteraren**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-137">Click hello **Start** menu, and then click **Server Manager**.</span></span>
2. <span data-ttu-id="fb50e-138">I **Serverhanteraren**väljer **lokal Server** från hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="fb50e-138">In **Server Manager**, select **Local Server** from hello left pane.</span></span> 
3. <span data-ttu-id="fb50e-139">Välj menyn hello **hantera** > **Lägg till roller och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-139">In hello menu, select **Manage** > **Add Roles and Features**.</span></span>
4. <span data-ttu-id="fb50e-140">I hello Lägg till roller och funktioner, på hello **installationstyp** väljer **rollbaserad eller funktionsbaserad installation**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-140">In hello Add Roles and Features Wizard, on hello **Installation Type** page, choose **Role-based or feature-based installation**, and then click **Next**.</span></span>
   
    ![Skärmbild som visar hello guiden Lägg till roller och funktioner på fliken för installationstyp](./media/hero-role/role-wizard.png)
5. <span data-ttu-id="fb50e-142">Välj hello VM hello-serverpoolen och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-142">Select hello VM from hello server pool and click **Next**.</span></span>
6. <span data-ttu-id="fb50e-143">På hello **serverroller** väljer **webbserver (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-143">On hello **Server Roles** page, select **Web Server (IIS)**.</span></span>
   
    ![Skärmbild som visar hello guiden Lägg till roller och funktioner på fliken för serverroller](./media/hero-role/add-iis.png)
7. <span data-ttu-id="fb50e-145">I hello popup om att lägga till funktioner som behövs för IIS, se till att **ta med hanteringsverktyg** är markerad och klicka sedan på **Lägg till funktioner**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-145">In hello pop-up about adding features needed for IIS, make sure that **Include management tools** is selected and then click **Add Features**.</span></span> <span data-ttu-id="fb50e-146">När popup-hello stängs klickar du på **nästa** i hello guiden.</span><span class="sxs-lookup"><span data-stu-id="fb50e-146">When hello pop-up closes, click **Next** in hello wizard.</span></span>
   
    ![Skärmbild som visar popup-tooconfirm lägger till hello IIS-rollen](./media/hero-role/confirm-add-feature.png)
8. <span data-ttu-id="fb50e-148">På hello funktioner klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-148">On hello features page, click **Next**.</span></span>
9. <span data-ttu-id="fb50e-149">På hello **webbserverroll (IIS)** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-149">On hello **Web Server Role (IIS)** page, click **Next**.</span></span> 
10. <span data-ttu-id="fb50e-150">På hello **rolltjänster** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-150">On hello **Role Services** page, click **Next**.</span></span> 
11. <span data-ttu-id="fb50e-151">På hello **bekräftelse** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-151">On hello **Confirmation** page, click **Install**.</span></span> 
12. <span data-ttu-id="fb50e-152">När hello installationen är klar klickar du på **Stäng** för hello guiden.</span><span class="sxs-lookup"><span data-stu-id="fb50e-152">When hello installation is complete, click **Close** on hello wizard.</span></span>

## <a name="open-port-80"></a><span data-ttu-id="fb50e-153">Öppna port 80</span><span class="sxs-lookup"><span data-stu-id="fb50e-153">Open port 80</span></span>
<span data-ttu-id="fb50e-154">Inkommande trafik via port 80 för VM-tooaccept måste du tooadd en inkommande regel toohello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="fb50e-154">In order for your VM tooaccept inbound traffic over port 80, you need tooadd an inbound rule toohello network security group.</span></span> 

1. <span data-ttu-id="fb50e-155">Öppna hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fb50e-155">Open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fb50e-156">I **virtuella datorer** Välj hello VM som du skapade.</span><span class="sxs-lookup"><span data-stu-id="fb50e-156">In **Virtual machines** select hello VM that you created.</span></span>
3. <span data-ttu-id="fb50e-157">Markera hello inställningar för virtuella datorer **nätverksgränssnitt** och sedan väljer hello befintliga nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="fb50e-157">In hello virtual machines settings, select **Network interfaces** and then select hello existing network interface.</span></span>
   
    ![Skärmbild som visar hello virtuella datorns inställning för hello nätverksgränssnitt](./media/hero-role/network-interface.png)
4. <span data-ttu-id="fb50e-159">I **Essentials** hello nätverksgränssnitt, klickar du på hello **nätverkssäkerhetsgruppen**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-159">In **Essentials** for hello network interface, click hello **Network security group**.</span></span>
   
    ![Skärmbild som visar hello Essentials avsnittet för hello nätverksgränssnittet](./media/hero-role/select-nsg.png)
5. <span data-ttu-id="fb50e-161">I hello **Essentials** bladet för hello NSG bör du ha en befintlig standard för inkommande trafik för **standard Tillåt rdp** vilket gör att du toolog i toohello VM.</span><span class="sxs-lookup"><span data-stu-id="fb50e-161">In hello **Essentials** blade for hello NSG, you should have one existing default inbound rule for **default-allow-rdp** which allows you toolog in toohello VM.</span></span> <span data-ttu-id="fb50e-162">Du lägger till en annan regel för inkommande trafik tooallow IIS-trafik.</span><span class="sxs-lookup"><span data-stu-id="fb50e-162">You will add another inbound rule tooallow IIS traffic.</span></span> <span data-ttu-id="fb50e-163">Klicka på **Ingående säkerhetsregel**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-163">Click **Inbound security rule**.</span></span>
   
    ![Skärmbild som visar hello Essentials avsnittet hello NSG](./media/hero-role/inbound.png)
6. <span data-ttu-id="fb50e-165">Klicka på **Lägg till** i **Ingående säkerhetsregler**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-165">In **Inbound security rules**, click **Add**.</span></span>
   
    ![Skärmbild som visar hello knappen tooadd en säkerhetsregel](./media/hero-role/add-rule.png)
7. <span data-ttu-id="fb50e-167">Klicka på **Lägg till** i **Ingående säkerhetsregler**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-167">In **Inbound security rules**, click **Add**.</span></span> <span data-ttu-id="fb50e-168">Typen **80** i hello portintervall och se till att **Tillåt** är markerad.</span><span class="sxs-lookup"><span data-stu-id="fb50e-168">Type **80** in hello port range and make sure **Allow** is selected.</span></span> <span data-ttu-id="fb50e-169">Klicka på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="fb50e-169">When you are done, click **OK**.</span></span>
   
    ![Skärmbild som visar hello knappen tooadd en säkerhetsregel](./media/hero-role/port-80.png)

<span data-ttu-id="fb50e-171">Mer information om NSG: er, regler för inkommande och utgående, finns [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="fb50e-171">For more information about NSGs, inbound and outbound rules, see [Allow external access tooyour VM using hello Azure portal](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="connect-toohello-default-iis-website"></a><span data-ttu-id="fb50e-172">Ansluta toohello IIS-standardwebbplatsen</span><span class="sxs-lookup"><span data-stu-id="fb50e-172">Connect toohello default IIS website</span></span>
1. <span data-ttu-id="fb50e-173">I hello Azure-portalen klickar du på **virtuella datorer** och välj sedan den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fb50e-173">In hello Azure portal, click **Virtual machines** and then select your VM.</span></span>
2. <span data-ttu-id="fb50e-174">I hello **Essentials** bladet, kopiera ditt **offentliga IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="fb50e-174">In hello **Essentials** blade, copy your **Public IP address**.</span></span>
   
    ![Skärmbild som visar där toofind hello offentliga IP-adressen för den virtuella datorn](./media/hero-role/ipaddress.png)
3. <span data-ttu-id="fb50e-176">Öppna en webbläsare och i hello adressfältet skriver du in din offentliga IP-adress så här: http://<publicIPaddress> och på **RETUR** toogo toothat adress.</span><span class="sxs-lookup"><span data-stu-id="fb50e-176">Open a browser and in hello address bar, type in your public IP address like this: http://<publicIPaddress> and click **Enter** toogo toothat address.</span></span>
4. <span data-ttu-id="fb50e-177">Din webbläsare ska öppna hello standardwebbsidan för IIS.</span><span class="sxs-lookup"><span data-stu-id="fb50e-177">Your browser should open hello default IIS web page.</span></span> <span data-ttu-id="fb50e-178">Det ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="fb50e-178">It looks something like this:</span></span>
   
    ![Skärmbild som visar vilka IIS standardsida hello ser ut i en webbläsare](./media/hero-role/iis-default.png)

## <a name="next-steps"></a><span data-ttu-id="fb50e-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb50e-180">Next steps</span></span>
* <span data-ttu-id="fb50e-181">Du kan också experimentera med [bifogar en datadisk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fb50e-181">You can also experiment with [attaching a data disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtual machine.</span></span> <span data-ttu-id="fb50e-182">Hårddiskar ökar den virtuella datorns lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="fb50e-182">Data disks provide more storage for your virtual machine.</span></span>

