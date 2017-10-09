---
title: "aaaCreate Azure Automation kör som-konto med PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur tooupgrade ditt Automation-konto med PowerShell toocreate hello kör som-konton om du inte utföra det här steget Skapa från hello-portalen."
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
ms.date: 04/14/2017
ms.author: magoedte
ms.openlocfilehash: 1049601321d2bc1e5f9d982f622788f8e4e4d797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-automation-run-as-account-using-powershell"></a>Uppdatera ett Automation Kör som-konto med hjälp av PowerShell
Du kan använda PowerShell tooupdate befintligt Automation-konto om:

* Du skapar ett Automation-konto men neka toocreate hello kör som-konto.
* Du använder redan en Automation-konto toomanage Resource Manager-resurser och du vill tooupdate hello konto tooinclude hello kör som-konto för runbook-autentisering.
* Du redan använder en Automation-konto toomanage klassiska resurser och du vill tooupdate den toouse hello klassiska kör som-konto i stället för att skapa ett nytt konto och migrera dina runbook-flöden och tillgångar tooit.   
* Vill du toocreate kör som och klassiska kör som-konto med hjälp av ett certifikat utfärdat av enterprise-certifikatutfärdare (CA).

## <a name="prerequisites"></a>Krav

* hello-skript kan köras endast på Windows 10 och Windows Server 2016 med Azure Resource Manager moduler 2.01 och senare. Det stöds inte i tidigare versioner av Windows.
* Azure PowerShell 1.0 och senare. Information om hello PowerShell 1.0-versionen finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* Ett Automation-konto som refereras till som hello värde hello *– AutomationAccountName* och *- ApplicationDisplayName* parametrar i hello följande PowerShell-skript.

tooget hello värden för *SubscriptionID*, *ResourceGroup*, och *AutomationAccountName*, som är nödvändiga parametrar för hello skript, hello följande:
1. I hello Azure-portalen, Välj ditt Automation-konto på hello **automatiseringskontot** bladet och väljer sedan **alla inställningar**. 
2. På hello **alla inställningar** bladet under **kontoinställningar**väljer **egenskaper**. 
3. Observera hello värden på hello **egenskaper** bladet.

![hello Automation-konto ”egenskaper” bladet](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a>Skapa PowerShell-skript för ett Kör som-konto
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

2. På datorn, startar **Windows PowerShell** från hello **starta** skärmen med utökade användarrättigheter.
3. Från hello utökade kommandoradsgränssnitt, gå toohello mapp som innehåller hello-skript som du skapade i steg 1.  
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
* Om du skapade ett klassiskt Kör som-konto med ett offentligt företagscertifikat (CER-fil) använder du det här certifikatet. Följ anvisningarna för hello för [överför en hanterings-API certifikat toohello klassiska Azure-portalen](../azure-api-management-certs.md), och sedan Validera hello behörighetskonfigurationen med klassisk distributionsresurser med hjälp av hello [exempelkod tooauthenticate med klassiska Azure-distributionsresurser](automation-verify-runas-authentication.md#classic-run-as-authentication). 
* Om du har gjort *inte* skapa klassiska kör som-konton, autentisera med Resource Manager-resurser och validera hello autentiseringsuppgifter konfigurationen med hjälp av hello [exempelkod för att autentisera med Service Management resurser](automation-verify-runas-authentication.md#automation-run-as-authentication).

## <a name="next-steps"></a>Nästa steg
* Mer information om tjänstens huvudnamn finns för[program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md).
* Mer information om certifikat och Azure-tjänster finns för[certifikat översikt för Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
