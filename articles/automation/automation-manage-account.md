---
title: Hantera Azure Automation-konto | Microsoft Docs
description: "Den här artikeln beskriver hur du hanterar konfigurationen av Automation-kontot, till exempel certifikatförnyelse, borttagning och felaktig konfiguration."
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
ms.openlocfilehash: 41efdbcacede74bac038342688362ff480cadc7e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-automation-account"></a><span data-ttu-id="5f7ba-103">Hantera Azure Automation-konto</span><span class="sxs-lookup"><span data-stu-id="5f7ba-103">Manage Azure Automation account</span></span>
<span data-ttu-id="5f7ba-104">Någon gång innan Automation-kontot går ut måste du förnya certifikatet.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-104">At some point before your Automation account expires, you will need to renew the certificate.</span></span> <span data-ttu-id="5f7ba-105">Om du tror att ditt Kör som-konto har komprometterats kan du ta bort och återskapa det.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-105">If you believe that the Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="5f7ba-106">Det här avsnittet beskriver hur du utför dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-106">This section discusses how to perform these operations.</span></span>

## <a name="self-signed-certificate-renewal"></a><span data-ttu-id="5f7ba-107">Förnya självsignerade certifikat</span><span class="sxs-lookup"><span data-stu-id="5f7ba-107">Self-signed certificate renewal</span></span>
<span data-ttu-id="5f7ba-108">Det självsignerade certifikatet som du skapade för Kör som-kontot går ut ett år efter det datum då det skapades.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-108">The self-signed certificate that you created for the Run As account expires one year from the date of creation.</span></span> <span data-ttu-id="5f7ba-109">Du kan förnya det när som helst innan det upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-109">You can renew it at any time before it expires.</span></span> <span data-ttu-id="5f7ba-110">När du förnyar det behålls det aktuella giltiga certifikatet för att säkerställa att alla runbookflöden som placerats i kö eller som körs aktivt, och som autentiserar med Kör som-kontot, inte påverkas negativt.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-110">When you renew it, the current valid certificate is retained to ensure that any runbooks that are queued up or actively running, and that authenticate with the Run As account, are not negatively affected.</span></span> <span data-ttu-id="5f7ba-111">Certifikatet förblir giltigt fram till dess förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-111">The certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="5f7ba-112">Om du har konfigurerat ditt Kör som-konto för Automation att använda ett certifikat som utfärdats av din företagscertifikatutfärdare och du använder det här alternativet, så ersätts företagscertifikatet av ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-112">If you have configured your Automation Run As account to use a certificate issued by your enterprise certificate authority and you use this option, the enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="5f7ba-113">Du förnyar certifikatet genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="5f7ba-113">To renew the certificate, do the following:</span></span>

1. <span data-ttu-id="5f7ba-114">Öppna ditt Automation-konto på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-114">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="5f7ba-115">På bladet **Automation-konto** väljer du **Kör som-konton** under **Kontoinställningar** i rutan **Kontoegenskaper**.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-115">On the **Automation Account** blade, in the **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Egenskapsrutan för Automation-konto](media/automation-manage-account/automation-account-properties-pane.png)
3. <span data-ttu-id="5f7ba-117">På egenskapsbladet för **Kör som-konton** väljer du antingen Kör som-kontot eller det klassiska Kör som-kontot som du vill förnya certifikatet för.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-117">On the **Run As Accounts** properties blade, select either the Run As account or the Classic Run As account that you want to renew the certificate for.</span></span>

