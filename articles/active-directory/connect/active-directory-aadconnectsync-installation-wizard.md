---
title: "Guiden Installera köra Azure AD Connect | Microsoft Docs"
description: "Beskriver hur installationsguiden fungerar andra gången du kör den."
keywords: "Installationsguiden för Azure AD Connect kan du konfigurera inställningar för underhåll den andra gången som du kör den"
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
ms.openlocfilehash: 42855b785c0ab334e33a622c8db912ce2438c627
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a><span data-ttu-id="adb4f-104">Azure AD Connect-synkronisering: guiden Installera körs en gång</span><span class="sxs-lookup"><span data-stu-id="adb4f-104">Azure AD Connect sync: Running the installation wizard a second time</span></span>
<span data-ttu-id="adb4f-105">Första gången du kör installationsguiden för Azure AD Connect vägleder den dig igenom hur du konfigurerar din installation.</span><span class="sxs-lookup"><span data-stu-id="adb4f-105">The first time you run the Azure AD Connect installation wizard, it walks you through how to configure your installation.</span></span> <span data-ttu-id="adb4f-106">Om du kör installationsguiden igen erbjuder alternativ för underhåll.</span><span class="sxs-lookup"><span data-stu-id="adb4f-106">If you run the installation wizard again, it offers options for maintenance.</span></span>

<span data-ttu-id="adb4f-107">Du hittar guiden i start-menyn med namnet **Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="adb4f-107">You can find the installation wizard in the start menu named **Azure AD Connect**.</span></span>

