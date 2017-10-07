---
title: "aaaCreate identitet för Azure-app med PowerShell | Microsoft Docs"
description: "Beskriver hur toouse Azure PowerShell toocreate ett Azure Active Directory-program och tjänstens huvudnamn och ge det åtkomst till tooresources via rollbaserad åtkomst kontroll. Den visar hur tooauthenticate program med ett lösenord eller ett certifikat."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: c534360799b590054a051e4426e5e27dccb559b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toocreate-a-service-principal-tooaccess-resources"></a>Använda Azure PowerShell toocreate en service principal tooaccess resurser

När du har en app eller skript som behöver tooaccess resurser kan du ställa in en identitet för hello app och autentisera hello appen med sina egna autentiseringsuppgifter. Den här identiteten kallas ett huvudnamn för tjänsten. Den här metoden kan du:

* Tilldela behörigheter toohello app identitet som skiljer sig från din egen behörighet. Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.
* Använda ett certifikat för autentisering när du kör ett oövervakat skript.

Det här avsnittet beskrivs hur du toouse [Azure PowerShell](/powershell/azure/overview) tooset dig allt du behöver för ett program toorun under sina egna autentiseringsuppgifter och identitet.

## <a name="required-permissions"></a>Behörigheter som krävs
toocomplete det här avsnittet, du måste ha tillräckliga behörigheter för både i Azure Active Directory och Azure-prenumerationen. Du måste specifikt vara kan toocreate en app i hello Azure Active Directory och tilldela hello service principal tooa roll. 

