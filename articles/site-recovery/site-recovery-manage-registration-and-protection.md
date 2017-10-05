---
title: Ta bort servrar och inaktivera skydd | Microsoft Docs
description: "Den här artikeln beskrivs hur du avregistrera servrar från en Site Recovery-valvet och att inaktivera skydd för virtuella datorer och fysiska servrar."
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
ms.openlocfilehash: 43f92a35dc9b04584badd1c9f1152470246b5012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="remove-servers-and-disable-protection"></a>Ta bort servrar och inaktivera skydd

Azure Site Recovery-tjänsten bidrar till din affärskontinuitet och haveriberedskap (BCDR) strategi. Tjänsten samordnar replikering, redundans och återställning av virtuella datorer och fysiska servrar. Datorer kan replikeras till Azure eller till ett sekundärt lokalt datacenter. En snabb översikt finns i [Vad är Azure Site Recovery?](site-recovery-overview.md)

Den här artikeln beskriver hur du avregistrera servrar från en Recovery Services-valvet i Azure-portalen och hur du inaktiverar skydd för datorer som skyddas av Site Recovery.

Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-connected-configuration-server"></a>Avregistrera en ansluten konfigurationsservern

Om du replikerar virtuella VMware-datorer eller Windows-/ Linux fysiska servrar till Azure kan du avregistrera en ansluten konfigurationsservern från ett valv på följande sätt:

1. Inaktivera skydd för datorn. I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.
2. Kopplingen mellan alla principer. I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **replikeringsprinciper**, dubbelklicka på den associerade princip. Högerklicka på konfigurationsservern > **ta bort association med**.
3. Ta bort alla ytterligare lokala process eller huvudmålservern servrar. I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på servern > **Ta bort**.
4. Ta bort konfigurationsservern.
5. Avinstallera mobilitetstjänsten på huvudmålservern manuellt (det här är antingen en separat server eller körs på konfigurationsservern).
6. Avinstallera alla ytterligare servrar.
7. Avinstallera konfigurationsservern.
8. Avinstallera instansen av MySQL som har installerats med Site Recovery på konfigurationsservern och.
9. Ta bort nyckeln i registret på konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>Avregistrera en ansluten konfigurationsservern

Om du replikerar virtuella VMware-datorer eller Windows-/ Linux fysiska servrar till Azure kan du avregistrera en ansluten konfigurationsservern från ett valv på följande sätt:

1. Inaktivera skydd för datorn. I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**. Välj **sluta hantera datorn**.
2. Ta bort alla ytterligare lokala process eller huvudmålservern servrar. I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på servern > **Ta bort**.
3. Ta bort konfigurationsservern.
4. Avinstallera mobilitetstjänsten på huvudmålservern manuellt (det här är antingen en separat server eller körs på konfigurationsservern).
5. Avinstallera alla ytterligare servrar.
6. Avinstallera konfigurationsservern.
7. Avinstallera instansen av MySQL som har installerats med Site Recovery på konfigurationsservern och.
8. Ta bort nyckeln i registret på konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Avregistrera en ansluten VMM-server

Som bästa praxis rekommenderar vi att du avregistrera VMM-servern när den är ansluten till Azure. Detta säkerställer att inställningar på VMM-servrar (och på andra VMM-servrar med parad moln) rensas korrekt. Du bör bara ta bort en ansluten server om det är ett permanent problem med anslutningen. Om VMM-servern inte är ansluten, behöver du köra ett skript för att rensa inställningarna manuellt.

1. Stoppa replikering av virtuella datorer i moln på VMM-servern som du vill ta bort.
2. Ta bort alla nätverksmappningar som används av moln på VMM-servern som du vill ta bort. I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på nätverksmappningen > **Ta bort**.
3. Kopplingen mellan replikeringsprinciper från moln på VMM-servern som du vill ta bort.  I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, dubbelklicka på den associera principen. Högerklicka på molnet > **ta bort association med**.
4. Ta bort VMM-servern eller aktiv nod i VMM. I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på servern > **ta bort** .
5. Avinstallera providern på VMM-servern manuellt. Om du har ett kluster kan du ta bort från alla noder.
6. Om du replikerar till Azure du manuellt ta bort Microsoft Recovery Services-agenten från Hyper-V-värdar i de borttagna moln.



