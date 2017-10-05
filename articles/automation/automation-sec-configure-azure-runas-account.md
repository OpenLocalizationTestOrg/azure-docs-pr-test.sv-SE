---
title: "Konfigurera ett Kör som-konto i Azure | Microsoft Docs"
description: "Den här självstudien beskriver steg för steg hur du skapar, testar och använder autentisering med säkerhetsobjekt i Azure Automation."
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
redirect_document_id: TRUE
ms.openlocfilehash: f88c775eba19bb227d0e206d6420c08b57305b92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a><span data-ttu-id="7d217-104">Autentisera runbookflöden med ett Kör som-konto i Azure</span><span class="sxs-lookup"><span data-stu-id="7d217-104">Authenticate runbooks with an Azure Run As account</span></span>
<span data-ttu-id="7d217-105">Den här artikeln beskriver hur du konfigurerar ett Azure Automation-konto på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-105">This article shows you how to configure an Azure Automation account in the Azure portal.</span></span> <span data-ttu-id="7d217-106">Du gör detta genom att använda funktionen Kör som-konto för att autentisera runbookflöden som hanterar resurser i Azure Resource Manager eller Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="7d217-106">To do so, you use the Run As account feature to authenticate runbooks managing resources in either Azure Resource Manager or Azure Service Management.</span></span>

<span data-ttu-id="7d217-107">När du skapar ett Automation-konto på Azure Portal skapar du automatiskt två konton:</span><span class="sxs-lookup"><span data-stu-id="7d217-107">When you create an Automation account in the Azure portal, you automatically create two accounts:</span></span>

* <span data-ttu-id="7d217-108">Ett Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-108">A Run As account.</span></span> <span data-ttu-id="7d217-109">Det här kontot skapar ett tjänstobjekt i Azure Active Directory (AD Azure) och ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="7d217-109">This account creates a service principal in Azure Active Directory (Azure AD) and a certificate.</span></span> <span data-ttu-id="7d217-110">Det tilldelar också den rollbaserade åtkomstkontrollen (RBAC) för deltagare, som hanterar Resource Manager-resurser med hjälp av runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-110">It also assigns the Contributor role-based access control (RBAC), which manages Resource Manager resources by using runbooks.</span></span>
* <span data-ttu-id="7d217-111">Ett klassiskt Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-111">A Classic Run As account.</span></span> <span data-ttu-id="7d217-112">Det här kontot överför ett hanteringscertifikat som används för att hantera Service Management-resurser eller klassiska resurser med hjälp av runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-112">This account uploads a management certificate, which is used to manage Service Management or classic resources by using runbooks.</span></span>

<span data-ttu-id="7d217-113">Genom att skapa ett Automation-konto kan du underlätta processen och snabbt börja skapa och distribuera runbookflöden för dina automatiseringsbehov.</span><span class="sxs-lookup"><span data-stu-id="7d217-113">Creating an Automation account simplifies the process for you and helps you quickly start building and deploying runbooks to support your automation needs.</span></span>

<span data-ttu-id="7d217-114">Med Kör som-konton och klassiska Kör som-konton kan du:</span><span class="sxs-lookup"><span data-stu-id="7d217-114">With Run As and Classic Run As accounts, you can:</span></span>

* <span data-ttu-id="7d217-115">Tillhandahålla en standardiserad metod för att autentisera med Azure när du hanterar Resource Manager- eller Service Management-resurser från runbookflöden på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-115">Provide a standardized way to authenticate with Azure when you manage Resource Manager or Service Management resources from runbooks in the Azure portal.</span></span>
* <span data-ttu-id="7d217-116">Automatisera användningen av globala runbook som du kan konfigurera i Azure Alerts.</span><span class="sxs-lookup"><span data-stu-id="7d217-116">Automate the use of global runbooks, which you can configure in Azure Alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="7d217-117">För [integreringsfunktionen för Azure-aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) med globala Automation-runbookflöden krävs ett Automation-konto som konfigurerats med ett Kör som-konto och ett klassiskt Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-117">The [Azure Alert integration feature](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) with Automation global runbooks requires an Automation account that's configured with a Run As account and a Classic Run As account.</span></span> <span data-ttu-id="7d217-118">Du kan välja ett Automation-konto som redan har definierade Kör som-konton och klassiska Kör som-konton eller välja att skapa ett nytt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-118">You can select an Automation account that already has defined Run As and Classic Run As accounts, or you can choose to create a new Automation account.</span></span>
>  

<span data-ttu-id="7d217-119">Den här artikeln beskriver hur du skapar ett Automation-konto från Azure Portal, hur du uppdaterar ett Automation-konto med hjälp av Azure PowerShell, hur hanterar kontokonfigurationen samt hur du autentiserar i dina runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-119">This article shows how to create an Automation account from the Azure portal, update an Automation account by using Azure PowerShell, manage the account configuration, and authenticate in your runbooks.</span></span>

<span data-ttu-id="7d217-120">Innan du skapar ett Automation-konto är det viktigt att du förstår och tänker på följande:</span><span class="sxs-lookup"><span data-stu-id="7d217-120">Before you begin creating an Automation account, it's a good idea to understand and consider the following:</span></span>

* <span data-ttu-id="7d217-121">Nya Automation-konton som du skapar påverkar inte Automation-konton som du redan har skapat i den klassiska distributionsmodellen eller i Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7d217-121">Creating an Automation account does not affect Automation accounts you might have already created in either the classic or Resource Manager deployment model.</span></span>
* <span data-ttu-id="7d217-122">Processen fungerar endast för Automation-konton som du skapar på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-122">The process works only for Automation accounts that you create in the Azure portal.</span></span> <span data-ttu-id="7d217-123">Om du försöker skapa ett konto från den klassiska Azure-portalen så replikeras inte Kör som-kontokonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-123">Attempting to create an account from the Azure classic portal does not replicate the Run As account configuration.</span></span>
* <span data-ttu-id="7d217-124">Om du redan har runbookflöden och tillgångar (till exempel scheman eller variabler) för att hantera klassiska resurser och du vill att dina runbookflöden ska autentisera med det nya klassiska Kör som-kontot, gör du något av följande:</span><span class="sxs-lookup"><span data-stu-id="7d217-124">If you already have runbooks and assets (such as schedules or variables) in place to manage classic resources, and you want runbooks to authenticate with the new Classic Run As account, do either of the following:</span></span>

  * <span data-ttu-id="7d217-125">Om du vill skapa ett klassiskt Kör som-konto följer du anvisningarna i avsnittet ”Hantera ett Kör som-konto”.</span><span class="sxs-lookup"><span data-stu-id="7d217-125">To create a Classic Run As account, follow the instructions in the "Managing your Run As account" section.</span></span> 
  * <span data-ttu-id="7d217-126">Om du vill uppdatera ett befintligt konto använder du PowerShell-skriptet i avsnittet ”Uppdatera ett Automation-konto med hjälp av PowerShell”.</span><span class="sxs-lookup"><span data-stu-id="7d217-126">To update your existing account, use the PowerShell script in the "Update your Automation account by using PowerShell" section.</span></span>
