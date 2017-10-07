---
title: aaaConnectors i hello Azure AD Synchronization Service Manager UI | Microsoft Docs
description: "Förstå hello kopplingar fliken i hello Synchronization Service Manager för Azure AD Connect."
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
ms.openlocfilehash: c0969630313178b1e299385b1289360c8f787cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-with-hello-azure-ad-connect-sync-service-manager"></a><span data-ttu-id="9043a-103">Med hjälp av anslutningar med hello Azure AD Connect Sync Service Manager</span><span class="sxs-lookup"><span data-stu-id="9043a-103">Using connectors with hello Azure AD Connect Sync Service Manager</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

<span data-ttu-id="9043a-105">hello är kopplingar används toomanage alla system hello Synkroniseringsmotorn är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="9043a-105">hello Connectors tab is used toomanage all systems hello sync engine is connected to.</span></span>

## <a name="connector-actions"></a><span data-ttu-id="9043a-106">Åtgärder för kopplingen</span><span class="sxs-lookup"><span data-stu-id="9043a-106">Connector actions</span></span>
| <span data-ttu-id="9043a-107">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="9043a-107">Action</span></span> | <span data-ttu-id="9043a-108">Kommentar</span><span class="sxs-lookup"><span data-stu-id="9043a-108">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="9043a-109">Skapa</span><span class="sxs-lookup"><span data-stu-id="9043a-109">Create</span></span> |<span data-ttu-id="9043a-110">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="9043a-110">Do not use.</span></span> <span data-ttu-id="9043a-111">Använda hello installationsguiden för att ansluta tooadditional AD-skogar.</span><span class="sxs-lookup"><span data-stu-id="9043a-111">For connecting tooadditional AD forests, use hello installation wizard.</span></span> |
| <span data-ttu-id="9043a-112">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="9043a-112">Properties</span></span> |<span data-ttu-id="9043a-113">Används för domän- och organisationsenhetsfiltrering.</span><span class="sxs-lookup"><span data-stu-id="9043a-113">Used for domain and OU filtering.</span></span> |
| [<span data-ttu-id="9043a-114">Ta bort</span><span class="sxs-lookup"><span data-stu-id="9043a-114">Delete</span></span>](#delete) |<span data-ttu-id="9043a-115">Använda tooeither hello data i hello connector utrymme eller toodelete anslutning tooa skog tas bort.</span><span class="sxs-lookup"><span data-stu-id="9043a-115">Used tooeither delete hello data in hello connector space or toodelete connection tooa forest.</span></span> |
| [<span data-ttu-id="9043a-116">Konfigurera körningsprofiler</span><span class="sxs-lookup"><span data-stu-id="9043a-116">Configure Run Profiles</span></span>](#configure-run-profiles) |<span data-ttu-id="9043a-117">Förutom domän filtrering ingenting tooconfigure här.</span><span class="sxs-lookup"><span data-stu-id="9043a-117">Except for domain filtering, nothing tooconfigure here.</span></span> <span data-ttu-id="9043a-118">Du kan använda den här åtgärden körningsprofiler toosee som redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="9043a-118">You can use this action toosee already configured run profiles.</span></span> |
| <span data-ttu-id="9043a-119">Kör</span><span class="sxs-lookup"><span data-stu-id="9043a-119">Run</span></span> |<span data-ttu-id="9043a-120">Använda toostart som ett tillfälligt körning av en profil.</span><span class="sxs-lookup"><span data-stu-id="9043a-120">Used toostart a one-off run of a profile.</span></span> |
| <span data-ttu-id="9043a-121">Stoppa</span><span class="sxs-lookup"><span data-stu-id="9043a-121">Stop</span></span> |<span data-ttu-id="9043a-122">Stoppar en koppling som körs på en profil.</span><span class="sxs-lookup"><span data-stu-id="9043a-122">Stops a Connector currently running a profile.</span></span> |
| <span data-ttu-id="9043a-123">Exportera koppling</span><span class="sxs-lookup"><span data-stu-id="9043a-123">Export Connector</span></span> |<span data-ttu-id="9043a-124">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="9043a-124">Do not use.</span></span> |
| <span data-ttu-id="9043a-125">Importera koppling</span><span class="sxs-lookup"><span data-stu-id="9043a-125">Import Connector</span></span> |<span data-ttu-id="9043a-126">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="9043a-126">Do not use.</span></span> |
| <span data-ttu-id="9043a-127">Uppdatera anslutningen</span><span class="sxs-lookup"><span data-stu-id="9043a-127">Update Connector</span></span> |<span data-ttu-id="9043a-128">Använd inte.</span><span class="sxs-lookup"><span data-stu-id="9043a-128">Do not use.</span></span> |
| <span data-ttu-id="9043a-129">Uppdatera Schema</span><span class="sxs-lookup"><span data-stu-id="9043a-129">Refresh Schema</span></span> |<span data-ttu-id="9043a-130">Uppdaterar hello cachelagrade schemat.</span><span class="sxs-lookup"><span data-stu-id="9043a-130">Refreshes hello cached schema.</span></span> <span data-ttu-id="9043a-131">Det är prioriterade toouse hello alternativ i installationsguiden för hello i stället eftersom som också uppdateringar synkroniseras regler.</span><span class="sxs-lookup"><span data-stu-id="9043a-131">It is preferred toouse hello option in hello installation wizard instead, since that also updates sync rules.</span></span> |
| [<span data-ttu-id="9043a-132">Söka Anslutarplats</span><span class="sxs-lookup"><span data-stu-id="9043a-132">Search Connector Space</span></span>](#search-connector-space) |<span data-ttu-id="9043a-133">Toofind objekt som används och för[följer ett objekt och dess data via hello system](#follow-an-object-and-its-data-through-the-system).</span><span class="sxs-lookup"><span data-stu-id="9043a-133">Used toofind objects and too[Follow an object and its data through hello system](#follow-an-object-and-its-data-through-the-system).</span></span> |

### <a name="delete"></a><span data-ttu-id="9043a-134">Ta bort</span><span class="sxs-lookup"><span data-stu-id="9043a-134">Delete</span></span>
<span data-ttu-id="9043a-135">hello borttagningsåtgärden används för två olika saker.</span><span class="sxs-lookup"><span data-stu-id="9043a-135">hello delete action is used for two different things.</span></span>  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

<span data-ttu-id="9043a-137">Hej alternativet **ta bort anslutningsplatsen endast** tar bort alla data, men behålla hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9043a-137">hello option **Delete connector space only** removes all data, but keep hello configuration.</span></span>

<span data-ttu-id="9043a-138">Hej alternativet **ta bort kopplingen och connector** tar bort hello data och hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9043a-138">hello option **Delete Connector and connector space** removes hello data and hello configuration.</span></span> <span data-ttu-id="9043a-139">Det här alternativet används när du inte vill att tooconnect tooa skog längre.</span><span class="sxs-lookup"><span data-stu-id="9043a-139">This option is used when you do not want tooconnect tooa forest anymore.</span></span>

<span data-ttu-id="9043a-140">Båda alternativen synkronisera alla objekt och uppdatera hello metaversum-objekt.</span><span class="sxs-lookup"><span data-stu-id="9043a-140">Both options sync all objects and update hello metaverse objects.</span></span> <span data-ttu-id="9043a-141">Den här åtgärden är en tidskrävande åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9043a-141">This action is a long running operation.</span></span>

### <a name="configure-run-profiles"></a><span data-ttu-id="9043a-142">Konfigurera körningsprofiler</span><span class="sxs-lookup"><span data-stu-id="9043a-142">Configure Run Profiles</span></span>
<span data-ttu-id="9043a-143">Det här alternativet kan du toosee hello körning av profiler som konfigurerats för en koppling.</span><span class="sxs-lookup"><span data-stu-id="9043a-143">This option allows you toosee hello run profiles configured for a Connector.</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a><span data-ttu-id="9043a-145">Söka Anslutarplats</span><span class="sxs-lookup"><span data-stu-id="9043a-145">Search Connector Space</span></span>
<span data-ttu-id="9043a-146">hello Sökåtgärd connector utrymme är användbara toofind objekt och felsökning av dataproblem med.</span><span class="sxs-lookup"><span data-stu-id="9043a-146">hello search connector space action is useful toofind objects and troubleshoot data issues.</span></span>

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

<span data-ttu-id="9043a-148">Starta genom att välja en **omfång**.</span><span class="sxs-lookup"><span data-stu-id="9043a-148">Start by selecting a **scope**.</span></span> <span data-ttu-id="9043a-149">Du kan söka baserat på data (RDN DN-fästpunkt, underträd) eller tillstånd hello-objekt (alla andra alternativ).</span><span class="sxs-lookup"><span data-stu-id="9043a-149">You can search based on data (RDN, DN, Anchor, Sub-Tree), or state of hello object (all other options).</span></span>  
<span data-ttu-id="9043a-150">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span><span class="sxs-lookup"><span data-stu-id="9043a-150">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span></span>  
<span data-ttu-id="9043a-151">Om du till exempel göra en underträd sökning får du alla objekt i en Organisationsenhet.</span><span class="sxs-lookup"><span data-stu-id="9043a-151">If you for example do a Sub-Tree search, you get all objects in one OU.</span></span>  
<span data-ttu-id="9043a-152">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span><span class="sxs-lookup"><span data-stu-id="9043a-152">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span></span>  
<span data-ttu-id="9043a-153">Från den här rutnät som du kan välja ett objekt, Välj **egenskaper**, och [följa den](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) från hello anslutningsplatsen källa, via hello metaversum och toohello mål anslutningsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9043a-153">From this grid you can select an object, select **properties**, and [follow it](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) from hello source connector space, through hello metaverse, and toohello target connector space.</span></span>

### <a name="changing-hello-ad-ds-account-password"></a><span data-ttu-id="9043a-154">Ändra lösenordet för hello AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="9043a-154">Changing hello AD DS account password</span></span>
<span data-ttu-id="9043a-155">Om du ändrar lösenordet för hello hello synkroniseringstjänsten kommer inte längre att kunna tooimport och exportera ändringar tooon lokala AD.</span><span class="sxs-lookup"><span data-stu-id="9043a-155">If you change hello account password, hello Synchronization Service will no longer be able tooimport/export changes tooon-premises AD.</span></span>   <span data-ttu-id="9043a-156">Du kan se hello följande:</span><span class="sxs-lookup"><span data-stu-id="9043a-156">You may see hello following:</span></span>

- <span data-ttu-id="9043a-157">hello importera och exportera steg för hello AD-anslutningen misslyckas med felet ”inga-start-autentiseringsuppgifter”.</span><span class="sxs-lookup"><span data-stu-id="9043a-157">hello import/export step for hello AD connector fails with "no-start-credentials" error.</span></span>
- <span data-ttu-id="9043a-158">Under Windows Loggboken hello programmets händelselogg innehåller ett fel med händelse-ID 6000 och meddelandet ”hello management agent” contoso.com ”kunde inte toorun eftersom hello autentiseringsuppgifter var ogiltiga”.</span><span class="sxs-lookup"><span data-stu-id="9043a-158">Under Windows Event Viewer, hello application event log contains an error with Event ID 6000 and message “hello management agent “contoso.com” failed toorun because hello credentials were invalid.”</span></span>

<span data-ttu-id="9043a-159">tooresolve hello utfärda update hello AD DS-användarkonto hello följande:</span><span class="sxs-lookup"><span data-stu-id="9043a-159">tooresolve hello issue, update hello AD DS user account using hello following:</span></span>


1. <span data-ttu-id="9043a-160">Starta hello Synchronization Service Manager (START → synkroniseringstjänsten).</span><span class="sxs-lookup"><span data-stu-id="9043a-160">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="9043a-161">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="9043a-161">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>
2. <span data-ttu-id="9043a-162">Gå toohello **kopplingar** fliken.</span><span class="sxs-lookup"><span data-stu-id="9043a-162">Go toohello **Connectors** tab.</span></span>
3. <span data-ttu-id="9043a-163">Välj hello AD-koppling som är konfigurerade toouse hello AD DS-konto.</span><span class="sxs-lookup"><span data-stu-id="9043a-163">Select hello AD Connector which is configured toouse hello AD DS account.</span></span>
4. <span data-ttu-id="9043a-164">Välj under åtgärder, **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="9043a-164">Under Actions, select **Properties**.</span></span>
5. <span data-ttu-id="9043a-165">I hello popup-fönstret, väljer du Anslut tooActive Directory-skogen:</span><span class="sxs-lookup"><span data-stu-id="9043a-165">In hello pop-up dialog, select Connect tooActive Directory Forest:</span></span>
6. <span data-ttu-id="9043a-166">hello skogsnamnet anger hello motsvarande lokala AD.</span><span class="sxs-lookup"><span data-stu-id="9043a-166">hello Forest name indicates hello corresponding on-prem AD.</span></span>
7. <span data-ttu-id="9043a-167">hello användarnamn Anger hello AD DS-konto som används för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="9043a-167">hello User name indicates hello AD DS account used for synchronization.</span></span>
8. <span data-ttu-id="9043a-168">Ange hello nytt lösenord för hello AD DS-konto i hello lösenordsrutan ![Azure AD Connect Sync kryptering nyckeln Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="9043a-168">Enter hello new password of hello AD DS account in hello Password textbox ![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>
9. <span data-ttu-id="9043a-169">Klicka på OK toosave hello nya lösenord och starta om hello synkroniseringstjänsten tooremove hello gamla lösenord från cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="9043a-169">Click OK toosave hello new password and restart hello Synchronization Service tooremove hello old password from memory cache.</span></span>



## <a name="next-steps"></a><span data-ttu-id="9043a-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9043a-170">Next steps</span></span>
<span data-ttu-id="9043a-171">Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9043a-171">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="9043a-172">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="9043a-172">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
