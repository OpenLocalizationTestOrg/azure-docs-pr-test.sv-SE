---
title: "Testmiljö för LOB-program | Microsoft Docs"
description: "Lär dig hur du skapar en webbaserad, driftsapplikationer i en hybridmiljö molnet för IT pro eller utveckling testning."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 92d2d8ce-60ed-4512-95e5-a7fe3b0ca00b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: 8e9a380458bfede80ed0fc1ee7e62b0caec09501
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a><span data-ttu-id="c6d49-103">Ställ in ett webbaserat LOB-program i ett hybridmoln för testning</span><span class="sxs-lookup"><span data-stu-id="c6d49-103">Set up a web-based LOB application in a hybrid cloud for testing</span></span>
<span data-ttu-id="c6d49-104">Det här avsnittet vägleder dig genom att skapa en simulerad hybrid cloud-miljö för att testa en webbaserad driftsapplikationer (LOB) finns i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c6d49-104">This topic steps you through creating a simulated hybrid cloud environment for testing a web-based line of business (LOB) application hosted in Microsoft Azure.</span></span> <span data-ttu-id="c6d49-105">Det här är den resulterande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="c6d49-105">Here is the resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="c6d49-106">Den här konfigurationen består av:</span><span class="sxs-lookup"><span data-stu-id="c6d49-106">This configuration consists of:</span></span>

* <span data-ttu-id="c6d49-107">En simulerad lokala nätverk finns i Azure (testlabb VNet).</span><span class="sxs-lookup"><span data-stu-id="c6d49-107">A simulated on-premises network hosted in Azure (the TestLab VNet).</span></span>
* <span data-ttu-id="c6d49-108">Ett virtuellt nätverk på mellan platser som finns i Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="c6d49-108">A cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="c6d49-109">En VNet-till-VNet-VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c6d49-109">A VNet-to-VNet VPN connection.</span></span>
* <span data-ttu-id="c6d49-110">En webbaserad LOB-server, SQLServer och sekundär domänkontrollant i TestVNET virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="c6d49-110">A web-based LOB server, SQL server, and secondary domain controller in the TestVNET virtual network.</span></span>

<span data-ttu-id="c6d49-111">Den här konfigurationen ger en grund och vanliga startpunkten från vilken du kan:</span><span class="sxs-lookup"><span data-stu-id="c6d49-111">This configuration provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="c6d49-112">Utveckla och testa LOB-program som finns på Internet Information Services (IIS) med en SQL Server 2014 databasen serverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="c6d49-112">Develop and test LOB applications hosted on Internet Information Services (IIS) with a SQL Server 2014 database backend in Azure.</span></span>
* <span data-ttu-id="c6d49-113">Utföra tester för simulerad hybrid molnbaserade IT arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="c6d49-113">Perform testing of this simulated hybrid cloud-based IT workload.</span></span>

<span data-ttu-id="c6d49-114">Det finns tre huvudsakliga steg för att skapa den här testmiljö för hybrid cloud:</span><span class="sxs-lookup"><span data-stu-id="c6d49-114">There are three major phases to setting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="c6d49-115">Ställ in simulerade hybrid cloud-miljö.</span><span class="sxs-lookup"><span data-stu-id="c6d49-115">Set up the simulated hybrid cloud environment.</span></span>
2. <span data-ttu-id="c6d49-116">Konfigurera SQL-serverdator (SQL1).</span><span class="sxs-lookup"><span data-stu-id="c6d49-116">Configure the SQL server computer (SQL1).</span></span>
3. <span data-ttu-id="c6d49-117">Konfigurera LOB-server (LOB1).</span><span class="sxs-lookup"><span data-stu-id="c6d49-117">Configure the LOB server (LOB1).</span></span>

<span data-ttu-id="c6d49-118">Arbetsbelastningen kräver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c6d49-118">This workload requires an Azure subscription.</span></span> <span data-ttu-id="c6d49-119">Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="c6d49-119">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

