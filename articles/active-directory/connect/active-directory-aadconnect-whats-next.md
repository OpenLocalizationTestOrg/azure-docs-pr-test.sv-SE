---
title: "Azure AD Connect: Nästa steg och hur du hanterar Azure AD Connect | Microsoft Docs"
description: "Lär dig hur du utökar standardkonfigurationen och operativa uppgifter för Azure AD Connect."
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
ms.openlocfilehash: beace24fa00c85a5038a3c39ae8f76af5fd12111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a><span data-ttu-id="8c061-103">Nästa steg och hur du hanterar Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="8c061-103">Next steps and how to manage Azure AD Connect</span></span>
<span data-ttu-id="8c061-104">Använd operativa procedurer i den här artikeln för att anpassa Azure Active Directory (AD Azure) Anslut för att uppfylla organisationens behov och krav.</span><span class="sxs-lookup"><span data-stu-id="8c061-104">Use the operational procedures in this article to customize Azure Active Directory (Azure AD) Connect to meet your organization's needs and requirements.</span></span>  

## <a name="add-additional-sync-admins"></a><span data-ttu-id="8c061-105">Lägg till ytterligare sync-administratörer</span><span class="sxs-lookup"><span data-stu-id="8c061-105">Add additional sync admins</span></span>
<span data-ttu-id="8c061-106">Som standard bara användaren som gjorde installationen och lokala administratörer kan hantera installerade Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="8c061-106">By default, only the user who did the installation and local admins are able to manage the installed sync engine.</span></span> <span data-ttu-id="8c061-107">För ytterligare användare kan komma åt och hantera Synkroniseringsmotorn, leta upp gruppen ADSyncAdmins på den lokala servern och lägga till dem i den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="8c061-107">For additional people to be able to access and manage the sync engine, locate the group named ADSyncAdmins on the local server and add them to this group.</span></span>

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a><span data-ttu-id="8c061-108">Tilldela licenser till användare i Azure AD Premium och Enterprise Mobility Suite</span><span class="sxs-lookup"><span data-stu-id="8c061-108">Assign licenses to Azure AD Premium and Enterprise Mobility Suite users</span></span>
<span data-ttu-id="8c061-109">Nu när användarna har synkroniserats till molnet, måste du tilldela dem en licens så att de kan komma igång med molnappar som Office 365.</span><span class="sxs-lookup"><span data-stu-id="8c061-109">Now that your users have been synchronized to the cloud, you need to assign them a license so they can get going with cloud apps such as Office 365.</span></span>

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a><span data-ttu-id="8c061-110">Så här tilldelar du en Azure AD Premium eller Enterprise Mobility Suite-licens</span><span class="sxs-lookup"><span data-stu-id="8c061-110">To assign an Azure AD Premium or Enterprise Mobility Suite License</span></span>

1. <span data-ttu-id="8c061-111">Logga in på Azure-portalen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="8c061-111">Sign in to the Azure portal as an admin.</span></span>
2. <span data-ttu-id="8c061-112">Välj **Active Directory** till vänster.</span><span class="sxs-lookup"><span data-stu-id="8c061-112">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="8c061-113">På den **Active Directory** sidan genom att dubbelklicka på katalogen som har användare som du vill konfigurera.</span><span class="sxs-lookup"><span data-stu-id="8c061-113">On the **Active Directory** page, double-click the directory that has the users you want to set up.</span></span>
4. <span data-ttu-id="8c061-114">Överst på katalogsidan väljer du **Licenser**.</span><span class="sxs-lookup"><span data-stu-id="8c061-114">At the top of the directory page, select **Licenses**.</span></span>
5. <span data-ttu-id="8c061-115">På den **licenser** väljer **Active Directory Premium** eller **Enterprise Mobility Suite**, och klicka sedan på **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="8c061-115">On the **Licenses** page, select **Active Directory Premium** or **Enterprise Mobility Suite**, and then click **Assign**.</span></span>
6. <span data-ttu-id="8c061-116">I dialogrutan väljer du de användare som du vill tilldela licenser till och klickar sedan på bockmarkeringen för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="8c061-116">In the dialog box, select the users you want to assign licenses to, and then click the check mark icon to save the changes.</span></span>

