---
title: "aaaCreate Azure Automation kör som-konton | Microsoft Docs"
description: "Den här artikeln beskriver hur tooupdate ditt Automation-kontot och skapa kör som-konton med PowerShell eller från hello-portalen."
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
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 94eb54fa0b518056a726d17146c63411e248273b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-automation-account-authentication-with-run-as-accounts"></a><span data-ttu-id="4d393-103">Uppdatera autentiseringen av ditt Automation-konto med Kör som-konton</span><span class="sxs-lookup"><span data-stu-id="4d393-103">Update your Automation account authentication with Run As accounts</span></span> 
<span data-ttu-id="4d393-104">Du kan uppdatera din befintliga Automation-konto från portalen hello eller använda PowerShell om:</span><span class="sxs-lookup"><span data-stu-id="4d393-104">You can update your existing Automation account from hello portal or use PowerShell if:</span></span>

* <span data-ttu-id="4d393-105">Du skapar ett Automation-konto men neka toocreate hello kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-105">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="4d393-106">Du använder redan en Automation-konto toomanage Resource Manager-resurser och du vill tooupdate hello konto tooinclude hello kör som-konto för runbook-autentisering.</span><span class="sxs-lookup"><span data-stu-id="4d393-106">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="4d393-107">Du redan använder en Automation-konto toomanage klassiska resurser och du vill tooupdate den toouse hello klassiska kör som-konto i stället för att skapa ett nytt konto och migrera dina runbook-flöden och tillgångar tooit.</span><span class="sxs-lookup"><span data-stu-id="4d393-107">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="4d393-108">Vill du toocreate kör som och klassiska kör som-konto med hjälp av ett certifikat utfärdat av enterprise-certifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="4d393-108">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4d393-109">Krav</span><span class="sxs-lookup"><span data-stu-id="4d393-109">Prerequisites</span></span>

* <span data-ttu-id="4d393-110">hello-skript kan köras endast på Windows 10 och Windows Server 2016 med Azure Resource Manager moduler 3.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="4d393-110">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 3.0.0 and later.</span></span> <span data-ttu-id="4d393-111">Det stöds inte i tidigare versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="4d393-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="4d393-112">Azure PowerShell 1.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="4d393-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="4d393-113">Information om hello PowerShell 1.0-versionen finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="4d393-113">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>
* <span data-ttu-id="4d393-114">Ett Automation-konto som refereras till som hello värde hello *– AutomationAccountName* och *- ApplicationDisplayName* parametrar i hello följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="4d393-114">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="4d393-115">tooget hello värden för *SubscriptionID*, *ResourceGroup*, och *AutomationAccountName*, som är nödvändiga parametrar för hello skript, hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d393-115">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello script, do hello following:</span></span>

1. <span data-ttu-id="4d393-116">I hello Azure-portalen, Välj ditt Automation-konto på hello **automatiseringskontot** bladet och väljer sedan **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="4d393-116">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span>  
2. <span data-ttu-id="4d393-117">På hello **alla inställningar** bladet under **kontoinställningar**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="4d393-117">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="4d393-118">Observera hello värden på hello **egenskaper** bladet.</span><span class="sxs-lookup"><span data-stu-id="4d393-118">Note hello values on hello **Properties** blade.</span></span><br><br> <span data-ttu-id="4d393-119">![hello Automation-konto ”egenskaper” bladet](media/automation-create-runas-account/automation-account-properties.png)</span><span class="sxs-lookup"><span data-stu-id="4d393-119">![hello Automation account "Properties" blade](media/automation-create-runas-account/automation-account-properties.png)</span></span>  

### <a name="required-permissions-tooupdate-your-automation-account"></a><span data-ttu-id="4d393-120">Krävs behörigheter tooupdate ditt Automation-konto</span><span class="sxs-lookup"><span data-stu-id="4d393-120">Required permissions tooupdate your Automation account</span></span>
<span data-ttu-id="4d393-121">tooupdate ett Automation-konto, måste du ha följande specifika privilegier hello och behörigheter krävs toocomplete det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4d393-121">tooupdate an Automation account, you must have hello following specific privileges and permissions required toocomplete this topic.</span></span>   
 
