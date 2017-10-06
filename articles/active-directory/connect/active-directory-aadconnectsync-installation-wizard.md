---
title: "Nytt körs hello Azure AD Connect-installationsguiden | Microsoft Docs"
description: "Beskriver hur fungerar hello installationsguiden hello andra gången du kör den."
keywords: "hello Azure AD Connect-installationsguiden kan du konfigurera Underhåll inställningar hello andra gången du kör den"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 83cc74aca471ef9b4f65f7f3582e3e48d3d81cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-running-hello-installation-wizard-a-second-time"></a><span data-ttu-id="cb81f-104">Azure AD Connect-synkronisering: hello installationsguiden körs en gång</span><span class="sxs-lookup"><span data-stu-id="cb81f-104">Azure AD Connect sync: Running hello installation wizard a second time</span></span>
<span data-ttu-id="cb81f-105">hello första gången du kör installationsguiden för hello Azure AD Connect den vägleder dig igenom hur tooconfigure din installation.</span><span class="sxs-lookup"><span data-stu-id="cb81f-105">hello first time you run hello Azure AD Connect installation wizard, it walks you through how tooconfigure your installation.</span></span> <span data-ttu-id="cb81f-106">Om du kör installationsguiden för hello igen erbjuder alternativ för underhåll.</span><span class="sxs-lookup"><span data-stu-id="cb81f-106">If you run hello installation wizard again, it offers options for maintenance.</span></span>

<span data-ttu-id="cb81f-107">Du kan hitta hello installationsguiden hello start-menyn med namnet **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="cb81f-107">You can find hello installation wizard in hello start menu named **Azure AD Connect**.</span></span>

