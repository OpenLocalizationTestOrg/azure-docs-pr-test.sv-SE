---
title: "Azure AD Connect-synkronisering: ändra tjänstkontot för Azure AD Connect Sync | Microsoft Docs"
description: "Här avsnittet beskrivs krypteringsnyckeln och hur du avbryta den när lösenordet ändras."
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
ms.openlocfilehash: bf6234d0810f870909957ee1c1e33c225a4922b9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a><span data-ttu-id="e0fee-104">Ändra lösenord för tjänstkonto i Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="e0fee-104">Changing the Azure AD Connect sync service account password</span></span>
<span data-ttu-id="e0fee-105">Om du ändrar lösenord för tjänstkonto i Azure AD Connect sync kommer synkroniseringstjänsten inte att kunna starta korrekt förrän du har avbrutna krypteringsnyckeln och initieras på nytt lösenord för tjänstkonto i Azure AD Connect-synkronisering.</span><span class="sxs-lookup"><span data-stu-id="e0fee-105">If you change the  Azure AD Connect sync service account password, the Synchronization Service will not be able start correctly until you have abandoned the encryption key and reinitialized the Azure AD Connect sync service account password.</span></span> 

<span data-ttu-id="e0fee-106">Azure AD Connect, som en del av Synkroniseringstjänsterna använder en krypteringsnyckel för att lagra lösenord i AD DS och AD Azure-tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="e0fee-106">Azure AD Connect, as part of the Synchronization Services uses an encryption key to store the passwords of the AD DS and Azure AD service accounts.</span></span>  <span data-ttu-id="e0fee-107">Dessa konton krypteras innan de lagras i databasen.</span><span class="sxs-lookup"><span data-stu-id="e0fee-107">These accounts are encrypted before they are stored in the database.</span></span> 

