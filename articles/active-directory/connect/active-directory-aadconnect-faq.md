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
ms.openlocfilehash: fd5988b2d4170166902bb5cc39603d4a0f83be59
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a><span data-ttu-id="0b2f4-103">Vanliga frågor om Azure Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="0b2f4-103">Frequently asked questions for Azure Active Directory Connect</span></span>

## <a name="general-installation"></a><span data-ttu-id="0b2f4-104">Allmän installation</span><span class="sxs-lookup"><span data-stu-id="0b2f4-104">General installation</span></span>
<span data-ttu-id="0b2f4-105">**F: installationen fungerar om den Azure AD-administratör har aktiverat 2FA?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-105">**Q: Will installation work if the Azure AD Global Admin has 2FA enabled?**</span></span>  
<span data-ttu-id="0b2f4-106">Med versionerna från februari 2016 stöds detta.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-106">With the builds from February 2016, this is supported.</span></span>

<span data-ttu-id="0b2f4-107">**F: finns det ett sätt att installera Azure AD Connect obevakad?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-107">**Q: Is there a way to install Azure AD Connect unattended?**</span></span>  
<span data-ttu-id="0b2f4-108">Det kan bara användas för att installera Azure AD Connect med installationsguiden.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-108">It is only supported to install Azure AD Connect using the installation wizard.</span></span> <span data-ttu-id="0b2f4-109">En obevakad och tyst installation stöds inte.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-109">An unattended and silent installation is not supported.</span></span>

<span data-ttu-id="0b2f4-110">**F: Jag har en skog där en domän inte kan kontaktas. Hur installerar Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-110">**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**</span></span>  
<span data-ttu-id="0b2f4-111">Med versionerna från februari 2016 stöds detta.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-111">With the builds from February 2016, this is supported.</span></span>

<span data-ttu-id="0b2f4-112">**F: hälsoagenten AD DS fungerar på server core?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-112">**Q: Does the AD DS health agent work on server core?**</span></span>  
<span data-ttu-id="0b2f4-113">Ja.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-113">Yes.</span></span> <span data-ttu-id="0b2f4-114">När du har installerat agenten måste slutföra du registreringen med följande PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="0b2f4-114">After installing the agent, you can complete the registration process using the following PowerShell cmdlet:</span></span> 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a><span data-ttu-id="0b2f4-115">Nätverk</span><span class="sxs-lookup"><span data-stu-id="0b2f4-115">Network</span></span>
<span data-ttu-id="0b2f4-116">**F: Jag har en brandvägg, nätverksenhet eller något annat som begränsar de maximala tiden anslutningarna kan vara öppen i nätverket. Hur lång tid ska min klient sida timeout tröskelvärdet vara när du använder Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-116">**Q: I have a firewall, network device, or something else that limits the maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**</span></span>  
<span data-ttu-id="0b2f4-117">Alla nätverksprogramvara fysiska enheter eller något annat som begränsar den maximala tiden anslutningar kan förbli öppen bör använda ett tröskelvärde på minst 5 minuter (300 sekunder) för anslutningen mellan den server där Azure AD Connect-klienten är installerad och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-117">All networking software, physical devices, or anything else that limits the maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between the server where the Azure AD Connect client is installed and Azure Active Directory.</span></span> <span data-ttu-id="0b2f4-118">Detta gäller även alla tidigare Microsoft Identity synkroniseringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-118">This also applies to all previously released Microsoft Identity synchronization tools.</span></span>

<span data-ttu-id="0b2f4-119">**F: är SLDs (enkel etikett domäner) stöds?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-119">**Q: Are SLDs (Single Label Domains) supported?**</span></span>  
<span data-ttu-id="0b2f4-120">Nej, Azure AD Connect har inte stöd för lokala skogar och domäner med SLDs.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-120">No, Azure AD Connect does not support on-premises forests/domains using SLDs.</span></span>

<span data-ttu-id="0b2f4-121">**F: är ”prickad” NetBios-namnet stöds?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-121">**Q: Are "dotted" NetBios named supported?**</span></span>  
<span data-ttu-id="0b2f4-122">Nej, Azure AD Connect har inte stöd för lokala skogar och domäner där NetBios-namnet innehåller en punkt ””. i namnet.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-122">No, Azure AD Connect does not support on-premises forests/domains where the NetBios name contains a period "." in the name.</span></span>

## <a name="federation"></a><span data-ttu-id="0b2f4-123">Federation</span><span class="sxs-lookup"><span data-stu-id="0b2f4-123">Federation</span></span>
<span data-ttu-id="0b2f4-124">**F: Vad gör jag om jag får ett e-postmeddelande som ber mig att förnya certifikatet min Office 365**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-124">**Q: What do I do if I receive an email that asking me to renew my Office 365 certificate**</span></span>  
<span data-ttu-id="0b2f4-125">Använd de anvisningar som beskrivs i den [förnya certifikat](active-directory-aadconnect-o365-certs.md) avsnittet om hur du förnyar certifikatet.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-125">Use the guidance that is outlined in the [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how to renew the certificate.</span></span>

