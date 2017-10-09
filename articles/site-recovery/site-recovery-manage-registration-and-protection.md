---
title: aaaRemove servrar och inaktivera skydd | Microsoft Docs
description: "Den här artikeln beskriver hur toounregister servrar från en Site Recovery-valvet och toodisable skydd för virtuella datorer och fysiska servrar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 95f20433f782c93685ad4bae93c6bc0e2d2f2356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-servers-and-disable-protection"></a>Ta bort servrar och inaktivera skydd

hello Azure Site Recovery-tjänsten bidrar tooyour affärskontinuitet och haveriberedskap (BCDR). hello service samordnar replikering, redundans och återställning av virtuella datorer och fysiska servrar. Datorer kan vara replikerade tooAzure eller tooa sekundärt lokalt datacenter. En snabb översikt finns i [Vad är Azure Site Recovery?](site-recovery-overview.md)

Den här artikeln beskriver hur toounregister servrar från en återställningstjänster valvet i hello Azure-portalen och hur toodisable skydd för datorer som skyddas av Site Recovery.

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-connected-configuration-server"></a>Avregistrera en ansluten konfigurationsservern

Om du replikera virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooAzure avregistrera du en ansluten konfigurationsservern från ett valv på följande sätt:

1. Inaktivera skydd för datorn. I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.
2. Kopplingen mellan alla principer. I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **replikeringsprinciper**, dubbelklicka på hello associerade principer. Högerklicka på hello konfigurationsservern > **ta bort association med**.
3. Ta bort alla ytterligare lokala process eller huvudmålservern servrar. I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på hello server > **Ta bort**.
4. Ta bort hello konfigurationsservern.
5. Avinstallera manuellt hello mobilitetstjänsten körs på hello huvudmålservern (det här är antingen en separat server eller körs på konfigurationsservern hello).
6. Avinstallera alla ytterligare servrar.
7. Avinstallera hello konfigurationsservern.
8. Avinstallera hello instans av MySQL som har installerats med Site Recovery på hello konfigurationsservern.
9. Ta bort hello nyckeln i hello register hello konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>Avregistrera en ansluten konfigurationsservern

Om du replikera virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooAzure avregistrera du en ansluten konfigurationsservern från ett valv på följande sätt:

1. Inaktivera skydd för datorn. I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**. Välj **sluta hantera hello datorn**.
2. Ta bort alla ytterligare lokala process eller huvudmålservern servrar. I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på hello server > **Ta bort**.
3. Ta bort hello konfigurationsservern.
4. Avinstallera manuellt hello mobilitetstjänsten körs på hello huvudmålservern (det här är antingen en separat server eller körs på konfigurationsservern hello).
5. Avinstallera alla ytterligare servrar.
6. Avinstallera hello konfigurationsservern.
7. Avinstallera hello instans av MySQL som har installerats med Site Recovery på hello konfigurationsservern.
8. Ta bort hello nyckeln i hello register hello konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Avregistrera en ansluten VMM-server

Som bästa praxis rekommenderar vi att du avregistrera hello VMM-servern när den är ansluten tooAzure. Detta säkerställer att inställningar på hello VMM-servrar (och på andra VMM-servrar med parad moln) rensas korrekt. Du bör bara ta bort en ansluten server om det är ett permanent problem med anslutningen. Om hello VMM-servern inte är ansluten måste toomanually kör ett skript tooclean inställningarna.

1. Stoppa replikering av virtuella datorer i moln på hello VMM-servern som du vill tooremove.
2. Ta bort alla nätverksmappningar som används av moln på hello VMM-servern som du vill toodelete. I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på hello nätverksmappning >  **Ta bort**.
3. Kopplingen mellan replikeringsprinciper från moln på hello VMM-servern som du vill tooremove.  I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, genom att dubbelklicka på hello associerade. Högerklicka på hello molntjänster > **ta bort association med**.
4. Ta bort hello VMM-servern eller aktiv nod i VMM. I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på hello server >  **Ta bort**.
5. Avinstallera hello providern manuellt på hello VMM-servern. Om du har ett kluster kan du ta bort från alla noder.
6. Om du replikerar tooAzure du manuellt ta bort hello Microsoft Recovery Services-agenten från Hyper-V-värdar i hello bort moln.



