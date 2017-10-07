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
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Ställ in ett webbaserat LOB-program i ett hybridmoln för testning
Det här avsnittet vägleder dig genom att skapa en simulerad hybrid cloud-miljö för att testa en webbaserad driftsapplikationer (LOB) finns i Microsoft Azure. Här är hello resulterande konfigurationen.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Den här konfigurationen består av:

* En simulerad lokala nätverk finns i Azure (hello testlabb VNet).
* Ett virtuellt nätverk på mellan platser som finns i Azure (TestVNET).
* En VNet-till-VNet-VPN-anslutning.
* En webbaserad LOB-server, SQLServer och sekundär domänkontrollant i hello TestVNET virtuella nätverk.

Den här konfigurationen ger en grund och vanliga startpunkten från vilken du kan:

* Utveckla och testa LOB-program som finns på Internet Information Services (IIS) med en SQL Server 2014 databasen serverdel i Azure.
* Utföra tester för simulerad hybrid molnbaserade IT arbetsbelastningen.

Det finns tre huvudsakliga faser toosetting den här hybrid cloud testmiljö:

1. Ställ in hello simulerade hybrid molnmiljö.
2. Konfigurera hello SQL-serverdator (SQL1).
3. Konfigurera hello LOB-server (LOB1).

Arbetsbelastningen kräver en Azure-prenumeration. Om du har en prenumeration med MSDN eller Visual Studio, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Ett exempel på en produktion LOB-program finns i Azure finns hello **affärsprogram** arkitektur modell på [Microsoft Software Arkitekturdiagram och ritningar](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-hello-simulated-hybrid-cloud-environment"></a>Fas 1: Konfigurera hello simulerade hybrid molnmiljö.
Skapa hello [simulerade hybrid cloud testmiljö](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Eftersom den här testmiljö inte kräver hello förekomst av hello APP1-servern på hello Corpnet-undernät, kan du stänga den nu.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-hello-sql-server-computer-sql1"></a>Fas 2: Konfigurera hello SQL-serverdator (SQL1)
Starta hello DC2 datorn från hello Azure-portalen, om det behövs.

Skapa en virtuell dator för SQL1 med följande kommandon i en Azure PowerShell-kommandotolk på den lokala datorn. Tidigare toorunning kommandona, Fyll i hello variabelvärden och ta bort Hej < och > tecken.

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

Använd hello Azure portal tooconnect tooSQL1 med hello lokala administratörskontot för SQL1.

Konfigurera Windows-brandväggen regler tooallow grundläggande anslutningsmöjligheter testnings- och SQL Server-trafik. Kör följande kommandon från en behörighet på Windows PowerShell-Kommandotolken på SQL1.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

hello ping-kommandot resulterar i fyra svar från IP-adressen 192.168.0.4.

Lägg till hello extra datadisk på SQL1 som en ny volym med enhetsbokstaven för hello F:.

1. Hello vänster i Serverhanteraren klickar du på **fil- och lagringstjänster**, och klicka sedan på **diskar**.
2. I hello innehållsrutan i hello **diskar** klickar du på **disk 2** (med hello **Partition** ställa in också**okänd**).
3. Klicka på **uppgifter**, och klicka sedan på **ny volym**.
4. Hello sidan innan du börjar av hello guiden Ny volym, klicka på **nästa**.
5. Hello väljer hello server och disk-sidan, klicka på **Disk 2**, och klicka sedan på **nästa**. När du uppmanas, klickar du på **OK**.
6. Klicka på hello ange hello storleken på hello volym sida, **nästa**.
7. På hello tilldela tooa enhet enhetsbeteckning eller mapp klickar du på **nästa**.
8. På hello Välj fil system inställningar klickar du på **nästa**.
9. På hello bekräfta val klickar du på **skapa**.
10. När du är klar klickar du på **Stäng**.

Du kan köra följande kommandon i Kommandotolken för hello Windows PowerShell på SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Därefter ansluta SQL1 toohello CORP Windows Server Active Directory-domänen med följande kommandon i hello Windows PowerShell-Kommandotolken på SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Använd hello corp\användare1 konto när du uppmanas toosupply autentiseringsuppgifter för domänkontot för hello **Add-Computer** kommando.

Efter omstart, använda hello Azure portal tooconnect tooSQL1 *med hello lokalt administratörskonto på SQL1*.

Konfigurera SQL Server 2014 toouse hello F: enhet för nya databaser och behörigheter för kontot.

1. Skriv från startskärmen hello **SQL Server Management**, och klicka sedan på **SQL Server 2014 Management Studio**.
2. I **ansluta tooServer**, klickar du på **Anslut**.
3. I trädvyn för hello Object Explorer högerklickar du på **SQL1**, och klicka sedan på **egenskaper**.
4. I hello **serveregenskaper** -fönstret klickar du på **databasinställningar**.
5. Leta upp hello **databasen standardplatserna** och ange dessa värden: 
   * För **Data**, ange sökväg till hello **f:\Data**.
   * För **loggen**, ange sökväg till hello **f:\Log**.
   * För **säkerhetskopiering**, ange sökväg till hello **f:\Backup**.
   * Obs: Endast nya databaser använda dessa platser.
6. Klicka på hello **OK** tooclose hello fönster.
7. I hello **Object Explorer** trädfönster öppna **säkerhet**.
8. Högerklicka på **inloggningar** och klicka sedan på **ny inloggning**.
9. I **inloggningsnamnet**, typen **corp\användare1**.
10. På hello **serverroller** klickar du på **sysadmin**, och klicka sedan på **OK**.
11. Stäng Microsoft SQL Server Management Studio.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-hello-lob-server-lob1"></a>Fas 3: Konfigurera hello LOB-server (LOB1)
Först skapa en virtuell dator för LOB1 med följande kommandon i hello Azure PowerShell-Kommandotolken på den lokala datorn.

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

Använd sedan hello Azure portal tooconnect tooLOB1 med hello autentiseringsuppgifter hello lokalt administratörskonto för LOB1.

Konfigurera Windows-brandväggen regeln tooallow trafik för att testa grundläggande anslutningsmöjligheter. Köra dessa kommandon från en behörighet på Windows PowerShell-Kommandotolken på LOB1.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

hello ping-kommandot resulterar i fyra svar från IP-adressen 192.168.0.4.

Därefter ansluta LOB1 toohello CORP Active Directory-domän med följande kommandon i Kommandotolken för Windows PowerShell hello.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Använd hello corp\användare1 konto när du uppmanas toosupply autentiseringsuppgifter för domänkontot för hello **Add-Computer** kommando.

Efter omstart, använda hello Azure portal tooconnect tooLOB1 med hello corp\användare1 konto och lösenord.

Därefter konfigurera LOB1 för IIS och testa åtkomst från KLIENT1.

1. Från Serverhanteraren klickar du på **Lägg till roller och funktioner**.
2. På hello **innan du börjar** klickar du på **nästa**.
3. På hello **Välj installationstyp** klickar du på **nästa**.
4. På hello **väljer målservern** klickar du på **nästa**.
5. På hello **serverroller** klickar du på **webbserver (IIS)** i hello lista över **roller**.
6. När du uppmanas, klickar du på **Lägg till funktioner**, och klicka sedan på **nästa**.
7. På hello **Välj funktioner** klickar du på **nästa**.
8. På hello **webbserver (IIS)** klickar du på **nästa**.
9. På hello **Välj rolltjänster** sidan Markera eller avmarkera kryssrutorna för hello för hello-tjänster som du behöver för att testa LOB-program och klicka sedan på **nästa**.
10. På hello **Bekräfta installationsinställningarna** klickar du på **installera**.
11. Vänta tills hello installation av komponenterna är klar och klicka sedan på **Stäng**.
12. Anslut toohello CLIENT1-datorn med autentiseringsuppgifter hello corp\användare1 hello Azure-portalen, och starta om Internet Explorer.
13. Ange i adressfältet hello **http://lob1/** och tryck sedan på RETUR. Du bör se hello standardwebbsidan för IIS 8.

Det här är din aktuella konfiguration.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Den här miljön är nu klar för du toodeploy webbaserade programmet på LOB1 och testa funktioner från KLIENT1 på hello Corpnet-undernätet.

## <a name="next-step"></a>Nästa steg
* Lägg till en ny virtuell dator med hello [Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