<span data-ttu-id="c6d49-120">Ett exempel på en produktion LOB-program finns i Azure finns i **affärsprogram** arkitektur modell på [Microsoft Software Arkitekturdiagram och ritningar](http://msdn.microsoft.com/dn630664).</span><span class="sxs-lookup"><span data-stu-id="c6d49-120">For an example of a production LOB application hosted in Azure, see the **Line of business applications** architecture blueprint at [Microsoft Software Architecture Diagrams and Blueprints](http://msdn.microsoft.com/dn630664).</span></span>

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a><span data-ttu-id="c6d49-121">Fas 1: Konfigurera en simulerad hybrid cloud-miljö</span><span class="sxs-lookup"><span data-stu-id="c6d49-121">Phase 1: Set up the simulated hybrid cloud environment</span></span>
<span data-ttu-id="c6d49-122">Skapa den [simulerade hybrid cloud testmiljö](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c6d49-122">Create the [simulated hybrid cloud test environment](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="c6d49-123">Eftersom den här testmiljö inte kräver förekomsten av APP1-servern i Corpnet-undernät, kan du stänga den nu.</span><span class="sxs-lookup"><span data-stu-id="c6d49-123">Because this test environment does not require the presence of the APP1 server on the Corpnet subnet, you can shut it down for now.</span></span>

<span data-ttu-id="c6d49-124">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c6d49-124">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a><span data-ttu-id="c6d49-125">Fas 2: Konfigurera SQL-serverdator (SQL1)</span><span class="sxs-lookup"><span data-stu-id="c6d49-125">Phase 2: Configure the SQL server computer (SQL1)</span></span>
<span data-ttu-id="c6d49-126">Starta datorn DC2 från Azure portal, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="c6d49-126">From the Azure portal, start the DC2 computer if needed.</span></span>

<span data-ttu-id="c6d49-127">Skapa en virtuell dator för SQL1 med följande kommandon i en Azure PowerShell-kommandotolk på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c6d49-127">Next, create a virtual machine for SQL1 with these commands at an Azure PowerShell command prompt on your local computer.</span></span> <span data-ttu-id="c6d49-128">Innan du kör kommandona, Fyll i variabelvärdena och ta bort den < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="c6d49-128">Prior to running these commands, fill in the variable values and remove the < and > characters.</span></span>

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="c6d49-129">Använda Azure portal för att ansluta till SQL1 med det lokala administratörskontot för SQL1.</span><span class="sxs-lookup"><span data-stu-id="c6d49-129">Use the Azure portal to connect to SQL1 using the local administrator account of SQL1.</span></span>

<span data-ttu-id="c6d49-130">Konfigurera regler för Windows-brandväggen att tillåta grundläggande anslutningsmöjligheter testning och SQL Server-trafik.</span><span class="sxs-lookup"><span data-stu-id="c6d49-130">Next, configure Windows Firewall rules to allow basic connectivity testing and SQL Server traffic.</span></span> <span data-ttu-id="c6d49-131">Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på SQL1.</span><span class="sxs-lookup"><span data-stu-id="c6d49-131">From an administrator-level Windows PowerShell command prompt on SQL1, run these commands.</span></span>

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="c6d49-132">Ping-kommandot ska resultera i fyra svar från IP-adressen 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="c6d49-132">The ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="c6d49-133">Lägg sedan till datadisken extra på SQL1 som en ny volym med enhetsbokstaven F:.</span><span class="sxs-lookup"><span data-stu-id="c6d49-133">Next, add the extra data disk on SQL1 as a new volume with the drive letter F:.</span></span>

1. <span data-ttu-id="c6d49-134">I den vänstra rutan i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-134">In the left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="c6d49-135">I innehållsrutan i den **diskar** klickar du på **disk 2** (med den **Partition** inställd på **okänd**).</span><span class="sxs-lookup"><span data-stu-id="c6d49-135">In the contents pane, in the **Disks** group, click **disk 2** (with the **Partition** set to **Unknown**).</span></span>
3. <span data-ttu-id="c6d49-136">Klicka på **uppgifter**, och klicka sedan på **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-136">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="c6d49-137">På sidan innan du börjar i guiden Ny volym, klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-137">On the Before you begin page of the New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="c6d49-138">Välj server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-138">On the Select the server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="c6d49-139">När du uppmanas, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-139">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="c6d49-140">Klicka på Ange storleken på volymen sidan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-140">On the Specify the size of the volume page, click **Next**.</span></span>
7. <span data-ttu-id="c6d49-141">Tilldela till en enhet enhetsbeteckning eller mapp, klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-141">On the Assign to a drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="c6d49-142">På sidan Välj fil system inställningar **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-142">On the Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="c6d49-143">På sidan Bekräfta val **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-143">On the Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="c6d49-144">När du är klar klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-144">When complete, click **Close**.</span></span>

<span data-ttu-id="c6d49-145">Kör följande kommandon i Kommandotolken för Windows PowerShell på SQL1:</span><span class="sxs-lookup"><span data-stu-id="c6d49-145">Run these commands at the Windows PowerShell command prompt on SQL1:</span></span>

    md f:\Data
    md f:\Log
    md f:\Backup

<span data-ttu-id="c6d49-146">Därefter ansluta SQL1 till domänen CORP Windows Server Active Directory med följande kommandon i Windows PowerShell-Kommandotolken på SQL1.</span><span class="sxs-lookup"><span data-stu-id="c6d49-146">Next, join SQL1 to the CORP Windows Server Active Directory domain with these commands at the Windows PowerShell prompt on SQL1.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="c6d49-147">Använd kontot corp\användare1 när du uppmanas för att ange autentiseringsuppgifter för domänkontot för den **Add-Computer** kommando.</span><span class="sxs-lookup"><span data-stu-id="c6d49-147">Use the CORP\User1 account when prompted to supply domain account credentials for the **Add-Computer** command.</span></span>

<span data-ttu-id="c6d49-148">Efter omstart, använder du den Azure-portalen för att ansluta till SQL1 *med det lokala administratörskontot för SQL1*.</span><span class="sxs-lookup"><span data-stu-id="c6d49-148">After restarting, use the Azure portal to connect to SQL1 *with the local administrator account of SQL1*.</span></span>

<span data-ttu-id="c6d49-149">Konfigurera SQL Server 2014 om du vill använda enheten F: för nya databaser och behörigheter för kontot.</span><span class="sxs-lookup"><span data-stu-id="c6d49-149">Next, configure SQL Server 2014 to use the F: drive for new databases and for user account permissions.</span></span>

1. <span data-ttu-id="c6d49-150">Ange från startskärmen **SQL Server Management**, och klicka sedan på **SQL Server 2014 Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-150">From the Start screen, type **SQL Server Management**, and then click **SQL Server 2014 Management Studio**.</span></span>
2. <span data-ttu-id="c6d49-151">I **Anslut till Server**, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-151">In **Connect to Server**, click **Connect**.</span></span>
3. <span data-ttu-id="c6d49-152">Högerklicka i fönstret Object Explorer trädet **SQL1**, och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-152">In the Object Explorer tree pane, right-click **SQL1**, and then click **Properties**.</span></span>
4. <span data-ttu-id="c6d49-153">I den **serveregenskaper** -fönstret klickar du på **databasinställningar**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-153">In the **Server Properties** window, click **Database Settings**.</span></span>
5. <span data-ttu-id="c6d49-154">Leta upp den **databasen standardplatserna** och ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="c6d49-154">Locate the **Database default locations** and set these values:</span></span> 
   * <span data-ttu-id="c6d49-155">För **Data**, anger du sökvägen **f:\Data**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-155">For **Data**, type the path **f:\Data**.</span></span>
   * <span data-ttu-id="c6d49-156">För **loggen**, anger du sökvägen **f:\Log**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-156">For **Log**, type the path **f:\Log**.</span></span>
   * <span data-ttu-id="c6d49-157">För **säkerhetskopiering**, anger du sökvägen **f:\Backup**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-157">For **Backup**, type the path **f:\Backup**.</span></span>
   * <span data-ttu-id="c6d49-158">Obs: Endast nya databaser använda dessa platser.</span><span class="sxs-lookup"><span data-stu-id="c6d49-158">Note: Only new databases use these locations.</span></span>
6. <span data-ttu-id="c6d49-159">Klicka på den **OK** att stänga fönstret.</span><span class="sxs-lookup"><span data-stu-id="c6d49-159">Click the **OK** to close the window.</span></span>
7. <span data-ttu-id="c6d49-160">I den **Object Explorer** trädfönster öppna **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-160">In the **Object Explorer** tree pane, open **Security**.</span></span>
8. <span data-ttu-id="c6d49-161">Högerklicka på **inloggningar** och klicka sedan på **ny inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-161">Right-click **Logins** and then click **New Login**.</span></span>
9. <span data-ttu-id="c6d49-162">I **inloggningsnamnet**, typen **corp\användare1**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-162">In **Login name**, type **CORP\User1**.</span></span>
10. <span data-ttu-id="c6d49-163">På den **serverroller** klickar du på **sysadmin**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-163">On the **Server Roles** page, click **sysadmin**, and then click **OK**.</span></span>
11. <span data-ttu-id="c6d49-164">Stäng Microsoft SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="c6d49-164">Close Microsoft SQL Server Management Studio.</span></span>

<span data-ttu-id="c6d49-165">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c6d49-165">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a><span data-ttu-id="c6d49-166">Fas 3: Konfigurera LOB-servern (LOB1)</span><span class="sxs-lookup"><span data-stu-id="c6d49-166">Phase 3: Configure the LOB server (LOB1)</span></span>
<span data-ttu-id="c6d49-167">Först skapa en virtuell dator för LOB1 med följande kommandon i Azure PowerShell-Kommandotolken på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c6d49-167">First, create a virtual machine for LOB1 with these commands at the Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="c6d49-168">Använd sedan Azure-portalen för att ansluta till LOB1 med autentiseringsuppgifterna för det lokala administratörskontot för LOB1.</span><span class="sxs-lookup"><span data-stu-id="c6d49-168">Next, use the Azure portal to connect to LOB1 with the credentials of the local administrator account of LOB1.</span></span>

<span data-ttu-id="c6d49-169">Konfigurera en regel för Windows-brandväggen tillåter trafik för att testa grundläggande anslutningsmöjligheter.</span><span class="sxs-lookup"><span data-stu-id="c6d49-169">Next, configure a Windows Firewall rule to allow traffic for basic connectivity testing.</span></span> <span data-ttu-id="c6d49-170">Köra dessa kommandon från en behörighet på Windows PowerShell-Kommandotolken på LOB1.</span><span class="sxs-lookup"><span data-stu-id="c6d49-170">From an administrator-level Windows PowerShell command prompt on LOB1, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="c6d49-171">Ping-kommandot ska resultera i fyra svar från IP-adressen 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="c6d49-171">The ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="c6d49-172">Därefter ansluta LOB1 CORP Active Directory-domän med följande kommandon i Windows PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c6d49-172">Next, join LOB1 to the CORP Active Directory domain with these commands at the Windows PowerShell prompt.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="c6d49-173">Använd kontot corp\användare1 när du uppmanas för att ange autentiseringsuppgifter för domänkontot för den **Add-Computer** kommando.</span><span class="sxs-lookup"><span data-stu-id="c6d49-173">Use the CORP\User1 account when prompted to supply domain account credentials for the **Add-Computer** command.</span></span>

<span data-ttu-id="c6d49-174">Efter omstart att använda Azure-portalen för att ansluta till LOB1 med CORP\User1-konto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="c6d49-174">After restarting, use the Azure portal to connect to LOB1 with the CORP\User1 account and password.</span></span>

<span data-ttu-id="c6d49-175">Därefter konfigurera LOB1 för IIS och testa åtkomst från KLIENT1.</span><span class="sxs-lookup"><span data-stu-id="c6d49-175">Next, configure LOB1 for IIS and test access from CLIENT1.</span></span>

1. <span data-ttu-id="c6d49-176">Från Serverhanteraren klickar du på **Lägg till roller och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-176">From Server Manager, click **Add roles and features**.</span></span>
2. <span data-ttu-id="c6d49-177">På den **innan du börjar** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-177">On the **Before you begin** page, click **Next**.</span></span>
3. <span data-ttu-id="c6d49-178">På den **Välj installationstyp** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-178">On the **Select installation type** page, click **Next**.</span></span>
4. <span data-ttu-id="c6d49-179">På den **väljer målservern** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-179">On the **Select destination server** page, click **Next**.</span></span>
5. <span data-ttu-id="c6d49-180">På den **serverroller** klickar du på **webbserver (IIS)** i listan över **roller**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-180">On the **Server roles** page, click **Web Server (IIS)** in the list of **Roles**.</span></span>
6. <span data-ttu-id="c6d49-181">När du uppmanas, klickar du på **Lägg till funktioner**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-181">When prompted, click **Add Features**, and then click **Next**.</span></span>
7. <span data-ttu-id="c6d49-182">På den **Välj funktioner** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-182">On the **Select features** page, click **Next**.</span></span>
8. <span data-ttu-id="c6d49-183">På den **webbserver (IIS)** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-183">On the **Web Server (IIS)** page, click **Next**.</span></span>
9. <span data-ttu-id="c6d49-184">På den **Välj rolltjänster** sidan, markera eller avmarkera kryssrutorna för de tjänster du behöver för att testa LOB-program och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-184">On the **Select role services** page, select or clear the check boxes for the services you need for testing your LOB application, and then click **Next**.</span></span>
10. <span data-ttu-id="c6d49-185">På den **Bekräfta installationsinställningarna** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-185">On the **Confirm installation selections** page, click **Install**.</span></span>
11. <span data-ttu-id="c6d49-186">Vänta tills installationen av komponenterna är klar och klicka sedan på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="c6d49-186">Wait until the installation of components has completed and then click **Close**.</span></span>
12. <span data-ttu-id="c6d49-187">Anslut till CLIENT1-datorn med autentiseringsuppgifter för corp\användare1 konto från Azure-portalen och starta om Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c6d49-187">From the Azure portal, connect to the CLIENT1 computer with the CORP\User1 account credentials, and then start Internet Explorer.</span></span>
13. <span data-ttu-id="c6d49-188">I adressfältet skriver **http://lob1/** och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="c6d49-188">In the Address bar, type **http://lob1/** and then press ENTER.</span></span> <span data-ttu-id="c6d49-189">Du bör se IIS 8 standardwebbsidan.</span><span class="sxs-lookup"><span data-stu-id="c6d49-189">You should see the default IIS 8 web page.</span></span>

<span data-ttu-id="c6d49-190">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c6d49-190">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="c6d49-191">Den här miljön är nu redo att distribuera din webbaserade program på LOB1 och testa funktioner från KLIENT1 på undernätet Corpnet.</span><span class="sxs-lookup"><span data-stu-id="c6d49-191">This environment is now ready for you to deploy your web-based application on LOB1 and test functionality from CLIENT1 on the Corpnet subnet.</span></span>

## <a name="next-step"></a><span data-ttu-id="c6d49-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6d49-192">Next step</span></span>
* <span data-ttu-id="c6d49-193">Lägg till en ny virtuell dator med hjälp av [Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c6d49-193">Add a new virtual machine using the [Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

