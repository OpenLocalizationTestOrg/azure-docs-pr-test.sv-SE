---
title: "Azure Active Directory Connect: Vanliga frågor och svar - | Microsoft Docs"
description: "Den här sidan har vanliga frågor om Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 39c0b127d3dcf6f45607ad8b4647a9ad79dfc732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a><span data-ttu-id="14638-103">Vanliga frågor om Azure Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="14638-103">Frequently asked questions for Azure Active Directory Connect</span></span>

## <a name="general-installation"></a><span data-ttu-id="14638-104">Allmän installation</span><span class="sxs-lookup"><span data-stu-id="14638-104">General installation</span></span>
<span data-ttu-id="14638-105">**F: installationen fungerar om hello Azure AD Global administratör har aktiverat 2FA?**</span><span class="sxs-lookup"><span data-stu-id="14638-105">**Q: Will installation work if hello Azure AD Global Admin has 2FA enabled?**</span></span>  
<span data-ttu-id="14638-106">Med hello versioner från februari 2016 stöds detta.</span><span class="sxs-lookup"><span data-stu-id="14638-106">With hello builds from February 2016, this is supported.</span></span>

<span data-ttu-id="14638-107">**F: finns det ett sätt tooinstall Azure AD Connect obevakad?**</span><span class="sxs-lookup"><span data-stu-id="14638-107">**Q: Is there a way tooinstall Azure AD Connect unattended?**</span></span>  
<span data-ttu-id="14638-108">Det är bara stöds tooinstall Azure AD Connect med hello installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="14638-108">It is only supported tooinstall Azure AD Connect using hello installation wizard.</span></span> <span data-ttu-id="14638-109">En obevakad och tyst installation stöds inte.</span><span class="sxs-lookup"><span data-stu-id="14638-109">An unattended and silent installation is not supported.</span></span>

<span data-ttu-id="14638-110">**F: Jag har en skog där en domän inte kan kontaktas. Hur installerar Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="14638-110">**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**</span></span>  
<span data-ttu-id="14638-111">Med hello versioner från februari 2016 stöds detta.</span><span class="sxs-lookup"><span data-stu-id="14638-111">With hello builds from February 2016, this is supported.</span></span>

<span data-ttu-id="14638-112">**F: kan hello AD DS health agent arbete på server core?**</span><span class="sxs-lookup"><span data-stu-id="14638-112">**Q: Does hello AD DS health agent work on server core?**</span></span>  
<span data-ttu-id="14638-113">Ja.</span><span class="sxs-lookup"><span data-stu-id="14638-113">Yes.</span></span> <span data-ttu-id="14638-114">Du kan slutföra hello registreringen med hello följande PowerShell-cmdlet när du har installerat hello agent:</span><span class="sxs-lookup"><span data-stu-id="14638-114">After installing hello agent, you can complete hello registration process using hello following PowerShell cmdlet:</span></span> 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a><span data-ttu-id="14638-115">Nätverk</span><span class="sxs-lookup"><span data-stu-id="14638-115">Network</span></span>
<span data-ttu-id="14638-116">**F: Jag har en brandvägg, nätverksenhet eller något annat som begränsar hello maximal tid anslutningar kan vara öppen i nätverket. Hur lång tid ska min klient sida timeout tröskelvärdet vara när du använder Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="14638-116">**Q: I have a firewall, network device, or something else that limits hello maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**</span></span>  
<span data-ttu-id="14638-117">Alla nätverksprogramvara fysiska enheter eller något annat som gränser hello maximal tid anslutningar kan förbli öppen bör använda ett tröskelvärde på minst 5 minuter (300 sekunder) för anslutningen mellan hello servern där hello Azure AD Connect-klienten är installerad och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="14638-117">All networking software, physical devices, or anything else that limits hello maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between hello server where hello Azure AD Connect client is installed and Azure Active Directory.</span></span> <span data-ttu-id="14638-118">Detta gäller även tooall som tidigare lanserats synkroniseringsverktyg för Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="14638-118">This also applies tooall previously released Microsoft Identity synchronization tools.</span></span>