<span data-ttu-id="e0fee-108">Krypteringsnyckeln är säkrad via [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0fee-108">The encryption key used is secured using [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span></span> <span data-ttu-id="e0fee-109">DPAPI skyddar en kryptering nyckel med hjälp av den **lösenordet för tjänstkontot för Azure AD Connect sync**.</span><span class="sxs-lookup"><span data-stu-id="e0fee-109">DPAPI protects the encryption key using the **password of the Azure AD Connect sync service account**.</span></span> 

<span data-ttu-id="e0fee-110">Om du behöver ändra tjänstkontots lösenord du kan använda procedurerna i [överges krypteringsnyckeln Azure AD Connect Sync](#abandoning-the-azure-ad-connect-sync-encryption-key) att åstadkomma detta.</span><span class="sxs-lookup"><span data-stu-id="e0fee-110">If you need to change the service account password you can use the procedures in [Abandoning the Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) to accomplish this.</span></span>  <span data-ttu-id="e0fee-111">De här procedurerna bör också användas om du behöver avbryta krypteringsnyckeln av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="e0fee-111">These procedures should also be used if you need to abandon the encryption key for any reason.</span></span>

##<a name="issues-that-arise-from-changing-the-password"></a><span data-ttu-id="e0fee-112">Problem som uppstår när du ändrar lösenordet</span><span class="sxs-lookup"><span data-stu-id="e0fee-112">Issues that arise from changing the password</span></span>
<span data-ttu-id="e0fee-113">Det finns två saker som behöver göras när du ändrar tjänstkontots lösenord.</span><span class="sxs-lookup"><span data-stu-id="e0fee-113">There are two things that need to be done when you change the service account password.</span></span>

<span data-ttu-id="e0fee-114">Du måste först ändra lösenordet under Windows Service Control Manager.</span><span class="sxs-lookup"><span data-stu-id="e0fee-114">First, you need to change the password under the Windows Service Control Manager.</span></span>  <span data-ttu-id="e0fee-115">Tills problemet är löst visas följande fel:</span><span class="sxs-lookup"><span data-stu-id="e0fee-115">Until this issue is resolved you will see following errors:</span></span>


- <span data-ttu-id="e0fee-116">Om du försöker starta synkroniseringstjänsten i Windows Service Control Manager felmeddelandet ”**Windows kunde inte starta tjänsten Microsoft Azure AD Sync på lokal dator**”.</span><span class="sxs-lookup"><span data-stu-id="e0fee-116">If you try to start the Synchronization Service in Windows Service Control Manager, you receive the error "**Windows could not start the Microsoft Azure AD Sync service on Local Computer**".</span></span> <span data-ttu-id="e0fee-117">**Fel 1069: Tjänsten kunde inte starta på grund av ett inloggningsfel.** "</span><span class="sxs-lookup"><span data-stu-id="e0fee-117">**Error 1069: The service did not start due to a logon failure.**"</span></span>
- <span data-ttu-id="e0fee-118">Under Windows Loggboken systemets händelselogg innehåller ett fel med **händelse-ID 7038** och meddelandet ”**i ADSync-tjänsten kunde inte logga in som med aktuellt konfigurerat lösenord på grund av följande fel: användarnamnet eller lösenordet är felaktigt.**”</span><span class="sxs-lookup"><span data-stu-id="e0fee-118">Under Windows Event Viewer, the system event log contains an error with **Event ID 7038** and message “**The ADSync service was unable to log on as with the currently configured password due to the following error: The user name or password is incorrect.**"</span></span>

<span data-ttu-id="e0fee-119">Andra kan vissa villkor kan om lösenordet uppdateras synkroniseringstjänsten inte längre hämta krypteringsnyckeln via DPAPI.</span><span class="sxs-lookup"><span data-stu-id="e0fee-119">Second, under specific conditions, if the password is updated, the Synchronization Service can no longer retrieve the encryption key via DPAPI.</span></span> <span data-ttu-id="e0fee-120">Synkroniseringstjänsten kan inte dekryptera lösenord krävs för att synkronisera till och från lokala AD och Azure AD utan krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="e0fee-120">Without the encryption key, the Synchronization Service cannot decrypt the passwords required to synchronize to/from on-premises AD and Azure AD.</span></span>
<span data-ttu-id="e0fee-121">Du ser fel som:</span><span class="sxs-lookup"><span data-stu-id="e0fee-121">You will see errors such as:</span></span>

- <span data-ttu-id="e0fee-122">Under Windows Service Control Manager om du försöker starta synkroniseringstjänsten och det går inte att hämta krypteringsnyckeln misslyckas med felet ”** Windows kunde inte starta Microsoft Azure AD Sync på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e0fee-122">Under Windows Service Control Manager, if you try to start the Synchronization Service and it cannot retrieve the encryption key, it fails with error “**Windows could not start the Microsoft Azure AD Sync on Local Computer.</span></span> <span data-ttu-id="e0fee-123">Mer information finns i händelseloggen System.</span><span class="sxs-lookup"><span data-stu-id="e0fee-123">For more information, review the System Event log.</span></span> <span data-ttu-id="e0fee-124">Om detta är en icke-Microsoft-tjänst Kontakta leverantören för tjänsten och tjänstspecifika felkoden **-21451857952 *** ”.</span><span class="sxs-lookup"><span data-stu-id="e0fee-124">If this is a non-Microsoft service, contact the service vendor, and refer to service-specific error code **-21451857952****.”</span></span>
- <span data-ttu-id="e0fee-125">Under Windows Loggboken i programmets händelselogg innehåller ett fel med **händelse-ID 6028** och felmeddelande *”**krypteringsnyckeln server kan inte nås.* *”*</span><span class="sxs-lookup"><span data-stu-id="e0fee-125">Under Windows Event Viewer, the application event log contains an error with **Event ID 6028** and error message *“**The server encryption key cannot be accessed.**”*</span></span>

<span data-ttu-id="e0fee-126">För att säkerställa att du inte får dessa fel, följer du procedurerna i [överges krypteringsnyckeln Azure AD Connect Sync](#abandoning-the-azure-ad-connect-sync-encryption-key) när du ändrar lösenordet.</span><span class="sxs-lookup"><span data-stu-id="e0fee-126">To ensure that you do not receive these errors, follow the procedures in [Abandoning the Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) when changing the password.</span></span>
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a><span data-ttu-id="e0fee-127">Överges krypteringsnyckeln Azure AD Connect Sync</span><span class="sxs-lookup"><span data-stu-id="e0fee-127">Abandoning the Azure AD Connect Sync encryption key</span></span>
>[!IMPORTANT]
><span data-ttu-id="e0fee-128">Följande procedurer gäller endast för Azure AD Connect build 1.1.443.0 eller äldre.</span><span class="sxs-lookup"><span data-stu-id="e0fee-128">The following procedures only apply to Azure AD Connect build 1.1.443.0 or older.</span></span>

<span data-ttu-id="e0fee-129">Använd följande procedurer för att undvika krypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="e0fee-129">Use the following procedures to abandon the encryption key.</span></span>

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a><span data-ttu-id="e0fee-130">Vad du gör om du behöver avbryta krypteringsnyckeln</span><span class="sxs-lookup"><span data-stu-id="e0fee-130">What to do if you need to abandon the encryption key</span></span>

<span data-ttu-id="e0fee-131">Om du behöver avbryta krypteringsnyckeln kan du använda följande procedurer för att åstadkomma detta.</span><span class="sxs-lookup"><span data-stu-id="e0fee-131">If you need to abandon the encryption key, use the following procedures to accomplish this.</span></span>

1. [<span data-ttu-id="e0fee-132">Avbryt befintliga krypteringsnyckeln</span><span class="sxs-lookup"><span data-stu-id="e0fee-132">Abandon the existing encryption key</span></span>](#abandon-the-existing-encryption-key)

2. [<span data-ttu-id="e0fee-133">Ange lösenordet för AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="e0fee-133">Provide the password of the AD DS account</span></span>](#provide-the-password-of-the-ad-ds-account)

3. [<span data-ttu-id="e0fee-134">Initiera om lösenordet för Azure AD sync-kontot</span><span class="sxs-lookup"><span data-stu-id="e0fee-134">Reinitialize the password of the Azure AD sync account</span></span>](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [<span data-ttu-id="e0fee-135">Starta synkroniseringstjänsten</span><span class="sxs-lookup"><span data-stu-id="e0fee-135">Start the Synchronization Service</span></span>](#start-the-synchronization-service)

#### <a name="abandon-the-existing-encryption-key"></a><span data-ttu-id="e0fee-136">Avbryt befintliga krypteringsnyckeln</span><span class="sxs-lookup"><span data-stu-id="e0fee-136">Abandon the existing encryption key</span></span>
<span data-ttu-id="e0fee-137">Avbryt den befintliga krypteringsnyckeln så att nya krypteringsnyckeln kan skapas:</span><span class="sxs-lookup"><span data-stu-id="e0fee-137">Abandon the existing encryption key so that new encryption key can be created:</span></span>

1. <span data-ttu-id="e0fee-138">Logga in på ditt Azure AD Connect-servern som administratör.</span><span class="sxs-lookup"><span data-stu-id="e0fee-138">Log in to your Azure AD Connect Server as administrator.</span></span>

2. <span data-ttu-id="e0fee-139">Starta en ny PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="e0fee-139">Start a new PowerShell session.</span></span>

3. <span data-ttu-id="e0fee-140">Navigera till mappen:`$env:Program Files\Microsoft Azure AD Sync\bin\`</span><span class="sxs-lookup"><span data-stu-id="e0fee-140">Navigate to folder: `$env:Program Files\Microsoft Azure AD Sync\bin\`</span></span>

4. <span data-ttu-id="e0fee-141">Kör kommandot:`./miiskmu.exe /a`</span><span class="sxs-lookup"><span data-stu-id="e0fee-141">Run the command: `./miiskmu.exe /a`</span></span>

![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a><span data-ttu-id="e0fee-143">Ange lösenordet för AD DS-konto</span><span class="sxs-lookup"><span data-stu-id="e0fee-143">Provide the password of the AD DS account</span></span>
<span data-ttu-id="e0fee-144">Som de befintliga lösenord som lagrats i databasen kan inte längre dekrypteras så behöver du ge synkroniseringstjänsten lösenordet för AD DS-konto.</span><span class="sxs-lookup"><span data-stu-id="e0fee-144">As the existing passwords stored inside the database can no longer be decrypted, you need to provide the Synchronization Service with the password of the AD DS account.</span></span> <span data-ttu-id="e0fee-145">Synkroniseringstjänsten krypterar lösenord med hjälp av den nya krypteringsnyckeln:</span><span class="sxs-lookup"><span data-stu-id="e0fee-145">The Synchronization Service encrypts the passwords using the new encryption key:</span></span>

1. <span data-ttu-id="e0fee-146">Starta hanteraren för synkroniseringstjänsten (START → synkroniseringstjänsten).</span><span class="sxs-lookup"><span data-stu-id="e0fee-146">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="e0fee-147">![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="e0fee-147">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  
2. <span data-ttu-id="e0fee-148">Gå till den **kopplingar** fliken.</span><span class="sxs-lookup"><span data-stu-id="e0fee-148">Go to the **Connectors** tab.</span></span>
3. <span data-ttu-id="e0fee-149">Välj den **AD-koppling** som motsvarar dina lokala AD.</span><span class="sxs-lookup"><span data-stu-id="e0fee-149">Select the **AD Connector** that corresponds to your on-premises AD.</span></span> <span data-ttu-id="e0fee-150">Upprepa följande steg för dem om du har flera AD-koppling.</span><span class="sxs-lookup"><span data-stu-id="e0fee-150">If you have more than one AD connector, repeat the following steps for each of them.</span></span>
4. <span data-ttu-id="e0fee-151">Under **åtgärder**väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="e0fee-151">Under **Actions**, select **Properties**.</span></span>
5. <span data-ttu-id="e0fee-152">I popup-fönstret väljer **Anslut till Active Directory-skog**:</span><span class="sxs-lookup"><span data-stu-id="e0fee-152">In the pop-up dialog, select **Connect to Active Directory Forest**:</span></span>
6. <span data-ttu-id="e0fee-153">Ange lösenordet för AD DS-konto i den **lösenord** textruta.</span><span class="sxs-lookup"><span data-stu-id="e0fee-153">Enter the password of the AD DS account in the **Password** textbox.</span></span> <span data-ttu-id="e0fee-154">Om du inte vet lösenordet måste du ange den till ett känt värde innan du utför det här steget.</span><span class="sxs-lookup"><span data-stu-id="e0fee-154">If you do not know its password, you must set it to a known value before performing this step.</span></span>
7. <span data-ttu-id="e0fee-155">Klicka på **OK** att spara det nya lösenordet och stänga popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="e0fee-155">Click **OK** to save the new password and close the pop-up dialog.</span></span>
<span data-ttu-id="e0fee-156">![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="e0fee-156">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a><span data-ttu-id="e0fee-157">Initiera om lösenordet för Azure AD sync-kontot</span><span class="sxs-lookup"><span data-stu-id="e0fee-157">Reinitialize the password of the Azure AD sync account</span></span>
<span data-ttu-id="e0fee-158">Du kan inte direkt ange lösenordet för Azure AD-tjänstkontot till synkroniseringstjänsten.</span><span class="sxs-lookup"><span data-stu-id="e0fee-158">You cannot directly provide the password of the Azure AD service account to the Synchronization Service.</span></span> <span data-ttu-id="e0fee-159">I stället måste du använda cmdlet **Lägg till ADSyncAADServiceAccount** att initiera om Azure AD-tjänstkontot.</span><span class="sxs-lookup"><span data-stu-id="e0fee-159">Instead, you need to use the cmdlet **Add-ADSyncAADServiceAccount** to reinitialize the Azure AD service account.</span></span> <span data-ttu-id="e0fee-160">Cmdlet: en återställer kontolösenordet och gör den tillgänglig för synkroniseringstjänsten:</span><span class="sxs-lookup"><span data-stu-id="e0fee-160">The cmdlet resets the account password and makes it available to the Synchronization Service:</span></span>

1. <span data-ttu-id="e0fee-161">Starta en ny PowerShell-session på Azure AD Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="e0fee-161">Start a new PowerShell session on the Azure AD Connect server.</span></span>
2. <span data-ttu-id="e0fee-162">Kör cmdlet `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="e0fee-162">Run cmdlet `Add-ADSyncAADServiceAccount`.</span></span>
3. <span data-ttu-id="e0fee-163">Ange autentiseringsuppgifter för Azure AD-Global administratör för Azure AD-klienten i popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="e0fee-163">In the pop-up dialog, provide the Azure AD Global admin credentials for your Azure AD tenant.</span></span>
<span data-ttu-id="e0fee-164">![Azure AD Connect Sync kryptering viktiga verktyg](media/active-directory-aadconnectsync-encryption-key/key7.png)</span><span class="sxs-lookup"><span data-stu-id="e0fee-164">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key7.png)</span></span>
4. <span data-ttu-id="e0fee-165">Om det lyckas visas PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="e0fee-165">If it is successful, you will see the PowerShell command prompt.</span></span>

#### <a name="start-the-synchronization-service"></a><span data-ttu-id="e0fee-166">Starta synkroniseringstjänsten</span><span class="sxs-lookup"><span data-stu-id="e0fee-166">Start the Synchronization Service</span></span>
<span data-ttu-id="e0fee-167">Nu när synkroniseringstjänsten har åtkomst till krypteringsnyckeln och alla lösenord som behövs, du kan starta om tjänsten i Windows Service Control Manager:</span><span class="sxs-lookup"><span data-stu-id="e0fee-167">Now that the Synchronization Service has access to the encryption key and all the passwords it needs, you can restart the service in the Windows Service Control Manager:</span></span>


1. <span data-ttu-id="e0fee-168">Gå till Windows Service Control Manager (START → Services).</span><span class="sxs-lookup"><span data-stu-id="e0fee-168">Go to Windows Service Control Manager (START → Services).</span></span>
2. <span data-ttu-id="e0fee-169">Välj **Microsoft Azure AD Sync** och klickar på Starta om.</span><span class="sxs-lookup"><span data-stu-id="e0fee-169">Select **Microsoft Azure AD Sync** and click Restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0fee-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0fee-170">Next steps</span></span>
<span data-ttu-id="e0fee-171">**Översiktsavsnitt**</span><span class="sxs-lookup"><span data-stu-id="e0fee-171">**Overview topics**</span></span>

* [<span data-ttu-id="e0fee-172">Azure AD Connect-synkronisering: Förstå och anpassa synkronisering</span><span class="sxs-lookup"><span data-stu-id="e0fee-172">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="e0fee-173">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0fee-173">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