## <a name="verify-the-scheduled-synchronization-task"></a><span data-ttu-id="8c061-117">Kontrollera aktiviteten schemalagd synkronisering</span><span class="sxs-lookup"><span data-stu-id="8c061-117">Verify the scheduled synchronization task</span></span>
<span data-ttu-id="8c061-118">Använda Azure portal för att kontrollera status för en synkronisering.</span><span class="sxs-lookup"><span data-stu-id="8c061-118">Use the Azure portal to check the status of a synchronization.</span></span>

### <a name="to-verify-the-scheduled-synchronization-task"></a><span data-ttu-id="8c061-119">Att verifiera aktiviteten schemalagd synkronisering</span><span class="sxs-lookup"><span data-stu-id="8c061-119">To verify the scheduled synchronization task</span></span>
1. <span data-ttu-id="8c061-120">Logga in på Azure-portalen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="8c061-120">Sign in to the Azure portal as an admin.</span></span>
2. <span data-ttu-id="8c061-121">Välj **Active Directory** till vänster.</span><span class="sxs-lookup"><span data-stu-id="8c061-121">On the left, select **Active Directory**.</span></span>
3. <span data-ttu-id="8c061-122">På den **Active Directory** sidan genom att dubbelklicka på katalogen som har användare som du vill konfigurera.</span><span class="sxs-lookup"><span data-stu-id="8c061-122">On the **Active Directory** page, double-click the directory that has the users you want to set up.</span></span>
4. <span data-ttu-id="8c061-123">Markera överst på sidan directory **katalogintegrering**.</span><span class="sxs-lookup"><span data-stu-id="8c061-123">At the top of the directory page, select **Directory Integration**.</span></span>
5. <span data-ttu-id="8c061-124">Under **integrering med lokala active directory**, Observera tid för senaste synkronisering.</span><span class="sxs-lookup"><span data-stu-id="8c061-124">Under **integration with local active directory**, note the last sync time.</span></span>

<span data-ttu-id="8c061-125"><center>![Tid för Directory-synkronisering](./media/active-directory-aadconnect-whats-next/verify.png)</center></span><span class="sxs-lookup"><span data-stu-id="8c061-125"><center>![Directory sync time](./media/active-directory-aadconnect-whats-next/verify.png)</center></span></span>

## <a name="start-a-scheduled-synchronization-task"></a><span data-ttu-id="8c061-126">Starta en schemalagd synkronisering-aktivitet</span><span class="sxs-lookup"><span data-stu-id="8c061-126">Start a scheduled synchronization task</span></span>
<span data-ttu-id="8c061-127">Om du behöver köra en aktivitet för synkronisering kan du göra detta genom att köra Azure AD Connect-guiden igen.</span><span class="sxs-lookup"><span data-stu-id="8c061-127">If you need to run a synchronization task, you can do this by running through the Azure AD Connect wizard again.</span></span>  <span data-ttu-id="8c061-128">Du måste ange dina autentiseringsuppgifter för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c061-128">You need to provide your Azure AD credentials.</span></span>  <span data-ttu-id="8c061-129">I guiden väljer du den **anpassa synkroniseringsalternativ** och på **nästa** att flytta via guiden.</span><span class="sxs-lookup"><span data-stu-id="8c061-129">In the wizard, select the **Customize synchronization options** task, and click **Next** to move through the wizard.</span></span> <span data-ttu-id="8c061-130">Se till att i slutet av **starta synkroniseringsprocessen så snart som den initiala konfigurationen är klar** är markerad.</span><span class="sxs-lookup"><span data-stu-id="8c061-130">At the end, ensure that the **Start the synchronization process as soon as the initial configuration completes** box is selected.</span></span>

