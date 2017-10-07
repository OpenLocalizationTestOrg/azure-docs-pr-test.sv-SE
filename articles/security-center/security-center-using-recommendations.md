---
title: "aaaUse Azure Security Center rekommendationer tooenhance säkerhet | Microsoft Docs"
description: " Lär dig hur toouse säkerhetsprinciper och säkerhetsrekommendationer i Azure Security Center toohelp minimera en säkerhetsattack med. "
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
ms.date: 05/08/2017
ms.author: terrylan
ms.openlocfilehash: bd314350a5abfceea3e171f2e1b55afe4549c1b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-security-center-recommendations-tooenhance-security"></a>Använda Azure Security Center rekommendationer tooenhance säkerhet
Du kan minska hello risken för en betydande säkerhetshändelse genom att konfigurera en säkerhetsprincip och implementera hello rekommendationerna i Azure Security Center. Den här artikeln visar hur toouse säkerhetsprinciper och säkerhetsrekommendationer i Security Center toohelp minimera en säkerhetsattack med.

> [!NOTE]
> Den här artikeln bygger på hello roller och begrepp som introduceras i hello Security Center [planerings-och operations](security-center-planning-and-operations-guide.md). Det är en bra idé tooreview hello guide för innan du fortsätter.
>
>

## <a name="managing-security-recommendations"></a>Hantera säkerhetsrekommendationer
En säkerhetsprincip definierar hello kontroller som rekommenderas för resurser inom hello angivna prenumerationen eller resursen grupp. I Security Center kan du definiera principer enligt tooyour företagets säkerhetskrav. Det finns fler toolearn [ställa in säkerhetsprinciper i Security Center](security-center-policies.md).

Säkerhetsprinciper för resursgrupper ärvs från prenumerationsnivån hello.

![Arv av säkerhet][1]

Om du behöver anpassade principer i särskilda resursgrupper kan inaktivera du arv i hello resursgruppen. toodisable, Ställ in arv tooUnique för hello bladet med säkerhetsprinciper och anpassa hello-kontroller som Security Center visar rekommendationer för.

Om du har arbetsbelastningar som inte kräver hello databasen Transparent Data kryptering (Datakrypteringsprincip) princip inaktivera hello princip hello prenumerationsnivå och aktiverar den bara i hello resursgrupper där SQL TDE krävs.

> [!NOTE]
> Om det finns en konflikt mellan prenumerationsnivå och resursgruppsnivå är det principen, företräde hello resursgruppsnivå är det principen.
>
>

Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser. När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer baserat på hello kontroller i hello säkerhetsprincip. hello rekommendationer leder dig igenom hello konfigureringen av hello behövs säkerhetsåtgärder.

Aktuella princip rekommendationerna i Security Center fokus på systemuppdateringar, Operativsystemets konfiguration nätverkssäkerhetsgrupper på undernät och virtuella datorer (VM), SQL Database Auditing TDE för SQL-databasen, och webbprogrammet brandväggar. Hello senaste täckning av Security Center rekommendationer finns [hantera säkerhetsrekommendationer i Security Center](security-center-recommendations.md).

## <a name="scenario"></a>Scenario
Det här scenariot visar hur toouse Security Center toohelp minska hello risken för en betydande säkerhetsincident genom att övervaka Security Center-rekommendationerna och vidtar åtgärder. hello scenariot använder hello fiktiva företag, Contoso och roller som presenteras i hello Security Center [planerings-och operations](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). hello roller representerar personer och grupper som kan använda Security Center tooperform olika säkerhetsrelaterade uppgifter. hello-rollerna är:

![Scenario-roller][2]

Contoso nyligen har migreras några av sina lokala resurser tooAzure. Contoso vill tooimplement och upprätthålla skydd som minskar attacken säkerhetsproblem tooa säkerhet för sina resurser i hello molnet.

## <a name="recommended-solution"></a>Rekommenderad lösning
En lösning är toouse Security Center tooprevent och identifiera säkerhetsproblem. Contoso har åtkomst tooSecurity Center via sina Azure-prenumeration. Hej [kostnadsfria nivån](security-center-pricing.md) av Security Center aktiveras automatiskt på alla Azure-prenumerationer och insamling av data är aktiverat på alla virtuella datorer i prenumerationen.

David i Contosos IT-säkerhet, konfigurerar en **säkerhetsprincip** med hjälp av Security Center. Security Center analyserar hello säkerhetstillstånd Contosos Azure-resurser. När Security Center identifierar potentiella säkerhetsrisker, skapas **rekommendationer** baserat på hello kontroller i hello säkerhetsprincip.

Jeff molnansvarig, ansvarar för att implementera och underhålla skydd i enlighet med Contosos säkerhetsprinciper. Jeff kan övervaka hello rekommendationer som skapats av Security Center tooapply skydd. hello rekommendationer om hur Jeff hello konfigureringen av hello behövs säkerhetsåtgärder.

