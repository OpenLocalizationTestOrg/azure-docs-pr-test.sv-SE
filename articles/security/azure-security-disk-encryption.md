---
title: "aaaAzure Disk Encryption för Windows och Linux virtuella IaaS-datorer | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över Microsoft Azure Disk Encryption för Windows och Linux virtuella IaaS-datorer."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: kakhan
ms.openlocfilehash: b685abdcc908e66d2352ec5ac2d9996aa75af1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Disk Encryption för Windows och Linux-IaaS-VM
Microsoft Azure är starkt allokerat tooensuring datasekretessen data suveränitet och gör att du toocontrol din Azure värdbaserade data via en mängd avancerad teknik tooencrypt styra och hantera krypteringsnycklar kontroll & granska åtkomsten till data. Det ger Azure-kunder hello flexibilitet toochoose hello lösning som bäst motsvarar deras affärsbehov. I det här dokumentet får du lära dig mer tooa nya tekniklösning ”Azure Disk Encryption för Windows och Linux IaaS VM” toohelp skydda och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden. hello dokumentet ger detaljerad vägledning om hur toouse hello Azure disk encryption funktioner inklusive hello stöds scenarier och hello användarupplevelser.

> [!NOTE]
> Vissa rekommendationerna kan öka data, nätverk och beräkning Resursanvändning, vilket resulterar i ytterligare kostnader för licens eller prenumeration.

