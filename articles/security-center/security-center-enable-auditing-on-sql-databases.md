---
title: "aaaEnable granskning och hotidentifiering identifiering på SQL-databaser i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** aktivera granskning och hotidentifiering identifiering på SQL-databaser **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: c94140acf37cabaca3e681ba5db79d6827e7b9db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>Aktivera granskning och hotidentifiering identifiering på SQL-databaser i Azure Security Center
Azure Security Center rekommenderar att du aktiverar granskning och hotidentifiering identifiering för alla SQL-databaser om granskning och hotidentifiering inte redan är aktiverad. Granskning och hotidentifiering identifiering kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.

När du har aktiverat granskning kan du konfigurera Hotidentifiering inställningar och e-postmeddelanden tooreceive säkerhetsaviseringar. Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen. Detta kan du toodetect och svara toopotential hot när de inträffar.

Den här rekommendationen gäller toohello Azure SQL-tjänsten. Det finns inget SQL som körs på virtuella datorer.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **aktivera Auditing & Threat detection på SQL-databaser**.  Då öppnas hello **aktivera Auditing & Threat detection på SQL-databaser** bladet.

   ![Aktivera granskning på SQL-databaser][1]
2. Välj en SQL database tooenable granskning på. Då öppnas hello **granskning och Hotidentifiering** bladet.

3. På hello **granskning och Hotidentifiering** bladet väljer **ON** under **granskning**.

   ![Aktivera granskning och hotidentifiering][2]
4. Gör så hello i [Hotidentifiering för SQL-databasen i hello Azure-portalen](../sql-database/sql-database-threat-detection-portal.md) tooturn på och konfigurera Hotidentifiering och tooconfigure hello lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande aktiviteter.

## <a name="see-also"></a>Se även
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”Enable Auditing & Threat detection på SQL-databaser”. toolearn mer om att skydda din SQL-databas finns hello följande:

* [Säkra din SQL Database](../sql-database/sql-database-security-overview.md)

toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