hello enklaste sättet toocheck om ditt konto har rätt behörigheter är hello-portalen. Se [Kontrollera behörighet](resource-group-create-service-principal-portal.md#required-permissions).

Nu fortsätter du tooa för att autentisera med:

* [lösenord](#create-service-principal-with-password)
* [självsignerat certifikat](#create-service-principal-with-self-signed-certificate)
* [certifikat från certifikatutfärdare](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>PowerShell-kommandon

tooset upp en tjänst som du använder:

| Kommando | Beskrivning |
| ------- | ----------- | 
| [Ny AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Skapar ett Azure Active Directory-tjänstens huvudnamn |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Tilldelar hello anges RBAC rollen toohello angivna huvudnamnet, på hello scope som anges. |


## <a name="create-service-principal-with-password"></a>Skapa tjänstens huvudnamn med lösenord

toocreate ett huvudnamn för tjänsten med hello deltagarrollen för din prenumeration, Använd: 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

hello exempel viloläge 20 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory. Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.

hello följande skript kan du toospecify ett scope än hello standard prenumerationen och försök hello rolltilldelning om ett fel uppstår:

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for hello AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Några objekt toonote hello skriptet:

* toogrant hello identitet åtkomst toohello standard prenumeration som du behöver inte tooprovide ResourceGroup eller SubscriptionId parametrar.
* Ange hello ResourceGroup parameter när du själv toolimit hello omfattning hello rollen tilldelning tooa resursgruppen.
*  Lägg till hello service principal toohello deltagarrollen i det här exemplet. Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).
* hello skript i viloläge för 15 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory. Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.
* Om du behöver toogrant hello service principal åtkomst toomore prenumerationer eller resursgrupper, kör hello `New-AzureRMRoleAssignment` cmdleten igen med olika omfång.


### <a name="provide-credentials-through-powershell"></a>Ange autentiseringsuppgifter med hjälp av PowerShell
Du måste nu toolog i som hello programmet tooperform funktioner. Använd hello för hello användarnamn, `ApplicationId` som du skapade för hello program. Använd hello något du angav när du skapar hello konto för hello lösenord. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

hello klient-ID är inte skiftlägeskänslig, så du kan bädda in den direkt i skriptet. Om du behöver tooretrieve hello klient-ID, Använd:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Skapa tjänstens huvudnamn med självsignerade certifikat

toocreate ett huvudnamn för tjänsten med ett självsignerat certifikat och hello deltagarrollen för din prenumeration, Använd: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

hello exempel viloläge 20 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory. Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.

hello följande skript kan du toospecify ett scope än hello standard prenumerationen och försök hello rolltilldelning om ett fel inträffar. Du måste ha Azure PowerShell 2.0 på Windows 10 eller Windows Server 2016.

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Några objekt toonote hello skriptet:

* toogrant hello identitet åtkomst toohello standard prenumeration som du behöver inte tooprovide ResourceGroup eller SubscriptionId parametrar.
* Ange hello ResourceGroup parameter när du själv toolimit hello omfattning hello rollen tilldelning tooa resursgruppen.
* Lägg till hello service principal toohello deltagarrollen i det här exemplet. Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).
* hello skript i viloläge för 15 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory. Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.
* Om du behöver toogrant hello service principal åtkomst toomore prenumerationer eller resursgrupper, kör hello `New-AzureRMRoleAssignment` cmdleten igen med olika omfång.

Om du **inte har Windows 10 eller Windows Server 2016 Technical Preview**, behöver du toodownload hello [självsignerat certifikat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) från Microsoft Script Center. Extrahera innehållet och importera hello-cmdlet som du behöver.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Ersätt hello följande två rader toogenerate hello certifikat i hello skript.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Ange certifikat via automatisk PowerShell-skript
Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app. En klient är en instans av Azure Active Directory. Om du bara har en prenumeration kan du använda:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

hello program-ID och klient-ID är inte skiftlägeskänslig, så du kan bädda in dem direkt i skriptet. Om du behöver tooretrieve hello klient-ID, Använd:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Om du behöver tooretrieve hello program-ID, Använd:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Skapa tjänstens huvudnamn med certifikat från certifikatutfärdare
toouse ett certifikat som utfärdats av en certifikatutfärdare toocreate tjänstens huvudnamn, Använd hello följande skript:

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId

 $KeyId = (New-Guid).Guid
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
 $KeyCredential.StartDate = $PFXCert.NotBefore
 $KeyCredential.EndDate= $PFXCert.NotAfter
 $KeyCredential.KeyId = $KeyId
 $KeyCredential.CertValue = $KeyValue

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -KeyCredentials $keyCredential
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

Några objekt toonote hello skriptet:

* Åtkomst är begränsade toohello prenumeration.
* Lägg till hello service principal toohello deltagarrollen i det här exemplet. Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).
* hello skript i viloläge för 15 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory. Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.
* Om du behöver toogrant hello service principal åtkomst toomore prenumerationer eller resursgrupper, kör hello `New-AzureRMRoleAssignment` cmdleten igen med olika omfång.

### <a name="provide-certificate-through-automated-powershell-script"></a>Ange certifikat via automatisk PowerShell-skript
Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app. En klient är en instans av Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

hello program-ID och klient-ID är inte skiftlägeskänslig, så du kan bädda in dem direkt i skriptet. Om du behöver tooretrieve hello klient-ID, Använd:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Om du behöver tooretrieve hello program-ID, Använd:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Ändra autentiseringsuppgifter

toochange hello autentiseringsuppgifter för en AD-app, antingen på grund av ett säkerhetsintrång eller en autentiseringsuppgift förfallodatum, använda hello [ta bort AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) och [ny AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.

tooremove alla hello autentiseringsuppgifter för ett program, Använd:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

tooadd ett lösenord, Använd:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

tooadd ett Certifikatvärde, skapa ett självsignerat certifikat som visas i det här avsnittet. Använd sedan:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-toosimplify-log-in"></a>Spara åtkomst-token toosimplify logga in
tooavoid tillhandahåller hello service principal autentiseringsuppgifterna varje gång den måste toolog i, kan du spara hello åtkomst-token.

toouse hello aktuella åtkomst-token i en senare session, spara hello profil.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Öppna hello-profilen och undersöka dess innehåll. Observera att den innehåller en åtkomst-token. I stället för att manuellt logga in igen, bara läsa hello profilen.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> hello åtkomst-token upphör att gälla, så att använda en sparad profil fungerar bara för så länge hello token är giltig.
>  

Alternativt kan du anropa REST-åtgärder från PowerShell toolog i. Du kan hämta hello åtkomsttoken för användning med andra åtgärder från hello autentiseringssvar. Ett exempel för att hämta hello åtkomst-token genom att anropa REST-åtgärder finns [genererar en åtkomst-Token](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Felsökning

Du kan stöta på hello följande fel när du skapar ett huvudnamn för tjänsten:

* **”Authentication_Unauthorized”** eller **”ingen prenumeration finns i kontexten för hello”.** – Du ser det här felet när ditt konto inte har hello [nödvändiga behörigheter](#required-permissions) på hello Azure Active Directory tooregister en app. Normalt ser du det här felet när endast administrativa användare i Azure Active Directory kan du registrera appar och ditt konto är inte en administratör. Be din administratör tooeither tilldela tooan administratörsroll eller tooenable användare tooregister appar.

* Ditt konto **”har inte tillstånd tooperform åtgärd 'Microsoft.Authorization/roleAssignments/write' över område '/subscriptions/ {guid}'”. ** -Det här felet visas när ditt konto inte har tillräckliga behörigheter tooassign en roll tooan identitet. Be din prenumeration administratören tooadd du tooUser åtkomst rollen Administratör.

## <a name="sample-applications"></a>Exempelprogram
För information om att logga in som hello program via olika plattformar, se:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Nästa steg
* Detaljerade anvisningar för att integrera ett program i Azure för att hantera resurser, se [Developer's guide tooauthorization med hello Azure Resource Manager API](resource-manager-api-authentication.md).
* En mer detaljerad förklaring av program och tjänstens huvudnamn finns [program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md). 
* Läs mer om Azure Active Directory-autentisering, [Autentiseringsscenarier för Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* En lista över tillgängliga åtgärder som kan beviljas eller nekas toousers finns [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).

