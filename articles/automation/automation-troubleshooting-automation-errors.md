---
title: "aaaTroubleshooting vanliga Azure Automation utfärdar | Microsoft Docs"
description: "Den här artikeln innehåller information toohelp felsöka och lösa vanliga problem i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
tags: top-support-issue
keywords: "fel när Automation, felsökning, problemet"
ms.assetid: 5f3cfe61-70b0-4e9c-b892-d02daaeee07d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: sngun; v-reagie
ms.openlocfilehash: eb7d1cc5726f2b7a86c860e8f0c8340fa4221b2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-common-issues-in-azure-automation"></a>Felsökning av vanliga problem i Azure Automation 
Den här artikeln innehåller hjälp med att felsöka vanliga fel som du kan uppleva i Azure Automation och ger förslag på lösningar tooresolve dem.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>När du arbetar med Azure Automation runbook-autentiseringsfel
### <a name="scenario-sign-in-tooazure-account-failed"></a>Scenario: Logga in tooAzure konto misslyckades
**Fel:** felmeddelandet hello ”Unknown_user_type: Okänd typ” när du arbetar med hello Add-AzureAccount eller Login-AzureRmAccount cmdlet: ar.

**Orsak till hello-fel:** det här felet uppstår om hello autentiseringsuppgifter tillgångsinformation är inte giltigt eller om hello användarnamn och lösenord som du använde toosetup hello Automation-autentiseringsuppgiftstillgång inte är giltigt.

**Felsökningstips:** i order toodetermine fel, ta hello följande steg:  