<span data-ttu-id="8c061-131"><center>![Starta synkroniseringen](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span><span class="sxs-lookup"><span data-stu-id="8c061-131"><center>![Start synchronization](./media/active-directory-aadconnect-whats-next/startsynch.png)</center></span></span>

<span data-ttu-id="8c061-132">Mer information om Azure AD Connect sync Scheduler finns [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span><span class="sxs-lookup"><span data-stu-id="8c061-132">For more information on the Azure AD Connect sync Scheduler, see [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).</span></span>

## <a name="additional-tasks-available-in-azure-ad-connect"></a><span data-ttu-id="8c061-133">Ytterligare uppgifter som är tillgängliga i Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="8c061-133">Additional tasks available in Azure AD Connect</span></span>
<span data-ttu-id="8c061-134">Efter den inledande installationen av Azure AD Connect, kan du alltid starta guiden igen från Azure AD Connect start sidan eller skrivbord genväg.</span><span class="sxs-lookup"><span data-stu-id="8c061-134">After your initial installation of Azure AD Connect, you can always start the wizard again from the Azure AD Connect start page or desktop shortcut.</span></span>  <span data-ttu-id="8c061-135">Du kommer att märka att gå igenom guiden igen ger nya alternativ i form av ytterligare uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8c061-135">You will notice that going through the wizard again provides some new options in the form of additional tasks.</span></span>  

<span data-ttu-id="8c061-136">Följande tabell innehåller en sammanfattning av dessa uppgifter och en kort beskrivning av varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8c061-136">The following table provides a summary of these tasks and a brief description of each task.</span></span>

![Lista över ytterligare aktiviteter](./media/active-directory-aadconnect-whats-next/addtasks.png)

| <span data-ttu-id="8c061-138">Ytterligare uppgifter</span><span class="sxs-lookup"><span data-stu-id="8c061-138">Additional task</span></span> | <span data-ttu-id="8c061-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8c061-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8c061-140">**Visa det valda scenariot**</span><span class="sxs-lookup"><span data-stu-id="8c061-140">**View the selected scenario**</span></span> |<span data-ttu-id="8c061-141">Visa din aktuella Azure AD Connect-lösning.</span><span class="sxs-lookup"><span data-stu-id="8c061-141">View your current Azure AD Connect solution.</span></span>  <span data-ttu-id="8c061-142">Detta inkluderar allmänna inställningar synkroniseras kataloger och synkroniseringsinställningar.</span><span class="sxs-lookup"><span data-stu-id="8c061-142">This includes general settings, synchronized directories, and sync settings.</span></span> |
| <span data-ttu-id="8c061-143">**Anpassa synkroniseringsalternativ**</span><span class="sxs-lookup"><span data-stu-id="8c061-143">**Customize synchronization options**</span></span> |<span data-ttu-id="8c061-144">Ändra den aktuella konfigurationen som att lägga till ytterligare Active Directory-skogar konfigurationen eller aktivera synkroniseringsalternativ, till exempel användare, grupp, enhet eller tillbakaskrivning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="8c061-144">Change the current configuration like adding additional Active Directory forests to the configuration, or enabling sync options such as user, group, device, or password write-back.</span></span> |
| <span data-ttu-id="8c061-145">**Aktivera Mellanlagringsläge**</span><span class="sxs-lookup"><span data-stu-id="8c061-145">**Enable Staging Mode**</span></span> |<span data-ttu-id="8c061-146">Steg-information som synkroniseras inte omedelbart och kan inte exporteras till Azure AD eller lokala Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8c061-146">Stage information that is not immediately synchronized and is not exported to Azure AD or on-premises Active Directory.</span></span>  <span data-ttu-id="8c061-147">Med den här funktionen kan du förhandsgranska synkroniseringar innan de inträffar.</span><span class="sxs-lookup"><span data-stu-id="8c061-147">With this feature, you can preview the synchronizations before they occur.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8c061-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c061-148">Next steps</span></span>
<span data-ttu-id="8c061-149">Lär dig mer om [integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="8c061-149">Learn more about [integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
