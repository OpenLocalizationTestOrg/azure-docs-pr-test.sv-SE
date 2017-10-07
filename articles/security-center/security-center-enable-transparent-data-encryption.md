---
title: aaaEnable Transparent datakryptering i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera Transparent Data kryptering **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 94c6e9a1feddaa48faac6c835d416c4d131cd5c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Aktivera Transparent datakryptering i Azure Security Center
Azure Security Center kommer rekommenderar att du aktiverar Transparent Data kryptering (TDE) på SQL-databaser om TDE inte redan är aktiverad. TDE skyddar dina data och hjälper dig att uppfylla efterlevnadskrav genom att kryptera din databas, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan ändringar tooyour program. Det finns fler toolearn [Transparent datakryptering med Azure SQL Database](https://msdn.microsoft.com/library/dn948096).

Den här rekommendationen gäller toohello Azure SQL-tjänsten. innehåller SQL som körs på virtuella datorer.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **aktivera Transparent datakryptering**.
   ![Aktivera transparent datakryptering][1]
2. Då öppnas hello **aktivera Transparent datakryptering på SQL-databaser** bladet. Välj en SQL-databas tooenable TDE på.
   ![Välj SQL DB tooenable TDE på][2]
3. På hello **Transparent datakryptering** bladet väljer **ON** under datakryptering och välj **spara** hello översta menyfliken av hello-bladet.
   ![Aktivera TDE][3]

   När TDE har aktiverats på hello valda SQL-databas hello **krypteringsstatus** ändras för**krypterade**.    

   ![Krypteringsstatus][4]

## <a name="see-also"></a>Se även
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”aktivera Transparent kryptering av Data”. toolearn mer information om SQL TDE finns hello följande:

* [Transparent datakryptering med Azure SQL Database](https://msdn.microsoft.com/library/dn948096)
* [Kom igång med Transparent Data kryptering (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