1. Kontrollera att du inte har några specialtecken, inklusive hello  **@**  tecken i hello Automation Tillgångsnamn på autentiseringsuppgifter att du använder tooconnect tooAzure.  
2. Kontrollera att du kan använda hello användarnamn och lösenord som lagras i hello Azure Automation-autentiseringsuppgift i din lokala PowerShell ISE-redigeraren. Du kan göra detta genom att köra följande cmdlets i hello PowerShell ISE hello:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred
3. Det innebär att du inte har konfigurerat dina Azure Active Directory-autentiseringsuppgifter korrekt om autentiseringen misslyckas lokalt. Se för[autentisera med Azure Active Directory tooAzure](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blogg efter tooget hello Azure Active Directory-konto ställts in korrekt.  

### <a name="scenario-unable-toofind-hello-azure-subscription"></a>Scenario: Det gick inte toofind hello Azure-prenumeration
**Fel:** felmeddelandet hello ”Hej prenumeration med namnet ``<subscription name>`` går inte att hitta” när du arbetar med hello väljer AzureSubscription eller Select-AzureRmSubscription cmdlet: ar.

**Orsak till hello-fel:** det här felet uppstår om hello prenumerationen inte är giltigt eller om hello Azure Active Directory-användare som försöker tooget hello prenumerationsinformation inte är konfigurerad som administratör för hello prenumeration.

**Felsökningstips:** i ordning toodetermine om du har autentiserats på rätt sätt tooAzure och har åtkomst toohello prenumeration som du försöker tooselect, kan hello följande steg:  

1. Se till att du kör hello **Add-AzureAccount** innan du kör hello **Välj AzureSubscription** cmdlet.  
2. Om du fortfarande ser det här felmeddelandet ändra din kod genom att lägga till hello **Get-AzureSubscription** cmdlet efter hello **Add-AzureAccount** cmdlet och sedan köra hello kod.  Nu ska du kontrollera om hello utdata från Get-AzureSubscription innehåller information om din prenumeration.  

   * Om du inte ser någon prenumerationsinformation i hello utdata, innebär det att hello prenumerationen inte initierats ännu.  
   * Om du ser hello prenumerationsinformation i hello utdata, bekräfta att du använder hello korrekt prenumerationsnamn eller ID med hello **Välj AzureSubscription** cmdlet.   

### <a name="scenario-authentication-tooazure-failed-because-multi-factor-authentication-is-enabled"></a>Scenario: Autentisering tooAzure misslyckades eftersom multifaktorautentisering har aktiverats
**Fel:** felmeddelandet hello ”Lägg till-AzureAccount: AADSTS50079: registrering av stark autentisering (bevis upp) krävs” när du autentiserar tooAzure med ditt Azure användarnamn och lösenord.

**Orsak till hello-fel:** om du har multifaktorautentisering på ditt Azure-konto kan du inte använda en Azure Active Directory användare tooauthenticate tooAzure.  Du måste i stället toouse ett certifikat eller en service principal tooauthenticate tooAzure.

**Felsökningstips:** toouse ett certifikat med hello Azure Service Management-cmdlets finns för[skapa och lägga till ett certifikat toomanage Azure services.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) toouse ett huvudnamn för tjänsten med Azure Resource Manager cmdlets finns för[att skapa tjänstens huvudnamn med hjälp av Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) och [autentiserar ett huvudnamn för tjänsten med Azure Resource Manager.](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="common-errors-when-working-with-runbooks"></a>Vanliga fel när du arbetar med runbooks
### <a name="scenario-hello-runbook-job-start-was-attempted-three-times-but-it-failed-toostart-each-time"></a>Scenario: hello runbook jobbstart gjordes tre gånger, men den kunde inte toostart varje gång
**Fel:** din runbook misslyckas med fel hello ”” hello jobbet har försökt tre gånger, men misslyckades ”.

**Orsak till hello-fel:** det här felet kan orsakas av hello följande orsaker:  

1. Gränsen för minne.  Vi har dokumenterats begränsar hur mycket minne som allokerats tooa Sandbox [Automation tjänstbegränsningarna](../azure-subscription-service-limits.md#automation-limits) så att ett jobb kan misslyckas den om du använder mer än 400 MB minne. 

2. Inkompatibel modul.  Detta kan inträffa om modulen beroenden inte är rätt och om inte din runbook vanligtvis returnerar ”kommandot gick inte att hitta” eller ”det går inte att binda parametern” visas. 

**Felsökningstips:** någon av följande lösningar hello kommer att åtgärda problemet hello:  

* Föreslagna metoder toowork inom minnesgränsen för hello är toosplit hello arbetsbelastningen mellan flera runbooks, inte bearbeta så mycket data i minnet, inte toowrite onödiga utdata från dina runbooks, eller överväga hur många kontrollpunkter som du skriver i din PowerShell arbetsflöde för runbooks.  

* Du behöver tooupdate Azure modulerna genom att följa stegen i hello [hur tooupdate Azure PowerShell-moduler i Azure Automation](automation-update-azure-modules.md).  


### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scenario: Runbook misslyckas på grund av avserialiserat objekt
**Fel:** din runbook misslyckas med hello felet ”Det går inte att binda parametern ``<ParameterName>``. Det går inte att konvertera hello ``<ParameterType>`` värde av typen Deserialized ``<ParameterType>`` tootype ``<ParameterType>``”.

**Orsak till hello-fel:** om din runbook är ett PowerShell-arbetsflöde, lagrar komplexa objekt i ett avserialiserat format i ordning toopersist runbook-tillstånd om hello arbetsflödet pausas.  

**Felsökningstips:**  
Något av följande tre lösningar hello kommer att åtgärda problemet:

1. Om du skicka komplexa objekt från en cmdlet tooanother omsluta dessa cmdlets i ett InlineScript.  
2. Skicka hello namn eller värde som du behöver från hello komplexa objektet i stället för att skicka hello hela objektet.  
3. Använda en PowerShell-runbook i stället för en PowerShell-arbetsflödesrunbook.  

### <a name="scenario-runbook-job-failed-because-hello-allocated-quota-exceeded"></a>Scenario: Runbook-jobbet misslyckades eftersom hello allokerade kvoten överskreds
**Fel:** runbook-jobb misslyckas med fel hello ”hello kvoten för hello jobbkörningstid per månad totala har nåtts för den här prenumerationen”.

**Orsak till hello-fel:** detta fel uppstår när hello jobbkörningen överskrider hello 500 minut ledigt kvoten för ditt konto. Den här kvoten gäller tooall typer av jobb körning av uppgifter som testar ett jobb, starta ett jobb från hello-portalen, köra ett jobb med hjälp av webhooks och schemalägga ett jobb tooexecute med hjälp av antingen hello Azure-portalen eller i ditt datacenter. Mer om prissättningen för Automation Se toolearn [Automation priser](https://azure.microsoft.com/pricing/details/automation/).

**Felsökningstips:** om du vill toouse mer än 500 minuters bearbetning per månad som du behöver toochange din prenumeration från hello kostnadsfria nivån toohello grundläggande nivån. Du kan uppgradera toohello grundläggande nivån av tar hello följande steg:  

1. Logga in tooyour Azure-prenumeration  
2. Välj hello Automation-konto som du vill tooupgrade  
3. Klicka på **inställningar** > **prisnivå och användning** > **prisnivå**  
4. På hello **Välj din prisnivå** bladet väljer **grundläggande**    

### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scenario: Cmdlet som inte känns igen när du kör en runbook
**Fel:** runbook-jobb misslyckas med fel hello ”``<cmdlet name>``: hello termen ``<cmdlet name>`` känns inte igen som hello namnet för en cmdlet, funktion, skriptfilen eller körbart program”.

**Orsak till hello-fel:** felet orsakas när hello PowerShell motorn inte kan hitta hello-cmdlet som du använder i din runbook.  Detta kan bero på att hello-modul som innehåller hello cmdlet saknas i hello konto, det finns en konflikt med ett runbook-namn eller hello cmdlet finns också i en annan modul och Automation kan inte matcha hello namn.

**Felsökningstips:** någon av följande lösningar hello kommer att åtgärda problemet hello:  

* Kontrollera att du har angett rätt hello cmdlet-namn.  
* Kontrollera att hello cmdlet finns i ditt Automation-konto och att det inte finns några konflikter. tooverify om hello cmdlet finns, öppna en runbook i redigeringsläge och Sök efter hello cmdlet du vill toofind i hello-biblioteket eller köra **Get-Command ``<CommandName>``** .  När du har validerat hello cmdleten är tillgängliga toohello konto, och att det inte finns några namnet står i konflikt med andra cmdlets eller runbooks, lägga till den toohello arbetsytan och se till att du använder en giltig parameter som angetts i din runbook.  
* Om du har en namnkonflikt och hello är tillgänglig i två olika moduler, kan du lösa detta genom att använda hello fullständigt kvalificerade namn för hello cmdlet. Du kan till exempel använda **ModuleName\CmdletName**.  
* Om du kör hello runbook lokalt i en hybrid worker-gruppen och sedan kontrollera att hello är cmdlet och modulen installerad på hello-dator som är värd för hello worker-hybrid.

### <a name="scenario-a-long-running-runbook-consistently-fails-with-hello-exception-hello-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-hello-same-checkpoint"></a>Scenario: En tidskrävande runbook misslyckas konsekvent med hello undantag ”: hello jobbet kan inte fortsätta eftersom det upprepade gånger har avlägsnats från hello samma kontrollpunkt”.
**Orsak till hello-fel:** genom design beteende på grund av toohello ”fördelning” övervakning av processer i Azure Automation, som automatiskt inaktiverar en runbook om den körs längre än tre timmar. Dock hello felmeddelande ger inte ”härnäst” alternativ. En runbook kan avbrytas för ett antal orsaker. Pausar hända oftast på grund av tooerrors. Till exempel ett undantagsfel i en runbook, ett nätverksfel eller en krasch på hello Runbook Worker kör hello runbook, kommer alla orsaka hello runbook toobe avbruten och starta från den senaste kontrollpunkten när återupptas.

**Felsökningstips:** hello dokumenterade lösning tooavoid det här problemet är toouse kontrollpunkter i ett arbetsflöde.  toolearn mer finns för[Learning PowerShell-arbetsflöden](automation-powershell-workflow.md#checkpoints).  En mer utförlig förklaring av ”fördelning” och kontrollpunkt kan hittas i den här bloggen artikeln [med kontrollpunkter i Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Vanliga fel när du importerar moduler
### <a name="scenario-module-fails-tooimport-or-cmdlets-cant-be-executed-after-importing"></a>Scenario: Modulen inte tooimport eller cmdlets kan inte utföras efter import
**Fel:** en modul misslyckas tooimport eller importeras korrekt, men inga cmdletar extraheras.

**Orsak till hello-fel:** några vanliga orsaker att en modul inte importeras kanske har tooAzure Automation är:  

* hello struktur matchar inte strukturen för hello att Automation måste ha den toobe i.  
* hello-modulen är beroende av en annan modul som inte har distribuerat tooyour Automation-konto.  
* hello modulen saknar dess beroenden i hello mapp.  
* Hej **ny AzureRmAutomationModule** cmdlet håller på att använda tooupload hello modulen, och du inte har gett hello fullständig lagringssökväg eller inte har hämtats hello modulen med hjälp av en offentligt tillgänglig URL.  

**Felsökningstips:**  
Något av följande lösningar hello kommer att åtgärda problemet hello:  

* Kontrollera att modulen hello följer hello följande format:  
  ModuleName.Zip  **->**  ModuleName eller versionsnummer  **->**  (ModuleName.psm1, ModuleName.psd1)
* Öppna hello .psd1 filen och om hello modulen har några beroenden.  Om du överför dessa moduler toohello Automation-konto.  
* Kontrollera att alla refererade DLL-filer finns i hello modulen mapp.  

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Vanliga fel när du arbetar med önskad tillstånd Configuration DSC)
### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scenario: Noden är statusen misslyckades med felet ”kunde inte hittas”
**Fel:** hello-noden har en rapport med **misslyckades** status och som innehåller hello felet ”Hej försök tooget hello åtgärd från servern https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``) / GetDscAction misslyckades på grund av en giltig konfiguration ``<guid>`` kan inte hittas ”.

**Orsak till hello-fel:** det här felet uppstår vanligen när hello nod tilldelas tooa namn (t.ex. ABC) i stället för ett namn på konfigurationsnod (t.ex. ABC. Webbserver).  

**Felsökningstips:**  

* Kontrollera att du tilldelar hello nod med ”nodkonfigurationsnamnet” och inte hello ”configuration”.  
* Du kan tilldela en nod configuration tooa nod med hjälp av Azure portal eller med en PowerShell-cmdlet.

  * I order tooassign nod configuration tooa nod med hjälp av Azure portal, öppna hello **DSC-noder** bladet Välj en nod och klicka på **tilldela nodkonfiguration** knappen.  
  * I order tooassign nod configuration tooa nod med hjälp av PowerShell-cmdleten, Använd **Set AzureRmAutomationDscNode** cmdlet

### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scenario: Inga nodkonfigurationer (MOF-filer) skapas när en konfiguration kompileras
**Fel:** DSC-Kompileringsjobb pausar hello-fel: ”kompilering slutfördes, men ingen nod configuration .mofs genererades”.

**Orsak till hello-fel:** när hello uttryck efter hello **nod** nyckelord i hello DSC-konfiguration för utvärderar $null och inga nodkonfigurationer skapas.    

**Felsökningstips:**  
Något av följande lösningar hello kommer att åtgärda problemet hello:  

* Kontrollera att hello uttryck nästa toohello **nod** nyckelord i hello-konfigurationsdefinition utvärderas inte för$ null.  
* Om du skickar ConfigurationData vid kompilering av hello konfiguration, se till att du skickar hello förväntade värden hello konfigurationen kräver från [ConfigurationData](automation-dsc-compile.md#configurationdata).

### <a name="scenario--hello-dsc-node-report-becomes-stuck-in-progress-state"></a>Scenario: hello DSC-nod rapporten blir fastnat ”pågående”-tillstånd
**Fel:** DSC-agenten matar ut ”ingen instans hittades med den angivna egenskapsvärden”.

**Orsak till hello-fel:** du har uppgraderat WMF-version och ha skadad WMI.  

**Felsökningstips:** följer du anvisningarna hello i hello [DSC kända problem och begränsningar](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) toofix hello problemet.

### <a name="scenario--unable-toouse-a-credential-in-a-dsc-configuration"></a>Scenario: Det gick inte toouse autentiseringsuppgifter i en DSC-konfiguration
**Fel:** DSC-Kompileringsjobb pausades hello-fel ”: System.InvalidOperationException fel vid bearbetning av egenskapen Credential om du av typen ``<some resource name>``: konvertera och lagra ett krypterat lösenord som klartext tillåts endast om PSDscAllowPlainTextPassword anges tootrue ”.

**Orsak till hello-fel:** du har använt en autentiseringsuppgift i en konfiguration, men inte anger rätt **ConfigurationData** tooset **PSDscAllowPlainTextPassword** tootrue för varje nod konfiguration.  

**Felsökningstips:**  

* Gör att toopass i hello rätt **ConfigurationData** tooset **PSDscAllowPlainTextPassword** tootrue för varje nodkonfiguration som nämns i hello konfiguration. Mer information finns för[tillgångar i Azure Automation DSC](automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Nästa steg
Om du har följt hello felsökningssteg ovan och det går inte att hitta hello svar, kan du granska hello supportalternativen nedan.

* Få hjälp från Azure-experter. Skicka din problemet toohello [MSDN Azure eller Stack Overflow-forum.](https://azure.microsoft.com/support/forums/).
* En Azure-supportincident-filen. Gå toohello [Azure stöder site](https://azure.microsoft.com/support/options/) och på **få support** under **Technical och fakturering support**.
* Skicka en begäran om skriptet på [Script Center](https://azure.microsoft.com/documentation/scripts/) om du söker efter en Azure Automation runbook-lösning eller en modul för integrering.
* Skicka feedback eller funktionen begäranden för Azure Automation på [User Voice](https://feedback.azure.com/forums/34192--general-feedback).
