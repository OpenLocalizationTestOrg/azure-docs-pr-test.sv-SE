---
ms.assetid: 
title: "aaaAzure Key Vault säkerhetsvärldar | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 1995119c9e9886829d6c50af921f275d10e382f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault-säkerhetsvärldar och geografisk gränser

Azure Key Vault är en tjänst med flera klienter och använder en pool med Maskinvarusäkerhetsmoduler (HSM) i varje Azure-plats. 

Alla HSM: er på Azure platser i hello hello samma geografiska region delar samma kryptografiska gräns (Thales Säkerhetsvärld). Till exempel hello östra USA och västra USA delar samma säkerhetsvärld eftersom de tillhör toohello USA geoplats. På liknande sätt kan alla platser i Japan resurs som Azure hello samma säkerhetsvärld och alla Azure platser i Australien Indien, och så vidare. 

## <a name="backup-and-restore-behavior"></a>Beteende för säkerhetskopiering och återställning

En säkerhetskopia som gjorts av en nyckel från nyckelvalvet i ett Azure-plats kan vara återställts tooa nyckelvalvet i en annan Azure-plats, förutsatt att båda dessa villkor är uppfyllda:

- Både hello Azure platser tillhör toohello samma geografiska plats
- Både hello nyckelvalv tillhör toohello samma Azure-prenumeration

Till exempel en säkerhetskopia som gjorts av en viss prenumeration på en nyckel i nyckelvalvet i västra Indien endast kan återställda tooanother nyckelvalvet i hello samma prenumeration och geoplats; Västra Indien, centrala Indien eller södra Indien.

## <a name="regions-and-products"></a>Regioner och produkter

- [Azure-regioner](https://azure.microsoft.com/regions/)
- [Microsoft-produkter efter region](https://azure.microsoft.com/regions/services/)

Regioner är mappade toosecurity världar visas som större rubriker i hello tabeller:

I hello produkter efter region artikel, till exempel hello **Americas** fliken innehåller ÖSTRA USA, centrala USA, västra USA alla mappning toohello Americas område. 

>[!NOTE]
>Ett undantag är att oss DOD ÖST och oss DOD centrala har sina egna säkerhetsvärldar. 

På liknande sätt på hello **Europa** fliken NORRA Europa och VÄSTEUROPA mappar båda toohello Europa region. hello gäller också på hello **Stillahavsområdet** fliken.



