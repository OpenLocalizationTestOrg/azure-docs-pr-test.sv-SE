---
title: "aaaAzure Disk Encryption vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln innehåller svar toofrequently frågor för Microsoft Azure Disk Encryption för Windows och Linux virtuella IaaS-datorer."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw
ms.openlocfilehash: 17f084628ba4ef22e9d37dd3052ef10f8eb2cd7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-frequently-asked-questions-faq"></a>Azure Disk Encryption vanliga frågor (FAQ)

Det här avsnittet får du svar frågor om Azure-diskkryptering för Windows och Linux IaaS-VM för mer information om den här tjänsten, läsa [Azure Disk Encryption för Windows och Linux IaaS-VM](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Allmänna frågor
**F.** Vilken region är Azure disk encryption i GA?

**S:** Azure disk encryption för Windows och Linux virtuella IaaS-datorer finns i GA i alla offentliga Azure-regioner.

**F:** vad användaren upplever är tillgängliga med Azure Disk Encryption?

**S:** Azure Disk Encryption GA stöder Azure Resource Manager-mallar, Azure PowerShell, Azure CLI. Detta ger dig stor flexibilitet i som du har tre olika alternativ för att aktivera kryptering för IaaS-VM. Mer information om hello användarupplevelse och steg-för-steg-anvisningar finns i hello Azure Disk Encryption distributionsscenarier och erfarenheter.

**F:** hur mycket kostar Azure Disk Encryption?

**S:** är gratis för kryptering av Virtuella diskar med Azure Disk Encryption.

**F:** vilka nivåer av virtuell dator kan du använda Azure Disk Encryption med?

**S:** Azure Disk Encryption är endast tillgänglig på virtuella datorer som Standard nivå inklusive [A, D, DS, G, GS, F](https://azure.microsoft.com/pricing/details/virtual-machines/) och så vidare serien IaaS-VM inklusive virtuella datorer med premium-lagring. Det är inte tillgänglig på virtuella datorer grundläggande nivån.

**F:** vad Linux-distributioner som stöds av Azure Disk Encryption?

**S:** Azure Disk Encryption stöds på hello följande Linux server-distributioner och versioner:

| Linux-Distribution | Version | Volymtyp som stöds för kryptering|
| --- | --- |--- |
| Ubuntu | 16.04-VARJE DAG-LTS | OS- och disk |
| Ubuntu | 14.04.5-DAILY-LTS | OS- och disk |
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
| SLES | Prioritet: 12-SP1 | Datadisk |
| SLES | HPC 12 | Datadisk |
| SLES | Prioritet: 11-SP4 | Datadisk |
| SLES | 11 SP4 | Datadisk |

**F:** hur kan jag igång med Azure Disk Encryption?

**S:** kunder kan lära dig hur tooget igång genom att läsa hello Azure Disk Encryption whitepaper finns [här](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

**F:** kan jag kryptera både start- och datavolymer med Azure Disk Encryption?

**S:** Ja, du kan kryptera start- och datavolymer för Windows och Linux virtuella IaaS-datorer. För virtuella Windows-datorer kan kryptera du inte hello data utan första encrpting hello systemvolymen. För Linux virtuella datorer kan kryptera du hello datavolym utan encryptinng hello systemvolymen först. När du har krypterat hello OS volym för Linux, stöds om du inaktiverar kryptering på en OS-volym för Linux virtuella IaaS-datorer inte

**F:** har Azure Disk Encryption aktivera en ”bring your own key” (BYOK) kapaciteten?

**S:** Ja, du kan ange en egen nyckel krypteringsnycklar. Dessa nycklar skyddas i Azure Key Vault som är hello KeyStore för Azure Disk Encryption. Mer information om hello viktiga krypteringsnyckeln stöd för scenarier, se hello Azure Disk Encryption distributionsscenarier och upplevelser

**F:** kan jag använda en Azure-skapade viktiga krypteringsnyckel?

**S:** Ja, du kan använda Azure Key vault toogenerate nyckelkryptering nyckel för Azure disk encryption användning. Dessa nycklar skyddas i Azure Key Vault som är hello KeyStore för Azure Disk Encryption. Mer information om hello viktiga krypteringsnyckeln stöd för scenarier, se hello Azure Disk Encryption distributionsscenarier och upplevelser

**F:** kan du använda lokala key management service/HSM toosafeguard hello krypteringsnycklar?

**S:** du inte använda hello lokalt key management service/HSM toosafeguard hello krypteringsnycklarna med Azure disk encryption. Du kan bara använda hello Azure key vault-tjänsten toosafeguard hello krypteringsnycklar. Mer information om hello viktiga krypteringsnyckeln stöd för scenarier, se hello Azure Disk Encryption distributionsscenarier och upplevelser

**F:** vad är hello krav tooconfigure Azure disk encryption?

**S:** hello Azure disk encryption nödvändiga PowerShell-skriptet toocreate AAD-program, skapa nya nyckelvalv eller konfigurera befintliga nyckelvalvet för disk encryption åtkomst tooenable kryptering och skydda hemligheter och nyckel.  Mer information om hello viktiga krypteringsnyckeln stöd för scenarier, se hello Azure Disk Encryption krav och distributionsscenarier och upplevelser

**F:** där får mer information om hur toouse PowerShell för att konfigurera Azure Disk Encryption?

**S:** vi har några bra artiklar om hur du kan utföra grundläggande uppgifter för Azure Disk Encryption, samt mer avancerade scenarier. Kolla för hello grundläggande uppgifter, utforska [Azure Disk Encryption med Azure PowerShell - del 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). För mer avancerade scenarier finns utforska [Azure Disk Encryption med Azure PowerShell – del 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/)

**F:** vilken version av Azure PowerShell stöds av Azure Disk Encryption?

**S:** Använd hello senaste versionen av Azure PowerShell SDK version tooconfigure Azure Disk Encryption. Hämta hello senaste versionen av [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption stöds inte av Azure SDK version 1.1.0.

> [!NOTE]
> hello Linux Azure disk encryption preview tillägget är föråldrad. Mer information finns i toodocumentation [här](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/)

**F:** kan jag använda Azure disk encryption på min anpassade Linux-avbildningen?

**S:** du kan inte använda Azure disk encryption på den anpassade Linux-avbildningen. Vi stöder endast hello Linux galleriavbildningar för hello stöds distributioner som beskrivs ovan. Vi stöder inte anpassade Linux avbildningar för närvarande

**F:** kan jag lägga till uppdateringar tooa Linux Red Hat VM med hjälp av Yum update?

**S:** Ja, du kan utföra en uppdatering och eller korrigera Red Hat Linnux VM följa instruktionerna [här](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/)

**F:** var kan jag gå tooask fråga eller ge feedback

**S:** kan du ange ställa frågor eller feedback om hello Azure disk encryption forum [här](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption)

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig mer om hello vanligast förekommande frågor relaterade tooAzure diskkryptering, mer information om den här tjänsten och sin förmåga att läsa:

- [Tillämpa diskkryptering i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Kryptera en virtuell Azure-dator](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Azure Data kryptering i vila](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