<span data-ttu-id="14638-119">**F: är SLDs (enkel etikett domäner) stöds?**</span><span class="sxs-lookup"><span data-stu-id="14638-119">**Q: Are SLDs (Single Label Domains) supported?**</span></span>  
<span data-ttu-id="14638-120">Nej, Azure AD Connect har inte stöd för lokala skogar och domäner med SLDs.</span><span class="sxs-lookup"><span data-stu-id="14638-120">No, Azure AD Connect does not support on-premises forests/domains using SLDs.</span></span>

<span data-ttu-id="14638-121">**F: är ”prickad” NetBios-namnet stöds?**</span><span class="sxs-lookup"><span data-stu-id="14638-121">**Q: Are "dotted" NetBios named supported?**</span></span>  
<span data-ttu-id="14638-122">Nej, Azure AD Connect har inte stöd för lokala skogar och domäner där hello NetBios-namn innehåller en punkt ””. i hello namn.</span><span class="sxs-lookup"><span data-stu-id="14638-122">No, Azure AD Connect does not support on-premises forests/domains where hello NetBios name contains a period "." in hello name.</span></span>

## <a name="federation"></a><span data-ttu-id="14638-123">Federation</span><span class="sxs-lookup"><span data-stu-id="14638-123">Federation</span></span>
<span data-ttu-id="14638-124">**F: Vad gör jag om jag får ett e-postmeddelande som ber mig toorenew mitt Office 365-certifikat**</span><span class="sxs-lookup"><span data-stu-id="14638-124">**Q: What do I do if I receive an email that asking me toorenew my Office 365 certificate**</span></span>  
<span data-ttu-id="14638-125">Använda hello vägledningen som beskrivs i hello [förnya certifikat](active-directory-aadconnect-o365-certs.md) avsnitt om hur toorenew hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="14638-125">Use hello guidance that is outlined in hello [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how toorenew hello certificate.</span></span>

<span data-ttu-id="14638-126">**F: Jag har ”Uppdatera automatiskt förlitande part” för O365 förlitande part. Måste jag tootake någon åtgärd när min certifikatet för tokensignering automatiskt rullar över?**</span><span class="sxs-lookup"><span data-stu-id="14638-126">**Q: I have "Automatically update relying party" set for O365 relying party. Do I have tootake any action when my token signing certificate automatically rolls over?**</span></span>  
<span data-ttu-id="14638-127">Använda hello vägledningen som beskrivs i artikel hello [förnya certifikat](active-directory-aadconnect-o365-certs.md).</span><span class="sxs-lookup"><span data-stu-id="14638-127">Use hello guidance that is outlined in hello article [renew certificates](active-directory-aadconnect-o365-certs.md).</span></span>

## <a name="environment"></a><span data-ttu-id="14638-128">Miljö</span><span class="sxs-lookup"><span data-stu-id="14638-128">Environment</span></span>
<span data-ttu-id="14638-129">**F: är it stöds toorename hello server när du har installerat Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="14638-129">**Q: Is it supported toorename hello server after Azure AD Connect has been installed?**</span></span>  
<span data-ttu-id="14638-130">Nej.</span><span class="sxs-lookup"><span data-stu-id="14638-130">No.</span></span> <span data-ttu-id="14638-131">Ändra servernamnet för hello blir orsak hello sync motorn toonot kan tooconnect toohello SQL-databas och hello-tjänsten inte kan toostart.</span><span class="sxs-lookup"><span data-stu-id="14638-131">Changing hello server name will cause hello sync engine toonot be able tooconnect toohello SQL database and hello service will not be able toostart.</span></span>

## <a name="identity-data"></a><span data-ttu-id="14638-132">Identitetsdata</span><span class="sxs-lookup"><span data-stu-id="14638-132">Identity data</span></span>
<span data-ttu-id="14638-133">**F: hello UPN (userPrincipalName) attribut i Azure AD matchar inte hello lokala UPN - varför?**</span><span class="sxs-lookup"><span data-stu-id="14638-133">**Q: hello UPN (userPrincipalName) attribute in Azure AD does not match hello on-prem UPN - why?**</span></span>  
<span data-ttu-id="14638-134">Finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="14638-134">See these articles:</span></span>

* [<span data-ttu-id="14638-135">Användarnamnen i Office 365, Azure eller Intune inte matchar hello lokal UPN eller alternativ inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="14638-135">User names in Office 365, Azure, or Intune don't match hello on-premises UPN or alternate login ID</span></span>](https://support.microsoft.com/en-us/kb/2523192)
* [<span data-ttu-id="14638-136">Ändringarna synkroniserats inte med hello Azure Active Directory Sync tool när du har ändrat hello UPN-namnet för en användare konto toouse en annan federerad domän</span><span class="sxs-lookup"><span data-stu-id="14638-136">Changes aren't synced by hello Azure Active Directory Sync tool after you change hello UPN of a user account toouse a different federated domain</span></span>](https://support.microsoft.com/en-us/kb/2669550)

<span data-ttu-id="14638-137">Du kan också konfigurera Azure AD tooallow hello sync motorn tooupdate hello userPrincipalName som beskrivs i [Azure AD Connect sync tjänstens funktioner](active-directory-aadconnectsyncservice-features.md).</span><span class="sxs-lookup"><span data-stu-id="14638-137">You can also configure Azure AD tooallow hello sync engine tooupdate hello userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).</span></span>

<span data-ttu-id="14638-138">**F: är it stöds toosoft matchar lokala AD-gruppen/kontakt-objekt med den befintliga Azure AD-grupp/kontakta objekt?**</span><span class="sxs-lookup"><span data-stu-id="14638-138">**Q: Is it supported toosoft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**</span></span>  
<span data-ttu-id="14638-139">Nej, detta stöds för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="14638-139">No, this is currently not supported.</span></span>

<span data-ttu-id="14638-140">**F: är it stöds toomanually ImmutableId attributet på befintliga Azure AD-gruppen/kontakta objekt toohard matchar den tooon lokala AD-gruppen/kontakt objekt?**</span><span class="sxs-lookup"><span data-stu-id="14638-140">**Q: Is it supported toomanually set ImmutableId attribute on existing Azure AD Group/Contact objects toohard match it tooon-premises AD Group/Contact objects?**</span></span>  
<span data-ttu-id="14638-141">Nej, detta stöds för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="14638-141">No, this is currently not supported.</span></span>



## <a name="custom-configuration"></a><span data-ttu-id="14638-142">Anpassad konfiguration</span><span class="sxs-lookup"><span data-stu-id="14638-142">Custom configuration</span></span>
<span data-ttu-id="14638-143">**F: var finns hello PowerShell-cmdlets för Azure AD Connect dokumenterade?**</span><span class="sxs-lookup"><span data-stu-id="14638-143">**Q: Where are hello PowerShell cmdlets for Azure AD Connect documented?**</span></span>  
<span data-ttu-id="14638-144">Andra PowerShell-cmdletar finns i Azure AD Connect stöds inte för kundens användning med hello undantag av hello-cmdletarna som beskrivs i den här platsen.</span><span class="sxs-lookup"><span data-stu-id="14638-144">With hello exception of hello cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.</span></span>

<span data-ttu-id="14638-145">**F: kan jag använda ”Server exportservern/importera” hittades i *Synchronization Service Manager* toomove konfiguration mellan servrar?**</span><span class="sxs-lookup"><span data-stu-id="14638-145">**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* toomove configuration between servers?**</span></span>  
<span data-ttu-id="14638-146">Nej.</span><span class="sxs-lookup"><span data-stu-id="14638-146">No.</span></span> <span data-ttu-id="14638-147">Det här alternativet kommer inte att hämta alla konfigurationsinställningar och ska inte användas.</span><span class="sxs-lookup"><span data-stu-id="14638-147">This option will not retrieve all configuration settings and should not be used.</span></span> <span data-ttu-id="14638-148">Du bör i stället hello guiden toocreate hello grundläggande konfigurationen på andra hello-server och använda hello sync regeln editor toogenerate PowerShell-skript toomove en anpassad regel mellan servrar.</span><span class="sxs-lookup"><span data-stu-id="14638-148">You should instead use hello wizard toocreate hello base configuration on hello second server and use hello sync rule editor toogenerate PowerShell scripts toomove any custom rule between servers.</span></span> <span data-ttu-id="14638-149">Se [svänga migrering](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="14638-149">See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span>

<span data-ttu-id="14638-150">**F: kan cachelagras lösenord för hello Azure-inloggning och kan detta förhindras eftersom den innehåller ett lösenord indataelementet med hello autocomplete = ”false” attributet?**</span><span class="sxs-lookup"><span data-stu-id="14638-150">**Q: Can passwords be cached for hello Azure sign-in page and can this be prevented since it contains a password input element with hello autocomplete = "false" attribute?**</span></span></br>
<span data-ttu-id="14638-151">Vi stöder för närvarande inte ändra hello HTML-attribut för hello inkommande lösenordsfält, inklusive hello autocomplete-tagg.</span><span class="sxs-lookup"><span data-stu-id="14638-151">We currently do not support modifying hello HTML attributes of hello Password input field, including hello autocomplete tag.</span></span> <span data-ttu-id="14638-152">Vi arbetar på en funktion som tillåter för anpassad Javascript som tillåter tooadd eventuella attributfält toohello lösenord.</span><span class="sxs-lookup"><span data-stu-id="14638-152">We are currently working on a feature that will allow for custom Javascript which will allow you tooadd any attribute toohello password field.</span></span> <span data-ttu-id="14638-153">Detta ska vara tillgängliga senare del av 2017.</span><span class="sxs-lookup"><span data-stu-id="14638-153">This should be available later part of 2017.</span></span>

<span data-ttu-id="14638-154">**F: på hello Azure-inloggningssida visas användarnamn för användare som har tidigare har loggat in.  Det här beteendet stängas av?**</span><span class="sxs-lookup"><span data-stu-id="14638-154">**Q: On hello Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</span></span></br>
<span data-ttu-id="14638-155">Vi stöder för närvarande inte ändra hello HTML-attribut för hello-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="14638-155">We currently do not support modifying hello HTML attributes of hello sign-in page.</span></span> <span data-ttu-id="14638-156">Vi arbetar på en funktion som tillåter för anpassad Javascript som tillåter tooadd eventuella attributfält toohello lösenord.</span><span class="sxs-lookup"><span data-stu-id="14638-156">We are currently working on a feature that will allow for custom Javascript which will allow you tooadd any attribute toohello password field.</span></span> <span data-ttu-id="14638-157">Detta ska vara tillgängliga senare del av 2017.</span><span class="sxs-lookup"><span data-stu-id="14638-157">This should be available later part of 2017.</span></span>

<span data-ttu-id="14638-158">**F: finns det ett sätt tooprevent samtidiga sessioner?**</span><span class="sxs-lookup"><span data-stu-id="14638-158">**Q: Is there a way tooprevent concurrent sessions?**</span></span></br>
<span data-ttu-id="14638-159">Nej.</span><span class="sxs-lookup"><span data-stu-id="14638-159">No.</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="14638-160">Felsökning</span><span class="sxs-lookup"><span data-stu-id="14638-160">Troubleshooting</span></span>
<span data-ttu-id="14638-161">**F: hur kan jag få hjälp med Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="14638-161">**Q: How can I get help with Azure AD Connect?**</span></span>

[<span data-ttu-id="14638-162">Sök hello Microsoft Knowledge Base (KB)</span><span class="sxs-lookup"><span data-stu-id="14638-162">Search hello Microsoft Knowledge Base (KB)</span></span>](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* <span data-ttu-id="14638-163">Sök hello Microsoft Knowledge Base (KB) för tekniska lösningar toocommon reparations problem med stöd för Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="14638-163">Search hello Microsoft Knowledge Base (KB) for technical solutions toocommon break-fix issues about Support for Azure AD Connect.</span></span>

[<span data-ttu-id="14638-164">Microsoft Azure Active Directory-forum</span><span class="sxs-lookup"><span data-stu-id="14638-164">Microsoft Azure Active Directory Forums</span></span>](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* <span data-ttu-id="14638-165">Du kan söka och Bläddra för tekniska frågor och svar från hello community eller egna fråga genom att klicka på [här](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span><span class="sxs-lookup"><span data-stu-id="14638-165">You can search and browse for technical questions and answers from hello community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span></span>

[<span data-ttu-id="14638-166">Teknisk support för Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="14638-166">Azure AD Connect customer support</span></span>](https://manage.windowsazure.com/?getsupport=true)

* <span data-ttu-id="14638-167">Använd den här länken tooget support via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="14638-167">Use this link tooget support through hello Azure portal.</span></span>

