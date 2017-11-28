---
title: "Azure snabbstart – skapa virtuell Windows-dator med Portal | Microsoft Docs"
description: "Azure snabbstart – skapa virtuell Windows-dator med Portal"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 31ac18add9c3fd956e0d37b1e0c1a510265c22e6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-portal"></a><span data-ttu-id="9c0f5-103">Skapa en virtuell Windows-dator med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c0f5-103">Create a Windows virtual machine with the Azure portal</span></span>

<span data-ttu-id="9c0f5-104">Det går att skapa virtuella datorer via Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-104">Azure virtual machines can be created through the Azure portal.</span></span> <span data-ttu-id="9c0f5-105">Den här metoden ger ett webbläsarbaserat användargränssnitt för att skapa och konfigurera virtuella datorer och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-105">This method provides a browser-based user interface for creating and configuring virtual machines and all related resources.</span></span> <span data-ttu-id="9c0f5-106">I den här snabbstarten beskrivs hur du skapar en virtuell dator och installerar en webbserver på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-106">This Quickstart steps through creating a virtual machine and installing a webserver on the VM.</span></span>

<span data-ttu-id="9c0f5-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="9c0f5-108">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="9c0f5-108">Log in to Azure</span></span>

<span data-ttu-id="9c0f5-109">Logga in på Azure Portal på http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-109">Log in to the Azure portal at http://portal.azure.com.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="9c0f5-110">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9c0f5-110">Create virtual machine</span></span>

1. <span data-ttu-id="9c0f5-111">Klicka på knappen **New** (Nytt) i det övre vänstra hörnet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-111">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="9c0f5-112">Välj **Compute**, och välj sedan **Windows Server 2016 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-112">Select **Compute**, and then select **Windows Server 2016 Datacenter**.</span></span> 

