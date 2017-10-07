---
title: kryptering av aaaApply i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** gäller disk encryption **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: cd803f1120018c5c86da91186eec1e59d425ede7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Tillämpa diskkryptering i Azure Security Center
Azure Security Center rekommenderar att du tillämpar kryptering om du har Windows eller Linux VM diskar som inte är krypterade med hjälp av Azure Disk Encryption. Diskkryptering kan du kryptera din Windows- och Linux IaaS-VM-diskar.  Kryptering rekommenderas för både hello Operativsystemet och datavolymer på den virtuella datorn.

Diskkryptering använder hello branschstandard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux. De här funktionerna ger OS och data kryptering toohelp skydda och skydda dina data och uppfylla din organisations säkerhet och efterlevnad åtaganden. Kryptering är integrerad med [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp du styra och hantera hello disk krypteringsnycklar och hemligheter i prenumerationen Key Vault samtidigt som du säkerställer att krypteras alla data i hello Virtuella diskar i vila i din [ Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Azure Disk Encryption stöds på hello efter Windows server-operativsystem - Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2. Diskkryptering stöds på hello följande Linux serveroperativsystem - Ubuntu och CentOS, SUSE SUSE Linux Enterprise Server (SLES).
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **gäller kryptering av**.
2. I hello **gäller diskkryptering** bladet visas en lista över virtuella datorer som diskkryptering rekommenderas.
3. Följ hello instruktioner tooapply kryptering toothese virtuella datorer.

![][1]

tooencrypt Azure virtuella datorer som identifierats av Security Center behov av kryptering, rekommenderar vi hello följande steg:

* Installera och konfigurera Azure PowerShell. Detta gör att du toorun hello PowerShell-kommandon krävs tooset in hello krav krävs tooencrypt Azure virtuella datorer.
* Hämta och köra hello Azure Disk Encryption krav Azure PowerShell-skript.
* Kryptera dina virtuella datorer.

[Kryptera en virtuell dator i Azure](security-center-disk-encryption.md) vägleder dig genom stegen.  Det här avsnittet förutsätter att du använder Windows 10 som hello klientdatorn där du konfigurera diskkryptering.

Det finns flera metoder som kan användas för virtuella datorer i Azure. Om du redan känner till väl Azure PowerShell eller Azure CLI, kanske du hellre toouse alternativa metoder. toolearn om dessa metoder finns [Azure disk encryption](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Se även
Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Använd disk encryption”. toolearn mer om diskkryptering, se hello följande:

* [Hantering av kryptering och nyckeln med Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video-36 min 39 sekund) – Lär dig hur toouse Diskhantering kryptering för virtuella IaaS-datorer och Azure Key Vault toohelp skydda och skydda dina data.
* [Kryptera en virtuell dator i Azure](security-center-disk-encryption.md) (dokument) – Lär dig hur tooencrypt Azure virtuella datorer.
* [Azure disk encryption](../security/azure-security-disk-encryption.md) (dokument) – Lär dig hur tooenable disk encryption för Windows och Linux virtuella datorer.

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
