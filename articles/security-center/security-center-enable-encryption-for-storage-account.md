---
title: "aaaEnable kryptering för storage-konto i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** Aktivera kryptering för Azure Storage konto **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: c5cbafbf3a8be86f213dcf1c0c0ddcc0222b3d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Aktivera kryptering för Azure storage-konto i Azure Security Center
Azure Security Center kan rekommenderar att du aktiverar Azure Storage Service-kryptering för data i vila.

Kryptering för lagring-tjänsten (SSE) fungerar genom att kryptera hello data när de skrivs tooAzure lagring och dekryptera hello data innan hämtning.  SSE är för närvarande endast tillgängligt för hello Azure Blob-tjänsten och kan användas för blockblobbar, sidblobbar, och tilläggsblobar.  Det finns fler toolearn [Lagringstjänstens kryptering av vilande data](../storage/common/storage-service-encryption.md).


> [!Note]
> När du har aktiverat kryptering krypteras endast nya data. Alla befintliga blobbar i ditt lagringskonto vara okrypterad. tooencrypt befintliga blobbar, se hello [vanliga frågor och svar Storage Service-kryptering](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Lagringstjänstens kryptering stöds endast i hanteraren för filserverresurser storage-konton. Klassiska lagringskonton stöds inte för närvarande. toounderstand hello klassiska och Resource Manager distributionsmodellerna finns [Azure distributionsmodeller](../azure-classic-rm.md).

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det här dokumentet är inte en stegvis guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **Aktivera kryptering för Azure Storage-konto**.
   ![Aktivera kryptering för lagringskonto][1]
2. Hej **aktivera lagringskryptering** blad öppnas. Det här bladet visar hello Azure storage-konton där storage kryptering har inaktiverats. I det här exemplet ska vi väljer **storageacct1**.
   ![Aktivera lagringskryptering][2]
3. Hej **kryptering** bladet för **storageacct1** öppnas. Välj **aktiverat**.
   ![Kryptering bladet][3]
4. Välj **Spara**.

Du har nu aktiverats lagringskryptering för **storageacct1**.


## <a name="see-also"></a>Se även
Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Aktivera kryptering för Azure Storage-konto”. toolearn mer om Azure Storage Service-kryptering, finns följande hello:

* [Azure Storage Service-kryptering av vilande Data](../storage/common/storage-service-encryption.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) -finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) -hittar du blogginlägg om säkerhet och Azure kompatibilitet.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