![Start-menyn](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

<span data-ttu-id="cb81f-109">När du startar installationsguiden för hello visas en sida med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="cb81f-109">When you start hello installation wizard, you see a page with these options:</span></span>

![Sida med en lista över ytterligare aktiviteter](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

<span data-ttu-id="cb81f-111">Om du har installerat AD FS med Azure AD Connect måste ha du ytterligare alternativ.</span><span class="sxs-lookup"><span data-stu-id="cb81f-111">If you have installed ADFS with Azure AD Connect, you have even more options.</span></span> <span data-ttu-id="cb81f-112">Hej ytterligare alternativ som du har för AD FS finns dokumenterade i [AD FS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="cb81f-112">hello additional options you have for ADFS are documented in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span></span>

<span data-ttu-id="cb81f-113">Välj en av hello uppgifter och klicka på **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="cb81f-113">Select one of hello tasks and click **Next** toocontinue.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb81f-114">När du har hello installationsguiden öppna har alla åtgärder i hello Synkroniseringsmotorn avbrutits.</span><span class="sxs-lookup"><span data-stu-id="cb81f-114">While you have hello installation wizard open, all operations in hello sync engine are suspended.</span></span> <span data-ttu-id="cb81f-115">Kontrollera att du stänger installationsguiden hello så snart du har slutfört konfigurationsändringarna.</span><span class="sxs-lookup"><span data-stu-id="cb81f-115">Make sure you close hello installation wizard as soon as you have completed your configuration changes.</span></span>
>
>

## <a name="view-current-configuration"></a><span data-ttu-id="cb81f-116">Visa aktuell konfiguration</span><span class="sxs-lookup"><span data-stu-id="cb81f-116">View current configuration</span></span>
<span data-ttu-id="cb81f-117">Det här alternativet ger dig en snabb överblick över dina för tillfället konfigurerade alternativ.</span><span class="sxs-lookup"><span data-stu-id="cb81f-117">This option gives you a quick view of your currently configured options.</span></span>

![Sida med en lista över alla alternativ och deras tillstånd](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

<span data-ttu-id="cb81f-119">Klicka på **föregående** toogo tillbaka.</span><span class="sxs-lookup"><span data-stu-id="cb81f-119">Click **Previous** toogo back.</span></span> <span data-ttu-id="cb81f-120">Om du väljer **avsluta**, Stäng av hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="cb81f-120">If you select **Exit**, you close hello installation wizard.</span></span>

## <a name="customize-synchronization-options"></a><span data-ttu-id="cb81f-121">Anpassa synkroniseringsalternativ</span><span class="sxs-lookup"><span data-stu-id="cb81f-121">Customize synchronization options</span></span>
<span data-ttu-id="cb81f-122">Det här alternativet är används toomake ändringar toohello synkroniseringskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="cb81f-122">This option is used toomake changes toohello sync configuration.</span></span> <span data-ttu-id="cb81f-123">Du ser en delmängd av alternativ från hello installationssökväg för anpassad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="cb81f-123">You see a subset of options from hello custom configuration installation path.</span></span> <span data-ttu-id="cb81f-124">Det visas här alternativet även om du har använt Snabbinstallation först.</span><span class="sxs-lookup"><span data-stu-id="cb81f-124">You see this option even if you used express installation initially.</span></span>

* <span data-ttu-id="cb81f-125">[Lägga till flera kataloger](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span><span class="sxs-lookup"><span data-stu-id="cb81f-125">[Add more directories](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span></span> <span data-ttu-id="cb81f-126">För att ta bort en katalog, se [ta bort en koppling](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span><span class="sxs-lookup"><span data-stu-id="cb81f-126">For removing a directory, see [Delete a Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span></span>
* <span data-ttu-id="cb81f-127">[Ändra domän och Organisationsenhet filtrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span><span class="sxs-lookup"><span data-stu-id="cb81f-127">[Change Domain and OU filtering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span></span>
* <span data-ttu-id="cb81f-128">Ta bort gruppfiltrering.</span><span class="sxs-lookup"><span data-stu-id="cb81f-128">Remove Group filtering.</span></span>
* <span data-ttu-id="cb81f-129">[Ändra valfria funktioner](active-directory-aadconnect-get-started-custom.md#optional-features).</span><span class="sxs-lookup"><span data-stu-id="cb81f-129">[Change optional features](active-directory-aadconnect-get-started-custom.md#optional-features).</span></span>

<span data-ttu-id="cb81f-130">hello andra alternativ från hello första installationen kan inte ändras och är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="cb81f-130">hello other options from hello initial installation cannot be changed and are not available.</span></span> <span data-ttu-id="cb81f-131">Dessa alternativ är:</span><span class="sxs-lookup"><span data-stu-id="cb81f-131">These options are:</span></span>

* <span data-ttu-id="cb81f-132">Ändra hello attributet toouse för userPrincipalName och sourceAnchor.</span><span class="sxs-lookup"><span data-stu-id="cb81f-132">Change hello attribute toouse for userPrincipalName and sourceAnchor.</span></span>
* <span data-ttu-id="cb81f-133">Ändra hello koppla metod för objekt från en annan skog.</span><span class="sxs-lookup"><span data-stu-id="cb81f-133">Change hello joining method for objects from different forest.</span></span>
* <span data-ttu-id="cb81f-134">Aktivera gruppbaserade filtrering.</span><span class="sxs-lookup"><span data-stu-id="cb81f-134">Enable group-based filtering.</span></span>

## <a name="refresh-directory-schema"></a><span data-ttu-id="cb81f-135">Uppdatera directory-schemat</span><span class="sxs-lookup"><span data-stu-id="cb81f-135">Refresh directory schema</span></span>
<span data-ttu-id="cb81f-136">Det här alternativet används om du har ändrat hello schema i en av dina lokala AD DS-skogar.</span><span class="sxs-lookup"><span data-stu-id="cb81f-136">This option is used if you have changed hello schema in one of your on-premises AD DS forests.</span></span> <span data-ttu-id="cb81f-137">Till exempel du kanske har installerat Exchange eller uppgraderat tooa Windows Server 2012-schemat med enhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="cb81f-137">For example, you might have installed Exchange or upgraded tooa Windows Server 2012 schema with device objects.</span></span> <span data-ttu-id="cb81f-138">I det här fallet behöver Azure AD Connect tooinstruct tooread hello schema igen från AD DS och uppdatera sin cache.</span><span class="sxs-lookup"><span data-stu-id="cb81f-138">In this case, you need tooinstruct Azure AD Connect tooread hello schema again from AD DS and update its cache.</span></span> <span data-ttu-id="cb81f-139">Den här åtgärden även återskapar hello Sync regler.</span><span class="sxs-lookup"><span data-stu-id="cb81f-139">This action also regenerates hello Sync Rules.</span></span> <span data-ttu-id="cb81f-140">Om du lägger till hello Exchange-schemat, som exempel läggs hello Sync regler för Exchange toohello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="cb81f-140">If you add hello Exchange schema, as an example, hello Sync Rules for Exchange are added toohello configuration.</span></span>

<span data-ttu-id="cb81f-141">När du väljer det här alternativet visas alla hello kataloger i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="cb81f-141">When you select this option, all hello directories in your configuration are listed.</span></span> <span data-ttu-id="cb81f-142">Du kan behålla hello standardinställningen och uppdatera alla skogar eller avmarkera vissa av dessa.</span><span class="sxs-lookup"><span data-stu-id="cb81f-142">You can keep hello default setting and refresh all forests or unselect some of them.</span></span>

![Sida med en lista över alla kataloger i hello-miljö](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a><span data-ttu-id="cb81f-144">Konfigurera mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="cb81f-144">Configure staging mode</span></span>
<span data-ttu-id="cb81f-145">Det här alternativet kan du tooenable och inaktivera mellanlagringsläge på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="cb81f-145">This option allows you tooenable and disable staging mode on hello server.</span></span> <span data-ttu-id="cb81f-146">Mer information om mellanlagrade läge och hur de används finns i [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="cb81f-146">More information about staging mode and how it is used can be found in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span></span>

<span data-ttu-id="cb81f-147">hello alternativet visar om mellanlagring är aktiverat eller inaktiverat:</span><span class="sxs-lookup"><span data-stu-id="cb81f-147">hello option shows if staging is currently enabled or disabled:</span></span>  
![Alternativ som visas också hello aktuell status för mellanlagringsläge](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

<span data-ttu-id="cb81f-149">toochange Hej tillstånd, markerar du det här alternativet och markera eller avmarkera hello.</span><span class="sxs-lookup"><span data-stu-id="cb81f-149">toochange hello state, select this option and select or unselect hello checkbox.</span></span>  
![Alternativ som visas också hello aktuell status för mellanlagringsläge](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a><span data-ttu-id="cb81f-151">Ändra användarens inloggning</span><span class="sxs-lookup"><span data-stu-id="cb81f-151">Change user sign-in</span></span>
<span data-ttu-id="cb81f-152">Det här alternativet kan du toochange från toofederation för synkronisering av lösenord eller hello tvärtom.</span><span class="sxs-lookup"><span data-stu-id="cb81f-152">This option allows you toochange from password sync toofederation or hello other way around.</span></span> <span data-ttu-id="cb81f-153">Du kan inte ändra för**konfigurerar inte**.</span><span class="sxs-lookup"><span data-stu-id="cb81f-153">You cannot change too**do not configure**.</span></span>

<span data-ttu-id="cb81f-154">Mer information om det här alternativet, se [användarinloggning](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span><span class="sxs-lookup"><span data-stu-id="cb81f-154">For more information on this option, see [user sign-in](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb81f-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb81f-155">Next steps</span></span>
* <span data-ttu-id="cb81f-156">Mer information om hello Konfigurationsmodell som används av Azure AD Connect-synkronisering i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="cb81f-156">Learn more about hello configuration model used by Azure AD Connect sync in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>

<span data-ttu-id="cb81f-157">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="cb81f-157">**Overview topics**</span></span>

* [<span data-ttu-id="cb81f-158">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="cb81f-158">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="cb81f-159">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb81f-159">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
