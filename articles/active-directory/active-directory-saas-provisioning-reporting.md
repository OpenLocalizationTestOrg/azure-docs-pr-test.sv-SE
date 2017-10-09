---
title: "Rapportering av Azure Active Directory automatisk användarkonto etablering tooSaaS program | Microsoft Docs"
description: "Lär dig hur toocheck hello status för automatisk användarkonto Etableringsjobb och hur tootroubleshoot hello etablering av enskilda användare."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: asmalser-msft
ms.openlocfilehash: 5dcf9e5dbaacf3a2c81183c5d81e331858671b86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-reporting-on-automatic-user-account-provisioning"></a>Självstudier: Rapportering om etablering av en automatisk användar-konto


Azure Active Directory innehåller en [användarkontot etableras](active-directory-saas-app-provisioning.md) som hjälper till att automatisera hello etablering Frigör etablering av användarkonton i SaaS-appar och andra system för hello syftet slutpunkt till slutpunkt identitet livscykel hantering. Azure AD stöder förintegrerade användaretablering kopplingar för alla hello program och system i hello ”aktuell” avsnittet av hello [Azure AD application gallery](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured).

Den här artikeln beskriver hur toocheck hello status för etablering av jobb när de har lagts upp och hur tootroubleshoot hello etablering av enskilda användare och grupper.

## <a name="overview"></a>Översikt

