---
title: "Kopplingar i Azure AD Synchronization Service Manager-Gränssnittet | Microsoft Docs"
description: "Förstå fliken kopplingar i hanteraren för synkroniseringstjänsten för Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0fae4b1755ca95466eeffb5ce61c1c7855d7381
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a><span data-ttu-id="6885f-103">Med hjälp av anslutningar med Azure AD Connect Sync Service Manager</span><span class="sxs-lookup"><span data-stu-id="6885f-103">Using connectors with the Azure AD Connect Sync Service Manager</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

<span data-ttu-id="6885f-105">Fliken kopplingar används för att hantera alla system Synkroniseringsmotorn är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="6885f-105">The Connectors tab is used to manage all systems the sync engine is connected to.</span></span>

## <a name="connector-actions"></a><span data-ttu-id="6885f-106">Åtgärder för kopplingen</span><span class="sxs-lookup"><span data-stu-id="6885f-106">Connector actions</span></span>
| <span data-ttu-id="6885f-107">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="6885f-107">Action</span></span> | <span data-ttu-id="6885f-108">Kommentar</span><span class="sxs-lookup"><span data-stu-id="6885f-108">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="6885f-109">Skapa</span><span class="sxs-lookup"><span data-stu-id="6885f-109">Create</span></span> |<span data-ttu-id="6885f-110">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="6885f-110">Do not use.</span></span> <span data-ttu-id="6885f-111">Använd installationsguiden för att ansluta till ytterligare AD-skogar.</span><span class="sxs-lookup"><span data-stu-id="6885f-111">For connecting to additional AD forests, use the installation wizard.</span></span> |
| <span data-ttu-id="6885f-112">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="6885f-112">Properties</span></span> |<span data-ttu-id="6885f-113">Används för domän- och organisationsenhetsfiltrering.</span><span class="sxs-lookup"><span data-stu-id="6885f-113">Used for domain and OU filtering.</span></span> |
| [<span data-ttu-id="6885f-114">Ta bort</span><span class="sxs-lookup"><span data-stu-id="6885f-114">Delete</span></span>](#delete) |<span data-ttu-id="6885f-115">För att ta bort data i anslutningsplatsen eller ta bort anslutningen till en skog.</span><span class="sxs-lookup"><span data-stu-id="6885f-115">Used to either delete the data in the connector space or to delete connection to a forest.</span></span> |
| [<span data-ttu-id="6885f-116">Konfigurera körningsprofiler</span><span class="sxs-lookup"><span data-stu-id="6885f-116">Configure Run Profiles</span></span>](#configure-run-profiles) |<span data-ttu-id="6885f-117">Förutom domän filtrering, inget att konfigurera här.</span><span class="sxs-lookup"><span data-stu-id="6885f-117">Except for domain filtering, nothing to configure here.</span></span> <span data-ttu-id="6885f-118">Du kan använda den här åtgärden för att se redan konfigurerade körning av profiler.</span><span class="sxs-lookup"><span data-stu-id="6885f-118">You can use this action to see already configured run profiles.</span></span> |
| <span data-ttu-id="6885f-119">Kör</span><span class="sxs-lookup"><span data-stu-id="6885f-119">Run</span></span> |<span data-ttu-id="6885f-120">Används för att starta en oneoff körning av en profil.</span><span class="sxs-lookup"><span data-stu-id="6885f-120">Used to start a one-off run of a profile.</span></span> |
| <span data-ttu-id="6885f-121">Stoppa</span><span class="sxs-lookup"><span data-stu-id="6885f-121">Stop</span></span> |<span data-ttu-id="6885f-122">Stoppar en koppling som körs på en profil.</span><span class="sxs-lookup"><span data-stu-id="6885f-122">Stops a Connector currently running a profile.</span></span> |
| <span data-ttu-id="6885f-123">Exportera koppling</span><span class="sxs-lookup"><span data-stu-id="6885f-123">Export Connector</span></span> |<span data-ttu-id="6885f-124">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="6885f-124">Do not use.</span></span> |
| <span data-ttu-id="6885f-125">Importera koppling</span><span class="sxs-lookup"><span data-stu-id="6885f-125">Import Connector</span></span> |<span data-ttu-id="6885f-126">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="6885f-126">Do not use.</span></span> |
| <span data-ttu-id="6885f-127">Uppdatera anslutningen</span><span class="sxs-lookup"><span data-stu-id="6885f-127">Update Connector</span></span> |<span data-ttu-id="6885f-128">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="6885f-128">Do not use.</span></span> |
| <span data-ttu-id="6885f-129">Uppdatera Schema</span><span class="sxs-lookup"><span data-stu-id="6885f-129">Refresh Schema</span></span> |<span data-ttu-id="6885f-130">Uppdaterar cachelagrade schemat.</span><span class="sxs-lookup"><span data-stu-id="6885f-130">Refreshes the cached schema.</span></span> <span data-ttu-id="6885f-131">Det är att föredra att använda alternativet i installationsguiden i stället eftersom som också uppdateringar synkroniseras regler.</span><span class="sxs-lookup"><span data-stu-id="6885f-131">It is preferred to use the option in the installation wizard instead, since that also updates sync rules.</span></span> |
| [<span data-ttu-id="6885f-132">Söka Anslutarplats</span><span class="sxs-lookup"><span data-stu-id="6885f-132">Search Connector Space</span></span>](#search-connector-space) |<span data-ttu-id="6885f-133">Används för att söka efter objekt och [följer ett objekt och dess data genom systemet](#follow-an-object-and-its-data-through-the-system).</span><span class="sxs-lookup"><span data-stu-id="6885f-133">Used to find objects and to [Follow an object and its data through the system](#follow-an-object-and-its-data-through-the-system).</span></span> |

### <a name="delete"></a><span data-ttu-id="6885f-134">Ta bort</span><span class="sxs-lookup"><span data-stu-id="6885f-134">Delete</span></span>
<span data-ttu-id="6885f-135">Åtgärden ta bort används för två olika saker.</span><span class="sxs-lookup"><span data-stu-id="6885f-135">The delete action is used for two different things.</span></span>  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

<span data-ttu-id="6885f-137">Alternativet **ta bort anslutningsplatsen endast** tar bort alla data, men behålla konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6885f-137">The option **Delete connector space only** removes all data, but keep the configuration.</span></span>

<span data-ttu-id="6885f-138">Alternativet **ta bort kopplingen och connector** tar bort data och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6885f-138">The option **Delete Connector and connector space** removes the data and the configuration.</span></span> <span data-ttu-id="6885f-139">Det här alternativet används när du inte vill ansluta till en skog längre.</span><span class="sxs-lookup"><span data-stu-id="6885f-139">This option is used when you do not want to connect to a forest anymore.</span></span>

<span data-ttu-id="6885f-140">Båda alternativen synkronisera alla objekt och uppdatera metaversum-objekt.</span><span class="sxs-lookup"><span data-stu-id="6885f-140">Both options sync all objects and update the metaverse objects.</span></span> <span data-ttu-id="6885f-141">Den här åtgärden är en tidskrävande åtgärd.</span><span class="sxs-lookup"><span data-stu-id="6885f-141">This action is a long running operation.</span></span>

### <a name="configure-run-profiles"></a><span data-ttu-id="6885f-142">Konfigurera körningsprofiler</span><span class="sxs-lookup"><span data-stu-id="6885f-142">Configure Run Profiles</span></span>
<span data-ttu-id="6885f-143">Det här alternativet kan du se körningsprofiler som konfigurerats för en koppling.</span><span class="sxs-lookup"><span data-stu-id="6885f-143">This option allows you to see the run profiles configured for a Connector.</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a><span data-ttu-id="6885f-145">Söka Anslutarplats</span><span class="sxs-lookup"><span data-stu-id="6885f-145">Search Connector Space</span></span>
<span data-ttu-id="6885f-146">Sökåtgärd connector utrymme är användbar för att söka efter objekt och felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="6885f-146">The search connector space action is useful to find objects and troubleshoot data issues.</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

<span data-ttu-id="6885f-148">Starta genom att välja en **omfång**.</span><span class="sxs-lookup"><span data-stu-id="6885f-148">Start by selecting a **scope**.</span></span> <span data-ttu-id="6885f-149">Du kan söka baserat på data (RDN DN-fästpunkt, underträd) eller läget för objektet (alla andra alternativ).</span><span class="sxs-lookup"><span data-stu-id="6885f-149">You can search based on data (RDN, DN, Anchor, Sub-Tree), or state of the object (all other options).</span></span>  
<span data-ttu-id="6885f-150">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span><span class="sxs-lookup"><span data-stu-id="6885f-150">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span></span>  
<span data-ttu-id="6885f-151">Om du till exempel göra en underträd sökning får du alla objekt i en Organisationsenhet.</span><span class="sxs-lookup"><span data-stu-id="6885f-151">If you for example do a Sub-Tree search, you get all objects in one OU.</span></span>  
<span data-ttu-id="6885f-152">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span><span class="sxs-lookup"><span data-stu-id="6885f-152">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span></span>  
<span data-ttu-id="6885f-153">Från den här rutnät som du kan välja ett objekt, Välj **egenskaper**, och [följa den](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) från anslutningsplatsen källa via metaversum och att målet anslutningsplatsen.</span><span class="sxs-lookup"><span data-stu-id="6885f-153">From this grid you can select an object, select **properties**, and [follow it](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) from the source connector space, through the metaverse, and to the target connector space.</span></span>

### <a name="changing-the-ad-ds-account-password"></a><span data-ttu-id="6885f-154">Ändra lösenordet för AD DS</span><span class="sxs-lookup"><span data-stu-id="6885f-154">Changing the AD DS account password</span></span>
<span data-ttu-id="6885f-155">Om du ändrar lösenordet för synkroniseringstjänsten inte längre att kunna importera och exportera ändringar i lokala AD.</span><span class="sxs-lookup"><span data-stu-id="6885f-155">If you change the account password, the Synchronization Service will no longer be able to import/export changes to on-premises AD.</span></span>   <span data-ttu-id="6885f-156">Du kan se följande:</span><span class="sxs-lookup"><span data-stu-id="6885f-156">You may see the following:</span></span>

- <span data-ttu-id="6885f-157">Importera och exportera steget för AD-anslutningen misslyckas med felet ”inga-start-autentiseringsuppgifter”.</span><span class="sxs-lookup"><span data-stu-id="6885f-157">The import/export step for the AD connector fails with "no-start-credentials" error.</span></span>
- <span data-ttu-id="6885f-158">Under Windows Loggboken innehåller programhändelseloggen ett fel med händelse-ID 6000 och meddelandet ”hanteringsagenten” contoso.com ”kunde inte köras eftersom autentiseringsuppgifterna var ogiltiga”.</span><span class="sxs-lookup"><span data-stu-id="6885f-158">Under Windows Event Viewer, the application event log contains an error with Event ID 6000 and message “The management agent “contoso.com” failed to run because the credentials were invalid.”</span></span>

<span data-ttu-id="6885f-159">Lös problemet genom att uppdatera AD DS-användarkonto med hjälp av följande:</span><span class="sxs-lookup"><span data-stu-id="6885f-159">To resolve the issue, update the AD DS user account using the following:</span></span>


1. <span data-ttu-id="6885f-160">Starta hanteraren för synkroniseringstjänsten (START → synkroniseringstjänsten).</span><span class="sxs-lookup"><span data-stu-id="6885f-160">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="6885f-161">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="6885f-161">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>
2. <span data-ttu-id="6885f-162">Gå till den **kopplingar** fliken.</span><span class="sxs-lookup"><span data-stu-id="6885f-162">Go to the **Connectors** tab.</span></span>
3. <span data-ttu-id="6885f-163">Välj den AD-anslutning som är konfigurerat för att använda AD DS-konto.</span><span class="sxs-lookup"><span data-stu-id="6885f-163">Select the AD Connector which is configured to use the AD DS account.</span></span>
4. <span data-ttu-id="6885f-164">Välj under åtgärder, **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="6885f-164">Under Actions, select **Properties**.</span></span>
5. <span data-ttu-id="6885f-165">Välj Anslut till Active Directory-skog i popup-fönstret:</span><span class="sxs-lookup"><span data-stu-id="6885f-165">In the pop-up dialog, select Connect to Active Directory Forest:</span></span>
6. <span data-ttu-id="6885f-166">Skogens namn anger det motsvarande lokalt AD.</span><span class="sxs-lookup"><span data-stu-id="6885f-166">The Forest name indicates the corresponding on-prem AD.</span></span>
7. <span data-ttu-id="6885f-167">Användarnamnet anger AD DS-konto som används för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="6885f-167">The User name indicates the AD DS account used for synchronization.</span></span>
8. <span data-ttu-id="6885f-168">Ange det nya lösenordet för AD DS-konto i lösenordsrutan ![Azure AD Connect Sync kryptering nyckeln Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="6885f-168">Enter the new password of the AD DS account in the Password textbox ![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>
9. <span data-ttu-id="6885f-169">Klicka på OK för att spara det nya lösenordet och starta om synkroniseringstjänsten för att ta bort det gamla lösenordet från cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="6885f-169">Click OK to save the new password and restart the Synchronization Service to remove the old password from memory cache.</span></span>



## <a name="next-steps"></a><span data-ttu-id="6885f-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6885f-170">Next steps</span></span>
<span data-ttu-id="6885f-171">Lär dig mer om den [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6885f-171">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="6885f-172">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="6885f-172">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