* <span data-ttu-id="4d393-122">AD-användarkontot måste toobe tillagda tooa roll med behörigheter motsvarande toohello deltagarrollen för Microsoft.Automation resurser som beskrivs i artikel [rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span><span class="sxs-lookup"><span data-stu-id="4d393-122">Your AD user account needs toobe added tooa role with permissions equivalent toohello Contributor role for Microsoft.Automation resources as outlined in article [Role-based access control in Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span></span>  
* <span data-ttu-id="4d393-123">Icke-administratörer i din Azure AD-klient kan [registrera AD-program](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) om hello App registreringar inställning har angetts för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="4d393-123">Non-admin users in your Azure AD tenant can [register AD applications](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) if hello App registrations setting is set too**Yes**.</span></span>  <span data-ttu-id="4d393-124">Om hello app registreringar inställning har angetts för**nr**, hello-användaren som utför den här åtgärden måste vara en global administratör i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d393-124">If hello app registrations setting is set too**No**, hello user performing this action must be a global administrator in Azure AD.</span></span> 

<span data-ttu-id="4d393-125">Om du inte är medlem i Active Directory-instans för hello prenumeration innan du läggs toohello global administratör/co-administrator roll hello prenumeration, läggs tooActive Directory som en gäst.</span><span class="sxs-lookup"><span data-stu-id="4d393-125">If you are not a member of hello subscription’s Active Directory instance before you are added toohello global administrator/co-administrator role of hello subscription, you are added tooActive Directory as a guest.</span></span> <span data-ttu-id="4d393-126">I så fall kan få du en ”du har inte behörighet toocreate...”</span><span class="sxs-lookup"><span data-stu-id="4d393-126">In this situation, you receive a “You do not have permissions toocreate…”</span></span> <span data-ttu-id="4d393-127">varning för hello **lägga till Automation-konto** bladet.</span><span class="sxs-lookup"><span data-stu-id="4d393-127">warning on hello **Add Automation Account** blade.</span></span> <span data-ttu-id="4d393-128">Användare som har lagts till toohello global administratör/co-administrator rollen först kan tas bort från hello prenumeration Active Directory-instans och toomake lagts till igen dem en fullständig användare i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4d393-128">Users who were added toohello global administrator/co-administrator role first can be removed from hello subscription's Active Directory instance and re-added toomake them a full User in Active Directory.</span></span> <span data-ttu-id="4d393-129">tooverify i den här situationen från hello **Azure Active Directory** rutan i hello Azure portal, Välj **användare och grupper**väljer **alla användare** och, när du har valt hello specifik användare, markerar du **profil**.</span><span class="sxs-lookup"><span data-stu-id="4d393-129">tooverify this situation, from hello **Azure Active Directory** pane in hello Azure portal, select **Users and groups**, select **All users** and, after you select hello specific user, select **Profile**.</span></span> <span data-ttu-id="4d393-130">Hej värdet för hello **användartyp** attributet under hello användare profil ska inte vara lika med **gäst**.</span><span class="sxs-lookup"><span data-stu-id="4d393-130">hello value of hello **User type** attribute under hello users profile should not equal **Guest**.</span></span>

## <a name="create-run-as-account-from-hello-portal"></a><span data-ttu-id="4d393-131">Skapa kör som-konto från hello-portalen</span><span class="sxs-lookup"><span data-stu-id="4d393-131">Create Run As account from hello portal</span></span>
<span data-ttu-id="4d393-132">Utför följande steg tooupdate hello Azure Automation-konto i det här avsnittet från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4d393-132">In this section, perform hello following steps tooupdate your Azure Automation account from  hello Azure portal.</span></span>  <span data-ttu-id="4d393-133">Du skapar hello kör som och klassiska kör som-konton individuellt och om du inte behöver toomanage resurser i hello klassiska Azure-portalen, kan du bara skapa hello Azure kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-133">You create hello Run As and Classic Run As accounts individually, and if you don't need toomanage resources in hello classic Azure portal, you can just create hello Azure Run As account.</span></span>  

<span data-ttu-id="4d393-134">hello process skapar hello följande objekt i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-134">hello process creates hello following items in your Automation account.</span></span>

<span data-ttu-id="4d393-135">**För Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="4d393-135">**For Run As accounts:**</span></span>

* <span data-ttu-id="4d393-136">Skapar ett Azure AD-program med ett självsignerat certifikat, skapar en tjänstens objektkonto för hello program i Azure AD och tilldelar hello deltagarrollen för hello konto i din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d393-136">Creates an Azure AD application with a self-signed certificate, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="4d393-137">Du kan ändra den här inställningen tooOwner eller någon annan roll.</span><span class="sxs-lookup"><span data-stu-id="4d393-137">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="4d393-138">Mer information finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="4d393-138">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="4d393-139">Skapar ett Automation-certifikattillgång med namnet *AzureRunAsCertificate* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-139">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-140">Hej certifikattillgång innehåller hello certifikatets privata nyckel som används av programmet hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d393-140">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="4d393-141">Skapar ett Automation-anslutningstillgång med namnet *AzureRunAsConnection* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-141">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-142">Hej anslutningstillgång innehåller hello applicationId, tenantId, prenumerations-ID och tumavtrycket för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4d393-142">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="4d393-143">**För klassiska Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="4d393-143">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="4d393-144">Skapar ett Automation-certifikattillgång med namnet *AzureClassicRunAsCertificate* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-144">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-145">Hej certifikattillgång innehåller hello certifikatets privata nyckel används av hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="4d393-145">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="4d393-146">Skapar ett Automation-anslutningstillgång med namnet *AzureClassicRunAsConnection* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-146">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-147">Hej anslutningstillgång innehåller hello prenumerationsnamn, prenumerations-ID och certifikatnamn för tillgången.</span><span class="sxs-lookup"><span data-stu-id="4d393-147">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

1. <span data-ttu-id="4d393-148">Logga in toohello Azure-portalen med ett konto som är medlem i rollen för hello Prenumerationsadministratörer och medadministratör för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d393-148">Sign in toohello Azure portal with an account that is a member of hello Subscription Admins role and co-administrator of hello subscription.</span></span>
2. <span data-ttu-id="4d393-149">Välj hello Automation-kontoblad **kör som-konton** under hello avsnittet **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="4d393-149">From hello Automation account blade, select **Run As Accounts** under hello section **Account Settings**.</span></span>  
3. <span data-ttu-id="4d393-150">Beroende på vilket konto du behöver väljer du antingen **Azures Kör som-konto** eller **Azures klassiska Kör som-konto**.</span><span class="sxs-lookup"><span data-stu-id="4d393-150">Depending on which account you require, select either **Azure Run As Account** or **Azure Classic Run As Account**.</span></span>  <span data-ttu-id="4d393-151">När du har valt antingen hello **lägga till Azure kör som** eller **lägga till Azure klassiska kör som-konto** bladet visas och när du har granskat hello översiktlig information på **skapa** tooproceed med skapa kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-151">After selecting either hello **Add Azure Run As** or **Add Azure Classic Run As Account** blade appears and after reviewing hello overview information, click **Create** tooproceed with Run As account creation.</span></span>  
4. <span data-ttu-id="4d393-152">Medan Azure skapar hello-kör som-konto, du kan följa förloppet för hello under **meddelanden** från hello-menyn och en banderoll visas om hello kontot skapas.</span><span class="sxs-lookup"><span data-stu-id="4d393-152">While Azure creates hello Run As account, you can track hello progress under **Notifications** from hello menu and a banner is displayed stating hello account is being created.</span></span>  <span data-ttu-id="4d393-153">Den här processen kan ta några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4d393-153">This process can take a few minutes toocomplete.</span></span>  

## <a name="create-run-as-account-using-powershell-script"></a><span data-ttu-id="4d393-154">Skapa ett Kör som-konto med ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="4d393-154">Create Run As account using PowerShell script</span></span>
<span data-ttu-id="4d393-155">Detta PowerShell-skript har stöd för hello följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="4d393-155">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="4d393-156">Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="4d393-156">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="4d393-157">Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="4d393-157">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="4d393-158">Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat.</span><span class="sxs-lookup"><span data-stu-id="4d393-158">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="4d393-159">Skapa ett kör som-konto och klassiska kör som-konto med hjälp av ett självsignerat certifikat i hello Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="4d393-159">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="4d393-160">Beroende på hello alternativ du väljer, skapar hello skriptet hello följande objekt.</span><span class="sxs-lookup"><span data-stu-id="4d393-160">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="4d393-161">**För Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="4d393-161">**For Run As accounts:**</span></span>

* <span data-ttu-id="4d393-162">Skapar en Azure AD application toobe exporteras med antingen hello självsignerat eller enterprise certifikatets offentliga nyckel, skapar en tjänstens objektkonto för hello program i Azure AD och tilldelar hello deltagarrollen för hello konto i din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d393-162">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="4d393-163">Du kan ändra den här inställningen tooOwner eller någon annan roll.</span><span class="sxs-lookup"><span data-stu-id="4d393-163">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="4d393-164">Mer information finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="4d393-164">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="4d393-165">Skapar ett Automation-certifikattillgång med namnet *AzureRunAsCertificate* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-165">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-166">Hej certifikattillgång innehåller hello certifikatets privata nyckel som används av programmet hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4d393-166">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="4d393-167">Skapar ett Automation-anslutningstillgång med namnet *AzureRunAsConnection* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-167">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-168">Hej anslutningstillgång innehåller hello applicationId, tenantId, prenumerations-ID och tumavtrycket för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4d393-168">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="4d393-169">**För klassiska Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="4d393-169">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="4d393-170">Skapar ett Automation-certifikattillgång med namnet *AzureClassicRunAsCertificate* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-170">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-171">Hej certifikattillgång innehåller hello certifikatets privata nyckel används av hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="4d393-171">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="4d393-172">Skapar ett Automation-anslutningstillgång med namnet *AzureClassicRunAsConnection* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4d393-172">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="4d393-173">Hej anslutningstillgång innehåller hello prenumerationsnamn, prenumerations-ID och certifikatnamn för tillgången.</span><span class="sxs-lookup"><span data-stu-id="4d393-173">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="4d393-174">Om du väljer något av alternativen för att skapa en klassisk kör som-konto när hello skriptet körs skapades överför hello offentliga certifikatarkiv (.cer-filnamnstillägget) toohello management för hello prenumeration som hello Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="4d393-174">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

1. <span data-ttu-id="4d393-175">Spara hello följande skript på datorn.</span><span class="sxs-lookup"><span data-stu-id="4d393-175">Save hello following script on your computer.</span></span> <span data-ttu-id="4d393-176">I det här exemplet spara om filen med hello filename *ny RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="4d393-176">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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
        $KeyCredential.EndDate = Get-Date $PfxCert.GetExpirationDateString()
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
        if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 0) -or ($AzureRMProfileVersion.Major -gt 3)))
        {
            Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
            return
        }

        Login-AzureRmAccount -Environment $EnvironmentName 
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
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="4d393-177">På datorn, startar **Windows PowerShell** från hello **starta** skärmen med utökade användarrättigheter.</span><span class="sxs-lookup"><span data-stu-id="4d393-177">On your computer, start **Windows PowerShell** from hello **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="4d393-178">Från hello utökade kommandoradsgränssnitt, gå toohello mapp som innehåller hello-skript som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="4d393-178">From hello elevated command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>  
4. <span data-ttu-id="4d393-179">Kör hello-skript med hjälp av hello parametervärden för hello-konfiguration som du behöver.</span><span class="sxs-lookup"><span data-stu-id="4d393-179">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="4d393-180">**Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat**</span><span class="sxs-lookup"><span data-stu-id="4d393-180">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="4d393-181">**Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat**</span><span class="sxs-lookup"><span data-stu-id="4d393-181">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="4d393-182">**Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat**</span><span class="sxs-lookup"><span data-stu-id="4d393-182">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="4d393-183">**Skapa ett kör som-konto och klassiska kör som-konto med hjälp av ett självsignerat certifikat i hello Azure Government-moln**</span><span class="sxs-lookup"><span data-stu-id="4d393-183">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="4d393-184">Du kommer att tillfrågas tooauthenticate med Azure när hello skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="4d393-184">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="4d393-185">Logga in med ett konto som är medlem i administratörsrollen i hello prenumeration och medadministratör för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d393-185">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="4d393-186">Observera följande hello efter hello skriptet har körts:</span><span class="sxs-lookup"><span data-stu-id="4d393-186">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="4d393-187">Om du har skapat en klassiska kör som-konto med ett självsignerat certifikat med offentlig (.cer-fil) hello skriptet skapar och sparar toohello tillfällig mapp på datorn under hello användarprofil *%USERPROFILE%\AppData\Local\Temp*, som du använde tooexecute hello PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="4d393-187">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="4d393-188">Om du skapade ett klassiskt Kör som-konto med ett offentligt företagscertifikat (CER-fil) använder du det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4d393-188">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="4d393-189">Följ anvisningarna för hello för [överför en hanterings-API certifikat toohello klassiska Azure-portalen](../azure-api-management-certs.md), och sedan Validera hello behörighetskonfigurationen med klassisk distributionsresurser med hjälp av hello [exempelkod tooauthenticate med klassiska Azure-distributionsresurser](automation-verify-runas-authentication.md#classic-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="4d393-189">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with classic deployment resources by using hello [sample code tooauthenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="4d393-190">Om du har gjort *inte* skapa klassiska kör som-konton, autentisera med Resource Manager-resurser och validera hello autentiseringsuppgifter konfigurationen med hjälp av hello [exempelkod för att autentisera med Service Management resurser](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="4d393-190">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d393-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d393-191">Next steps</span></span>
* <span data-ttu-id="4d393-192">Mer information om tjänstens huvudnamn finns för[program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="4d393-192">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="4d393-193">Mer information om certifikat och Azure-tjänster finns för[certifikat översikt för Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="4d393-193">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
