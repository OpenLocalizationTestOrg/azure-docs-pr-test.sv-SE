---
title: "Azure AD Connect-synkronisering: ändra hello Azure AD Connect Sync-tjänstkontot | Microsoft Docs"
description: "Här avsnittet beskrivs hello krypteringsnyckeln och hur tooabandon det efter hello lösenord har ändrats."
services: active-directory
keywords: "Azure AD-synkroniseringstjänstkontot, lösenord"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 11948ac4662f722e4f684ef6c9b9ccdc6387e60f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-azure-ad-connect-sync-service-account-password"></a><span data-ttu-id="d2ab4-104">Ändra hello Azure AD Connect sync kontolösenord</span><span class="sxs-lookup"><span data-stu-id="d2ab4-104">Changing hello Azure AD Connect sync service account password</span></span>
<span data-ttu-id="d2ab4-105">Om du ändrar hello Azure AD Connect sync kontolösenord kommer hello-synkroniseringstjänsten inte att kunna starta korrekt förrän du har avbrutna hello krypteringsnyckeln och initieras hello Azure AD Connect sync kontolösenord.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-105">If you change hello  Azure AD Connect sync service account password, hello Synchronization Service will not be able start correctly until you have abandoned hello encryption key and reinitialized hello Azure AD Connect sync service account password.</span></span> 

<span data-ttu-id="d2ab4-106">Azure AD Connect, som en del av hello synkroniseringstjänster använder en kryptering viktiga toostore hello lösenord hello AD DS och Azure AD-tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-106">Azure AD Connect, as part of hello Synchronization Services uses an encryption key toostore hello passwords of hello AD DS and Azure AD service accounts.</span></span>  <span data-ttu-id="d2ab4-107">Dessa konton krypteras innan de lagras i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-107">These accounts are encrypted before they are stored in hello database.</span></span> 