### <a name="unregister-an-unconnected-vmm-server"></a>Avregistrera en ansluten VMM-servern

1. Stoppa replikering av virtuella datorer i moln på hello VMM-servern som du vill tooremove.
2. Ta bort alla nätverksmappningar som används av moln på hello VMM-servern som du vill toodelete. I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på hello nätverksmappning >  **Ta bort**.
3. Observera hello-ID för hello VMM-servern.
4. Kopplingen mellan replikeringsprinciper från moln på hello VMM-servern som du vill tooremove.  I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, genom att dubbelklicka på hello associerade. Högerklicka på hello molntjänster > **ta bort association med**.
5. Ta bort hello VMM-servern eller aktiva noden. I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på hello server >  **Ta bort**.
6. Hämta och köra hello [rensningsskript](http://aka.ms/asr-cleanup-script-vmm) på hello VMM-servern. Öppna PowerShell med hello **kör som administratör** alternativ, toochange hello körningsprincipen för hello standard (LocalMachine) omfång. Ange hello-ID för hello VMM-servern som du vill tooremove i hello skript. hello skriptet tar bort registrering och molnet länkning av information från hello-server.
5. Kör hello rensningsskript på andra VMM-servrar som innehåller moln som har parats ihop med moln på hello VMM-servern som du vill tooremove.
6. Kör hello Rensa skriptet på alla andra passiva VMM-klusternoder som har hello-Provider installerad.
7. Avinstallera hello providern manuellt på hello VMM-servern. Om du har ett kluster kan du ta bort från alla noder.
8. Om du replikerar tooAzure, du kan ta bort hello Microsoft Recovery Services-agenten från Hyper-V-värdar i hello bort moln.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Avregistrera en Hyper-V-värd i en Hyper-V-plats

Hyper-V-värdar som inte hanteras av VMM samlas till en Hyper-V-plats. Ta bort en värd i en Hyper-V-plats på följande sätt:

1. Inaktivera replikering för Hyper-V virtuella datorer finns på hello värd.
2. Kopplingen mellan principer för hello Hyper-V-platsen. I **Site Recovery-infrastruktur** > **för Hyper-V-platser** >  **replikeringsprinciper**, genom att dubbelklicka på hello associerade. Högerklicka på hello plats > **ta bort association med**.
3. Ta bort Hyper-V-värdar. I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-värdar**, högerklicka på hello server >  **Ta bort**.
4. Ta bort hello Hyper-V-platsen när alla värdar har tagits bort från den. I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-platser**, högerklicka på hello plats >  **Ta bort**.
5. Kör följande skript på varje Hyper-V-värd som du har tagit bort hello. hello skriptet rensar inställningarna på hello-servern och Avregistrerar från hello-valvet.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run hello script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove hello old Azure Site Recovery Provider related properties. Do you want toocontinue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping hello Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all hello certificates toobe deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete hello certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a>Inaktivera skyddet för en VMware-VM eller fysiska servrar

1. I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.
2. I **ta bort datorn**, väljer du något av följande alternativ:
    - **Inaktivera skyddet för hello datorn (rekommenderas)**. Använd det här alternativet toostop replikerar hello-datorn. Site Recovery-inställningarna kommer att rensas automatiskt. Endast visas det här alternativet i hello följande omständigheter:
        - **Du har ändrat storlek hello VM volym**– när du ändrar storlek på en volym hello virtuella datorn försätts i ett kritiskt tillstånd. Välj det här alternativet toodisables skyddet samtidigt som företaget behåller återställningspunkter i Azure. När du aktiverar skyddet för hello datorn igen kommer hello data för hello storlek volymen att överförda tooAzure.
        - **Du har nyligen kör redundans**– när du har kört en växling vid fel tootest din miljö, Välj det här alternativet toostart skydda lokala datorer igen. Varje virtuell dator inaktiveras och måste tooenable skyddet för dem igen. Inaktivera hello-datorn med den här inställningen påverkar inte hello replikerade virtuella datorn i Azure. Inte avinstallera mobilitetstjänsten för hello från hello-datorn.
    - **Sluta hantera hello datorn**. Om du väljer det här alternativet tas hello datorn endast bort från hello-valvet. Lokala skyddsinställningarna för hello datorn påverkas inte. tooremove inställningarna på hello datorn och tooremove hello dator från hello Azure-prenumeration, du måste tooclean hello inställningar genom att avinstallera mobilitetstjänsten för hello.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Inaktivera skyddet för en Hyper-V virtuell dator i VMM-moln

1. I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.
2. I **ta bort datorn**, väljer du något av följande alternativ:

    - **Inaktivera skyddet för hello datorn (rekommenderas)**. Använd det här alternativet toostop replikerar hello-datorn. Site Recovery-inställningarna kommer att rensas automatiskt.
    - **Sluta hantera hello datorn**. Om du väljer det här alternativet tas hello datorn endast bort från hello-valvet. Lokala skyddsinställningarna för hello datorn påverkas inte. tooremove inställningarna på hello datorn och tooremove hello dator från hello Azure-prenumeration, måste tooclean hello inställningar in manuellt med hjälp av hello anvisningarna nedan. Observera att om du väljer toodelete hello virtuell dator och datorns hårddiskar måste de ska tas bort från hello målplats.

### <a name="clean-up-protection-settings---replication-tooa-secondary-vmm-site"></a>Rensa skyddsinställningarna - replikering tooa sekundär VMM-plats

Om du har valt **sluta hantera hello datorn** och replikerar tooa sekundär plats, kör skriptet på hello primärservern tooclean hello inställningarna för hello primära virtuella datorn. Klicka på hello PowerShell knappen tooopen hello VMM PowerShell-konsolen i hello VMM-konsolen. Ersätt SQLVM1 med hello namnet på den virtuella datorn.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Kör det här skriptet tooclean hello inställningarna för hello sekundära virtuella datorn på hello sekundär VMM-servern:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. Uppdatera hello virtuella datorer på hello Hyper-V-värdservern på hello sekundär VMM-servern så att hello sekundära VM hämtar identifieras igen hello VMM-konsolen.
4. hello senare steg Rensa hello replikeringsinställningarna på hello VMM-servern. Om du vill toostop replikering för hello virtuell dator, kör följande skript OJ hello hello primära och sekundära virtuella datorer. Ersätt SQLVM1 med hello namnet på den virtuella datorn.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-tooazure"></a>Rensa skyddsinställningarna - replikering tooAzure

1. Om du har valt **sluta hantera hello datorn** och replikera tooAzure kan du köra skriptet på hello VMM-källservern med hjälp av PowerShell från hello VMM-konsolen.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. hello senare steg avmarkera hello replikeringsinställningarna på hello VMM-servern. toostop replikering för hello virtuell dator som kör på hello Hyper-V-värdservern kör det här skriptet. Ersätt SQLVM1 med hello namnet på den virtuella datorn och host01.contoso.com med hello namnet på hello Hyper-V-värdservern.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Inaktivera skyddet för en Hyper-V virtuell dator på en Hyper-V-plats

Använd följande procedur om du replikerar virtuella Hyper-V-datorer tooAzure utan en VMM-server.

1. I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.
2. I **ta bort datorn**, kan du välja hello följande alternativ:

   - **Inaktivera skyddet för hello datorn (rekommenderas)**. Använd det här alternativet toostop replikerar hello-datorn. Site Recovery-inställningarna kommer att rensas automatiskt.
   - **Sluta hantera hello datorn**. Om du väljer det här alternativet hello datorn endast tas bort från hello-valvet. Lokala skyddsinställningarna för hello datorn påverkas inte. tooremove inställningarna på hello datorn och tooremove hello virtuell dator från hello Azure-prenumeration, måste tooclean hello inställningar in manuellt. Om du väljer toodelete hello virtuella datorn och dess hårddiskar tas bort från hello målplatsen.
3. Om du har valt **sluta hantera hello datorn**, kör det här skriptet på hello källa Hyper-V-värdservern, tooremove replikering för hello virtuell dator. Ersätt SQLVM1 med hello namnet på den virtuella datorn.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