<span data-ttu-id="0b2f4-126">**F: Jag har ”Uppdatera automatiskt förlitande part” för O365 förlitande part. Måste jag göra något när min certifikatet för tokensignering automatiskt rullar över?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-126">**Q: I have "Automatically update relying party" set for O365 relying party. Do I have to take any action when my token signing certificate automatically rolls over?**</span></span>  
<span data-ttu-id="0b2f4-127">Använd de anvisningar som beskrivs i artikel [förnya certifikat](active-directory-aadconnect-o365-certs.md).</span><span class="sxs-lookup"><span data-stu-id="0b2f4-127">Use the guidance that is outlined in the article [renew certificates](active-directory-aadconnect-o365-certs.md).</span></span>

## <a name="environment"></a><span data-ttu-id="0b2f4-128">Miljö</span><span class="sxs-lookup"><span data-stu-id="0b2f4-128">Environment</span></span>
<span data-ttu-id="0b2f4-129">**F: finns det stöd för att byta namn på servern när du har installerat Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-129">**Q: Is it supported to rename the server after Azure AD Connect has been installed?**</span></span>  
<span data-ttu-id="0b2f4-130">Nej.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-130">No.</span></span> <span data-ttu-id="0b2f4-131">Ändrar namnet på servern kan Synkroniseringsmotorn inte kan ansluta till SQL-databasen och tjänsten kommer inte att kunna starta.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-131">Changing the server name will cause the sync engine to not be able to connect to the SQL database and the service will not be able to start.</span></span>

## <a name="identity-data"></a><span data-ttu-id="0b2f4-132">Identitetsdata</span><span class="sxs-lookup"><span data-stu-id="0b2f4-132">Identity data</span></span>
<span data-ttu-id="0b2f4-133">**F: UPN (userPrincipalName) attributet i Azure AD matchar inte lokal UPN - varför?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-133">**Q: The UPN (userPrincipalName) attribute in Azure AD does not match the on-prem UPN - why?**</span></span>  
<span data-ttu-id="0b2f4-134">Finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="0b2f4-134">See these articles:</span></span>

