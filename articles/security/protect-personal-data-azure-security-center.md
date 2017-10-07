---
title: aaaProtect personliga data med Azure Security Center | Microsoft Docs
description: "skydda personliga data med hjälp av Azure security center"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 8e2c893d13318392f47fa912089d52618f9e7b45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Skydda personliga data från överträdelser och attacker: Azure Security Center

Den här artikeln hjälper dig att förstå hur toouse Azure Security Center tooprotect personliga data från överträdelser och attacker.

## <a name="scenario"></a>Scenario 

Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavsområdet och baltiska havet samt hello brittiska staterna. toohelp i dessa åtgärder har det fått flera mindre kryssning rader i Italien, Tyskland, Danmark och hello Storbritannien

hello företag använder Microsoft Azure toostore företagsdata i hello molnet. Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation. Den inkluderar också personal information såsom:

- Adresser
- telefonnummer
- skatt ID-nummer
- medicinska information

hello kryssning rad har också en stor databas med ersättning och förmåner medlemmar. Företagets anställda åtkomst hello nätverket från hello företagets fjärranslutna kontor och resa agenter finns runt hello world har åtkomst till företagsresurser för toosome.
Personliga data överförs över hello nätverk mellan dessa platser och hello Microsoft-datacenter.

## <a name="problem-statement"></a>Problembeskrivning

hello företaget är orolig hello risken för angrepp på sina Azure-resurser. De vill tooprevent exponering av kunders och anställdas personliga data toounauthorized personer. De vill ha hjälp med både förebyggande och svar/reparation, samt ett effektivt sätt toomonitor hello pågående säkerheten för deras molnresurser.
Behöver de en stark försvarslinje mot dagens avancerade och organiserade angripare.

## <a name="company-goal"></a>Företagets mål

En av hello företagets mål är tooensure hello sekretess kundernas och anställdas personliga data genom att skydda mot hot. En av sina mål är toorespond omedelbart toosigns för intrång toomitigate hello påverkan. Det krävs ett sätt som tooassess hello aktuell status för säkerhet, identifiera sårbara konfigurationer och åtgärda dem.

## <a name="solutions"></a>Lösningar

Microsoft Azure Security Center (ASC) ger en integrerad säkerhet säkerhetsövervakning och principhantering hanteringslösning. Lätt att använda och effektivt hot ger funktioner för förebyggande, identifiering och svar.

### <a name="prevention"></a>Prevention (Skydd)

ASC hjälper dig att förhindra överträdelser genom att aktivera tooset säkerhetsprinciper, ger just-in-time-åtkomst och implementera säkerhetsrekommendationer.

En säkerhetsprincip definierar hello antal kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen. Åtkomst kan vara används toolock ned inkommande trafik tooyour virtuella Azure-datorer att minska exponeringen tooattacks precis i tid. Säkerhetsrekommendationer skapas ASC när du har analyserat hello säkerhetstillståndet hos dina Azure-resurser.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Hur ställer in säkerhetsprinciper i ASC?

Det går att ställa in särskilda säkerhetsprinciper för varje prenumeration. toomodify säkerhetspolicyn, bör du vara ägare eller deltagare i den aktuella prenumerationen. I hello Azure-portalen, hello följande:

1. Välj **princip** hello ASC instrumentpanelen.

2. Välj hello prenumeration där du vill tooenable hello principen.

3. Välj **skyddsprincip** tooconfigure principer per prenumeration. **Samla in data från virtuella datorer** ska anges för**på.**

