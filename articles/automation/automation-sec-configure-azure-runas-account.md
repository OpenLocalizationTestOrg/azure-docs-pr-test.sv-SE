---
title: "aaaConfigure ett Azure kör som-konto | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello skapa, testa och exempel användning av säkerhetsobjekt autentisering i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "tjänstobjektnamn, setspn, azure-autentisering"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: True
ms.openlocfilehash: 06675d2f6b9d8e7260ffaead4f2b2f61c2b98d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Autentisera runbookflöden med ett Kör som-konto i Azure
Den här artikeln visar hur tooconfigure en Azure Automation-kontot i hello Azure-portalen. toodo så är fallet bör du använda hello kör som-konto funktionen tooauthenticate runbooks hantera resurser i Azure Resource Manager eller Azure Service Management.

När du skapar ett Automation-konto i hello Azure-portalen kan skapa du automatiskt två konton:

* Ett Kör som-konto. Det här kontot skapar ett tjänstobjekt i Azure Active Directory (AD Azure) och ett certifikat. Det ger också hello deltagare rollbaserad åtkomstkontroll (RBAC), som hanterar Resource Manager-resurser med hjälp av runbooks.
* Ett klassiskt Kör som-konto. Det här kontot laddar upp ett hanteringscertifikat som används toomanage Service Management eller klassiska resurser med hjälp av runbooks.

Skapa en Automation konto gör hello enklare för dig och hjälper dig att snabbt börja skapa och distribuera runbooks toosupport ditt automation måste.

Med Kör som-konton och klassiska Kör som-konton kan du:

* Ange ett standardiserat sätt tooauthenticate med Azure när du hanterar hanteraren för filserverresurser eller Service Management-resurser från runbooks i hello Azure-portalen.
* Automatisera hello användning av globala runbooks som du kan konfigurera i Azure-aviseringar.

> [!NOTE]
> Hej [Azure integration postaviseringsfunktionen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) med Automation globala runbooks kräver ett Automation-konto som har konfigurerats med en Kör som-konto och klassiska kör som-konto. Du kan välja ett Automation-konto som redan har definierats kör som och klassiska kör som-konton eller du toocreate ett nytt Automation-konto.
>  

Den här artikeln visar hur toocreate ett Automation-konto från hello Azure-portalen, uppdatera ett Automation-konto med hjälp av Azure PowerShell, hantera hello kontokonfiguration och autentiseras i dina runbooks.

Innan du börjar skapa ett Automation-konto, är en bra idé toounderstand och Tänk hello följande:

* Skapa ett Automation-konto påverkar inte Automation-konton som du kanske redan har skapat i hello klassiska eller Resource Manager-distributionsmodellen.
* hello processen fungerar endast för Automation-konton som du skapar i hello Azure-portalen. Försöker toocreate ett konto från hello klassiska Azure-portalen replikerar inte konfigurationen hello kör som-konto.
* Om du redan har runbookflöden och tillgångar (till exempel scheman eller variabler) i plats toomanage klassiska resurser och du vill runbooks tooauthenticate med hello nya klassiska kör som-konto, gör du något av följande hello:

  * toocreate klassiska kör som-konton, följ instruktionerna för hello under hello ”hantera dina kör som-konto”. 
  * tooupdate ditt befintliga konto, Använd hello PowerShell-skript under hello ”uppdatera ditt Automation-konto med hjälp av PowerShell”.