I ordning för Jeff tooimplement och upprätthålla skydd och eliminera säkerhetsproblem, he måste:

- Övervaka säkerhetsrekommendationer som tillhandahålls av Security Center
- Utvärdera säkerhetsrekommendationer och bestämma om han ska använda eller ignorera
- Tillämpa säkerhetsrekommendationer

Vi följer Jeffs steg toosee hur han använda Security Center rekommendationer tooguide honom via hello konfigureringen av kontroller tooeliminate säkerhetsproblem.

## <a name="how-tooimplement-this-solution"></a>Hur tooimplement den här lösningen
Jeff loggar in för[Azure-portalen](https://azure.microsoft.com/features/azure-portal/) och öppnas hello Security Center-konsolen. Som en del av dagliga övervakningsaktiviteter, kontrollerar han toosee om säkerhetsrekommendationer genom att utföra följande steg hello:

1. Jeff väljer hello **rekommendationer** panelen tooopen hello **rekommendationer** bladet.
   ![Välj hello rekommendationer sida vid sida][3]
2. Jeff granskar hello lista över rekommendationer. Han ser att Security Center tillhandahåller hello lista över rekommendationerna i prioritetsordning, från högsta prioritet toolowest prioritet. Han beslutar tooaddress en hög prioritet rekommendation hello-listan. Han väljer **installera Endpoint Protection** på hello **rekommendationer** bladet.
3. Hej **installera Endpoint Protection** öppnas ett blad med en lista över virtuella datorer utan program mot skadlig kod aktiverat. Jeff granskar hello lista över virtuella datorer, markerar alla virtuella datorer och väljer sedan **installera på 3 virtuella datorer**.
   ![Installera slutpunktsskydd][4]
4. Hej **Välj Endpoint Protection** blad öppnas med Jeff med två program mot skadlig kod. Jeff väljer hello **Microsoft Antimalware** lösning.
5. Mer information om hello program mot skadlig kod visas. Jeff väljer **skapa**.
   ![Microsoft program mot skadlig kod][5]
6. Jeff anger konfigurationsinställningar för hello krävs på hello **installera** bladet och väljer **OK**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) nu aktiva vid hello valda virtuella datorer.

Jeff fortsätter toomove via hello hög prioritet och Medelhög prioritet rekommendationer att fatta beslut om genomförandet. Jeff refererar till hello [hantera säkerhetsrekommendationer](security-center-recommendations.md) artikel toounderstand hello rekommendationer och det var och en gör om han gäller den.

Jeff lär sig som [Microsoft Security Response Center (MSRC)](../security/azure-security-response-center.md) utför väljer säkerhetsövervakning hello Azure-nätverk och infrastruktur och tar emot hot intelligence och missbruk klagomål från tredje part. Om Jeff erbjuder säkerhet kontaktinformation för Contosos Azure-prenumeration, Microsoft kontakter Contoso om hello MSRC upptäcker att Contoso kundinformation används redan av en olaglig eller obehörig part. Vi följer Jeff han gäller hello **ange säkerhet kontaktinformation** rekommendation (en rekommendation allvarlighetsgraden medium i hello lista över rekommendationerna ovan).

1. Jeff väljer **ange säkerhet kontaktinformation** på hello **rekommendationer** bladet som öppnas hello **ange säkerhet kontaktinformation** bladet.
2. Jeff väljer hello Azure-prenumeration tooprovide kontaktinformation på. En andra **ange säkerhet kontaktinformation** blad öppnas.
   ![Kontakta säkerhetsinformation][6]
3. På hello andra **ange säkerhet kontaktinformation** bladet Jeff anger:

  - hello säkerhet kontakta e-postadresser avgränsade med kommatecken (det inte är en begränsa toohello antalet e-postadresser som han kan ange)
  - en säkerhet kontakta telefonnummer

4. Jeff aktiverar också hello alternativet **Skicka mig e-postmeddelanden om aviseringar** tooreceive e-post om varningar med hög angelägenhetsgrad.
5. Jeff väljer **OK** tooapply hello säkerhet kontakta information tooContoso prenumeration.

Slutligen Jeff granskar hello med låg prioritet rekommendation **reparera OS säkerhetsrisker** och anger den här rekommendationen inte är tillämplig. Han vill toodismiss hello rekommendation. Jeff väljer hello tre punkter som visas toohello höger och sedan väljer **Ignorera**.
   ![Ignorera rekommendation][7]

## <a name="conclusion"></a>Slutsats
Övervakning av rekommendationerna i Security Center kan hjälpa dig att undvika säkerhetsproblem innan en attack inträffar. Du kan förhindra att en säkerhetsincident genom att implementera och underhålla skydd med säkerhetsprinciper i Security Center.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png