## <a name="overview"></a>Översikt
Azure Disk Encryption är en ny funktion som hjälper dig att kryptera din Windows- och Linux IaaS virtuella diskar. Azure Disk Encryption utnyttjar hello branschstandarden [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux tooprovide volymkryptering för hello OS och hello datadiskar. hello-lösning är integrerad med [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp du styra och hantera hello-diskkryptering nycklar och hemligheter i nyckelvalvet-prenumeration. hello lösning betyder också att krypteras alla data på hello virtuella diskar i vila i ditt Azure storage.

Azure disk encryption för Windows och Linux IaaS-VM är nu i **allmän tillgänglighet** i alla Azure offentliga och AzureGov regioner för Standard virtuella datorer och virtuella datorer med premium-lagring.

### <a name="encryption-scenarios"></a>Scenarier för kryptering
hello Azure Disk Encryption lösning stöder hello efter kundscenarier:

* Aktivera kryptering på den nya virtuella IaaS-datorer skapas från förkrypterade VHD- och krypteringsnycklar
* Aktivera kryptering på den nya virtuella IaaS-datorer skapas från galleriavbildningar för hello stöds Azure
* Aktivera kryptering på befintliga virtuella IaaS-datorer körs i Azure
* Inaktivera kryptering på Windows IaaS-VM
* Inaktivera kryptering på dataenheter för Linux virtuella IaaS-datorer
* Aktivera kryptering för hanterade diskar virtuella datorer
* Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM
* Säkerhetskopiering och återställning av krypterade virtuella datorer som krypterats med nyckelkryptering nyckel

hello-lösningen stöder hello följande scenarier för virtuella IaaS-datorer när de är aktiverade i Microsoft Azure:

* Integrering med Azure Key Vault
* Standardnivån VMs: [A, D, DS, G, GS, F och så vidare serien IaaS-VM](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Aktivera kryptering på Windows och Linux virtuella IaaS-datorer och virtuella datorer för hanterade diskar från hello stöds avbildningar i Azure-galleriet
* Inaktivera kryptering på Operativsystemet och enheter för Windows IaaS-VM och hanterade diskar virtuella datorer
* Inaktivera kryptering på dataenheter för Linux virtuella IaaS-datorer och hanterade diskar virtuella datorer
* Aktivera kryptering på IaaS virtuella datorer som kör Windows klient-OS
* Aktivera kryptering på volymer med monteringssökvägar
* Aktivera kryptering på den virtuella Linux-datorer konfigurerade med disk striping (RAID) med hjälp av mdadm
* Aktivera kryptering på den virtuella Linux-datorer med hjälp av LVM för datadiskar
* Aktivera kryptering på virtuella Windows-datorer konfigurerade med lagringsutrymmen
* Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM
* Alla offentliga Azure och AzureGov regioner som stöds

hello-lösningen stöder inte hello följande scenarier, funktioner och teknik:

* Grundnivån IaaS-VM
* Om du inaktiverar kryptering på en OS-enhet för Linux virtuella IaaS-datorer
* Om du inaktiverar kryptering på en dataenhet om hello OS-enheten är krypterad för Linux virtuella Iaas-datorer
* Virtuella IaaS-datorer som skapas med hjälp av hello klassiska Virtuella metod för skapande av
* Aktivera kryptering på Windows och Linux-IaaS-VM kunden anpassade avbildningar inte stöds. Aktivera enccryption på Linux LVM OS disk stöds inte för närvarande. Det här stödet kommer snart.
* Integrering med din lokala nyckelhanteringstjänst
* Azure Files (delade filsystem), Network File System (NFS), dynamiska volymer och virtuella Windows-datorer som är konfigurerade med programvarubaserad RAID-system
* Säkerhetskopiering och återställning av krypterade virtuella datorer, krypterade utan nyckelkryptering nyckel.
* Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM.

> [!NOTE]
> Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med hello KEK konfiguration. Den stöds inte på virtuella datorer som är krypterade utan KEK. KEK är en valfri parameter som aktiverar VM-kryptering. Det här stödet kommer snart.
> Uppdatera krypteringsinställningarna för en befintlig krypterade premium-lagring VM inte stöds. Det här stödet kommer snart.

### <a name="encryption-features"></a>Krypteringsfunktioner
När du aktiverar och distribuera Azure Disk Encryption för Azure IaaS-VM är hello följande funktioner aktiverade, beroende på konfigurationen hello:

* Kryptering av startvolymen för hello OS volym tooprotect hello i vila i lagringsutrymmet
* Kryptering av data volymer tooprotect hello datavolymer i vila i lagringsutrymmet
* Om du inaktiverar kryptering på hello OS och data enheter för Windows IaaS-VM
* Om du inaktiverar kryptering på hello data enheter för Linux virtuella IaaS-datorer (endast om OS är inte krypterad enhet)
* Skydda hello krypteringsnycklar och hemligheter i nyckelvalvet-prenumeration
* Rapportering hello krypteringsstatus för hello krypterade IaaS VM
* Borttagning av disk-kryptering konfigurationsinställningar från hello virtuell IaaS-dator
* Säkerhetskopiering och återställning av krypterade virtuella datorer med hjälp av hello Azure Backup-tjänsten

> [!NOTE]
> Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med hello KEK konfiguration. Den stöds inte på virtuella datorer som är krypterade utan KEK. KEK är en valfri parameter som aktiverar VM-kryptering.

Azure Disk Encryption för IaaS VMS för Windows och Linux-lösningen innehåller:

* hello diskkryptering tillägget för Windows.
* hello diskkryptering tillägget för Linux.
* hello-diskkryptering PowerShell-cmdlets.
* Hej diskkryptering Azure-kommandoradsgränssnittet (CLI) cmdlets.
* hello-diskkryptering Azure Resource Manager-mallar.

hello Azure Disk Encryption lösning stöds på virtuella IaaS-datorer som kör Windows eller Linux OS. Mer information om operativsystem som stöds hello finns hello ”krav” avsnittet.

> [!NOTE]
> Det finns inga extra kostnad för att kryptera Virtuella diskar med Azure Disk Encryption.

### <a name="value-proposition"></a>Förslagsvärde
När du använder hello Azure Disk Encryption-management-lösning kan du uppfylla hello följande affärsbehov:

* IaaS-VM är skyddade i vila, eftersom du kan använda branschstandardiserad teknik tooaddress organisations säkerhet och efterlevnad kraven för datakryptering.
* IaaS-VM start under kund-kontrollerade nycklar och principer, och du kan granska deras användning i nyckelvalvet.

### <a name="encryption-workflow"></a>Arbetsflöde för kryptering
kryptering av tooenable för Windows och Linux virtuella datorer, hello följande:

1. Välj ett scenario för kryptering bland hello före kryptering scenarier.
2. Välja tooenabling diskkryptering via hello Azure Disk Encryption Resource Manager-mall, PowerShell-cmdlets eller CLI-kommando och ange hello kryptering konfiguration.

   * Överför hello krypterade VHD tooyour storage-konto och hello encryption key väsentlig tooyour nyckelvalv hello kund-krypterad VHD scenariot. Ange hello kryptering configuration tooenable på en ny IaaS-VM.
   * Ange hello kryptering configuration tooenable på hello IaaS VM för nya virtuella datorer som skapas från hello Marketplace och befintliga virtuella datorer som redan körs i Azure.

3. Bevilja åtkomst toohello Azure-plattformen tooread hello-krypteringsnyckeln material (krypteringsnycklar BitLocker för Windows-System) och lösenfrasen för Linux från ditt nyckelvalv tooenable kryptering på hello IaaS VM.

4. Ange hello Azure Active Directory (Azure AD) application identitet toowrite hello kryptering viktiga väsentlig tooyour nyckelvalv. På så sätt kan kryptering på hello IaaS VM hello-scenarier som nämns i steg 2.

5. Azure uppdaterar hello VM modell med kryptering och hello nyckelvalv konfigurationen och ställer in den kryptera virtuella datorn.

 ![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Arbetsflöde för dekryptering
kryptering av toodisable för IaaS-VM, fullständig hello följande anvisningar:

1. Välj toodisable kryptering (dekryptering) på en körs IaaS-VM i Azure via hello Azure Disk Encryption Resource Manager-mall eller PowerShell-cmdlets och ange hello dekryptering konfiguration.

 Det här steget inaktiverar kryptering av hello OS eller hello datavolym eller båda på hello som använder Windows IaaS-VM. Men som nämns i föregående avsnitt i hello stöds om du inaktiverar kryptering för OS-disk för Linux inte. hello dekryptering steg tillåts endast för dataenheter i virtuella Linux-datorer så länge hello OS-disken inte krypteras.
2. Azure-uppdateringar hello VM modell och hello IaaS VM markeras dekrypterade. hello innehållet i hello VM krypteras inte längre i vila.

> [!NOTE]
> hello inaktivera kryptering åtgärden tar inte bort din key vault och hello kryptering nyckelmaterial (krypteringsnycklar BitLocker för Windows-System) eller lösenfrasen för Linux.
 > Om du inaktiverar kryptering för OS-disk för Linux stöds inte. hello dekryptering steg tillåts endast för dataenheter i virtuella Linux-datorer.
Inaktivera disk datakryptering för Linux stöds inte om hello OS-enheten är krypterad.

## <a name="prerequisites"></a>Krav
Innan du aktiverar Azure Disk Encryption på Azure IaaS-VM för hello stöds scenarier som beskrivs i hello ”Overview” avsnittet finns hello följande krav:

* Du måste ha ett giltigt aktiv Azure-prenumeration toocreate resurser i Azure i hello stöds regioner.
* Azure Disk Encryption stöds på hello följande versioner av Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 och Windows Server 2016.
* Azure Disk Encryption stöds på följande Windows-klientversioner hello: klienten för Windows 8 och Windows 10-klient.

> [!NOTE]
> Windows Server 2008 R2, måste du ha .NET Framework 4.5 installerat innan du aktiverar kryptering i Azure. Du kan installera det från Windows Update genom att installera hello valfri uppdatering Microsoft .NET Framework 4.5.2 för Windows Server 2008 R2 x64-baserade system ([KB2901983](https://support.microsoft.com/kb/2901983)).

* Det finns stöd för Azure Disk Encryption hello följande Azure-galleriet baserad på Linux-server-distributioner och versioner:

| Linux-Distribution | Version | Volymtyp som stöds för kryptering|
| --- | --- |--- |
| Ubuntu | 16.04-VARJE DAG-LTS | OS- och disk |
| Ubuntu | 14.04.5-DAILY-LTS | OS- och disk |
| Ubuntu | 12.10 | Datadisk |
| Ubuntu | 12.04 | Datadisk |
| RHEL | 7.3 | OS- och disk |
| RHEL | 7.2 | OS- och disk |
| RHEL | 6.8 | OS- och disk |
| RHEL | 6.7 | Datadisk |
| CentOS | 7.3 | OS- och disk |
| CentOS | 7.2n | OS- och disk |
| CentOS | 6.8 | OS- och disk |
| CentOS | 7.1 | Datadisk |
| CentOS | 7.0 | Datadisk |
| CentOS | 6.7 | Datadisk |
| CentOS | 6.6 | Datadisk |
| CentOS | 6.5 | Datadisk |
| openSUSE | 13.2 | Datadisk |
| SLES | 12 SP1 | Datadisk |
| SLES | 12-SP1 (Premium) | Datadisk |
| SLES | HPC 12 | Datadisk |
| SLES | 11 SP4 (Premium) | Datadisk |
| SLES | 11 SP4 | Datadisk |

* Azure Disk Encryption kräver att dina nyckelvalvet och virtuella datorer finns i hello samma Azure-region och prenumeration.

> [!NOTE]
> Konfigurera hello resurser i olika områden orsakar ett fel i hello Azure Disk Encryption funktionen aktiveras.

* tooset upp och konfigurera nyckelvalvet för Azure Disk Encryption, se avsnittet **ställa upp och konfigurera nyckelvalvet för Azure Disk Encryption** i hello *krav* i den här artikeln.
* tooset upp och konfigurera Azure AD-program i Azure Active directory för Azure Disk Encryption, se avsnittet **konfigurera hello Azure AD-program i Azure Active Directory** i hello *krav* avsnitt i den här artikeln.
* tooset upp och konfigurerar hello nyckelvalv åtkomstprincip för hello Azure AD-program, finns i avsnittet **konfigurera hello nyckelvalv åtkomstprincip för hello Azure AD application** i hello *krav* avsnitt i den här artikeln.
* tooprepare en förkrypterade Windows VHD finns i avsnittet **förbereda en förkrypterade Windows VHD** i hello *bilaga*.
* tooprepare en förkrypterade Linux VHD finns i avsnittet **förbereda en förkrypterade Linux VHD** i hello *bilaga*.
* hello Azure-plattformen måste åtkomst toohello krypteringsnycklar och hemligheter i nyckelvalvet-toomake dem tillgängliga toohello virtuella datorn när den startar och dekrypterar hello virtuella OS volym. toogrant behörigheter tooAzure plattform, ange hello **EnabledForDiskEncryption** egenskap i hello nyckelvalvet. Mer information finns i **ställa upp och konfigurera nyckelvalvet för Azure Disk Encryption** i hello tillägg.
* Ditt nyckelvalv hemlighet och KEK URL: er måste vara en ny version. Azure tillämpar den här begränsningen för versionshantering. KEK URL: er och giltig hemlighet finns hello följande exempel:

  * Exempel på en giltig hemliga URL: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Exempel på en giltig URL för KEK: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Encryption stöder inte att ange portnummer som en del av KEK URL: er och hemligheter i nyckelvalvet. Exempel på inte stöds och stöds nyckelvalv URL: er finns i hello följande:

  * Oacceptabel key vault-URL *https://contosovault.vault.azure.net:443/hemligheter/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Godkända key vault-URL: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* tooenable hello Azure Disk Encryption funktionen hello virtuella IaaS-datorer måste uppfylla hello följande nätverkskrav endpoint konfiguration:
  * tooget en token tooconnect tooyour nyckelvalvet hello IaaS VM måste vara kan tooconnect tooan Azure Active Directory-slutpunkt, \[login.microsoftonline.com\].
  * toowrite hello kryptering nycklar tooyour nyckelvalvet hello IaaS VM måste vara kan tooconnect toohello nyckelvalv slutpunkt.
  * Hej IaaS VM måste vara kan tooconnect tooan Azure storage endpoint att värdar hello Azure-tillägget databasen och ett Azure storage-konto att värdar hello VHD-filer.

  > [!NOTE]
  > Om din säkerhetsprincip begränsar åtkomst från virtuella datorer i Azure toohello Internet, kan du lösa hello föregående URI och konfigurera en specifik regel tooallow utgående anslutning toohello IP-adresser.
  >
  >tooconfigure och åtkomst till Azure Key Vault bakom en brandvägg (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)

* Använd hello senaste versionen av Azure PowerShell SDK version tooconfigure Azure Disk Encryption. Hämta hello senaste versionen av [versionen av Azure PowerShell](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > Azure Disk Encryption stöds inte på [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Om du får ett fel relaterade toousing Azure PowerShell 1.1.0, se [Azure Disk Encryption fel relaterade tooAzure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* toorun alla Azure CLI-kommandon och koppla den till din Azure-prenumeration, måste du först installera Azure CLI:
  * tooinstall Azure CLI och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure CLI](../cli-install-nodejs.md).
  * toouse Azure CLI för Mac, Linux och Windows med Azure Resource Manager finns [Azure CLI-kommandona i Resource Manager-läget](../virtual-machines/azure-cli-arm-commands.md).

* När du krypterar en hanterade diskar är obligatoriska nödvändiga tootake en ögonblicksbild av hello hanteras disk eller en säkerhetskopia av hello disk utanför Azure Disk Encryption tidigare tooenabling kryptering.  Utan en säkerhetskopia för kan ett oväntat fel under krypteringen återge hello disk- och Virtuella tillgänglig utan ett återställningsalternativ.  Ange AzureRmVMDiskEncryptionExtension för närvarande inte säkerhetskopiera hanterade diskar och kommer fel om användas mot en hanterad disk såvida inte hello - skipVmBackup parametern har angetts.  Den här parametern är osäkra toouse om inte en säkerhetskopia har redan gjorts utanför Azure Disk Encryption.   När hello skipVmBackup - parametern anges, hello cmdlet inte att göra en säkerhetskopia av hello hanterade disken tidigare tooencryption.  Därför anses en obligatorisk nödvändiga toomake till en säkerhetskopia av hello hanterade diskar virtuella datorn är i drift tidigare tooenabling Azure Disk Encryption återställning är senare behövs.  
> [!NOTE]
 > hello - skipVmBackup parametern ska aldrig användas om en ögonblicksbild eller säkerhetskopiering har redan gjorts utanför Azure Disk Encryption. 

* hello Azure Disk Encryption lösningen använder hello externa nyckelskydd för BitLocker för Windows IaaS-VM. Domän push anslutna virtuella datorer, inte i alla grupprinciper som genomdriva TPM-skydd. Information om hello Grupprincip för ”Tillåt BitLocker utan en kompatibel TPM” finns [BitLocker gruppolicy referens](https://technet.microsoft.com/library/ee706521).
* BitLocker-principen på domänanslutna virtuella datorer med anpassade Grupprincip måste innehålla hello följande inställning: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption misslyckas när anpassade grupprincipinställningarna för Bitlocker är inkompatibla. På datorer som inte hade hello kan rätt principinställning hello nya principer, tillämpas att tvinga hello ny princip tooupdate (gpupdate.exe/Force) och starta sedan om krävas.  
* toocreate ett Azure AD-program, skapa nyckelvalvet, eller konfigurera en befintlig nyckelvalv och aktivera kryptering, se hello [PowerShell-skript för Azure Disk Encryption nödvändiga](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* tooconfigure-diskkryptering krav med hello Azure CLI, se [Bash skriptet](https://github.com/ejarvi/ade-cli-getting-started).
* kryptera dina virtuella datorer med hjälp av hello Azure Disk Encryption key configuration toouse hello Azure Backup service tooback upp och återställa krypterade virtuella datorer, vid kryptering har aktiverats med Azure Disk Encryption. hello Backup-tjänsten har stöd för virtuella datorer som krypteras med endast KEK-konfiguration. Se [hur tooback upp och återställning av krypterade virtuella datorer med Azure Backup kryptering](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).

* När du krypterar en Linux OS-volym, Observera att en VM-omstart krävs för närvarande hello slutet av hello-processen. Detta kan göras via hello portal, powershell eller CLI.   tootrack hello förloppet för kryptering, regelbundet avsöka hello statusmeddelande som returneras av Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.  När kryptering är klar anger hello hello statusmeddelande som returneras av det här kommandot detta.  Till exempel ”ProgressMessage: OS-disken har krypterats, starta om hello VM” på den här punkten hello VM kan startas om och användas.  

* Azure Disk Encryption för Linux kräver data diskar toohave ett anslutet filsystem i Linux tidigare tooencryption

* Rekursivt monterade diskar inte stöds av hello Azure Disk Encryption för Linux. Till exempel om hello målsystemet har monterats en disk på /foo/bar och sedan en annan på /foo/bar/baz hello kryptering av /foo/bar/baz lyckas, men misslyckas kryptering av/foo/stapel. 

* Azure Disk Encryption stöds bara på Azure stöds galleriavbildningar som uppfyller dessa krav för hello. Kunden anpassade avbildningar stöds inte på grund av toocustom partition system och processen beteenden som kan finnas på dessa bilder. Dessutom Bildbaserad även galleriet Virtuella datorer som ursprungligen uppfyllda förutsättningar men har ändrats efter att skapa kan vara inkompatibla.  För att därför hello förslag proceduren för att kryptera en Linux VM toostart från en avbildning av ren galleriet, kryptera hello VM och sedan lägga till anpassad programvara eller data toohello VM efter behov.  

> [!NOTE]
> Säkerhetskopiering och återställning av krypterade virtuella datorer stöds bara för virtuella datorer som är krypterade med hello KEK konfiguration. Den stöds inte på virtuella datorer som är krypterade utan KEK. KEK är en valfri parameter som möjliggör VM.

#### <a name="set-up-hello-azure-ad-application-in-azure-active-directory"></a>Ställ in hello Azure AD-program i Azure Active Directory
När du behöver kryptering toobe aktiverad på en aktiv virtuell dator i Azure, Azure Disk Encryption genererar och skriver hello kryptering nycklar tooyour nyckelvalvet. Hantera krypteringsnycklar i nyckelvalvet kräver Azure AD-autentisering.

Skapa ett Azure AD-program för det här ändamålet. Du kan hitta detaljerade anvisningar för att registrera ett program under hello ”hämta en identitet för hello programmet” hello blogginlägget [Azure Key Vault - steg-för-steg](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Det här exemplet innehåller också ett antal användbara exempel för att installera och konfigurera nyckelvalvet. Du kan använda client secret-baserad autentisering eller klientautentisering certifikatbaserad Azure AD för autentisering.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Klienten hemlighet-baserad autentisering för Azure AD
hello avsnitten som följer kan hjälpa dig att konfigurera en hemlighet-baserade klientautentisering för Azure AD.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Skapa ett Azure AD-program med hjälp av Azure PowerShell
Använd följande PowerShell-cmdlet toocreate en Azure AD-programmet hello:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId är hello Azure AD ClientID och $aadClientSecret är att du ska använda senare tooenable Azure Disk Encryption hello klienthemlighet. Skydda hello Azure AD-klienthemlighet på lämpligt sätt.

##### <a name="setting-up-hello-azure-ad-client-id-and-secret-from-hello-azure-classic-portal"></a>Ställa in hello Azure AD-klient-ID och Hemlig från hello klassiska Azure-portalen
Du kan också ställa in din Azure AD-klient-ID och Hemlig med hjälp av hello [klassiska Azure-portalen]( https://manage.windowsazure.com). tooperform detta uppgift, hello följande:

1. Klicka på hello **Active Directory** fliken.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. Klicka på **Lägg till program**, och sedan hello programmet typnamnet.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. Klicka på hello pilknappen och sedan konfigurera hello programegenskaper.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. Klicka på hello hello nedre vänstra hörnet toofinish är markerat. hello konfigurationssidan för programmet, och hello Azure AD-klient-ID visas på hello hello sidans nederkant.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. Spara hello Azure AD-klienthemlighet genom att klicka på hello **spara** knappen. Observera hello Azure AD klienthemlighet i textrutan för hello nycklar. Skydda på lämpligt sätt.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > hello föregående flödet stöds inte på hello klassiska Azure-portalen.

##### <a name="use-an-existing-application"></a>Använda ett befintligt program
tooexecute hello följande kommandon, hämta och använda hello [Azure AD PowerShell-modulen](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> hello måste följande kommandon köras från en ny PowerShell-fönstret. Använd inte Azure PowerShell eller hello Azure Resource Manager tooexecute hello kommandon. Vi rekommenderar den här metoden eftersom dessa cmdlets finns i hello MSOnline modul eller Azure AD PowerShell.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Certifikatbaserad autentisering för Azure AD
> [!NOTE]
> Azure AD certifikatbaserad autentisering stöds för närvarande inte på Linux virtuella datorer.

Hej avsnitten som följer visa hur tooconfigure certifikatbaserad autentisering för Azure AD.

##### <a name="create-an-azure-ad-application"></a>Skapa en Azure AD-program
toocreate ett Azure AD-program, köra hello följande PowerShell-cmdlets:

> [!NOTE]
> Ersätt hello följande `yourpassword` sträng med säkra lösenord och skydda hello lösenord.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

När du har slutfört det här steget överför nyckelvalv en PFX-filen tooyour och aktivera hello åtkomst principinformation som behövs toodeploy som certifikatet tooa VM.

##### <a name="use-an-existing-azure-ad-application"></a>Använda ett befintligt Azure AD-program
Om du konfigurerar certifikatbaserad autentisering för ett befintligt program använda hello PowerShell-cmdlets som visas här. Vara säker på att tooexecute dem från ett nytt PowerShell-fönster.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

När du har slutfört det här steget överför nyckelvalv en PFX-filen tooyour och aktivera hello åtkomstprincip som behövs för toodeploy hello certifikat tooa VM.

##### <a name="upload-a-pfx-file-tooyour-key-vault"></a>Överför nyckelvalv en PFX-filen tooyour
En detaljerad förklaring av den här processen finns [hello officiella Azure Key Vault-teamets blogg](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). Hello följande PowerShell-cmdlets är allt du behöver för hello-aktivitet. Vara säker på att tooexecute dem från Azure PowerShell-konsolen.

> [!NOTE]
> Ersätt hello följande `yourpassword` sträng med säkra lösenord och skydda hello lösenord.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-tooan-existing-vm"></a>Distribuera ett certifikat i ditt nyckelvalv tooan befintliga VM
När du är klar med att ladda upp hello PFX distribuera ett certifikat i hello nyckelvalv tooan befintlig virtuell dator med hello följande:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-hello-key-vault-access-policy-for-hello-azure-ad-application"></a>Ställ in hello nyckelvalv åtkomstprincip för hello Azure AD-program
Azure AD-program måste rättigheter tooaccess hello nycklar och hemligheter i hello-valvet. Använd hello [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet toogrant behörigheter toohello, används hello klient-ID (som skapades när programmet hello registrerades) som programmet hello _– ServicePrincipalName_ parametervärdet. toolearn finns fler hello bloggposten [Azure Key Vault - steg-för-steg](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Här är ett exempel på hur tooperform detta uppgift PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> Azure Disk Encryption kräver tooconfigure hello följande åtkomst principer tooyour Azure AD-klientprogram: _WrapKey_ och _ange_ behörigheter.

## <a name="terminology"></a>Terminologi
toounderstand vissa hello vanliga termer används av den här tekniken, Använd hello följande terminologi tabell:

| Terminologi | Definition |
| --- | --- |
| Azure AD | Azure AD [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Azure AD-kontot är en förutsättning för autentisering, lagra och hämta hemligheter från en nyckelvalvet. |
| Azure Key Vault | Key Vault är en kryptografisk, key management-tjänst som baseras på FIPS Federal Information Processing Standards validerade maskinvarusäkerhetsmoduler, som bidrar till att skydda din kryptografiska nycklar och hemligheter känslig. Mer information finns i [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentation. |
| ARM | Azure Resource Manager |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) är en branschstandard identifieras Windows volym krypteringsteknik som har använt tooenable diskkryptering på Windows IaaS-VM. |
| BEK | Krypteringsnycklarna i BitLocker är används tooencrypt hello OS startvolymen och datavolymer. hello BitLocker nycklar skyddas i ett nyckelvalv som hemligheter. |
| CLI | Se [Azure-kommandoradsgränssnittet](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) är hello Linux-baserade och transparent diskkryptering delsystem som har använt tooenable diskkryptering på Linux IaaS-VM. |
| KEK | Viktiga krypteringsnyckeln är hello asymmetrisk nyckel (RSA 2048) att du kan använda tooprotect eller omsluter hello hemlighet. Du kan ange en säkerhets-och maskinvara moduler (HSM)-skyddad nyckel eller programvaruskyddad nyckel. Mer information finns i [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentation. |
| PS-cmdlets | Se [Azure PowerShell-cmdlets](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Installera och konfigurera nyckelvalvet för Azure Disk Encryption
Azure Disk Encryption hjälper till att skydda hello-diskkryptering nycklar och hemligheter i nyckelvalvet. tooset in nyckelvalvet för Azure Disk Encryption fullständig hello steg i varje hello följande avsnitt.

#### <a name="create-a-key-vault"></a>Skapa ett nyckelvalv
toocreate nyckelvalvet, med någon av följande alternativ för hello:

* [”101-nyckel-valvet-skapa” Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Azure PowerShell nyckelvalv-cmdlets](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Hur för[Secure nyckelvalvet](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Om du redan har installerat en nyckelvalv för din prenumeration, hoppar du över toohello nästa avsnitt.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Ställa in en krypteringsnyckel nyckel (valfritt)
Lägg till en KEK tooyour nyckelvalv om du vill toouse en KEK för ett extra säkerhetslager för hello BitLocker krypteringsnycklar. Använd hello [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet toocreate en krypteringsnyckel nyckel i hello nyckelvalvet. Du kan också importera en KEK från din lokala nyckelhantering HSM. Mer information finns i [Key Vault dokumentationen](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Du kan lägga till hello KEK genom att gå tooAzure Resource Manager eller genom att använda nyckelvalv-gränssnittet.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Ange behörigheter för nyckelvalvet
hello Azure-plattformen måste åtkomst toohello krypteringsnycklar och hemligheter i nyckelvalvet-toomake dem tillgängliga toohello VM för att starta och dekryptera hello volymer. toogrant behörigheter toohello Azure-plattformen, ange hello **EnabledForDiskEncryption** egenskap i hello nyckeln valvet med hello nyckelvalv PowerShell-cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Du kan också ange hello **EnabledForDiskEncryption** egenskapen genom att besöka hello [resursutforskaren Azure](https://resources.azure.com).

Som tidigare nämnts kan du ange hello **EnabledForDiskEncryption** -egenskapen i nyckelvalvet. Annars misslyckas hello distributionen.

Du kan konfigurera principer för ditt Azure AD-program från hello nyckelvalv gränssnitt som visas här:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

På hello **avancerade åtkomstprinciper** och se till att nyckelvalvet är aktiverat för Azure Disk Encryption:

![Azure key vault](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Distributionsscenarier för disk-kryptering och användarupplevelser
Du kan aktivera många scenarier för disk-kryptering och hello steg kan variera bl.a toohello scenario. hello beskriver följande avsnitt hello scenarier i detalj.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-hello-marketplace"></a>Aktivera kryptering på den nya virtuella IaaS-datorer som skapas från hello Marketplace
Du kan aktivera kryptering på nya Windows IaaS-VM från hello Marketplace i Azure med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello kryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.

2. Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på en ny IaaS-VM.

> [!NOTE]
> Den här mallen skapar en ny krypterade Windows virtuell dator som använder bild för hello Windows Server 2012-galleriet.

Du kan aktivera kryptering på en ny IaaS RedHat Linux 7.2 virtuell dator med en 200 GB RAID-0-matris med den här [Resource Manager-mall](https://aka.ms/fde-rhel). När du har distribuerat hello mallen verifiera hello VM krypteringsstatus med hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet, enligt beskrivningen i [kryptera OS enhet på en körande Linux VM](#encrypting-os-drive-on-a-running-linux-vm). När hello datorn returnerar statusvärdet _VMRestartPending_, starta om hello VM.

hello följande tabell listar hello Resource Manager-mallens parametrar för nya virtuella datorer från hello Marketplace-scenario med Azure AD-klient-ID:

| Parameter | Beskrivning |
| --- | --- |
| adminUserName | Admin användarnamn för hello virtuell dator. |
| adminPassword | Administratörslösenord för hello virtuella datorn. |
| newStorageAccountName | Namnet på hello storage-konto toostore OS och data virtuella hårddiskar. |
| vmSize | Storleken på hello VM. För närvarande stöds endast Standard A, D och G serien. |
| virtualNetworkName | Namnet på hello VNet som hello VM NIC ska tillhöra. |
| subnetName | Namnet på hello undernät i hello VNet som hello VM NIC ska tillhöra. |
| AADClientID | Klient-ID för hello Azure AD-program som har behörigheter toowrite hemligheter tooyour nyckelvalvet. |
| AADClientSecret | Klienthemlighet för hello Azure AD-program som har behörigheter toowrite hemligheter tooyour nyckelvalvet. |
| keyVaultURL | URL för hello nyckeln valvet som hello BitLocker nyckel ska överföras till. Du kan hämta den med hjälp av cmdlet hello `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | URL för hello nyckelkryptering nyckel som används tooencrypt hello genereras BitLocker-nyckel (valfritt). |
| keyVaultResourceGroup | Resursgruppen för hello nyckelvalvet. |
| vmName | Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på. |

> [!NOTE]
> _KeyEncryptionKeyURL_ är en valfri parameter. Du kan sätta egna KEK toofurther skydda hello datakrypteringsnyckeln (lösenfrasen hemlig) i nyckelvalvet.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Aktivera kryptering på den nya virtuella IaaS-datorer som skapas från kund-krypterad VHD- och krypteringsnycklar
Du kan aktivera kryptering genom att använda hello Resource Manager-mall, PowerShell-cmdlets eller CLI-kommandona i det här scenariot. hello följande avsnitt beskrivs i större detalj hello Resource Manager-mall och CLI-kommandona.

Följ instruktionerna för hello från någon av dessa avsnitt för att förbereda inför krypterade avbildningar som kan användas i Azure. När hello avbildningen har skapats kan kan du använda hello steg i hello nästa avsnitt toocreate en krypterad Azure VM.

* [Förbereda en förkrypterade Windows VHD](#preparing-a-pre-encrypted-windows-vhd)
* [Förbereda en förkrypterade Linux VHD](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-hello-resource-manager-template"></a>Med hjälp av hello Resource Manager-mall
Du kan aktivera kryptering på den kryptera virtuella Hårddisken med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello kryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.

2. Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på hello nya IaaS VM.

hello följande tabell listar hello Resource Manager-mallens parametrar för den kryptera virtuella Hårddisken:

| Parameter | Beskrivning |
| --- | --- |
| newStorageAccountName | Namnet på hello storage-konto toostore hello krypterad OS-VHD. Det här lagringskontot bör redan har skapats i hello samma resursgrupp och samma plats som hello VM. |
| osVhdUri | URI för hello OS VHD från hello storage-konto. |
| osType | OS-produkttyp (Windows-/ Linux). |
| virtualNetworkName | Namnet på hello VNet som hello VM NIC ska tillhöra. hello namn bör redan har skapats i hello samma resursgrupp och samma plats som hello VM. |
| subnetName | Namnet på hello undernät på hello VNet som hello VM NIC ska tillhöra. |
| vmSize | Storleken på hello VM. För närvarande stöds endast Standard A, D och G serien. |
| keyVaultResourceID | hello ResourceID som identifierar hello nyckelvalv resurs i Azure Resource Manager. Du kan hämta den med hjälp av PowerShell-cmdlet för hello `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | URL till hello disk-krypteringsnyckeln som har ställts in i hello nyckelvalvet. |
| keyVaultKekUrl | URL till hello nyckelkryptering nyckel för att kryptera hello genereras disk-krypteringsnyckeln. |
| vmName | Namnet på hello IaaS VM. |

#### <a name="using-powershell-cmdlets"></a>Med hjälp av PowerShell-cmdlets
Du kan aktivera kryptering på den kryptera virtuella Hårddisken med hjälp av PowerShell-cmdlet för hello [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>Med hjälp av CLI-kommandon
kryptering av tooenable för det här scenariot med hjälp av CLI-kommandona hello följande:

1. Ange åtkomstprinciper i nyckelvalvet:

   * Ange hello **EnabledForDiskEncryption** flaggan:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Ange behörigheter tooAzure AD programmet toowrite hemligheter tooyour nyckelvalv:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. tooenable kryptering på en befintlig eller körs VM typ:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Hämta krypteringsstatus:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. tooenable kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken, Använd hello följande parametrar med hello `azure vm create` kommando:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Aktivera kryptering på befintlig eller körs IaaS Windows VM i Azure
Du kan aktivera kryptering genom att använda hello Resource Manager-mall, PowerShell-cmdlets eller CLI-kommandona i det här scenariot. hello följande avsnitt beskrivs i detalj hur tooenable den med hjälp av hello Resource Manager-mall och CLI-kommandona.

#### <a name="using-hello-resource-manager-template"></a>Med hjälp av hello Resource Manager-mall
Du kan aktivera kryptering på befintliga eller IaaS Windows virtuella datorer som körs i Azure med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello kryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.

2. Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på hello befintlig eller använder IaaS-VM.

hello visas följande tabell hello Resource Manager mallparametrar för befintlig eller virtuella datorer som använder en Azure AD-klient-ID som körs:

| Parameter | Beskrivning |
| --- | --- |
| AADClientID | Klient-ID för hello Azure AD-program som har behörigheter toowrite hemligheter toohello nyckelvalvet. |
| AADClientSecret | Klienthemlighet för hello Azure AD-program som har behörigheter toowrite hemligheter toohello nyckelvalvet. |
| keyVaultName | Namnet på nyckeln hello valvet som hello BitLocker nyckel ska överföras till. Du kan hämta den med hjälp av cmdlet hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL för hello nyckelkryptering nyckel som används tooencrypt hello genereras BitLocker-nyckel. Den här parametern är valfri om du väljer **nokek** i hello UseExistingKek nedrullningsbara listan. Om du väljer **kek** i hello UseExistingKek nedrullningsbara listan, måste du ange hello _keyEncryptionKeyURL_ värde. |
| volumeType | Typ av volymen som hello krypteringsåtgärden utförs på. Giltiga värden är _OS_, _Data_, och _alla_. |
| sequenceVersion | Sekvens version av hello BitLocker igen. Öka det här versionsnumret varje gång en diskkryptering åtgärden utförs på hello samma virtuella dator. |
| vmName | Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på. |

> [!NOTE]
> _KeyEncryptionKeyURL_ är en valfri parameter. Du kan hämta din egen KEK toofurther skydda hello datakrypteringsnyckeln (BitLocker-kryptering hemliga) i hello nyckelvalvet.

#### <a name="using-powershell-cmdlets"></a>Med hjälp av PowerShell-cmdlets
Information om hur du aktiverar kryptering med Azure Disk Encryption med PowerShell-cmdlets finns hello blogginlägg [utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) och [utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>Med hjälp av CLI-kommandon
tooenable kryptering på befintlig eller använder IaaS Windows VM i Azure med hjälp av CLI-kommandona hello följande:

1. tooset åtkomstprinciper i hello nyckelvalvet:
   * Ange hello **EnabledForDiskEncryption** flaggan:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Ange behörigheter tooAzure AD programmet toowrite hemligheter tooyour nyckelvalv:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. tooenable kryptering på en befintlig eller körs VM:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. tooget krypteringsstatus:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. tooenable kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken, Använd hello följande parametrar med hello `azure vm create` kommando:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Aktivera kryptering på en befintlig eller körs IaaS Linux-dator i Azure
Du kan aktivera kryptering på en befintlig eller körs IaaS Linux-dator i Azure med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Klicka på **distribuera tooAzure** ange hello kryptering konfiguration på hello på hello Azure Snabbkurs mallen **parametrar** bladet och klicka sedan på **OK**.

2. Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på hello befintlig eller använder IaaS-VM.

hello i den följande tabellen listas Resource Manager mallparametrar för befintlig eller virtuella datorer som använder en Azure AD-klient-ID som körs:

| Parameter | Beskrivning |
| --- | --- |
| AADClientID | Klient-ID för hello Azure AD-program som har behörigheter toowrite hemligheter toohello nyckelvalvet. |
| AADClientSecret | Klienthemlighet för hello Azure AD-program som har behörigheter toowrite hemligheter tooyour nyckelvalvet. |
| keyVaultName | Namnet på nyckeln hello valvet som hello BitLocker nyckel ska överföras till. Du kan hämta den med hjälp av cmdlet hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL för hello nyckelkryptering nyckel som används tooencrypt hello genereras BitLocker-nyckel. Den här parametern är valfri om du väljer **nokek** i hello UseExistingKek nedrullningsbara listan. Om du väljer **kek** i hello UseExistingKek nedrullningsbara listan, måste du ange hello _keyEncryptionKeyURL_ värde. |
| volumeType | Typ av volymen som hello krypteringsåtgärden utförs på. Giltiga värden som stöds är _OS_ eller _alla_ (för RHEL 7.2, CentOS 7.2 och Ubuntu 16.04) och _Data_ (för alla andra distributioner). |
| sequenceVersion | Sekvens version av hello BitLocker igen. Öka det här versionsnumret varje gång en diskkryptering åtgärden utförs på hello samma virtuella dator. |
| vmName | Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på. |
| Lösenfrasen | Ange ett starkt lösenord som hello datakrypteringsnyckeln. |

> [!NOTE]
> _KeyEncryptionKeyURL_ är en valfri parameter. Du kan sätta egna KEK toofurther skydda hello datakrypteringsnyckeln (lösenfrasen hemlig) i nyckelvalvet.

#### <a name="cli-commands"></a>CLI-kommandon
Du kan aktivera kryptering på den kryptera virtuella Hårddisken genom att installera och använda hello [CLI kommandot](../cli-install-nodejs.md). tooenable kryptering på befintlig eller IaaS Linux virtuella datorer som körs i Azure med hjälp av CLI-kommandona hello följande:

1. Ange åtkomstprinciper i hello nyckelvalvet:

 * Ange hello **EnabledForDiskEncryption** flaggan:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Ange behörigheter tooAzure AD programmet toowrite hemligheter tooyour nyckelvalv:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. tooenable kryptering på en befintlig eller körs VM:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Hämta krypteringsstatus:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. tooenable kryptering på en ny virtuell dator från den kryptera virtuella Hårddisken, Använd hello följande parametrar med hello `azure vm create` kommando:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-hello-encryption-status-of-an-encrypted-iaas-vm"></a>Hämta hello krypteringsstatus för en krypterad IaaS-VM
Du kan hämta hello krypteringsstatus med Azure Resource Manager [PowerShell-cmdlets](/powershell/azure/overview), eller CLI-kommandona. hello följande avsnitt förklarar hur toouse hello klassiska Azure-portalen och CLI-kommandon tooget hello krypteringsstatus.

#### <a name="get-hello-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Hämta hello krypteringsstatus för en krypterad Windows-dator med hjälp av Azure Resource Manager
Du kan få hello krypteringsstatus för hello IaaS VM från Azure Resource Manager hello följande:

1. Logga in toohello [klassiska Azure-portalen](https://portal.azure.com/), och klicka sedan på **virtuella datorer** i hello vänster toosee en sammanfattning av hello virtuella datorer i din prenumeration. Du kan filtrera hello virtuella datorer vyn genom att välja hello prenumerationsnamn i hello **prenumeration** listrutan.

2. Hello överst i hello **virtuella datorer** klickar du på **kolumner**.

3. På hello **Välj kolumnen** bladet väljer **diskkryptering**, och klicka sedan på **uppdatering**. Du bör se hello diskkryptering kolumn som visar hello krypteringsstatus _aktiverad_ eller _inte aktiverat_ för varje VM som visas i följande bild hello:

 ![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-hello-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-hello-disk-encryption-powershell-cmdlet"></a>Hämta hello krypteringsstatus för en krypterad (Windows-/ Linux) IaaS VM med hjälp av PowerShell-cmdlet för hello-diskkryptering
Du kan hämta hello krypteringsstatus för hello IaaS VM från hello-diskkryptering PowerShell-cmdleten `Get-AzureRmVMDiskEncryptionStatus`. tooget hello krypteringsinställningar för den virtuella datorn anger hello följande:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Du kan inspektera hello utdata från _Get-AzureRmVMDiskEncryptionStatus_ nyckeln URL: er för kryptering.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

Hej OSVolumeEncrypted och DataVolumesEncrypted värden anges too_Encrypted_ som visar att båda volymerna krypteras med Azure Disk Encryption. Information om hur du aktiverar kryptering med Azure Disk Encryption med PowerShell-cmdlets finns hello blogginlägg [utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) och [utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> På Linux virtuella datorer, det tar tre toofour minuter hello `Get-AzureRmVMDiskEncryptionStatus` cmdlet tooreport hello krypteringsstatus.

#### <a name="get-hello-encryption-status-of-hello-iaas-vm-from-hello-disk-encryption-cli-command"></a>Hämta hello krypteringsstatus för hello IaaS VM från hello-diskkryptering CLI kommando
Du kan hämta hello krypteringsstatus för hello IaaS VM med kommandot hello-diskkryptering CLI `azure vm show-disk-encryption-status`. tooget hello krypteringsinställningar för den virtuella datorn, ange din Azure CLI-session:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Inaktivera kryptering på använder Windows IaaS-VM
Du kan inaktivera kryptering på en aktiv Windows eller Linux IaaS-VM via hello Azure Disk Encryption Resource Manager-mall eller PowerShell-cmdlets och ange hello dekryptering konfiguration.

##### <a name="windows-vm"></a>Windows VM
hello inaktivera kryptering steget inaktiverar kryptering av hello OS, hello datavolym eller båda på hello som använder Windows IaaS-VM. Du kan inte inaktivera hello systemvolymen och lämna hello datavolym krypteras. När hello inaktivera kryptering steg utförs hello Azure klassiska distributionsmodellen uppdaterar hello VM modell och hello Windows IaaS-VM är markerad dekrypterade. hello innehållet i hello VM krypteras inte längre i vila. hello dekryptering tar inte bort din key vault och hello kryptering nyckelmaterial (krypteringsnycklar BitLocker för Windows och lösenfrasen för Linux).

##### <a name="linux-vm"></a>Linux VM
hello inaktivera kryptering steget inaktiverar kryptering av hello datavolym på hello som kör Linux IaaS-VM. Det här steget fungerar bara om hello OS-disken inte krypteras.

> [!NOTE]
> Om du inaktiverar kryptering på hello OS-disken tillåts inte för virtuella Linux-datorer.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Inaktivera kryptering på en befintlig eller körs IaaS-VM
Du kan inaktivera diskkryptering på Windows IaaS virtuella datorer som körs med hjälp av hello [Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. Klicka på hello Azure Snabbkurs mallen **distribuera tooAzure**, ange hello dekryptering konfiguration på hello **parametrar** bladet och klicka sedan på **OK**.

2. Välj hello prenumeration, resursgrupp, resursgruppens plats, juridiska villkor och avtalet och klicka sedan på **skapa** tooenable kryptering på en ny IaaS-VM.

För Linux virtuella datorer kan du inaktivera kryptering med hjälp av hello [inaktivera kryptering på en körs Linux VM](https://aka.ms/decrypt-linuxvm) mall.

hello visas följande tabell mallparametrar för Resource Manager för inaktivering av kryptering på en körs IaaS-VM:

| Parameter | Beskrivning |
| --- | --- |
| vmName | Namnet på hello VM som hello krypteringsåtgärden är toobe utförs på.
| volumeType | Typ av volymen som en dekrypteringsåtgärd utförs på. Giltiga värden är _OS_, _Data_, och _alla_. Du kan inte inaktivera kryptering på kör Windows IaaS-VM OS/startvolymen utan att inaktivera kryptering på hello _Data_ volym. Observera också att om du inaktiverar kryptering på hello OS-disken inte tillåts för virtuella Linux-datorer. |
| sequenceVersion | Sekvens version av hello BitLocker igen. Öka det här versionsnumret varje gång en disk dekrypteringsåtgärden utförs på hello samma virtuella dator. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Inaktivera kryptering på en befintlig eller körs IaaS-VM
toodisable kryptering på en befintlig eller körs IaaS-VM med hjälp av PowerShell-cmdleten hello finns [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Denna cmdlet har stöd för både Windows- och Linux virtuella datorer. toodisable kryptering, ett tillägg installeras på hello virtuell dator. Om hello _namn_ parameter har angetts något tillägg med hello standardnamnet _AzureDiskEncryption för virtuella Windows-datorer_ skapas.

Hej AzureDiskEncryptionForLinux tillägget används på Linux virtuella datorer.

> [!NOTE]
> Denna cmdlet startas hello virtuell dator.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Aktivera kryptering på förkrypterade IaaS-VM med Azure-hanterade disken
Använd hello Azure hanteras Disk ARM-mall toocreate en krypterad virtuell dator från en förkrypterade virtuell Hårddisk med hello ARM-mall finns i   
[Skapa en ny krypterade hanterade disk från en förkrypterade VHD/storage-blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Aktivera kryptering på en ny Linux IaaS virtuell dator med hanterad Azure-disken
Använd hello Azure hanteras Disk ARM-mall toocreate en ny krypterade Linux IaaS-VM med hello ARM-mall finns i   
[Distributionen av RHEL 7.2 med fullständig diskkryptering] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Aktivera kryptering på en ny Windows IaaS-VM med Azure-hanterade disken
 Använd hello Azure hanteras Disk ARM-mall toocreate en ny krypterade Linux IaaS-VM med hello ARM-mall finns i   
 [Skapa en ny krypterade Windows IaaS hanteras Disk virtuell dator från galleriet avbildning] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >Det är obligatoriskt toosnapshot och/eller säkerhetskopiering hanterade diskar baserade VM-instans utanför och tidigare tooenabling Azure Disk Encryption.  En ögonblicksbild av hello hanterade diskar kan hämtas från hello-portalen eller Azure Backup kan användas.  Säkerhetskopieringar kan du kontrollera att ett återställningsalternativ är möjligt i hello fall av ett oväntat fel under krypteringen.  När en säkerhetskopia görs vara hello Set-AzureRmVMDiskEncryptionExtension cmdlet används tooencrypt hanterade diskar genom att ange hello skipVmBackup - parametern.  Det här kommandot misslyckas mot hanterade diskbaserat VM tills en säkerhetskopia som har gjorts och den här parametern har angetts.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Uppdatera krypteringsinställningarna för en befintlig krypterade icke-premium virtuell dator
  Använd hello befintliga Azure-disken kryptering stöds gränssnitt för att köra VM [PS-cmdlets, CLI eller ARM-mallar] tooupdate hello krypteringsinställningar som AAD klient ID/hemlig nyckel krypteringsnyckeln [KEK] BitLocker-krypteringsnyckeln för Windows-VM eller lösenfrasen för Linux VM etc. hello update krypteringsinställning stöds bara för virtuella datorer som backas upp av icke-premium-lagring. Det är NNOT stöd för virtuella datorer som backas upp av premium-lagring.

## <a name="appendix"></a>Bilaga
### <a name="connect-tooyour-subscription"></a>Ansluta tooyour prenumeration
Innan du fortsätter kan du granska hello *krav* i den här artikeln. När du har kontrollerat att alla krav har uppfyllts, ansluter du tooyour prenumeration hello följande:

1. Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:

    `Login-AzureRmAccount`

2. Om du har flera prenumerationer och vill toospecify en toouse, skriver du hello följande toosee hello prenumerationer för ditt konto:

    `Get-AzureRmSubscription`

3. toospecify hello prenumeration som du vill toouse, typ:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. tooverify att hello prenumeration konfigurerats är korrekt, typ:

    `Get-AzureRmSubscription`

5. tooconfirm hello Azure Disk Encryption är installerade, typ:

    `Get-command *diskencryption*`

6. följande utdata hello bekräftar hello Azure Disk Encryption PowerShell-installationen:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Förbereda en förkrypterade Windows VHD
hello avsnitten som följer är nödvändiga tooprepare en förkrypterade Windows VHD för distribution som en krypterad VHD i Azure IaaS. Använd hello information tooprepare och starta en ny Windows VM (VHD) på Azure Site Recovery eller Azure.

#### <a name="update-group-policy-tooallow-non-tpm-for-os-protection"></a>Uppdatera grupp princip tooallow utan TPM för OS-skydd
Konfigurera hello BitLocker grupprincipinställningen **BitLocker-diskkryptering**, som du hittar **lokal datorprincip** > **Datorkonfiguration**  >  **Administrationsmallar** > **Windows-komponenter**. Ändra inställningen för**operativsystemsenheter** > **kräver ytterligare autentisering vid start** > **Tillåt BitLocker utan en kompatibel TPM**, enligt följande bild hello:

![Microsoft Antimalware i Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>Installera komponenter för BitLocker-funktion
För Windows Server 2012 och senare, Använd hello följande kommando:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

För Windows Server 2008 R2, använder du hello följande kommando:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-hello-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Förbereda hello systemvolymen för BitLocker med hjälp av`bdehdcfg`
toocompress hello OS-partitionen och Förbered hello datorn för BitLocker, köra hello följande kommando:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-hello-os-volume-by-using-bitlocker"></a>Skydda hello systemvolymen med BitLocker
Använd hello [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) kommandot tooenable kryptering på hello startvolymen med hjälp av ett externt nyckelskydd. Också placera hello extern nyckel (.bek-fil) på hello extern enhet eller volym. Kryptering är aktiverat på hello system/startvolymen efter hello nästa omstart.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> Förbered hello VM med en separat/Dataresurs VHD för att hämta hello externa nyckeln med hjälp av BitLocker.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Kryptera en OS-enheten på en Linux-VM som körs
Kryptering av en OS-enhet på en Linux-VM som körs stöds på hello följande distributioner:

* RHEL 7.2
* CentOS 7.2
* Ubuntu 16.04

##### <a name="prerequisites-for-os-disk-encryption"></a>Krav för OS-diskkryptering

* hello VM måste skapas från hello Marketplace-avbildning i Azure Resource Manager.
* Azure virtuell dator med minst 4 GB RAM-minne (rekommenderas storleken är 7 GB).
* (För RHEL och CentOS) Inaktivera SELinux. toodisable SELinux, se ”4.4.2. Inaktivera SELinux ”i hello [SELinux användar- och Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) på hello VM.
* När du inaktiverar SELinux startas hello VM minst en gång.

##### <a name="steps"></a>Steg
1. Skapa en virtuell dator med någon av hello distributioner har angett tidigare.

 OS-diskkryptering stöds för CentOS 7.2 via en särskild avbildning. toouse det bild, ange ”7.2n” som hello SKU när du skapar hello VM:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Konfigurera hello VM bl.a tooyour behov. Om du ska tooencrypt alla hello (OS + data)-enheter behöver hello dataenheter toobe angivna och monteras från /etc/fstab.

 > [!NOTE]
 > Använd UUID =... toospecify dataenheter i/etc/fstab istället för att ange hello blockera enhetens namn (till exempel /dev/sdb1). Hej ordningen på enheter ändringar på hello VM under krypteringen. Om den virtuella datorn är beroende av en viss ordning för blockera enheter, misslyckas toomount dem efter kryptering.

3. Logga ut från hello SSH-sessioner.

4. tooencrypt hello OS, ange volumeType som **alla** eller **OS** när du [Aktivera kryptering](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Alla användare kan processer som inte körs som `systemd` tjänster ska avslutas med en `SIGKILL`. Starta om hello VM. När du aktiverar OS-diskkryptering på en aktiv virtuell dator planera VM driftstopp.

5. Regelbundet övervaka hello kryptering genom hello instruktionerna i hello [nästa avsnitt](#monitoring-os-encryption-progress).

6. När Get-AzureRmVmDiskEncryptionStatus visar ”VMRestartPending”, startar du om den virtuella datorn genom att logga in tooit eller genom att använda hello-portalen, PowerShell eller CLI.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot hello VM
    ```
Innan du startar om rekommenderar vi att du sparar [starta diagnostik](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) av hello VM.

#### <a name="monitoring-os-encryption-progress"></a>Övervaka förloppet för OS-kryptering
Du kan övervaka förloppet för OS-kryptering på tre sätt:

* Använd hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet och inspektera hello ProgressMessage fält:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 När hello VM når ”OS-disken kryptering igång”, tar ungefär 40 too50 minuter på en Premium-lagring säkerhetskopieras VM.

 Eftersom [utfärda #388](https://github.com/Azure/WALinuxAgent/issues/388) i WALinuxAgent, `OsVolumeEncrypted` och `DataVolumesEncrypted` visas som `Unknown` i vissa distributioner. Med WALinuxAgent version 2.1.5 och senare kan det här problemet löses automatiskt. Om du ser `Unknown` hello utdata och du kan kontrollera disk krypteringsstatus med hjälp av hello Azure Resursläsaren.

 Gå för[resursutforskaren Azure](https://resources.azure.com/), och expandera sedan den här hierarkin i hello markeringen panelen vänster:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 Rulla ned toosee hello krypteringsstatus enheter i hello InstanceView.

 ![VM-instansvyn](./media/azure-security-disk-encryption/vm-instanceview.png)

* Titta på [starta diagnostik](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/). Meddelanden från hello ADE tillägget prefixet `[AzureDiskEncryption]`.

* Logga in toohello VM via SSH och hämta hello tillägget loggen:

    /var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Vi rekommenderar att du inte loggar in toohello VM när OS-kryptering pågår. Kopiera hello loggar endast när hello andra två metoder har misslyckats.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Förbereda en förkrypterade Linux VHD
##### <a name="ubuntu-16"></a>Ubuntu 16
Konfigurera kryptering under installationen av hello distribution hello följande:

1. Välj **konfigurera krypterade volymer** när du partitionera hello diskar.

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Skapa en separat startenheten som inte får vara krypterade. Kryptera din rotenhet.

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Ange en lösenfras. Detta är hello lösenfras som du överför toohello nyckelvalvet.

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Slut partitionering.

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. När du startar hello VM och ange en lösenfras använda hello lösenfras som du angav i steg 3.

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Förbereda hello VM för överföring till Azure med hjälp av [instruktionerna](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Kör inte hello sista steget (avställningsskript hello VM) ännu.

Konfigurera kryptering toowork med Azure hello följande:

1. Skapa en fil under /usr/local/sbin/azure_crypt_key.sh, med hello innehåll i hello följande skript. Betala uppmärksamhet toohello KeyFileName, eftersom det är hello lösenfras filnamn som används av Azure.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED toofind suitable passphrase file ..." >&2
        echo -n "Try tooenter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. Ändra hello crypt config i */etc/crypttab*. Det bör se ut så här:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Om du redigerar *azure_crypt_key.sh* i Windows och du har kopierat tooLinux, kör `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Lägg till körrättigheter toohello skript:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Redigera */etc/initramfs-tools/modules* genom att lägga till rader:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Kör `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` börjar gälla.

7. Nu kan du ta bort etableringen hello VM.

 ![Ubuntu 16.04 installationen](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Fortsätta toohello nästa steg och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2
tooconfigure kryptering under installationen av hello distribution hello följande:
1. När du partitionera hello diskar väljer **kryptera volymen grupp**, och sedan ange ett lösenord. Detta är hello lösenord som du kommer att överföra tooyour nyckelvalvet.

 ![Konfigurera openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. Starta hello VM som använder ditt lösenord.

 ![Konfigurera openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Förbered hello VM för uppladdning av tooAzure genom att följa instruktionerna hello i [förbereda en virtuell dator SLES eller openSUSE för Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Kör inte hello sista steget (avställningsskript hello VM) ännu.

tooconfigure kryptering toowork med Azure, hello följande:
1. Redigera hello /etc/dracut.conf och Lägg till följande rad hello:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Kommentera ut dessa rader hello slutet av hello filen /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. Lägg till följande rad hello början av hello filen /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello:
 ```
    DRACUT_SYSTEMD=0
 ```
Och ändra alla förekomster av:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
till:
```
    if [ 1 ]; then
```
4. Redigera /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh och Lägg till för ”# öppna LUKS enhet”:

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Kör `/usr/sbin/dracut -f -v` tooupdate hello initrd.

6. Nu du kan ta bort etableringen hello VM och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.

##### <a name="centos-7"></a>CentOS 7
tooconfigure kryptering under installationen av hello distribution hello följande:
1. Välj **kryptera data** när du partitionera diskarna.

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Kontrollera att **kryptera** har valts för rot-partitionen.

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Ange en lösenfras. Detta är hello lösenfras som du kommer att överföra tooyour nyckelvalvet.

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. När du startar hello VM och ange en lösenfras använda hello lösenfras som du angav i steg 3.

 ![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Förbered hello VM för överföring till Azure med hjälp av hello ”CentOS 7.0 +” instruktionerna i [förbereda en CentOS-baserad virtuell dator för Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Kör inte hello sista steget (avställningsskript hello VM) ännu.

6. Nu du kan ta bort etableringen hello VM och [överför den virtuella Hårddisken](#upload-encrypted-vhd-to-an-azure-storage-account) till Azure.

tooconfigure kryptering toowork med Azure, hello följande:

1. Redigera hello /etc/dracut.conf och Lägg till följande rad hello:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Kommentera ut dessa rader hello slutet av hello filen /usr/lib/dracut/modules.d/90crypt/module-setup.sh:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. Lägg till följande rad hello början av hello filen /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh hello:
```
    DRACUT_SYSTEMD=0
```
Och ändra alla förekomster av:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
till
```
    if [ 1 ]; then
```
4. Redigera /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh och Lägg till detta efter hello ”# öppna LUKS enhet”:
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Kör hello ”/ usr/sbin/dracut - f - v” tooupdate hello initrd.

![CentOS 7 installationen](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-tooan-azure-storage-account"></a>Överför krypterade VHD tooan Azure storage-konto
När BitLocker-kryptering eller DM-Crypt kryptering har aktiverats måste krypteras hello lokala VHD måste toobe upp tooyour storage-konto.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-hello-disk-encryption-secret-for-hello-pre-encrypted-vm-tooyour-key-vault"></a>Överför hello-diskkryptering hemligheten för hello förkrypterade VM tooyour nyckelvalv
hello-diskkryptering hemlighet som du hämtade måste tidigare laddas upp som en hemlighet i nyckelvalvet. Hej nyckelvalv måste toohave diskkryptering och behörigheter som har aktiverats för din Azure AD-klient.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Disk encryption hemlighet inte krypterats med en KEK
tooset in hello hemlighet i nyckelvalvet, Använd [Set AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Om du har en virtuell dator för Windows hello bek filen kodas som en base64-sträng och överföra tooyour nyckelvalv med hello `Set-AzureKeyVaultSecret` cmdlet. För Linux hello lösenfras kodas som en base64-sträng och överföra toohello nyckelvalvet. Dessutom se till att hello följande taggar anges när du skapar hello hemlighet i nyckelvalvet hello.

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Använd hello `$secretUrl` i hello nästa steg för [kopplar hello OS-disk utan att använda KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Disk encryption hemlighet krypteras med en KEK
Innan du laddar upp hello hemliga toohello nyckelvalv kryptera om du vill den med hjälp av en nyckel krypteringsnyckel. Använd hello radbyte [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst kryptera hello hemligheten med hello viktiga krypteringsnyckeln. hello utdata från åtgärden radbyte är en base64-URL-kodade sträng som du kan sedan överföra som en hemlighet hello [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

Använd `$KeyEncryptionKey` och `$secretUrl` i hello nästa steg för [kopplar hello OS-disk med hjälp av KEK](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Ange en hemlig URL när du ansluter en OS-disk
#### <a name="without-using-a-kek"></a>Utan att använda en KEK
När du bifogar hello OS-disk, behöver du toopass `$secretUrl`. hello URL har genererats i hello ”Disk encryption hemlighet inte krypterats med en KEK” avsnittet.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Med hjälp av en KEK
När du ansluter hello OS-disk, skicka `$KeyEncryptionKey` och `$secretUrl`. hello URL har genererats i hello ”Disk encryption hemlighet inte krypterats med en KEK” avsnittet.

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a>Hämta denna guide
Du kan hämta den här guiden från hello [TechNet-galleriet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

## <a name="for-more-information"></a>Mer information
[Utforska Azure Disk Encryption med Azure PowerShell - del 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Utforska Azure Disk Encryption med Azure PowerShell - del 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
