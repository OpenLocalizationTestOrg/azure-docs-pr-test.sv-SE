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
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a><span data-ttu-id="f3089-104">Autentisera runbookflöden med ett Kör som-konto i Azure</span><span class="sxs-lookup"><span data-stu-id="f3089-104">Authenticate runbooks with an Azure Run As account</span></span>
<span data-ttu-id="f3089-105">Den här artikeln visar hur tooconfigure en Azure Automation-kontot i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-105">This article shows you how tooconfigure an Azure Automation account in hello Azure portal.</span></span> <span data-ttu-id="f3089-106">toodo så är fallet bör du använda hello kör som-konto funktionen tooauthenticate runbooks hantera resurser i Azure Resource Manager eller Azure Service Management.</span><span class="sxs-lookup"><span data-stu-id="f3089-106">toodo so, you use hello Run As account feature tooauthenticate runbooks managing resources in either Azure Resource Manager or Azure Service Management.</span></span>

<span data-ttu-id="f3089-107">När du skapar ett Automation-konto i hello Azure-portalen kan skapa du automatiskt två konton:</span><span class="sxs-lookup"><span data-stu-id="f3089-107">When you create an Automation account in hello Azure portal, you automatically create two accounts:</span></span>

* <span data-ttu-id="f3089-108">Ett Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-108">A Run As account.</span></span> <span data-ttu-id="f3089-109">Det här kontot skapar ett tjänstobjekt i Azure Active Directory (AD Azure) och ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-109">This account creates a service principal in Azure Active Directory (Azure AD) and a certificate.</span></span> <span data-ttu-id="f3089-110">Det ger också hello deltagare rollbaserad åtkomstkontroll (RBAC), som hanterar Resource Manager-resurser med hjälp av runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-110">It also assigns hello Contributor role-based access control (RBAC), which manages Resource Manager resources by using runbooks.</span></span>
* <span data-ttu-id="f3089-111">Ett klassiskt Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-111">A Classic Run As account.</span></span> <span data-ttu-id="f3089-112">Det här kontot laddar upp ett hanteringscertifikat som används toomanage Service Management eller klassiska resurser med hjälp av runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-112">This account uploads a management certificate, which is used toomanage Service Management or classic resources by using runbooks.</span></span>

<span data-ttu-id="f3089-113">Skapa en Automation konto gör hello enklare för dig och hjälper dig att snabbt börja skapa och distribuera runbooks toosupport ditt automation måste.</span><span class="sxs-lookup"><span data-stu-id="f3089-113">Creating an Automation account simplifies hello process for you and helps you quickly start building and deploying runbooks toosupport your automation needs.</span></span>

<span data-ttu-id="f3089-114">Med Kör som-konton och klassiska Kör som-konton kan du:</span><span class="sxs-lookup"><span data-stu-id="f3089-114">With Run As and Classic Run As accounts, you can:</span></span>

* <span data-ttu-id="f3089-115">Ange ett standardiserat sätt tooauthenticate med Azure när du hanterar hanteraren för filserverresurser eller Service Management-resurser från runbooks i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-115">Provide a standardized way tooauthenticate with Azure when you manage Resource Manager or Service Management resources from runbooks in hello Azure portal.</span></span>
* <span data-ttu-id="f3089-116">Automatisera hello användning av globala runbooks som du kan konfigurera i Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f3089-116">Automate hello use of global runbooks, which you can configure in Azure Alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="f3089-117">Hej [Azure integration postaviseringsfunktionen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) med Automation globala runbooks kräver ett Automation-konto som har konfigurerats med en Kör som-konto och klassiska kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-117">hello [Azure Alert integration feature](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) with Automation global runbooks requires an Automation account that's configured with a Run As account and a Classic Run As account.</span></span> <span data-ttu-id="f3089-118">Du kan välja ett Automation-konto som redan har definierats kör som och klassiska kör som-konton eller du toocreate ett nytt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-118">You can select an Automation account that already has defined Run As and Classic Run As accounts, or you can choose toocreate a new Automation account.</span></span>
>  

<span data-ttu-id="f3089-119">Den här artikeln visar hur toocreate ett Automation-konto från hello Azure-portalen, uppdatera ett Automation-konto med hjälp av Azure PowerShell, hantera hello kontokonfiguration och autentiseras i dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-119">This article shows how toocreate an Automation account from hello Azure portal, update an Automation account by using Azure PowerShell, manage hello account configuration, and authenticate in your runbooks.</span></span>

<span data-ttu-id="f3089-120">Innan du börjar skapa ett Automation-konto, är en bra idé toounderstand och Tänk hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3089-120">Before you begin creating an Automation account, it's a good idea toounderstand and consider hello following:</span></span>

* <span data-ttu-id="f3089-121">Skapa ett Automation-konto påverkar inte Automation-konton som du kanske redan har skapat i hello klassiska eller Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f3089-121">Creating an Automation account does not affect Automation accounts you might have already created in either hello classic or Resource Manager deployment model.</span></span>
* <span data-ttu-id="f3089-122">hello processen fungerar endast för Automation-konton som du skapar i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-122">hello process works only for Automation accounts that you create in hello Azure portal.</span></span> <span data-ttu-id="f3089-123">Försöker toocreate ett konto från hello klassiska Azure-portalen replikerar inte konfigurationen hello kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-123">Attempting toocreate an account from hello Azure classic portal does not replicate hello Run As account configuration.</span></span>
* <span data-ttu-id="f3089-124">Om du redan har runbookflöden och tillgångar (till exempel scheman eller variabler) i plats toomanage klassiska resurser och du vill runbooks tooauthenticate med hello nya klassiska kör som-konto, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="f3089-124">If you already have runbooks and assets (such as schedules or variables) in place toomanage classic resources, and you want runbooks tooauthenticate with hello new Classic Run As account, do either of hello following:</span></span>

  * <span data-ttu-id="f3089-125">toocreate klassiska kör som-konton, följ instruktionerna för hello under hello ”hantera dina kör som-konto”.</span><span class="sxs-lookup"><span data-stu-id="f3089-125">toocreate a Classic Run As account, follow hello instructions in hello "Managing your Run As account" section.</span></span> 
  * <span data-ttu-id="f3089-126">tooupdate ditt befintliga konto, Använd hello PowerShell-skript under hello ”uppdatera ditt Automation-konto med hjälp av PowerShell”.</span><span class="sxs-lookup"><span data-stu-id="f3089-126">tooupdate your existing account, use hello PowerShell script in hello "Update your Automation account by using PowerShell" section.</span></span>
