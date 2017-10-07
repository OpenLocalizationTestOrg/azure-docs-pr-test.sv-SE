---
title: aaaSecurity aviseringar efter typ i Azure Security Center | Microsoft Docs
description: "Den här artikeln beskrivs hello olika typer av tillgängliga säkerhetsaviseringar i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.openlocfilehash: ee69cb9035c35f5bc2ed51f9b9d6f29486b4caf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Förstå säkerhetsaviseringar i Azure Security Center
Den här artikeln hjälper dig att toounderstand hello olika typer av säkerhetsaviseringar och relaterade insikter som är tillgängliga i Azure Security Center. Mer information om hur toomanage varningar och incidenter, se [hantering och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> tooset upp avancerade identifieringar uppgradera tooAzure Security Center Standard. En kostnadsfri 60-dagars utvärderingsversion är tillgänglig. tooupgrade, Välj **prisnivån** i hello [säkerhetsprincip](security-center-policies.md). toolearn finns fler hello [sida med priser](https://azure.microsoft.com/pricing/details/security-center/).
>

## <a name="what-type-of-alerts-are-available"></a>Vilka typer av aviseringar finns?
Azure Security Center använder olika [identifieringsfunktionerna](security-center-detection-capabilities.md) tooalert kunder toopotential attacker målobjekt för sina miljöer. Dessa aviseringar innehåller värdefull information om hello vad utlöses hello aviseringen hello resurser som mål, och hello källan för hello-attack. hello information som ingår i en avisering varierar beroende på hello typ av analytics används toodetect hello hot. Händelser kan också innehålla ytterligare sammanhangsinformation som kan vara användbart när du utreder ett hot.  Den här artikeln innehåller information om hello efter aviseringstyper:

* Virtual Machine Behavioral Analysis (VMBA)
* Nätverksanalys
* Resursanalys
* Sammanhangsbaserad information

## <a name="virtual-machine-behavioral-analysis"></a>Beteendeanalys av virtuell dator
Azure Security Center kan använda beteendeanalys tooidentify komprometteras resurser baserat på analys av händelseloggar för virtuell dator. Till exempel processgenereringshändelser och inloggningshändelser. Det finns dessutom korrelation med andra signaler toocheck för underlag för en omfattande kampanj.

> [!NOTE]
> Mer information om hur identifieringsfunktionerna i Security Center fungerar finns i [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md).
>

### <a name="crash-analysis"></a>Kraschanalys
Analys av krascher dump minne är en metod som används för toodetect avancerad skadlig kod som kan tooevade traditionella säkerhetslösningar. Olika typer av skadlig kod försök tooreduce hello risken för identifieras av antivirusprodukter genom att skriva aldrig toodisk eller genom att kryptera programvarukomponenter som skrivs toodisk. Detta gör hello skadlig kod svårt toodetect med hjälp av traditionella program mot skadlig kod metoder. Den här typen av skadlig kod kan identifieras med hjälp av minnesanalys, eftersom skadlig kod måste lämna spårningar i minnet i ordning toofunction.

När programmet kraschar avbildar en krasch-dump en del av minnet när hello hello krascher. hello krascher kan orsakas av skadlig kod, Allmänt program eller system problem. Genom att analysera hello minne i hello kraschdump Security Center identifierar tekniker används tooexploit sårbarheter i program, komma åt känsliga data och smyg finns kvar i en komprometterad dator. Detta åstadkoms med minsta prestanda påverkan toohosts som hello analys utförs av hello Security Center tillbaka avslutas.

hello följande fält är vanliga toohello krasch dump avisering exempel som visas nedan:

* DUMPFILE: Namnet på hello kraschdumpfil.
* PROCESS: Namn på hello kraschar processen.
* PROCESSVERSION: Version av hello kraschar processen.

### <a name="shellcode-discovered"></a>Identifierad Shellcode
Shellcode är hello-nyttolast som körs när skadlig kod på säkerhetsproblemet programvara. Den här aviseringen anger att en analys av kraschdumpfiler har identifierat körbar kod som uppvisar beteenden som vanligen utförs av skadliga nyttolaster. Icke-skadlig programvara kan uppvisa detta beteende, men det är inte typiskt i normala programutvecklingsrutiner.

Hej Shellcode avisering exempel innehåller hello följande ytterligare fält:

* ADRESS: hello plats i minnet av hello shellcode.

Det här är ett exempel på den här typen av varning:

![Shellcode-varning](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Identifierad modulkapning
Windows använder dynamiska länkbibliotek (dll) tooallow programvara tooutilize vanliga Windows systemets funktion. DLL kapa inträffar när skadlig kod ändras hello DLL belastningen ordning tooload skadliga nyttolaster i minnet, där skadlig kod kan köras. Den här varningen anger att hello krascher dump analys upptäckte en liknande namn modul som läses in från två olika sökvägar. En hello läsa in sökväg för kommer från en gemensam Windows system binära plats.

Legitima programutvecklare ändra ibland hello DLL inläsningsordningen för icke-skadliga orsaker, till exempel instrumentering, utöka hello Windows-Operativsystemet eller utöka ett Windows-program. toohelp skilja mellan skadlig och potentiellt skadliga ändringar toohello DLL inläsningsordning, Azure Security Center kontrollerar om en modul som lästes in överensstämmer tooa misstänkta profil. hello resultatet av den här kontrollen anges efter hello ”SIGNATUR” fält för hello aviseringen och avspeglas i hello allvarlighetsgraden hello aviseringen och aviseringsbeskrivningen avisering steg. tooresearch om hello-modulen är ogiltigt eller skadliga, analysera hello på diskkopia av hello kapa modulen. Du kan exempelvis kontrollera hello filens digitala signatur eller köra antivirusprogram.

Dessutom toohello vanliga fälten beskrivs i hello tidigare ”Shellcode identifierade” den här aviseringen innehåller hello följande fält:

* SIGNATUR: Anger om hello kapa modulen överensstämmer tooa misstänkt beteende profil.
* HIJACKEDMODULE: hello namnet på hello uppsnappas modulen för Windows-system.
* HIJACKEDMODULEPATH: hello sökvägen hello uppsnappas modulen för Windows-system.
* HIJACKINGMODULEPATH: hello sökväg hello kapning modulen.

Det här är ett exempel på den här typen av varning:

![Varning vid modulkapning](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Identifierad maskering av Windows-modul
Skadlig kod kan använda nätverksnamnen för Windows-systemfiler (till exempel SVCHOST. EXE) eller moduler (till exempel NTDLL. DLL-fil) för*blandas* och dölja hello uppbyggnad hello skadlig programvara från administratörer. Den här varningen anger att hello krascher dump analys identifieras som hello kraschdumpfil innehåller moduler som använder Windows system Modulnamnen, men inte uppfyller andra villkor som är typiska för Windows-moduler. När hello på diskkopia av hello imiterade modul kan ger mer information om hello legitima eller skadliga arten av den här modulen. Analysen kan inbegripa följande åtgärder:

* Bekräfta hello filen i fråga levereras som en del av en legitim programpaket.
* Kontrollera hello filens digitala signatur.
* Köra antivirusprogram på hello.

Dessutom toohello vanliga fält beskrivs tidigare i ”Shellcode identifierade” hello, aviseringen innehåller hello följande fält:

* INFORMATION: Beskriver om hello modulen metadata är giltiga och om hello modulen lästes in från en sökväg i filsystemet.
* NAME: hello namnet på hello imiterade Windows-modulen.
* SÖKVÄG: hello sökvägen toohello imiterade Windows-modulen.

Den här aviseringen även extraherar och visar vissa fält från hello modulen PE-huvudet, till exempel ”KONTROLLSUMMA” och ”TIDSSTÄMPEL”. Dessa fält visas endast om hello fält finns i hello-modulen. Se hello [Microsoft PE och COFF-specifikationen](https://msdn.microsoft.com/windows/hardware/gg463119.aspx) mer information om dessa fält.

Det här är ett exempel på den här typen av varning:

![Avisering om Windows-maskering](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Identifierad ändring av binär systemfil
Skadlig kod kan ändra core systemfiler i ordning toocovertly åtkomst till data eller smyg kvar på en komprometterad system. Den här varningen anger att hello krascher dump analysis har upptäckt att core Windows OS-binärfiler har ändrats i minnet eller på disk.

Legitima programutvecklare kan ibland ändra systemmoduler i minnet för icke-skadliga ändamål, till exempel Detours eller för programkompatibilitet. toohelp skilja mellan skadlig och potentiellt legitima moduler, Azure Security Center kontrollerar om hello ändrade modulen överensstämmer tooa misstänkta profil. hello resultatet av den här kontrollen visas med hello allvarlighetsgraden hello aviseringen och aviseringsbeskrivningen avisering steg.

Dessutom toohello vanliga fält beskrivs tidigare i ”Shellcode identifierade” hello, aviseringen innehåller hello följande fält:

* MODULNAMN: Namn på hello ändrade för systemet.
* MODULEVERSION: Version av hello ändrade för systemet.

Det här är ett exempel på den här typen av varning:

![Varning om binär systemfil](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Misstänkt process kördes
Security Center identifierar en misstänkt process som körs på hello virtuella måldatorn och utlöser en varning. hello identifiering ser inte ut för hello specifikt namn, men ser ut för parametern hello körbar fil. Därför även om hello angripare byter hello körbara, kan Security Center fortfarande identifiera misstänkta hello-processen.

Det här är ett exempel på den här typen av varning:

![Varning om misstänkt process](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domain-accounts-queried"></a>Flera domänkonton efterfrågas
Security Center kan identifiera flera försöker tooquery Active Directory-domänkonton, vilket vanligtvis utförs av angripare under rekognosering i nätverket. Angripare kan utnyttja den här tekniken tooquery hello tooidentify hello domänanvändare, identifiera hello domänkonton admin, identifiera hello-datorer som är domänkontrollanter, och även identifiera hello potentiella domän förtroenderelation med andra domäner.

Det här är ett exempel på den här typen av varning:

![Varning om flera domänkonton](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>Medlemmarna i gruppen lokala administratörer har räknats upp

Security Center kommer tootrigger en avisering när hello säkerhetshändelse 4798, i Windows Server 2016 och Windows 10, rensa. Detta händer när grupper med lokala administratörer räknas upp, något som vanligtvis utförs av angripare under spaning i nätverket. Angripare kan utnyttja den här tekniken tooquery hello identiteten för användare med administratörsbehörighet.

Det här är ett exempel på den här typen av varning:

![Lokal administratör](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Avvikande blandning av versaler och gemener

Security Center utlöser en avisering när den identifierar hello användning av en blandning av versaler och gemener hello-kommandoraden. En angripare kan använda den här tekniken toohide från skiftlägeskänsliga eller hash-baserad dator regeln.

Det här är ett exempel på den här typen av varning:

![Avvikande blandning](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Misstänkt attack av med gyllene Kerberos-biljett

En komprometterad [krbtgt](https://technet.microsoft.com/library/dn745899.aspx) nyckeln kan användas av en angripare toocreate Kerberos ”gyllene biljetter” så att hello angripare tooimpersonate alla användare som de önskar. Security Center kommer tootrigger en avisering när den identifierar den här typen av aktivitet.

> [!NOTE] 
> Mer information om gyllene Kerberos-biljetter finns i [Åtgärder mot stöld av autentiseringsuppgifter i Windows 10](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx).

Det här är ett exempel på den här typen av varning:

![Gyllene biljett](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Ett misstänkt konto har skapats

Security Center utlöser en avisering när ett konto skapas som liknar ett befintligt konto med integrerade administratörsprivilegier. Den här tekniken kan användas av angripare toocreate en falsk kontot tooavoid som upptäckt av mänsklig verifiering.
 
Det här är ett exempel på den här typen av varning:

![Misstänkt konto](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Misstänkt brandväggsregel skapad

Angripare kan försöka toocircumvent värden säkerhet genom att skapa anpassade brandväggen regler tooallow skadliga program toocommunicate med kommando- och eller toolaunch attacker via hello nätverket via hello komprometteras värden. Security Center utlöser en avisering när den upptäcker att en ny brandväggsregel har skapats från en körbar fil på en misstänkt plats.
 
Det här är ett exempel på den här typen av varning:

![Brandväggsregel](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>Misstänkt kombination av HTA och PowerShell

Security Center utlöser en avisering när den identifierar att en Microsoft HTML-programvärd (HTA) startar PowerShell-kommandon. Det här är en teknik som används av angripare toolaunch skadliga PowerShell-skript.
 
Det här är ett exempel på den här typen av varning:

![HTA och PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>Nätverksanalys
Hotidentifieringen i nätverk av Security Center sker genom automatisk insamling av säkerhetsinformation från din Azure IPFIX-trafik (Internet Protocol Flow Information Export). Den här informationen kan ofta korrelerar information från flera källor, tooidentify hot analyseras.

### <a name="suspicious-outgoing-traffic-detected"></a>Identifierad misstänkt utgående trafik
Nätverksenheter kan identifieras och profileras startas i mycket hello samma sätt som andra typer av system. Angripare börjar vanligtvis med att skanna eller söka av portar. I nästa exempel hello har misstänkt SSH (Secure Shell) trafik från en virtuell dator. I det här scenariot är en SSH-råstyrkeattack eller en avsökningsattack möjlig.

![Varning om misstänkt utgående trafik](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Den här aviseringen ger den information som du kan använda tooidentify hello resurs som har använt tooinitiate angreppet. Den här aviseringen innehåller också information tooidentify hello komprometteras datorn hello identifieringstiden plus hello protokoll och port som användes. Det här bladet ger dig även en lista över steg som kan vara används toomitigate problemet.

### <a name="network-communication-with-a-malicious-machine"></a>Nätverkskommunikation med en obehörig dator
Med hjälp av Microsofts insamling av hotinformation kan Azure Security Center identifiera komprometterade datorer som kommunicerar med skadliga IP-adresser. I många fall är hello skadliga adressen en kommando- och center. I det här fallet Security Center upptäckte att hello kommunikation har skett genom att använda ponny inläsaren skadlig kod (även kallat [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![varning om nätverkskommunikation](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Den här aviseringen ger information som gör att du tooidentify hello resurs som har använt tooinitiate angrepp, hello angripna resursen, hello drabbade IP, hello angripare IP och hello identifieringstiden.

> [!NOTE]
> Live-IP-adresserna har tagits bort i den här skärmbilden i sekretessyfte.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Möjlig utgående Denial of Service-attack upptäcktes
Onormalt nätverkstrafik som kommer från en virtuell dator kan Security Center tootrigger orsaka en potentiell denial of service-typ av angrepp.

Det här är ett exempel på den här typen av varning:

![Utgående DOS](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Resursanalys
Security Center resource analys fokuserar på plattformen som en tjänst (PaaS)-tjänster, till exempel hello integrering med hello [Azure SQL Database hotidentifiering](../sql-database/sql-database-threat-detection.md) funktion. Security Center utlöser utifrån hello analys resultat från dessa områden en resurs-relaterade avisering.

### <a name="potential-sql-injection"></a>Potentiell SQL-inmatning
SQL injection är en attack där skadlig kod infogas i strängar som senare skickas tooan instans av SQL Server för parsning och körning. Sårbarhet för den här typen av inmatning bör granskas för alla procedurer som konstruerar SQL-instruktioner eftersom SQL Server kör alla syntaktiskt giltiga frågor som den tar emot. SQL-Hotidentifiering använder maskininlärning, beteendeanalys och avvikelseidentifiering identifiering toodetermine misstänkta händelser som kan ske i Azure SQL-databaser. Exempel:

* Försök till databasåtkomst av en tidigare anställd
* SQL-inmatningsattacker
* Onormal åtkomst tooa produktionsdatabasen från en användare i hemmet

![Varning om potentiell SQL-inmatning](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

hello informationen i den här aviseringen kan vara används tooidentify hello angripna resursen och hello identifieringstiden hello tillståndet för hello-attack. Det ger också en länk toofurther undersökningen steg.

### <a name="vulnerability-toosql-injection"></a>Säkerhetsproblem tooSQL injektion
Den här aviseringen utlöses när ett programfel hittas på en databas. Den här varningen kan tyda på ett möjligt säkerhetsproblem tooSQL injektion attacker.

![Varning om potentiell SQL-inmatning](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Onormal åtkomst från en okänd plats
Den här aviseringen utlöses när en åtkomsthändelse från en okänd IP-adress har identifierats på hello-servern, vilket inte påträffades i hello senaste perioden.

![Varning om onormal åtkomst](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Sammanhangsbaserad information
Vid en undersökning analytiker behöver extra kontexten tooreach en domen om hello uppbyggnad hello hot och hur toomitigate den.  Till exempel ett nätverk avvikelseidentifiering upptäcktes, men utan att förstå vad som händer hello nätverket eller med beaktande toohello riktade resurs den är varje hårda toounderstand vilka åtgärder tootake nästa. tooaid med som en säkerhetsincident kan omfatta artefakter, relaterade händelser och information som kan hjälpa hello ansvarige. hello tillgängligheten för ytterligare information kan variera beroende på hello typ av hot har upptäckts hello konfigurationen av din miljö och inte tillgängliga för alla incidenter som säkerhet.

Om ytterligare information är tillgänglig, visas den i hello säkerhetsincident nedan hello listan över aviseringar. Detta kan innehålla information som:

- Logga raderingshändelser
- PNP-enhet ansluten från okänd källa
- Aviseringar som inte medför åtgärd 

![Varning om onormal åtkomst](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>Se även
I den här artikeln har du lärt dig om hello olika typer av säkerhetsaviseringar i Security Center. toolearn mer om Security Center finns hello följande:

* [Hantera säkerhetsincidenter i Azure Security Center](security-center-incident.md)
* [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md)
* [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md)
* [Vanliga frågor om Azure Security Center](security-center-faq.md): finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/): Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
