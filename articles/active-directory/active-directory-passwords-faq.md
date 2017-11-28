---
title: "Vanliga frågor och svar: Azure AD SSPR | Microsoft Docs"
description: "Vanliga frågor och svar om Azure AD-lösenordet för självbetjäning återställa"
services: active-directory
keywords: "Hantering av Active directory-lösenord, lösenordshantering, Azure AD self service för lösenordsåterställning"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: b3fab99ff9fab5bc67fa70113dc5b06fac775b09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="password-management-frequently-asked-questions"></a><span data-ttu-id="7ca5d-104">Vanliga och frågor svar om lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="7ca5d-104">Password management frequently asked questions</span></span>

<span data-ttu-id="7ca5d-105">Följande är några vanliga frågor och svar för alla objekt som är relaterade till återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-105">The following are some frequently asked questions for all things related to password reset.</span></span>

<span data-ttu-id="7ca5d-106">Om du har en allmän fråga om Azure AD och självbetjäning lösenord återställning som inte besvaras här, du kan be gemenskapen för att få hjälp på den [Azure Ad-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-106">If you have a general question about Azure AD and self-service password reset, that is not answered here, you can ask the community for assistance on the [Azure Ad forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span></span> <span data-ttu-id="7ca5d-107">Medlemmar i gruppen innehåller tekniker, projektledare, MVP och andra IT-proffs.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-107">Members of the community include Engineers, Product Managers, MVPs, and fellow IT Professionals.</span></span>

<span data-ttu-id="7ca5d-108">Dessa vanliga frågor är uppdelat i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7ca5d-108">This FAQ is split into the following sections:</span></span>

* [<span data-ttu-id="7ca5d-109">**Frågor om registreringen för lösenordsåterställning**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-109">**Questions about Password Reset Registration**</span></span>](#password-reset-registration)
* [<span data-ttu-id="7ca5d-110">**Frågor om återställning av lösenord**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-110">**Questions about Password Reset**</span></span>](#password-reset)
* [<span data-ttu-id="7ca5d-111">**Frågor om ändring av lösenord**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-111">**Questions about Password Change**</span></span>](#password-change)
* [<span data-ttu-id="7ca5d-112">**Frågor om lösenordshantering rapporter**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-112">**Questions about password management Reports**</span></span>](#password-management-reports)
* [<span data-ttu-id="7ca5d-113">**Frågor om tillbakaskrivning av lösenord**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-113">**Questions about password writeback**</span></span>](#password-writeback)

## <a name="password-reset-registration"></a><span data-ttu-id="7ca5d-114">Registrering för lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="7ca5d-114">Password reset registration</span></span>
* <span data-ttu-id="7ca5d-115">**F: kan användarna registrera sina egna data för återställning av lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-115">**Q:  Can my users register their own password reset data?**</span></span>

  > <span data-ttu-id="7ca5d-116">**S:** Ja, så länge återställning av lösenord är aktiverat och de licensierade kan de gå till portalen för registrering för lösenordsåterställning på http://aka.ms/ssprsetup att registrera sina autentiseringsinformationen.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-116">**A:** Yes, as long as password reset is enabled and they are licensed, they can go to the Password Reset Registration portal at http://aka.ms/ssprsetup to register their authentication information.</span></span> <span data-ttu-id="7ca5d-117">Användarna kan också registrera genom att gå till åtkomstpanelen på http://myapps.microsoft.com, på fliken profil och på Registrera dig för lösenordsåterställning alternativet.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-117">Users can also register by going to the access panel at http://myapps.microsoft.com, clicking the profile tab, and clicking the Register for Password Reset option.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-118">**F: kan jag definiera data om återställning av lösenord för åt mina användare?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-118">**Q:  Can I define password reset data on behalf of my users?**</span></span>

  > <span data-ttu-id="7ca5d-119">**S:** Ja, kan du göra det med Azure AD Connect PowerShell den [Azure-portalen](https://portal.azure.com), eller administrationsportalen för Office.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-119">**A:** Yes, you can do so with Azure AD Connect, PowerShell, the [Azure portal](https://portal.azure.com), or the Office Admin portal.</span></span> <span data-ttu-id="7ca5d-120">Mer information finns i artikeln [Data som används av Azure AD Självbetjäning för återställning av lösenord](active-directory-passwords-data.md).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-120">For more information, see the article [Data used by Azure AD Self-Service Password Reset](active-directory-passwords-data.md).</span></span>
  >
  >
* <span data-ttu-id="7ca5d-121">**F: kan jag synkroniserar data om säkerhetsfrågor lokalt?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-121">**Q:  Can I synchronize data for security questions from on premises?**</span></span>

  > <span data-ttu-id="7ca5d-122">**S:** detta inte är möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-122">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-123">**F: kan användarna registrera data så att andra användare inte kan se dessa data?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-123">**Q:  Can my users register data in such a way that other users cannot see this data?**</span></span>

  > <span data-ttu-id="7ca5d-124">**S:** Ja, när användare registrerar data med hjälp av återställa Lösenordsregistreringsportal sparas i privata autentisering fält som endast visas av globala administratörer och användare.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-124">**A:** Yes, when users register data using the Password Reset Registration Portal it is saved into private authentication fields that are only visible by Global Administrators and the user.</span></span>
    >
    > [!NOTE]
    > <span data-ttu-id="7ca5d-125">Om en **Azure administratörskontot** registrerar sina telefonnummer för autentisering så fylls också i fältet mobiltelefon och är synliga.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-125">If an **Azure Administrator account** registers their authentication phone number it is also populated into the mobile phone field and is visible.</span></span>
    >
  >
  >
* <span data-ttu-id="7ca5d-126">**F: Mina användare måste registreras innan de kan använda lösenordsåterställning?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-126">**Q:  Do my users have to be registered before they can use password reset?**</span></span>

  > <span data-ttu-id="7ca5d-127">**S:** Nej, om du definierar tillräckligt med autentiseringsinformation för åt användarna behöver inte registrera.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-127">**A:** No, if you define enough authentication information on their behalf, users do not have to register.</span></span> <span data-ttu-id="7ca5d-128">Lösenordsåterställning fungerar så länge som du har korrekt formaterat data som lagras i relevanta fält i katalogen.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-128">Password reset works as long as you have properly formatted data stored in the appropriate fields in the directory.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-129">**F: kan jag synkronisera eller ange telefon för autentisering, e-autentisering eller autentisering telefon fält åt mina användare?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-129">**Q:  Can I synchronize or set the Authentication Phone, Authentication Email or Alternate Authentication Phone fields on behalf of my users?**</span></span>

  > <span data-ttu-id="7ca5d-130">**S:** detta inte är möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-130">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-131">**F: hur vet vilket alternativ för att visa mina användare i portalen för registrering?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-131">**Q:  How does the registration portal know which options to show my users?**</span></span>

  > <span data-ttu-id="7ca5d-132">**S:** på registreringsportalen för lösenordsåterställning bara visar alternativ som du har aktiverat för dina användare.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-132">**A:** The password reset registration portal only shows the options that you have enabled for your users.</span></span> <span data-ttu-id="7ca5d-133">Dessa alternativ finns under avsnittet princip för lösenordsåterställning för användare i din katalog konfigurera fliken.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-133">These options are found under the User Password Reset Policy section of your directory’s Configure tab.</span></span> <span data-ttu-id="7ca5d-134">Detta innebär till exempel att om du inte aktiverar säkerhetsfrågor sedan användare inte kan registrera dig för det alternativet.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-134">For example, this means that if you do not enable security questions, then users are not able to register for that option.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-135">**F: när en användare anses vara registrerad?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-135">**Q:  When is a user considered registered?**</span></span>

  > <span data-ttu-id="7ca5d-136">**S:** en användare anses registrerade för SSPR när de har registrerat minst **antal metoder som krävs för att återställa** som du har angett i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-136">**A:** A user is considered registered for SSPR when they have registered at least the **Number of methods required to reset** that you have set in the [Azure portal](https://portal.azure.com).</span></span>
  >
  >
## <a name="password-reset"></a><span data-ttu-id="7ca5d-137">Lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="7ca5d-137">Password reset</span></span>
* <span data-ttu-id="7ca5d-138">**F: hur lång tid ska gå att få ett e-post, SMS eller telefonsamtal från lösenordsåterställning?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-138">**Q:  How long should I wait to receive an email, SMS, or phone call from password reset?**</span></span>

  > <span data-ttu-id="7ca5d-139">**S:** e-post, SMS-meddelanden och samtal ska komma in under en minut med normal fallet 5-20 sekunder.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-139">**A:** Email, SMS messages, and phone calls should arrive in under one minute, with the normal case being 5-20 seconds.</span></span>
    ><span data-ttu-id="7ca5d-140">Om du inte har fått meddelandet inom den här tiden:</span><span class="sxs-lookup"><span data-stu-id="7ca5d-140">If you do not receive the notification in this time frame:</span></span>
        > * <span data-ttu-id="7ca5d-141">Kontrollera mappen för skräppost.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-141">Check your junk folder.</span></span>
        > * <span data-ttu-id="7ca5d-142">Kontrollera numret eller e-post som kontaktas är den som du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-142">Check the number or email being contacted is the one you expect.</span></span>
        > * <span data-ttu-id="7ca5d-143">Kontrollera autentiseringsdata i katalogen är korrekt formaterad.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-143">Check the authentication data in the directory is correctly formatted.</span></span>
                >     * <span data-ttu-id="7ca5d-144">Exempel: ”+ 1 4255551234” eller ”user@contoso.com”</span><span class="sxs-lookup"><span data-stu-id="7ca5d-144">Example: "+1 4255551234" or "user@contoso.com"</span></span>
  >
  >
* <span data-ttu-id="7ca5d-145">**F: på vilka språk som stöds av lösenordsåterställning?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-145">**Q:  What languages are supported by password reset?**</span></span>

  > <span data-ttu-id="7ca5d-146">**S:** Användargränssnittet för lösenordsåterställning SMS-meddelanden och samtal är lokaliserade till samma språk som stöds i Office 365.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-146">**A:** The password reset UI, SMS messages, and voice calls are localized in the same languages that are supported in Office 365.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-147">**F: Vad delar av återställning av lösenord hämta märkta när jag har angett organisationens företagsanpassning i min katalog har konfigurera fliken?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-147">**Q:  What parts of the password reset experience get branded when I set organizational branding in my directory’s configure tab?**</span></span>

  > <span data-ttu-id="7ca5d-148">**S:** portalen för återställning av lösenord visar din organisations logotyp och du kan konfigurera kontakten länken administratören att peka till en anpassad e-postadress eller URL.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-148">**A:** The password reset portal shows your organizational logo and allows you to configure the Contact your administrator link to point to a custom email or URL.</span></span> <span data-ttu-id="7ca5d-149">E-postmeddelandet som skickas av lösenordsåterställning innehåller organisationens logotyp, färger, namn i brödtexten i e-postmeddelandet och anpassa namn.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-149">Any email that gets sent by password reset includes your organization’s logo, colors, name in the body of the email, and customized from name.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-150">**F: hur kan jag för att informera användarna om hur du går att återställa sina lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-150">**Q:  How can I educate my users about where to go to reset their passwords?**</span></span>

  > <span data-ttu-id="7ca5d-151">**S:** kan du skicka användarna till https://passwordreset.microsoftonline.com direkt eller kan du be dem att klicka på den **kan inte komma åt ditt kontolänken** finns på alla arbets- eller Skol-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-151">**A:** You can send your users to https://passwordreset.microsoftonline.com directly, or you can instruct them to click the **Can’t access your account link** found on any Work or School sign-in page.</span></span> <span data-ttu-id="7ca5d-152">Du kan också publicera dessa länkar på en plats som är lätt tillgängliga för användarna.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-152">You can also publish these links in a place easily accessible to your users.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-153">**F: kan jag använda den här sidan från en mobil enhet?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-153">**Q:  Can I use this page from a mobile device?**</span></span>

  > <span data-ttu-id="7ca5d-154">**S:** Ja, den här sidan fungerar på mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-154">**A:** Yes, this page works on mobile devices.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-155">**F: du stöder upplåsning lokala active directory-konton när användarna återställa sina lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-155">**Q:  Do you support unlocking local active directory accounts when users reset their passwords?**</span></span>

  > <span data-ttu-id="7ca5d-156">**S:** Ja, när en användare återställer sitt lösenord och tillbakaskrivning av lösenord har distribuerats med Azure AD Connect, användarens konto låses automatiskt när de återställa sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-156">**A:** Yes, when a user resets their password and password writeback has been deployed using Azure AD Connect, that user’s account is automatically unlocked when they reset their password.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-157">**F: hur kan jag integrera lösenordsåterställning direkt till min användarens inloggning Skrivbordsmiljö?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-157">**Q:  How can I integrate password reset directly into my user’s desktop sign-in experience?**</span></span>

  > <span data-ttu-id="7ca5d-158">**S:** om du är en Azure AD Premium-kund kan du installera Microsoft Identity Manager utan extra kostnad och distribuera återställningslösning för lokala lösenord för att uppfylla detta krav.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-158">**A:** If you are an Azure AD Premium customer, you can install Microsoft Identity Manager at no additional cost and deploy the on-premises password reset solution to meet this requirement.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-159">**F: kan jag ange olika säkerhetsfrågor för olika språk?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-159">**Q:  Can I set different security questions for different locales?**</span></span>

  > <span data-ttu-id="7ca5d-160">**S:** detta inte är möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-160">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-161">**F: hur många frågor kan vi konfigurera för alternativet säkerhetsfrågor autentisering?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-161">**Q:  How many questions can we configure for the Security Questions authentication option?**</span></span>

  > <span data-ttu-id="7ca5d-162">**S:** du kan konfigurera upp till 20 anpassade säkerhetsfrågor i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-162">**A:** You can configure up to 20 custom security questions in the [Azure portal](https://portal.azure.com).</span></span>
  >
  >
* <span data-ttu-id="7ca5d-163">**F: hur lång tid kanske säkerhetsfrågor?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-163">**Q:  How long may security questions be?**</span></span>

  > <span data-ttu-id="7ca5d-164">**S:** säkerhetsfrågor kan innehålla mellan 3 och 200 tecken.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-164">**A:** Security questions may be between 3 and 200 characters long.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-165">**F: hur lång tid kanske svar på säkerhetsfrågor?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-165">**Q:  How long may answers to security questions be?**</span></span>

  > <span data-ttu-id="7ca5d-166">**S:** svar får inte vara 3 40 tecken.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-166">**A:** Answers may be 3 to 40 characters long.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-167">**F: är dubbla svar på säkerhetsfrågor avvisade?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-167">**Q:  Are duplicate answers to security questions rejected?**</span></span>

  > <span data-ttu-id="7ca5d-168">**S:** Ja, vi avvisa dubbla svar på säkerhetsfrågor.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-168">**A:** Yes, we reject duplicate answers to security questions.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-169">**F: kan en användare registrera samma säkerhetsfråga mer än en gång?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-169">**Q:  May a user register the same security question more than once?**</span></span>

  > <span data-ttu-id="7ca5d-170">**S:** Nej, när en användare som registrerar en fråga, de kan inte registrera dig för att fråga en andra gång.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-170">**A:** No, once a user registers a particular question, they may not register for that question a second time.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-171">**F: är det möjligt att ange minimigräns av säkerhetsfrågor för registrering och återställa?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-171">**Q:  Is it possible to set a minimum limit of security questions for registration and reset?**</span></span>

  > <span data-ttu-id="7ca5d-172">**S:** Ja, en gräns kan anges för registrering och en annan för återställning.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-172">**A:** Yes, one limit can be set for registration and another for reset.</span></span> <span data-ttu-id="7ca5d-173">3-5 säkerhetsfrågor kan krävas för registrering och 3-5 kan krävas för återställning.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-173">3-5 security questions may be required for registration and 3-5 may be required for reset.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-174">**F: om en användare har registrerats fler än det maximala antalet frågor som krävs för att återställa, hur säkerhetsfrågor väljs under återställning?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-174">**Q:  If a user has registered more than the maximum number of questions required to reset, how are security questions selected during reset?**</span></span>

  > <span data-ttu-id="7ca5d-175">**S:** N säkerhetsfrågor väljs slumpmässigt utanför det totala antalet frågor en användare har registrerats för, där N är den **antalet frågor som krävs för att återställa**.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-175">**A:** N security questions are selected at random out of the total number of questions a user has registered for, where N is the **Number of questions required to reset**.</span></span> <span data-ttu-id="7ca5d-176">Till exempel om en användare har 5 säkerhetsfrågor registrerade, men endast 3 krävs för att återställa, är 3 av 5 valt slumpmässigt och uppvisas vid återställning.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-176">For example, if a user has 5 security questions registered, but only 3 are required to reset, 3 of the 5 are selected randomly and presented at reset.</span></span> <span data-ttu-id="7ca5d-177">Om användaren får svar på frågorna fel återkommer val av processen för att förhindra att fråga hammering.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-177">If the user gets the answers to the questions wrong, the selection process reoccurs to prevent question hammering.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-178">**F: du hindrar användare från att försöka lösenordsåterställning många gånger under en kort tidsperiod?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-178">**Q:  Do you prevent users from attempting password reset many times in a short time period?**</span></span>

  > <span data-ttu-id="7ca5d-179">**S:** Ja, det finns säkerhetsfunktioner som är inbyggda i för att skydda från missbruk för återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-179">**A:** Yes, there are security features built into password reset to protect from misuse.</span></span> <span data-ttu-id="7ca5d-180">Användare kan bara försök 5 Återställ lösenordsförsök inom en timme innan är utelåst under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-180">Users may only try 5 password reset attempts within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="7ca5d-181">Användare kan endast försök att validera ett telefonnummer 5 gånger inom en timme innan är utelåst under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-181">Users may only try to validate a phone number 5 times within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="7ca5d-182">Användare kan bara försöker en enda autentiseringsmetod 5 gånger inom en timme innan är utelåst under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-182">Users may only try a single authentication method 5 times within an hour before being locked out for 24 hours.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-183">**F: för hur länge är e-post och SMS engångskod giltiga?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-183">**Q:  For how long are the email and SMS one-time passcode valid?**</span></span>

  > <span data-ttu-id="7ca5d-184">**S:** sessioners livstid för återställning av lösenord är 105 minuter.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-184">**A:** The session lifetime for password reset is 105 minutes.</span></span> <span data-ttu-id="7ca5d-185">Användaren har 105 minuter att återställa sina lösenord från början av återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-185">From the beginning of the password reset operation, the user has 105 minutes to reset their password.</span></span> <span data-ttu-id="7ca5d-186">E-post och SMS engångskod är ogiltiga när den här tiden har löpt ut.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-186">The email and SMS one-time passcode are invalid after this time period expires.</span></span>
  >
  >

## <a name="password-change"></a><span data-ttu-id="7ca5d-187">Ändra lösenordet</span><span class="sxs-lookup"><span data-stu-id="7ca5d-187">Password change</span></span>
* <span data-ttu-id="7ca5d-188">**F: var ska gå Mina användare ändra sina lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-188">**Q:  Where should my users go to change their passwords?**</span></span>

  > <span data-ttu-id="7ca5d-189">**S:** användare kan ändra sina lösenord överallt där de ser sina profilbild eller ikonen (precis som i det övre högra hörnet av deras [Office 365](https://portal.office.com) eller [åtkomstpanelen](https://myapps.microsoft.com) inträffar.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-189">**A:** Users may change their passwords anywhere they see their profile picture or icon (like in the upper right corner of their [Office 365](https://portal.office.com) or [Access Panel](https://myapps.microsoft.com) experiences.</span></span> <span data-ttu-id="7ca5d-190">Användare kan ändra sina lösenord från den [profil åtkomstpanelsidan](https://account.activedirectory.windowsazure.com/r#/profile).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-190">Users may change their passwords from the [Access Panel profile page](https://account.activedirectory.windowsazure.com/r#/profile).</span></span> <span data-ttu-id="7ca5d-191">Användare kan också behöva ändra sina lösenord automatiskt på skärmen för Azure AD om sina lösenord har gått ut.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-191">Users may also be asked to change their passwords automatically at the Azure AD sign-in screen if their passwords have expired.</span></span> <span data-ttu-id="7ca5d-192">Slutligen användare kan navigera till den [Azure AD-lösenord ändra Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) direkt om de vill ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-192">Finally, users may navigate to the [Azure AD Password Change Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) directly if they wish to change their passwords.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-193">**F: kan användarna meddelas i Office-portalen när sina lokala lösenord upphör att gälla?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-193">**Q:  Can my users be notified in the Office Portal when their on-premises password expires?**</span></span>

  > <span data-ttu-id="7ca5d-194">**S:** detta är idag möjligt om du använder AD FS genom att följa anvisningarna här: [skicka lösenord princip anspråk med AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-194">**A:** This is possible today if you are using ADFS by following the instructions here: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span></span> <span data-ttu-id="7ca5d-195">Om du använder hash-synkronisering av lösenord, är det inte möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-195">If you are using password hash synchronization, this is not possible today.</span></span> <span data-ttu-id="7ca5d-196">Det beror på att vi inte synkronisera lösenordsprinciper från lokalt, så det inte är möjligt för oss att publicera utgången meddelanden till molnet upplevelser.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-196">This is because we do not sync password policies from on-premises, so it is not possible for us to post expiry notifications to cloud experiences.</span></span> <span data-ttu-id="7ca5d-197">I båda fallen är det också möjligt att [meddela användare vars lösenord ska upphöra att gälla med hjälp av PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-197">In either case, it is also possible to [notify users whose passwords are about to expire by using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span></span>
  >
  >

## <a name="password-management-reports"></a><span data-ttu-id="7ca5d-198">Rapporter för lösenord</span><span class="sxs-lookup"><span data-stu-id="7ca5d-198">Password management reports</span></span>
* <span data-ttu-id="7ca5d-199">**F: hur lång tid tar det för att data ska visas i rapporter för lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-199">**Q:  How long does it take for data to show up on the password management reports?**</span></span>

  > <span data-ttu-id="7ca5d-200">**S:** Data ska visas på rapporterna som lösenord management inom 5-10 minuter.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-200">**A:** Data should appear on the password management reports within 5-10 minutes.</span></span> <span data-ttu-id="7ca5d-201">Den vissa instanser som det kan ta upp till en timme ska visas.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-201">It some instances it may take up to an hour to appear.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-202">**F: hur filtrera rapporter för lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-202">**Q:  How can I filter the password management reports?**</span></span>

  > <span data-ttu-id="7ca5d-203">**S:** du kan filtrera rapporter för lösenord genom att klicka på små förstoringsglaset till extrema höger om kolumnetiketterna längst upp i rapporten.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-203">**A:** You can filter the password management reports by clicking the small magnifying glass to the extreme right of the column labels, near the top of the report.</span></span> <span data-ttu-id="7ca5d-204">Om du vill göra bättre filtrering kan du hämta rapport till excel och skapa en pivottabell.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-204">If you want to do richer filtering, you can download the report to excel and create a pivot table.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-205">**F: Vad är det maximala antalet händelser som lagras i management-rapporter lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-205">**Q: What is the maximum number of events are stored in the password management reports?**</span></span>

  > <span data-ttu-id="7ca5d-206">**S:** upp till 75 000 återställning eller lösenord registreringen för lösenordsåterställning händelser lagras i rapporter för lösenord, utsträckning tillbaka upp till 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-206">**A:** Up to 75,000 password reset or password reset registration events are stored in the password management reports, spanning back up to 30 days.</span></span>  <span data-ttu-id="7ca5d-207">Vi arbetar för att expandera det här numret för att inkludera fler händelser.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-207">We are working to expand this number to include more events.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-208">**F: hur långt tillbaka göra lösenordshantering rapporter gå?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-208">**Q:  How far back do the password management reports go?**</span></span>

  > <span data-ttu-id="7ca5d-209">**S:** lösenordshanteringen rapporterar Visa åtgärder som sker inom de senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-209">**A:** The password management reports show operations occurring within the last 30 days.</span></span> <span data-ttu-id="7ca5d-210">Just nu om du vill arkivera informationen, kan du hämta rapporter med jämna mellanrum och spara dem på en annan plats.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-210">For now, if you need to archive this data, you can download the reports periodically and save them in a separate location.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-211">**F: finns det ett maximalt antal rader som kan visas på management-rapporter lösenord?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-211">**Q:  Is there a maximum number of rows that can appear on the password management reports?**</span></span>

  > <span data-ttu-id="7ca5d-212">**S:** Ja, högst 75 000 rader kan visas på något av lösenord för hantering av rapporter, om de som visas i Användargränssnittet eller håller på att hämtas.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-212">**A:** Yes, a maximum of 75,000 rows may appear on either of the password management reports, whether they are being shown in the UI or being downloaded.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-213">**F: finns det en API för åtkomst till lösenordsåterställning eller registrering rapporterar data?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-213">**Q:  Is there an API to access the password reset or registration reporting data?**</span></span>

  > <span data-ttu-id="7ca5d-214">**S:** Ja, finns följande dokumentation för att lära dig hur du kan komma åt lösenordsåterställning reporting dataström.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-214">**A:** Yes, see the following documentation to learn how you can access the password reset reporting data stream.</span></span>  <span data-ttu-id="7ca5d-215">[Lär dig att komma åt lösenordsåterställning rapporteringshändelser programmässigt](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span><span class="sxs-lookup"><span data-stu-id="7ca5d-215">[Learn how to access password reset reporting events programmatically](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span></span>
  >
  >

## <a name="password-writeback"></a><span data-ttu-id="7ca5d-216">Tillbakaskrivning av lösenord</span><span class="sxs-lookup"><span data-stu-id="7ca5d-216">Password writeback</span></span>
* <span data-ttu-id="7ca5d-217">**F: hur fungerar tillbakaskrivning av lösenord i bakgrunden?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-217">**Q:  How does password writeback work behind the scenes?**</span></span>

  > <span data-ttu-id="7ca5d-218">**S:** finns [hur tillbakaskrivning av lösenord fungerar](active-directory-passwords-writeback.md) för en förklaring om vad som händer när du aktiverar tillbakaskrivning av lösenord och hur data som flödar genom systemet tillbaka till din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-218">**A:** See [How password writeback works](active-directory-passwords-writeback.md) for an explanation of what happens when you enable password writeback, and how data flows through the system back into your on-premises environment.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-219">**F: hur lång tid tillbakaskrivning av lösenord tar ska fungera?  Finns det en synkronisering fördröjning som med synkronisering av lösenords-hash?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-219">**Q:  How long does password writeback take to work?  Is there a synchronization delay like with password hash sync?**</span></span>

  > <span data-ttu-id="7ca5d-220">**S:** tillbakaskrivning av lösenord är snabbmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-220">**A:** Password writeback is instant.</span></span> <span data-ttu-id="7ca5d-221">Det är en synkron pipeline som fungerar helt annorlunda än hash-synkronisering av lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-221">It is a synchronous pipeline that works fundamentally differently than password hash synchronization.</span></span> <span data-ttu-id="7ca5d-222">Tillbakaskrivning av lösenord kan användare få realtid feedback om genomförandet av deras återställning av lösenord eller ändra igen.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-222">Password writeback allows users to get real-time feedback about the success of their password reset or change operation.</span></span> <span data-ttu-id="7ca5d-223">Den genomsnittliga tiden för en lyckad tillbakaskrivning av lösenord är under 500 ms.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-223">The average time for a successful writeback of a password is under 500 ms.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-224">**F: hur påverkas mitt konto/molnåtkomst om mitt lokalt konto har inaktiverats?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-224">**Q:  If my on-premises account is disabled, how is my cloud account/access affected?**</span></span>

  > <span data-ttu-id="7ca5d-225">**S:** om ditt lokala ID inaktiveras ditt moln-ID-/ access inaktiveras även vid nästa synkroniseringsintervall via AAD Connect byt standard är var 30: e minut.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-225">**A:** If your on-premises ID is disabled, your cloud ID/access will also be disabled at the next sync interval via AAD Connect byt default this is every 30 minutes.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-226">**F: om mitt konto lokalt är begränsad av en lokal Active Directory-lösenordsprincip, SSPR lyder under den här principen när jag ändrar lösenordet?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-226">**Q:  If my on-premises account is constrained by an on-premises Active Directory password policy, does SSPR obey this policy when I change the password?**</span></span>

  > <span data-ttu-id="7ca5d-227">**S:** Ja, SSPR förlitar sig på och som följer av lokalt AD-lösenordsprincip, inklusive vanliga lösenordsprincip för AD-domän, samt alla definierats detaljerade lösenordsprinciper för en viss användare.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-227">**A:** Yes, SSPR relies on and abides by the on-premises AD password policy, including typical AD domain password policy, as well as any defined fine grained password policies targeted to a given user.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-228">**F: vilka typer av konton fungerar tillbakaskrivning av lösenord för?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-228">**Q:  What types of accounts does password writeback work for?**</span></span>

  > <span data-ttu-id="7ca5d-229">**S:** tillbakaskrivning av lösenord fungerar för federerat och lösenord Hash synkroniserade användare.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-229">**A:** Password writeback works for Federated and Password Hash Synchronized users.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-230">**F: tillbakaskrivning av lösenord kan genomdriva lösenordsprinciper för min domän?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-230">**Q:  Does password writeback enforce my domain’s password policies?**</span></span>

  > <span data-ttu-id="7ca5d-231">**S:** Ja, tillbakaskrivning av lösenord tillämpar ålder för lösenord, historik, komplexitet, filter och andra begränsningar som du kan infördes för lösenord i den lokala domänen.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-231">**A:** Yes, password writeback enforces password age, history, complexity, filters, and any other restriction you may put in place on passwords in your local domain.</span></span>
  >
  >
* <span data-ttu-id="7ca5d-232">**F: är tillbakaskrivning av lösenord säker?  Hur vet jag att jag inte hämta över?**</span><span class="sxs-lookup"><span data-stu-id="7ca5d-232">**Q:  Is password writeback secure?  How can I be sure I won’t get hacked?**</span></span>

  > <span data-ttu-id="7ca5d-233">**S:** Ja, tillbakaskrivning av lösenord är säker.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-233">**A:** Yes, password writeback is secure.</span></span> <span data-ttu-id="7ca5d-234">Om du vill läsa mer om de fyra säkerhetslagren implementerats av tjänsten för tillbakaskrivning av lösenord, ta en titt på [lösenord tillbakaskrivning säkerhetsmodell](active-directory-passwords-writeback.md#password-writeback-security-model) avsnitt i hur tillbakaskrivning av lösenord fungerar.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-234">To read more about the four layers of security implemented by the password writeback service, check out the [Password writeback security model](active-directory-passwords-writeback.md#password-writeback-security-model) section in How password writeback works.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="7ca5d-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ca5d-235">Next steps</span></span>

<span data-ttu-id="7ca5d-236">Följande länkar ger ytterligare information om lösenordsåterställning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ca5d-236">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="7ca5d-237">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ca5d-237">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="7ca5d-238">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="7ca5d-238">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="7ca5d-239">[**Data**](active-directory-passwords-data.md) – Förstå de data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="7ca5d-239">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="7ca5d-240">[**Distribution**](active-directory-passwords-best-practices.md) – Planera och distribuera SSPR till dina användare med hjälp av informationen finns här</span><span class="sxs-lookup"><span data-stu-id="7ca5d-240">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="7ca5d-241">[**Anpassa**](active-directory-passwords-customize.md) – Anpassa utseendet för företagets SSPR-funktion.</span><span class="sxs-lookup"><span data-stu-id="7ca5d-241">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="7ca5d-242">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="7ca5d-242">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="7ca5d-243">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="7ca5d-243">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="7ca5d-244">[**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?</span><span class="sxs-lookup"><span data-stu-id="7ca5d-244">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="7ca5d-245">[**Teknisk djupdykning** ](active-directory-passwords-how-it-works.md) – Ta en titt bakom kulisserna för att förstå hur det hela fungerar</span><span class="sxs-lookup"><span data-stu-id="7ca5d-245">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="7ca5d-246">[**Felsökning** ](active-directory-passwords-troubleshoot.md) – Lär dig att lösa vanliga problem med SSPR</span><span class="sxs-lookup"><span data-stu-id="7ca5d-246">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>