![Start-menyn](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

<span data-ttu-id="adb4f-109">När du startar guiden, visas en sida med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="adb4f-109">When you start the installation wizard, you see a page with these options:</span></span>

![Sida med en lista över ytterligare aktiviteter](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

<span data-ttu-id="adb4f-111">Om du har installerat AD FS med Azure AD Connect måste ha du ytterligare alternativ.</span><span class="sxs-lookup"><span data-stu-id="adb4f-111">If you have installed ADFS with Azure AD Connect, you have even more options.</span></span> <span data-ttu-id="adb4f-112">Ytterligare alternativ som du har för AD FS finns dokumenterade i [AD FS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="adb4f-112">The additional options you have for ADFS are documented in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span></span>

<span data-ttu-id="adb4f-113">Välj en av uppgifter och klicka på **nästa** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="adb4f-113">Select one of the tasks and click **Next** to continue.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="adb4f-114">När du har installationsguiden öppna har alla åtgärder i Synkroniseringsmotorn avbrutits.</span><span class="sxs-lookup"><span data-stu-id="adb4f-114">While you have the installation wizard open, all operations in the sync engine are suspended.</span></span> <span data-ttu-id="adb4f-115">Kontrollera att du stänger installationsguiden så snart du har slutfört konfigurationsändringarna.</span><span class="sxs-lookup"><span data-stu-id="adb4f-115">Make sure you close the installation wizard as soon as you have completed your configuration changes.</span></span>
>
>

## <a name="view-current-configuration"></a><span data-ttu-id="adb4f-116">Visa aktuell konfiguration</span><span class="sxs-lookup"><span data-stu-id="adb4f-116">View current configuration</span></span>
<span data-ttu-id="adb4f-117">Det här alternativet ger dig en snabb överblick över dina för tillfället konfigurerade alternativ.</span><span class="sxs-lookup"><span data-stu-id="adb4f-117">This option gives you a quick view of your currently configured options.</span></span>

![Sida med en lista över alla alternativ och deras tillstånd](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

<span data-ttu-id="adb4f-119">Klicka på **föregående** att gå tillbaka.</span><span class="sxs-lookup"><span data-stu-id="adb4f-119">Click **Previous** to go back.</span></span> <span data-ttu-id="adb4f-120">Om du väljer **avsluta**, du stänger installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="adb4f-120">If you select **Exit**, you close the installation wizard.</span></span>

## <a name="customize-synchronization-options"></a><span data-ttu-id="adb4f-121">Anpassa synkroniseringsalternativ</span><span class="sxs-lookup"><span data-stu-id="adb4f-121">Customize synchronization options</span></span>
<span data-ttu-id="adb4f-122">Det här alternativet används för att göra ändringar i konfigurationen för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="adb4f-122">This option is used to make changes to the sync configuration.</span></span> <span data-ttu-id="adb4f-123">Du ser en delmängd av alternativ från installationssökvägen anpassad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="adb4f-123">You see a subset of options from the custom configuration installation path.</span></span> <span data-ttu-id="adb4f-124">Det visas här alternativet även om du har använt Snabbinstallation först.</span><span class="sxs-lookup"><span data-stu-id="adb4f-124">You see this option even if you used express installation initially.</span></span>

* <span data-ttu-id="adb4f-125">[Lägga till flera kataloger](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span><span class="sxs-lookup"><span data-stu-id="adb4f-125">[Add more directories](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span></span> <span data-ttu-id="adb4f-126">För att ta bort en katalog, se [ta bort en koppling](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span><span class="sxs-lookup"><span data-stu-id="adb4f-126">For removing a directory, see [Delete a Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span></span>
* <span data-ttu-id="adb4f-127">[Ändra domän och Organisationsenhet filtrering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span><span class="sxs-lookup"><span data-stu-id="adb4f-127">[Change Domain and OU filtering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span></span>
* <span data-ttu-id="adb4f-128">Ta bort gruppfiltrering.</span><span class="sxs-lookup"><span data-stu-id="adb4f-128">Remove Group filtering.</span></span>
* <span data-ttu-id="adb4f-129">[Ändra valfria funktioner](active-directory-aadconnect-get-started-custom.md#optional-features).</span><span class="sxs-lookup"><span data-stu-id="adb4f-129">[Change optional features](active-directory-aadconnect-get-started-custom.md#optional-features).</span></span>

<span data-ttu-id="adb4f-130">Andra alternativ från den första installationen kan inte ändras och är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="adb4f-130">The other options from the initial installation cannot be changed and are not available.</span></span> <span data-ttu-id="adb4f-131">Dessa alternativ är:</span><span class="sxs-lookup"><span data-stu-id="adb4f-131">These options are:</span></span>

* <span data-ttu-id="adb4f-132">Ändra attributet som ska användas för userPrincipalName och sourceAnchor.</span><span class="sxs-lookup"><span data-stu-id="adb4f-132">Change the attribute to use for userPrincipalName and sourceAnchor.</span></span>
* <span data-ttu-id="adb4f-133">Ändra anslutande metod för objekt från en annan skog.</span><span class="sxs-lookup"><span data-stu-id="adb4f-133">Change the joining method for objects from different forest.</span></span>
* <span data-ttu-id="adb4f-134">Aktivera gruppbaserade filtrering.</span><span class="sxs-lookup"><span data-stu-id="adb4f-134">Enable group-based filtering.</span></span>

## <a name="refresh-directory-schema"></a><span data-ttu-id="adb4f-135">Uppdatera directory-schemat</span><span class="sxs-lookup"><span data-stu-id="adb4f-135">Refresh directory schema</span></span>
<span data-ttu-id="adb4f-136">Det här alternativet används om du har ändrat schemat i en av dina lokala AD DS-skogar.</span><span class="sxs-lookup"><span data-stu-id="adb4f-136">This option is used if you have changed the schema in one of your on-premises AD DS forests.</span></span> <span data-ttu-id="adb4f-137">Du kan till exempel har installerat Exchange eller uppgraderas till en Windows Server 2012-schemat med enhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="adb4f-137">For example, you might have installed Exchange or upgraded to a Windows Server 2012 schema with device objects.</span></span> <span data-ttu-id="adb4f-138">I det här fallet måste du instruera Azure AD Connect att läsa schemat igen från AD DS och uppdatera sin cache.</span><span class="sxs-lookup"><span data-stu-id="adb4f-138">In this case, you need to instruct Azure AD Connect to read the schema again from AD DS and update its cache.</span></span> <span data-ttu-id="adb4f-139">Den här åtgärden återskapar också regler för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="adb4f-139">This action also regenerates the Sync Rules.</span></span> <span data-ttu-id="adb4f-140">Om du lägger till Exchange-schemat som exempel har Sync-regler för Exchange lagts till konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="adb4f-140">If you add the Exchange schema, as an example, the Sync Rules for Exchange are added to the configuration.</span></span>

<span data-ttu-id="adb4f-141">När du väljer det här alternativet visas alla kataloger i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="adb4f-141">When you select this option, all the directories in your configuration are listed.</span></span> <span data-ttu-id="adb4f-142">Du kan behålla standardinställningen och uppdatera alla skogar eller avmarkera vissa av dessa.</span><span class="sxs-lookup"><span data-stu-id="adb4f-142">You can keep the default setting and refresh all forests or unselect some of them.</span></span>

![Sida med en lista över alla kataloger i miljön](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a><span data-ttu-id="adb4f-144">Konfigurera mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="adb4f-144">Configure staging mode</span></span>
<span data-ttu-id="adb4f-145">Det här alternativet kan du aktivera och inaktivera mellanlagringsläge på servern.</span><span class="sxs-lookup"><span data-stu-id="adb4f-145">This option allows you to enable and disable staging mode on the server.</span></span> <span data-ttu-id="adb4f-146">Mer information om mellanlagrade läge och hur de används finns i [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="adb4f-146">More information about staging mode and how it is used can be found in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span></span>

<span data-ttu-id="adb4f-147">Alternativet visar om mellanlagring är aktiverat eller inaktiverat:</span><span class="sxs-lookup"><span data-stu-id="adb4f-147">The option shows if staging is currently enabled or disabled:</span></span>  
![Alternativ som visas också det aktuella tillståndet för mellanlagringsläge](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

<span data-ttu-id="adb4f-149">Välj det här alternativet om du vill ändra tillståndet och markera eller avmarkera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="adb4f-149">To change the state, select this option and select or unselect the checkbox.</span></span>  
![Alternativ som visas också det aktuella tillståndet för mellanlagringsläge](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a><span data-ttu-id="adb4f-151">Ändra användarens inloggning</span><span class="sxs-lookup"><span data-stu-id="adb4f-151">Change user sign-in</span></span>
<span data-ttu-id="adb4f-152">Det här alternativet kan du ändra från synkronisering av lösenord till federation eller tvärtom.</span><span class="sxs-lookup"><span data-stu-id="adb4f-152">This option allows you to change from password sync to federation or the other way around.</span></span> <span data-ttu-id="adb4f-153">Du kan inte ändra till **konfigurerar inte**.</span><span class="sxs-lookup"><span data-stu-id="adb4f-153">You cannot change to **do not configure**.</span></span>

<span data-ttu-id="adb4f-154">Mer information om det här alternativet, se [användarinloggning](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span><span class="sxs-lookup"><span data-stu-id="adb4f-154">For more information on this option, see [user sign-in](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span></span>

## <a name="next-steps"></a><span data-ttu-id="adb4f-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="adb4f-155">Next steps</span></span>
* <span data-ttu-id="adb4f-156">Mer information om av konfigurationsmodellen som Azure AD Connect-synkronisering i [förstå deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="adb4f-156">Learn more about the configuration model used by Azure AD Connect sync in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>

<span data-ttu-id="adb4f-157">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="adb4f-157">**Overview topics**</span></span>

* [<span data-ttu-id="adb4f-158">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="adb4f-158">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="adb4f-159">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="adb4f-159">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
