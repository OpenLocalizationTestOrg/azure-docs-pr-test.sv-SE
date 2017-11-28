---
title: "Azure AD Connect: Nästa steg och hur toomanage Azure AD Connect | Microsoft Docs"
description: "Lär dig hur tooextend hello standardkonfigurationen och operativa uppgifter för Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4404aaff24d54d76b83baca3b331a6a250ba4c03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="next-steps-and-how-toomanage-azure-ad-connect"></a><span data-ttu-id="6e72d-103">Nästa steg och hur toomanage Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="6e72d-103">Next steps and how toomanage Azure AD Connect</span></span>
<span data-ttu-id="6e72d-104">Använd hello operativa procedurer i den här artikeln toocustomize Azure Active Directory (AD Azure) Anslut toomeet din organisations behov och krav.</span><span class="sxs-lookup"><span data-stu-id="6e72d-104">Use hello operational procedures in this article toocustomize Azure Active Directory (Azure AD) Connect toomeet your organization's needs and requirements.</span></span>  

## <a name="add-additional-sync-admins"></a><span data-ttu-id="6e72d-105">Lägg till ytterligare sync-administratörer</span><span class="sxs-lookup"><span data-stu-id="6e72d-105">Add additional sync admins</span></span>
<span data-ttu-id="6e72d-106">Som standard är endast hello-användaren som hello installation och lokala administratörer kan toomanage hello installerat Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="6e72d-106">By default, only hello user who did hello installation and local admins are able toomanage hello installed sync engine.</span></span> <span data-ttu-id="6e72d-107">För andra personer toobe kan tooaccess och hantera hello Synkroniseringsmotorn, leta upp hello-gruppen ADSyncAdmins på hello lokala servern och lägga till dem toothis grupp.</span><span class="sxs-lookup"><span data-stu-id="6e72d-107">For additional people toobe able tooaccess and manage hello sync engine, locate hello group named ADSyncAdmins on hello local server and add them toothis group.</span></span>

## <a name="assign-licenses-tooazure-ad-premium-and-enterprise-mobility-suite-users"></a><span data-ttu-id="6e72d-108">Tilldela licenser tooAzure AD Premium och Enterprise Mobility Suite-användare</span><span class="sxs-lookup"><span data-stu-id="6e72d-108">Assign licenses tooAzure AD Premium and Enterprise Mobility Suite users</span></span>
<span data-ttu-id="6e72d-109">Nu när användarna har synkroniserats toohello moln, behöver du tooassign dem en licens så att de kan komma igång med molnappar som Office 365.</span><span class="sxs-lookup"><span data-stu-id="6e72d-109">Now that your users have been synchronized toohello cloud, you need tooassign them a license so they can get going with cloud apps such as Office 365.</span></span>

### <a name="tooassign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a><span data-ttu-id="6e72d-110">tooassign en Azure AD Premium eller Enterprise Mobility Suite-licens</span><span class="sxs-lookup"><span data-stu-id="6e72d-110">tooassign an Azure AD Premium or Enterprise Mobility Suite License</span></span>

1. <span data-ttu-id="6e72d-111">Logga in toohello Azure-portalen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="6e72d-111">Sign in toohello Azure portal as an admin.</span></span>
2. <span data-ttu-id="6e72d-112">Hello vänster markerar **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-112">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="6e72d-113">På hello **Active Directory** dubbelklickar hello directory som har hello-användare som du vill att tooset.</span><span class="sxs-lookup"><span data-stu-id="6e72d-113">On hello **Active Directory** page, double-click hello directory that has hello users you want tooset up.</span></span>
4. <span data-ttu-id="6e72d-114">Överst hello i hello katalogsidan, Välj **licenser**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-114">At hello top of hello directory page, select **Licenses**.</span></span>
5. <span data-ttu-id="6e72d-115">På hello **licenser** väljer **Active Directory Premium** eller **Enterprise Mobility Suite**, och klicka sedan på **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-115">On hello **Licenses** page, select **Active Directory Premium** or **Enterprise Mobility Suite**, and then click **Assign**.</span></span>
6. <span data-ttu-id="6e72d-116">Välj hello användare tooassign licenser till, och klicka sedan på hello markerat ikonen toosave hello ändringar hello i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6e72d-116">In hello dialog box, select hello users you want tooassign licenses to, and then click hello check mark icon toosave hello changes.</span></span>