<span data-ttu-id="d2ab4-108">hello krypteringsnyckeln är säkrad via [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2ab4-108">hello encryption key used is secured using [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span></span> <span data-ttu-id="d2ab4-109">DPAPI skyddar hello krypteringsnyckeln med hjälp av hello **lösenordet för tjänstkontot för hello Azure AD Connect sync**.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-109">DPAPI protects hello encryption key using hello **password of hello Azure AD Connect sync service account**.</span></span> 

<span data-ttu-id="d2ab4-110">Om du behöver toochange hello tjänstkontots lösenord kan du använda hello procedurer i [Abandoning hello Azure AD Connect Sync krypteringsnyckeln](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish detta.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-110">If you need toochange hello service account password you can use hello procedures in [Abandoning hello Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish this.</span></span>  <span data-ttu-id="d2ab4-111">De här procedurerna bör också användas om du behöver tooabandon hello krypteringsnyckeln av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-111">These procedures should also be used if you need tooabandon hello encryption key for any reason.</span></span>

##<a name="issues-that-arise-from-changing-hello-password"></a><span data-ttu-id="d2ab4-112">Problem som uppstår när du ändrar hello lösenord</span><span class="sxs-lookup"><span data-stu-id="d2ab4-112">Issues that arise from changing hello password</span></span>
<span data-ttu-id="d2ab4-113">Det finns två saker som behöver toobe när du ändrar hello kontolösenord.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-113">There are two things that need toobe done when you change hello service account password.</span></span>

<span data-ttu-id="d2ab4-114">Du måste först toochange hello lösenord under hello Windows Service Control Manager.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-114">First, you need toochange hello password under hello Windows Service Control Manager.</span></span>  <span data-ttu-id="d2ab4-115">Tills problemet är löst visas följande fel:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-115">Until this issue is resolved you will see following errors:</span></span>


- <span data-ttu-id="d2ab4-116">Om du försöker toostart hello synkroniseringstjänsten i Windows Service Control Manager felmeddelandet hello ”**Windows kunde inte starta hello Microsoft Azure AD Sync-tjänsten på den lokala datorn**”.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-116">If you try toostart hello Synchronization Service in Windows Service Control Manager, you receive hello error "**Windows could not start hello Microsoft Azure AD Sync service on Local Computer**".</span></span> <span data-ttu-id="d2ab4-117">**Fel 1069: hello-tjänsten kunde inte starta på grund av tooa misslyckades.** "</span><span class="sxs-lookup"><span data-stu-id="d2ab4-117">**Error 1069: hello service did not start due tooa logon failure.**"</span></span>
- <span data-ttu-id="d2ab4-118">Under Windows Loggboken hello systemets händelselogg innehåller ett fel med **händelse-ID 7038** och meddelandet ”**hello ADSync-tjänsten har toolog på som hello konfigurerat lösenord på grund av toohello följande fel: hello-användarnamnet eller lösenordet är felaktigt.** "</span><span class="sxs-lookup"><span data-stu-id="d2ab4-118">Under Windows Event Viewer, hello system event log contains an error with **Event ID 7038** and message “**hello ADSync service was unable toolog on as with hello currently configured password due toohello following error: hello user name or password is incorrect.**"</span></span>

<span data-ttu-id="d2ab4-119">Andra kan vissa villkor kan om hello lösenordet uppdateras hello synkroniseringstjänsten inte längre hämta hello krypteringsnyckeln via DPAPI.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-119">Second, under specific conditions, if hello password is updated, hello Synchronization Service can no longer retrieve hello encryption key via DPAPI.</span></span> <span data-ttu-id="d2ab4-120">Utan hello krypteringsnyckel hello synkroniseringstjänsten inte kan dekryptera hello lösenord krävs toosynchronize till och från lokala AD och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-120">Without hello encryption key, hello Synchronization Service cannot decrypt hello passwords required toosynchronize to/from on-premises AD and Azure AD.</span></span>
<span data-ttu-id="d2ab4-121">Du ser fel som:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-121">You will see errors such as:</span></span>

- <span data-ttu-id="d2ab4-122">Under Windows Service Control Manager om du försöker toostart hello-synkroniseringstjänsten och det går inte att hämta hello krypteringsnyckeln misslyckas med felet ”** Windows kunde inte starta hello Microsoft Azure AD Sync på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-122">Under Windows Service Control Manager, if you try toostart hello Synchronization Service and it cannot retrieve hello encryption key, it fails with error “**Windows could not start hello Microsoft Azure AD Sync on Local Computer.</span></span> <span data-ttu-id="d2ab4-123">Mer information finns i händelseloggen för hello System.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-123">For more information, review hello System Event log.</span></span> <span data-ttu-id="d2ab4-124">Om detta är en icke-Microsoft-tjänst, kontakta leverantören för hello-tjänsten och hittar tooservice-specifika felkoden **-21451857952 *** ”.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-124">If this is a non-Microsoft service, contact hello service vendor, and refer tooservice-specific error code **-21451857952****.”</span></span>
- <span data-ttu-id="d2ab4-125">Under Windows Loggboken hello programmets händelselogg innehåller ett fel med **händelse-ID 6028** och felmeddelande *”**hello krypteringsnyckeln på servern kan inte nås.* *”*</span><span class="sxs-lookup"><span data-stu-id="d2ab4-125">Under Windows Event Viewer, hello application event log contains an error with **Event ID 6028** and error message *“**hello server encryption key cannot be accessed.**”*</span></span>

<span data-ttu-id="d2ab4-126">tooensure att du inte får dessa fel följa hello procedurerna i [Abandoning hello Azure AD Connect Sync krypteringsnyckeln](#abandoning-the-azure-ad-connect-sync-encryption-key) när du ändrar hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-126">tooensure that you do not receive these errors, follow hello procedures in [Abandoning hello Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) when changing hello password.</span></span>
 
## <a name="abandoning-hello-azure-ad-connect-sync-encryption-key"></a><span data-ttu-id="d2ab4-127">Överges hello Azure AD Connect Sync-krypteringsnyckeln</span><span class="sxs-lookup"><span data-stu-id="d2ab4-127">Abandoning hello Azure AD Connect Sync encryption key</span></span>
>[!IMPORTANT]
><span data-ttu-id="d2ab4-128">hello följande procedurer gäller endast tooAzure AD Connect build 1.1.443.0 eller äldre.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-128">hello following procedures only apply tooAzure AD Connect build 1.1.443.0 or older.</span></span>

<span data-ttu-id="d2ab4-129">Använd följande procedurer tooabandon hello krypteringsnyckeln hello.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-129">Use hello following procedures tooabandon hello encryption key.</span></span>

### <a name="what-toodo-if-you-need-tooabandon-hello-encryption-key"></a><span data-ttu-id="d2ab4-130">Vilka toodo om du behöver tooabandon hello krypteringsnyckeln</span><span class="sxs-lookup"><span data-stu-id="d2ab4-130">What toodo if you need tooabandon hello encryption key</span></span>

<span data-ttu-id="d2ab4-131">Om du behöver tooabandon hello krypteringsnyckeln används hello följande procedurer tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-131">If you need tooabandon hello encryption key, use hello following procedures tooaccomplish this.</span></span>

1. [<span data-ttu-id="d2ab4-132">Avbryt hello befintliga krypteringsnyckel</span><span class="sxs-lookup"><span data-stu-id="d2ab4-132">Abandon hello existing encryption key</span></span>](#abandon-the-existing-encryption-key)

2. [<span data-ttu-id="d2ab4-133">Ange hello lösenord för hello AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="d2ab4-133">Provide hello password of hello AD DS account</span></span>](#provide-the-password-of-the-ad-ds-account)

3. [<span data-ttu-id="d2ab4-134">Initiera om hello lösenordet för hello Azure AD sync-kontot</span><span class="sxs-lookup"><span data-stu-id="d2ab4-134">Reinitialize hello password of hello Azure AD sync account</span></span>](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [<span data-ttu-id="d2ab4-135">Starta hello-synkroniseringstjänsten</span><span class="sxs-lookup"><span data-stu-id="d2ab4-135">Start hello Synchronization Service</span></span>](#start-the-synchronization-service)

#### <a name="abandon-hello-existing-encryption-key"></a><span data-ttu-id="d2ab4-136">Avbryt hello befintliga krypteringsnyckel</span><span class="sxs-lookup"><span data-stu-id="d2ab4-136">Abandon hello existing encryption key</span></span>
<span data-ttu-id="d2ab4-137">Avbryt hello befintliga krypteringsnyckel så att nya krypteringsnyckeln kan skapas:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-137">Abandon hello existing encryption key so that new encryption key can be created:</span></span>

1. <span data-ttu-id="d2ab4-138">Logga in tooyour Azure AD Connect-servern som administratör.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-138">Log in tooyour Azure AD Connect Server as administrator.</span></span>

2. <span data-ttu-id="d2ab4-139">Starta en ny PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-139">Start a new PowerShell session.</span></span>

3. <span data-ttu-id="d2ab4-140">Navigera toofolder:`$env:Program Files\Microsoft Azure AD Sync\bin\`</span><span class="sxs-lookup"><span data-stu-id="d2ab4-140">Navigate toofolder: `$env:Program Files\Microsoft Azure AD Sync\bin\`</span></span>

4. <span data-ttu-id="d2ab4-141">Kör hello-kommando:`./miiskmu.exe /a`</span><span class="sxs-lookup"><span data-stu-id="d2ab4-141">Run hello command: `./miiskmu.exe /a`</span></span>

![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-hello-password-of-hello-ad-ds-account"></a><span data-ttu-id="d2ab4-143">Ange hello lösenord för hello AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="d2ab4-143">Provide hello password of hello AD DS account</span></span>
<span data-ttu-id="d2ab4-144">Som hello befintliga lösenord som lagrats i hello databasen kan inte längre dekrypteras så behöver tooprovide hello synkroniseringstjänsten med hello lösenord för hello AD DS-konto.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-144">As hello existing passwords stored inside hello database can no longer be decrypted, you need tooprovide hello Synchronization Service with hello password of hello AD DS account.</span></span> <span data-ttu-id="d2ab4-145">hello synkroniseringstjänsten krypterar hello lösenord med hello nya krypteringsnyckeln:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-145">hello Synchronization Service encrypts hello passwords using hello new encryption key:</span></span>

1. <span data-ttu-id="d2ab4-146">Starta hello Synchronization Service Manager (START → synkroniseringstjänsten).</span><span class="sxs-lookup"><span data-stu-id="d2ab4-146">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="d2ab4-147">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="d2ab4-147">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  
2. <span data-ttu-id="d2ab4-148">Gå toohello **kopplingar** fliken.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-148">Go toohello **Connectors** tab.</span></span>
3. <span data-ttu-id="d2ab4-149">Välj hello **AD-koppling** som motsvarar tooyour lokala AD.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-149">Select hello **AD Connector** that corresponds tooyour on-premises AD.</span></span> <span data-ttu-id="d2ab4-150">Upprepa hello följande steg för dem om du har flera AD-koppling.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-150">If you have more than one AD connector, repeat hello following steps for each of them.</span></span>
4. <span data-ttu-id="d2ab4-151">Under **åtgärder**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-151">Under **Actions**, select **Properties**.</span></span>
5. <span data-ttu-id="d2ab4-152">I popup-dialogrutan hello väljer **ansluta tooActive Directory-skog**:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-152">In hello pop-up dialog, select **Connect tooActive Directory Forest**:</span></span>
6. <span data-ttu-id="d2ab4-153">Ange hello lösenord för hello AD DS-konto i hello **lösenord** textruta.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-153">Enter hello password of hello AD DS account in hello **Password** textbox.</span></span> <span data-ttu-id="d2ab4-154">Om du inte vet lösenordet måste du ange den tooa känt värde innan du utför det här steget.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-154">If you do not know its password, you must set it tooa known value before performing this step.</span></span>
7. <span data-ttu-id="d2ab4-155">Klicka på **OK** toosave hello nytt lösenord och Stäng hello popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-155">Click **OK** toosave hello new password and close hello pop-up dialog.</span></span>
<span data-ttu-id="d2ab4-156">![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="d2ab4-156">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>

#### <a name="reinitialize-hello-password-of-hello-azure-ad-sync-account"></a><span data-ttu-id="d2ab4-157">Initiera om hello lösenordet för hello Azure AD sync-kontot</span><span class="sxs-lookup"><span data-stu-id="d2ab4-157">Reinitialize hello password of hello Azure AD sync account</span></span>
<span data-ttu-id="d2ab4-158">Du kan inte direkt ange hello lösenordet för hello Azure AD-tjänsten konto toohello synkroniseringstjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-158">You cannot directly provide hello password of hello Azure AD service account toohello Synchronization Service.</span></span> <span data-ttu-id="d2ab4-159">I så fall måste toouse hello cmdlet **Lägg till ADSyncAADServiceAccount** tooreinitialize hello Azure AD-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-159">Instead, you need toouse hello cmdlet **Add-ADSyncAADServiceAccount** tooreinitialize hello Azure AD service account.</span></span> <span data-ttu-id="d2ab4-160">hello cmdlet återställer lösenordet för hello och gör den tillgänglig toohello synkroniseringstjänsten:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-160">hello cmdlet resets hello account password and makes it available toohello Synchronization Service:</span></span>

1. <span data-ttu-id="d2ab4-161">Starta en ny PowerShell-session på hello Azure AD Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-161">Start a new PowerShell session on hello Azure AD Connect server.</span></span>
2. <span data-ttu-id="d2ab4-162">Kör cmdlet `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-162">Run cmdlet `Add-ADSyncAADServiceAccount`.</span></span>
3. <span data-ttu-id="d2ab4-163">I hello popup-fönstret, anger du autentiseringsuppgifter för hello Azure AD Global administratör för Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-163">In hello pop-up dialog, provide hello Azure AD Global admin credentials for your Azure AD tenant.</span></span>
<span data-ttu-id="d2ab4-164">![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key7.png)</span><span class="sxs-lookup"><span data-stu-id="d2ab4-164">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key7.png)</span></span>
4. <span data-ttu-id="d2ab4-165">Om det lyckas visas hello PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-165">If it is successful, you will see hello PowerShell command prompt.</span></span>

#### <a name="start-hello-synchronization-service"></a><span data-ttu-id="d2ab4-166">Starta hello-synkroniseringstjänsten</span><span class="sxs-lookup"><span data-stu-id="d2ab4-166">Start hello Synchronization Service</span></span>
<span data-ttu-id="d2ab4-167">Hello synkroniseringstjänsten har åtkomst toohello krypteringsnyckel och alla hello lösenord som behövs, kan du starta om tjänsten hello i hello Windows Service Control Manager:</span><span class="sxs-lookup"><span data-stu-id="d2ab4-167">Now that hello Synchronization Service has access toohello encryption key and all hello passwords it needs, you can restart hello service in hello Windows Service Control Manager:</span></span>


1. <span data-ttu-id="d2ab4-168">Gå tooWindows Service Control Manager (START → Services).</span><span class="sxs-lookup"><span data-stu-id="d2ab4-168">Go tooWindows Service Control Manager (START → Services).</span></span>
2. <span data-ttu-id="d2ab4-169">Välj **Microsoft Azure AD Sync** och klickar på Starta om.</span><span class="sxs-lookup"><span data-stu-id="d2ab4-169">Select **Microsoft Azure AD Sync** and click Restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2ab4-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2ab4-170">Next steps</span></span>
<span data-ttu-id="d2ab4-171">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="d2ab4-171">**Overview topics**</span></span>

* [<span data-ttu-id="d2ab4-172">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="d2ab4-172">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="d2ab4-173">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d2ab4-173">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
