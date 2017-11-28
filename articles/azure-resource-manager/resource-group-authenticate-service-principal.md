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
# <a name="use-azure-powershell-toocreate-a-service-principal-tooaccess-resources"></a><span data-ttu-id="16597-104">Använda Azure PowerShell toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="16597-104">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>

<span data-ttu-id="16597-105">När du har en app eller skript som behöver tooaccess resurser kan du ställa in en identitet för hello app och autentisera hello appen med sina egna autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="16597-105">When you have an app or script that needs tooaccess resources, you can set up an identity for hello app and authenticate hello app with its own credentials.</span></span> <span data-ttu-id="16597-106">Den här identiteten kallas ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="16597-106">This identity is known as a service principal.</span></span> <span data-ttu-id="16597-107">Den här metoden kan du:</span><span class="sxs-lookup"><span data-stu-id="16597-107">This approach enables you to:</span></span>

* <span data-ttu-id="16597-108">Tilldela behörigheter toohello app identitet som skiljer sig från din egen behörighet.</span><span class="sxs-lookup"><span data-stu-id="16597-108">Assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="16597-109">Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="16597-109">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="16597-110">Använda ett certifikat för autentisering när du kör ett oövervakat skript.</span><span class="sxs-lookup"><span data-stu-id="16597-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="16597-111">Det här avsnittet beskrivs hur du toouse [Azure PowerShell](/powershell/azure/overview) tooset dig allt du behöver för ett program toorun under sina egna autentiseringsuppgifter och identitet.</span><span class="sxs-lookup"><span data-stu-id="16597-111">This topic shows you how toouse [Azure PowerShell](/powershell/azure/overview) tooset up everything you need for an application toorun under its own credentials and identity.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="16597-112">Behörigheter som krävs</span><span class="sxs-lookup"><span data-stu-id="16597-112">Required permissions</span></span>
<span data-ttu-id="16597-113">toocomplete det här avsnittet, du måste ha tillräckliga behörigheter för både i Azure Active Directory och Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="16597-113">toocomplete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="16597-114">Du måste specifikt vara kan toocreate en app i hello Azure Active Directory och tilldela hello service principal tooa roll.</span><span class="sxs-lookup"><span data-stu-id="16597-114">Specifically, you must be able toocreate an app in hello Azure Active Directory, and assign hello service principal tooa role.</span></span> 

