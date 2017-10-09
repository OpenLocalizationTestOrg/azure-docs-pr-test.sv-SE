---
title: "Felsökning av Disk Encryption aaaAzure | Microsoft Docs"
description: "Den här artikeln innehåller felsökningstips för Microsoft Azure Disk Encryption för Windows och Linux virtuella IaaS-datorer."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: ce0e23bd-07eb-43af-a56c-aa1a73bdb747
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: devtiw
ms.openlocfilehash: 2ecb8df1fb869e3bf8f3be4be4494e6485e75695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Felsökningsguide för Azure Disk Encryption

Den här guiden är för arbetet för IT-proffs, information säkerhetsanalytiker och problem som rör molnet administratörer vars organisationer som använder Azure disk encryption och måste vägledning tootroubleshoot-diskkryptering.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Felsökning av Linux OS-diskkryptering

Linux OS-diskkryptering måste demontera hello OS enheten tidigare toorunning det via hello fullständig krypteringsprocessen.   Om inte ett felmeddelande för ”kunde inte toounmount efter...” Felmeddelandet är sannolikt toooccur.

Detta är troligen när OS-diskkryptering görs på en målmiljön VM som har ändrats eller har ändrats från dess stöds lager galleriet avbildning.  Exempel på avvikelser från hello stöds avbildningen, som kan störa hello tillägget möjlighet toounmount hello OS enhet:
- Anpassade avbildningar som matchar inte längre ett filsystem som stöds och/eller partitioneringsschema.
- Anpassade avbildningar med program, till exempel antivirusprogram, Docker, SAP, MongoDB eller Apache Cassandra körs i hello OS tidigare tooencryption.  Dessa program är svåra tooterminate och när de behåller öppna handtag toohello OS enhet hello enheten kan inte omonterade orsakar fel.
- Anpassade skript som körs i Stäng tid närhet toohello kryptering steg kan påverka och orsaka det här felet. Detta kan inträffa när en Resource Manager-mall definierar flera tillägg tooexecute samtidigt, eller när tillägget för anpassat skript eller annan åtgärd körs samtidigt toodisk kryptering.   Serialisering och isolera sådana åtgärder kan lösa problemet med hello.
- När SELinux inte har inaktiverat tidigare tooenabling kryptering, demontera hello steget misslyckas.  SELinux kan aktiveras igen efter kryptering har slutförts.
- När hello OS-disken använder ett LVM schema (även om det finns stöd för diskar begränsad LVM data LVM OS-disken är inte)
- När minimikraven på minne är inte uppfyllda (7GB rekommenderas för OS-diskkryptering)
- När dataenheter har rekursivt monteras under /mnt/ katalog eller varandra (till exempel /mnt/data1, /mnt/data2, /data3 + /data3/data4 osv.)
- När andra Azure Disk Encryption [krav](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) för Linux är inte uppfyllda

## <a name="unable-tooencrypt"></a>Det går inte tooencrypt

I vissa fall visas hello Linux-diskkryptering toobe fastnat på ”OS diskkryptering igång” och SSH är inaktiverat. Den här processen kan ta mellan toocomplete 3-16 timmar på en bild för lager galleriet.  Om flera TB datadiskar läggs kan hello ta dagar. hello Linux OS-disk kryptering sekvens demonterar hello OS enhet tillfälligt och utför block för block kryptering av hello hela OS-disk innan ommontering i det kryptera tillståndet.   Till skillnad från Azure Disk Encryption i Windows tillåter Linux-diskkryptering inte samtidig användning av hello VM medan hello kryptering pågår.  hello prestandaegenskaper för hello VM, bland annat hello storlek hello disken och om hello lagringskonto backas upp av standard- eller premium (SSD) lagring kan avsevärt påverka hello tid som krävs för toocomplete kryptering.

