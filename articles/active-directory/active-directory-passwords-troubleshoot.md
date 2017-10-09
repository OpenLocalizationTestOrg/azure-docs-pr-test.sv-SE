---
title: "Felsöka: Azure AD SSPR | Microsoft Docs"
description: "Felsöka Azure AD Självbetjäning för lösenordsåterställning"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 361ef5f37e4e9de83d3f9d5a8e5177d186fe86d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-self-service-password-reset"></a>Hur tootroubleshoot Självbetjäning för lösenordsåterställning

Om du har problem med lösenordsåterställning via självbetjäning kan hello-objekt som följer hjälpa dig tooget saker arbetar snabbt.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-may-see"></a>Felsöka självbetjäning lösenordsåterställning som en användare kan se

| Fel | Information | Teknisk information |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Vi beklagar <br> Du kan inte återställa ditt lösenord just nu eftersom din administratör har inaktiverat återställning av lösenord för din organisation. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem tooenable den här funktionen. toolearn läsa fler [hjälp, jag har glömt mitt Azure AD-lösenord](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Vi har upptäckt att återställning av lösenord inte har aktiverats av administratören. Kontakta administratören och be dem tooenable för återställning av lösenord för din organisation. |
| WritebackNotEnabled = 10 |Vi beklagar <br> Du kan inte återställa ditt lösenord just nu eftersom administratören inte har aktiverat en tjänst som behövs för din organisation. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem toocheck organisationens konfiguration. toolearn mer om den här nödvändiga tjänsten läsa [konfigurera tillbakaskrivning av lösenord](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-writeback#configuring-password-writeback). | SSPR_0010: Vi har upptäckt att tillbakaskrivning av lösenord inte har aktiverats. Kontakta administratören och be dem tooenable tillbakaskrivning av lösenord. |
| SsprNotEnabledInUserPolicy = 11 | Vi beklagar  <br> Du kan inte återställa ditt lösenord just nu eftersom din administratör inte har konfigurerat för återställning av lösenord för din organisation. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem tooconfigure lösenordsåterställning. toolearn mer om lösenord återställa konfigurationen skrivskyddade hello artikel [Snabbstart: Azure AD Självbetjäning för lösenordsåterställning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Din organisation har definierat en princip för lösenordsåterställning. Kontakta administratören och be dem toodefine en princip för lösenordsåterställning. |
| UserNotLicensed = 12 | Vi beklagar <br> Du kan inte återställa ditt lösenord just nu eftersom nödvändiga licenser saknas från din organisation. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem toocheck licenstilldelning. toolearn mer om licensiering hello artikeln [Licensing krav för Azure AD-lösenordet för självbetjäning återställa](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Din organisation har inte hello krävs licenser nödvändiga tooperform lösenordsåterställning. Kontakta administratören och be dem tooreview licenstilldelning. |
| UserNotMemberOfScopedAccessGroup = 13 | Vi beklagar <br> Du kan inte återställa ditt lösenord just nu eftersom din administratör inte har konfigurerat ditt konto toouse lösenordsåterställning. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem tooconfigure ditt konto för återställning av lösenord. Mer information om konfigurationen av kontot för lösenordsåterställning hello artikeln toolearn [lanserar lösenordsåterställning för användare](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012: Du är inte medlem i en grupp som har aktiverats för lösenordsåterställning. Kontakta administratören och begär toobe tillagda toohello gruppen. |
| UserNotProperlyConfigured = 14 | Vi beklagar <br> Du kan inte återställa ditt lösenord just nu eftersom det saknas nödvändig information från ditt konto. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem tooreset lösenordet åt dig. När du har tooyour konto kan du lära dig hur registrera hello nödvändig information genom att följa hello stegen i artikeln hello [registrera dig för lösenordsåterställning via självbetjäning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-reset-register). | SSPR_0014: Ytterligare säkerhetsinformation är nödvändiga tooreset ditt lösenord. tooproceed, kontakta din administratör och be dem tooreset ditt lösenord. När du har åtkomst tooyour konto kan du registrera ytterligare säkerhetsinformation på https://aka.ms/ssprsetup. Din administratör kan lägga till ytterligare information om tooyour säkerhetskonto genom att följa stegen hello i [anges, och Läs autentiseringsdata för återställning av lösenord](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired = 29 | Vi beklagar <br> Vi kan inte återställa ditt lösenord just nu på grund av ett problem med din organisations lösenord återställa konfigurationen. Det finns inga ytterligare åtgärder som du kan vidta tooresolve för den här situationen. Kontakta administratören och be dem tooinvestigate. toolearn mer om hello potentiella problem hello artikeln [felsöka tillbakaskrivning av lösenord](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Vi är tooreset lösenordet förfaller tooan fel i din lokala konfiguration. Kontakta administratören och be dem tooinvestigate. |
| OnPremisesConnectivityError = 30 | Vi beklagar <br> Vi kan inte återställa ditt lösenord just nu på grund av anslutningen problem tooyour organisation. Det finns ingen åtgärd tootake just nu, men hello problemet kan eventuellt lösas om du försöker igen senare. Kontakta administratören och be dem tooinvestigate om hello problemet kvarstår. information om problem med nätverksanslutningen läsa toolearn [felsöka tillbakaskrivning av lösenord anslutning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Vi kan inte återställa ditt lösenord på grund av tooa dålig anslutning med din lokala miljö. Kontakta administratören och be dem tooinvestigate.|


## <a name="troubleshoot-password-reset-configuration-in-hello-azure-portal"></a>Felsöka lösenord Återställ konfigurationen i hello Azure-portalen

| **Fel** | Lösning |
| --- | --- |
| Visas inte hello **lösenordsåterställning** avsnitt under Azure AD i hello Azure-portalen | Detta kan inträffa om du inte har en Azure AD Premium eller Basic licens toohello administratör utför hello igen. <br> Detta kan lösas genom att tilldela en licens toohello administratörskontot i fråga med hello artikel [tilldela kontrollerar och lösa problem med licenser](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| En specifik konfiguration-alternativet visas inte | Många hello UI-element är dolda tills behövs. Försök att aktivera alla hello alternativ toosee. |
| Jag kan inte se hello **lokalt integration** fliken | Det här alternativet blir bara visas om du har laddat ned Azure AD Connect och konfigurerat tillbakaskrivning av lösenord. Mer information om det här avsnittet finns hello artikel [komma igång med Azure AD Connect med standardinställningar](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Felsöka lösenord återställa reporting

| **Fel** | Lösning |
| --- | --- |
| Jag kan inte se några lösenord management-aktivitetstyper som visas i hello **Self-Service lösenordshantering** audit händelsekategori | Detta kan inträffa om du inte har en Azure AD Premium eller Basic licens toohello administratör utför hello igen. <br> Detta kan lösas genom att tilldela en licens toohello administratörskontot i fråga med hello artikeln [tilldela kontrollerar och lösa problem med licenser] |
| Användaren registreringar visa flera gånger | För närvarande när en användare som registrerar logga vi just nu varje enskild typ av data som registrerats som en separat händelse. <br> Om du vill tooaggregate dessa data, kan du hämta hello rapporten och öppna hello data som en pivottabell i excel toohave mer flexibilitet.

## <a name="troubleshoot-password-reset-registration-portal"></a>Felsöka registreringsportalen för lösenordsåterställning

| **Fel** | Lösning |
| --- | --- |
| Katalogen är inte aktiverad för återställning av lösenord **administratören har inte aktiverat du toouse funktionen** | Växeln hello **Self service lösenordsåterställning aktiverat** flaggan för**en grupp** eller **alla** och på **spara** |
| Användaren har inte en Azure AD Premium eller Basic licens **administratören har inte aktiverat du toouse funktionen** | Detta kan inträffa om du inte har en Azure AD Premium eller Basic licens toohello administratör utför hello igen. <br> Detta kan lösas genom att tilldela en licens toohello administratörskontot i fråga med hello artikel [tilldela kontrollerar och lösa problem med licenser](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Fel vid bearbetning av begäran | Detta kan orsakas av många problem, men vanligtvis felet orsakas av något avbrott eller konfiguration av problem med en tjänst. Om du ser detta fel och den påverkar ditt företag, kan du kontakta Microsoft support för ytterligare hjälp. |

## <a name="troubleshoot-password-reset-portal"></a>Felsökning av lösenordsåterställning, portal

| **Fel** | Lösning |
| --- | --- |
| Katalogen är inte aktiverat för återställning av lösenord. | Växeln hello **Self service lösenordsåterställning aktiverat** flaggan för**en grupp** eller **alla** och på **spara** |
| Användaren har inte en Azure AD Premium eller Basic-licens | Detta kan inträffa om du inte har en Azure AD Premium eller Basic licens toohello administratör utför hello igen. <br> Detta kan lösas genom att tilldela en licens toohello administratörskontot i fråga med hello artikel [tilldela kontrollerar och lösa problem med licenser](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Katalogen är aktiverad för återställning av lösenord, men användaren har autentiseringsinformation saknas eller är a-format | Se till att användaren har formaterats korrekt kontaktdata på filen i katalogen hello innan du fortsätter. Mer information om det här avsnittet finns hello artikel [Data som används av Azure AD Självbetjäning för återställning av lösenord](active-directory-passwords-data.md). |
| Katalogen är aktiverad för återställning av lösenord, men en användare bara har en dataenhet kontakta när principen anges toorequire två verifieringssteg | Kontrollera att användaren har minst två korrekt konfigurerade kontaktmetoder (exempel: mobiltelefon **och** Arbetstelefon) innan du fortsätter. |
| Katalogen är aktiverad för återställning av lösenord, och användaren är korrekt konfigurerad, men användaren är toobe kontaktas | Detta kan vara hello resultatet av en tillfällig tjänstfel eller felaktig kontaktdata som vi inte kunde identifieras korrekt. <br> <br> Om hello användare behöver vänta 10 sekunder en försök igen och ”kontaktar du administratören” visas. Klicka på försök igen försöker hello anropet medan du klickar på ”kontaktar du administratören” skickas en formuläret e-tooadministrators som begär en lösenordsåterställning toobe utföras för användarkontot. |
| Användaren får aldrig hello lösenordsåterställning SMS eller telefonsamtal | Detta kan bero hello a utformad telefonnummer i hello directory. Kontrollera att hello telefonnummer har formatet hello ”+ kopia xxxyyyzzzzXeeee”. <br> <br> Återställning av lösenord stöder inte tillägg, även om du anger en i hello directory (de tas bort innan anropet hello skickas). Försök med ett tal utan ett tillägg, eller integrera hello tillägg i hello telefonnummer i din PBX. |
| Användaren får aldrig lösenordsåterställning av e-post | hello vanligaste orsaken till problemet är att hello-meddelande har avvisats av ett skräppostfilter. Kontrollera din skräppost, mappen Skräppost eller borttagna objekt för hello e-post. <br> Se också till att du kontrollerar hello rätt e-post för hello-meddelande. |
| Jag har angett en princip för lösenordsåterställning, men när ett administratörskontot använder återställning av lösenord, tillämpas inte principen | Microsoft hanterar och kontroller Hej administratör lösenordsåterställning princip tooensure hello högsta säkerhetsnivå. |
| Användaren som förhindrade försöker utföra för många gånger under en dag för återställning av lösenord | Vi implementera en automatisk begränsning mekanism tooblock användare från att försöka tooreset sina lösenord för många gånger under en kort tidsperiod. Detta inträffar när: <br> 1. Användaren försöker toovalidate ett telefonnummer 5 gånger under en timme. <br> 2. Användaren försöker toouse hello säkerhet frågor gate 5 gånger under en timme. <br> 3. Användaren försöker tooreset ett lösenord för hello samma användarkonto 5 gånger under en timme. <br> toofix kan instruera hello användaren toowait 24 timmar efter hello senaste försöket, och hello användaren kommer sedan vara kan tooreset sina lösenord. |
| Användaren ser ett fel vid validering av sitt telefonnummer | Felet uppstår när hello telefonnummer som angetts inte matchar hello telefonnumret för filen. Kontrollera att hello användaren anger hello fullständiga telefonnummer, inklusive området och land kod vid försök toouse en telefonbaserad metod för återställning av lösenord. |
| Fel vid bearbetning av begäran | Detta kan orsakas av många problem, men vanligtvis felet orsakas av något avbrott eller konfiguration av problem med en tjänst. Om du ser detta fel och den påverkar ditt företag, kan du kontakta Microsoft support för ytterligare hjälp. |

## <a name="troubleshoot-password-writeback"></a>Felsöka tillbakaskrivning av lösenord

| **Fel** | Lösning |
| --- | --- |
| Lösenord återställa tjänsten startar inte lokalt med fel 6800 i hello Azure AD Connect datorn programmets händelselogg. <br> <br> När onboarding, federerad eller lösenords-hash synkroniserade kan inte användare återställa sina lösenord. | När tillbakaskrivning av lösenord är aktiverat, anropar hello Synkroniseringsmotorn hello tillbakaskrivning tooperform hello bibliotekskonfiguration (onboarding) av pratar toohello onboarding Molntjänsten. Fel uppstod under onboarding eller när hello WCF-slutpunkt för tillbakaskrivning av lösenord resultat i fel i händelseloggen för hello i händelseloggen för din Azure AD Connect-datorn. <br> <br> Under omstart av ADSync-tjänsten om tillbakaskrivning konfigurerades har hello WCF-slutpunkt startats. Dock om hello start av hello slutpunkten misslyckas, kommer logga en händelse 6800 och låta hello sync-tjänsten startades. Förekomst av den här händelsen innebär att tillbakaskrivning av lösenord hello-slutpunkten inte har har startats. Händelselogginformation för den här händelsen (6800) tillsammans med händelseloggposter generera genom PasswordResetService komponenten anger varför hello slutpunkten kunde inte startas. Granska felen händelseloggen och försök toorestart hello Azure AD Connect om tillbakaskrivning av lösenord fortfarande inte fungerar. Om hello problemet kvarstår försök toodisable och aktivera tillbakaskrivning av lösenord på nytt.
| När en användare försöker tooreset ett lösenord eller låsa upp ett konto med aktiverat tillbakaskrivning av lösenord, misslyckas hello åtgärden. <br> <br> Dessutom kan du se en händelse i händelseloggen som hello Azure AD Connect har som innehåller ”: Synkroniseringsmotorn returnerade ett fel hr = 800700CE, message = hello filnamn eller filnamnstillägg är för lång” när hello upplåsande åtgärd utförs. | Hitta hello Active Directory-konto för Azure AD Connect och Återställ hello lösenord toocontain högst 127 tecken. Öppna sedan synkroniseringstjänsten hello Start-menyn. Navigera tooConnectors och hitta hello Active Directory-kopplingen. Markera den och klicka på Egenskaper. Navigera toohello autentiseringsuppgifter och ange hello nytt lösenord. Välj OK tooclose hello sida. |
| Vid hello sista steget i hello Azure AD Connect-installationen visas ett felmeddelande som anger att tillbakaskrivning av lösenord inte att konfigurera. <br> <br> hello Azure AD Connect programmets händelselogg innehåller fel 32009 med texten ”fel vid hämtning auth token”. | Detta fel uppstår i hello följande två fall:<br> <br> a. Du har angett ett felaktigt lösenord för hello globala administratörskonto angetts hello början av hello Azure AD Connect-installationen.<br> b. Du har försökt toouse en federerad användare för hello globala administratörskonto angetts hello början av hello Azure AD Connect-installationen.<br> <br> toofix felet, se till att du inte använder ett federerat konto för hello global administratör du angett hello början av hello Azure AD Connect-installationen och det angivna hello lösenordet är korrekt. |
| hello Azure AD Connect datorn händelseloggen innehåller fel 32002 utlöstes av hello PasswordResetService. <br> <br> läser hello-fel: ”fel vid anslutning tooServiceBus, hello tokenleverantör har tooprovide en säkerhetstoken...” | Din lokala miljö är inte kan tooconnect toohello service bus-slutpunkten i hello molnet. Det här felet beror normalt på en brandväggsregel blockerar en utgående anslutning tooa viss port eller webbadress. Se [krav på](active-directory-passwords-how-it-works.md#network-requirements) för mer information. När du har uppdaterat reglerna bör omstart hello Azure AD Connect-datorn och tillbakaskrivning av lösenord börja fungera igen. |
| När arbetar för vissa tid, federerad eller lösenords-hash har synkroniserats kan användare återställa sina lösenord. | I sällsynta fall misslyckas hello tillbakaskrivning av lösenord tjänsten toorestart när Azure AD Connect har startats om. I dessa fall, kontrollera först om tillbakaskrivning av lösenord visas toobe aktiverad på plats. Detta kan göras med hello Azure AD Connect-guiden eller powershell (HowTos finns i avsnittet ovan). Om funktionen hello visas toobe aktiverat, försök att aktivera eller inaktivera hello funktionen igen hello Användargränssnittet eller PowerShell. Om det inte fungerar, försök helt avinstallera och installera om Azure AD Connect. |
| Federerade eller lösenords-hash synkroniserade användare som försöker tooreset sina lösenord finns ett fel när skickar hello lösenord som anger det uppstod ett problem med tjänsten. <br ><br> Dessutom toothis, under lösenord återställa operations kan det uppstå ett fel om hanteringsagenten nekas åtkomst i din på lokala händelseloggar. | Om du se felen i händelseloggen kontrollerar du att hello AD MA-konto (som har angetts i guiden hello när hello configuration) har hello behörighet för tillbakaskrivning av lösenord. <br> <br> **När behörigheten ges kan det ta upp too1 timme för hello behörigheter tootrickle via sdprop bakgrundsaktivitet på hello DC.** <br> <br> För att återställa toowork lösenord, måste hello behörighet toobe stämplats på hello säkerhetsbeskrivningen för hello användarobjekt vars lösenord återställs. Förrän den här behörigheten visas på hello användarobjektet, fortsätter lösenordsåterställning toofail med åtkomst nekad. |
| Federerade eller lösenords-hash synkroniserade användare som försöker tooreset sina lösenord finns ett fel när skickar hello lösenord som anger det uppstod ett problem med tjänsten. <br> <br> Dessutom toothis, under lösenord återställa operations kan det uppstå ett fel i händelseloggarna från hello Azure AD Connect-tjänsten som anger felmeddelandet ”Det gick inte att hitta objektet”. | Det här felet tyder vanligtvis på att hello Synkroniseringsmotorn är toofind hello användarobjektet i hello AAD anslutningsplatsen eller hello MV- eller AD connector utrymme länkat. <br> <br> tootroubleshoot, se till att användaren hello verkligen synkroniseras från lokala tooAAD via hello aktuell instans av Azure AD Connect och inspektera hello objekt i hello-kopplingens utrymmen och MV hello tillstånd. Bekräfta att hello AD CS-objektet är connector toohello MV-objekt via hello ”Microsoft.InfromADUserAccountEnabled.xxx” regeln.|
| Federerade eller lösenords-hash synkroniserade användare som försöker tooreset sina lösenord finns ett fel när skickar hello lösenord som anger det uppstod ett problem med tjänsten. <br> <br> Dessutom toothis, under lösenord återställa operations kan det uppstå ett fel i händelseloggarna från hello Azure AD Connect-tjänsten som anger felet ”flera matchningar hittades”. | Detta anger att hello Synkroniseringsmotorn upptäckte att hello MV-objekt är anslutna toomore än en AD CS-objekt via hello ”Microsoft.InfromADUserAccountEnabled.xxx”. Det innebär att användaren hello har ett aktiverat konto i mer än en skog. **Det här scenariot stöds inte för tillbakaskrivning av lösenord.** |
| Lösenordsåtgärder misslyckas med ett konfigurationsfel. hello programmets händelselogg innehåller <br> <br> Azure AD Connect fel 6329 med text: 0x8023061f (hello åtgärden misslyckades eftersom synkronisering av lösenord inte är aktiverat på den här hanteringsagenten). | Det här inträffar om hello Azure AD Connect-konfigurationen är ändrade tooadd en ny AD-skog (eller tooremove och readd en befintlig skog) efter hello tillbakaskrivning av lösenord funktionen har redan aktiverats. Lösenordsåtgärder för användare i ett sådant tillagda nyligen skogar misslyckas. toofix hello problemet, inaktivera och återaktivera hello tillbakaskrivning av lösenord funktionen efter konfigurationsändringar för hello skogen har slutförts. |

## <a name="password-writeback-event-log-error-codes"></a>Felkoder för lösenord tillbakaskrivning av händelseloggen

Ett bra tips när du felsöker problem med tillbakaskrivning av lösenord är tooinspect program i händelseloggen på datorn Azure AD Connect. Den här händelseloggen innehåller händelser från två källor av intresse för tillbakaskrivning av lösenord. Hej PasswordResetService källa beskriver åtgärder och åtgärden för problem relaterade toohello för tillbakaskrivning av lösenord. hello ADSync källa beskriver åtgärder och problem relaterade toosetting lösenord i din AD-miljö.

### <a name="source-of-event-is-adsync"></a>Källan för händelsen är ADSync

| Kod | Namn/meddelande | Beskrivning |
| --- | --- | --- |
| 6329 | BORGEN: MMS(4924) 0x80230619 – ”en begränsning förhindrar hello lösenord från att ändra toohello aktuella som anges”. | Den här händelsen inträffar när hello tillbakaskrivning av lösenord tjänsten försöker tooset ett lösenord på din lokala katalog som inte uppfyller hello lösenordets ålder, historik, komplexitet eller filtrera kraven i hello domän. <br> <br> Om du har en minsta lösenord ålder och nyligen har ändrat hello lösenordet i fönstret tid kan du inte är kan toochange hello lösenord igen tills hello angetts ålder i din domän. Minimiålder ska ställas in too0 i testsyfte kan. <br> <br> Om du har tidigare lösenordskrav aktiverad och du måste välja ett lösenord som inte har använts i hello sista N gånger, där N är hello lösenordshistorik inställningen. Om du väljer ett lösenord som har använts i hello sista N gånger, sedan visas ett fel i det här fallet. Historik ska ställas in too0 i testsyfte kan. <br> <br> Om du har kraven på lösenordskomplexitet alla tillämpas när hello användare försöker toochange eller återställa ett lösenord. <br> <br> Om du har aktiverat lösenordsfilter och en användare väljer ett lösenord som inte uppfyller hello filtreringskriterier, hello Återställ eller ändra åtgärden misslyckas. |
| HR 8023042 | Synkroniseringsmotorn returnerade ett fel hr = 80230402, message = en försök tooget ett objekt misslyckades eftersom det finns duplicerade poster med hello samma fästpunkt | Den här händelsen inträffar när hello samma användar-id är aktiverat i flera domäner. Till exempel om du synkroniserar konto/resursskogar och har hello samma användar-id finns och är aktiverad i varje det här felet kan uppstå. <br> <br> Det här felet kan också inträffa om du använder en icke-unikt fästpunktsattributet (till exempel alias eller UPN) och två användare delar samma fästpunktsattributet. <br> <br> tooresolve problemet, se till att du inte har några dubbletter av användare inom din domäner och att du använder en unik fästpunktsattributet för varje användare. |

### <a name="source-of-event-is-passwordresetservice"></a>Källan för händelsen är PasswordResetService

| Kod | Namn/meddelande | Beskrivning |
| --- | --- | --- |
| 31001 | PasswordResetStart | Den här händelsen indikerar att hello lokala tjänsten identifierats en begäran för en federerad eller lösenord hash-synkroniserade användare från hello molnet för lösenordsåterställning. Den här händelsen är hello första händelsen i alla lösenord återställa tillbakaskrivningen. |
| 31002 | PasswordResetSuccess | Den här händelsen indikerar att en användare har markerat ett nytt lösenord under en åtgärd för återställning av lösenord fastställde vi att det här lösenordet uppfyller företagets lösenordsprincip och lösenordet har skrivits tillbaka toohello lokala AD-miljö. |
| 31003 | PasswordResetFail | Den här händelsen indikerar att en användare har valt ett lösenord och att lösenordet har anlänt toohello lokala miljö, men ett fel uppstod när vi försökte tooset hello lösenord i hello lokala AD-miljö. Detta kan bero på flera orsaker: <br> <br> hello användarens lösenord inte uppfyller hello ålder, historik och komplexitet eller filtrera krav för hello domän. Prova på ett helt nytt lösenord tooresolve detta. <br> <br> hello MA-tjänstkontot har inte hello behörighet tooset hello nytt lösenord hello användarkontot i fråga. <br> <br> hello användarkontot finns i en skyddad grupp, till exempel domän- eller enterprise administratörer som inte tillåter lösenord uppsättning åtgärder. |
| 31004 | OnboardingEventStart | Den här händelsen inträffar om du aktiverar tillbakaskrivning av lösenord med Azure AD Connect och anger att vi startas onboarding din organisation toohello webbtjänst för tillbakaskrivning av lösenord. |
| 31005 | OnboardingEventSuccess | Den här händelsen indikerar hello onboarding-processen har slutförts och att kapaciteten för tillbakaskrivning av lösenord är klar toouse. |
| 31006 | ChangePasswordStart | Den här händelsen indikerar att hello lokala tjänsten identifierats en begäran för en federerad eller lösenord hash-synkroniserade användare från hello molnet. Den här händelsen är hello första händelsen i varje tillbakaskrivning av åtgärd för lösenordsbyte. |
| 31007 | ChangePasswordSuccess | Den här händelsen indikerar att en användare har markerat ett nytt lösenord under en åtgärd för lösenordsbyte fastställde vi att det här lösenordet uppfyller företagets lösenordsprincip och lösenordet har skrivits tillbaka toohello lokala AD-miljö. |
| 31008 | ChangePasswordFail | Den här händelsen indikerar att en användare har valt ett lösenord och att lösenordet har anlänt toohello lokala miljö, men ett fel uppstod när vi försökte tooset hello lösenord i hello lokala AD-miljö. Detta kan bero på flera orsaker: <br> <br> hello användarens lösenord inte uppfyller hello ålder, historik och komplexitet eller filtrera krav för hello domän. Prova på ett helt nytt lösenord tooresolve detta. <br> <br> hello MA-tjänstkontot har inte hello behörighet tooset hello nytt lösenord hello användarkontot i fråga. <br> <br> hello användarkontot finns i en skyddad grupp, till exempel domän- eller enterprise administratörer som inte tillåter lösenord uppsättning åtgärder. |
| 31009 | ResetUserPasswordByAdminStart | hello lokalt tjänsten upptäckte en lösenordsåterställning begäran för en federerad eller lösenord hash-synkroniserade användare från Hej administratör för en användares räkning. Den här händelsen är hello första händelsen i varje admin-initierade återställning tillbakaskrivning av lösenord. |
| 31010 | ResetUserPasswordByAdminSuccess | Hej administratör har markerat ett nytt lösenord under en admin-initierade återställning av lösenord, fastställde vi att det här lösenordet uppfyller företagets lösenordsprincip och lösenordet har skrivits tillbaka toohello lokala AD-miljö. |
| 31011 | ResetUserPasswordByAdminFail | Hej administratör valt ett lösenord för en användares räkning och lösenordet anlänt har toohello lokala miljö, men ett fel uppstod när vi försökte tooset hello lösenord i hello lokala AD-miljö. Detta kan bero på flera orsaker: <br> <br> hello användarens lösenord inte uppfyller hello ålder, historik och komplexitet eller filtrera krav för hello domän. Prova på ett helt nytt lösenord tooresolve detta. <br> <br> hello MA-tjänstkontot har inte hello behörighet tooset hello nytt lösenord hello användarkontot i fråga. <br> <br> hello användarkontot finns i en skyddad grupp, till exempel domän- eller enterprise administratörer som inte tillåter lösenord uppsättning åtgärder. |
| 31012 | OffboardingEventStart | Den här händelsen inträffar om du inaktiverar tillbakaskrivning av lösenord med Azure AD Connect och anger att vi startas övervakningsinstrumentpanelen din organisation toohello webbtjänst för tillbakaskrivning av lösenord. |
| 31013| OffboardingEventSuccess| Den här händelsen indikerar hello övervakningsinstrumentpanelen processen lyckades och att kapaciteten för tillbakaskrivning av lösenord har inaktiverats. |
| 31014| OffboardingEventFail| Den här händelsen indikerar hello övervakningsinstrumentpanelen processen inte lyckades. Detta kan bero på tooa Behörighetsfel på hello molnet eller lokalt administratörskonto anges under konfigurationen, eller eftersom du försöker toouse en global administratör för federerade moln när du inaktiverar tillbakaskrivning av lösenord. toofix detta, kontrollera din behörighet och att du inte är något federerad konto när du konfigurerar hello kapaciteten för tillbakaskrivning av lösenord.|
| 31015| WriteBackServiceStarted| Den här händelsen indikerar att hello tillbakaskrivning av lösenord tjänsten har startats och är redo tooaccept lösenord management-begäranden från hello molnet.|
| 31016| WriteBackServiceStopped| Den här händelsen indikerar att hello tillbakaskrivning av lösenord-tjänsten har stoppats och att lösenordet management förfrågningar från hello molnet kommer att misslyckas.|
| 31017| AuthTokenSuccess| Den här händelsen indikerar att vi har hämtat en Autentiseringstoken för hello global administratör anges under Azure AD Connect toostart hello övervakningsinstrumentpanelen eller onboarding installationsprocessen.|
| 31018| KeyPairCreationSuccess| Den här händelsen anger vi skapat hello krypteringsnyckel för lösenord som används tooencrypt lösenord från hello toobe skickas tooyour lokalt molnmiljö.|
| 32000| UnknownError| Den här händelsen indikerar ett okänt fel under en åtgärd för hantering. Titta på hello Undantagstext i hello händelse för mer information. Om du har problem, försök att inaktivera och aktivera tillbakaskrivning av lösenord. Om det inte hjälper kan innehålla en kopia av händelseloggen tillsammans med hello spårnings-id angivna insider tooyour supportteknikern.|
| 32001| ServiceError| Den här händelsen indikerar att det uppstod ett fel vid anslutning toohello moln lösenord återställa tjänsten och sker vanligtvis när hello lokalt tjänsten kunde tooconnect toohello lösenordsåterställning av webbtjänsten.|
| 32002| ServiceBusError| Den här händelsen indikerar att det uppstod ett fel vid anslutning tooyour klient service bus-instans. Detta kan inträffa eftersom du blockerar utgående anslutningar i din lokala miljö. Kontrollera din brandvägg tooensure du Tillåt anslutningar via TCP 443 och toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ och försök igen. Om du fortfarande har problem, försök att inaktivera och aktivera tillbakaskrivning av lösenord.|
| 32003| InPutValidationError| Den här händelsen indikerar att hello inmatningen som skickades tooour-webbtjänstens API var ogiltigt. Hello försök igen.|
| 32004| DecryptionError| Den här händelsen indikerar att det gick inte att dekryptera hello lösenord som anlänt från hello molnet. Det kan bero på ett dekryptering viktiga matchningsfel mellan hello tjänst i molnet och lokala miljö. tooresolve, inaktivera och återaktivera tillbakaskrivning av lösenord i din lokala miljö.|
| 32005| ConfigurationError| Under onboarding spara vi klient-specifik information i en konfigurationsfil i din lokala miljö. Den här händelsen indikerar att det uppstod ett fel vid sparande av den här filen eller som när hello tjänsten startades det gick inte att läsa hello-filen. toofix problemet, försök att inaktivera och aktivera tillbakaskrivning av lösenord tooforce en omarbetning av konfigurationsfilen.|
| 32007| OnBoardingConfigUpdateError| Under onboarding skicka vi data från hello molnet toohello lokala tjänsten för återställning av lösenord. Dessa data skrivs sedan tooan InMemory-filen innan den skickas toohello sync service toostore informationen på ett säkert sätt på disken. Den här händelsen tyder på problem med skrivning eller uppdatera data i minnet. toofix problemet, försök att inaktivera och aktivera tillbakaskrivning av lösenord tooforce en omarbetning av den här konfigurationen.|
| 32008| ValidationError| Den här händelsen anger vi tagit emot ett ogiltigt svar från hello lösenordsåterställning av webbtjänsten. toofix problemet, försök att inaktivera och aktivera tillbakaskrivning av lösenord.|
| 32009| AuthTokenError| Den här händelsen indikerar att det inte gick att hämta tillstånd token för hello globala administratörskonto anges under installationen av Azure AD Connect. Det här felet kan orsakas av ett felaktigt användarnamn eller lösenord som angetts för hello globala administratörskonto eller eftersom hello globala administratörskonto har angetts är federerat. toofix problemet, kör konfiguration med hello korrigera användarnamn och lösenord och kontrollera Hej administratör är ett hanterat konto (endast molnbaserad eller lösenord synkroniseras).|
| 32010| CryptoError| Den här händelsen indikerar ett fel uppstod när genererar hello krypteringsnyckel för lösenord eller dekryptera ett lösenord som kommer från hello-Molntjänsten. Det här felet sannolikt indikerar ett problem med din miljö. Titta på hello information om din händelseloggen toolearn mer och lösa problemet. Du kan också prova att inaktivera och aktivera igen hello tillbakaskrivning av lösenord service tooresolve detta.|
| 32011| OnBoardingServiceError| Den här händelsen indikerar att hello lokala tjänsten inte kunde korrekt kommunicera med hello web service tooinitiate hello onboarding lösenordsåterställningen. Detta kan bero på en brandväggsregel eller hämta en token för autentisering för din klient. toofix, se till att du inte blockerar utgående anslutningar via TCP 443 och TCP 9350 9354 eller toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ och den hello AAD-administratörskonto som du använder tooonboard inte är federerat.|
| 32013| OffBoardingError| Den här händelsen indikerar att hello lokala tjänsten inte kunde korrekt kommunicera med hello web service tooinitiate hello övervakningsinstrumentpanelen lösenordsåterställningen. Detta kan bero på en brandväggsregel eller hämta ett Autentiseringstoken för din klient. toofix, se till att du inte blockerar utgående anslutningar via 443 eller toohttps://ssprsbprodncu-sb.accesscontrol.windows.net/ och den hello AAD-administratörskonto som du använder toooffboard inte är federerat.|
| 32014| ServiceBusWarning| Den här händelsen indikerar att vi hade tooretry tooconnect tooyour klientorganisationens service bus-instans. Under normala förhållanden bör detta inte vara ett problem, men om du ser den här händelsen många gånger bör du överväga att kontrollera ditt nätverk anslutning tooservice bus, särskilt om det är en hög fördröjning eller långsam anslutning.|
| 32015| ReportServiceHealthError| I ordning toomonitor hello hälsotillståndet för din tjänst för tillbakaskrivning av lösenord skicka pulsslag data tooour lösenordsåterställning webbtjänsten var femte minut. Den här händelsen indikerar att ett fel uppstod när den skickar information tillbaka toohello cloud webbtjänsten för hälsotillstånd. Den här hälsoinformation innehåller inte en OII eller personligt identifierbar information data och är rent en pulsslag och grundläggande statistik så att vi kan ge information om status för tjänsten i hello molnet.|
| 33001| ADUnKnownError| Den här händelsen indikerar att det uppstod ett okänt fel som returneras av AD, händelseloggen hello Azure AD Connect-servern för händelser från hello ADSync källan för mer information om felet.|
| 33002| ADUserNotFoundError| Den här händelsen indikerar att hello-användare som försöker tooreset eller ändra ett lösenord inte hittades i hello lokala katalog. Detta kan inträffa när hello användare som har tagits bort lokalt inte men hello moln, eller om det finns ett problem med synkronisering. Kontrollera synkroniseringsloggarna och hello senast några sync kör detaljer för mer information.|
| 33003| ADMutliMatchError| När en lösenordsåterställning eller ändringsbegäran härstammar från hello molntjänster, använder vi hello molnet fästpunkt anges under hello installationen av Azure AD Connect toodetermine hur toolink som begär den bakre tooa användare i din lokala miljö. Den här händelsen indikerar att det finns två användare i din lokala katalog med hello samma fästpunktsattributet i molnet. Kontrollera synkroniseringsloggarna och hello senast några sync kör detaljer för mer information.|
| 33004| ADPermissionsError| Den här händelsen anger att hello hanteringsagenten tjänstkontot inte har hello lämpliga behörigheter för hello-kontot i fråga tooset ett nytt lösenord. Kontrollera att hello MA-konto i hello användarens skog har behörigheter för återställning och ändra lösenord för alla objekt i hello skog. Mer information om hur gör toothis finns i steg 4: Konfigurera hello Active Directory-behörigheter.|
| 33005| ADUserAccountDisabled| Den här händelsen indikerar att vi försökte tooreset eller ändra ett lösenord för ett konto som har inaktiverats av lokala. Aktivera hello-konto och försök igen om hello.|
| 33006| ADUserAccountLockedOut| Händelsen indikerar att vi försökte tooreset eller ändra ett lösenord för ett konto som var utelåst lokalt. Utelåsningar kan inträffa när en användare har försökt en ändring eller återställning av lösenord för många gånger under en kort tid. Lås upp hello-konto och försök igen om hello.|
| 33007| ADUserIncorrectPassword| Den här händelsen indikerar hello användaren angav ett felaktigt aktuella lösenord när du utför ett lösenord ska kunna ändras. Ange hello aktuella lösenordet och försök igen.|
| 33008| ADPasswordPolicyError| Den här händelsen inträffar när hello tillbakaskrivning av lösenord tjänsten försöker tooset ett lösenord på din lokala katalog som inte uppfyller hello lösenordets ålder, historik, komplexitet eller filtrera kraven i hello domän. <br> <br> Om du har en minsta lösenord ålder och nyligen har ändrat hello lösenordet i fönstret tid kan du inte är kan toochange hello lösenord igen tills hello angetts ålder i din domän. Minimiålder ska ställas in too0 i testsyfte kan. <br> <br> Om du har tidigare lösenordskrav aktiverad och du måste välja ett lösenord som inte har använts i hello sista N gånger, där N är hello lösenordshistorik inställningen. Om du väljer ett lösenord som har använts i hello sista N gånger, sedan visas ett fel i det här fallet. Historik ska ställas in too0 i testsyfte kan. <br> <br> Om du har kraven på lösenordskomplexitet alla tillämpas när hello användare försöker toochange eller återställa ett lösenord. <br> <br> Om du har aktiverat lösenordsfilter och en användare väljer ett lösenord som inte uppfyller hello filtreringskriterier, hello Återställ eller ändra åtgärden misslyckas.|
| 33009| ADConfigurationError| Den här händelsen indikerar ett problem uppstod skriva ett lösenord tillbaka tooyour lokal katalog på grund av konfigurationsproblem för tooa med Active Directory. Kontrollera hello Azure AD Connect datorn programloggen för meddelanden från hello ADSync-tjänsten för mer information om vilka fel uppstod.|


## <a name="troubleshoot-password-writeback-connectivity"></a>Felsöka anslutningsbarheten för tillbakaskrivning av lösenord

Om det uppstår avbrott i tjänsten med hello tillbakaskrivning av lösenord komponent av Azure AD Connect följer här några snabba steg du kan vidta tooresolve detta:

* [Starta om hello Azure AD Connect-synkroniseringstjänsten](#restart-the-azure-ad-connect-sync-service)
* [Inaktivera och återaktivera hello tillbakaskrivning av lösenord funktion](#disable-and-re-enable-the-password-writeback-feature)
* [Installera hello senaste Azure AD Connect-versionen](#install-the-latest-azure-ad-connect-release)
* [Felsök tillbakaskrivning av lösenord](#troubleshoot-password-writeback)

I allmänhet rekommenderar vi att du kör dessa steg i hello ordning ovanför toorecover din tjänst hello snabbaste sätt.

### <a name="restart-hello-azure-ad-connect-sync-service"></a>Starta om hello Azure AD Connect-synkroniseringstjänsten

Starta om hello hjälper Azure AD Connect-synkroniseringstjänsten tooresolve problem med nätverksanslutningen eller andra tillfälliga problem med hello-tjänsten.

1. Som administratör, klickar du på **starta** på hello-server som kör **Azure AD Connect**.
2. Typen **”services.msc”** i hello sökrutan och tryck på **RETUR**.
3. Leta efter hello **Microsoft Azure AD Sync** post.
4. Högerklicka på hello service post, klicka på **starta om**, och vänta tills hello åtgärden toocomplete.

   ![Starta om hello Azure AD Sync-tjänsten][Service Restart]

De här stegen återupprätta anslutningen med hello-Molntjänsten och lös eventuella avbrott som du kan ha problem. Om omstart hello synkroniseringstjänsten inte löser problemet, rekommenderar vi att du försöker toodisable och aktivera funktionen för hello tillbakaskrivning av lösenord som ett nästa steg.

### <a name="disable-and-re-enable-hello-password-writeback-feature"></a>Inaktivera och återaktivera hello tillbakaskrivning av lösenord funktion

Inaktivera och återaktivera hello tillbakaskrivning av lösenord hjälper funktionen tooresolve anslutningsproblem.

1. Som administratör, öppna hello **Azure AD Connect-konfigurationsguiden**.
2. På hello **ansluta tooAzure AD** dialogrutan, ange din **autentiseringsuppgifter för global administratör för Azure AD**
3. På hello **ansluta tooAD DS** dialogrutan, ange din **AD DS-administratörsautentiseringsuppgifter**.
4. På hello **identifiera användarna unikt** dialogrutan klickar du på hello **nästa** knappen.
5. På hello **valfria funktioner** dialogrutan avmarkera hello **tillbakaskrivning av lösenord** kryssrutan.
6. Klicka på **nästa** via hello återstående dialogsidorna utan att ändra något förrän du får toohello **klar tooconfigure** sidan.
7. Se till att hello konfigurera visar hello **tillbakaskrivning lösenordsalternativet som inaktiverade** och klicka sedan på hello grön **konfigurera** knappen toocommit ändringarna.
8. På hello **avslutad** dialogrutan avmarkera hello **synkronisera nu** alternativ och klickar sedan på **Slutför** tooclose hello guiden.
9. Öppna hello **Azure AD Connect-konfigurationsguiden**.
10. **Upprepa steg 2 – 8**, förutom att se till att du **Kontrollera hello lösenordsalternativet tillbakaskrivning** på hello **valfria funktioner** skärmen toore aktivera hello-tjänsten.

De här stegen återupprätta anslutningen med vår tjänst i molnet och Lös avbrott som du kan ha problem.

Inaktivera och återaktivera hello tillbakaskrivning av lösenord funktionen inte löser problemet kan rekommenderar vi att du försöker tooreinstall Azure AD Connect som ett nästa steg.

### <a name="install-hello-latest-azure-ad-connect-release"></a>Installera hello senaste Azure AD Connect-versionen

Installera Azure AD Connect kan lösa konfiguration och problem med nätverksanslutningen mellan våra molntjänster och din lokala AD-miljö.

Vi rekommenderar du utför det här steget endast efter en hello två första stegen som beskrivs ovan.

> [!WARNING]
> Om du har anpassat hello out-of-box sync regler **säkerhetskopiera dessa innan du fortsätter med uppgraderingen och distribuera dem manuellt när du är klar**.

1. Hämta hello senaste versionen av Azure AD Connect från hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
2. Eftersom du redan har installerat Azure AD Connect måste tooperform en uppgradering på plats tooupdate din Azure AD Connect-installationen toohello senaste versionen.
3. Kör hello hämtade paketet och följ hello på skärmen instruktioner tooupdate Azure AD Connect-datorn.

De här stegen återupprätta anslutningen med vår tjänst i molnet och lös eventuella avbrott som du kan ha problem.

Om hello senaste versionen av hello Azure AD Connect-servern inte löser problemet rekommenderar vi att du försöker inaktivera och nytt aktivera tillbakaskrivning av lösenord som ett sista steg när du har installerat hello senaste versionen.

## <a name="verify-whether-azure-ad-connect-has-hello-required-permission-for-password-writeback"></a>Kontrollera om Azure AD Connect har hello krävs behörighet för tillbakaskrivning av lösenord 
Azure AD Connect kräver AD **Återställ lösenord** behörighet tooperform tillbakaskrivning av lösenord. toofind reda på om Azure AD Connect har hello behörigheten för en given lokala AD-användarkonto, du kan använda hello gällande behörigheter för Windows-funktionen:

1. Logga in tooAzure AD Connect-servern och starta hello **Synchronization Service Manager** (Start → synkroniseringstjänsten).
2. Under hello **kopplingar** väljer hello lokala **AD-koppling** och på **egenskaper**.  
![Gällande behörigheter – steg 2](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
3. Välj hello i hello popup-fönstret, **ansluta tooActive Directory-skog** och notera ned hello **användarnamn** egenskapen. Detta är hello AD DS-konto som används av Azure AD Connect tooperform katalogsynkronisering. För tillbakaskrivning av Azure AD Connect tooperform lösenord, måste hello AD DS-konto ha behörighet för Återställ lösenord.  
![Gällande behörigheter - steg 3](./media/active-directory-passwords-troubleshoot/checkpermission02.png)  
4. Logga in tooan lokalt Domain Controller och starta hello **Active Directory-användare och datorer** program.
5. Klicka på **visa** och se till att **avancerade funktioner** är aktiverat.  
![Gällande behörigheter – steg 5](./media/active-directory-passwords-troubleshoot/checkpermission03.png)  
6. Leta efter hello AD-användarkonto som du vill tooverify. Högerklicka på hello-konto och välj **egenskaper**.  
![Gällande behörigheter - steg 6](./media/active-directory-passwords-troubleshoot/checkpermission04.png)  
7. Hello popup-fönstret, Välj toohello **säkerhet** och klicka på **Avancerat**.  
![Gällande behörigheter - steg 7](./media/active-directory-passwords-troubleshoot/checkpermission05.png)  
8. Hello avancerade säkerhetsinställningar popup-fönstret, Välj toohello **effektiv åtkomst** fliken.
9. Klicka på **väljer en användare** och välj hello AD DS-konto som används av Azure AD Connect (se steg 3). Klicka på **visa effektiv åtkomst**.  
![Gällande behörigheter - steg 9](./media/active-directory-passwords-troubleshoot/checkpermission06.png)  
10. Bläddra nedåt och leta efter **Återställ lösenord**. Om posten Hej kontrolleras innebär det att hello AD DS-konto har behörighet tooreset hello lösenordet för hello valt AD-användarkontot.  
![Gällande behörigheter - steg 10](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure AD-forum

Om du har en allmän fråga om Azure AD och lösenordsåterställning via självbetjäning, du kan be hello community för att få hjälp på hello [Azure AD-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Medlemmar i communityn hello innehåller tekniker, projektledare, MVP och andra IT-proffs.

## <a name="contact-microsoft-support"></a>Kontakta Microsoft support

Om du inte hittar hello svaret tooan problemet är vårt supportteam alltid tillgänglig tooassist du ytterligare.

tooproperly hjälpa, ber vi ange så mycket information som möjligt när du öppnar ett ärende inklusive:

* **Allmän beskrivning av hello fel** -vad är hello fel? Vad hette hello beteende som upptäcktes? Hur kan vi återskapa hello fel? Ange så mycket information som möjligt.
* **Sidan** -vilken sida har du på när du har upptäckt hello fel? Ange hello URL Om du kan tooand en skärmbild.
* **Stöd för koden** -vad var hello stöd kod som genereras när användaren hello såg hello fel? 
    * toofind detta återge hello fel, klicka på hello stöder kod länken längst ned hello hello-skärmen och skicka hello stöd tekniker hello GUID som är ett resultat.
    ![Hitta hello stöd kod längst ned hello hello-skärmen][Support Code]
    * Om du är på en sida utan en supportkod längst ned hello trycka på F12 och Sök efter SID och CID och skicka dessa två resultaten toohello supportteknikern.
* **Datum, tid och tidszon** -inkludera hello exakt datum och tid **med hello tidszonen** att hello fel har inträffat.
* **Användar-ID** -som var hello användare såg hello fel? (user@contoso.com)
    * Är detta en federerad användare?
    * Är detta en lösenord hash-synkroniserade användare?
    * Är detta en enda användaren i molnet?
* **Licensiering** -har hello användaren har tilldelats en Azure AD Premium eller Azure AD Basic licens?
* **Programhändelseloggen** - om du använder tillbakaskrivning av lösenord och hello felet finns i du lokal infrastruktur, är en komprimerad kopia av din programhändelseloggen från hello Azure AD Connect-servern när du kontaktar supporten.

    

[Service Restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Starta om hello Azure AD Sync-tjänsten"
[Support Code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Stöd för kod finns på hello längst till höger på hello fönster"

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
