---
title: "aaa Kom igång med Azure Automation | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över Azure Automation-tjänsten genom att granska hello design och implementeringslösning information i förberedelse tooonboard hello erbjudande från Azure Marketplace."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 434e8ea28c55ff9bda1d2e46a7a6b8378a3baa0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-automation"></a>Komma igång med Azure Automation

Den här Kom igång-guide beskriver grundläggande begrepp relaterade toohello distribution av Azure Automation. Om du är ny tooAutomation i Azure eller har erfarenhet av automation arbetsflödet program som System Center Orchestrator kan den här guiden hjälper dig att förstå hur tooprepare och publicera automatisering.  Därefter kan förbereds du toobegin utveckla runbooks för att stödja dina behov för automatisering av processen. 


## <a name="automation-architecture-overview"></a>Översikt över arkitekturen i Automation

![Översikt över Azure Automation](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Automation är en programvara som en tjänst (SaaS) som ger en skalbar och tillförlitlig, flera innehavare miljö tooautomate bearbetar med runbooks och hantera konfigurationen ändringar tooWindows och Linux-system med hjälp av Desired State Configuration (DSC) i Azure, andra molntjänster eller lokalt. Enheter i ditt Automation-konto, till exempel runbooks, tillgångar och Kör som-konton är isolerade från andra Automation-konton i din prenumeration och andra prenumerationer.  

De runbooks som du kör i Azure körs i Automation-sandboxar på virtuella Azure PaaS-datorer (plattform som en tjänst).  Automation-sandboxar ger innehavare isolering för alla aspekter av runbook-körningen – moduler, lagring, minne, nätverkskommunikation, jobbströmmar m.m. Den här rollen hanteras av hello-tjänsten och kan inte nås från Azure eller Azure Automation-konto för du toocontrol.         

tooautomate hello distribution och hantering av resurser i ditt lokala datacenter eller andra molntjänster, när du har skapat ett Automation-konto kan du ange en eller flera datorer toorun hello [Hybrid Runbook Worker (HRW)](automation-hybrid-runbook-worker.md) roll.  Varje HRW kräver hello Microsoft-hanteringsagenten med en anslutning tooa logganalys-arbetsytan och ett Automation-konto.  Logganalys är används toobootstrap hello installation, underhålla hello Microsoft-hanteringsagenten och övervaka hello funktionerna i hello HRW.  hello leverans av runbooks och hello instruktion toorun dem utförs av Azure Automation.

Du kan distribuera flera HRW tooprovide hög tillgänglighet för dina runbooks, laddas saldo runbook-jobb, och i vissa fall dedikerar dem för specifika arbetsbelastningar eller miljöer.  hello Microsoft Monitoring Agent på hello HRW initierar kommunikation med hello Automation-tjänsten via TCP-port 443 och det finns inga brandväggskrav för inkommande.  När du har runbook som körs på en HRW i hello-miljön, och du vill att runbook hello tooperform hanteringsuppgifter mot andra datorer eller tjänster i den miljön, kan det behöver vara andra portar hello runbook åtkomst till.  Om din IT-säkerhetsprinciper inte tillåter datorer på ditt nätverk tooconnect toohello Internet, granska hello artikel [OMS Gateway](../log-analytics/log-analytics-oms-gateway.md), som fungerar som proxy för hello HRW toocollect jobbstatusen och ta emot konfigurationsinformation från Automation-konto.

Runbooks som körs på en HRW kör hello gäller hello lokala systemkontot på hello datorn rekommenderas som hello säkerhetskontext när du utför administrativa åtgärder på hello lokala Windows-dator. Om du vill hello runbook toorun åtgärder mot resurser utanför hello lokal dator kan behöva toodefine säker inloggningstillgångar i hello Automation-konto som du kan komma åt från hello runbook och använda tooauthenticate med hello extern resurs. Du kan använda [autentiseringsuppgifter](automation-credentials.md), [certifikat](automation-certificates.md), och [anslutning](automation-connections.md) tillgångar i din runbook med cmdlet: ar som gör att du toospecify autentiseringsuppgifter så att du kan autentisera dem.

Lagras i Azure Automation DSC-konfigurationer kan vara direkt tooAzure virtuella datorer. Andra fysiska och virtuella datorer kan begära konfigurationer från hello hämtningsservern för Azure Automation DSC.  För att hantera konfigurationer av din lokala fysiska eller virtuella Windows- och Linux-system, behöver du inte toodeploy någon infrastruktur toosupport hello hämtningsservern Automation DSC, endast utgående Internetåtkomst från varje system toobe som hanteras av Automation DSC , kommunicerar via TCP-port 443 toohello OMS-tjänsten.   

## <a name="prerequisites"></a>Krav

### <a name="automation-dsc"></a>Automation DSC
Azure Automation DSC kan vara används toomanage olika datorer:

* Virtuella Azure-datorer (klassisk) som kör Windows eller Linux
* Virtuella Azure-datorer som kör Windows eller Linux
* Virtuella AWS-datorer (Amazon Web Services) som kör Windows eller Linux
* Fysiska/virtuella Windows-datorer som körs lokalt, eller i ett annat moln än Azure eller AWS
* Fysiska/virtuella Linux-datorer som körs lokalt, eller i ett annat moln än Azure eller AWS

hello senaste versionen av WMF 5 måste installeras för hello PowerShell DSC-agent för Windows toobe kan toocommunicate med Azure Automation. hello senaste versionen av hello [PowerShell DSC-agent för Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150) måste vara installerad för Linux toobe kan toocommunicate med Azure Automation.

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker  
När du ställer in en dator toorun hybrid runbook jobb, måste datorn ha hello följande:

* Windows Server 2012 eller senare
* Windows PowerShell 4.0 eller senare.  Vi rekommenderar att du installerar Windows PowerShell 5.0 på hello datorn för ökad tillförlitlighet. Du kan hämta ny hello-version från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=50395)
* Minst två kärnor
* Minst 4 GB RAM-minne

