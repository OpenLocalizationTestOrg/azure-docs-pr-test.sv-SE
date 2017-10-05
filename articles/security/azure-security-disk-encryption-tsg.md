---
title: "Azure Disk Encryption felsökning | Microsoft Docs"
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
ms.openlocfilehash: 5f482a92b8fcd71a1b767fcc5741bc57605997ea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Felsökningsguide för Azure Disk Encryption

Den här guiden är för arbetet för IT-proffs, information security analytiker och molnet administratörer vars organisationer som använder Azure disk encryption och måste vägledning för att felsöka diskkryptering relaterade problem.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Felsökning av Linux OS-diskkryptering

Linux OS-diskkryptering måste demontera OS-enheten innan du kör med fullständig disk krypteringsprocessen.   Om inte ett felmeddelande för ”det gick inte att demontera efter...” Felmeddelandet är sannolikt att.

Detta är troligen när OS-diskkryptering görs på en målmiljön VM som har ändrats eller har ändrats från dess stöds lager galleriet avbildning.  Exempel på avvikelser från stöds avbildningen, som kan störa tilläggets möjlighet att demontera OS-enhet:
- Anpassade avbildningar som matchar inte längre ett filsystem som stöds och/eller partitioneringsschema.
- Anpassade avbildningar med program, till exempel antivirusprogram, Docker, SAP, MongoDB eller Apache Cassandra körs i OS före kryptering.  Dessa program är svåra att avsluta och när de behåller öppna filreferenser på OS-enhet på enheten kan inte omonterade orsakar fel.
- Anpassade skript som körs i Stäng tid närhet till steget kryptering kan påverka och orsaka det här felet. Detta kan inträffa när en Resource Manager-mall definierar flera tillägg för att köra samtidigt, eller när tillägget för anpassat skript eller annan åtgärd som körs samtidigt för diskkryptering.   Serialisering och isolera sådana åtgärder kan lösa problemet.
- När SELinux inte har inaktiverats innan du aktiverar kryptering, misslyckas koppla från steget.  SELinux kan aktiveras igen efter kryptering har slutförts.
- När OS-disken använder ett LVM schema (även om det finns stöd för diskar begränsad LVM data LVM OS-disken är inte)
- När minimikraven på minne är inte uppfyllda (7GB rekommenderas för OS-diskkryptering)
- När dataenheter har rekursivt monteras under /mnt/ katalog eller varandra (till exempel /mnt/data1, /mnt/data2, /data3 + /data3/data4 osv.)
- När andra Azure Disk Encryption [krav](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) för Linux är inte uppfyllda

## <a name="unable-to-encrypt"></a>Det gick inte att kryptera

I vissa fall har Linux diskkryptering verkar ha fastnat i ”OS-disken kryptering igång” och SSH inaktiverats. Den här processen kan ta mellan 3-16 timmar att slutföra en bild lager galleriet.  Om flera TB datadiskar läggs kan processen ta dagar. Linux-Operativsystemdatorn disk encryption sekvensen demonterar enhetens OS tillfälligt och utför block för block kryptering av hela OS-disken innan ommontering i det kryptera tillståndet.   Till skillnad från Azure Disk Encryption i Windows tillåter Linux-diskkryptering inte samtidig användning av den virtuella datorn medan kryptering pågår.  Prestandaegenskaperna för den virtuella datorn, inklusive storleken på disken och om lagringskontot backas upp av standard- eller premium (SSD) lagring kan avsevärt påverka den tid som krävs till fullständig kryptering.

Om du vill kontrollera status för fältet ProgressMessage som returnerades från den [Get-AzureRmVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) kommando kan användas.   När OS-enheten krypteras den virtuella datorn försätts i tillståndet underhåll och SSH också inaktiveras för att förhindra eventuella avbrott i den pågående processen.  EncryptionInProgress rapporteras för flesta av tiden medan kryptering pågår, följt av flera timmar senare med en VMRestartPending meddelande där uppmanas att starta om den virtuella datorn.  Exempel:


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
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