<span data-ttu-id="16597-115">hello enklaste sättet toocheck om ditt konto har rätt behörigheter är hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="16597-115">hello easiest way toocheck whether your account has adequate permissions is through hello portal.</span></span> <span data-ttu-id="16597-116">Se [Kontrollera behörighet](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="16597-116">See [Check required permission](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="16597-117">Nu fortsätter du tooa för att autentisera med:</span><span class="sxs-lookup"><span data-stu-id="16597-117">Now, proceed tooa section for authenticating with:</span></span>

* [<span data-ttu-id="16597-118">lösenord</span><span class="sxs-lookup"><span data-stu-id="16597-118">password</span></span>](#create-service-principal-with-password)
* [<span data-ttu-id="16597-119">självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="16597-119">self-signed certificate</span></span>](#create-service-principal-with-self-signed-certificate)
* [<span data-ttu-id="16597-120">certifikat från certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="16597-120">certificate from Certificate Authority</span></span>](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a><span data-ttu-id="16597-121">PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="16597-121">PowerShell commands</span></span>

<span data-ttu-id="16597-122">tooset upp en tjänst som du använder:</span><span class="sxs-lookup"><span data-stu-id="16597-122">tooset up a service principal, you use:</span></span>

| <span data-ttu-id="16597-123">Kommando</span><span class="sxs-lookup"><span data-stu-id="16597-123">Command</span></span> | <span data-ttu-id="16597-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="16597-124">Description</span></span> |
| ------- | ----------- | 
| [<span data-ttu-id="16597-125">Ny AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="16597-125">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="16597-126">Skapar ett Azure Active Directory-tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="16597-126">Creates an Azure Active Directory service principal</span></span> |
| [<span data-ttu-id="16597-127">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="16597-127">New-AzureRmRoleAssignment</span></span>](/powershell/module/azurerm.resources/new-azurermroleassignment) | <span data-ttu-id="16597-128">Tilldelar hello anges RBAC rollen toohello angivna huvudnamnet, på hello scope som anges.</span><span class="sxs-lookup"><span data-stu-id="16597-128">Assigns hello specified RBAC role toohello specified principal, at hello specified scope.</span></span> |


## <a name="create-service-principal-with-password"></a><span data-ttu-id="16597-129">Skapa tjänstens huvudnamn med lösenord</span><span class="sxs-lookup"><span data-stu-id="16597-129">Create service principal with password</span></span>

<span data-ttu-id="16597-130">toocreate ett huvudnamn för tjänsten med hello deltagarrollen för din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-130">toocreate a service principal with hello Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="16597-131">hello exempel viloläge 20 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-131">hello example sleeps for 20 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="16597-132">Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.</span><span class="sxs-lookup"><span data-stu-id="16597-132">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>

<span data-ttu-id="16597-133">hello följande skript kan du toospecify ett scope än hello standard prenumerationen och försök hello rolltilldelning om ett fel uppstår:</span><span class="sxs-lookup"><span data-stu-id="16597-133">hello following script enables you toospecify a scope other than hello default subscription, and retries hello role assignment if an error occurs:</span></span>

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

<span data-ttu-id="16597-134">Några objekt toonote hello skriptet:</span><span class="sxs-lookup"><span data-stu-id="16597-134">A few items toonote about hello script:</span></span>

* <span data-ttu-id="16597-135">toogrant hello identitet åtkomst toohello standard prenumeration som du behöver inte tooprovide ResourceGroup eller SubscriptionId parametrar.</span><span class="sxs-lookup"><span data-stu-id="16597-135">toogrant hello identity access toohello default subscription, you do not need tooprovide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="16597-136">Ange hello ResourceGroup parameter när du själv toolimit hello omfattning hello rollen tilldelning tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="16597-136">Specify hello ResourceGroup parameter only when you want toolimit hello scope of hello role assignment tooa resource group.</span></span>
*  <span data-ttu-id="16597-137">Lägg till hello service principal toohello deltagarrollen i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="16597-137">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="16597-138">Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="16597-138">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="16597-139">hello skript i viloläge för 15 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-139">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="16597-140">Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.</span><span class="sxs-lookup"><span data-stu-id="16597-140">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="16597-141">Om du behöver toogrant hello service principal åtkomst toomore prenumerationer eller resursgrupper, kör hello `New-AzureRMRoleAssignment` cmdleten igen med olika omfång.</span><span class="sxs-lookup"><span data-stu-id="16597-141">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>


### <a name="provide-credentials-through-powershell"></a><span data-ttu-id="16597-142">Ange autentiseringsuppgifter med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="16597-142">Provide credentials through PowerShell</span></span>
<span data-ttu-id="16597-143">Du måste nu toolog i som hello programmet tooperform funktioner.</span><span class="sxs-lookup"><span data-stu-id="16597-143">Now, you need toolog in as hello application tooperform operations.</span></span> <span data-ttu-id="16597-144">Använd hello för hello användarnamn, `ApplicationId` som du skapade för hello program.</span><span class="sxs-lookup"><span data-stu-id="16597-144">For hello user name, use hello `ApplicationId` that you created for hello application.</span></span> <span data-ttu-id="16597-145">Använd hello något du angav när du skapar hello konto för hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="16597-145">For hello password, use hello one you specified when creating hello account.</span></span> 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

<span data-ttu-id="16597-146">hello klient-ID är inte skiftlägeskänslig, så du kan bädda in den direkt i skriptet.</span><span class="sxs-lookup"><span data-stu-id="16597-146">hello tenant ID is not sensitive, so you can embed it directly in your script.</span></span> <span data-ttu-id="16597-147">Om du behöver tooretrieve hello klient-ID, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-147">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a><span data-ttu-id="16597-148">Skapa tjänstens huvudnamn med självsignerade certifikat</span><span class="sxs-lookup"><span data-stu-id="16597-148">Create service principal with self-signed certificate</span></span>

<span data-ttu-id="16597-149">toocreate ett huvudnamn för tjänsten med ett självsignerat certifikat och hello deltagarrollen för din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-149">toocreate a service principal with a self-signed certificate and hello Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="16597-150">hello exempel viloläge 20 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-150">hello example sleeps for 20 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="16597-151">Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.</span><span class="sxs-lookup"><span data-stu-id="16597-151">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>

<span data-ttu-id="16597-152">hello följande skript kan du toospecify ett scope än hello standard prenumerationen och försök hello rolltilldelning om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="16597-152">hello following script enables you toospecify a scope other than hello default subscription, and retries hello role assignment if an error occurs.</span></span> <span data-ttu-id="16597-153">Du måste ha Azure PowerShell 2.0 på Windows 10 eller Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="16597-153">You must have Azure PowerShell 2.0 on Windows 10 or Windows Server 2016.</span></span>

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

<span data-ttu-id="16597-154">Några objekt toonote hello skriptet:</span><span class="sxs-lookup"><span data-stu-id="16597-154">A few items toonote about hello script:</span></span>

* <span data-ttu-id="16597-155">toogrant hello identitet åtkomst toohello standard prenumeration som du behöver inte tooprovide ResourceGroup eller SubscriptionId parametrar.</span><span class="sxs-lookup"><span data-stu-id="16597-155">toogrant hello identity access toohello default subscription, you do not need tooprovide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="16597-156">Ange hello ResourceGroup parameter när du själv toolimit hello omfattning hello rollen tilldelning tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="16597-156">Specify hello ResourceGroup parameter only when you want toolimit hello scope of hello role assignment tooa resource group.</span></span>
* <span data-ttu-id="16597-157">Lägg till hello service principal toohello deltagarrollen i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="16597-157">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="16597-158">Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="16597-158">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="16597-159">hello skript i viloläge för 15 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-159">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="16597-160">Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.</span><span class="sxs-lookup"><span data-stu-id="16597-160">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="16597-161">Om du behöver toogrant hello service principal åtkomst toomore prenumerationer eller resursgrupper, kör hello `New-AzureRMRoleAssignment` cmdleten igen med olika omfång.</span><span class="sxs-lookup"><span data-stu-id="16597-161">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

<span data-ttu-id="16597-162">Om du **inte har Windows 10 eller Windows Server 2016 Technical Preview**, behöver du toodownload hello [självsignerat certifikat generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) från Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="16597-162">If you **do not have Windows 10 or Windows Server 2016 Technical Preview**, you need toodownload hello [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from Microsoft Script Center.</span></span> <span data-ttu-id="16597-163">Extrahera innehållet och importera hello-cmdlet som du behöver.</span><span class="sxs-lookup"><span data-stu-id="16597-163">Extract its contents and import hello cmdlet you need.</span></span>

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
<span data-ttu-id="16597-164">Ersätt hello följande två rader toogenerate hello certifikat i hello skript.</span><span class="sxs-lookup"><span data-stu-id="16597-164">In hello script, substitute hello following two lines toogenerate hello certificate.</span></span>
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="16597-165">Ange certifikat via automatisk PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="16597-165">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="16597-166">Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app.</span><span class="sxs-lookup"><span data-stu-id="16597-166">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="16597-167">En klient är en instans av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-167">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="16597-168">Om du bara har en prenumeration kan du använda:</span><span class="sxs-lookup"><span data-stu-id="16597-168">If you only have one subscription, you can use:</span></span>

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

<span data-ttu-id="16597-169">hello program-ID och klient-ID är inte skiftlägeskänslig, så du kan bädda in dem direkt i skriptet.</span><span class="sxs-lookup"><span data-stu-id="16597-169">hello application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="16597-170">Om du behöver tooretrieve hello klient-ID, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-170">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="16597-171">Om du behöver tooretrieve hello program-ID, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-171">If you need tooretrieve hello application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a><span data-ttu-id="16597-172">Skapa tjänstens huvudnamn med certifikat från certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="16597-172">Create service principal with certificate from Certificate Authority</span></span>
<span data-ttu-id="16597-173">toouse ett certifikat som utfärdats av en certifikatutfärdare toocreate tjänstens huvudnamn, Använd hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="16597-173">toouse a certificate issued from a Certificate Authority toocreate service principal, use hello following script:</span></span>

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

<span data-ttu-id="16597-174">Några objekt toonote hello skriptet:</span><span class="sxs-lookup"><span data-stu-id="16597-174">A few items toonote about hello script:</span></span>

* <span data-ttu-id="16597-175">Åtkomst är begränsade toohello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="16597-175">Access is scoped toohello subscription.</span></span>
* <span data-ttu-id="16597-176">Lägg till hello service principal toohello deltagarrollen i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="16597-176">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="16597-177">Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="16597-177">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="16597-178">hello skript i viloläge för 15 sekunder tooallow tid för hello nya service principal toopropagate i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-178">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="16597-179">Om skriptet inte vänta tillräckligt länge visas ett felmeddelande om ”: PrincipalNotFound: Principal {id} finns inte i hello directory”.</span><span class="sxs-lookup"><span data-stu-id="16597-179">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="16597-180">Om du behöver toogrant hello service principal åtkomst toomore prenumerationer eller resursgrupper, kör hello `New-AzureRMRoleAssignment` cmdleten igen med olika omfång.</span><span class="sxs-lookup"><span data-stu-id="16597-180">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="16597-181">Ange certifikat via automatisk PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="16597-181">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="16597-182">Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app.</span><span class="sxs-lookup"><span data-stu-id="16597-182">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="16597-183">En klient är en instans av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="16597-183">A tenant is an instance of Azure Active Directory.</span></span>

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

<span data-ttu-id="16597-184">hello program-ID och klient-ID är inte skiftlägeskänslig, så du kan bädda in dem direkt i skriptet.</span><span class="sxs-lookup"><span data-stu-id="16597-184">hello application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="16597-185">Om du behöver tooretrieve hello klient-ID, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-185">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="16597-186">Om du behöver tooretrieve hello program-ID, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-186">If you need tooretrieve hello application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a><span data-ttu-id="16597-187">Ändra autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="16597-187">Change credentials</span></span>

<span data-ttu-id="16597-188">toochange hello autentiseringsuppgifter för en AD-app, antingen på grund av ett säkerhetsintrång eller en autentiseringsuppgift förfallodatum, använda hello [ta bort AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) och [ny AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.</span><span class="sxs-lookup"><span data-stu-id="16597-188">toochange hello credentials for an AD app, either because of a security compromise or a credential expiration, use hello [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) and [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.</span></span>

<span data-ttu-id="16597-189">tooremove alla hello autentiseringsuppgifter för ett program, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-189">tooremove all hello credentials for an application, use:</span></span>

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

<span data-ttu-id="16597-190">tooadd ett lösenord, Använd:</span><span class="sxs-lookup"><span data-stu-id="16597-190">tooadd a password, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

<span data-ttu-id="16597-191">tooadd ett Certifikatvärde, skapa ett självsignerat certifikat som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="16597-191">tooadd a certificate value, create a self-signed certificate as shown in this topic.</span></span> <span data-ttu-id="16597-192">Använd sedan:</span><span class="sxs-lookup"><span data-stu-id="16597-192">Then, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-toosimplify-log-in"></a><span data-ttu-id="16597-193">Spara åtkomst-token toosimplify logga in</span><span class="sxs-lookup"><span data-stu-id="16597-193">Save access token toosimplify log in</span></span>
<span data-ttu-id="16597-194">tooavoid tillhandahåller hello service principal autentiseringsuppgifterna varje gång den måste toolog i, kan du spara hello åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="16597-194">tooavoid providing hello service principal credentials every time it needs toolog in, you can save hello access token.</span></span>

<span data-ttu-id="16597-195">toouse hello aktuella åtkomst-token i en senare session, spara hello profil.</span><span class="sxs-lookup"><span data-stu-id="16597-195">toouse hello current access token in a later session, save hello profile.</span></span>
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
<span data-ttu-id="16597-196">Öppna hello-profilen och undersöka dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="16597-196">Open hello profile and examine its contents.</span></span> <span data-ttu-id="16597-197">Observera att den innehåller en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="16597-197">Notice that it contains an access token.</span></span> <span data-ttu-id="16597-198">I stället för att manuellt logga in igen, bara läsa hello profilen.</span><span class="sxs-lookup"><span data-stu-id="16597-198">Instead of manually logging in again, simply load hello profile.</span></span>
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> <span data-ttu-id="16597-199">hello åtkomst-token upphör att gälla, så att använda en sparad profil fungerar bara för så länge hello token är giltig.</span><span class="sxs-lookup"><span data-stu-id="16597-199">hello access token expires, so using a saved profile only works for as long as hello token is valid.</span></span>
>  

<span data-ttu-id="16597-200">Alternativt kan du anropa REST-åtgärder från PowerShell toolog i.</span><span class="sxs-lookup"><span data-stu-id="16597-200">Alternatively, you can invoke REST operations from PowerShell toolog in.</span></span> <span data-ttu-id="16597-201">Du kan hämta hello åtkomsttoken för användning med andra åtgärder från hello autentiseringssvar.</span><span class="sxs-lookup"><span data-stu-id="16597-201">From hello authentication response, you can retrieve hello access token for use with other operations.</span></span> <span data-ttu-id="16597-202">Ett exempel för att hämta hello åtkomst-token genom att anropa REST-åtgärder finns [genererar en åtkomst-Token](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="16597-202">For an example of retrieving hello access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="debug"></a><span data-ttu-id="16597-203">Felsökning</span><span class="sxs-lookup"><span data-stu-id="16597-203">Debug</span></span>

<span data-ttu-id="16597-204">Du kan stöta på hello följande fel när du skapar ett huvudnamn för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="16597-204">You may encounter hello following errors when creating a service principal:</span></span>

* <span data-ttu-id="16597-205">**”Authentication_Unauthorized”** eller **”ingen prenumeration finns i kontexten för hello”.**</span><span class="sxs-lookup"><span data-stu-id="16597-205">**"Authentication_Unauthorized"** or **"No subscription found in hello context."**</span></span> <span data-ttu-id="16597-206">– Du ser det här felet när ditt konto inte har hello [nödvändiga behörigheter](#required-permissions) på hello Azure Active Directory tooregister en app.</span><span class="sxs-lookup"><span data-stu-id="16597-206">- You see this error when your account does not have hello [required permissions](#required-permissions) on hello Azure Active Directory tooregister an app.</span></span> <span data-ttu-id="16597-207">Normalt ser du det här felet när endast administrativa användare i Azure Active Directory kan du registrera appar och ditt konto är inte en administratör. Be din administratör tooeither tilldela tooan administratörsroll eller tooenable användare tooregister appar.</span><span class="sxs-lookup"><span data-stu-id="16597-207">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

* <span data-ttu-id="16597-208">Ditt konto **”har inte tillstånd tooperform åtgärd 'Microsoft.Authorization/roleAssignments/write' över område '/subscriptions/ {guid}'”. ** -Det här felet visas när ditt konto inte har tillräckliga behörigheter tooassign en roll tooan identitet.</span><span class="sxs-lookup"><span data-stu-id="16597-208">Your account **"does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions tooassign a role tooan identity.</span></span> <span data-ttu-id="16597-209">Be din prenumeration administratören tooadd du tooUser åtkomst rollen Administratör.</span><span class="sxs-lookup"><span data-stu-id="16597-209">Ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="16597-210">Exempelprogram</span><span class="sxs-lookup"><span data-stu-id="16597-210">Sample applications</span></span>
<span data-ttu-id="16597-211">För information om att logga in som hello program via olika plattformar, se:</span><span class="sxs-lookup"><span data-stu-id="16597-211">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="16597-212">.NET</span><span class="sxs-lookup"><span data-stu-id="16597-212">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="16597-213">Java</span><span class="sxs-lookup"><span data-stu-id="16597-213">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="16597-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="16597-214">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="16597-215">Python</span><span class="sxs-lookup"><span data-stu-id="16597-215">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="16597-216">Ruby</span><span class="sxs-lookup"><span data-stu-id="16597-216">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="16597-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16597-217">Next steps</span></span>
* <span data-ttu-id="16597-218">Detaljerade anvisningar för att integrera ett program i Azure för att hantera resurser, se [Developer's guide tooauthorization med hello Azure Resource Manager API](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="16597-218">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="16597-219">En mer detaljerad förklaring av program och tjänstens huvudnamn finns [program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="16597-219">For a more detailed explanation of applications and service principals, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span> 
* <span data-ttu-id="16597-220">Läs mer om Azure Active Directory-autentisering, [Autentiseringsscenarier för Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="16597-220">For more information about Azure Active Directory authentication, see [Authentication Scenarios for Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span></span>
* <span data-ttu-id="16597-221">En lista över tillgängliga åtgärder som kan beviljas eller nekas toousers finns [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="16597-221">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>

