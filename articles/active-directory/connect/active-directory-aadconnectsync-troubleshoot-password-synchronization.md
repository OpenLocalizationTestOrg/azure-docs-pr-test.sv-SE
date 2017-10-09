---
title: "aaaTroubleshoot Lösenordssynkronisering med Azure AD Connect-synkronisering | Microsoft Docs"
description: "Den här artikeln innehåller information om hur tootroubleshoot lösenord synkroniseringsproblem."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 390eafec792cb39251627c14cb754f8bb30035b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a><span data-ttu-id="ac4be-103">Felsöka Lösenordssynkronisering med Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="ac4be-103">Troubleshoot password synchronization with Azure AD Connect sync</span></span>
<span data-ttu-id="ac4be-104">Det här avsnittet innehåller instruktioner för hur tootroubleshoot problem med synkronisering av lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-104">This topic provides steps for how tootroubleshoot issues with password synchronization.</span></span> <span data-ttu-id="ac4be-105">Om lösenord inte synkroniseras som förväntat, kan det vara för en delmängd av användare eller för alla användare.</span><span class="sxs-lookup"><span data-stu-id="ac4be-105">If passwords are not synchronizing as expected, it can be either for a subset of users or for all users.</span></span> <span data-ttu-id="ac4be-106">För Azure Active Directory (AD Azure) ansluta distribution med version 1.1.524.0 eller senare, det finns nu en diagnostiska cmdlet som du kan använda tootroubleshoot lösenord synkroniseringsproblem:</span><span class="sxs-lookup"><span data-stu-id="ac4be-106">For Azure Active Directory (Azure AD) Connect deployment with version 1.1.524.0 or later, there is now a diagnostic cmdlet that you can use tootroubleshoot password synchronization issues:</span></span>