* tooauthenticate med hjälp av hello nytt kör som-konto och klassiska kör som Automation-konto behöver du toomodify dina befintliga runbooks med hello exempelkoden som visas i avsnittet hello [Autentiseringskod](#authentication-code-examples).

    >[!NOTE]
    >hello kör som-konto är för autentisering gentemot Resource Manager-resurser med hjälp av hello certifikatbaserad tjänstens huvudnamn. hello klassiska kör som-konto är för att autentisera mot Service Management-resurser med ett certifikat.

## <a name="create-an-automation-account-from-hello-azure-portal"></a>Skapa ett Automation-konto från hello Azure-portalen
I det här avsnittet skapar du ett Azure Automation-konto från hello Azure-portalen, vilket i sin tur skapar både en Kör som-konto och klassiska kör som-konto.

>[!NOTE]
>toocreate ett Automation-konto som du måste vara Rollmedlem hello Service administratörer eller medadministratör för hello-prenumeration som ger åtkomst toohello prenumeration. Du måste också läggas till som en användare toothat prenumeration standardinstansen för Active Directory. hello kontot behöver inte toobe som tilldelats en privilegierad roll.
>
>Om du inte är medlem i Active Directory-instans för hello prenumeration innan du lade toohello medadministratör rollen hello prenumeration kan läggs du tooActive Directory som en gäst. I den här instansen får du en ”du har inte behörighet toocreate...” varning för hello **lägga till Automation-konto** bladet.
>
>Användare som har lagts till toohello samtidigt administratörsroll först kan tas bort från hello prenumeration Active Directory-instans och läggas till toomake dem en fullständig användare i Active Directory. tooverify detta hello **Azure Active Directory** rutan i hello Azure-portalen genom att välja **användare och grupper**, välja **alla användare** och, när du har valt hello specifika användare, välja **profil**. Hej värdet för hello **användartyp** attributet under hello användare profil ska inte vara lika med **gäst**.
>

1. Logga in toohello Azure-portalen med ett konto som är medlem i administratörsrollen i hello prenumeration och medadministratör för hello prenumeration.

2. Välj **Automation-konton**.

3. På hello **Automation-konton** bladet, klickar du på **Lägg till**.
Hej **lägga till Automation-konto** blad öppnas.

 ![Hej ”Lägg till Automation-konto” bladet](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Om ditt konto inte är medlem i administratörsrollen i hello prenumeration och medadministratör för prenumerationen hello hello följande varning visas på hello **lägga till Automation-konto** bladet:
   >
   >![Varningsmeddelande för Lägg till Automation-konto](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. På hello **lägga till Automation-konto** bladet i hello **namn** Skriv ett namn för det nya Automation-kontot.

5. Om du har mer än en prenumeration hello följande:

    a. Under **prenumeration**, ange ett nytt hello-konto.

    b. Under **Resursgrupp** klickar du på **Skapa ny** eller på **Använd befintlig**.

    c. Under **Plats** anger du ett Azure-datacenter.

6. Under **Skapa Kör som-konto i Azure** väljer du **Ja** och klickar sedan på **Skapa**.

   > [!NOTE]
   > Om du väljer inte toocreate hello kör som-konto genom att välja **nr**, visas ett varningsmeddelande hello **lägga till Automation-konto** bladet. Även om hello kontot skapas i hello Azure-portalen har inte en motsvarande autentiseringsidentitet inom din klassiska eller Resource Manager prenumeration katalogtjänsten. Hello-kontot har därför ingen åtkomst tooresources i din prenumeration. Det här scenariot förhindrar att runbookflöden som refererar till det här kontot autentiseras och utför åtgärder mot resurser i dessa distributionsmodeller.
   >
   > ![Varningsmeddelande på hello ”Lägg till Automation-konto” blad](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Dessutom kan tilldelas inte hello deltagarrollen eftersom hello tjänstens huvudnamn inte har skapats.
   >

7. Medan Azure skapar hello Automation-konto, du kan följa förloppet för hello under **meddelanden** hello-menyn.

### <a name="resources"></a>Resurser
När hello Automation-konto har skapats, skapas flera resurser automatiskt åt dig. hello resurser sammanfattas i följande två tabeller hello:

#### <a name="run-as-account-resources"></a>Resurser för Kör som-konton

| Resurs | Beskrivning |
| --- | --- |
| AzureAutomationTutorial-runbook | Ett exempel grafisk runbook som visar hur hello tooauthenticate med hjälp av kör som-konto och hämtar alla hello Resource Manager-resurser. |
| AzureAutomationTutorialScript-runbook | Ett exempel PowerShell-runbook som visar hur hello tooauthenticate med hjälp av kör som-konto och hämtar alla hello Resource Manager-resurser. |
| AzureRunAsCertificate | Hej certifikattillgång som skapas automatiskt när du skapar ett Automation-konto eller använda hello följande PowerShell-skript för ett befintligt konto. hello certifikatet kan tooauthenticate med Azure så att du kan hantera Azure Resource Manager-resurser från runbooks. hello certifikatet har en livslängd för ett år. |
| AzureRunAsConnection | Hej anslutningstillgång som skapas automatiskt när du skapar ett Automation-konto eller använda hello PowerShell-skript för ett befintligt konto. |

#### <a name="classic-run-as-account-resources"></a>Resurser för klassiska Kör som-konton

| Resurs | Beskrivning |
| --- | --- |
| AzureClassicAutomationTutorial-runbook | En grafisk exempel-runbook som hämtar alla hello virtuella datorer som skapas med hello klassiska distributionsmodellen för en prenumeration med hjälp av hello klassiska kör som-konto (certifikat) och sedan skriver hello VM namn och status. |
| AzureClassicAutomationTutorial Script-runbook | Ett exempel PowerShell-runbook som hämtar alla hello klassiska virtuella datorer i en prenumeration med hjälp av hello klassiska kör som-konto (certifikat) och sedan skriver hello VM-namn och status. |
| AzureClassicRunAsCertificate | hello skapas automatiskt certifikattillgång använda tooauthenticate med Azure så att du kan hantera Azure klassiska resurser från runbooks. hello certifikatet har en livslängd för ett år. |
| AzureClassicRunAsConnection | hello automatiskt skapade anslutningstillgång använda tooauthenticate med Azure så att du kan hantera Azure klassiska resurser från runbooks. |

## <a name="verify-run-as-authentication"></a>Verifiera Kör som-autentisering
Utför en test små tooconfirm som du kan autentisera med hjälp av hello nytt kör som-konto.

1. Öppna hello Automation-konto som du skapade tidigare i hello Azure-portalen.

2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.

3. Välj hello **AzureAutomationTutorialScript** runbook, och klicka sedan på **starta** toostart hello runbook. hello följande händelser inträffar:
 * En [runbook-jobbet](automation-runbook-execution.md) skapas hello **jobbet** bladet visas och hello jobbstatus visas i hello **jobbsammanfattning** panelen.
 * hello jobbstatus börjar som **i kö**, vilket betyder att det väntar på att en runbook worker i hello molnet toobecome tillgängliga.
 * hello statusen **Start** när en arbetsprocess anspråk hello jobb.
 * hello statusen **kör** när hello runbook börjar köras.
 * När hello runbook-jobbet har körts ska du se statusen **slutförd**.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.  
Hej **utdata** bladet visas, visar att hello runbook har autentiserats och returneras en lista över alla resurser som är tillgängliga i hello resursgruppen.

5. Stäng hello **utdata** bladet tooreturn toohello **jobbsammanfattning** bladet.

6. Stäng hello **jobbsammanfattning** bladet och hello motsvarande **AzureAutomationTutorialScript** runbook-bladet.

## <a name="verify-classic-run-as-authentication"></a>Verifiera klassisk Kör som-autentisering
Utföra en liknande liten testa tooconfirm som du kan autentisera med hjälp av hello nya klassiska kör som-konto.

1. Öppna hello Automation-konto som du skapade tidigare i hello Azure-portalen.

2. Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.

3. Välj hello **AzureClassicAutomationTutorialScript** runbook, och klicka sedan på **starta** för startar hello runbook. hello följande händelser inträffar:

 * En [runbook-jobbet](automation-runbook-execution.md) skapas hello **jobbet** bladet visas och hello jobbstatus visas i hello **jobbsammanfattning** panelen.
 * hello jobbstatus börjar som **i kö**, vilket betyder att det väntar på att en runbook worker i hello molnet toobecome tillgängliga.
 * hello statusen **Start** när en arbetsprocess anspråk hello jobb.
 * hello statusen **kör** när hello runbook börjar köras.
 * När hello runbook-jobbet har körts ska du se statusen **slutförd**.

    ![Runbook-test för säkerhetsobjekt](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.  
Hej **utdata** bladet visas, där den hello runbook har autentiserats och returneras en lista över alla klassiska virtuella datorer i hello prenumeration.

5. Stäng hello **utdata** bladet tooreturn toohello **jobbsammanfattning** bladet.

6. Stäng hello **jobbsammanfattning** bladet och hello motsvarande **AzureAutomationTutorialScript** runbook-bladet.

## <a name="managing-your-run-as-account"></a>Hantera ett Kör som-konto
Någon gång innan det Automation-kontot upphör att gälla måste toorenew hello certifikat. Om du tror att hello kör som-konto har komprometterats kan du ta bort och skapa den igen. Det här avsnittet beskrivs hur tooperform dessa åtgärder.

### <a name="self-signed-certificate-renewal"></a>Förnya självsignerade certifikat
hello självsignerat certifikat som du skapade för hello kör som-konto går ut ett år från hello dag skapas. Du kan förnya det när som helst innan det upphör att gälla. När du förnyar det är hello aktuella giltiga certifikat kvarhållna tooensure att alla runbooks som lagts i kö upp eller aktivt körs och som autentiserar med hello kör som-konto inte påverkas negativt. hello intyg är giltigt fram till dess förfallodatum.

> [!NOTE]
> Om du har konfigurerat ditt Automation kör som-konto toouse ett certifikat utfärdat av en enterprise-certifikatutfärdare och du använder det här alternativet, ersätts hello företagscertifikat av ett självsignerat certifikat.

toorenew hello certifikat, hello följande:

1. Öppna hello Automation-konto i hello Azure-portalen.

2. På hello **Automation-konto** bladet i hello **konto egenskaper** rutan under **kontoinställningar**väljer **kör som-konton**.

    ![Egenskapsrutan för Automation-konto](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller hello klassiska kör som-konto som du vill ha toorenew hello certifikat för.

4. På hello **egenskaper** bladet för hello valt konto, klickar du på **förnya certifikat**.

    ![Förnya certifikat för Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. Medan hello-certifikat förnyas du kan följa förloppet för hello under **meddelanden** hello-menyn.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Ta bort ett Kör som-konto eller ett klassiskt Kör som-konto
Det här avsnittet beskrivs hur toodelete och sedan skapa ett kör som eller klassiska kör som-konto. När du utför den här åtgärden sparas hello Automation-konto. När du har tagit bort ett kör som eller klassiska kör som-konto kan skapa du den igen hello Azure-portalen.

1. Öppna hello Automation-konto i hello Azure-portalen.

2. På hello **automatiseringskontot** bladet i hello konto egenskapsrutan, Välj **kör som-konton**.

3. På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller klassiska kör som-konto som du vill toodelete. Klicka sedan på hello **egenskaper** bladet för hello valt konto, klickar du på **ta bort**.

 ![Ta bort Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. Medan hello konto raderas, du kan följa förloppet hello under **meddelanden** hello-menyn.

5. När hello-konto har tagits bort, du kan skapa den igen på hello **kör som-konton** egenskapsbladet genom att välja hello skapa alternativet **Azure kör som-konto**.

 ![Skapa nytt hello Automation kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Felaktig konfiguration
Vissa konfigurationsobjekt som är nödvändiga för hello kör som eller klassiska kör som-konto toofunction kanske korrekt har tagits bort eller skapats felaktigt under installationen. hello-objekt är:

* Certifikattillgång
* Anslutningstillgång
* Kör som-konto har tagits bort från hello deltagarrollen
* Huvudnamn för tjänsten eller program i Azure AD

I föregående hello och andra instanser av felaktig konfiguration hello Automation-konto identifierar hello ändras och visar statusen *ofullständigt* på hello **kör som-konton** egenskapsbladet för hello konto.

![Konfigurationsstatusen Ofullständig för Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

När du väljer hello-kör som-konto hello konto **egenskaper** hello följande felmeddelande visas:

![Varningsmeddelande om ofullständig konfiguration av Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

Du kan snabbt lösa problemen kör som-konto genom att ta bort och återskapa hello-konto.

## <a name="update-your-automation-account-by-using-powershell"></a>Uppdatera Automation-kontot med hjälp av PowerShell
Du kan använda PowerShell tooupdate befintligt Automation-konto om:

* Du skapar ett Automation-konto men neka toocreate hello kör som-konto.
* Du använder redan en Automation-konto toomanage Resource Manager-resurser och du vill tooupdate hello konto tooinclude hello kör som-konto för runbook-autentisering.
* Du redan använder en Automation-konto toomanage klassiska resurser och du vill tooupdate den toouse hello klassiska kör som-konto i stället för att skapa ett nytt konto och migrera dina runbook-flöden och tillgångar tooit.   
* Vill du toocreate kör som och klassiska kör som-konto med hjälp av ett certifikat utfärdat av enterprise-certifikatutfärdare (CA).

hello skriptet har hello följande krav:

* hello-skript kan köras endast på Windows 10 och Windows Server 2016 med Azure Resource Manager moduler 2.01 och senare. Det stöds inte i tidigare versioner av Windows.
* Azure PowerShell 1.0 och senare. Information om hello PowerShell 1.0-versionen finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* Ett Automation-konto som refereras till som hello värde hello *– AutomationAccountName* och *- ApplicationDisplayName* parametrar i hello följande PowerShell-skript.

tooget hello värden för *SubscriptionID*, *ResourceGroup*, och *AutomationAccountName*, som är nödvändiga parametrar för hello skript, hello följande:
1. I hello Azure-portalen, Välj ditt Automation-konto på hello **automatiseringskontot** bladet och väljer sedan **alla inställningar**. 
2. På hello **alla inställningar** bladet under **kontoinställningar**väljer **egenskaper**. 
3. Observera hello värden på hello **egenskaper** bladet.

![hello Automation-konto ”egenskaper” bladet](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Skapa ett PowerShell-skript för ett Kör som-konto
Detta PowerShell-skript har stöd för hello följande konfigurationer:

* Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat.
* Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat.
* Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat.
* Skapa ett kör som-konto och klassiska kör som-konto med hjälp av ett självsignerat certifikat i hello Azure Government-molnet.

Beroende på hello alternativ du väljer, skapar hello skriptet hello följande objekt.

**För Kör som-konton:**

* Skapar en Azure AD application toobe exporteras med antingen hello självsignerat eller enterprise certifikatets offentliga nyckel, skapar en tjänstens objektkonto för hello program i Azure AD och tilldelar hello deltagarrollen för hello konto i din aktuella prenumeration. Du kan ändra den här inställningen tooOwner eller någon annan roll. Mer information finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).
* Skapar ett Automation-certifikattillgång med namnet *AzureRunAsCertificate* angiven i hello Automation-konto. Hej certifikattillgång innehåller hello certifikatets privata nyckel som används av programmet hello Azure AD.
* Skapar ett Automation-anslutningstillgång med namnet *AzureRunAsConnection* angiven i hello Automation-konto. Hej anslutningstillgång innehåller hello applicationId, tenantId, prenumerations-ID och tumavtrycket för certifikatet.

**För klassiska Kör som-konton:**

* Skapar ett Automation-certifikattillgång med namnet *AzureClassicRunAsCertificate* angiven i hello Automation-konto. Hej certifikattillgång innehåller hello certifikatets privata nyckel används av hello certifikat.
* Skapar ett Automation-anslutningstillgång med namnet *AzureClassicRunAsConnection* angiven i hello Automation-konto. Hej anslutningstillgång innehåller hello prenumerationsnamn, prenumerations-ID och certifikatnamn för tillgången.

>[!NOTE]
> Om du väljer något av alternativen för att skapa en klassisk kör som-konto när hello skriptet körs skapades överför hello offentliga certifikatarkiv (.cer-filnamnstillägget) toohello management för hello prenumeration som hello Automation-kontot.
> 

tooexecute Hej skript och överför hello certifikat, hello följande:

1. Spara hello följande skript på datorn. I det här exemplet spara om filen med hello filename *ny RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds tooallow hello service principal application toobecome active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate hello ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload hello .cer format of #CERT# toohello Management store by following hello steps below." + [Environment]::NewLine +
                    "Log in toohello Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload hello .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate hello ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Klicka på **Start** på datorn och starta sedan **Windows PowerShell** med utökade användarrättigheter.

3. Från hello utökade PowerShell kommandoradsgränssnitt, gå toohello mapp som innehåller hello-skript som du skapade i steg 1.

4. Kör hello-skript med hjälp av hello parametervärden för hello-konfiguration som du behöver.

    **Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Skapa ett kör som-konto och klassiska kör som-konto med hjälp av ett självsignerat certifikat i hello Azure Government-moln**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Du kommer att tillfrågas tooauthenticate med Azure när hello skriptet har körts. Logga in med ett konto som är medlem i administratörsrollen i hello prenumeration och medadministratör för hello prenumeration.
    >
    >

Observera följande hello efter hello skriptet har körts:
* Om du har skapat en klassiska kör som-konto med ett självsignerat certifikat med offentlig (.cer-fil) hello skriptet skapar och sparar toohello tillfällig mapp på datorn under hello användarprofil *%USERPROFILE%\AppData\Local\Temp*, som du använde tooexecute hello PowerShell-session.
* Om du skapade ett klassiskt Kör som-konto med ett offentligt företagscertifikat (CER-fil) använder du det här certifikatet. Följ anvisningarna för hello för [överför en hanterings-API certifikat toohello klassiska Azure-portalen](../azure-api-management-certs.md), och sedan Validera hello behörighetskonfigurationen med Service Management-resurser med hjälp av hello [exempelkod tooauthenticate med Service Management-resurser](#sample-code-to-authenticate-with-service-management-resources). 
* Om du har gjort *inte* skapa klassiska kör som-konton, autentisera med Resource Manager-resurser och validera hello autentiseringsuppgifter konfigurationen med hjälp av hello [exempelkod för att autentisera med Service Management resurser](#sample-code-to-authenticate-with-resource-manager-resources).

## <a name="sample-code-tooauthenticate-with-resource-manager-resources"></a>Exempel kod tooauthenticate med Resource Manager-resurser
Du kan använda hello följande uppdaterad exempelkod tas från hello *AzureAutomationTutorialScript* exempel-runbook, tooauthenticate med hello kör som-konto toomanage Resource Manager-resurser med dina runbooks.

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get hello connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in tooAzure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context tooa specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }

toohelp du tooeasily fungerar mellan flera prenumerationer, hello skriptet innehåller två ytterligare rader med kod som har stöd för refererar till en prenumerationskontext. En variabeltillgång med namnet *SubscriptionId* innehåller hello-ID för hello prenumeration. Efter hello `Add-AzureRmAccount` cmdlet instruktionen hello [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet anges med hello parameteruppsättning *- SubscriptionId*. Om hello variabelnamn för Allmänt kan du ändra tooinclude ett prefix till den eller använda en annan naming convention toomake den enklare tooidentify. Du kan också använda hello parameteruppsättning *- SubscriptionName* i stället för *- SubscriptionId* med en motsvarande variabeltillgång.

Hej cmdlet som du använder för att autentisera i hello runbook `Add-AzureRmAccount`, använder hello *ServicePrincipalCertificate* parameteruppsättning. Det autentiserar med hjälp av hello service principal certifikat, inte hello användarens autentiseringsuppgifter.

## <a name="sample-code-tooauthenticate-with-service-management-resources"></a>Exempel kod tooauthenticate med Service Management-resurser
Du kan använda följande uppdaterad exempelkod som hämtas från hello hello *AzureClassicAutomationTutorialScript* exempel-runbook, tooauthenticate med hjälp av hello klassiska kör som-kontot toomanage klassiska resurser med din runbooks.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Nästa steg
* [Program och tjänstobjekt i Azure AD](../active-directory/active-directory-application-objects.md)
* [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md)
* [Certifikatöversikt för Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)