4. <span data-ttu-id="5f7ba-118">På bladet **Egenskaper** för det valda kontot klickar du på **Förnya certifikat**.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-118">On the **Properties** blade for the selected account, click **Renew certificate**.</span></span>

    ![Förnya certifikat för Kör som-konto](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="5f7ba-120">Medan certifikatet förnyas kan du följa förloppet under **Meddelanden** på menyn.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-120">While the certificate is being renewed, you can track the progress under **Notifications** from the menu.</span></span>

## <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="5f7ba-121">Ta bort ett Kör som-konto eller ett klassiskt Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="5f7ba-121">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="5f7ba-122">Det här avsnittet beskriver hur du tar bort och sedan återskapar ett Kör som-konto eller ett klassiskt Kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-122">This section describes how to delete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="5f7ba-123">När du utför den här åtgärden behålls Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-123">When you perform this action, the Automation account is retained.</span></span> <span data-ttu-id="5f7ba-124">När du har tagit bort ett Kör som-konto eller ett klassiskt Kör som-konto kan du återskapa det på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-124">After you delete a Run As or Classic Run As account, you can re-create it in the Azure portal.</span></span>

1. <span data-ttu-id="5f7ba-125">Öppna ditt Automation-konto på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-125">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="5f7ba-126">På bladet **Automation-konto** väljer du **Kör som-konton** i rutan för kontoegenskaper.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-126">On the **Automation account** blade, in the account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="5f7ba-127">På egenskapsbladet för **Kör som-konton** väljer du antingen Kör som-kontot eller det klassiska Kör som-kontot som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-127">On the **Run As Accounts** properties blade, select either the Run As account or Classic Run As account that you want to delete.</span></span> <span data-ttu-id="5f7ba-128">Klicka på **Ta bort** på bladet **Egenskaper** för det valda kontot.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-128">Then, on the **Properties** blade for the selected account, click **Delete**.</span></span>

 ![Ta bort Kör som-konto](media/automation-manage-account/automation-account-delete-runas.png)

4. <span data-ttu-id="5f7ba-130">Medan kontot tas bort kan du följa förloppet under **Meddelanden** på menyn.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-130">While the account is being deleted, you can track the progress under **Notifications** from the menu.</span></span>

5. <span data-ttu-id="5f7ba-131">När kontot har tagits bort måste du återskapa det på egenskapsbladet för **Kör som-konton** genom att välja alternativet Skapa för **Kör som-konto i Azure**.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-131">After the account has been deleted, you can re-create it on the **Run As Accounts** properties blade by selecting the create option **Azure Run As Account**.</span></span>

 ![Återskapa Kör som-kontot för Automation](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a><span data-ttu-id="5f7ba-133">Felaktig konfiguration</span><span class="sxs-lookup"><span data-stu-id="5f7ba-133">Misconfiguration</span></span>
<span data-ttu-id="5f7ba-134">Vissa konfigurationsobjekt som krävs för att Kör som-kontot eller det klassiska Kör som-kontot ska fungera kanske har tagits bort eller kanske inte skapades korrekt under den ursprungliga konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-134">Some configuration items necessary for the Run As or Classic Run As account to function properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="5f7ba-135">Exempel på dessa objekt är:</span><span class="sxs-lookup"><span data-stu-id="5f7ba-135">The items include:</span></span>

* <span data-ttu-id="5f7ba-136">Certifikattillgång</span><span class="sxs-lookup"><span data-stu-id="5f7ba-136">Certificate asset</span></span>
* <span data-ttu-id="5f7ba-137">Anslutningstillgång</span><span class="sxs-lookup"><span data-stu-id="5f7ba-137">Connection asset</span></span>
* <span data-ttu-id="5f7ba-138">Kör som-konto har tagits bort från deltagarrollen</span><span class="sxs-lookup"><span data-stu-id="5f7ba-138">Run As account has been removed from the contributor role</span></span>
* <span data-ttu-id="5f7ba-139">Huvudnamn för tjänsten eller program i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f7ba-139">Service principal or application in Azure AD</span></span>

<span data-ttu-id="5f7ba-140">I den föregående instansen och andra instanser av felaktiga konfigurationer identifierar Automation-kontot ändringarna och visar statusen *Ofullständig* på egenskapsbladet för **Kör som-konton** för kontot.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-140">In the preceding and other instances of misconfiguration, the Automation account detects the changes and displays a status of *Incomplete* on the **Run As Accounts** properties blade for the account.</span></span>

![Konfigurationsstatusen Ofullständig för Kör som-konto](media/automation-manage-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="5f7ba-142">När du väljer Kör som-kontot visas följande felmeddelande i rutan **Egenskaper** för kontot:</span><span class="sxs-lookup"><span data-stu-id="5f7ba-142">When you select the Run As account, the account **Properties** pane displays the following error message:</span></span>

![Varningsmeddelande om ofullständig konfiguration av Kör som-konto](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="5f7ba-144">.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-144">.</span></span>

<span data-ttu-id="5f7ba-145">Du kan snabbt lösa dessa problem med Kör som-kontot genom att ta bort och återskapa kontot.</span><span class="sxs-lookup"><span data-stu-id="5f7ba-145">You can quickly resolve these Run As account issues by deleting and re-creating the account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f7ba-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f7ba-146">Next steps</span></span>
* <span data-ttu-id="5f7ba-147">Mer information om tjänstobjekt finns i [Programobjekt och tjänstobjekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="5f7ba-147">For more information about Service Principals, refer to [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="5f7ba-148">Mer information om rollbaserad åtkomstkontroll i Azure Automation finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5f7ba-148">For more information about Role-based Access Control in Azure Automation, refer to [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="5f7ba-149">Mer information om certifikat och Azure-tjänster finns i [Översikt över certifikat för Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="5f7ba-149">For more information about certificates and Azure services, refer to [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>