## <a name="verify-hello-scheduled-synchronization-task"></a><span data-ttu-id="6e72d-117">Verifiera hello schemalagd synkroniseringsaktiviteten</span><span class="sxs-lookup"><span data-stu-id="6e72d-117">Verify hello scheduled synchronization task</span></span>
<span data-ttu-id="6e72d-118">Använd hello Azure portal toocheck hello status för en synkronisering.</span><span class="sxs-lookup"><span data-stu-id="6e72d-118">Use hello Azure portal toocheck hello status of a synchronization.</span></span>

### <a name="tooverify-hello-scheduled-synchronization-task"></a><span data-ttu-id="6e72d-119">tooverify hello schemalagd synkronisering aktivitet</span><span class="sxs-lookup"><span data-stu-id="6e72d-119">tooverify hello scheduled synchronization task</span></span>
1. <span data-ttu-id="6e72d-120">Logga in toohello Azure-portalen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="6e72d-120">Sign in toohello Azure portal as an admin.</span></span>
2. <span data-ttu-id="6e72d-121">Hello vänster markerar **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-121">On hello left, select **Active Directory**.</span></span>
3. <span data-ttu-id="6e72d-122">På hello **Active Directory** dubbelklickar hello directory som har hello-användare som du vill att tooset.</span><span class="sxs-lookup"><span data-stu-id="6e72d-122">On hello **Active Directory** page, double-click hello directory that has hello users you want tooset up.</span></span>
4. <span data-ttu-id="6e72d-123">Överst hello i hello katalogsidan, Välj **katalogintegrering**.</span><span class="sxs-lookup"><span data-stu-id="6e72d-123">At hello top of hello directory page, select **Directory Integration**.</span></span>
5. <span data-ttu-id="6e72d-124">Under **integrering med lokala active directory**, Observera hello tid för senaste synkronisering.</span><span class="sxs-lookup"><span data-stu-id="6e72d-124">Under **integration with local active directory**, note hello last sync time.</span></span>

<span data-ttu-id="6e72d-125"><center>![Tid för Directory-synkronisering](./media/active-directory-aadconnect-whats-next/verify.png)</center></span><span class="sxs-lookup"><span data-stu-id="6e72d-125"><center>![Directory sync time](./media/active-directory-aadconnect-whats-next/verify.png)</center></span></span>

## <a name="start-a-scheduled-synchronization-task"></a><span data-ttu-id="6e72d-126">Starta en schemalagd synkronisering-aktivitet</span><span class="sxs-lookup"><span data-stu-id="6e72d-126">Start a scheduled synchronization task</span></span>
<span data-ttu-id="6e72d-127">Om du behöver toorun en synkroniseringsuppgiften kan göra du detta genom att köra hello Azure AD Connect-guiden igen.</span><span class="sxs-lookup"><span data-stu-id="6e72d-127">If you need toorun a synchronization task, you can do this by running through hello Azure AD Connect wizard again.</span></span>  <span data-ttu-id="6e72d-128">Du måste tooprovide dina autentiseringsuppgifter för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e72d-128">You need tooprovide your Azure AD credentials.</span></span>  <span data-ttu-id="6e72d-129">Hello i guiden väljer du hello **anpassa synkroniseringsalternativ** och på **nästa** toomove hello guiden.</span><span class="sxs-lookup"><span data-stu-id="6e72d-129">In hello wizard, select hello **Customize synchronization options** task, and click **Next** toomove through hello wizard.</span></span> <span data-ttu-id="6e72d-130">Se till att hello i slutet av hello **startar hello synkroniseringsprocessen så snart hello initiala konfigurationen är klar** är markerad.</span><span class="sxs-lookup"><span data-stu-id="6e72d-130">At hello end, ensure that hello **Start hello synchronization process as soon as hello initial configuration completes** box is selected.</span></span>

