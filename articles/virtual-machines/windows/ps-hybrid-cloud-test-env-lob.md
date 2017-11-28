---
title: "testmiljö för aaaLOB program | Microsoft Docs"
description: "Lär dig hur en webbaserad, toocreate affärsprogram i en hybrid cloud miljö för IT pro eller utveckling testning."
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
ms.openlocfilehash: e9f825bbb1cbeb841ee3c974ebf6721dc236c311
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a><span data-ttu-id="ed6d8-103">Ställ in ett webbaserat LOB-program i ett hybridmoln för testning</span><span class="sxs-lookup"><span data-stu-id="ed6d8-103">Set up a web-based LOB application in a hybrid cloud for testing</span></span>
<span data-ttu-id="ed6d8-104">Det här avsnittet vägleder dig genom att skapa en simulerad hybrid cloud-miljö för att testa en webbaserad driftsapplikationer (LOB) finns i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-104">This topic steps you through creating a simulated hybrid cloud environment for testing a web-based line of business (LOB) application hosted in Microsoft Azure.</span></span> <span data-ttu-id="ed6d8-105">Här är hello resulterande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-105">Here is hello resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="ed6d8-106">Den här konfigurationen består av:</span><span class="sxs-lookup"><span data-stu-id="ed6d8-106">This configuration consists of:</span></span>

* <span data-ttu-id="ed6d8-107">En simulerad lokala nätverk finns i Azure (hello testlabb VNet).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-107">A simulated on-premises network hosted in Azure (hello TestLab VNet).</span></span>
* <span data-ttu-id="ed6d8-108">Ett virtuellt nätverk på mellan platser som finns i Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-108">A cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="ed6d8-109">En VNet-till-VNet-VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-109">A VNet-to-VNet VPN connection.</span></span>
* <span data-ttu-id="ed6d8-110">En webbaserad LOB-server, SQLServer och sekundär domänkontrollant i hello TestVNET virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-110">A web-based LOB server, SQL server, and secondary domain controller in hello TestVNET virtual network.</span></span>

<span data-ttu-id="ed6d8-111">Den här konfigurationen ger en grund och vanliga startpunkten från vilken du kan:</span><span class="sxs-lookup"><span data-stu-id="ed6d8-111">This configuration provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="ed6d8-112">Utveckla och testa LOB-program som finns på Internet Information Services (IIS) med en SQL Server 2014 databasen serverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-112">Develop and test LOB applications hosted on Internet Information Services (IIS) with a SQL Server 2014 database backend in Azure.</span></span>
* <span data-ttu-id="ed6d8-113">Utföra tester för simulerad hybrid molnbaserade IT arbetsbelastningen.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-113">Perform testing of this simulated hybrid cloud-based IT workload.</span></span>

<span data-ttu-id="ed6d8-114">Det finns tre huvudsakliga faser toosetting den här hybrid cloud testmiljö:</span><span class="sxs-lookup"><span data-stu-id="ed6d8-114">There are three major phases toosetting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="ed6d8-115">Ställ in hello simulerade hybrid molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-115">Set up hello simulated hybrid cloud environment.</span></span>
2. <span data-ttu-id="ed6d8-116">Konfigurera hello SQL-serverdator (SQL1).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-116">Configure hello SQL server computer (SQL1).</span></span>
3. <span data-ttu-id="ed6d8-117">Konfigurera hello LOB-server (LOB1).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-117">Configure hello LOB server (LOB1).</span></span>

<span data-ttu-id="ed6d8-118">Arbetsbelastningen kräver en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-118">This workload requires an Azure subscription.</span></span> <span data-ttu-id="ed6d8-119">Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-119">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