* <span data-ttu-id="f3089-127">tooauthenticate med hjälp av hello nytt kör som-konto och klassiska kör som Automation-konto behöver du toomodify dina befintliga runbooks med hello exempelkoden som visas i avsnittet hello [Autentiseringskod](#authentication-code-examples).</span><span class="sxs-lookup"><span data-stu-id="f3089-127">tooauthenticate by using hello new Run As account and Classic Run As Automation account, you  need toomodify your existing runbooks with hello example code provided in hello section [Authentication code examples](#authentication-code-examples).</span></span>

    >[!NOTE]
    ><span data-ttu-id="f3089-128">hello kör som-konto är för autentisering gentemot Resource Manager-resurser med hjälp av hello certifikatbaserad tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="f3089-128">hello Run As account is for authentication against Resource Manager resources using hello certificate-based service principal.</span></span> <span data-ttu-id="f3089-129">hello klassiska kör som-konto är för att autentisera mot Service Management-resurser med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-129">hello Classic Run As account is for authenticating against Service Management resources with a management certificate.</span></span>

## <a name="create-an-automation-account-from-hello-azure-portal"></a><span data-ttu-id="f3089-130">Skapa ett Automation-konto från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f3089-130">Create an Automation account from hello Azure portal</span></span>
<span data-ttu-id="f3089-131">I det här avsnittet skapar du ett Azure Automation-konto från hello Azure-portalen, vilket i sin tur skapar både en Kör som-konto och klassiska kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-131">In this section, you create an Azure Automation account from hello Azure portal, which in turn creates both a Run As account and a Classic Run As account.</span></span>

>[!NOTE]
><span data-ttu-id="f3089-132">toocreate ett Automation-konto som du måste vara Rollmedlem hello Service administratörer eller medadministratör för hello-prenumeration som ger åtkomst toohello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-132">toocreate an Automation account, you must be a member of hello Service Admins role or co-administrator of hello subscription that is granting access toohello subscription.</span></span> <span data-ttu-id="f3089-133">Du måste också läggas till som en användare toothat prenumeration standardinstansen för Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f3089-133">You must also be added as a user toothat subscription's default Active Directory instance.</span></span> <span data-ttu-id="f3089-134">hello kontot behöver inte toobe som tilldelats en privilegierad roll.</span><span class="sxs-lookup"><span data-stu-id="f3089-134">hello account does not need toobe assigned a privileged role.</span></span>
>
><span data-ttu-id="f3089-135">Om du inte är medlem i Active Directory-instans för hello prenumeration innan du lade toohello medadministratör rollen hello prenumeration kan läggs du tooActive Directory som en gäst.</span><span class="sxs-lookup"><span data-stu-id="f3089-135">If you are not a member of hello subscription’s Active Directory instance before you are added toohello co-administrator role of hello subscription, you will be added tooActive Directory as a guest.</span></span> <span data-ttu-id="f3089-136">I den här instansen får du en ”du har inte behörighet toocreate...”</span><span class="sxs-lookup"><span data-stu-id="f3089-136">In this instance, you will receive a “You do not have permissions toocreate…”</span></span> <span data-ttu-id="f3089-137">varning för hello **lägga till Automation-konto** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-137">warning on hello **Add Automation Account** blade.</span></span>
>
><span data-ttu-id="f3089-138">Användare som har lagts till toohello samtidigt administratörsroll först kan tas bort från hello prenumeration Active Directory-instans och läggas till toomake dem en fullständig användare i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f3089-138">Users who were added toohello co-administrator role first can be removed from hello subscription's Active Directory instance and re-added toomake them a full User in Active Directory.</span></span> <span data-ttu-id="f3089-139">tooverify detta hello **Azure Active Directory** rutan i hello Azure-portalen genom att välja **användare och grupper**, välja **alla användare** och, när du har valt hello specifika användare, välja **profil**.</span><span class="sxs-lookup"><span data-stu-id="f3089-139">tooverify this situation from hello **Azure Active Directory** pane in hello Azure portal by selecting **Users and groups**, selecting **All users** and, after you select hello specific user, selecting **Profile**.</span></span> <span data-ttu-id="f3089-140">Hej värdet för hello **användartyp** attributet under hello användare profil ska inte vara lika med **gäst**.</span><span class="sxs-lookup"><span data-stu-id="f3089-140">hello value of hello **User type** attribute under hello users profile should not equal **Guest**.</span></span>
>

1. <span data-ttu-id="f3089-141">Logga in toohello Azure-portalen med ett konto som är medlem i administratörsrollen i hello prenumeration och medadministratör för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-141">Sign in toohello Azure portal with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>

2. <span data-ttu-id="f3089-142">Välj **Automation-konton**.</span><span class="sxs-lookup"><span data-stu-id="f3089-142">Select **Automation Accounts**.</span></span>

3. <span data-ttu-id="f3089-143">På hello **Automation-konton** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f3089-143">On hello **Automation Accounts** blade, click **Add**.</span></span>
<span data-ttu-id="f3089-144">Hej **lägga till Automation-konto** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="f3089-144">hello **Add Automation Account** blade opens.</span></span>

 ![Hej ”Lägg till Automation-konto” bladet](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > <span data-ttu-id="f3089-146">Om ditt konto inte är medlem i administratörsrollen i hello prenumeration och medadministratör för prenumerationen hello hello följande varning visas på hello **lägga till Automation-konto** bladet:</span><span class="sxs-lookup"><span data-stu-id="f3089-146">If your account is not a member of hello subscription administrators role and co-administrator of hello subscription, hello following warning is displayed on hello **Add Automation Account** blade:</span></span>
   >
   >![Varningsmeddelande för Lägg till Automation-konto](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. <span data-ttu-id="f3089-148">På hello **lägga till Automation-konto** bladet i hello **namn** Skriv ett namn för det nya Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="f3089-148">On hello **Add Automation Account** blade, in hello **Name** box, type a name for your new Automation account.</span></span>

5. <span data-ttu-id="f3089-149">Om du har mer än en prenumeration hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3089-149">If you have more than one subscription, do hello following:</span></span>

    <span data-ttu-id="f3089-150">a.</span><span class="sxs-lookup"><span data-stu-id="f3089-150">a.</span></span> <span data-ttu-id="f3089-151">Under **prenumeration**, ange ett nytt hello-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-151">Under **Subscription**, specify one for hello new account.</span></span>

    <span data-ttu-id="f3089-152">b.</span><span class="sxs-lookup"><span data-stu-id="f3089-152">b.</span></span> <span data-ttu-id="f3089-153">Under **Resursgrupp** klickar du på **Skapa ny** eller på **Använd befintlig**.</span><span class="sxs-lookup"><span data-stu-id="f3089-153">Under **Resource Group**, click **Create new** or **Use existing**.</span></span>

    <span data-ttu-id="f3089-154">c.</span><span class="sxs-lookup"><span data-stu-id="f3089-154">c.</span></span> <span data-ttu-id="f3089-155">Under **Plats** anger du ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="f3089-155">Under **Location**, specify an Azure datacenter.</span></span>

6. <span data-ttu-id="f3089-156">Under **Skapa Kör som-konto i Azure** väljer du **Ja** och klickar sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f3089-156">Under **Create Azure Run As account**, select **Yes**, and then click **Create**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f3089-157">Om du väljer inte toocreate hello kör som-konto genom att välja **nr**, visas ett varningsmeddelande hello **lägga till Automation-konto** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-157">If you choose not toocreate hello Run As account by selecting **No**, a warning message is displayed hello **Add Automation Account** blade.</span></span> <span data-ttu-id="f3089-158">Även om hello kontot skapas i hello Azure-portalen har inte en motsvarande autentiseringsidentitet inom din klassiska eller Resource Manager prenumeration katalogtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3089-158">Although hello account is created in hello Azure portal, it does not have a corresponding authentication identity within your classic or Resource Manager subscription directory service.</span></span> <span data-ttu-id="f3089-159">Hello-kontot har därför ingen åtkomst tooresources i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-159">Consequently, hello account has no access tooresources in your subscription.</span></span> <span data-ttu-id="f3089-160">Det här scenariot förhindrar att runbookflöden som refererar till det här kontot autentiseras och utför åtgärder mot resurser i dessa distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="f3089-160">This scenario prevents any runbooks that reference this account from authenticating and performing tasks against resources in those deployment models.</span></span>
   >
   > ![Varningsmeddelande på hello ”Lägg till Automation-konto” blad](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > <span data-ttu-id="f3089-162">Dessutom kan tilldelas inte hello deltagarrollen eftersom hello tjänstens huvudnamn inte har skapats.</span><span class="sxs-lookup"><span data-stu-id="f3089-162">Additionally, because hello service principal is not created, hello Contributor role is not assigned.</span></span>
   >

7. <span data-ttu-id="f3089-163">Medan Azure skapar hello Automation-konto, du kan följa förloppet för hello under **meddelanden** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="f3089-163">While Azure creates hello Automation account, you can track hello progress under **Notifications** from hello menu.</span></span>

### <a name="resources"></a><span data-ttu-id="f3089-164">Resurser</span><span class="sxs-lookup"><span data-stu-id="f3089-164">Resources</span></span>
<span data-ttu-id="f3089-165">När hello Automation-konto har skapats, skapas flera resurser automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="f3089-165">When hello Automation account is successfully created, several resources are automatically created for you.</span></span> <span data-ttu-id="f3089-166">hello resurser sammanfattas i följande två tabeller hello:</span><span class="sxs-lookup"><span data-stu-id="f3089-166">hello resources are summarized in hello following two tables:</span></span>

#### <a name="run-as-account-resources"></a><span data-ttu-id="f3089-167">Resurser för Kör som-konton</span><span class="sxs-lookup"><span data-stu-id="f3089-167">Run As account resources</span></span>

| <span data-ttu-id="f3089-168">Resurs</span><span class="sxs-lookup"><span data-stu-id="f3089-168">Resource</span></span> | <span data-ttu-id="f3089-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3089-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f3089-170">AzureAutomationTutorial-runbook</span><span class="sxs-lookup"><span data-stu-id="f3089-170">AzureAutomationTutorial Runbook</span></span> | <span data-ttu-id="f3089-171">Ett exempel grafisk runbook som visar hur hello tooauthenticate med hjälp av kör som-konto och hämtar alla hello Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="f3089-171">An example graphical runbook that demonstrates how tooauthenticate by using hello Run As account and gets all hello Resource Manager resources.</span></span> |
| <span data-ttu-id="f3089-172">AzureAutomationTutorialScript-runbook</span><span class="sxs-lookup"><span data-stu-id="f3089-172">AzureAutomationTutorialScript Runbook</span></span> | <span data-ttu-id="f3089-173">Ett exempel PowerShell-runbook som visar hur hello tooauthenticate med hjälp av kör som-konto och hämtar alla hello Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="f3089-173">An example PowerShell runbook that demonstrates how tooauthenticate by using hello Run As account and gets all hello Resource Manager resources.</span></span> |
| <span data-ttu-id="f3089-174">AzureRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="f3089-174">AzureRunAsCertificate</span></span> | <span data-ttu-id="f3089-175">Hej certifikattillgång som skapas automatiskt när du skapar ett Automation-konto eller använda hello följande PowerShell-skript för ett befintligt konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-175">hello certificate asset that's automatically created when you create an Automation account or use hello following PowerShell script for an existing account.</span></span> <span data-ttu-id="f3089-176">hello certifikatet kan tooauthenticate med Azure så att du kan hantera Azure Resource Manager-resurser från runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-176">hello certificate allows you tooauthenticate with Azure so that you can manage Azure Resource Manager resources from runbooks.</span></span> <span data-ttu-id="f3089-177">hello certifikatet har en livslängd för ett år.</span><span class="sxs-lookup"><span data-stu-id="f3089-177">hello certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="f3089-178">AzureRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="f3089-178">AzureRunAsConnection</span></span> | <span data-ttu-id="f3089-179">Hej anslutningstillgång som skapas automatiskt när du skapar ett Automation-konto eller använda hello PowerShell-skript för ett befintligt konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-179">hello connection asset that's automatically created when you create an Automation account or use hello PowerShell script for an existing account.</span></span> |

#### <a name="classic-run-as-account-resources"></a><span data-ttu-id="f3089-180">Resurser för klassiska Kör som-konton</span><span class="sxs-lookup"><span data-stu-id="f3089-180">Classic Run As account resources</span></span>

| <span data-ttu-id="f3089-181">Resurs</span><span class="sxs-lookup"><span data-stu-id="f3089-181">Resource</span></span> | <span data-ttu-id="f3089-182">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3089-182">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f3089-183">AzureClassicAutomationTutorial-runbook</span><span class="sxs-lookup"><span data-stu-id="f3089-183">AzureClassicAutomationTutorial Runbook</span></span> | <span data-ttu-id="f3089-184">En grafisk exempel-runbook som hämtar alla hello virtuella datorer som skapas med hello klassiska distributionsmodellen för en prenumeration med hjälp av hello klassiska kör som-konto (certifikat) och sedan skriver hello VM namn och status.</span><span class="sxs-lookup"><span data-stu-id="f3089-184">An example graphical runbook that gets all hello VMs that are created using hello classic deployment model in a subscription by using hello Classic Run As account (certificate), and then writes hello VM name and status.</span></span> |
| <span data-ttu-id="f3089-185">AzureClassicAutomationTutorial Script-runbook</span><span class="sxs-lookup"><span data-stu-id="f3089-185">AzureClassicAutomationTutorial Script Runbook</span></span> | <span data-ttu-id="f3089-186">Ett exempel PowerShell-runbook som hämtar alla hello klassiska virtuella datorer i en prenumeration med hjälp av hello klassiska kör som-konto (certifikat) och sedan skriver hello VM-namn och status.</span><span class="sxs-lookup"><span data-stu-id="f3089-186">An example PowerShell runbook that gets all hello classic VMs in a subscription by using hello Classic Run As account (certificate), and then writes hello VM name and status.</span></span> |
| <span data-ttu-id="f3089-187">AzureClassicRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="f3089-187">AzureClassicRunAsCertificate</span></span> | <span data-ttu-id="f3089-188">hello skapas automatiskt certifikattillgång använda tooauthenticate med Azure så att du kan hantera Azure klassiska resurser från runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-188">hello automatically created certificate asset that you use tooauthenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> <span data-ttu-id="f3089-189">hello certifikatet har en livslängd för ett år.</span><span class="sxs-lookup"><span data-stu-id="f3089-189">hello certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="f3089-190">AzureClassicRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="f3089-190">AzureClassicRunAsConnection</span></span> | <span data-ttu-id="f3089-191">hello automatiskt skapade anslutningstillgång använda tooauthenticate med Azure så att du kan hantera Azure klassiska resurser från runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-191">hello automatically created connection asset that you use tooauthenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> |

## <a name="verify-run-as-authentication"></a><span data-ttu-id="f3089-192">Verifiera Kör som-autentisering</span><span class="sxs-lookup"><span data-stu-id="f3089-192">Verify Run As authentication</span></span>
<span data-ttu-id="f3089-193">Utför en test små tooconfirm som du kan autentisera med hjälp av hello nytt kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-193">Perform a small test tooconfirm that you can successfully authenticate by using hello new Run As account.</span></span>

1. <span data-ttu-id="f3089-194">Öppna hello Automation-konto som du skapade tidigare i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-194">In hello Azure portal, open hello Automation account that you created earlier.</span></span>

2. <span data-ttu-id="f3089-195">Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-195">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>

3. <span data-ttu-id="f3089-196">Välj hello **AzureAutomationTutorialScript** runbook, och klicka sedan på **starta** toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f3089-196">Select hello **AzureAutomationTutorialScript** runbook, and then click **Start** toostart hello runbook.</span></span> <span data-ttu-id="f3089-197">hello följande händelser inträffar:</span><span class="sxs-lookup"><span data-stu-id="f3089-197">hello following events occur:</span></span>
 * <span data-ttu-id="f3089-198">En [runbook-jobbet](automation-runbook-execution.md) skapas hello **jobbet** bladet visas och hello jobbstatus visas i hello **jobbsammanfattning** panelen.</span><span class="sxs-lookup"><span data-stu-id="f3089-198">A [runbook job](automation-runbook-execution.md) is created, hello **Job** blade is displayed, and hello job status is displayed in hello **Job Summary** tile.</span></span>
 * <span data-ttu-id="f3089-199">hello jobbstatus börjar som **i kö**, vilket betyder att det väntar på att en runbook worker i hello molnet toobecome tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f3089-199">hello job status begins as **Queued**, indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span>
 * <span data-ttu-id="f3089-200">hello statusen **Start** när en arbetsprocess anspråk hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f3089-200">hello status becomes **Starting** when a worker claims hello job.</span></span>
 * <span data-ttu-id="f3089-201">hello statusen **kör** när hello runbook börjar köras.</span><span class="sxs-lookup"><span data-stu-id="f3089-201">hello status becomes **Running** when hello runbook starts running.</span></span>
 * <span data-ttu-id="f3089-202">När hello runbook-jobbet har körts ska du se statusen **slutförd**.</span><span class="sxs-lookup"><span data-stu-id="f3089-202">When hello runbook job has finished running, you should see a status of **Completed**.</span></span>

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. <span data-ttu-id="f3089-203">toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.</span><span class="sxs-lookup"><span data-stu-id="f3089-203">toosee hello detailed results of hello runbook, click hello **Output** tile.</span></span>  
<span data-ttu-id="f3089-204">Hej **utdata** bladet visas, visar att hello runbook har autentiserats och returneras en lista över alla resurser som är tillgängliga i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f3089-204">hello **Output** blade is displayed, showing that hello runbook has successfully authenticated and returned a list of all resources available in hello resource group.</span></span>

5. <span data-ttu-id="f3089-205">Stäng hello **utdata** bladet tooreturn toohello **jobbsammanfattning** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-205">Close hello **Output** blade tooreturn toohello **Job Summary** blade.</span></span>

6. <span data-ttu-id="f3089-206">Stäng hello **jobbsammanfattning** bladet och hello motsvarande **AzureAutomationTutorialScript** runbook-bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-206">Close hello **Job Summary** blade and hello corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="verify-classic-run-as-authentication"></a><span data-ttu-id="f3089-207">Verifiera klassisk Kör som-autentisering</span><span class="sxs-lookup"><span data-stu-id="f3089-207">Verify Classic Run As authentication</span></span>
<span data-ttu-id="f3089-208">Utföra en liknande liten testa tooconfirm som du kan autentisera med hjälp av hello nya klassiska kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-208">Perform a similar small test tooconfirm that you can successfully authenticate by using hello new Classic Run As account.</span></span>

1. <span data-ttu-id="f3089-209">Öppna hello Automation-konto som du skapade tidigare i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-209">In hello Azure portal, open hello Automation account that you created earlier.</span></span>

2. <span data-ttu-id="f3089-210">Klicka på hello **Runbooks** panelen tooopen hello lista över runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-210">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>

3. <span data-ttu-id="f3089-211">Välj hello **AzureClassicAutomationTutorialScript** runbook, och klicka sedan på **starta** för startar hello runbook.</span><span class="sxs-lookup"><span data-stu-id="f3089-211">Select hello **AzureClassicAutomationTutorialScript** runbook, and then click **Start** too start hello runbook.</span></span> <span data-ttu-id="f3089-212">hello följande händelser inträffar:</span><span class="sxs-lookup"><span data-stu-id="f3089-212">hello following events occur:</span></span>

 * <span data-ttu-id="f3089-213">En [runbook-jobbet](automation-runbook-execution.md) skapas hello **jobbet** bladet visas och hello jobbstatus visas i hello **jobbsammanfattning** panelen.</span><span class="sxs-lookup"><span data-stu-id="f3089-213">A [runbook job](automation-runbook-execution.md) is created, hello **Job** blade is displayed, and hello job status is displayed in hello **Job Summary** tile.</span></span>
 * <span data-ttu-id="f3089-214">hello jobbstatus börjar som **i kö**, vilket betyder att det väntar på att en runbook worker i hello molnet toobecome tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f3089-214">hello job status begins as **Queued**, indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span>
 * <span data-ttu-id="f3089-215">hello statusen **Start** när en arbetsprocess anspråk hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f3089-215">hello status becomes **Starting** when a worker claims hello job.</span></span>
 * <span data-ttu-id="f3089-216">hello statusen **kör** när hello runbook börjar köras.</span><span class="sxs-lookup"><span data-stu-id="f3089-216">hello status becomes **Running** when hello runbook starts running.</span></span>
 * <span data-ttu-id="f3089-217">När hello runbook-jobbet har körts ska du se statusen **slutförd**.</span><span class="sxs-lookup"><span data-stu-id="f3089-217">When hello runbook job has finished running, you should see a status of **Completed**.</span></span>

    ![Runbook-test för säkerhetsobjekt](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. <span data-ttu-id="f3089-219">toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.</span><span class="sxs-lookup"><span data-stu-id="f3089-219">toosee hello detailed results of hello runbook, click hello **Output** tile.</span></span>  
<span data-ttu-id="f3089-220">Hej **utdata** bladet visas, där den hello runbook har autentiserats och returneras en lista över alla klassiska virtuella datorer i hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-220">hello **Output** blade is displayed, showing that hello runbook has successfully authenticated and returned a list of all classic VMs in hello subscription.</span></span>

5. <span data-ttu-id="f3089-221">Stäng hello **utdata** bladet tooreturn toohello **jobbsammanfattning** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-221">Close hello **Output** blade tooreturn toohello **Job Summary** blade.</span></span>

6. <span data-ttu-id="f3089-222">Stäng hello **jobbsammanfattning** bladet och hello motsvarande **AzureAutomationTutorialScript** runbook-bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-222">Close hello **Job Summary** blade and hello corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="managing-your-run-as-account"></a><span data-ttu-id="f3089-223">Hantera ett Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="f3089-223">Managing your Run As account</span></span>
<span data-ttu-id="f3089-224">Någon gång innan det Automation-kontot upphör att gälla måste toorenew hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-224">At some point before your Automation account expires, you will need toorenew hello certificate.</span></span> <span data-ttu-id="f3089-225">Om du tror att hello kör som-konto har komprometterats kan du ta bort och skapa den igen.</span><span class="sxs-lookup"><span data-stu-id="f3089-225">If you believe that hello Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="f3089-226">Det här avsnittet beskrivs hur tooperform dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f3089-226">This section discusses how tooperform these operations.</span></span>

### <a name="self-signed-certificate-renewal"></a><span data-ttu-id="f3089-227">Förnya självsignerade certifikat</span><span class="sxs-lookup"><span data-stu-id="f3089-227">Self-signed certificate renewal</span></span>
<span data-ttu-id="f3089-228">hello självsignerat certifikat som du skapade för hello kör som-konto går ut ett år från hello dag skapas.</span><span class="sxs-lookup"><span data-stu-id="f3089-228">hello self-signed certificate that you created for hello Run As account expires one year from hello date of creation.</span></span> <span data-ttu-id="f3089-229">Du kan förnya det när som helst innan det upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="f3089-229">You can renew it at any time before it expires.</span></span> <span data-ttu-id="f3089-230">När du förnyar det är hello aktuella giltiga certifikat kvarhållna tooensure att alla runbooks som lagts i kö upp eller aktivt körs och som autentiserar med hello kör som-konto inte påverkas negativt.</span><span class="sxs-lookup"><span data-stu-id="f3089-230">When you renew it, hello current valid certificate is retained tooensure that any runbooks that are queued up or actively running, and that authenticate with hello Run As account, are not negatively affected.</span></span> <span data-ttu-id="f3089-231">hello intyg är giltigt fram till dess förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="f3089-231">hello certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="f3089-232">Om du har konfigurerat ditt Automation kör som-konto toouse ett certifikat utfärdat av en enterprise-certifikatutfärdare och du använder det här alternativet, ersätts hello företagscertifikat av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-232">If you have configured your Automation Run As account toouse a certificate issued by your enterprise certificate authority and you use this option, hello enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="f3089-233">toorenew hello certifikat, hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3089-233">toorenew hello certificate, do hello following:</span></span>

1. <span data-ttu-id="f3089-234">Öppna hello Automation-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-234">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="f3089-235">På hello **Automation-konto** bladet i hello **konto egenskaper** rutan under **kontoinställningar**väljer **kör som-konton**.</span><span class="sxs-lookup"><span data-stu-id="f3089-235">On hello **Automation Account** blade, in hello **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Egenskapsrutan för Automation-konto](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. <span data-ttu-id="f3089-237">På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller hello klassiska kör som-konto som du vill ha toorenew hello certifikat för.</span><span class="sxs-lookup"><span data-stu-id="f3089-237">On hello **Run As Accounts** properties blade, select either hello Run As account or hello Classic Run As account that you want toorenew hello certificate for.</span></span>

4. <span data-ttu-id="f3089-238">På hello **egenskaper** bladet för hello valt konto, klickar du på **förnya certifikat**.</span><span class="sxs-lookup"><span data-stu-id="f3089-238">On hello **Properties** blade for hello selected account, click **Renew certificate**.</span></span>

    ![Förnya certifikat för Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="f3089-240">Medan hello-certifikat förnyas du kan följa förloppet för hello under **meddelanden** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="f3089-240">While hello certificate is being renewed, you can track hello progress under **Notifications** from hello menu.</span></span>

### <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="f3089-241">Ta bort ett Kör som-konto eller ett klassiskt Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="f3089-241">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="f3089-242">Det här avsnittet beskrivs hur toodelete och sedan skapa ett kör som eller klassiska kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-242">This section describes how toodelete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="f3089-243">När du utför den här åtgärden sparas hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-243">When you perform this action, hello Automation account is retained.</span></span> <span data-ttu-id="f3089-244">När du har tagit bort ett kör som eller klassiska kör som-konto kan skapa du den igen hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-244">After you delete a Run As or Classic Run As account, you can re-create it in hello Azure portal.</span></span>

1. <span data-ttu-id="f3089-245">Öppna hello Automation-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f3089-245">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="f3089-246">På hello **automatiseringskontot** bladet i hello konto egenskapsrutan, Välj **kör som-konton**.</span><span class="sxs-lookup"><span data-stu-id="f3089-246">On hello **Automation account** blade, in hello account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="f3089-247">På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller klassiska kör som-konto som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="f3089-247">On hello **Run As Accounts** properties blade, select either hello Run As account or Classic Run As account that you want toodelete.</span></span> <span data-ttu-id="f3089-248">Klicka sedan på hello **egenskaper** bladet för hello valt konto, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f3089-248">Then, on hello **Properties** blade for hello selected account, click **Delete**.</span></span>

 ![Ta bort Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. <span data-ttu-id="f3089-250">Medan hello konto raderas, du kan följa förloppet hello under **meddelanden** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="f3089-250">While hello account is being deleted, you can track hello progress under **Notifications** from hello menu.</span></span>

5. <span data-ttu-id="f3089-251">När hello-konto har tagits bort, du kan skapa den igen på hello **kör som-konton** egenskapsbladet genom att välja hello skapa alternativet **Azure kör som-konto**.</span><span class="sxs-lookup"><span data-stu-id="f3089-251">After hello account has been deleted, you can re-create it on hello **Run As Accounts** properties blade by selecting hello create option **Azure Run As Account**.</span></span>

 ![Skapa nytt hello Automation kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a><span data-ttu-id="f3089-253">Felaktig konfiguration</span><span class="sxs-lookup"><span data-stu-id="f3089-253">Misconfiguration</span></span>
<span data-ttu-id="f3089-254">Vissa konfigurationsobjekt som är nödvändiga för hello kör som eller klassiska kör som-konto toofunction kanske korrekt har tagits bort eller skapats felaktigt under installationen.</span><span class="sxs-lookup"><span data-stu-id="f3089-254">Some configuration items necessary for hello Run As or Classic Run As account toofunction properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="f3089-255">hello-objekt är:</span><span class="sxs-lookup"><span data-stu-id="f3089-255">hello items include:</span></span>

* <span data-ttu-id="f3089-256">Certifikattillgång</span><span class="sxs-lookup"><span data-stu-id="f3089-256">Certificate asset</span></span>
* <span data-ttu-id="f3089-257">Anslutningstillgång</span><span class="sxs-lookup"><span data-stu-id="f3089-257">Connection asset</span></span>
* <span data-ttu-id="f3089-258">Kör som-konto har tagits bort från hello deltagarrollen</span><span class="sxs-lookup"><span data-stu-id="f3089-258">Run As account has been removed from hello contributor role</span></span>
* <span data-ttu-id="f3089-259">Huvudnamn för tjänsten eller program i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3089-259">Service principal or application in Azure AD</span></span>

<span data-ttu-id="f3089-260">I föregående hello och andra instanser av felaktig konfiguration hello Automation-konto identifierar hello ändras och visar statusen *ofullständigt* på hello **kör som-konton** egenskapsbladet för hello konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-260">In hello preceding and other instances of misconfiguration, hello Automation account detects hello changes and displays a status of *Incomplete* on hello **Run As Accounts** properties blade for hello account.</span></span>

![Konfigurationsstatusen Ofullständig för Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="f3089-262">När du väljer hello-kör som-konto hello konto **egenskaper** hello följande felmeddelande visas:</span><span class="sxs-lookup"><span data-stu-id="f3089-262">When you select hello Run As account, hello account **Properties** pane displays hello following error message:</span></span>

![Varningsmeddelande om ofullständig konfiguration av Kör som-konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="f3089-264">.</span><span class="sxs-lookup"><span data-stu-id="f3089-264">.</span></span>

<span data-ttu-id="f3089-265">Du kan snabbt lösa problemen kör som-konto genom att ta bort och återskapa hello-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-265">You can quickly resolve these Run As account issues by deleting and re-creating hello account.</span></span>

## <a name="update-your-automation-account-by-using-powershell"></a><span data-ttu-id="f3089-266">Uppdatera Automation-kontot med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="f3089-266">Update your Automation account by using PowerShell</span></span>
<span data-ttu-id="f3089-267">Du kan använda PowerShell tooupdate befintligt Automation-konto om:</span><span class="sxs-lookup"><span data-stu-id="f3089-267">You can use PowerShell tooupdate your existing Automation account if:</span></span>

* <span data-ttu-id="f3089-268">Du skapar ett Automation-konto men neka toocreate hello kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-268">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="f3089-269">Du använder redan en Automation-konto toomanage Resource Manager-resurser och du vill tooupdate hello konto tooinclude hello kör som-konto för runbook-autentisering.</span><span class="sxs-lookup"><span data-stu-id="f3089-269">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="f3089-270">Du redan använder en Automation-konto toomanage klassiska resurser och du vill tooupdate den toouse hello klassiska kör som-konto i stället för att skapa ett nytt konto och migrera dina runbook-flöden och tillgångar tooit.</span><span class="sxs-lookup"><span data-stu-id="f3089-270">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="f3089-271">Vill du toocreate kör som och klassiska kör som-konto med hjälp av ett certifikat utfärdat av enterprise-certifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="f3089-271">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

<span data-ttu-id="f3089-272">hello skriptet har hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="f3089-272">hello script has hello following prerequisites:</span></span>

* <span data-ttu-id="f3089-273">hello-skript kan köras endast på Windows 10 och Windows Server 2016 med Azure Resource Manager moduler 2.01 och senare.</span><span class="sxs-lookup"><span data-stu-id="f3089-273">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="f3089-274">Det stöds inte i tidigare versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="f3089-274">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="f3089-275">Azure PowerShell 1.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="f3089-275">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="f3089-276">Information om hello PowerShell 1.0-versionen finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f3089-276">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="f3089-277">Ett Automation-konto som refereras till som hello värde hello *– AutomationAccountName* och *- ApplicationDisplayName* parametrar i hello följande PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="f3089-277">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="f3089-278">tooget hello värden för *SubscriptionID*, *ResourceGroup*, och *AutomationAccountName*, som är nödvändiga parametrar för hello skript, hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3089-278">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello scripts, do hello following:</span></span>
1. <span data-ttu-id="f3089-279">I hello Azure-portalen, Välj ditt Automation-konto på hello **automatiseringskontot** bladet och väljer sedan **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f3089-279">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="f3089-280">På hello **alla inställningar** bladet under **kontoinställningar**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="f3089-280">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="f3089-281">Observera hello värden på hello **egenskaper** bladet.</span><span class="sxs-lookup"><span data-stu-id="f3089-281">Note hello values on hello **Properties** blade.</span></span>

![hello Automation-konto ”egenskaper” bladet](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a><span data-ttu-id="f3089-283">Skapa ett PowerShell-skript för ett Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="f3089-283">Create a Run As account PowerShell script</span></span>
<span data-ttu-id="f3089-284">Detta PowerShell-skript har stöd för hello följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="f3089-284">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="f3089-285">Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-285">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="f3089-286">Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-286">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="f3089-287">Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-287">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="f3089-288">Skapa ett kör som-konto och klassiska kör som-konto med hjälp av ett självsignerat certifikat i hello Azure Government-molnet.</span><span class="sxs-lookup"><span data-stu-id="f3089-288">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="f3089-289">Beroende på hello alternativ du väljer, skapar hello skriptet hello följande objekt.</span><span class="sxs-lookup"><span data-stu-id="f3089-289">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="f3089-290">**För Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="f3089-290">**For Run As accounts:**</span></span>

* <span data-ttu-id="f3089-291">Skapar en Azure AD application toobe exporteras med antingen hello självsignerat eller enterprise certifikatets offentliga nyckel, skapar en tjänstens objektkonto för hello program i Azure AD och tilldelar hello deltagarrollen för hello konto i din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-291">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="f3089-292">Du kan ändra den här inställningen tooOwner eller någon annan roll.</span><span class="sxs-lookup"><span data-stu-id="f3089-292">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="f3089-293">Mer information finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="f3089-293">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="f3089-294">Skapar ett Automation-certifikattillgång med namnet *AzureRunAsCertificate* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-294">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="f3089-295">Hej certifikattillgång innehåller hello certifikatets privata nyckel som används av programmet hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f3089-295">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="f3089-296">Skapar ett Automation-anslutningstillgång med namnet *AzureRunAsConnection* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-296">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="f3089-297">Hej anslutningstillgång innehåller hello applicationId, tenantId, prenumerations-ID och tumavtrycket för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="f3089-297">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="f3089-298">**För klassiska Kör som-konton:**</span><span class="sxs-lookup"><span data-stu-id="f3089-298">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="f3089-299">Skapar ett Automation-certifikattillgång med namnet *AzureClassicRunAsCertificate* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-299">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="f3089-300">Hej certifikattillgång innehåller hello certifikatets privata nyckel används av hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="f3089-300">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="f3089-301">Skapar ett Automation-anslutningstillgång med namnet *AzureClassicRunAsConnection* angiven i hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="f3089-301">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="f3089-302">Hej anslutningstillgång innehåller hello prenumerationsnamn, prenumerations-ID och certifikatnamn för tillgången.</span><span class="sxs-lookup"><span data-stu-id="f3089-302">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="f3089-303">Om du väljer något av alternativen för att skapa en klassisk kör som-konto när hello skriptet körs skapades överför hello offentliga certifikatarkiv (.cer-filnamnstillägget) toohello management för hello prenumeration som hello Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="f3089-303">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

<span data-ttu-id="f3089-304">tooexecute Hej skript och överför hello certifikat, hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3089-304">tooexecute hello script and upload hello certificate, do hello following:</span></span>

1. <span data-ttu-id="f3089-305">Spara hello följande skript på datorn.</span><span class="sxs-lookup"><span data-stu-id="f3089-305">Save hello following script on your computer.</span></span> <span data-ttu-id="f3089-306">I det här exemplet spara om filen med hello filename *ny RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="f3089-306">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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

2. <span data-ttu-id="f3089-307">Klicka på **Start** på datorn och starta sedan **Windows PowerShell** med utökade användarrättigheter.</span><span class="sxs-lookup"><span data-stu-id="f3089-307">On your computer, click **Start**, and then start **Windows PowerShell** with elevated user rights.</span></span>

3. <span data-ttu-id="f3089-308">Från hello utökade PowerShell kommandoradsgränssnitt, gå toohello mapp som innehåller hello-skript som du skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="f3089-308">From hello elevated PowerShell command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>

4. <span data-ttu-id="f3089-309">Kör hello-skript med hjälp av hello parametervärden för hello-konfiguration som du behöver.</span><span class="sxs-lookup"><span data-stu-id="f3089-309">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="f3089-310">**Skapa ett Kör som-konto med hjälp av ett självsignerat certifikat**</span><span class="sxs-lookup"><span data-stu-id="f3089-310">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="f3089-311">**Skapa ett Kör som-konto och ett klassiska Kör som-konto med hjälp av ett självsignerat certifikat**</span><span class="sxs-lookup"><span data-stu-id="f3089-311">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="f3089-312">**Skapa ett Kör som-konto och ett klassiskt Kör som-konto med hjälp av ett företagscertifikat**</span><span class="sxs-lookup"><span data-stu-id="f3089-312">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="f3089-313">**Skapa ett kör som-konto och klassiska kör som-konto med hjälp av ett självsignerat certifikat i hello Azure Government-moln**</span><span class="sxs-lookup"><span data-stu-id="f3089-313">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="f3089-314">Du kommer att tillfrågas tooauthenticate med Azure när hello skriptet har körts.</span><span class="sxs-lookup"><span data-stu-id="f3089-314">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="f3089-315">Logga in med ett konto som är medlem i administratörsrollen i hello prenumeration och medadministratör för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-315">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="f3089-316">Observera följande hello efter hello skriptet har körts:</span><span class="sxs-lookup"><span data-stu-id="f3089-316">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="f3089-317">Om du har skapat en klassiska kör som-konto med ett självsignerat certifikat med offentlig (.cer-fil) hello skriptet skapar och sparar toohello tillfällig mapp på datorn under hello användarprofil *%USERPROFILE%\AppData\Local\Temp*, som du använde tooexecute hello PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="f3089-317">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="f3089-318">Om du skapade ett klassiskt Kör som-konto med ett offentligt företagscertifikat (CER-fil) använder du det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="f3089-318">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="f3089-319">Följ anvisningarna för hello för [överför en hanterings-API certifikat toohello klassiska Azure-portalen](../azure-api-management-certs.md), och sedan Validera hello behörighetskonfigurationen med Service Management-resurser med hjälp av hello [exempelkod tooauthenticate med Service Management-resurser](#sample-code-to-authenticate-with-service-management-resources).</span><span class="sxs-lookup"><span data-stu-id="f3089-319">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with Service Management resources by using hello [sample code tooauthenticate with Service Management Resources](#sample-code-to-authenticate-with-service-management-resources).</span></span> 
* <span data-ttu-id="f3089-320">Om du har gjort *inte* skapa klassiska kör som-konton, autentisera med Resource Manager-resurser och validera hello autentiseringsuppgifter konfigurationen med hjälp av hello [exempelkod för att autentisera med Service Management resurser](#sample-code-to-authenticate-with-resource-manager-resources).</span><span class="sxs-lookup"><span data-stu-id="f3089-320">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](#sample-code-to-authenticate-with-resource-manager-resources).</span></span>

## <a name="sample-code-tooauthenticate-with-resource-manager-resources"></a><span data-ttu-id="f3089-321">Exempel kod tooauthenticate med Resource Manager-resurser</span><span class="sxs-lookup"><span data-stu-id="f3089-321">Sample code tooauthenticate with Resource Manager resources</span></span>
<span data-ttu-id="f3089-322">Du kan använda hello följande uppdaterad exempelkod tas från hello *AzureAutomationTutorialScript* exempel-runbook, tooauthenticate med hello kör som-konto toomanage Resource Manager-resurser med dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-322">You can use hello following updated sample code, taken from hello *AzureAutomationTutorialScript* example runbook, tooauthenticate by using hello Run As account toomanage Resource Manager resources with your runbooks.</span></span>

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

<span data-ttu-id="f3089-323">toohelp du tooeasily fungerar mellan flera prenumerationer, hello skriptet innehåller två ytterligare rader med kod som har stöd för refererar till en prenumerationskontext.</span><span class="sxs-lookup"><span data-stu-id="f3089-323">toohelp you tooeasily work between multiple subscriptions, hello script includes two additional lines of code that support referencing a subscription context.</span></span> <span data-ttu-id="f3089-324">En variabeltillgång med namnet *SubscriptionId* innehåller hello-ID för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f3089-324">A variable asset named *SubscriptionId* contains hello ID of hello subscription.</span></span> <span data-ttu-id="f3089-325">Efter hello `Add-AzureRmAccount` cmdlet instruktionen hello [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet anges med hello parameteruppsättning *- SubscriptionId*.</span><span class="sxs-lookup"><span data-stu-id="f3089-325">After hello `Add-AzureRmAccount` cmdlet statement, hello [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet is stated with hello parameter set *-SubscriptionId*.</span></span> <span data-ttu-id="f3089-326">Om hello variabelnamn för Allmänt kan du ändra tooinclude ett prefix till den eller använda en annan naming convention toomake den enklare tooidentify.</span><span class="sxs-lookup"><span data-stu-id="f3089-326">If hello variable name is too generic, you can revise it tooinclude a prefix or use another naming convention toomake it easier tooidentify.</span></span> <span data-ttu-id="f3089-327">Du kan också använda hello parameteruppsättning *- SubscriptionName* i stället för *- SubscriptionId* med en motsvarande variabeltillgång.</span><span class="sxs-lookup"><span data-stu-id="f3089-327">Alternatively, you can use hello parameter set *-SubscriptionName* instead of *-SubscriptionId* with a corresponding variable asset.</span></span>

<span data-ttu-id="f3089-328">Hej cmdlet som du använder för att autentisera i hello runbook `Add-AzureRmAccount`, använder hello *ServicePrincipalCertificate* parameteruppsättning.</span><span class="sxs-lookup"><span data-stu-id="f3089-328">hello cmdlet that you use for authenticating in hello runbook, `Add-AzureRmAccount`, uses hello *ServicePrincipalCertificate* parameter set.</span></span> <span data-ttu-id="f3089-329">Det autentiserar med hjälp av hello service principal certifikat, inte hello användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f3089-329">It authenticates by using hello service principal certificate, not hello user credentials.</span></span>

## <a name="sample-code-tooauthenticate-with-service-management-resources"></a><span data-ttu-id="f3089-330">Exempel kod tooauthenticate med Service Management-resurser</span><span class="sxs-lookup"><span data-stu-id="f3089-330">Sample code tooauthenticate with Service Management resources</span></span>
<span data-ttu-id="f3089-331">Du kan använda följande uppdaterad exempelkod som hämtas från hello hello *AzureClassicAutomationTutorialScript* exempel-runbook, tooauthenticate med hjälp av hello klassiska kör som-kontot toomanage klassiska resurser med din runbooks.</span><span class="sxs-lookup"><span data-stu-id="f3089-331">You can use hello following updated sample code, which is taken from hello *AzureClassicAutomationTutorialScript* example runbook, tooauthenticate by using hello Classic Run As account toomanage classic resources with your runbooks.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f3089-332">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3089-332">Next steps</span></span>
* [<span data-ttu-id="f3089-333">Program och tjänstobjekt i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3089-333">Application and service principal objects in Azure AD</span></span>](../active-directory/active-directory-application-objects.md)
* [<span data-ttu-id="f3089-334">Rollbaserad åtkomstkontroll i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="f3089-334">Role-based access control in Azure Automation</span></span>](automation-role-based-access-control.md)
* [<span data-ttu-id="f3089-335">Certifikatöversikt för Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="f3089-335">Certificates overview for Azure Cloud Services</span></span>](../cloud-services/cloud-services-certs-create.md)