När du uppmanas att starta om den virtuella datorn efter att starta om den virtuella datorn och ger 2 – 3 minuter för datorn startats och sista stegen utförs på måldatorn, visar statusmeddelandet att kryptering slutligen har slutförts.   När det här meddelandet är tillgänglig, förväntas den krypterade enheten OS vara redo för användning och för den virtuella datorn att användas igen.

I fall där den här sekvensen inte påträffades, eller om boot information, pågående meddelande eller andra felindikatorer rapportera att OS-kryptering misslyckades mitt i den här processen (till exempel om du ser felet ”Det gick inte att demontera” som beskrivs i den här guiden), rekommenderas att återställa den virtuella datorn till ögonblicksbild eller säkerhetskopior som gjorts omedelbart före kryptering.  Innan nästa försök rekommenderas att omvärdera egenskaperna för den virtuella datorn och se till att alla krav är uppfyllda.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Felsöka Azure Disk Encryption bakom en brandvägg
När anslutningen är begränsad av en brandvägg, proxy krav eller grupp (NSG) för nätverkssäkerhet, kan möjligheten att utföra uppgifter som krävs av tillägget avbrytas.   Detta kan resultera i statusmeddelanden, till exempel ”tillståndets status är inte tillgänglig på den virtuella datorn” och i förväntade scenarier som inte kan slutföras.  Avsnitten som följer har några vanliga brandväggsproblem som du kan undersöka.

### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
En grupp för nätverkssäkerhet tillämpas fortfarande måste tillåta slutpunkten att uppfylla dokumenterade nätverkskonfigurationen [krav](https://docs.microsoft.com/azure/security/azure-security-disk-encryption#prerequisites) för diskkryptering.

### <a name="azure-keyvault-behind-firewall"></a>Azure Keyvault bakom brandväggen
Den virtuella datorn måste kunna komma åt nyckelvalvet. Referera till hjälp med åtkomst till viktiga fel finns bakom en brandvägg som underhålls av den [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall) team.

### <a name="linux-package-management-behind-firewall"></a>Linux-paketet management bakom brandväggen
Azure Disk Encryption för Linux bygger på mål-distribution systemet för pakethantering att installera nödvändiga komponenter innan du aktiverar kryptering vid körning.  Brandväggsinställningar förhindrar att den virtuella datorn från att kunna ladda ned och installera komponenterna, förväntas efterföljande försök.    Hur du konfigurerar detta kan variera av distribution.  På Red Hat säkerställer att prenumerationen manager och yum har ställts in korrekt det är viktigt att när en proxyserver krävs.  Se [detta](https://access.redhat.com/solutions/189533) Red Hat support-artikeln om det här avsnittet.  

## <a name="troubleshooting-windows-server-2016-server-core"></a>Felsöka Windows Server 2016 Server Core

Bdehdcfg-komponenten är inte tillgängligt som standard på Windows Server 2016 Server Core. Den här komponenten krävs av Azure Disk Encryption.

För att lösa problemet, kopiera följande 4 filer från en Windows Server 2016 Data Center VM till mappen c:\windows\system32 för Server Core-avbildningen:

```
bdehdcfg.exe
bdehdcfglib.dll
bdehdcfglib.dll.mui
bdehdcfg.exe.mui
```

Kör sedan följande kommando:

```
bdehdcfg.exe -target default
```

Detta skapar en partition 550MB och sedan efter en omstart, du kan använda Diskpart Kontrollera volymerna och fortsätta.  

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
I det här dokumentet du lära dig mer om några vanliga problem i Azure disk encryption och hur du felsöker. Mer information om den här tjänsten och dess funktioner:

- [Tillämpa diskkryptering i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Kryptera en virtuell Azure-dator](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure Data kryptering i vila](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