* <span data-ttu-id="7d217-127">För att autentisera med det nya Kör som-kontot och det klassiska Kör som-kontot för Automation måste du ändra dina befintliga runbookflöden med hjälp av exempelkoden i avsnittet [Exempel på autentiseringskod](#authentication-code-examples).</span><span class="sxs-lookup"><span data-stu-id="7d217-127">To authenticate by using the new Run As account and Classic Run As Automation account, you  need to modify your existing runbooks with the example code provided in the section [Authentication code examples](#authentication-code-examples).</span></span>

    >[!NOTE]
    ><span data-ttu-id="7d217-128">Kör som-kontot används för autentisering mot Resource Manager-resurser med hjälp av det certifikatbaserade tjänstobjektet.</span><span class="sxs-lookup"><span data-stu-id="7d217-128">The Run As account is for authentication against Resource Manager resources using the certificate-based service principal.</span></span> <span data-ttu-id="7d217-129">Det klassisk Kör som-kontot används för att autentisera mot Service Management-resurser med ett hanteringscertifikat.</span><span class="sxs-lookup"><span data-stu-id="7d217-129">The Classic Run As account is for authenticating against Service Management resources with a management certificate.</span></span>

## <a name="create-an-automation-account-from-the-azure-portal"></a><span data-ttu-id="7d217-130">Skapa ett Automation-konto från Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7d217-130">Create an Automation account from the Azure portal</span></span>
<span data-ttu-id="7d217-131">I det här avsnittet skapar du ett Azure Automation-konto från Azure Portal, vilket i sin tur skapar både ett Kör som-konto och ett klassiskt Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-131">In this section, you create an Azure Automation account from the Azure portal, which in turn creates both a Run As account and a Classic Run As account.</span></span>

>[!NOTE]
><span data-ttu-id="7d217-132">För att kunna skapa ett Automation-konto måste du vara medlem i rollen Tjänstadministratörer eller vara medadministratör för den prenumeration som ger åtkomst till prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-132">To create an Automation account, you must be a member of the Service Admins role or co-administrator of the subscription that is granting access to the subscription.</span></span> <span data-ttu-id="7d217-133">Du måste också läggas till som användare i prenumerationens Active Directory-standardinstans.</span><span class="sxs-lookup"><span data-stu-id="7d217-133">You must also be added as a user to that subscription's default Active Directory instance.</span></span> <span data-ttu-id="7d217-134">Kontot behöver inte tilldelas en privilegierad roll.</span><span class="sxs-lookup"><span data-stu-id="7d217-134">The account does not need to be assigned a privileged role.</span></span>
>
><span data-ttu-id="7d217-135">Om du inte är medlem i prenumerationens Active Directory-instans innan du läggs till i rollen som medadministratör för prenumerationen, läggs du till i Active Directory som gäst.</span><span class="sxs-lookup"><span data-stu-id="7d217-135">If you are not a member of the subscription’s Active Directory instance before you are added to the co-administrator role of the subscription, you will be added to Active Directory as a guest.</span></span> <span data-ttu-id="7d217-136">I detta fall visas varningen ”Du har inte behörighet att skapa ...”</span><span class="sxs-lookup"><span data-stu-id="7d217-136">In this instance, you will receive a “You do not have permissions to create…”</span></span> <span data-ttu-id="7d217-137">på bladet **Lägg till Automation-konto**.</span><span class="sxs-lookup"><span data-stu-id="7d217-137">warning on the **Add Automation Account** blade.</span></span>
>
><span data-ttu-id="7d217-138">Användare som har lagts till i rollen som medadministratör kan först tas bort från prenumerationens Active Directory-instans och sedan läggas till igen så att de blir fullständiga användare i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7d217-138">Users who were added to the co-administrator role first can be removed from the subscription's Active Directory instance and re-added to make them a full User in Active Directory.</span></span> <span data-ttu-id="7d217-139">Du kan kontrollera detta i rutan **Azure Active Directory** på Azure Portal genom att välja **Användare och grupper**, välja **Alla användare**, välja den specifika användaren och sedan välja **Profil**.</span><span class="sxs-lookup"><span data-stu-id="7d217-139">To verify this situation from the **Azure Active Directory** pane in the Azure portal by selecting **Users and groups**, selecting **All users** and, after you select the specific user, selecting **Profile**.</span></span> <span data-ttu-id="7d217-140">Värdet för attributet **Användartyp** under användarens profil bör inte vara lika med **Gäst**.</span><span class="sxs-lookup"><span data-stu-id="7d217-140">The value of the **User type** attribute under the users profile should not equal **Guest**.</span></span>
>

1. <span data-ttu-id="7d217-141">Logga in på Azure Portal med ett konto som är medlem i rollen för prenumerationsadministratörer och som är medadministratör för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-141">Sign in to the Azure portal with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>

2. <span data-ttu-id="7d217-142">Välj **Automation-konton**.</span><span class="sxs-lookup"><span data-stu-id="7d217-142">Select **Automation Accounts**.</span></span>

3. <span data-ttu-id="7d217-143">Klicka på **Lägg till** på bladet **Automation-konton**.</span><span class="sxs-lookup"><span data-stu-id="7d217-143">On the **Automation Accounts** blade, click **Add**.</span></span>
<span data-ttu-id="7d217-144">Bladet **Lägg till Automation-konto** öppnas.</span><span class="sxs-lookup"><span data-stu-id="7d217-144">The **Add Automation Account** blade opens.</span></span>

 ![Bladet ”Lägg till Automation-konto”](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > <span data-ttu-id="7d217-146">Om ditt konto inte är medlem i rollen för prenumerationsadministratörer och medadministratör för prenumerationen visas följande varning på bladet **Lägg till Automation-konto**:</span><span class="sxs-lookup"><span data-stu-id="7d217-146">If your account is not a member of the subscription administrators role and co-administrator of the subscription, the following warning is displayed on the **Add Automation Account** blade:</span></span>
   >
   >![Varningsmeddelande för Lägg till Automation-konto](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. <span data-ttu-id="7d217-148">På bladet **Lägg till Automation-konto** skriver du namnet på det nya Automation-kontot i rutan **Namn**.</span><span class="sxs-lookup"><span data-stu-id="7d217-148">On the **Add Automation Account** blade, in the **Name** box, type a name for your new Automation account.</span></span>

5. <span data-ttu-id="7d217-149">Om du har mer än en prenumeration gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7d217-149">If you have more than one subscription, do the following:</span></span>

    <span data-ttu-id="7d217-150">a.</span><span class="sxs-lookup"><span data-stu-id="7d217-150">a.</span></span> <span data-ttu-id="7d217-151">Under **Prenumeration** anger du en prenumeration för det nya kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-151">Under **Subscription**, specify one for the new account.</span></span>

    <span data-ttu-id="7d217-152">b.</span><span class="sxs-lookup"><span data-stu-id="7d217-152">b.</span></span> <span data-ttu-id="7d217-153">Under **Resursgrupp** klickar du på **Skapa ny** eller på **Använd befintlig**.</span><span class="sxs-lookup"><span data-stu-id="7d217-153">Under **Resource Group**, click **Create new** or **Use existing**.</span></span>

    <span data-ttu-id="7d217-154">c.</span><span class="sxs-lookup"><span data-stu-id="7d217-154">c.</span></span> <span data-ttu-id="7d217-155">Under **Plats** anger du ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="7d217-155">Under **Location**, specify an Azure datacenter.</span></span>

6. <span data-ttu-id="7d217-156">Under **Skapa Kör som-konto i Azure** väljer du **Ja** och klickar sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7d217-156">Under **Create Azure Run As account**, select **Yes**, and then click **Create**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="7d217-157">Om du väljer att inte skapa Kör som-kontot genom att välja **Nej** visas ett varningsmeddelande på bladet **Lägg till Automation-konto**.</span><span class="sxs-lookup"><span data-stu-id="7d217-157">If you choose not to create the Run As account by selecting **No**, a warning message is displayed the **Add Automation Account** blade.</span></span> <span data-ttu-id="7d217-158">Även om kontot skapas på Azure Portal har det ingen motsvarande autentiseringsidentitet i katalogtjänsten för din klassiska prenumeration eller Resource Manager-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7d217-158">Although the account is created in the Azure portal, it does not have a corresponding authentication identity within your classic or Resource Manager subscription directory service.</span></span> <span data-ttu-id="7d217-159">Kontot har därför inte åtkomst till resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7d217-159">Consequently, the account has no access to resources in your subscription.</span></span> <span data-ttu-id="7d217-160">Det här scenariot förhindrar att runbookflöden som refererar till det här kontot autentiseras och utför åtgärder mot resurser i dessa distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="7d217-160">This scenario prevents any runbooks that reference this account from authenticating and performing tasks against resources in those deployment models.</span></span>
   >
   > ![Varningsmeddelande på bladet ”Lägg till Automation-konto”](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > <span data-ttu-id="7d217-162">Dessutom, eftersom tjänstobjektet inte skapas, så tilldelas inte rollen Deltagare.</span><span class="sxs-lookup"><span data-stu-id="7d217-162">Additionally, because the service principal is not created, the Contributor role is not assigned.</span></span>
   >

7. <span data-ttu-id="7d217-163">Medan Azure skapar Automation-kontot kan du följa förloppet under **Meddelanden** på menyn.</span><span class="sxs-lookup"><span data-stu-id="7d217-163">While Azure creates the Automation account, you can track the progress under **Notifications** from the menu.</span></span>

### <a name="resources"></a><span data-ttu-id="7d217-164">Resurser</span><span class="sxs-lookup"><span data-stu-id="7d217-164">Resources</span></span>
<span data-ttu-id="7d217-165">När Automation-kontot har skapats skapas flera resurser automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7d217-165">When the Automation account is successfully created, several resources are automatically created for you.</span></span> <span data-ttu-id="7d217-166">Resurserna sammanfattas i följande två tabeller:</span><span class="sxs-lookup"><span data-stu-id="7d217-166">The resources are summarized in the following two tables:</span></span>

#### <a name="run-as-account-resources"></a><span data-ttu-id="7d217-167">Resurser för Kör som-konton</span><span class="sxs-lookup"><span data-stu-id="7d217-167">Run As account resources</span></span>

| <span data-ttu-id="7d217-168">Resurs</span><span class="sxs-lookup"><span data-stu-id="7d217-168">Resource</span></span> | <span data-ttu-id="7d217-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d217-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7d217-170">AzureAutomationTutorial-runbook</span><span class="sxs-lookup"><span data-stu-id="7d217-170">AzureAutomationTutorial Runbook</span></span> | <span data-ttu-id="7d217-171">Ett exempel på en grafisk runbook som visar hur du autentiserar med hjälp av Kör som-kontot och hur du hämtar alla Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="7d217-171">An example graphical runbook that demonstrates how to authenticate by using the Run As account and gets all the Resource Manager resources.</span></span> |
| <span data-ttu-id="7d217-172">AzureAutomationTutorialScript-runbook</span><span class="sxs-lookup"><span data-stu-id="7d217-172">AzureAutomationTutorialScript Runbook</span></span> | <span data-ttu-id="7d217-173">Ett exempel på en PowerShell-runbook som visar hur du autentiserar med hjälp av Kör som-kontot och hur du hämtar alla Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="7d217-173">An example PowerShell runbook that demonstrates how to authenticate by using the Run As account and gets all the Resource Manager resources.</span></span> |
| <span data-ttu-id="7d217-174">AzureRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="7d217-174">AzureRunAsCertificate</span></span> | <span data-ttu-id="7d217-175">Certifikattillgången som skapas automatiskt när du skapar ett Automation-konto eller använder följande PowerShell-skript för ett befintligt konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-175">The certificate asset that's automatically created when you create an Automation account or use the following PowerShell script for an existing account.</span></span> <span data-ttu-id="7d217-176">Certifikatet gör att du kan autentisera med Azure så att du kan hantera Azure Resource Manager-resurser från runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-176">The certificate allows you to authenticate with Azure so that you can manage Azure Resource Manager resources from runbooks.</span></span> <span data-ttu-id="7d217-177">Certifikatet har en livslängd på ett år.</span><span class="sxs-lookup"><span data-stu-id="7d217-177">The certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="7d217-178">AzureRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="7d217-178">AzureRunAsConnection</span></span> | <span data-ttu-id="7d217-179">Anslutningstillgången som skapas automatiskt när du skapar ett Automation-konto eller använder PowerShell-skriptet för ett befintligt konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-179">The connection asset that's automatically created when you create an Automation account or use the PowerShell script for an existing account.</span></span> |

#### <a name="classic-run-as-account-resources"></a><span data-ttu-id="7d217-180">Resurser för klassiska Kör som-konton</span><span class="sxs-lookup"><span data-stu-id="7d217-180">Classic Run As account resources</span></span>

| <span data-ttu-id="7d217-181">Resurs</span><span class="sxs-lookup"><span data-stu-id="7d217-181">Resource</span></span> | <span data-ttu-id="7d217-182">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d217-182">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7d217-183">AzureClassicAutomationTutorial-runbook</span><span class="sxs-lookup"><span data-stu-id="7d217-183">AzureClassicAutomationTutorial Runbook</span></span> | <span data-ttu-id="7d217-184">Ett exempel på en grafisk runbook som hämtar alla virtuella datorer som skapas med den klassiska distributionsmodellen i en prenumeration med hjälp av det klassiska Kör som-kontot (certifikat) och som sedan skriver namnet och statusen för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="7d217-184">An example graphical runbook that gets all the VMs that are created using the classic deployment model in a subscription by using the Classic Run As account (certificate), and then writes the VM name and status.</span></span> |
| <span data-ttu-id="7d217-185">AzureClassicAutomationTutorial Script-runbook</span><span class="sxs-lookup"><span data-stu-id="7d217-185">AzureClassicAutomationTutorial Script Runbook</span></span> | <span data-ttu-id="7d217-186">Ett exempel på en PowerShell-runbook som hämtar alla klassiska virtuella datorer i en prenumeration med hjälp av det klassiska Kör som-kontot (certifikat) och som sedan skriver namnet och statusen för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="7d217-186">An example PowerShell runbook that gets all the classic VMs in a subscription by using the Classic Run As account (certificate), and then writes the VM name and status.</span></span> |
| <span data-ttu-id="7d217-187">AzureClassicRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="7d217-187">AzureClassicRunAsCertificate</span></span> | <span data-ttu-id="7d217-188">Certifikattillgången som skapas automatiskt och som du använder för att autentisera med Azure så att du kan hantera klassiska Azure-resurser från runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-188">The automatically created certificate asset that you use to authenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> <span data-ttu-id="7d217-189">Certifikatet har en livslängd på ett år.</span><span class="sxs-lookup"><span data-stu-id="7d217-189">The certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="7d217-190">AzureClassicRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="7d217-190">AzureClassicRunAsConnection</span></span> | <span data-ttu-id="7d217-191">Anslutningstillgången som skapas automatiskt och som du använder för att autentisera med Azure så att du kan hantera klassiska Azure-resurser från runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-191">The automatically created connection asset that you use to authenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> |

## <a name="verify-run-as-authentication"></a><span data-ttu-id="7d217-192">Verifiera Kör som-autentisering</span><span class="sxs-lookup"><span data-stu-id="7d217-192">Verify Run As authentication</span></span>
<span data-ttu-id="7d217-193">Utför ett litet test för att bekräfta att du kan autentisera med hjälp av det nya Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-193">Perform a small test to confirm that you can successfully authenticate by using the new Run As account.</span></span>

1. <span data-ttu-id="7d217-194">Öppna Automation-kontot som du skapade tidigare på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-194">In the Azure portal, open the Automation account that you created earlier.</span></span>

2. <span data-ttu-id="7d217-195">Öppna listan med runbookflöden genom att klicka på panelen med **runbookflöden**.</span><span class="sxs-lookup"><span data-stu-id="7d217-195">Click the **Runbooks** tile to open the list of runbooks.</span></span>

3. <span data-ttu-id="7d217-196">Välj **AzureAutomationTutorialScript**-runbooken och klicka sedan på **Starta** för att starta runbooken.</span><span class="sxs-lookup"><span data-stu-id="7d217-196">Select the **AzureAutomationTutorialScript** runbook, and then click **Start** to start the runbook.</span></span> <span data-ttu-id="7d217-197">Följande sker:</span><span class="sxs-lookup"><span data-stu-id="7d217-197">The following events occur:</span></span>
 * <span data-ttu-id="7d217-198">Ett [runbook-jobb](automation-runbook-execution.md) skapas, bladet **Jobb** visas och jobbstatusen visas på panelen **Jobbsammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="7d217-198">A [runbook job](automation-runbook-execution.md) is created, the **Job** blade is displayed, and the job status is displayed in the **Job Summary** tile.</span></span>
 * <span data-ttu-id="7d217-199">Jobbets första status är **I kö** vilket betyder att det väntar på att en runbook-arbetsroll i molnet ska bli tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7d217-199">The job status begins as **Queued**, indicating that it is waiting for a runbook worker in the cloud to become available.</span></span>
 * <span data-ttu-id="7d217-200">Statusen ändras till **Startar** när ett arbetsobjekt begär jobbet.</span><span class="sxs-lookup"><span data-stu-id="7d217-200">The status becomes **Starting** when a worker claims the job.</span></span>
 * <span data-ttu-id="7d217-201">Statusen ändras till **Körs** när runbooken börjar köras.</span><span class="sxs-lookup"><span data-stu-id="7d217-201">The status becomes **Running** when the runbook starts running.</span></span>
 * <span data-ttu-id="7d217-202">När runbook-jobbet har körts bör statusen vara **Slutfört**.</span><span class="sxs-lookup"><span data-stu-id="7d217-202">When the runbook job has finished running, you should see a status of **Completed**.</span></span>

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. <span data-ttu-id="7d217-203">Om du vill visa ett detaljerat resultat av runbook-jobbet klickar du på panelen **Utdata**.</span><span class="sxs-lookup"><span data-stu-id="7d217-203">To see the detailed results of the runbook, click the **Output** tile.</span></span>  
<span data-ttu-id="7d217-204">Bladet **Utdata** visas och anger att runbooken har autentiserats och returnerat en lista över alla tillgängliga resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="7d217-204">The **Output** blade is displayed, showing that the runbook has successfully authenticated and returned a list of all resources available in the resource group.</span></span>

5. <span data-ttu-id="7d217-205">Stäng bladet **Utdata** och gå tillbaka till bladet **Jobbsammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="7d217-205">Close the **Output** blade to return to the **Job Summary** blade.</span></span>

6. <span data-ttu-id="7d217-206">Stäng bladet **Jobbsammanfattning** och motsvarande blad för **AzureAutomationTutorialScript**-runbooken.</span><span class="sxs-lookup"><span data-stu-id="7d217-206">Close the **Job Summary** blade and the corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="verify-classic-run-as-authentication"></a><span data-ttu-id="7d217-207">Verifiera klassisk Kör som-autentisering</span><span class="sxs-lookup"><span data-stu-id="7d217-207">Verify Classic Run As authentication</span></span>
<span data-ttu-id="7d217-208">Utför ett liknande litet test för att bekräfta att du kan autentisera med hjälp av det nya klassiska Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-208">Perform a similar small test to confirm that you can successfully authenticate by using the new Classic Run As account.</span></span>

1. <span data-ttu-id="7d217-209">Öppna Automation-kontot som du skapade tidigare på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-209">In the Azure portal, open the Automation account that you created earlier.</span></span>

2. <span data-ttu-id="7d217-210">Öppna listan med runbookflöden genom att klicka på panelen med **runbookflöden**.</span><span class="sxs-lookup"><span data-stu-id="7d217-210">Click the **Runbooks** tile to open the list of runbooks.</span></span>

3. <span data-ttu-id="7d217-211">Välj **AzureAutomationTutorialScript**-runbooken och klicka på **Starta** för att starta runbooken.</span><span class="sxs-lookup"><span data-stu-id="7d217-211">Select the **AzureClassicAutomationTutorialScript** runbook, and then click **Start** to  start the runbook.</span></span> <span data-ttu-id="7d217-212">Följande sker:</span><span class="sxs-lookup"><span data-stu-id="7d217-212">The following events occur:</span></span>

 * <span data-ttu-id="7d217-213">Ett [runbook-jobb](automation-runbook-execution.md) skapas, bladet **Jobb** visas och jobbstatusen visas på panelen **Jobbsammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="7d217-213">A [runbook job](automation-runbook-execution.md) is created, the **Job** blade is displayed, and the job status is displayed in the **Job Summary** tile.</span></span>
 * <span data-ttu-id="7d217-214">Jobbets första status är **I kö** vilket betyder att det väntar på att en runbook-arbetsroll i molnet ska bli tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7d217-214">The job status begins as **Queued**, indicating that it is waiting for a runbook worker in the cloud to become available.</span></span>
 * <span data-ttu-id="7d217-215">Statusen ändras till **Startar** när ett arbetsobjekt begär jobbet.</span><span class="sxs-lookup"><span data-stu-id="7d217-215">The status becomes **Starting** when a worker claims the job.</span></span>
 * <span data-ttu-id="7d217-216">Statusen ändras till **Körs** när runbooken börjar köras.</span><span class="sxs-lookup"><span data-stu-id="7d217-216">The status becomes **Running** when the runbook starts running.</span></span>
 * <span data-ttu-id="7d217-217">När runbook-jobbet har körts bör statusen vara **Slutfört**.</span><span class="sxs-lookup"><span data-stu-id="7d217-217">When the runbook job has finished running, you should see a status of **Completed**.</span></span>

    ![Runbook-test för säkerhetsobjekt](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. <span data-ttu-id="7d217-219">Om du vill visa ett detaljerat resultat av runbook-jobbet klickar du på panelen **Utdata**.</span><span class="sxs-lookup"><span data-stu-id="7d217-219">To see the detailed results of the runbook, click the **Output** tile.</span></span>  
<span data-ttu-id="7d217-220">Bladet **Utdata** visas och anger att runbooken har autentiserats och returnerat en lista över alla klassiska virtuella datorer i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-220">The **Output** blade is displayed, showing that the runbook has successfully authenticated and returned a list of all classic VMs in the subscription.</span></span>

5. <span data-ttu-id="7d217-221">Stäng bladet **Utdata** och gå tillbaka till bladet **Jobbsammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="7d217-221">Close the **Output** blade to return to the **Job Summary** blade.</span></span>

6. <span data-ttu-id="7d217-222">Stäng bladet **Jobbsammanfattning** och motsvarande blad för **AzureAutomationTutorialScript**-runbooken.</span><span class="sxs-lookup"><span data-stu-id="7d217-222">Close the **Job Summary** blade and the corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="managing-your-run-as-account"></a><span data-ttu-id="7d217-223">Hantera ett Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="7d217-223">Managing your Run As account</span></span>
<span data-ttu-id="7d217-224">Någon gång innan Automation-kontot går ut måste du förnya certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7d217-224">At some point before your Automation account expires, you will need to renew the certificate.</span></span> <span data-ttu-id="7d217-225">Om du tror att ditt Kör som-konto har komprometterats kan du ta bort och återskapa det.</span><span class="sxs-lookup"><span data-stu-id="7d217-225">If you believe that the Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="7d217-226">Det här avsnittet beskriver hur du utför dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7d217-226">This section discusses how to perform these operations.</span></span>

### <a name="self-signed-certificate-renewal"></a><span data-ttu-id="7d217-227">Förnya självsignerade certifikat</span><span class="sxs-lookup"><span data-stu-id="7d217-227">Self-signed certificate renewal</span></span>
<span data-ttu-id="7d217-228">Det självsignerade certifikatet som du skapade för Kör som-kontot går ut ett år efter det datum då det skapades.</span><span class="sxs-lookup"><span data-stu-id="7d217-228">The self-signed certificate that you created for the Run As account expires one year from the date of creation.</span></span> <span data-ttu-id="7d217-229">Du kan förnya det när som helst innan det upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="7d217-229">You can renew it at any time before it expires.</span></span> <span data-ttu-id="7d217-230">När du förnyar det behålls det aktuella giltiga certifikatet för att säkerställa att alla runbookflöden som placerats i kö eller som körs aktivt, och som autentiserar med Kör som-kontot, inte påverkas negativt.</span><span class="sxs-lookup"><span data-stu-id="7d217-230">When you renew it, the current valid certificate is retained to ensure that any runbooks that are queued up or actively running, and that authenticate with the Run As account, are not negatively affected.</span></span> <span data-ttu-id="7d217-231">Certifikatet förblir giltigt fram till dess förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="7d217-231">The certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="7d217-232">Om du har konfigurerat ditt Kör som-konto för Automation att använda ett certifikat som utfärdats av din företagscertifikatutfärdare och du använder det här alternativet, så ersätts företagscertifikatet av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="7d217-232">If you have configured your Automation Run As account to use a certificate issued by your enterprise certificate authority and you use this option, the enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="7d217-233">Du förnyar certifikatet genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="7d217-233">To renew the certificate, do the following:</span></span>

1. <span data-ttu-id="7d217-234">Öppna ditt Automation-konto på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-234">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="7d217-235">På bladet **Automation-konto** väljer du **Kör som-konton** under **Kontoinställningar** i rutan **Kontoegenskaper**.</span><span class="sxs-lookup"><span data-stu-id="7d217-235">On the **Automation Account** blade, in the **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Egenskapsrutan för Automation-konto](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. <span data-ttu-id="7d217-237">På egenskapsbladet för **Kör som-konton** väljer du antingen Kör som-kontot eller det klassiska Kör som-kontot som du vill förnya certifikatet för.</span><span class="sxs-lookup"><span data-stu-id="7d217-237">On the **Run As Accounts** properties blade, select either the Run As account or the Classic Run As account that you want to renew the certificate for.</span></span>

4. <span data-ttu-id="7d217-238">På bladet **Egenskaper** för det valda kontot klickar du på **Förnya certifikat**.</span><span class="sxs-lookup"><span data-stu-id="7d217-238">On the **Properties** blade for the selected account, click **Renew certificate**.</span></span>

    ![Förnya certifikat för Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="7d217-240">Medan certifikatet förnyas kan du följa förloppet under **Meddelanden** på menyn.</span><span class="sxs-lookup"><span data-stu-id="7d217-240">While the certificate is being renewed, you can track the progress under **Notifications** from the menu.</span></span>

### <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="7d217-241">Ta bort ett Kör som-konto eller ett klassiskt Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="7d217-241">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="7d217-242">Det här avsnittet beskriver hur du tar bort och sedan återskapar ett Kör som-konto eller ett klassiskt Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="7d217-242">This section describes how to delete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="7d217-243">När du utför den här åtgärden behålls Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-243">When you perform this action, the Automation account is retained.</span></span> <span data-ttu-id="7d217-244">När du har tagit bort ett Kör som-konto eller ett klassiskt Kör som-konto kan du återskapa det på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-244">After you delete a Run As or Classic Run As account, you can re-create it in the Azure portal.</span></span>

1. <span data-ttu-id="7d217-245">Öppna ditt Automation-konto på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7d217-245">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="7d217-246">På bladet **Automation-konto** väljer du **Kör som-konton** i rutan för kontoegenskaper.</span><span class="sxs-lookup"><span data-stu-id="7d217-246">On the **Automation account** blade, in the account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="7d217-247">På egenskapsbladet för **Kör som-konton** väljer du antingen Kör som-kontot eller det klassiska Kör som-kontot som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="7d217-247">On the **Run As Accounts** properties blade, select either the Run As account or Classic Run As account that you want to delete.</span></span> <span data-ttu-id="7d217-248">Klicka på **Ta bort** på bladet **Egenskaper** för det valda kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-248">Then, on the **Properties** blade for the selected account, click **Delete**.</span></span>

 ![Ta bort Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. <span data-ttu-id="7d217-250">Medan kontot tas bort kan du följa förloppet under **Meddelanden** på menyn.</span><span class="sxs-lookup"><span data-stu-id="7d217-250">While the account is being deleted, you can track the progress under **Notifications** from the menu.</span></span>

5. <span data-ttu-id="7d217-251">När kontot har tagits bort måste du återskapa det på egenskapsbladet för **Kör som-konton** genom att välja alternativet Skapa för **Kör som-konto i Azure**.</span><span class="sxs-lookup"><span data-stu-id="7d217-251">After the account has been deleted, you can re-create it on the **Run As Accounts** properties blade by selecting the create option **Azure Run As Account**.</span></span>

 ![Återskapa Kör som-kontot för Automation](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a><span data-ttu-id="7d217-253">Felaktig konfiguration</span><span class="sxs-lookup"><span data-stu-id="7d217-253">Misconfiguration</span></span>
<span data-ttu-id="7d217-254">Vissa konfigurationsobjekt som krävs för att Kör som-kontot eller det klassiska Kör som-kontot ska fungera kanske har tagits bort eller kanske inte skapades korrekt under den ursprungliga konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-254">Some configuration items necessary for the Run As or Classic Run As account to function properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="7d217-255">Exempel på dessa objekt är:</span><span class="sxs-lookup"><span data-stu-id="7d217-255">The items include:</span></span>

* <span data-ttu-id="7d217-256">Certifikattillgång</span><span class="sxs-lookup"><span data-stu-id="7d217-256">Certificate asset</span></span>
* <span data-ttu-id="7d217-257">Anslutningstillgång</span><span class="sxs-lookup"><span data-stu-id="7d217-257">Connection asset</span></span>
* <span data-ttu-id="7d217-258">Kör som-konto har tagits bort från deltagarrollen</span><span class="sxs-lookup"><span data-stu-id="7d217-258">Run As account has been removed from the contributor role</span></span>
* <span data-ttu-id="7d217-259">Huvudnamn för tjänsten eller program i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d217-259">Service principal or application in Azure AD</span></span>

<span data-ttu-id="7d217-260">I den föregående instansen och andra instanser av felaktiga konfigurationer identifierar Automation-kontot ändringarna och visar statusen *Ofullständig* på egenskapsbladet för **Kör som-konton** för kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-260">In the preceding and other instances of misconfiguration, the Automation account detects the changes and displays a status of *Incomplete* on the **Run As Accounts** properties blade for the account.</span></span>

![Konfigurationsstatusen Ofullständig för Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="7d217-262">När du väljer Kör som-kontot visas följande felmeddelande i rutan **Egenskaper** för kontot:</span><span class="sxs-lookup"><span data-stu-id="7d217-262">When you select the Run As account, the account **Properties** pane displays the following error message:</span></span>

![Varningsmeddelande om ofullständig konfiguration av Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="7d217-264">.</span><span class="sxs-lookup"><span data-stu-id="7d217-264">.</span></span>

<span data-ttu-id="7d217-265">Du kan snabbt lösa dessa problem med Kör som-kontot genom att ta bort och återskapa kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-265">You can quickly resolve these Run As account issues by deleting and re-creating the account.</span></span>

## <a name="update-your-automation-account-by-using-powershell"></a><span data-ttu-id="7d217-266">Uppdatera Automation-kontot med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7d217-266">Update your Automation account by using PowerShell</span></span>
<span data-ttu-id="7d217-267">Du kan använda PowerShell för att uppdatera ett befintligt Automation-konto om:</span><span class="sxs-lookup"><span data-stu-id="7d217-267">You can use PowerShell to update your existing Automation account if:</span></span>

* <span data-ttu-id="7d217-268">Du skapar ett Automation-konto men väljer att inte skapa Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-268">You create an Automation account but decline to create the Run As account.</span></span>
* <span data-ttu-id="7d217-269">Du redan har ett Automation-konto för att hantera Resource Manager-resurser och du vill uppdatera det med ett Kör som-konto för runbook-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7d217-269">You already use an Automation account to manage Resource Manager resources and you want to update the account to include the Run As account for runbook authentication.</span></span>
* <span data-ttu-id="7d217-270">Du redan hanterar klassiska resurser med hjälp av ett Automation-konto och du vill uppdatera det och använda det klassiska Kör som-kontot i stället för att skapa ett nytt konto och migrera dina runbookflöden och tillgångar till det.</span><span class="sxs-lookup"><span data-stu-id="7d217-270">You already use an Automation account to manage classic resources and you want to update it to use the Classic Run As account instead of creating a new account and migrating your runbooks and assets to it.</span></span>   
* <span data-ttu-id="7d217-271">Du vill skapa ett Kör som-konto och ett klassiskt Kör som-konto genom att använda ett certifikat utfärdat av en företagscertifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="7d217-271">You want to create a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

<span data-ttu-id="7d217-272">Skriptet har följande krav:</span><span class="sxs-lookup"><span data-stu-id="7d217-272">The script has the following prerequisites:</span></span>

* <span data-ttu-id="7d217-273">Skriptet kan endast köras i Windows 10 och Windows Server 2016 med Azure Resource Manager-moduler med version 2.01 och senare.</span><span class="sxs-lookup"><span data-stu-id="7d217-273">The script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="7d217-274">Det stöds inte i tidigare versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="7d217-274">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="7d217-275">Azure PowerShell 1.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="7d217-275">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="7d217-276">Information om PowerShell 1.0-versionen finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7d217-276">For information about the PowerShell 1.0 release, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="7d217-277">Ett Automation-konto, som refereras som värdet för *-AutomationAccountName*- och *-ApplicationDisplayName*-parametrarna i följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="7d217-277">An Automation account, which is referenced as the value for the *–AutomationAccountName* and *-ApplicationDisplayName* parameters in the following PowerShell script.</span></span>

<span data-ttu-id="7d217-278">Du hämtar värdena för *SubscriptionID*, *ResourceGroup* och *AutomationAccountName*, som är obligatoriska parametrar för skripten, genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="7d217-278">To get the values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for the scripts, do the following:</span></span>
1. <span data-ttu-id="7d217-279">På Azure Portal väljer du ditt Automation-konto på bladet **Automation-konto** och väljer sedan **Alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7d217-279">In the Azure portal, select your Automation account on the **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="7d217-280">På bladet **Alla inställningar** väljer du **Egenskaper** under **Kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="7d217-280">On the **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="7d217-281">Notera värdena på bladet **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="7d217-281">Note the values on the **Properties** blade.</span></span>

![Bladet Egenskaper för Automation-kontot](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a><span data-ttu-id="7d217-283">Skapa ett PowerShell-skript för ett Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="7d217-283">Create a Run As account PowerShell script</span></span>
<span data-ttu-id="7d217-284">Det här PowerShell-skriptet har stöd för följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="7d217-284">This PowerShell script includes support for the following configurations:</span></span>

* <span data-ttu-id="7d217-285">Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="7d217-285">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="7d217-286">Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="7d217-286">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="7d217-287">Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat.</span><span class="sxs-lookup"><span data-stu-id="7d217-287">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="7d217-288">Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett självsignerat certifikat i Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="7d217-288">Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud.</span></span>

<span data-ttu-id="7d217-289">Skriptet skapar följande objekt, beroende på de konfigurationsalternativ som du väljer.</span><span class="sxs-lookup"><span data-stu-id="7d217-289">Depending on the configuration option you select, the script creates the following items.</span></span>

<span data-ttu-id="7d217-290">**För Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="7d217-290">**For Run As accounts:**</span></span>

* <span data-ttu-id="7d217-291">Skapar ett Azure AD-program som ska exporteras med det självsignerade certifikatets eller företagscertifikatets offentliga nyckel, skapar ett tjänstobjektskonto för programmet i Azure AD och tilldelar rollen Deltagare för kontot i din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7d217-291">Creates an Azure AD application to be exported with either the self-signed or enterprise certificate public key, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="7d217-292">Du kan ändra den här inställningen till Ägare eller en annan roll.</span><span class="sxs-lookup"><span data-stu-id="7d217-292">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="7d217-293">Mer information finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="7d217-293">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="7d217-294">Skapar en Automation-certifikattillgång med namnet *AzureRunAsCertificate* i det angivna Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-294">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="7d217-295">Certifikattillgången innehåller certifikatets privata nyckel som används av Azure AD-programmet.</span><span class="sxs-lookup"><span data-stu-id="7d217-295">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="7d217-296">Skapar en Automation-anslutningstillgång med namnet *AzureRunAsConnection* i det angivna Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-296">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="7d217-297">Anslutningstillgången innehåller applicationId, tenantId, subscriptionId och certifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="7d217-297">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="7d217-298">**För klassiska Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="7d217-298">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="7d217-299">Skapar en Automation-certifikattillgång med namnet *AzureClassicRunAsCertificate* i det angivna Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-299">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="7d217-300">Certifikattillgången innehåller den privata nyckelns certifikat som används av hanteringscertifikatet.</span><span class="sxs-lookup"><span data-stu-id="7d217-300">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="7d217-301">Skapar en Automation-anslutningstillgång med namnet *AzureClassicRunAsConnection* i det angivna Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="7d217-301">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="7d217-302">Anslutningstillgången innehåller prenumerationsnamnet, subscriptionId och certifikattillgångens namn.</span><span class="sxs-lookup"><span data-stu-id="7d217-302">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="7d217-303">Om du väljer något av alternativen för att skapa ett klassiskt Kör som-konto laddar du upp det offentliga certifikatet (filnamnstillägget .cer) när skriptet har körts till hanteringsarkivet för den prenumeration som Automation-kontot skapades i.</span><span class="sxs-lookup"><span data-stu-id="7d217-303">If you select either option for creating a Classic Run As account, after the script is executed, upload the public certificate (.cer file name extension) to the management store for the subscription that the Automation account was created in.</span></span>
> 

<span data-ttu-id="7d217-304">Så här kör du skriptet och laddar upp certifikatet:</span><span class="sxs-lookup"><span data-stu-id="7d217-304">To execute the script and upload the certificate, do the following:</span></span>

1. <span data-ttu-id="7d217-305">Spara följande skript på datorn.</span><span class="sxs-lookup"><span data-stu-id="7d217-305">Save the following script on your computer.</span></span> <span data-ttu-id="7d217-306">I det här exemplet sparar du det med filnamnet *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="7d217-306">In this example, save it with the filename *New-RunAsAccount.ps1*.</span></span>

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

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
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
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="7d217-307">Klicka på **Start** på datorn och starta sedan **Windows PowerShell** med utökade användarrättigheter.</span><span class="sxs-lookup"><span data-stu-id="7d217-307">On your computer, click **Start**, and then start **Windows PowerShell** with elevated user rights.</span></span>

3. <span data-ttu-id="7d217-308">Från det upphöjda PowerShell-kommandoradsgränssnittet går du till mappen som innehåller skriptet som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="7d217-308">From the elevated PowerShell command-line shell, go to the folder that contains the script you created in step 1.</span></span>

4. <span data-ttu-id="7d217-309">Kör skriptet genom att använda de parametervärden för konfigurationen som du behöver.</span><span class="sxs-lookup"><span data-stu-id="7d217-309">Execute the script by using the parameter values for the configuration you require.</span></span>

    <span data-ttu-id="7d217-310">**Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat**</span><span class="sxs-lookup"><span data-stu-id="7d217-310">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="7d217-311">**Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat**</span><span class="sxs-lookup"><span data-stu-id="7d217-311">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="7d217-312">**Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat**</span><span class="sxs-lookup"><span data-stu-id="7d217-312">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="7d217-313">**Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett självsignerat certifikat i Azure Government-molnet**</span><span class="sxs-lookup"><span data-stu-id="7d217-313">**Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="7d217-314">När skriptet har körts uppmanas du att autentisera med Azure.</span><span class="sxs-lookup"><span data-stu-id="7d217-314">After the script has executed, you will be prompted to authenticate with Azure.</span></span> <span data-ttu-id="7d217-315">Logga in med ett konto som är medlem i rollen för prenumerationsadministratörer och som är medadministratör för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-315">Sign in with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>
    >
    >

<span data-ttu-id="7d217-316">Notera följande när skriptet har körts:</span><span class="sxs-lookup"><span data-stu-id="7d217-316">After the script has executed successfully, note the following:</span></span>
* <span data-ttu-id="7d217-317">Om du skapade ett klassiskt Kör som-konto med ett självsignerat offentligt certifikat (CER-fil) skapas och sparas det av skriptet i mappen för tillfälliga filer på datorn under användarprofilen *%USERPROFILE%\AppData\Local\Temp*, som du använde för att köra PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="7d217-317">If you created a Classic Run As account with a self-signed public certificate (.cer file), the script creates and saves it to the temporary files folder on your computer under the user profile *%USERPROFILE%\AppData\Local\Temp*, which you used to execute the PowerShell session.</span></span>
* <span data-ttu-id="7d217-318">Om du skapade ett klassiskt Kör som-konto med ett offentligt företagscertifikat (CER-fil) använder du det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7d217-318">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="7d217-319">Följ anvisningarna för hur du [laddar upp ett hanterings-API-certifikat till den klassiska Azure-portalen](../azure-api-management-certs.md) och verifiera sedan konfigurationen av autentiseringsuppgifterna med Service Management-resurser med hjälp av [exempelkoden för autentisering med Service Management-resurser](#sample-code-to-authenticate-with-service-management-resources).</span><span class="sxs-lookup"><span data-stu-id="7d217-319">Follow the instructions for [uploading a management API certificate to the Azure classic portal](../azure-api-management-certs.md), and then validate the credential configuration with Service Management resources by using the [sample code to authenticate with Service Management Resources](#sample-code-to-authenticate-with-service-management-resources).</span></span> 
* <span data-ttu-id="7d217-320">Om du *inte* skapade ett klassiskt Kör som-konto autentiserar du med Resource Manager-resurser och verifierar konfigurationen av autentiseringsuppgifterna med hjälp av [exempelkoden för autentisering med Service Management-resurser](#sample-code-to-authenticate-with-resource-manager-resources).</span><span class="sxs-lookup"><span data-stu-id="7d217-320">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate the credential configuration by using the [sample code for authenticating with Service Management resources](#sample-code-to-authenticate-with-resource-manager-resources).</span></span>

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a><span data-ttu-id="7d217-321">Exempelkod för att autentisera med Resource Manager-resurser</span><span class="sxs-lookup"><span data-stu-id="7d217-321">Sample code to authenticate with Resource Manager resources</span></span>
<span data-ttu-id="7d217-322">Du kan använda följande uppdaterade exempelkod, som kommer från exempel-runbooken *AzureAutomationTutorialScript*, om du vill autentisera med hjälp av Kör som-kontot för att hantera Resource Manager-resurser med dina runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-322">You can use the following updated sample code, taken from the *AzureAutomationTutorialScript* example runbook, to authenticate by using the Run As account to manage Resource Manager resources with your runbooks.</span></span>

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
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

<span data-ttu-id="7d217-323">Skriptet innehåller två ytterligare rader med kod som gör det möjligt att referera till en prenumerationskontext så att du enkelt kan arbeta med flera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7d217-323">To help you to easily work between multiple subscriptions, the script includes two additional lines of code that support referencing a subscription context.</span></span> <span data-ttu-id="7d217-324">En variabeltillgång med namnet *SubscriptionId* innehåller ID:t för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7d217-324">A variable asset named *SubscriptionId* contains the ID of the subscription.</span></span> <span data-ttu-id="7d217-325">Efter cmdlet-instruktionen `Add-AzureRmAccount` används cmdleten [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) med parameteruppsättningen *-SubscriptionId*.</span><span class="sxs-lookup"><span data-stu-id="7d217-325">After the `Add-AzureRmAccount` cmdlet statement, the [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet is stated with the parameter set *-SubscriptionId*.</span></span> <span data-ttu-id="7d217-326">Om variabelnamnet är för allmänt kan du ändra det och lägga till ett prefix eller använda en annan namngivningskonvention så att det blir lättare att identifiera.</span><span class="sxs-lookup"><span data-stu-id="7d217-326">If the variable name is too generic, you can revise it to include a prefix or use another naming convention to make it easier to identify.</span></span> <span data-ttu-id="7d217-327">Du kan också använda parameteruppsättningen *-SubscriptionName* i stället för *-SubscriptionId* med en tillhörande variabeltillgång.</span><span class="sxs-lookup"><span data-stu-id="7d217-327">Alternatively, you can use the parameter set *-SubscriptionName* instead of *-SubscriptionId* with a corresponding variable asset.</span></span>

<span data-ttu-id="7d217-328">Den cmdlet som du använder för autentisering i runbooken, `Add-AzureRmAccount`, använder parameteruppsättningen *ServicePrincipalCertificate*.</span><span class="sxs-lookup"><span data-stu-id="7d217-328">The cmdlet that you use for authenticating in the runbook, `Add-AzureRmAccount`, uses the *ServicePrincipalCertificate* parameter set.</span></span> <span data-ttu-id="7d217-329">Den autentiserar med hjälp av tjänstobjektscertifikatet, inte användarautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="7d217-329">It authenticates by using the service principal certificate, not the user credentials.</span></span>

## <a name="sample-code-to-authenticate-with-service-management-resources"></a><span data-ttu-id="7d217-330">Exempelkod för att autentisera med Service Management-resurser</span><span class="sxs-lookup"><span data-stu-id="7d217-330">Sample code to authenticate with Service Management resources</span></span>
<span data-ttu-id="7d217-331">Du kan använda följande uppdaterade exempelkod, som kommer från exempel-runbooken *AzureClassicAutomationTutorialScript*, om du vill autentisera med hjälp av det klassiska Kör som-kontot för att hantera klassiska resurser med dina runbookflöden.</span><span class="sxs-lookup"><span data-stu-id="7d217-331">You can use the following updated sample code, which is taken from the *AzureClassicAutomationTutorialScript* example runbook, to authenticate by using the Classic Run As account to manage classic resources with your runbooks.</span></span>

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a><span data-ttu-id="7d217-332">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d217-332">Next steps</span></span>
* [<span data-ttu-id="7d217-333">Program och tjänstobjekt i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d217-333">Application and service principal objects in Azure AD</span></span>](../active-directory/active-directory-application-objects.md)
* [<span data-ttu-id="7d217-334">Rollbaserad åtkomstkontroll i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="7d217-334">Role-based access control in Azure Automation</span></span>](automation-role-based-access-control.md)
* [<span data-ttu-id="7d217-335">Certifikatöversikt för Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="7d217-335">Certificates overview for Azure Cloud Services</span></span>](../cloud-services/cloud-services-certs-create.md)