### <a name="unregister-an-unconnected-vmm-server"></a>Avregistrera en ansluten VMM-servern

1. Stoppa replikering av virtuella datorer i moln på VMM-servern som du vill ta bort.
2. Ta bort alla nätverksmappningar som används av moln på VMM-servern som du vill ta bort. I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på nätverksmappningen > **Ta bort**.
3. Observera ID för VMM-servern.
4. Kopplingen mellan replikeringsprinciper från moln på VMM-servern som du vill ta bort.  I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, dubbelklicka på den associera principen. Högerklicka på molnet > **ta bort association med**.
5. Ta bort VMM-servern eller aktiva noden. I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på servern > **ta bort** .
6. Hämta och kör den [rensningsskript](http://aka.ms/asr-cleanup-script-vmm) på VMM-servern. Öppna PowerShell med den **kör som administratör** alternativet för att ändra körningsprincipen för standardomfånget som (LocalMachine). Ange ID för VMM-servern som du vill ta bort i skriptet. Skriptet tar bort registrering och molnet länkning av information från servern.
5. Kör skriptet för rensning på andra VMM-servrar som innehåller moln som har parats ihop med moln på VMM-servern som du vill ta bort.
6. Kör skriptet för rensning på alla andra passiva VMM-klusternoder som har installerad.
7. Avinstallera providern på VMM-servern manuellt. Om du har ett kluster kan du ta bort från alla noder.
8. Om du replikerar till Azure kan du ta bort Microsoft Recovery Services-agenten Hyper-V-värdar i de borttagna moln.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Avregistrera en Hyper-V-värd i en Hyper-V-plats

Hyper-V-värdar som inte hanteras av VMM samlas till en Hyper-V-plats. Ta bort en värd i en Hyper-V-plats på följande sätt:

1. Inaktivera replikering för Hyper-V virtuella datorer finns på värden.
2. Kopplingen mellan principer för Hyper-V-platsen. I **Site Recovery-infrastruktur** > **för Hyper-V-platser** >  **replikeringsprinciper**, dubbelklicka på den associera principen. Högerklicka på platsen > **ta bort association med**.
3. Ta bort Hyper-V-värdar. I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-värdar**, högerklicka på servern >  **Ta bort**.
4. Ta bort Hyper-V-platsen när alla värdar har tagits bort från den. I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-platser**, högerklicka på platsen > **ta bort** .
5. Kör följande skript på varje Hyper-V-värd som tagits bort. Skriptet rensar inställningarna på servern och Avregistrerar från valvet.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
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
                "Stopping the Azure Site Recovery service..."
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

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
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

1. I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.
2. I **ta bort datorn**, väljer du något av följande alternativ:
    - **Inaktivera skyddet för datorn (rekommenderas)**. Använd det här alternativet för att stoppa replikering av datorn. Site Recovery-inställningarna kommer att rensas automatiskt. Endast visas det här alternativet under följande omständigheter:
        - **Du har ändrat storlek VM volymen**– när du ändrar storlek på en volym som den virtuella datorn försätts i ett kritiskt tillstånd. Välj det här alternativet inaktiverar skyddet medan behåller återställningspunkter i Azure. När du aktiverar skyddet igen för datorn överförs data för den nya storleken volymen till Azure.
        - **Du har nyligen kör redundans**– när du har kört en växling vid fel för att testa din miljö, Välj det här alternativet för att börja skydda lokala datorer igen. Varje virtuell dator inaktiveras och måste du aktivera skyddet för dem igen. Om du inaktiverar datorn med den här inställningen påverkar inte den replikerade virtuella datorn i Azure. Inte avinstallera mobilitetstjänsten på datorn.
    - **Sluta hantera datorn**. Om du väljer det här alternativet måste tas datorn endast bort från valvet. Lokala skyddsinställningar för datorn påverkas inte. Du måste rensa inställningarna genom att avinstallera mobilitetstjänsten att ta bort inställningarna på datorn och ta bort datorn från Azure-prenumerationen.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Inaktivera skyddet för en Hyper-V virtuell dator i VMM-moln

1. I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.
2. I **ta bort datorn**, väljer du något av följande alternativ:

    - **Inaktivera skyddet för datorn (rekommenderas)**. Använd det här alternativet för att stoppa replikering av datorn. Site Recovery-inställningarna kommer att rensas automatiskt.
    - **Sluta hantera datorn**. Om du väljer det här alternativet måste tas datorn endast bort från valvet. Lokala skyddsinställningar för datorn påverkas inte. Ta bort inställningar på datorn och ta bort datorn från Azure-prenumerationen, måste du rensa inställningarna manuellt med hjälp av anvisningarna nedan. Observera att om du väljer att ta bort den virtuella datorn och dess hårddiskar de ska tas bort från målplatsen.

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a>Rensa skyddsinställningarna - replikering till en sekundär plats för VMM

Om du har valt **sluta hantera datorn** och du replikerar till en sekundär plats, köra detta skript på den primära servern att rensa inställningarna för den primära virtuella datorn. Klicka på knappen PowerShell om du vill öppna VMM PowerShell-konsolen i VMM-konsolen. Ersätt SQLVM1 med namnet på den virtuella datorn.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Kör skriptet för att rensa inställningarna för den sekundära virtuella datorn på den sekundära VMM-servern:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. Uppdatera de virtuella datorerna på Hyper-V-värdservern på den sekundära VMM-servern så att den sekundära virtuella datorn hämtar identifieras igen i VMM-konsolen.
4. Stegen ovan rensa bort replikeringsinställningarna på VMM-servern. Om du vill stoppa replikering för den virtuella datorn kör du följande skript OJ de primära och sekundära virtuella datorerna. Ersätt SQLVM1 med namnet på den virtuella datorn.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a>Rensa skyddsinställningarna - replikering till Azure

1. Om du har valt **sluta hantera datorn** och du replikerar till Azure, köra skriptet på VMM-källservern med hjälp av PowerShell från VMM-konsolen.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Replikeringsinställningarna på VMM-servern avmarkerar du stegen ovan. Kör skriptet för att stoppa replikering för den virtuella datorn körs på Hyper-V-värdservern. Ersätt SQLVM1 med namnet på den virtuella datorn och host01.contoso.com med namnet på Hyper-V-värdservern.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Inaktivera skyddet för en Hyper-V virtuell dator på en Hyper-V-plats

Använd den här proceduren om du replikerar virtuella Hyper-V-datorer till Azure utan en VMM-server.

1. I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.
2. I **ta bort datorn**, du kan välja följande alternativ:

   - **Inaktivera skyddet för datorn (rekommenderas)**. Använd det här alternativet för att stoppa replikering av datorn. Site Recovery-inställningarna kommer att rensas automatiskt.
   - **Sluta hantera datorn**. Om du väljer det här alternativet kommer bara datorn tas bort från valvet. Lokala skyddsinställningar för datorn påverkas inte. Ta bort inställningar på datorn och ta bort den virtuella datorn från Azure-prenumerationen, måste du rensa inställningarna manuellt. Om du väljer att ta bort den virtuella datorn och dess hårddiskar kommer de att tas bort från målplatsen.
3. Om du har valt **sluta hantera datorn**, köra skriptet på Hyper-V-värd källservern att ta bort replikering för den virtuella datorn. Ersätt SQLVM1 med namnet på den virtuella datorn.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
