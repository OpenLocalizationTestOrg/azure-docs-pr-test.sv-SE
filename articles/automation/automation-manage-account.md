---
title: aaaManage Azure Automation-konto | Microsoft Docs
description: "Den här artikeln beskriver hur toomanage hello konfigurationen av ditt Automation-konto som certifikatförnyelse, borttagning och felaktig konfiguration."
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
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 75e41f601a138d4e8853aa79dcbab6696a5e9fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-automation-account"></a><span data-ttu-id="4a027-103">Hantera Azure Automation-konto</span><span class="sxs-lookup"><span data-stu-id="4a027-103">Manage Azure Automation account</span></span>
<span data-ttu-id="4a027-104">Någon gång innan det Automation-kontot upphör att gälla måste toorenew hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="4a027-104">At some point before your Automation account expires, you will need toorenew hello certificate.</span></span> <span data-ttu-id="4a027-105">Om du tror att hello kör som-konto har komprometterats kan du ta bort och skapa den igen.</span><span class="sxs-lookup"><span data-stu-id="4a027-105">If you believe that hello Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="4a027-106">Det här avsnittet beskrivs hur tooperform dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4a027-106">This section discusses how tooperform these operations.</span></span>

## <a name="self-signed-certificate-renewal"></a><span data-ttu-id="4a027-107">Förnya självsignerade certifikat</span><span class="sxs-lookup"><span data-stu-id="4a027-107">Self-signed certificate renewal</span></span>
<span data-ttu-id="4a027-108">hello självsignerat certifikat som du skapade för hello kör som-konto går ut ett år från hello dag skapas.</span><span class="sxs-lookup"><span data-stu-id="4a027-108">hello self-signed certificate that you created for hello Run As account expires one year from hello date of creation.</span></span> <span data-ttu-id="4a027-109">Du kan förnya det när som helst innan det upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="4a027-109">You can renew it at any time before it expires.</span></span> <span data-ttu-id="4a027-110">När du förnyar det är hello aktuella giltiga certifikat kvarhållna tooensure att alla runbooks som lagts i kö upp eller aktivt körs och som autentiserar med hello kör som-konto inte påverkas negativt.</span><span class="sxs-lookup"><span data-stu-id="4a027-110">When you renew it, hello current valid certificate is retained tooensure that any runbooks that are queued up or actively running, and that authenticate with hello Run As account, are not negatively affected.</span></span> <span data-ttu-id="4a027-111">hello intyg är giltigt fram till dess förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="4a027-111">hello certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="4a027-112">Om du har konfigurerat ditt Automation kör som-konto toouse ett certifikat utfärdat av en enterprise-certifikatutfärdare och du använder det här alternativet, ersätts hello företagscertifikat av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="4a027-112">If you have configured your Automation Run As account toouse a certificate issued by your enterprise certificate authority and you use this option, hello enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="4a027-113">toorenew hello certifikat, hello följande:</span><span class="sxs-lookup"><span data-stu-id="4a027-113">toorenew hello certificate, do hello following:</span></span>

1. <span data-ttu-id="4a027-114">Öppna hello Automation-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a027-114">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="4a027-115">På hello **Automation-konto** bladet i hello **konto egenskaper** rutan under **kontoinställningar**väljer **kör som-konton**.</span><span class="sxs-lookup"><span data-stu-id="4a027-115">On hello **Automation Account** blade, in hello **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Egenskapsrutan för Automation-konto](media/automation-manage-account/automation-account-properties-pane.png)
3. <span data-ttu-id="4a027-117">På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller hello klassiska kör som-konto som du vill ha toorenew hello certifikat för.</span><span class="sxs-lookup"><span data-stu-id="4a027-117">On hello **Run As Accounts** properties blade, select either hello Run As account or hello Classic Run As account that you want toorenew hello certificate for.</span></span>