3. <span data-ttu-id="9c0f5-113">Ange informationen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-113">Enter the virtual machine information.</span></span> <span data-ttu-id="9c0f5-114">Användarnamnet och lösenordet som anges här används för att logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-114">The user name and password entered here is used to log in to the virtual machine.</span></span> <span data-ttu-id="9c0f5-115">När du är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-115">When complete, click **OK**.</span></span>

    ![Ange grundläggande information om de virtuella datorerna på portalens blad](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. <span data-ttu-id="9c0f5-117">Välj en storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-117">Select a size for the VM.</span></span> <span data-ttu-id="9c0f5-118">Om du vill se fler storlekar väljer du **Visa alla** eller så ändrar du filtret för **disktyper som stöds**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-118">To see more sizes, select **View all** or change the **Supported disk type** filter.</span></span> 

    ![Skärmbild som visar storlekar på virtuella datorer](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. <span data-ttu-id="9c0f5-120">Acceptera alla standardvärden på bladet Inställningar och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-120">On the settings blade, keep the defaults and click **OK**.</span></span>

6. <span data-ttu-id="9c0f5-121">På sammanfattningssidan klickar du på **Ok** för att starta distributionen av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-121">On the summary page, click **Ok** to start the virtual machine deployment.</span></span>

7. <span data-ttu-id="9c0f5-122">Den virtuella datorn fästs på Azure Portals instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-122">The VM will be pinned to the Azure portal dashboard.</span></span> <span data-ttu-id="9c0f5-123">När distributionen är klar öppnar den virtuella datorn VM-sammanfattningsbladet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-123">Once the deployment has completed, the VM summary blade automatically opens.</span></span>


## <a name="connect-to-virtual-machine"></a><span data-ttu-id="9c0f5-124">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="9c0f5-124">Connect to virtual machine</span></span>

<span data-ttu-id="9c0f5-125">Skapa en fjärrskrivbordsanslutning till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-125">Create a remote desktop connection to the virtual machine.</span></span>

1. <span data-ttu-id="9c0f5-126">Klicka på knappen **Anslut** på den virtuella datorns egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-126">Click the **Connect** button on the virtual machine properties.</span></span> <span data-ttu-id="9c0f5-127">En protokollfil för fjärrskrivbord (.rdp-fil) skapas och hämtas.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-127">A Remote Desktop Protocol file (.rdp file) is created and downloaded.</span></span>

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. <span data-ttu-id="9c0f5-129">Öppna den hämtade RDP-filen för att ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-129">To connect to your VM, open the downloaded RDP file.</span></span> <span data-ttu-id="9c0f5-130">Om du uppmanas till detta klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-130">If prompted, click **Connect**.</span></span> <span data-ttu-id="9c0f5-131">På en Mac-dator behöver du en RDP-klient som denna [Fjärrskrivbordsklient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) från Mac App Store.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-131">On a Mac, you need an RDP client such as this [Remote Desktop Client](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) from the Mac App Store.</span></span>

3. <span data-ttu-id="9c0f5-132">Ange användarnamnet och lösenordet du angav när du skapade den virtuella datorn och klicka sedan på **Ok**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-132">Enter the user name and password you specified when creating the virtual machine, then click **Ok**.</span></span>

4. <span data-ttu-id="9c0f5-133">Du kan få en certifikatvarning under inloggningen.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-133">You may receive a certificate warning during the sign-in process.</span></span> <span data-ttu-id="9c0f5-134">Klicka på **Ja** eller **Fortsätt** för att fortsätta med anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-134">Click **Yes** or **Continue** to proceed with the connection.</span></span>


## <a name="install-iis-using-powershell"></a><span data-ttu-id="9c0f5-135">Installera IIS med PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c0f5-135">Install IIS using PowerShell</span></span>

<span data-ttu-id="9c0f5-136">Starta en PowerShell-session och kör följande kommando för att installera IIS på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-136">On the virtual machine, start a PowerShell session and run the following command to install IIS.</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="9c0f5-137">När det är klart avslutar du RDP-sessionen och återgår till VM-egenskaperna i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-137">When done, exit the RDP session and return the VM properties in the Azure portal.</span></span>

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="9c0f5-138">Öppna port 80 för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="9c0f5-138">Open port 80 for web traffic</span></span> 

<span data-ttu-id="9c0f5-139">En nätverkssäkerhetsgrupp (NSG) säkrar ingående och utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-139">A Network security group (NSG) secures inbound and outbound traffic.</span></span> <span data-ttu-id="9c0f5-140">När en VM skapas från Azure Portal skapas en regel för inkommande trafik på port 3389 för RDP-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-140">When a VM is created from the Azure portal, an inbound rule is created on port 3389 for RDP connections.</span></span> <span data-ttu-id="9c0f5-141">Eftersom denna VM är värd för en webbserver måste du skapa en NSG-regel för port 80.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-141">Because this VM hosts a webserver, an NSG rule needs to be created for port 80.</span></span>

1. <span data-ttu-id="9c0f5-142">Klicka på namnet på den virtuella datorns **resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-142">On the virtual machine, click the name of the **Resource group**.</span></span>
2. <span data-ttu-id="9c0f5-143">Välj **nätverkssäkerhetsgruppen**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-143">Select the **network security group**.</span></span> <span data-ttu-id="9c0f5-144">Du kan identifiera NSG med kolumnen **Typ**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-144">The NSG can be identified using the **Type** column.</span></span> 
3. <span data-ttu-id="9c0f5-145">På menyn till vänster under inställningarna klickar du på **Ingående säkerhetsregel**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-145">On the left-hand menu, under settings, click **Inbound security rules**.</span></span>
4. <span data-ttu-id="9c0f5-146">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-146">Click on **Add**.</span></span>
5. <span data-ttu-id="9c0f5-147">Skriv **http** i fältet **Namn**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-147">In **Name**, type **http**.</span></span> <span data-ttu-id="9c0f5-148">Kontrollera att 80 har angetts för **Portintervall** och att **Tillåt** har angetts för **Åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-148">Make sure **Port range** is set to 80 and **Action** is set to **Allow**.</span></span> 
6. <span data-ttu-id="9c0f5-149">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-149">Click **OK**.</span></span>


## <a name="view-the-iis-welcome-page"></a><span data-ttu-id="9c0f5-150">Visa välkomstsidan för IIS</span><span class="sxs-lookup"><span data-stu-id="9c0f5-150">View the IIS welcome page</span></span>

<span data-ttu-id="9c0f5-151">När IIS är installerat och port 80 är öppen för din VM kan webbservern nu nås från internet.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-151">With IIS installed, and port 80 open to your VM, the webserver can now be accessed from the internet.</span></span> <span data-ttu-id="9c0f5-152">Öppna en webbläsare och ange den virtuella datorns offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-152">Open a web browser, and enter the public IP address of the VM.</span></span> <span data-ttu-id="9c0f5-153">den offentliga IP-adressen finns på VM-bladet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-153">the public IP address can be found on the VM blade in the Azure portal.</span></span>

![Standardwebbplatsen i IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="9c0f5-155">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="9c0f5-155">Clean up resources</span></span>

<span data-ttu-id="9c0f5-156">Ta bort resursgruppen, den virtuella datorn och alla relaterade resurser när de inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-156">When no longer needed, delete the resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="9c0f5-157">Om du vill göra det väljer du resursgruppen på bladet för den virtuella datorn och klickar på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-157">To do so, select the resource group from the virtual machine blade and click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c0f5-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c0f5-158">Next steps</span></span>

<span data-ttu-id="9c0f5-159">I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-159">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="9c0f5-160">Om du vill veta mer om virtuella Azure-datorer fortsätter du till självstudien för virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="9c0f5-160">To learn more about Azure virtual machines, continue to the tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9c0f5-161">Självstudier om virtuella Azure Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="9c0f5-161">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
