---
title: "aaaProtecting Azure SQL-tjänsten och data i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet behandlar rekommendationerna i Azure Security Center som hjälper dig skydda dina data och Azure SQL-tjänsten och vara kompatibla med säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 75d782d3c2418f9645139e4cd6ecb7765c488f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Skydda SQL Azure-tjänsten och data i Azure Security Center
Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som guidar dig igenom hello konfigureringen av hello behövs kontroller.  Rekommendationer gäller tooAzure resurstyper: virtuella datorer (VM), nätverk, SQL och data och program.

Den här artikeln tar rekommendationer som gäller tooAzure SQL-tjänsten och data. Rekommendationer center runt aktivera granskning för Azure SQL-servrar och databaser, aktivera kryptering för SQL-databaser och aktivera kryptering av Azure storage-konto.  Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga SQL-tjänsten och data rekommendationer och det var och en gör om du använder den.

## <a name="available-sql-service-and-data-recommendations"></a>Tillgängliga rekommendationer för SQL-tjänsten och data
| Rekommendation | Beskrivning |
| --- | --- |
| [Aktivera granskning och hotidentifiering på SQL-servrar](security-center-enable-auditing-on-sql-servers.md) |Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-servrar (Azure SQL-tjänsten; inte omfattar SQL som körs på virtuella datorer). |
| [Aktivera granskning och hotidentifiering på SQL-databaser](security-center-enable-auditing-on-sql-databases.md) |Rekommenderar att du aktiverar granskning och hotidentifiering identifiering för Azure SQL-databaser (Azure SQL-tjänsten; inte omfattar SQL som körs på virtuella datorer). |
| [Aktivera Transparent datakryptering på SQL-databaser](security-center-enable-transparent-data-encryption.md) |Rekommenderar att du aktiverar kryptering för SQL-databaser (endast Azure SQL-tjänsten). |

## <a name="see-also"></a>Se även
toolearn mer om rekommendationer som gäller tooother Azure resurstyper finns hello följande:

* [Skydda dina virtuella datorer i Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Skydda dina program i Azure Security Center](security-center-application-recommendations.md)
* [Skydda ditt nätverk i Azure Security Center](security-center-network-recommendations.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