### <a name="permissions-required-toocreate-automation-account"></a>Behörigheter som krävs toocreate Automation-konto
toocreate eller uppdatera ett Automation-konto måste du ha följande specifika privilegier hello och behörigheter krävs toocomplete det här avsnittet.   
 
* I ordning toocreate ett Automation-konto, din AD-användarkontot måste toobe tillagda tooa roll med behörigheter motsvarande toohello ägarrollen för Microsoft.Automation resurser som beskrivs i artikel [rollbaserad åtkomstkontroll i Azure Automation ](automation-role-based-access-control.md).  
* Om hello App registreringar inställning har angetts för**Ja**, icke-administratörer i din Azure AD-klient kan [registrera AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Om hello app registreringar inställning har angetts för**nr**, hello-användaren som utför den här åtgärden måste vara en global administratör i Azure AD. 

Om du inte är medlem i Active Directory-instans för hello prenumeration innan du läggs toohello global administratör/co-administrator roll hello prenumeration, läggs tooActive Directory som en gäst. I så fall kan få du en ”du har inte behörighet toocreate...” varning för hello **lägga till Automation-konto** bladet. Användare som har lagts till toohello global administratör/co-administrator rollen först kan tas bort från hello prenumeration Active Directory-instans och toomake lagts till igen dem en fullständig användare i Active Directory. tooverify i den här situationen från hello **Azure Active Directory** rutan i hello Azure portal, Välj **användare och grupper**väljer **alla användare** och, när du har valt hello specifik användare, markerar du **profil**. Hej värdet för hello **användartyp** attributet under hello användare profil ska inte vara lika med **gäst**.

## <a name="authentication-planning"></a>Planera för autentisering
Azure Automation kan du tooautomate åtgärder mot resurser i Azure, lokalt, och med andra molntjänstleverantörer.  För en runbook-tooperform åtgärderna som krävs, måste den ha behörigheter toosecurely åtkomst hello resurser med hello lägsta möjliga rättigheter som krävs inom hello prenumeration.  

### <a name="what-is-an-automation-account"></a>Vad är ett Automation-konto? 
Alla hello automation utföra åtgärder mot resurser med hjälp av hello Azure-cmdlets i Azure Automation autentisera tooAzure med hjälp av Azure Active Directory organisationens identitet credential-baserad autentisering.  Ett Automation-konto är separat från hello kontot du använder toosign på toohello portal tooconfigure och Azure-resurser.  Automation-resurser som ingår i ett konto är hello följande:

* **Certifikat** – innehåller ett certifikat som används för autentisering från en runbook eller DSC-konfiguration eller lägger till dem.
* **Anslutningar** -innehåller autentiserings- och information som krävs för tooconnect tooan externa tjänster eller program från en runbook eller DSC-konfigurationen.
* **Autentiseringsuppgifter** -är ett PSCredential-objekt som innehåller säkerhetsreferenser, till exempel användarnamn och lösenord som krävs för tooauthenticate från en runbook eller DSC-konfigurationen.
* **Integreringsmoduler** -är PowerShell-moduler som ingår i en Azure Automation-konto toomake användning av cmdlets i runbooks och DSC-konfigurationer.
* **Scheman** – innehåller scheman som startar eller stoppar en runbook vid en angiven tidpunkt, även med återkommande frekvens.
* **Variabler** – innehåller värden som är tillgängliga från en runbook eller DSC-konfiguration.
* **DSC-konfigurationer** -är PowerShell-skript som beskriver hur tooconfigure en funktion i operativsystemet eller inställningen eller installera ett program på en Windows- eller Linux-dator.  
* **Runbooks** – är en uppsättning uppgifter som kör automatiserade processer i Azure Automation baserat på Windows PowerShell.    

hello Automation resurser för varje Automation-konto som är associerade med en enda Azure region, men Automation-konton kan hantera alla hello resurser i din prenumeration. Skapa Automation-konton i olika regioner om du har principer som kräver data och resurser toobe isolerade tooa specifik region.

> [!NOTE]
> Automation-konton och hello resurser som de innehåller som skapas i hello Azure-portalen, kan inte komma åt hello klassiska Azure-portalen. Om du vill toomanage dessa konton och sina resurser med Windows PowerShell, måste du använda hello Azure Resource Manager moduler.
> 

När du skapar ett Automation-konto i hello Azure-portalen kan skapa du automatiskt två entiteter för autentisering:

* Ett Kör som-konto. Det här kontot skapar ett tjänstobjekt i Azure Active Directory (AD Azure) och ett certifikat. Det ger också hello deltagare rollbaserad åtkomstkontroll (RBAC), som hanterar Resource Manager-resurser med hjälp av runbooks.
* Ett klassiskt Kör som-konto. Det här kontot Överför ett hanteringscertifikat som används toomanage klassiska resurser med hjälp av runbooks.

Rollbaserad åtkomstkontroll är tillgängligt med Azure Resource Manager toogrant tillåtna åtgärder tooan Azure AD-användarkontot och kör som-konto och autentisering som tjänstens huvudnamn.  Läs [rollbaserad åtkomstkontroll i Azure Automation-artikel](automation-role-based-access-control.md) mer toohelp utveckla din modell för att hantera behörigheter för automatisering.  

#### <a name="authentication-methods"></a>Autentiseringsmetoder
hello sammanfattas följande tabell hello olika autentiseringsmetoder för varje miljö som stöds av Azure Automation.

| Metod | Miljö 
| --- | --- | 
| Azure Kör som-konto och klassiskt Kör som-konto |Azure Resource Manager och klassisk Azure-distribution |  
| Azure AD-användarkonto |Azure Resource Manager och klassisk Azure-distribution |  
| Windows-autentisering |Lokala Datacenter eller andra molntjänstleverantör med hello Hybrid Runbook Worker |  
| AWS-autentiseringsuppgifter |Amazon Web Services |  

Under hello **hur to\Authentication och säkerhet** avsnittet, stöder artiklar som ger översikt över och de implementeringssteg tooconfigure autentisering för dessa miljöer, antingen med en befintlig eller ny konto du specifikt för den miljön.  För hello Azure kör som och klassiska kör som-konto, hello avsnittet [uppdatering Automation kör som-konto](automation-create-runas-account.md) beskriver hur tooupdate befintligt Automation-konto med hello kör som-konton från hello-portalen eller med hjälp av PowerShell om den inte har ursprungligen konfigurerats med en Kör som- eller klassiska kör som-konto. Om du vill toocreate kör som och klassiska kör som-konto med ett certifikat som utfärdats av en företagscertifikatutfärdare (CA), granska den här artikeln toolearn hur toocreate hello användarkonton med den här konfigurationen.     
 
## <a name="network-planning"></a>Planera för nätverk
För hello Hybrid Runbook Worker tooconnect tooand registrera med Microsoft Operations Management Suite (OMS), måste den ha åtkomst toohello portnummer och hello URL: er som beskrivs nedan.  Det är dessutom toohello [portar och URL: er som krävs för hello Microsoft Monitoring Agent](../log-analytics/log-analytics-windows-agents.md#network) tooconnect tooOMS. Om du använder en proxyserver för kommunikation mellan hello agent och hello OMS-tjänsten måste tooensure att hello lämpliga resurser är tillgängliga. Om du använder en brandvägg toorestrict åtkomst toohello Internet måste tooconfigure din brandvägg toopermit åtkomst.

hello informationen under listan hello port och URL: er som krävs för hello Hybrid Runbook Worker toocommunicate med automatisering.

* Port: Endast TCP 443 krävs för utgående Internet-åtkomst
* Global URL:  *.azure-automation.net

Om du har ett Automation-konto som har definierats för en viss region och du vill toorestrict kommunikation med regionala datacenter, hello följande tabell innehåller hello DNS-post för varje region.

| **Region** | **DNS-post** |
| --- | --- |
| Södra centrala USA |scus-jobruntimedata-prod-su1.azure-automation.net |
| Östra USA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Västra centrala USA | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Västra Europa |we-jobruntimedata-prod-su1.azure-automation.net |
| Norra Europa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Centrala Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Sydostasien |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indien, centrala |cid-jobruntimedata-prod-su1.azure-automation.net |
| Östra Japan |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Sydöstra Australien |ase-jobruntimedata-prod-su1.azure-automation.net |
| Storbritannien, södra | uks-jobruntimedata-prod-su1.azure-automation.net |
| Virginia (USA-förvaltad region) | usge-jobruntimedata-prod-su1.azure-automation.us |

Hämta en lista över IP-adresser i stället för namn och granska hello [Azure Datacenter-IP-adress](https://www.microsoft.com/download/details.aspx?id=41653) XML-fil från hello Microsoft Download Center. 

> [!NOTE]
> Den här filen innehåller hello IP-adressintervall (inklusive adressintervall beräkning, SQL och lagring) används i hello Microsoft Azure-datacenter. En uppdaterad fil skickas varje vecka som visar hello som för närvarande har distribuerats intervall och eventuella kommande ändringar toohello IP-adressintervall. Nya adressintervall som visas i hello-filen används inte i hello Datacenter för minst en vecka. Kontrollera download hello nya xml filen varje vecka och utföra hello nödvändiga ändringar på webbplatsen toocorrectly identifiera tjänster som körs i Azure. Express Route-användare kan Observera att den här filen användas tooupdate hello BGP-annons Azure utrymme i hello första veckan i månaden. 
> 

## <a name="creating-an-automation-account"></a>Skapa ett Automation-konto

Det finns olika sätt som du kan skapa ett Automation-konto i hello Azure-portalen.  hello i den följande tabellen beskrivs varje typ av distribution och skillnader.  

|Metod | Beskrivning |
|-------|-------------|
| Välj kontrollen & Automation hello Marketplace | Ett erbjudande, vilket skapar en Automation-konto och ett OMS-arbetsytan länkade tooone i hello samma resursgrupp och region.  Integrering med OMS också innehåller hello fördelen med att använda Log Analytics toomonitor och analysera runbook-jobbet status och jobbstatus dataströmmar med tiden och använda avancerade funktioner tooescalate eller undersöka problem. hello erbjuder också distribuerar hello ändringsspårning & uppdateringshantering lösningar som är aktiverade som standard. |
| Välj Automation hello Marketplace | Skapar ett Automation-konto i en ny eller befintlig resursgrupp som inte är länkade tooan OMS-arbetsytan och innehåller inte några tillgängliga lösningar från hello Automation- och kontrollservern erbjudande. Detta är en grundläggande konfiguration som presenteras tooAutomation och kan hjälpa dig att lära dig hur toowrite runbooks, DSC-konfigurationer och Använd hello funktionerna i hello-tjänsten. |
| Valda hanteringslösningar | Om du väljer en lösning –  **[uppdateringshantering](../operations-management-suite/oms-solution-update-management.md)**,  **[Starta/stoppa virtuella datorer vid låg belastning](automation-solution-vm-management.md)**, eller  **[ Ändringsspårning](../log-analytics/log-analytics-change-tracking.md)**  de uppmaning OMS-arbetsytan och tooselect en befintlig Automation eller erbjuda du hello alternativet toocreate som krävs för hello lösning toobe distribuerats i din prenumeration. |

Det här avsnittet vägleder dig genom att skapa ett Automation-konto och OMS-arbetsytan med onboarding hello Automation- och kontrollservern erbjudande.  toocreate fristående Automation-konto för testning eller toopreview hello, granska hello följande artikel [skapa fristående Automation-konto](automation-create-standalone-account.md).  

### <a name="create-automation-account-integrated-with-oms"></a>Skapa Automation-konto integrerat med OMS
hello bör metoden tooonboard Automation är genom att välja hello Automation- och kontrollservern erbjudande hello Marketplace.  Detta skapar både ett Automation-konto och upprättar hello integrering med en OMS-arbetsyta, inklusive hello alternativet tooinstall hello hanteringslösningar som är tillgängliga med hello erbjudande.  

1. Logga in toohello Azure-portalen med ett konto som är medlem i rollen för hello Prenumerationsadministratörer och medadministratör för hello prenumeration.

2. Klicka på **Ny**.<br><br> ![Välj alternativet Ny på Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. Sök efter **Automation** och sedan i hello sökresultat väljer **Automation- och kontrollservern***.<br><br> ![Sök efter och välj Automatisering och kontroll från Marketplace](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   

4. När du har läst hello beskrivning för hello erbjudande, klickar du på **skapa**.  

5. På hello **Automation- och kontrollservern** inställningar-bladet välj **OMS-arbetsytan**.  På hello **OMS arbetsytor** bladet Välj en OMS arbetsytan länkade toohello som tillhör samma Azure-prenumeration som hello Automation-konto eller skapa en OMS-arbetsyta.  Om du inte har en OMS-arbetsytan väljer **Skapa ny arbetsyta** och på hello **OMS-arbetsytan** bladet utföra hello följande: 
   - Ange ett namn för hello nya **OMS-arbetsytan**.
   - Välj en **prenumeration** toolink tooby att välja hello nedrullningsbara listan om hello standard valt inte är lämplig.
   - Du kan skapa en **resursgrupp** eller välja en befintlig resursgrupp.  
   - Välj en **Plats**.  Hello enda platser som är tillgängliga för närvarande **Australien sydost**, **östra USA**, **Sydostasien**, **Väst centrala oss**, och  **Västra Europa**.
   - Välj en **Prisnivå**.  hello lösningen erbjuds i två nivåer: Frigör och Per nod (OMS) tjänstnivån.  hello kostnadsfria nivån har en gräns på hello mängden data som samlas in varje dag, Bevarandeperiod och runbook-jobbet runtime minuter.  hello Per nod (OMS) nivån har inte en gräns på hello mängden data som samlas in varje dag.  
   - Välj **Automation-konto**.  Om du skapar en ny OMS-arbetsyta du krävs tooalso skapa ett Automation-konto som är kopplat till hello ny OMS-arbetsytan angavs tidigare, inklusive Azure-prenumeration, resursgrupp och region.  Du kan välja **skapa ett Automation-konto** och på hello **Automation-konto** bladet ange hello följande: 
  - I hello **namn** anger hello namnet på hello Automation-konto.

    Alla andra alternativ fylls i automatiskt baserat på valda hello OMS-arbetsytan och dessa alternativ kan inte ändras.  Ett Azure kör som-konto är hello standardmetoden för autentisering för hello erbjudande.  När du klickar på **OK**hello konfigurationsalternativ verifieras och hello Automation-kontot har skapats.  Du kan följa förloppet under **meddelanden** hello-menyn. 

    Annars väljer du ett befintligt Automation Kör som-konto.  hello-kontot som du väljer kan inte redan vara länkade tooanother OMS-arbetsyta, annars visas ett meddelande i hello-bladet.  Om den redan är länkad måste tooselect ett annat Automation kör som-konto eller skapa en.

    När du har slutfört hello information som krävs, klickar du på **skapa**.  hello information har verifierats och hello Automation-konto och kör som-konton skapas.  Du kommer tillbaka toohello **OMS-arbetsytan** bladet automatiskt.  

6. När du har angett hello krävs information på hello **OMS-arbetsytan** bladet, klickar du på **skapa**.  Hello information har verifierats och hello arbetsytan har skapats, du kan följa förloppet under **meddelanden** hello-menyn.  Du kommer tillbaka toohello **Lägg till lösning** bladet.  

7. På hello **Automation- och kontrollservern** inställningar-bladet bekräfta tooinstall hello rekommenderade förvald lösningar. Om du avmarkerar någon av dessa kan du installera dem individuellt senare.  

8. Klicka på **skapa** tooproceed med onboarding Automation och en OMS-arbetsyta. Alla inställningar verifieras och försöker sedan toodeploy hello erbjudandet i din prenumeration.  Den här processen kan ta flera sekunder toocomplete och du kan följa förloppet under **meddelanden** hello-menyn. 

När hello erbjudande har publicerats eller så kan du börja skapa runbooks, fungerar med hello hanteringslösningar som du har aktiverat kan distribuera en [Runbook worker-Hybrid](automation-hybrid-runbook-worker.md) roll eller börjar arbeta med [logganalys](https://docs.microsoft.com/azure/log-analytics) toocollect data som genereras av resurser i molnet eller lokala miljön.   

## <a name="next-steps"></a>Nästa steg
* Du kan bekräfta att det nya Automation-kontot kan autentisera mot Azure-resurser genom att granska [testa autentisering av Azure Automation Kör som-konto](automation-verify-runas-authentication.md).
* tooget igång med att skapa runbooks måste du först granska hello [runbook-automatiseringstyper](automation-runbook-types.md) som stöds och relaterade att tänka på innan du börjar redigera.