<span data-ttu-id="6e72d-131"><center>![Starta synkroniseringen](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span><span class="sxs-lookup"><span data-stu-id="6e72d-131"><center>![Start synchronization](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span></span>

<span data-ttu-id="6e72d-132">Mer information om hello Azure AD Connect sync Scheduler finns [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="6e72d-132">For more information on hello Azure AD Connect sync Scheduler, see [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span>

## <a name="additional-tasks-available-in-azure-ad-connect"></a><span data-ttu-id="6e72d-133">Ytterligare uppgifter som är tillgängliga i Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="6e72d-133">Additional tasks available in Azure AD Connect</span></span>
<span data-ttu-id="6e72d-134">Efter den inledande installationen av Azure AD Connect, kan du alltid starta hello guiden igen från hello Azure AD Connect start sida eller fjärrskrivbord snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="6e72d-134">After your initial installation of Azure AD Connect, you can always start hello wizard again from hello Azure AD Connect start page or desktop shortcut.</span></span>  <span data-ttu-id="6e72d-135">Du kommer att märka att gå igenom guiden hello igen ger nya alternativ i hello form av ytterligare uppgifter.</span><span class="sxs-lookup"><span data-stu-id="6e72d-135">You will notice that going through hello wizard again provides some new options in hello form of additional tasks.</span></span>  

<span data-ttu-id="6e72d-136">hello följande tabell innehåller en sammanfattning av dessa uppgifter och en kort beskrivning av varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6e72d-136">hello following table provides a summary of these tasks and a brief description of each task.</span></span>

![Lista över ytterligare aktiviteter](./media/active-directory-aadconnect-whats-next/addtasks.png)

| <span data-ttu-id="6e72d-138">Ytterligare uppgifter</span><span class="sxs-lookup"><span data-stu-id="6e72d-138">Additional task</span></span> | <span data-ttu-id="6e72d-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6e72d-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6e72d-140">**Visa hello valda scenario**</span><span class="sxs-lookup"><span data-stu-id="6e72d-140">**View hello selected scenario**</span></span> |<span data-ttu-id="6e72d-141">Visa din aktuella Azure AD Connect-lösning.</span><span class="sxs-lookup"><span data-stu-id="6e72d-141">View your current Azure AD Connect solution.</span></span>  <span data-ttu-id="6e72d-142">Detta inkluderar allmänna inställningar synkroniseras kataloger och synkroniseringsinställningar.</span><span class="sxs-lookup"><span data-stu-id="6e72d-142">This includes general settings, synchronized directories, and sync settings.</span></span> |
| <span data-ttu-id="6e72d-143">**Anpassa synkroniseringsalternativ**</span><span class="sxs-lookup"><span data-stu-id="6e72d-143">**Customize synchronization options**</span></span> |<span data-ttu-id="6e72d-144">Ändra hello aktuella konfigurationen som lägger till ytterligare Active Directory-skogar toohello konfiguration eller aktivera synkroniseringsalternativ, till exempel användare, grupp, enhet eller tillbakaskrivning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="6e72d-144">Change hello current configuration like adding additional Active Directory forests toohello configuration, or enabling sync options such as user, group, device, or password write-back.</span></span> |
| <span data-ttu-id="6e72d-145">**Aktivera Mellanlagringsläge**</span><span class="sxs-lookup"><span data-stu-id="6e72d-145">**Enable Staging Mode**</span></span> |<span data-ttu-id="6e72d-146">Steg-information som synkroniseras inte omedelbart och kan inte exporteras tooAzure AD eller lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e72d-146">Stage information that is not immediately synchronized and is not exported tooAzure AD or on-premises Active Directory.</span></span>  <span data-ttu-id="6e72d-147">Med den här funktionen kan du förhandsgranska hello synkroniseringar innan de inträffar.</span><span class="sxs-lookup"><span data-stu-id="6e72d-147">With this feature, you can preview hello synchronizations before they occur.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6e72d-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6e72d-148">Next steps</span></span>
<span data-ttu-id="6e72d-149">Lär dig mer om [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="6e72d-149">Learn more about [integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