* <span data-ttu-id="ac4be-107">Om du har ett problem om inga lösenord synkroniseras, se toohello [utan lösenord synkroniseras: felsöka med cmdleten hello diagnostiska](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-107">If you have an issue where no passwords are synchronized, refer toohello [No passwords are synchronized: troubleshoot by using hello diagnostic cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

* <span data-ttu-id="ac4be-108">Om du har problem med enskilda objekt, läsa toohello [ett objekt synkroniserar inte lösenord: felsöka med cmdleten hello diagnostiska](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-108">If you have an issue with individual objects, refer toohello [One object is not synchronizing passwords: troubleshoot by using hello diagnostic cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

<span data-ttu-id="ac4be-109">För äldre versioner av Azure AD Connect-distribution:</span><span class="sxs-lookup"><span data-stu-id="ac4be-109">For older versions of Azure AD Connect deployment:</span></span>

* <span data-ttu-id="ac4be-110">Om du har ett problem om inga lösenord synkroniseras, se toohello [utan lösenord synkroniseras: manuell felsökningssteg](#no-passwords-are-synchronized-manual-troubleshooting-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-110">If you have an issue where no passwords are synchronized, refer toohello [No passwords are synchronized: manual troubleshooting steps](#no-passwords-are-synchronized-manual-troubleshooting-steps) section.</span></span>

* <span data-ttu-id="ac4be-111">Om du har problem med enskilda objekt, läsa toohello [ett objekt synkroniserar inte lösenord: manuell felsökningssteg](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-111">If you have an issue with individual objects, refer toohello [One object is not synchronizing passwords: manual troubleshooting steps](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) section.</span></span>

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-hello-diagnostic-cmdlet"></a><span data-ttu-id="ac4be-112">Inga lösenord ska synkroniseras: felsöka med cmdleten hello diagnostik</span><span class="sxs-lookup"><span data-stu-id="ac4be-112">No passwords are synchronized: troubleshoot by using hello diagnostic cmdlet</span></span>
<span data-ttu-id="ac4be-113">Du kan använda hello `Invoke-ADSyncDiagnostics` cmdlet toofigure reda på varför utan lösenord synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="ac4be-113">You can use hello `Invoke-ADSyncDiagnostics` cmdlet toofigure out why no passwords are synchronized.</span></span>

> [!NOTE]
> <span data-ttu-id="ac4be-114">Hej `Invoke-ADSyncDiagnostics` cmdlet är endast tillgänglig för Azure AD Connect-version 1.1.524.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ac4be-114">hello `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-hello-diagnostics-cmdlet"></a><span data-ttu-id="ac4be-115">Kör hello diagnostik cmdlet</span><span class="sxs-lookup"><span data-stu-id="ac4be-115">Run hello diagnostics cmdlet</span></span>
<span data-ttu-id="ac4be-116">tootroubleshoot problem där ingen lösenord ska synkroniseras:</span><span class="sxs-lookup"><span data-stu-id="ac4be-116">tootroubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="ac4be-117">Öppna en ny Windows PowerShell-session på Azure AD Connect-servern med hello **kör som administratör** alternativet.</span><span class="sxs-lookup"><span data-stu-id="ac4be-117">Open a new Windows PowerShell session on your Azure AD Connect server with hello **Run as Administrator** option.</span></span>

2. <span data-ttu-id="ac4be-118">Kör `Set-ExecutionPolicy RemoteSigned` eller `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="ac4be-118">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="ac4be-119">Kör `Import-Module ADSyncDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="ac4be-119">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="ac4be-120">Kör `Invoke-ADSyncDiagnostics -PasswordSync`.</span><span class="sxs-lookup"><span data-stu-id="ac4be-120">Run `Invoke-ADSyncDiagnostics -PasswordSync`.</span></span>

### <a name="understand-hello-results-of-hello-cmdlet"></a><span data-ttu-id="ac4be-121">Förstå hello resultatet för hello-cmdlet</span><span class="sxs-lookup"><span data-stu-id="ac4be-121">Understand hello results of hello cmdlet</span></span>
<span data-ttu-id="ac4be-122">diagnostiska hello-cmdlet utför hello efter kontroller.</span><span class="sxs-lookup"><span data-stu-id="ac4be-122">hello diagnostic cmdlet performs hello following checks:</span></span>

* <span data-ttu-id="ac4be-123">Verifierar att hello funktionen för synkronisering av lösenord har aktiverats för din Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="ac4be-123">Validates that hello password synchronization feature is enabled for your Azure AD tenant.</span></span>

* <span data-ttu-id="ac4be-124">Verifierar att hello Azure AD Connect servern inte är i mellanlagringsläge.</span><span class="sxs-lookup"><span data-stu-id="ac4be-124">Validates that hello Azure AD Connect server is not in staging mode.</span></span>

* <span data-ttu-id="ac4be-125">För varje befintlig lokal Active Directory-koppling (vilket motsvarar tooan befintliga Active Directory-skog):</span><span class="sxs-lookup"><span data-stu-id="ac4be-125">For each existing on-premises Active Directory connector (which corresponds tooan existing Active Directory forest):</span></span>

   * <span data-ttu-id="ac4be-126">Verifierar att hello funktionen för synkronisering av lösenord är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="ac4be-126">Validates that hello password synchronization feature is enabled.</span></span>
   
   * <span data-ttu-id="ac4be-127">Söker efter lösenord synkronisering pulsslag händelser i hello Windows händelse loggar.</span><span class="sxs-lookup"><span data-stu-id="ac4be-127">Searches for password synchronization heartbeat events in hello Windows Application Event logs.</span></span>

   * <span data-ttu-id="ac4be-128">För varje Active Directory-domän under hello lokala Active Directory-koppling:</span><span class="sxs-lookup"><span data-stu-id="ac4be-128">For each Active Directory domain under hello on-premises Active Directory connector:</span></span>

      * <span data-ttu-id="ac4be-129">Verifierar att hello domän kan nås från hello Azure AD Connect-servern.</span><span class="sxs-lookup"><span data-stu-id="ac4be-129">Validates that hello domain is reachable from hello Azure AD Connect server.</span></span>

      * <span data-ttu-id="ac4be-130">Verifierar att hello Active Directory Domain Services (AD DS)-konton som används av hello lokala Active Directory-koppling har hello rätt användarnamn, lösenord och behörigheter som krävs för synkronisering av lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-130">Validates that hello Active Directory Domain Services (AD DS) accounts used by hello on-premises Active Directory connector has hello correct username, password, and permissions required for password synchronization.</span></span>

<span data-ttu-id="ac4be-131">hello följande diagram visar hello resultatet för hello-cmdlet för en domän, lokala Active Directory-topologi:</span><span class="sxs-lookup"><span data-stu-id="ac4be-131">hello following diagram illustrates hello results of hello cmdlet for a single-domain, on-premises Active Directory topology:</span></span>

![Diagnostikdata för Lösenordssynkronisering](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

<span data-ttu-id="ac4be-133">hello resten av det här avsnittet beskriver specifika resultat som returneras av hello-cmdlet och motsvarande problem.</span><span class="sxs-lookup"><span data-stu-id="ac4be-133">hello rest of this section describes specific results that are returned by hello cmdlet and corresponding issues.</span></span>

#### <a name="password-synchronization-feature-isnt-enabled"></a><span data-ttu-id="ac4be-134">Funktionen för synkronisering av lösenord är inte aktiverat</span><span class="sxs-lookup"><span data-stu-id="ac4be-134">Password synchronization feature isn't enabled</span></span>
<span data-ttu-id="ac4be-135">Om du inte har aktiverat Lösenordssynkronisering med hjälp av hello Azure AD Connect-guiden, returnerades hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="ac4be-135">If you haven't enabled password synchronization by using hello Azure AD Connect wizard, hello following error is returned:</span></span>

![Lösenordssynkronisering är inte aktiverat](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a><span data-ttu-id="ac4be-137">Azure AD Connect-servern är i mellanlagringsläge</span><span class="sxs-lookup"><span data-stu-id="ac4be-137">Azure AD Connect server is in staging mode</span></span>
<span data-ttu-id="ac4be-138">Om hello Azure AD Connect-servern är i mellanlagringsläge, Lösenordssynkronisering är tillfälligt inaktiverad och hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="ac4be-138">If hello Azure AD Connect server is in staging mode, password synchronization is temporarily disabled, and hello following error is returned:</span></span>

![Azure AD Connect-servern är i mellanlagringsläge](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a><span data-ttu-id="ac4be-140">Inga lösenord synkronisering heartbeat-händelser</span><span class="sxs-lookup"><span data-stu-id="ac4be-140">No password synchronization heartbeat events</span></span>
<span data-ttu-id="ac4be-141">Varje lokala Active Directory-koppling har sin egen synkroniseringskanalen för lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-141">Each on-premises Active Directory connector has its own password synchronization channel.</span></span> <span data-ttu-id="ac4be-142">När hello lösenord synkronisering kanal upprättas och det inte finns några toobe för ändringar av lösenord synkroniseras, genereras heartbeat-händelser (händelse-ID 654) var 30: e minut under hello Windows händelselogg.</span><span class="sxs-lookup"><span data-stu-id="ac4be-142">When hello password synchronization channel is established and there aren't any password changes toobe synchronized, a heartbeat event (EventId 654) is generated once every 30 minutes under hello Windows Application Event Log.</span></span> <span data-ttu-id="ac4be-143">För varje Active Directory connector lokalt hello cmdleten söker igenom för motsvarande heartbeat-händelser i hello senaste tre timmar.</span><span class="sxs-lookup"><span data-stu-id="ac4be-143">For each on-premises Active Directory connector, hello cmdlet searches for corresponding heartbeat events in hello past three hours.</span></span> <span data-ttu-id="ac4be-144">Om inga heartbeat-händelsen hittades, felmeddelande hello följande:</span><span class="sxs-lookup"><span data-stu-id="ac4be-144">If no heartbeat event is found, hello following error is returned:</span></span>

![Inga lösenord synkronisering hjärta slå händelse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a><span data-ttu-id="ac4be-146">AD DS-kontot har inte rätt behörighet</span><span class="sxs-lookup"><span data-stu-id="ac4be-146">AD DS account does not have correct permissions</span></span>
<span data-ttu-id="ac4be-147">Om hello AD DS-konto som används av hello lokal Active Directory connector toosynchronize lösenordshashvärden inte har åtkomstbehörighet hello returneras hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="ac4be-147">If hello AD DS account that's used by hello on-premises Active Directory connector toosynchronize password hashes does not have hello appropriate permissions, hello following error is returned:</span></span>

![Felaktig autentiseringsuppgifter](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a><span data-ttu-id="ac4be-149">Felaktigt användarnamn för AD DS-konto eller lösenord</span><span class="sxs-lookup"><span data-stu-id="ac4be-149">Incorrect AD DS account username or password</span></span>
<span data-ttu-id="ac4be-150">Om hello AD DS-konto används av hello lokala Active Directory connector toosynchronize lösenordshashvärden har ett felaktigt användarnamn eller lösenord, hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="ac4be-150">If hello AD DS account used by hello on-premises Active Directory connector toosynchronize password hashes has an incorrect username or password, hello following error is returned:</span></span>

![Felaktig autentiseringsuppgifter](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-hello-diagnostic-cmdlet"></a><span data-ttu-id="ac4be-152">Ett objekt synkroniserar inte lösenord: felsöka med cmdleten hello diagnostik</span><span class="sxs-lookup"><span data-stu-id="ac4be-152">One object is not synchronizing passwords: troubleshoot by using hello diagnostic cmdlet</span></span>
<span data-ttu-id="ac4be-153">Du kan använda hello `Invoke-ADSyncDiagnostics` cmdlet toodetermine varför ett objekt inte synkroniserar lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-153">You can use hello `Invoke-ADSyncDiagnostics` cmdlet toodetermine why one object is not synchronizing passwords.</span></span>

> [!NOTE]
> <span data-ttu-id="ac4be-154">Hej `Invoke-ADSyncDiagnostics` cmdlet är endast tillgänglig för Azure AD Connect-version 1.1.524.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ac4be-154">hello `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-hello-diagnostics-cmdlet"></a><span data-ttu-id="ac4be-155">Kör hello diagnostik cmdlet</span><span class="sxs-lookup"><span data-stu-id="ac4be-155">Run hello diagnostics cmdlet</span></span>
<span data-ttu-id="ac4be-156">tootroubleshoot problem där ingen lösenord ska synkroniseras:</span><span class="sxs-lookup"><span data-stu-id="ac4be-156">tootroubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="ac4be-157">Öppna en ny Windows PowerShell-session på Azure AD Connect-servern med hello **kör som administratör** alternativet.</span><span class="sxs-lookup"><span data-stu-id="ac4be-157">Open a new Windows PowerShell session on your Azure AD Connect server with hello **Run as Administrator** option.</span></span>

2. <span data-ttu-id="ac4be-158">Kör `Set-ExecutionPolicy RemoteSigned` eller `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="ac4be-158">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="ac4be-159">Kör `Import-Module ADSyncDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="ac4be-159">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="ac4be-160">Kör följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="ac4be-160">Run hello following cmdlet:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   <span data-ttu-id="ac4be-161">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ac4be-161">For example:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-hello-results-of-hello-cmdlet"></a><span data-ttu-id="ac4be-162">Förstå hello resultatet för hello-cmdlet</span><span class="sxs-lookup"><span data-stu-id="ac4be-162">Understand hello results of hello cmdlet</span></span>
<span data-ttu-id="ac4be-163">diagnostiska hello-cmdlet utför hello efter kontroller.</span><span class="sxs-lookup"><span data-stu-id="ac4be-163">hello diagnostic cmdlet performs hello following checks:</span></span>

* <span data-ttu-id="ac4be-164">Undersöker hello tillståndet för hello Active Directory-objekt i hello Active Directory connector diskutrymme, metaversum och Azure AD-anslutningsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ac4be-164">Examines hello state of hello Active Directory object in hello Active Directory connector space, Metaverse, and Azure AD connector space.</span></span>

* <span data-ttu-id="ac4be-165">Verifierar att det finns Synkroniseringsregler med Lösenordssynkronisering aktiverad och tillämpas toohello Active Directory-objekt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-165">Validates that there are synchronization rules with password synchronization enabled and applied toohello Active Directory object.</span></span>

* <span data-ttu-id="ac4be-166">Försöker tooretrieve och visa hello resultaten av hello senaste försök toosynchronize hello lösenord för hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="ac4be-166">Attempts tooretrieve and display hello results of hello last attempt toosynchronize hello password for hello object.</span></span>

<span data-ttu-id="ac4be-167">hello följande diagram visar hello resultatet för hello-cmdlet när du felsöker Lösenordssynkronisering för ett enskilt objekt:</span><span class="sxs-lookup"><span data-stu-id="ac4be-167">hello following diagram illustrates hello results of hello cmdlet when troubleshooting password synchronization for a single object:</span></span>

![Diagnostikdata för Lösenordssynkronisering - objekt](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

<span data-ttu-id="ac4be-169">hello resten av det här avsnittet beskriver specifika resultat som returneras av hello-cmdlet och motsvarande problem.</span><span class="sxs-lookup"><span data-stu-id="ac4be-169">hello rest of this section describes specific results returned by hello cmdlet and corresponding issues.</span></span>

#### <a name="hello-active-directory-object-isnt-exported-tooazure-ad"></a><span data-ttu-id="ac4be-170">hello Active Directory-objekt är inte exporterade tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="ac4be-170">hello Active Directory object isn't exported tooAzure AD</span></span>
<span data-ttu-id="ac4be-171">Lösenordssynkronisering för den här lokala Active Directory-konto misslyckas eftersom det finns inget motsvarande objekt i hello Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="ac4be-171">Password synchronization for this on-premises Active Directory account fails because there is no corresponding object in hello Azure AD tenant.</span></span> <span data-ttu-id="ac4be-172">hello följande fel returnerades:</span><span class="sxs-lookup"><span data-stu-id="ac4be-172">hello following error is returned:</span></span>

![Azure AD-objekt som saknas](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a><span data-ttu-id="ac4be-174">Användaren har ett tillfälligt lösenord</span><span class="sxs-lookup"><span data-stu-id="ac4be-174">User has a temporary password</span></span>
<span data-ttu-id="ac4be-175">För närvarande stöder inte Azure AD Connect synkroniserar tillfälliga lösenord med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac4be-175">Currently, Azure AD Connect does not support synchronizing temporary passwords with Azure AD.</span></span> <span data-ttu-id="ac4be-176">Ett lösenord anses toobe tillfällig om hello **ändra lösenord vid nästa inloggning** alternativet är inställt på hello lokala Active Directory-användare.</span><span class="sxs-lookup"><span data-stu-id="ac4be-176">A password is considered toobe temporary if hello **Change password at next logon** option is set on hello on-premises Active Directory user.</span></span> <span data-ttu-id="ac4be-177">hello följande fel returnerades:</span><span class="sxs-lookup"><span data-stu-id="ac4be-177">hello following error is returned:</span></span>

![Tillfälligt lösenord exporteras inte](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-toosynchronize-password-arent-available"></a><span data-ttu-id="ac4be-179">Resultaten av senaste försök toosynchronize lösenord är inte tillgängliga</span><span class="sxs-lookup"><span data-stu-id="ac4be-179">Results of last attempt toosynchronize password aren't available</span></span>
<span data-ttu-id="ac4be-180">Som standard lagrar Azure AD Connect hello resultaten av lösenord synkroniseringsförsök under sju dagar.</span><span class="sxs-lookup"><span data-stu-id="ac4be-180">By default, Azure AD Connect stores hello results of password synchronization attempts for seven days.</span></span> <span data-ttu-id="ac4be-181">Om det finns inga resultat för hello valda Active Directory-objekt, returneras hello följande varning:</span><span class="sxs-lookup"><span data-stu-id="ac4be-181">If there are no results available for hello selected Active Directory object, hello following warning is returned:</span></span>

![Diagnostikdata för objekt - ingen historik för synkronisering av lösenord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a><span data-ttu-id="ac4be-183">Inga lösenord ska synkroniseras: manuell felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="ac4be-183">No passwords are synchronized: manual troubleshooting steps</span></span>
<span data-ttu-id="ac4be-184">Följ dessa steg toodetermine varför utan lösenord synkroniseras:</span><span class="sxs-lookup"><span data-stu-id="ac4be-184">Follow these steps toodetermine why no passwords are synchronized:</span></span>

1. <span data-ttu-id="ac4be-185">Är hello Connect-servern i [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode)?</span><span class="sxs-lookup"><span data-stu-id="ac4be-185">Is hello Connect server in [staging mode](active-directory-aadconnectsync-operations.md#staging-mode)?</span></span> <span data-ttu-id="ac4be-186">En server i mellanlagringsläge synkroniseras inte eventuella lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-186">A server in staging mode does not synchronize any passwords.</span></span>

2. <span data-ttu-id="ac4be-187">Kör skript hello hello [hämta hello status för synkronisering lösenordsinställningar](#get-the-status-of-password-sync-settings) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-187">Run hello script in hello [Get hello status of password sync settings](#get-the-status-of-password-sync-settings) section.</span></span> <span data-ttu-id="ac4be-188">Den ger dig en översikt över hello synkroniseringskonfiguration för lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-188">It gives you an overview of hello password sync configuration.</span></span>  

    ![PowerShell-skriptets utdata från inställningar för synkronisering av lösenord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. <span data-ttu-id="ac4be-190">Om hello-funktionen inte har aktiverats i Azure AD eller om hello synkroniseringsstatus för kanal inte har aktiverats, kör du installationsguiden för hello Connect.</span><span class="sxs-lookup"><span data-stu-id="ac4be-190">If hello feature is not enabled in Azure AD or if hello sync channel status is not enabled, run hello Connect installation wizard.</span></span> <span data-ttu-id="ac4be-191">Välj **anpassa synkroniseringsalternativ**, och avmarkera Lösenordssynkronisering. Den här ändringen inaktiverar tillfälligt hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="ac4be-191">Select **Customize synchronization options**, and unselect password sync. This change temporarily disables hello feature.</span></span> <span data-ttu-id="ac4be-192">Sedan kör hello guiden igen och aktivera synkronisering av lösenord på nytt. Kör skript hello igen tooverify som hello konfigurationen är korrekt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-192">Then run hello wizard again and re-enable password sync. Run hello script again tooverify that hello configuration is correct.</span></span>

4. <span data-ttu-id="ac4be-193">Titta i hello efter fel i händelseloggen.</span><span class="sxs-lookup"><span data-stu-id="ac4be-193">Look in hello event log for errors.</span></span> <span data-ttu-id="ac4be-194">Leta efter hello följande händelser, vilket visar att det finns ett problem:</span><span class="sxs-lookup"><span data-stu-id="ac4be-194">Look for hello following events, which would indicate a problem:</span></span>
    * <span data-ttu-id="ac4be-195">Meddelandekälla: ID för ”katalogsynkronisering”: 0, 611, 652, 655 om du ser dessa händelser du har anslutningsproblem med.</span><span class="sxs-lookup"><span data-stu-id="ac4be-195">Source: "Directory synchronization" ID: 0, 611, 652, 655 If you see these events, you have a connectivity problem.</span></span> <span data-ttu-id="ac4be-196">hello händelseloggmeddelande innehåller information om skog där du har problem.</span><span class="sxs-lookup"><span data-stu-id="ac4be-196">hello event log message contains forest information where you have a problem.</span></span> <span data-ttu-id="ac4be-197">Mer information finns i [anslutningsproblem](#connectivity problem).</span><span class="sxs-lookup"><span data-stu-id="ac4be-197">For more information, see [Connectivity problem](#connectivity problem).</span></span>

5. <span data-ttu-id="ac4be-198">Om du ser inget pulsslag från eller om inget annat arbetat köra [utlösa en fullständig synkronisering av alla lösenord](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="ac4be-198">If you see no heartbeat or if nothing else worked, run [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span> <span data-ttu-id="ac4be-199">Kör skriptet för hello bara en gång.</span><span class="sxs-lookup"><span data-stu-id="ac4be-199">Run hello script only once.</span></span>

6. <span data-ttu-id="ac4be-200">Se hello [felsökning av ett objekt som inte synkroniserar lösenord](#one-object-is-not-synchronizing-passwords) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-200">See hello [Troubleshoot one object that is not synchronizing passwords](#one-object-is-not-synchronizing-passwords) section.</span></span>

### <a name="connectivity-problems"></a><span data-ttu-id="ac4be-201">Problem med nätverksanslutningen</span><span class="sxs-lookup"><span data-stu-id="ac4be-201">Connectivity problems</span></span>

<span data-ttu-id="ac4be-202">Har du anslutningen till Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ac4be-202">Do you have connectivity with Azure AD?</span></span>

<span data-ttu-id="ac4be-203">Hello-konto har nödvändiga behörigheter tooread hello lösenord hash-värden i alla domäner?</span><span class="sxs-lookup"><span data-stu-id="ac4be-203">Does hello account have required permissions tooread hello password hashes in all domains?</span></span> <span data-ttu-id="ac4be-204">Om du har installerat Connect med standardinställningar bör hello redan vara korrekt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-204">If you installed Connect by using Express settings, hello permissions should already be correct.</span></span> 

<span data-ttu-id="ac4be-205">Om du har använt anpassad installation kan du ange hello behörigheter manuellt genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="ac4be-205">If you used custom installation, set hello permissions manually by doing hello following:</span></span>
    
1. <span data-ttu-id="ac4be-206">toofind hello konto som används av hello Active Directory-koppling start **Synchronization Service Manager**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-206">toofind hello account used by hello Active Directory connector, start **Synchronization Service Manager**.</span></span> 
 
2. <span data-ttu-id="ac4be-207">Gå för**kopplingar**, och sök sedan efter hello lokala Active Directory-skog som du felsöker.</span><span class="sxs-lookup"><span data-stu-id="ac4be-207">Go too**Connectors**, and then search for hello on-premises Active Directory forest you are troubleshooting.</span></span> 
 
3. <span data-ttu-id="ac4be-208">Välj hello-koppling och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-208">Select hello connector, and then click **Properties**.</span></span> 
 
4. <span data-ttu-id="ac4be-209">Gå för**ansluta tooActive Directory-skog**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-209">Go too**Connect tooActive Directory Forest**.</span></span>  
    
    ![Konto som används av Active Directory-koppling](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    <span data-ttu-id="ac4be-211">Observera hello användarnamn och hello domän där hello konto finns.</span><span class="sxs-lookup"><span data-stu-id="ac4be-211">Note hello username and hello domain where hello account is located.</span></span>
    
5. <span data-ttu-id="ac4be-212">Starta **Active Directory-användare och datorer**, och sedan kontrollera att hello-kontot som du hittade tidigare har hello Följ behörigheter hello roten i alla domäner i skogen:</span><span class="sxs-lookup"><span data-stu-id="ac4be-212">Start **Active Directory Users and Computers**, and then verify that hello account you found earlier has hello follow permissions set at hello root of all domains in your forest:</span></span>
    * <span data-ttu-id="ac4be-213">Replikera katalogändringar</span><span class="sxs-lookup"><span data-stu-id="ac4be-213">Replicate Directory Changes</span></span>
    * <span data-ttu-id="ac4be-214">Replikera katalogen ändras alla</span><span class="sxs-lookup"><span data-stu-id="ac4be-214">Replicate Directory Changes All</span></span>

6. <span data-ttu-id="ac4be-215">Är hello domänkontrollanter som kan nås av Azure AD Connect?</span><span class="sxs-lookup"><span data-stu-id="ac4be-215">Are hello domain controllers reachable by Azure AD Connect?</span></span> <span data-ttu-id="ac4be-216">Om hello Connect-servern inte kan ansluta tooall domänkontrollanter, konfigurera **bara använda prioriterade domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-216">If hello Connect server cannot connect tooall domain controllers, configure **Only use preferred domain controller**.</span></span>  
    
    ![Domänkontrollanten som används av Active Directory-koppling](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. <span data-ttu-id="ac4be-218">Gå tillbaka för**Synchronization Service Manager** och **konfigurera katalogpartitionen**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-218">Go back too**Synchronization Service Manager** and **Configure Directory Partition**.</span></span> 
 
8. <span data-ttu-id="ac4be-219">Välj din domän i **Välj katalogpartitioner**väljer hello **endast använder prioriterade domänkontrollanter** kryssrutan och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-219">Select your domain in **Select directory partitions**, select hello **Only use preferred domain controllers** check box, and then click **Configure**.</span></span> 

9. <span data-ttu-id="ac4be-220">Ange hello domänkontrollanter som Connect ska använda för synkronisering av lösenord i hello-listan. hello samma lista används för att importera och exportera samt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-220">In hello list, enter hello domain controllers that Connect should use for password sync. hello same list is used for import and export as well.</span></span> <span data-ttu-id="ac4be-221">Utföra de här stegen för alla domäner.</span><span class="sxs-lookup"><span data-stu-id="ac4be-221">Do these steps for all your domains.</span></span>

10. <span data-ttu-id="ac4be-222">Om hello skript visar att det finns inget pulsslag, köra hello skript i [utlösa en fullständig synkronisering av alla lösenord](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="ac4be-222">If hello script shows that there is no heartbeat, run hello script in [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span>

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a><span data-ttu-id="ac4be-223">Ett objekt synkroniserar inte lösenord: manuell felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="ac4be-223">One object is not synchronizing passwords: manual troubleshooting steps</span></span>
<span data-ttu-id="ac4be-224">Du kan enkelt felsöka problem för synkronisering av lösenord genom att granska hello statusen på ett objekt.</span><span class="sxs-lookup"><span data-stu-id="ac4be-224">You can easily troubleshoot password synchronization issues by reviewing hello status of an object.</span></span>

1. <span data-ttu-id="ac4be-225">I **Active Directory-användare och datorer**, söka efter hello användare och kontrollera sedan att hello **användaren måste byta lösenord vid nästa inloggning** kryssrutan är avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="ac4be-225">In **Active Directory Users and Computers**, search for hello user, and then verify that hello **User must change password at next logon** check box is cleared.</span></span>  

    ![Active Directory-produktiva lösenord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    <span data-ttu-id="ac4be-227">Om hello är markerad, be hello användaren toosign i och ändra hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="ac4be-227">If hello check box is selected, ask hello user toosign in and change hello password.</span></span> <span data-ttu-id="ac4be-228">Temporära lösenord är inte synkroniserade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac4be-228">Temporary passwords are not synchronized with Azure AD.</span></span>

2. <span data-ttu-id="ac4be-229">Om hello lösenord verkar vara korrekta i Active Directory, följer du hello användare i hello Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="ac4be-229">If hello password looks correct in Active Directory, follow hello user in hello sync engine.</span></span> <span data-ttu-id="ac4be-230">Du kan se om det finns ett felmeddelande på hello objekt av följande hello användare från lokala Active Directory tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="ac4be-230">By following hello user from on-premises Active Directory tooAzure AD, you can see whether there is a descriptive error on hello object.</span></span>

    <span data-ttu-id="ac4be-231">a.</span><span class="sxs-lookup"><span data-stu-id="ac4be-231">a.</span></span> <span data-ttu-id="ac4be-232">Starta hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="ac4be-232">Start hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span></span>

    <span data-ttu-id="ac4be-233">b.</span><span class="sxs-lookup"><span data-stu-id="ac4be-233">b.</span></span> <span data-ttu-id="ac4be-234">Klicka på **kopplingar**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-234">Click **Connectors**.</span></span>

    <span data-ttu-id="ac4be-235">c.</span><span class="sxs-lookup"><span data-stu-id="ac4be-235">c.</span></span> <span data-ttu-id="ac4be-236">Välj hello **Active Directory-kopplingen** där hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="ac4be-236">Select hello **Active Directory Connector** where hello user is located.</span></span>

    <span data-ttu-id="ac4be-237">d.</span><span class="sxs-lookup"><span data-stu-id="ac4be-237">d.</span></span> <span data-ttu-id="ac4be-238">Välj **söka Anslutarplats**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-238">Select **Search Connector Space**.</span></span>

    <span data-ttu-id="ac4be-239">e.</span><span class="sxs-lookup"><span data-stu-id="ac4be-239">e.</span></span> <span data-ttu-id="ac4be-240">I hello **omfång** väljer **DN eller fästpunkt**, och ange sedan hello fullständig DN hello-användare som du felsöker.</span><span class="sxs-lookup"><span data-stu-id="ac4be-240">In hello **Scope** box, select **DN or Anchor**, and then enter hello full DN of hello user you are troubleshooting.</span></span>

    ![Sök efter användare i anslutningsplatsen med DN](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    <span data-ttu-id="ac4be-242">f.</span><span class="sxs-lookup"><span data-stu-id="ac4be-242">f.</span></span> <span data-ttu-id="ac4be-243">Leta upp hello användare du söker efter och klicka sedan på **egenskaper** toosee alla hello attribut.</span><span class="sxs-lookup"><span data-stu-id="ac4be-243">Locate hello user you are looking for, and then click **Properties** toosee all hello attributes.</span></span> <span data-ttu-id="ac4be-244">Om hello användaren inte är i hello sökresultat, kontrollera din [filtreringsregler](active-directory-aadconnectsync-configure-filtering.md) och se till att du kör [tillämpa och verifiera ändringar](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) för hello användaren tooappear i Connect.</span><span class="sxs-lookup"><span data-stu-id="ac4be-244">If hello user is not in hello search result, verify your [filtering rules](active-directory-aadconnectsync-configure-filtering.md) and make sure that you run [Apply and verify changes](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) for hello user tooappear in Connect.</span></span>

    <span data-ttu-id="ac4be-245">g.</span><span class="sxs-lookup"><span data-stu-id="ac4be-245">g.</span></span> <span data-ttu-id="ac4be-246">toosee hello lösenord synkronisera information om hello-objekt för hello gångna veckan klickar du på **loggen**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-246">toosee hello password sync details of hello object for hello past week, click **Log**.</span></span>  

    ![Information om objekt logg](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    <span data-ttu-id="ac4be-248">Om hello grupprincipobjektet loggar är tom, har Azure AD Connect tooread hello lösenords-hash från Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ac4be-248">If hello object log is empty, Azure AD Connect has been unable tooread hello password hash from Active Directory.</span></span> <span data-ttu-id="ac4be-249">Fortsätta felsökningen med [anslutningsfel](#connectivity-errors).</span><span class="sxs-lookup"><span data-stu-id="ac4be-249">Continue your troubleshooting with [Connectivity Errors](#connectivity-errors).</span></span> <span data-ttu-id="ac4be-250">Om du ser något annat värde än **lyckade**, se toohello tabellen i [loggen för synkronisering av lösenord](#password-sync-log).</span><span class="sxs-lookup"><span data-stu-id="ac4be-250">If you see any other value than **success**, refer toohello table in [Password sync log](#password-sync-log).</span></span>

    <span data-ttu-id="ac4be-251">h.</span><span class="sxs-lookup"><span data-stu-id="ac4be-251">h.</span></span> <span data-ttu-id="ac4be-252">Välj hello **härkomst** fliken och se till att minst en synkroniseringsregel hello **PasswordSync** kolumnen är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-252">Select hello **lineage** tab, and make sure that at least one sync rule in hello **PasswordSync** column is **True**.</span></span> <span data-ttu-id="ac4be-253">I standardkonfigurationen hello hello hello synkroniseringsregel heter **i från AD - användare AccountEnabled**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-253">In hello default configuration, hello name of hello sync rule is **In from AD - User AccountEnabled**.</span></span>  

    ![Övriga information om en användare](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    <span data-ttu-id="ac4be-255">Jag.</span><span class="sxs-lookup"><span data-stu-id="ac4be-255">i.</span></span> <span data-ttu-id="ac4be-256">Klicka på **metaversum objektegenskaper** toodisplay en lista över användarattribut.</span><span class="sxs-lookup"><span data-stu-id="ac4be-256">Click **Metaverse Object Properties** toodisplay a list of user attributes.</span></span>  

    ![Metaversum-information](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    <span data-ttu-id="ac4be-258">Kontrollera att det finns inga **cloudFiltered** attributet.</span><span class="sxs-lookup"><span data-stu-id="ac4be-258">Verify that there is no **cloudFiltered** attribute present.</span></span> <span data-ttu-id="ac4be-259">Se till att hello domänattribut (domainFQDN och domainNetBios) har hello förväntade värden.</span><span class="sxs-lookup"><span data-stu-id="ac4be-259">Make sure that hello domain attributes (domainFQDN and domainNetBios) have hello expected values.</span></span>

    <span data-ttu-id="ac4be-260">j.</span><span class="sxs-lookup"><span data-stu-id="ac4be-260">j.</span></span> <span data-ttu-id="ac4be-261">Klicka på hello **kopplingar** fliken. Kontrollera att du ser kopplingar tooboth lokala Active Directory och Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac4be-261">Click hello **Connectors** tab. Make sure that you see connectors tooboth on-premises Active Directory and Azure AD.</span></span>

    ![Metaversum-information](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    <span data-ttu-id="ac4be-263">k.</span><span class="sxs-lookup"><span data-stu-id="ac4be-263">k.</span></span> <span data-ttu-id="ac4be-264">Välj hello raden som representerar Azure AD, klickar du på **egenskaper**, och klicka sedan på hello **härkomst** fliken hello utrymme kopplingsobjektet måste ha en utgående regel hello **PasswordSync** kolumnuppsättningen för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-264">Select hello row that represents Azure AD, click **Properties**, and then click hello **Lineage** tab. hello connector space object should have an outbound rule in hello **PasswordSync** column set too**True**.</span></span> <span data-ttu-id="ac4be-265">I standardkonfigurationen hello hello hello synkroniseringsregel heter **ut tooAAD - användare ansluta**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-265">In hello default configuration, hello name of hello sync rule is **Out tooAAD - User Join**.</span></span>  

    ![Dialogrutan för koppling objektegenskaper för Anslutarplats](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a><span data-ttu-id="ac4be-267">Logg för synkronisering av lösenord</span><span class="sxs-lookup"><span data-stu-id="ac4be-267">Password sync log</span></span>
<span data-ttu-id="ac4be-268">hello i statuskolumnen kan ha hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="ac4be-268">hello status column can have hello following values:</span></span>

| <span data-ttu-id="ac4be-269">Status</span><span class="sxs-lookup"><span data-stu-id="ac4be-269">Status</span></span> | <span data-ttu-id="ac4be-270">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ac4be-270">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ac4be-271">Lyckades</span><span class="sxs-lookup"><span data-stu-id="ac4be-271">Success</span></span> |<span data-ttu-id="ac4be-272">Lösenordet har synkroniserats.</span><span class="sxs-lookup"><span data-stu-id="ac4be-272">Password has been successfully synchronized.</span></span> |
| <span data-ttu-id="ac4be-273">FilteredByTarget</span><span class="sxs-lookup"><span data-stu-id="ac4be-273">FilteredByTarget</span></span> |<span data-ttu-id="ac4be-274">Ange lösenordet för**användaren måste byta lösenord vid nästa inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ac4be-274">Password is set too**User must change password at next logon**.</span></span> <span data-ttu-id="ac4be-275">Lösenordet har inte synkroniserats.</span><span class="sxs-lookup"><span data-stu-id="ac4be-275">Password has not been synchronized.</span></span> |
| <span data-ttu-id="ac4be-276">NoTargetConnection</span><span class="sxs-lookup"><span data-stu-id="ac4be-276">NoTargetConnection</span></span> |<span data-ttu-id="ac4be-277">Inga objekt i hello metaversum eller hello Azure AD-anslutningsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ac4be-277">No object in hello metaverse or in hello Azure AD connector space.</span></span> |
| <span data-ttu-id="ac4be-278">SourceConnectorNotPresent</span><span class="sxs-lookup"><span data-stu-id="ac4be-278">SourceConnectorNotPresent</span></span> |<span data-ttu-id="ac4be-279">Inga objekt hittades i hello lokala Active Directory-anslutningsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ac4be-279">No object found in hello on-premises Active Directory connector space.</span></span> |
| <span data-ttu-id="ac4be-280">TargetNotExportedToDirectory</span><span class="sxs-lookup"><span data-stu-id="ac4be-280">TargetNotExportedToDirectory</span></span> |<span data-ttu-id="ac4be-281">hello-objekt på hello Azure AD-anslutningsplatsen har inte ännu exporterats.</span><span class="sxs-lookup"><span data-stu-id="ac4be-281">hello object in hello Azure AD connector space has not yet been exported.</span></span> |
| <span data-ttu-id="ac4be-282">MigratedCheckDetailsForMoreInfo</span><span class="sxs-lookup"><span data-stu-id="ac4be-282">MigratedCheckDetailsForMoreInfo</span></span> |<span data-ttu-id="ac4be-283">Loggposten skapades innan version 1.0.9125.0 och visas i det tidigare tillståndet.</span><span class="sxs-lookup"><span data-stu-id="ac4be-283">Log entry was created before build 1.0.9125.0 and is shown in its legacy state.</span></span> |
| <span data-ttu-id="ac4be-284">Fel</span><span class="sxs-lookup"><span data-stu-id="ac4be-284">Error</span></span> |<span data-ttu-id="ac4be-285">Tjänsten returnerade ett okänt fel.</span><span class="sxs-lookup"><span data-stu-id="ac4be-285">Service returned an unknown error.</span></span> |
| <span data-ttu-id="ac4be-286">Okänd</span><span class="sxs-lookup"><span data-stu-id="ac4be-286">Unknown</span></span> |<span data-ttu-id="ac4be-287">Ett fel uppstod vid försök tooprocess en batch med lösenordshashvärden.</span><span class="sxs-lookup"><span data-stu-id="ac4be-287">An error occurred while trying tooprocess a batch of password hashes.</span></span>  |
| <span data-ttu-id="ac4be-288">MissingAttribute</span><span class="sxs-lookup"><span data-stu-id="ac4be-288">MissingAttribute</span></span> |<span data-ttu-id="ac4be-289">Specifika attribut (till exempel Kerberos-hash) krävs av Azure AD Domain Services är inte tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="ac4be-289">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services are not available.</span></span> |
| <span data-ttu-id="ac4be-290">RetryRequestedByTarget</span><span class="sxs-lookup"><span data-stu-id="ac4be-290">RetryRequestedByTarget</span></span> |<span data-ttu-id="ac4be-291">Specifika attribut (till exempel Kerberos-hash) krävs av Azure AD Domain Services var inte tillgängliga tidigare.</span><span class="sxs-lookup"><span data-stu-id="ac4be-291">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services were not available previously.</span></span> <span data-ttu-id="ac4be-292">Det görs ett försök tooresynchronize hello användarens lösenords-hash.</span><span class="sxs-lookup"><span data-stu-id="ac4be-292">An attempt tooresynchronize hello user's password hash is made.</span></span> |

## <a name="scripts-toohelp-troubleshooting"></a><span data-ttu-id="ac4be-293">Felsökning av skript toohelp</span><span class="sxs-lookup"><span data-stu-id="ac4be-293">Scripts toohelp troubleshooting</span></span>

### <a name="get-hello-status-of-password-sync-settings"></a><span data-ttu-id="ac4be-294">Hämta hello statusen för inställningar för synkronisering av lösenord</span><span class="sxs-lookup"><span data-stu-id="ac4be-294">Get hello status of password sync settings</span></span>
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update hello script toouse hello appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a><span data-ttu-id="ac4be-295">Starta en fullständig synkronisering av alla lösenord</span><span class="sxs-lookup"><span data-stu-id="ac4be-295">Trigger a full sync of all passwords</span></span>
> [!NOTE]
> <span data-ttu-id="ac4be-296">Kör skriptet en gång.</span><span class="sxs-lookup"><span data-stu-id="ac4be-296">Run this script only once.</span></span> <span data-ttu-id="ac4be-297">Om du behöver toorun den mer än en gång, är något annat hello problem.</span><span class="sxs-lookup"><span data-stu-id="ac4be-297">If you need toorun it more than once, something else is hello problem.</span></span> <span data-ttu-id="ac4be-298">tootroubleshoot hello problem, kontakta Microsoft support.</span><span class="sxs-lookup"><span data-stu-id="ac4be-298">tootroubleshoot hello problem, contact Microsoft support.</span></span>

<span data-ttu-id="ac4be-299">Du kan utlösa en fullständig synkronisering av alla lösenord med hjälp av hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="ac4be-299">You can trigger a full sync of all passwords by using hello following script:</span></span>

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a><span data-ttu-id="ac4be-300">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac4be-300">Next steps</span></span>
* [<span data-ttu-id="ac4be-301">Implementera Lösenordssynkronisering med Azure AD Connect-synkronisering</span><span class="sxs-lookup"><span data-stu-id="ac4be-301">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md)
* [<span data-ttu-id="ac4be-302">Azure AD Connect Sync: Anpassa synkroniseringsalternativ</span><span class="sxs-lookup"><span data-stu-id="ac4be-302">Azure AD Connect Sync: Customizing synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="ac4be-303">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ac4be-303">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