status för toocheck hello ProgressMessage fält som returneras från hello [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) kommando kan användas.   Medan hello OS enheten krypteras hello VM försätts i tillståndet underhåll och SSH är också inaktiverade tooprevent avbrott toohello pågående processer.  EncryptionInProgress rapporteras för hello merparten av hello tid medan kryptering pågår, följt av flera timmar senare med ett VMRestartPending-meddelande om att toorestart hello VM.  Exempel:


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot hello VM
```

Du uppmanas när tooreboot hello VM och efter omstart hello VM, och ger 2 – 3 minuter för hello omstart och sista stegen toobe utförs på hello mål, hello statusmeddelande visar att kryptering slutligen har slutförts.   När det här meddelandet är tillgänglig, hello krypterade OS-enheten är förväntade toobe redo för användning och hello VM toobe kan användas igen.

I fall där den här sekvensen inte påträffades, eller om boot information, pågående meddelande eller andra felindikatorer rapportera att OS-kryptering misslyckades hello mitten av den här processen (till exempel om du ser hello ”misslyckades toounmount” fel som beskrivs i den här guiden) Det rekommenderas toorestore hello VM tillbaka toohello ögonblicksbild eller säkerhetskopior som gjorts omedelbart före tooencryption.  Tidigare toohello nästa försök, är det föreslagna toore-utvärdera hello VM hello egenskaper och se till att alla krav är uppfyllda.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Felsöka Azure Disk Encryption bakom en brandvägg
När anslutningen begränsas av en brandvägg, proxy krav eller grupp (NSG) för nätverkssäkerhet, hello möjligheten för hello tillägget tooperform behövs uppgifter kan avbrytas.   Detta kan resultera i statusmeddelanden, till exempel ”tillståndets status är inte tillgänglig på hello VM” och i förväntade scenarier misslyckas toofinish.  hello avsnitten som följer har några vanliga brandväggsproblem som du kan undersöka.

### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
Nätverket grupp säkerhetsinställningar tillämpas fortfarande måste tillåta hello toomeet hello dokumenterade nätverket slutpunktskonfiguration [krav](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) för diskkryptering.

### <a name="azure-keyvault-behind-firewall"></a>Azure Keyvault bakom brandväggen
hello VM måste vara kan tooaccess nyckelvalvet. Se tooguidance på åtkomst tookey fel finns bakom en brandvägg som underhålls av hello [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) team.

### <a name="linux-package-management-behind-firewall"></a>Linux-paketet management bakom brandväggen
Azure Disk Encryption för Linux beroende hello mål distribution paketet management system tooinstall behövs nödvändiga komponenter tidigare tooenabling kryptering vid körning.  Om brandväggsinställningar förhindra hello VM kan toodownload och installera komponenterna, förväntas efterföljande försök.    hello steg tooconfigure detta kan variera efter distributionen.  På Red Hat säkerställer att prenumerationen manager och yum har ställts in korrekt det är viktigt att när en proxyserver krävs.  Se [detta](https://access.redhat.com/solutions/189533) Red Hat support-artikeln om det här avsnittet.  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Felsöka Windows Server 2016 Server Core

Hej bdehdcfg komponenten är inte tillgängligt som standard på Windows Server 2016 Server Core. Den här komponenten krävs av Azure Disk Encryption.

tooworkaround problemet genom att kopiera hello följande 4 filer från en Windows Server 2016 Data Center VM toohello c:\windows\system32 mapp för hello Server Core bild:

```
bdehdcfg.exe
bdehdcfglib.dll
bdehdcfglib.dll.mui
bdehdcfg.exe.mui
```

Kör följande kommando hello:

```
bdehdcfg.exe -target default
```

Detta skapar en partition 550MB och sedan efter en omstart, du kan använda Diskpart toocheck hello volymer och fortsätta.  

Exempel:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
## <a name="see-also"></a>Se även
I det här dokumentet du lära dig mer om några vanliga problem i Azure disk encryption och hur tootroubleshoot. Mer information om den här tjänsten och dess funktioner:

- [Tillämpa diskkryptering i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Kryptera en virtuell Azure-dator](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure Data kryptering i vila](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