4. I hello **skyddsprincip** alternativ, Välj **på** tooenable hello säkerhetsrekommendationer som är relevanta för hello prenumeration.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Mer detaljerade instruktioner och en förklaring av varje hello principrekommendationer som kan aktiveras finns [ställa in säkerhetsprinciper i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Hur konfigurerar jag precis i tid åtkomst (JIT)?

När JIT aktiveras låser Security Center ned inkommande trafik tooyour virtuella Azure-datorer genom att skapa en regel för NSG. Du väljer hello portar på hello VM toowhich inkommande trafik ska låsas. toouse JIT kan hello följande:

1. Välj hello **precis i tid VM åtkomst panelen** hello ASC bladet.

2. Välj hello **rekommenderas** fliken.

3. Under **VMs**, Välj hello virtuella datorer som du vill tooenable. Detta placerar en markering nästa tooa VM. 
4. Välj **aktivera JIT** på virtuella datorer.
5. Välj **Spara**.

Du kan se hello standardportarna som ASC rekommenderar aktiverats för JIT. Du kan också lägga till och konfigurera en ny port som du vill använda tooenable hello precis i Tidslösning. Hej **precis i tid VM access** panelen i hello Security Center visar hello antalet virtuella datorer som är konfigurerad för JIT-åtkomst. Här visas också hello antal godkända åtkomstbegäranden som görs i hello föregående vecka.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

Mer information om hur toodo, och ytterligare information om precis i Time-åtkomst finns i [hantera virtuella åtkomst med hjälp av precis i tid.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

#### <a name="how-do-i-implement-asc-security-recommendations"></a>Hur jag implementera ASC säkerhetsrekommendationer?

När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer. hello rekommendationer leder dig igenom hello konfigureringen av hello behövs kontroller. 
1. Välj hello **rekommendationer** rutan på hello ASC instrumentpanel.

2. Visa hello rekommendationer som visas i tabellformat där varje rad utgör en rekommendation.

3. Välj toofilter rekommendationer **Filter** och markera hello allvarlighetsgrad och tillstånd för värden som du vill toosee.

4. toodismiss en rekommendation som inte är tillämplig, kan du högerklicka och välja **Ignorera.**

5. Utvärdera vilka rekommendation tillämpas först.

6. Tillämpa hello rekommendationer i prioritetsordning.

En lista över möjliga rekommendationer och praktiska om hur tooapply, se [hantera säkerhetsrekommendationer i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)

### <a name="detection-and-response"></a>Identifiering och svar

Identifiering och svar går tillsammans som du vill toorespond så snabbt som möjligt när ett hot har identifierats.
Hotidentifiering ASC fungerar genom att automatiskt samla in säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar. ASC kan snabbt uppdateras som angripare släpper nya och allt mer avancerade kryphål. Mer detaljerad information om hur ASCS hotidentifiering fungerar finns [identifieringsfunktionerna i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-toosecurity-alerts"></a>Hur jag hanterar och åtgärdar toosecurity aviseringar?

En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med hello information du behöver tooquickly undersöka hello problem. Security Center innehåller också rekommendationer för hur tooremediate angreppet. toomanage säkerheten aviseringar, göra hello följande:

1. Välj hello **säkerhetsaviseringar** panelen i hello ASC instrumentpanelen. Detta visar information för varje avisering.

2. toofilter varningar efter datum, status och allvarlighetsgrad, Välj **Filter** och välj sedan hello-värden som du vill ha toosee.

3. toorespond tooan avisering markerar du den och granska hello information, och välj sedan hello angripna resursen.

4. I hello **beskrivning** fältet visas information, inklusive rekommenderade åtgärderna.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Mer detaljerad information om svarar toosecurity aviseringar finns [hantering och svarar toosecurity aviseringar i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

För ytterligare hjälp i undersöker säkerhetsaviseringar hello företag kan integrera ASC aviseringar med sin egen SIEM-lösning med hjälp av [Azure Log-integrering](https://aka.ms/AzLog).

#### <a name="how-do-i-manage-security-incidents"></a>Hur hanterar säkerhetsincidenter?

Är en sammanställning av alla aviseringar för en resurs som överensstämmer med kill kedjan mönster i ASC, en säkerhetsincident. En Incident avslöja hello lista över relaterade aviseringar, vilket gör att du tooobtain mer information om varje förekomst. Incidenter visas i hello säkerhetsaviseringar och bladet.

tooreview och hantera säkerhetsincidenter, hello följande:

1. Välj hello **säkerhetsaviseringar** panelen. Om en säkerhetsincident har upptäckts, visas den under hello aviseringar för organisationen. Har en ikon som skiljer sig från andra aviseringar.

2. Välj hello incident toosee mer information om den här säkerhetsincident. Ytterligare information innehåller en fullständig beskrivning, dess allvarlighetsgrad, dess aktuella tillstånd, hello angripna resursen, hello steg för hello incidenten och hello aviseringar som ingår i den här incidenten.

Du kan filtrera toosee **incidenter endast**, **aviseringar endast**, eller **båda**.

#### <a name="how-do-i-access-hello-threat-intelligence-report"></a>Hur kommer jag åt hello hot Intelligence-rapporten?

ASC analyserar information från flera källor tooidentify hot. tooassist incidenter team undersöka och åtgärda hot, Security Center innehåller en hot intelligence-rapport som innehåller information om hello hot som har identifierats.

Security Center har tre typer av hot rapporter som kan variera efter attack.
hello-rapporter som är tillgängliga är:

- Aktivitetsgrupp rapport: erbjuder djup dyker ned i angripare, mål och medvetande.

- Kampanj rapport: fokuserar på information om specifika attack kampanjer.

- Översiktsrapport för hot: omfattar alla objekt i hello föregående två rapporter.

Den här typen av information är mycket användbar när hello incidenter, där det finns en pågående undersökning toounderstand hello källa hello attacker, hello angriparens motiveringen och vilka toodo toomitigate i detta fall glidande framåt.

1. tooaccess hello hotinformation rapport, hello följande:

2. Välj hello **säkerhetsaviseringar** rutan på hello ASC instrumentpanel.

3. Välj hello Säkerhetsvarning som du vill tooobtain mer information.

4. I hello **rapporter** klickar hello länken toohello hot intelligence-rapporten.

5. Hello PDF-fil som du kan hämta öppnas.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

Mer information om hello ASC hot intelligence-rapporten finns [Azure Security Center hot Intelligence-rapporten.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)

### <a name="assessment"></a>Bedömning

toohelp med testning, bedömning och utvärdering av din säkerhetstillståndet ASC ger för inbyggt säkerhetsproblem med Qualys molnet agenter som en del av den virtuella datorn rekommendationer komponenten.

hello Qualys agenten rapporterar säkerhetsproblem toohello Qualys management dataplattform, som sedan skickar tillbaka tooASC säkerhetsproblem och hälsoövervakning data. Hej rekommendation tooadd en vulnerability assessment lösning visas i hello **rekommendationer** bladet på instrumentpanelen för hello ASC.

När hello säkerhetsproblem lösning har installerats på hello målet VM Security Center genomsökningar hello VM toodetect och identifiera system- och säkerhetsproblem. Identifierat problem visas under hello **rekommendationer för virtuella datorer** alternativet.

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Hur jag för att implementera en lösning för vulnerability assessment? 

Om en virtuell dator inte har ett inbyggt säkerhetsproblem assessment lösningen har redan distribuerats rekommenderar Security Center att installeras.

1. Hello ASC instrumentpanelen på hello **rekommendationer** bladet väljer **lägger till en vulnerability assessment lösning.**

2. Välj hello virtuella datorer där du vill att tooinstall hello säkerhetsproblem lösning.

3. Klicka på **installera på virtuella datorer [antal].**

4. Markera en partnerlösning i hello Azure Marketplace, eller under **använda befintlig lösning,** Välj **Qualys.**

5. Du kan aktivera hello uppdateringsinställningar för automatisk eller inaktivera i hello **partnerlösningar** bladet.

Ytterligare instruktioner för hur tooimplement en lösning för vulnerability assessment, se [utvärdering av säkerhetsrisker i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

## <a name="next-steps"></a>Nästa steg

- [Snabbstartsguide för Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [Introduktion tooAzure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Integrera Azure Security Center-aviseringar med Azure logganalys-integration](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Öka Azure Security Center med integrerad Vulnerability Assessment](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