Etablering kopplingar är främst ställa in och konfigurerats för att använda hello [Azure-hanteringsportalen](https://portal.azure.com), med följande hello [tillhandahålls dokumentationen](active-directory-saas-tutorial-list.md) för hello program där användarkontot etablering önskas. När konfigurerad och igång, kan Etableringsjobb för ett program rapporteras på ett av två sätt:

* **Azure-hanteringsportalen** -den här artikeln beskriver främst hämtar rapportinformation från hello [Azure-hanteringsportalen](https://portal.azure.com), som innehåller både en etablering sammanfattningsrapport samt detaljerad etablering granska loggarna för ett visst program.

* **Granska API** -Azure Active Directory innehåller också en granskningslogg API granskningsloggar aktiverar programmässiga hämtning av hello detaljerad etablering. Se [Azure Active Directory audit API-referens](active-directory-reporting-api-audit-reference.md) för dokumentationen specifika toousing detta API. När den här artikeln specifikt inte omfattar hur toouse hello API, den detalj hello typer av händelser som registreras i granskningsloggen hello-etablering.

### <a name="definitions"></a>Definitioner

Den här artikeln använder hello följande villkor som anges nedan:

* **Datakällan System** -hello-databas med användare som hello Azure AD-etablering tjänsten synkroniseras från. Azure Active Directory är hello källsystemet för hello merparten av förintegrerade etablering kopplingar, men det finns vissa undantag (exempel: Workday inkommande synkronisering).

* **Rikta System** -hello-databas med användare som hello etablering Azure AD-tjänsten har synkroniserats. Detta är vanligtvis ett SaaS-program (exempel: Salesforce, ServiceNow, Google Apps, Dropbox för företag), men i vissa fall kan vara ett lokalt system, till exempel Active Directory (exempel: Workday inkommande synkronisering tooActive Directory).


## <a name="getting-provisioning-reports-from-hello-azure-management-portal"></a>Hämta etablering rapporter från hello Azure-hanteringsportalen

tooget etablering rapportinformation för ett visst program startas genom att starta hello [Azure-hanteringsportalen](https://portal.azure.com) och bläddra toohello företagsprogram som etablering har konfigurerats. Du etablerar användare tooLinkedIn utöka hello navigering sökvägen toohello programinformation är:

**Azure Active Directory > företagsprogram > alla program > LinkedIn höjer**

Härifrån kan du har åtkomst till både hello etablering sammanfattningsrapport och hello etablering granskningsloggar, både beskrivs nedan.


### <a name="provisioning-summary-report"></a>Etablering sammanfattningsrapport

hello etablering sammanfattningsrapport är synlig i hello **etablering** fliken för det angivna programmet. Den finns under hello synkroniseringsinformation under **inställningar**, och ger hello följande information:

* Hej totala antalet användare och grupper som har synkroniserats och finns för närvarande i omfånget för etablering mellan hello källsystemet och hello målsystemet.

* hello senaste hello tidssynkronisering kördes. Synkronisering sker vanligtvis var 20 – 40 minuter efter en fullständig synkronisering har slutförts.

* Huruvida en första fullständig synkronisering har slutförts.

* Huruvida hello etableringsprocessen har placerats i karantän och vilka hello orsak hello karantän status är t.ex. (fel toocommunicate med målsystemet på grund av tooinvalid administratörsautentiseringsuppgifter)

hello etablering sammanfattningsrapport ska hello första plats administratörer utseende toocheck på hello driftstatusen för hello Etableringsjobbet.

 ![Översiktsrapport](./media/active-directory-saas-provisioning-reporting/summary_report.PNG)

### <a name="provisioning-audit-logs"></a>Etablering granskningsloggar
Alla aktiviteter som utförs av hello Etablerar tjänsten registreras i granskningsloggarna hello Azure AD, som kan visas i hello **granskningsloggar** fliken under hello **Kontoetablering** kategori. Loggade händelsen aktivitetstyper inkluderar:

* **Importera händelser** -en ”import” händelse registreras varje gång etablering hello Azure AD-tjänsten hämtar information om en enskild användare eller grupp från källsystemet eller målsystemet. Under synkroniseringen hämtas användare från källsystemet hello först med hello resultat som är registrerade som ”importera” händelser. hello matchande ID hello hämta användare tillfrågas sedan mot hello mål system toocheck om de finns med hello resultat som också registreras som ”importera” händelser. Dessa händelser registrera alla mappade användarattribut och deras värden som visades när hello hello händelse av etablering hello Azure AD-tjänsten. 

* **Synkronisering regeln händelser** - händelserna rapportera hello resultaten av hello attributet mappningsregler och eventuella konfigurerade målgrupp filter när informationen har importerats och utvärderas från hello käll- och system. Till exempel om en användare i ett källsystem anses toobe i omfånget för etablering och anses vara toonot finns i målsystemet hello, kommer sedan denna händelse registrerar att hello användaren att tillhandahållas i hello målsystemet. 

* **Exportera händelser** -en ”export” händelse registreras varje gång hello Azure AD-etablering tjänsten skriver en användare konto eller en grupp objekt tooa målsystemet. Dessa händelser registrera alla användarattribut och deras värden som har skrivits av hello Azure AD-etablering tjänsten när hello hello-händelse. Om ett fel uppstod vid skrivning till hello användarens konto eller en grupp objekt toohello målsystemet, kommer den att visas här.

* **Bearbeta escrow händelser** -processen escrows inträffa när hello etableras påträffar ett fel när en åtgärd och börjar tooretry hello åtgärden på ett intervall som inte tid. En ”escrow” händelse registreras varje gång en etableringsåtgärden har dragits tillbaka.

När du tittar på etablering händelser för en enskild användare inträffar normalt hello händelser i den här ordningen:

1. Importera händelse: användaren har hämtats från källsystemet hello.

2. Importera händelse: målsystemet är efterfrågade toocheck hello befintliga hello hämta användare.

3. Regeln synkroniseringshändelsen: användardata från käll- och system utvärderas mot hello konfigurerats attributet matchningsregler och scope filter toodetermine vad, eventuella ska utföras.

4. Exportera händelse: om hello synkroniseringshändelsen regel anges att åtgärden ska utföras (t.ex. Lägg till, uppdatera, ta bort), och sedan hello resultaten av hello åtgärden registreras i en Export-händelse.

![Skapa en testanvändare i Azure AD](./media/active-directory-saas-provisioning-reporting/audit_logs.PNG)


### <a name="looking-up-provisioning-events-for-a-specific-user"></a>Leta upp etablering händelser för en viss användare

hello är vanligaste för hello etablering granskningsloggar användningsfall toocheck hello Etableringsstatus för ett användarkonto för enskilda. toolook hello senaste etablering händelser för en specifik användare:

1. Gå toohello **granskningsloggar** avsnitt.

2. Från hello **kategori** väljer du **Kontoetablering**.

3. I hello **datumintervall** -menyn, Välj hello datumintervall toosearch,

4. I hello **Sök** menyraden, ange hello användar-ID för hello-användare som du vill toosearch för. hello format för ID-värde ska matcha det du valt som hello primära matchande ID: T i hello attributet mappningskonfigurationen (t.ex. userPrincipalName eller medarbetare ID-nummer). hello-ID-värde krävs visas i hello Target(s) kolumn.

5. Tryck på RETUR toosearch. hello senaste etablering händelser returneras först.

6. Om händelser returneras Observera hello aktivitetstyper och om de lyckades eller misslyckades. Om inga resultat returneras sedan innebär hello användaren antingen finns inte eller har ännu inte identifierats av hello etableringsprocessen om en fullständig synkronisering inte har slutförts ännu.

7. Klicka på enskilda händelser tooview utökad information, inklusive alla egenskaper för användare som har hämtats, utvärderas eller skrivs som en del av hello-händelse.


### <a name="tips-for-viewing-hello-provisioning-audit-logs"></a>Tips för att visa hello etablering granskningsloggar

För bästa läsbarhet i hello Azure portal väljer hello **kolumner** knappen och välj dessa kolumner:

* **Datum** -visar hello datum hello händelsen inträffade.
* **Target(s)** -visar hello app och användar-ID som är hello ämnen hello-händelse.
* **Aktiviteten** -hello aktivitetstyp, enligt beskrivningen ovan.
* **Status för** – oavsett om hello händelsen lyckades eller inte.
* **Statusanledning** -en sammanfattning av vad som hände i hello etablering händelse.


## <a name="troubleshooting"></a>Felsökning

hello etablering sammanfattning rapport och granska loggarna spela en viktig roll som hjälper administratörer felsöka olika användarkonto etablering problem.

Scenariobaserade vägledning om hur tootroubleshoot automatisk användaretablering, se [problem med att konfigurera och etablera användare tooan programmet](active-directory-application-provisioning-content-map.md).


## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användare konto-etablering för företag-appar](active-directory-enterprise-apps-manage-provisioning.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