* [<span data-ttu-id="0b2f4-135">Användarnamnen i Office 365, Azure eller Intune matchar inte lokal UPN eller alternativ inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="0b2f4-135">User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID</span></span>](https://support.microsoft.com/en-us/kb/2523192)
* [<span data-ttu-id="0b2f4-136">Ändringarna synkroniserats inte med verktyget Azure Active Directory-synkronisering när du ändrar UPN-namnet för ett användarkonto om du vill använda en annan federerad domän</span><span class="sxs-lookup"><span data-stu-id="0b2f4-136">Changes aren't synced by the Azure Active Directory Sync tool after you change the UPN of a user account to use a different federated domain</span></span>](https://support.microsoft.com/en-us/kb/2669550)

<span data-ttu-id="0b2f4-137">Du kan också konfigurera Azure AD för att tillåta att uppdatera userPrincipalName som beskrivs i Synkroniseringsmotorn [Azure AD Connect sync tjänstens funktioner](active-directory-aadconnectsyncservice-features.md).</span><span class="sxs-lookup"><span data-stu-id="0b2f4-137">You can also configure Azure AD to allow the sync engine to update the userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).</span></span>

<span data-ttu-id="0b2f4-138">**F: är det stöd till mjuka matchning lokala AD-gruppen/kontakt-objekt med den befintliga Azure AD-grupp/kontakta objekt?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-138">**Q: Is it supported to soft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**</span></span>  
<span data-ttu-id="0b2f4-139">Nej, detta stöds för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-139">No, this is currently not supported.</span></span>

<span data-ttu-id="0b2f4-140">**F: är det stöd för att manuellt ange ImmutableId-attributet på befintliga Azure AD-grupp/kontakt-objekt som matchar den hårddisk lokala AD-gruppen/kontakt objekt?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-140">**Q: Is it supported to manually set ImmutableId attribute on existing Azure AD Group/Contact objects to hard match it to on-premises AD Group/Contact objects?**</span></span>  
<span data-ttu-id="0b2f4-141">Nej, detta stöds för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-141">No, this is currently not supported.</span></span>



## <a name="custom-configuration"></a><span data-ttu-id="0b2f4-142">Anpassad konfiguration</span><span class="sxs-lookup"><span data-stu-id="0b2f4-142">Custom configuration</span></span>
<span data-ttu-id="0b2f4-143">**F: var dokumenteras PowerShell-cmdlets för Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-143">**Q: Where are the PowerShell cmdlets for Azure AD Connect documented?**</span></span>  
<span data-ttu-id="0b2f4-144">Andra PowerShell-cmdletar finns i Azure AD Connect stöds inte för kundens användning med undantag för de cmdlets som beskrivs i den här platsen.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-144">With the exception of the cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.</span></span>

<span data-ttu-id="0b2f4-145">**F: kan jag använda ”Server exportservern/importera” hittades i *Synchronization Service Manager* att flytta configuration mellan servrar?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-145">**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* to move configuration between servers?**</span></span>  
<span data-ttu-id="0b2f4-146">Nej.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-146">No.</span></span> <span data-ttu-id="0b2f4-147">Det här alternativet kommer inte att hämta alla konfigurationsinställningar och ska inte användas.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-147">This option will not retrieve all configuration settings and should not be used.</span></span> <span data-ttu-id="0b2f4-148">I stället bör du använda guiden för att skapa den grundläggande konfigurationen på den andra servern och använda redigeraren för synkronisering av regeln för att generera PowerShell-skript för att flytta en anpassad regel mellan servrar.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-148">You should instead use the wizard to create the base configuration on the second server and use the sync rule editor to generate PowerShell scripts to move any custom rule between servers.</span></span> <span data-ttu-id="0b2f4-149">Se [svänga migrering](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="0b2f4-149">See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span>

<span data-ttu-id="0b2f4-150">**F: kan cachelagras lösenord för Azure inloggningssidan och kan detta förhindras eftersom den innehåller ett lösenord indataelementet med Komplettera automatiskt = ”false” attributet?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-150">**Q: Can passwords be cached for the Azure sign-in page and can this be prevented since it contains a password input element with the autocomplete = "false" attribute?**</span></span></br>
<span data-ttu-id="0b2f4-151">Vi stöder för närvarande inte ändrar HTML-attribut i ett lösenordsfält, inklusive autocomplete-taggen.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-151">We currently do not support modifying the HTML attributes of the Password input field, including the autocomplete tag.</span></span> <span data-ttu-id="0b2f4-152">Vi arbetar på en funktion som tillåter för anpassad Javascript som gör att du kan lägga till ett attribut i lösenordsfältet.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-152">We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field.</span></span> <span data-ttu-id="0b2f4-153">Detta ska vara tillgängliga senare del av 2017.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-153">This should be available later part of 2017.</span></span>

<span data-ttu-id="0b2f4-154">**F: på Azure-inloggning sidan visas användarnamn för användare som har tidigare har loggat in.  Det här beteendet stängas av?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-154">**Q: On the Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</span></span></br>
<span data-ttu-id="0b2f4-155">Vi stöder för närvarande inte ändra HTML-attribut på sidan logga in.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-155">We currently do not support modifying the HTML attributes of the sign-in page.</span></span> <span data-ttu-id="0b2f4-156">Vi arbetar på en funktion som tillåter för anpassad Javascript som gör att du kan lägga till ett attribut i lösenordsfältet.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-156">We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field.</span></span> <span data-ttu-id="0b2f4-157">Detta ska vara tillgängliga senare del av 2017.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-157">This should be available later part of 2017.</span></span>

<span data-ttu-id="0b2f4-158">**F: finns det ett sätt att förhindra samtidiga sessioner?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-158">**Q: Is there a way to prevent concurrent sessions?**</span></span></br>
<span data-ttu-id="0b2f4-159">Nej.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-159">No.</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="0b2f4-160">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0b2f4-160">Troubleshooting</span></span>
<span data-ttu-id="0b2f4-161">**F: hur kan jag få hjälp med Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="0b2f4-161">**Q: How can I get help with Azure AD Connect?**</span></span>

[<span data-ttu-id="0b2f4-162">Sök i Microsoft Knowledge Base (KB)</span><span class="sxs-lookup"><span data-stu-id="0b2f4-162">Search the Microsoft Knowledge Base (KB)</span></span>](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* <span data-ttu-id="0b2f4-163">Söka i Microsoft Knowledge Base (KB) för tekniska lösningar på vanliga problem med reparations om stöd för Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-163">Search the Microsoft Knowledge Base (KB) for technical solutions to common break-fix issues about Support for Azure AD Connect.</span></span>

[<span data-ttu-id="0b2f4-164">Microsoft Azure Active Directory-forum</span><span class="sxs-lookup"><span data-stu-id="0b2f4-164">Microsoft Azure Active Directory Forums</span></span>](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* <span data-ttu-id="0b2f4-165">Du kan söka och Bläddra för tekniska frågor och svar från gemenskapen eller egna fråga genom att klicka på [här](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span><span class="sxs-lookup"><span data-stu-id="0b2f4-165">You can search and browse for technical questions and answers from the community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span></span>

[<span data-ttu-id="0b2f4-166">Teknisk support för Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="0b2f4-166">Azure AD Connect customer support</span></span>](https://manage.windowsazure.com/?getsupport=true)

* <span data-ttu-id="0b2f4-167">Använd den här länken för att få support via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b2f4-167">Use this link to get support through the Azure portal.</span></span>