4. <span data-ttu-id="4a027-118">På hello **egenskaper** bladet för hello valt konto, klickar du på **förnya certifikat**.</span><span class="sxs-lookup"><span data-stu-id="4a027-118">On hello **Properties** blade for hello selected account, click **Renew certificate**.</span></span>

    ![Förnya certifikat för Kör som-konto](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="4a027-120">Medan hello-certifikat förnyas du kan följa förloppet för hello under **meddelanden** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="4a027-120">While hello certificate is being renewed, you can track hello progress under **Notifications** from hello menu.</span></span>

## <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="4a027-121">Ta bort ett Kör som-konto eller ett klassiskt Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="4a027-121">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="4a027-122">Det här avsnittet beskrivs hur toodelete och sedan skapa ett kör som eller klassiska kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="4a027-122">This section describes how toodelete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="4a027-123">När du utför den här åtgärden sparas hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4a027-123">When you perform this action, hello Automation account is retained.</span></span> <span data-ttu-id="4a027-124">När du har tagit bort ett kör som eller klassiska kör som-konto kan skapa du den igen hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a027-124">After you delete a Run As or Classic Run As account, you can re-create it in hello Azure portal.</span></span>

1. <span data-ttu-id="4a027-125">Öppna hello Automation-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a027-125">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="4a027-126">På hello **automatiseringskontot** bladet i hello konto egenskapsrutan, Välj **kör som-konton**.</span><span class="sxs-lookup"><span data-stu-id="4a027-126">On hello **Automation account** blade, in hello account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="4a027-127">På hello **kör som-konton** egenskapsbladet, Välj antingen hello kör som-konto eller klassiska kör som-konto som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="4a027-127">On hello **Run As Accounts** properties blade, select either hello Run As account or Classic Run As account that you want toodelete.</span></span> <span data-ttu-id="4a027-128">Klicka sedan på hello **egenskaper** bladet för hello valt konto, klickar du på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="4a027-128">Then, on hello **Properties** blade for hello selected account, click **Delete**.</span></span>

 ![Ta bort Kör som-konto](media/automation-manage-account/automation-account-delete-runas.png)

4. <span data-ttu-id="4a027-130">Medan hello konto raderas, du kan följa förloppet hello under **meddelanden** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="4a027-130">While hello account is being deleted, you can track hello progress under **Notifications** from hello menu.</span></span>

5. <span data-ttu-id="4a027-131">När hello-konto har tagits bort, du kan skapa den igen på hello **kör som-konton** egenskapsbladet genom att välja hello skapa alternativet **Azure kör som-konto**.</span><span class="sxs-lookup"><span data-stu-id="4a027-131">After hello account has been deleted, you can re-create it on hello **Run As Accounts** properties blade by selecting hello create option **Azure Run As Account**.</span></span>

 ![Skapa nytt hello Automation kör som-konto](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a><span data-ttu-id="4a027-133">Felaktig konfiguration</span><span class="sxs-lookup"><span data-stu-id="4a027-133">Misconfiguration</span></span>
<span data-ttu-id="4a027-134">Vissa konfigurationsobjekt som är nödvändiga för hello kör som eller klassiska kör som-konto toofunction kanske korrekt har tagits bort eller skapats felaktigt under installationen.</span><span class="sxs-lookup"><span data-stu-id="4a027-134">Some configuration items necessary for hello Run As or Classic Run As account toofunction properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="4a027-135">hello-objekt är:</span><span class="sxs-lookup"><span data-stu-id="4a027-135">hello items include:</span></span>

* <span data-ttu-id="4a027-136">Certifikattillgång</span><span class="sxs-lookup"><span data-stu-id="4a027-136">Certificate asset</span></span>
* <span data-ttu-id="4a027-137">Anslutningstillgång</span><span class="sxs-lookup"><span data-stu-id="4a027-137">Connection asset</span></span>
* <span data-ttu-id="4a027-138">Kör som-konto har tagits bort från hello deltagarrollen</span><span class="sxs-lookup"><span data-stu-id="4a027-138">Run As account has been removed from hello contributor role</span></span>
* <span data-ttu-id="4a027-139">Huvudnamn för tjänsten eller program i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a027-139">Service principal or application in Azure AD</span></span>

<span data-ttu-id="4a027-140">I föregående hello och andra instanser av felaktig konfiguration hello Automation-konto identifierar hello ändras och visar statusen *ofullständigt* på hello **kör som-konton** egenskapsbladet för hello konto.</span><span class="sxs-lookup"><span data-stu-id="4a027-140">In hello preceding and other instances of misconfiguration, hello Automation account detects hello changes and displays a status of *Incomplete* on hello **Run As Accounts** properties blade for hello account.</span></span>

![Konfigurationsstatusen Ofullständig för Kör som-konto](media/automation-manage-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="4a027-142">När du väljer hello-kör som-konto hello konto **egenskaper** hello följande felmeddelande visas:</span><span class="sxs-lookup"><span data-stu-id="4a027-142">When you select hello Run As account, hello account **Properties** pane displays hello following error message:</span></span>

![Varningsmeddelande om ofullständig konfiguration av Kör som-konto](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="4a027-144">.</span><span class="sxs-lookup"><span data-stu-id="4a027-144">.</span></span>

<span data-ttu-id="4a027-145">Du kan snabbt lösa problemen kör som-konto genom att ta bort och återskapa hello-konto.</span><span class="sxs-lookup"><span data-stu-id="4a027-145">You can quickly resolve these Run As account issues by deleting and re-creating hello account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a027-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a027-146">Next steps</span></span>
* <span data-ttu-id="4a027-147">Mer information om tjänstens huvudnamn finns för[program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="4a027-147">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="4a027-148">Mer information om rollbaserad åtkomstkontroll i Azure Automation finns för[rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="4a027-148">For more information about Role-based Access Control in Azure Automation, refer too[Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="4a027-149">Mer information om certifikat och Azure-tjänster finns för[certifikat översikt för Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="4a027-149">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
