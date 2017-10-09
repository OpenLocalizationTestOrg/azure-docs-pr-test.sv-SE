---
title: "aaaProvide säkerhet kontaktinformation i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooprovide säkerhet kontaktinformation i Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 6c378c9c84c6e4fce70b2541e4cc121018700269
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Ange säkerhet kontaktinformation i Azure Security Center
Azure Security Center kommer rekommenderar att du förser säkerhet kontaktinformation för din Azure-prenumeration om du inte redan har gjort. Den här informationen kommer att användas av Microsoft toocontact om hello Microsoft Security Response Center (MSRC) upptäcker att din kundinformation används redan av en olaglig eller obehörig part. MSRC utför väljer säkerhetsövervakning hello Azure-nätverk och infrastruktur och tar emot hot intelligence och missbruk klagomål från tredje part.

Ett e-postmeddelande skickas på hello första dagliga förekomsten av en avisering och endast för varningar med hög angelägenhetsgrad. E-postinställningar kan bara konfigureras för prenumerationsprinciper. Resursgrupper inom en prenumeration ärver inställningarna.

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.  Det är alltså inte en steg-för-steg-guide.
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation
1. I hello **rekommendationer** bladet väljer **ange säkerhet kontaktinformation**.
   ![Ger säkerhet kontakt][1]
2. Då öppnas bladet hello **ange säkerhet kontaktinformation**. Välj Azure-prenumeration hello tooprovide kontaktinformation.
   ![Ange säkerhetskontaktinformation][2]
3. En andra **ange säkerhet kontaktinformation** blad öppnas.

   * Ange hello säkerhet kontakta e-postadress eller adresser som avgränsas med kommatecken. Det finns inte en begränsa toohello antalet e-postadresser som du kan ange.
   * Ange en säkerhet kontakta internationella telefonnummer.
   * tooreceive e-post om varningar med hög angelägenhetsgrad, aktivera hello **Skicka mig e-postmeddelanden om aviseringar**.
   * I framtida hello har du hello alternativet toosend e-postaviseringar toosubscription ägare. Det här alternativet är för närvarande nedtonade.
   * Välj **OK** tooapply hello säkerhet kontakta information tooyour prenumeration.

## <a name="see-also"></a>Se även
toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hello Azure security nyheter och information.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
