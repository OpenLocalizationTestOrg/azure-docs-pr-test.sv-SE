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
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Ställ in ett webbaserat LOB-program i ett hybridmoln för testning
Det här avsnittet vägleder dig genom att skapa en simulerad hybrid cloud-miljö för att testa en webbaserad driftsapplikationer (LOB) finns i Microsoft Azure. Det här är den resulterande konfigurationen.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Den här konfigurationen består av:

* En simulerad lokala nätverk finns i Azure (testlabb VNet).
* Ett virtuellt nätverk på mellan platser som finns i Azure (TestVNET).
* En VNet-till-VNet-VPN-anslutning.
* En webbaserad LOB-server, SQLServer och sekundär domänkontrollant i TestVNET virtuella nätverk.

Den här konfigurationen ger en grund och vanliga startpunkten från vilken du kan:

* Utveckla och testa LOB-program som finns på Internet Information Services (IIS) med en SQL Server 2014 databasen serverdel i Azure.
* Utföra tester för simulerad hybrid molnbaserade IT arbetsbelastningen.

Det finns tre huvudsakliga steg för att skapa den här testmiljö för hybrid cloud:

1. Ställ in simulerade hybrid cloud-miljö.
2. Konfigurera SQL-serverdator (SQL1).
3. Konfigurera LOB-server (LOB1).

Arbetsbelastningen kräver en Azure-prenumeration. Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Ett exempel på en produktion LOB-program finns i Azure finns i **affärsprogram** arkitektur modell på [Microsoft Software Arkitekturdiagram och ritningar](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Fas 1: Konfigurera en simulerad hybrid cloud-miljö
Skapa den [simulerade hybrid cloud testmiljö](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Eftersom den här testmiljö inte kräver förekomsten av APP1-servern i Corpnet-undernät, kan du stänga den nu.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Fas 2: Konfigurera SQL-serverdator (SQL1)
Starta datorn DC2 från Azure portal, om det behövs.

Skapa en virtuell dator för SQL1 med följande kommandon i en Azure PowerShell-kommandotolk på den lokala datorn. Innan du kör kommandona, Fyll i variabelvärdena och ta bort den < och > tecken.

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

Använda Azure portal för att ansluta till SQL1 med det lokala administratörskontot för SQL1.

Konfigurera regler för Windows-brandväggen att tillåta grundläggande anslutningsmöjligheter testning och SQL Server-trafik. Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på SQL1.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Ping-kommandot ska resultera i fyra svar från IP-adressen 192.168.0.4.

Lägg sedan till datadisken extra på SQL1 som en ny volym med enhetsbokstaven F:.

1. I den vänstra rutan i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.
2. I innehållsrutan i den **diskar** klickar du på **disk 2** (med den **Partition** inställd på **okänd**).
3. Klicka på **uppgifter**, och klicka sedan på **ny volym**.
4. På sidan innan du börjar i guiden Ny volym, klickar på **nästa**.
5. Välj server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**. När du uppmanas, klickar du på **OK**.
6. Klicka på Ange storleken på volymen sidan **nästa**.
7. Tilldela till en enhet enhetsbeteckning eller mapp, klicka på **nästa**.
8. På sidan Välj fil system inställningar **nästa**.
9. På sidan Bekräfta val **skapa**.
10. När du är klar klickar du på **Stäng**.

Kör följande kommandon i Kommandotolken för Windows PowerShell på SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Därefter ansluta SQL1 till domänen CORP Windows Server Active Directory med följande kommandon i Windows PowerShell-Kommandotolken på SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Använd kontot corp\användare1 när du uppmanas för att ange autentiseringsuppgifter för domänkontot för den **Add-Computer** kommando.

Efter omstart, använder du den Azure-portalen för att ansluta till SQL1 *med det lokala administratörskontot för SQL1*.

Konfigurera SQL Server 2014 om du vill använda enheten F: för nya databaser och behörigheter för kontot.

1. Ange från startskärmen **SQL Server Management**, och klicka sedan på **SQL Server 2014 Management Studio**.
2. I **Anslut till Server**, klickar du på **Anslut**.
3. Högerklicka i fönstret Object Explorer trädet **SQL1**, och klicka sedan på **egenskaper**.
4. I den **serveregenskaper** -fönstret klickar du på **databasinställningar**.
5. Leta upp den **databasen standardplatserna** och ange dessa värden: 
   * För **Data**, anger du sökvägen **f:\Data**.
   * För **loggen**, anger du sökvägen **f:\Log**.
   * För **säkerhetskopiering**, anger du sökvägen **f:\Backup**.
   * Obs: Endast nya databaser använda dessa platser.
6. Klicka på den **OK** att stänga fönstret.
7. I den **Object Explorer** trädfönster öppna **säkerhet**.
8. Högerklicka på **inloggningar** och klicka sedan på **ny inloggning**.
9. I **inloggningsnamnet**, typen **corp\användare1**.
10. På den **serverroller** klickar du på **sysadmin**, och klicka sedan på **OK**.
11. Stäng Microsoft SQL Server Management Studio.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a>Fas 3: Konfigurera LOB-servern (LOB1)
Först skapa en virtuell dator för LOB1 med följande kommandon i Azure PowerShell-Kommandotolken på den lokala datorn.

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

Använd sedan Azure-portalen för att ansluta till LOB1 med autentiseringsuppgifterna för det lokala administratörskontot för LOB1.

Konfigurera en regel för Windows-brandväggen tillåter trafik för att testa grundläggande anslutningsmöjligheter. Köra dessa kommandon från en behörighet på Windows PowerShell-Kommandotolken på LOB1.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Ping-kommandot ska resultera i fyra svar från IP-adressen 192.168.0.4.

Därefter ansluta LOB1 CORP Active Directory-domän med följande kommandon i Windows PowerShell-Kommandotolken.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Använd kontot corp\användare1 när du uppmanas för att ange autentiseringsuppgifter för domänkontot för den **Add-Computer** kommando.

Efter omstart att använda Azure-portalen för att ansluta till LOB1 med CORP\User1-konto och lösenord.

Därefter konfigurera LOB1 för IIS och testa åtkomst från KLIENT1.

1. Från Serverhanteraren klickar du på **Lägg till roller och funktioner**.
2. På den **innan du börjar** klickar du på **nästa**.
3. På den **Välj installationstyp** klickar du på **nästa**.
4. På den **väljer målservern** klickar du på **nästa**.
5. På den **serverroller** klickar du på **webbserver (IIS)** i listan över **roller**.
6. När du uppmanas, klickar du på **Lägg till funktioner**, och klicka sedan på **nästa**.
7. På den **Välj funktioner** klickar du på **nästa**.
8. På den **webbserver (IIS)** klickar du på **nästa**.
9. På den **Välj rolltjänster** sidan, markera eller avmarkera kryssrutorna för de tjänster du behöver för att testa LOB-program och klicka sedan på **nästa**.
10. På den **Bekräfta installationsinställningarna** klickar du på **installera**.
11. Vänta tills installationen av komponenterna är klar och klicka sedan på **Stäng**.
12. Anslut till CLIENT1-datorn med autentiseringsuppgifter för corp\användare1 konto från Azure-portalen och starta om Internet Explorer.
13. I adressfältet skriver **http://lob1/** och tryck sedan på RETUR. Du bör se IIS 8 standardwebbsidan.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Den här miljön är nu redo att distribuera din webbaserade program på LOB1 och testa funktioner från KLIENT1 på undernätet Corpnet.

## <a name="next-step"></a>Nästa steg
* Lägg till en ny virtuell dator med hjälp av [Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