<span data-ttu-id="ed6d8-120">Ett exempel på en produktion LOB-program finns i Azure finns hello **affärsprogram** arkitektur modell på [Microsoft Software Arkitekturdiagram och ritningar](http://msdn.microsoft.com/dn630664).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-120">For an example of a production LOB application hosted in Azure, see hello **Line of business applications** architecture blueprint at [Microsoft Software Architecture Diagrams and Blueprints](http://msdn.microsoft.com/dn630664).</span></span>

## <a name="phase-1-set-up-hello-simulated-hybrid-cloud-environment"></a><span data-ttu-id="ed6d8-121">Fas 1: Konfigurera hello simulerade hybrid molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-121">Phase 1: Set up hello simulated hybrid cloud environment</span></span>
<span data-ttu-id="ed6d8-122">Skapa hello [simulerade hybrid cloud testmiljö](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-122">Create hello [simulated hybrid cloud test environment](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="ed6d8-123">Eftersom den här testmiljö inte kräver hello förekomst av hello APP1-servern på hello Corpnet-undernät, kan du stänga den nu.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-123">Because this test environment does not require hello presence of hello APP1 server on hello Corpnet subnet, you can shut it down for now.</span></span>

<span data-ttu-id="ed6d8-124">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-124">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-hello-sql-server-computer-sql1"></a><span data-ttu-id="ed6d8-125">Fas 2: Konfigurera hello SQL-serverdator (SQL1)</span><span class="sxs-lookup"><span data-stu-id="ed6d8-125">Phase 2: Configure hello SQL server computer (SQL1)</span></span>
<span data-ttu-id="ed6d8-126">Starta hello DC2 datorn från hello Azure-portalen, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-126">From hello Azure portal, start hello DC2 computer if needed.</span></span>

<span data-ttu-id="ed6d8-127">Skapa en virtuell dator för SQL1 med följande kommandon i en Azure PowerShell-kommandotolk på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-127">Next, create a virtual machine for SQL1 with these commands at an Azure PowerShell command prompt on your local computer.</span></span> <span data-ttu-id="ed6d8-128">Tidigare toorunning kommandona, Fyll i hello variabelvärden och ta bort Hej < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-128">Prior toorunning these commands, fill in hello variable values and remove hello < and > characters.</span></span>

    $rgName="<your resource group name>"
    $locName="<hello Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for hello SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="ed6d8-129">Använd hello Azure portal tooconnect tooSQL1 med hello lokala administratörskontot för SQL1.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-129">Use hello Azure portal tooconnect tooSQL1 using hello local administrator account of SQL1.</span></span>

<span data-ttu-id="ed6d8-130">Konfigurera Windows-brandväggen regler tooallow grundläggande anslutningsmöjligheter testnings- och SQL Server-trafik.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-130">Next, configure Windows Firewall rules tooallow basic connectivity testing and SQL Server traffic.</span></span> <span data-ttu-id="ed6d8-131">Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på SQL1.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-131">From an administrator-level Windows PowerShell command prompt on SQL1, run these commands.</span></span>

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="ed6d8-132">hello ping-kommandot resulterar i fyra svar från IP-adressen 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-132">hello ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="ed6d8-133">Lägg till hello extra datadisk på SQL1 som en ny volym med enhetsbokstaven för hello F:.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-133">Next, add hello extra data disk on SQL1 as a new volume with hello drive letter F:.</span></span>

1. <span data-ttu-id="ed6d8-134">Hello vänster i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-134">In hello left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="ed6d8-135">I hello innehållsrutan i hello **diskar** klickar du på **disk 2** (med hello **Partition** ställa in också**okänd**).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-135">In hello contents pane, in hello **Disks** group, click **disk 2** (with hello **Partition** set too**Unknown**).</span></span>
3. <span data-ttu-id="ed6d8-136">Klicka på **uppgifter**, och klicka sedan på **ny volym**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-136">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="ed6d8-137">Hello sidan innan du börjar av hello guiden Ny volym, klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-137">On hello Before you begin page of hello New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="ed6d8-138">Hello väljer hello server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-138">On hello Select hello server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="ed6d8-139">När du uppmanas, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-139">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="ed6d8-140">Klicka på hello ange hello storleken på hello volym sida, **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-140">On hello Specify hello size of hello volume page, click **Next**.</span></span>
7. <span data-ttu-id="ed6d8-141">På hello tilldela tooa enhet enhetsbeteckning eller mapp klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-141">On hello Assign tooa drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="ed6d8-142">På hello Välj fil system inställningar klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-142">On hello Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="ed6d8-143">På hello bekräfta val klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-143">On hello Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="ed6d8-144">När du är klar klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-144">When complete, click **Close**.</span></span>

<span data-ttu-id="ed6d8-145">Du kan köra följande kommandon i Kommandotolken för hello Windows PowerShell på SQL1:</span><span class="sxs-lookup"><span data-stu-id="ed6d8-145">Run these commands at hello Windows PowerShell command prompt on SQL1:</span></span>

    md f:\Data
    md f:\Log
    md f:\Backup

<span data-ttu-id="ed6d8-146">Därefter ansluta SQL1 toohello CORP Windows Server Active Directory-domänen med följande kommandon i hello Windows PowerShell-Kommandotolken på SQL1.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-146">Next, join SQL1 toohello CORP Windows Server Active Directory domain with these commands at hello Windows PowerShell prompt on SQL1.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="ed6d8-147">Använd hello corp\användare1 konto när du uppmanas toosupply autentiseringsuppgifter för domänkontot för hello **Add-Computer** kommando.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-147">Use hello CORP\User1 account when prompted toosupply domain account credentials for hello **Add-Computer** command.</span></span>

<span data-ttu-id="ed6d8-148">Efter omstart, använda hello Azure portal tooconnect tooSQL1 *med hello lokalt administratörskonto på SQL1*.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-148">After restarting, use hello Azure portal tooconnect tooSQL1 *with hello local administrator account of SQL1*.</span></span>

<span data-ttu-id="ed6d8-149">Konfigurera SQL Server 2014 toouse hello F: enhet för nya databaser och behörigheter för kontot.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-149">Next, configure SQL Server 2014 toouse hello F: drive for new databases and for user account permissions.</span></span>

1. <span data-ttu-id="ed6d8-150">Skriv från startskärmen hello **SQL Server Management**, och klicka sedan på **SQL Server 2014 Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-150">From hello Start screen, type **SQL Server Management**, and then click **SQL Server 2014 Management Studio**.</span></span>
2. <span data-ttu-id="ed6d8-151">I **ansluta tooServer**, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-151">In **Connect tooServer**, click **Connect**.</span></span>
3. <span data-ttu-id="ed6d8-152">I trädvyn för hello Object Explorer högerklickar du på **SQL1**, och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-152">In hello Object Explorer tree pane, right-click **SQL1**, and then click **Properties**.</span></span>
4. <span data-ttu-id="ed6d8-153">I hello **serveregenskaper** -fönstret klickar du på **databasinställningar**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-153">In hello **Server Properties** window, click **Database Settings**.</span></span>
5. <span data-ttu-id="ed6d8-154">Leta upp hello **databasen standardplatserna** och ange dessa värden:</span><span class="sxs-lookup"><span data-stu-id="ed6d8-154">Locate hello **Database default locations** and set these values:</span></span> 
   * <span data-ttu-id="ed6d8-155">För **Data**, ange sökväg till hello **f:\Data**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-155">For **Data**, type hello path **f:\Data**.</span></span>
   * <span data-ttu-id="ed6d8-156">För **loggen**, ange sökväg till hello **f:\Log**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-156">For **Log**, type hello path **f:\Log**.</span></span>
   * <span data-ttu-id="ed6d8-157">För **säkerhetskopiering**, ange sökväg till hello **f:\Backup**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-157">For **Backup**, type hello path **f:\Backup**.</span></span>
   * <span data-ttu-id="ed6d8-158">Obs: Endast nya databaser använda dessa platser.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-158">Note: Only new databases use these locations.</span></span>
6. <span data-ttu-id="ed6d8-159">Klicka på hello **OK** tooclose hello fönster.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-159">Click hello **OK** tooclose hello window.</span></span>
7. <span data-ttu-id="ed6d8-160">I hello **Object Explorer** trädfönster öppna **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-160">In hello **Object Explorer** tree pane, open **Security**.</span></span>
8. <span data-ttu-id="ed6d8-161">Högerklicka på **inloggningar** och klicka sedan på **ny inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-161">Right-click **Logins** and then click **New Login**.</span></span>
9. <span data-ttu-id="ed6d8-162">I **inloggningsnamnet**, typen **corp\användare1**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-162">In **Login name**, type **CORP\User1**.</span></span>
10. <span data-ttu-id="ed6d8-163">På hello **serverroller** klickar du på **sysadmin**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-163">On hello **Server Roles** page, click **sysadmin**, and then click **OK**.</span></span>
11. <span data-ttu-id="ed6d8-164">Stäng Microsoft SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-164">Close Microsoft SQL Server Management Studio.</span></span>

<span data-ttu-id="ed6d8-165">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-165">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-hello-lob-server-lob1"></a><span data-ttu-id="ed6d8-166">Fas 3: Konfigurera hello LOB-server (LOB1)</span><span class="sxs-lookup"><span data-stu-id="ed6d8-166">Phase 3: Configure hello LOB server (LOB1)</span></span>
<span data-ttu-id="ed6d8-167">Först skapa en virtuell dator för LOB1 med följande kommandon i hello Azure PowerShell-Kommandotolken på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-167">First, create a virtual machine for LOB1 with these commands at hello Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="ed6d8-168">Använd sedan hello Azure portal tooconnect tooLOB1 med hello autentiseringsuppgifter hello lokalt administratörskonto för LOB1.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-168">Next, use hello Azure portal tooconnect tooLOB1 with hello credentials of hello local administrator account of LOB1.</span></span>

<span data-ttu-id="ed6d8-169">Konfigurera Windows-brandväggen regeln tooallow trafik för att testa grundläggande anslutningsmöjligheter.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-169">Next, configure a Windows Firewall rule tooallow traffic for basic connectivity testing.</span></span> <span data-ttu-id="ed6d8-170">Köra dessa kommandon från en behörighet på Windows PowerShell-Kommandotolken på LOB1.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-170">From an administrator-level Windows PowerShell command prompt on LOB1, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="ed6d8-171">hello ping-kommandot resulterar i fyra svar från IP-adressen 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-171">hello ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="ed6d8-172">Därefter ansluta LOB1 toohello CORP Active Directory-domän med följande kommandon i Kommandotolken för Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-172">Next, join LOB1 toohello CORP Active Directory domain with these commands at hello Windows PowerShell prompt.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="ed6d8-173">Använd hello corp\användare1 konto när du uppmanas toosupply autentiseringsuppgifter för domänkontot för hello **Add-Computer** kommando.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-173">Use hello CORP\User1 account when prompted toosupply domain account credentials for hello **Add-Computer** command.</span></span>

<span data-ttu-id="ed6d8-174">Efter omstart, använda hello Azure portal tooconnect tooLOB1 med hello corp\användare1 konto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-174">After restarting, use hello Azure portal tooconnect tooLOB1 with hello CORP\User1 account and password.</span></span>

<span data-ttu-id="ed6d8-175">Därefter konfigurera LOB1 för IIS och testa åtkomst från KLIENT1.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-175">Next, configure LOB1 for IIS and test access from CLIENT1.</span></span>

1. <span data-ttu-id="ed6d8-176">Från Serverhanteraren klickar du på **Lägg till roller och funktioner**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-176">From Server Manager, click **Add roles and features**.</span></span>
2. <span data-ttu-id="ed6d8-177">På hello **innan du börjar** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-177">On hello **Before you begin** page, click **Next**.</span></span>
3. <span data-ttu-id="ed6d8-178">På hello **Välj installationstyp** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-178">On hello **Select installation type** page, click **Next**.</span></span>
4. <span data-ttu-id="ed6d8-179">På hello **väljer målservern** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-179">On hello **Select destination server** page, click **Next**.</span></span>
5. <span data-ttu-id="ed6d8-180">På hello **serverroller** klickar du på **webbserver (IIS)** i hello lista över **roller**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-180">On hello **Server roles** page, click **Web Server (IIS)** in hello list of **Roles**.</span></span>
6. <span data-ttu-id="ed6d8-181">När du uppmanas, klickar du på **Lägg till funktioner**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-181">When prompted, click **Add Features**, and then click **Next**.</span></span>
7. <span data-ttu-id="ed6d8-182">På hello **Välj funktioner** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-182">On hello **Select features** page, click **Next**.</span></span>
8. <span data-ttu-id="ed6d8-183">På hello **webbserver (IIS)** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-183">On hello **Web Server (IIS)** page, click **Next**.</span></span>
9. <span data-ttu-id="ed6d8-184">På hello **Välj rolltjänster** sidan Markera eller avmarkera kryssrutorna för hello för hello-tjänster som du behöver för att testa LOB-program och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-184">On hello **Select role services** page, select or clear hello check boxes for hello services you need for testing your LOB application, and then click **Next**.</span></span>
10. <span data-ttu-id="ed6d8-185">På hello **Bekräfta installationsinställningarna** klickar du på **installera**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-185">On hello **Confirm installation selections** page, click **Install**.</span></span>
11. <span data-ttu-id="ed6d8-186">Vänta tills hello installation av komponenterna är klar och klicka sedan på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-186">Wait until hello installation of components has completed and then click **Close**.</span></span>
12. <span data-ttu-id="ed6d8-187">Anslut toohello CLIENT1-datorn med autentiseringsuppgifter hello corp\användare1 hello Azure-portalen, och starta om Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-187">From hello Azure portal, connect toohello CLIENT1 computer with hello CORP\User1 account credentials, and then start Internet Explorer.</span></span>
13. <span data-ttu-id="ed6d8-188">Ange i adressfältet hello **http://lob1/** och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-188">In hello Address bar, type **http://lob1/** and then press ENTER.</span></span> <span data-ttu-id="ed6d8-189">Du bör se hello standardwebbsidan för IIS 8.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-189">You should see hello default IIS 8 web page.</span></span>

<span data-ttu-id="ed6d8-190">Det här är din aktuella konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-190">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="ed6d8-191">Den här miljön är nu klar för du toodeploy webbaserade programmet på LOB1 och testa funktioner från KLIENT1 på hello Corpnet-undernätet.</span><span class="sxs-lookup"><span data-stu-id="ed6d8-191">This environment is now ready for you toodeploy your web-based application on LOB1 and test functionality from CLIENT1 on hello Corpnet subnet.</span></span>

## <a name="next-step"></a><span data-ttu-id="ed6d8-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed6d8-192">Next step</span></span>
* <span data-ttu-id="ed6d8-193">Lägg till en ny virtuell dator med hello [Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ed6d8-193">Add a new virtual machine using hello [Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

