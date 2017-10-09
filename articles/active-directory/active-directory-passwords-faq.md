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
ms.openlocfilehash: d04a9efeb3b35421aa605cadb2aa25f656a4d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="password-management-frequently-asked-questions"></a><span data-ttu-id="954e6-104">Vanliga och frågor svar om lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="954e6-104">Password management frequently asked questions</span></span>

<span data-ttu-id="954e6-105">hello följande är några vanliga frågor för allt som rör relaterade toopassword återställa.</span><span class="sxs-lookup"><span data-stu-id="954e6-105">hello following are some frequently asked questions for all things related toopassword reset.</span></span>

<span data-ttu-id="954e6-106">Om du har en allmän fråga om Azure AD och självbetjäning lösenord återställning som inte besvaras här, du kan be hello community för att få hjälp på hello [Azure Ad-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span><span class="sxs-lookup"><span data-stu-id="954e6-106">If you have a general question about Azure AD and self-service password reset, that is not answered here, you can ask hello community for assistance on hello [Azure Ad forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span></span> <span data-ttu-id="954e6-107">Medlemmar i communityn hello innehåller tekniker, projektledare, MVP och andra IT-proffs.</span><span class="sxs-lookup"><span data-stu-id="954e6-107">Members of hello community include Engineers, Product Managers, MVPs, and fellow IT Professionals.</span></span>

<span data-ttu-id="954e6-108">Dessa vanliga frågor är uppdelat i hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="954e6-108">This FAQ is split into hello following sections:</span></span>

* [<span data-ttu-id="954e6-109">**Frågor om registreringen för lösenordsåterställning**</span><span class="sxs-lookup"><span data-stu-id="954e6-109">**Questions about Password Reset Registration**</span></span>](#password-reset-registration)
* [<span data-ttu-id="954e6-110">**Frågor om återställning av lösenord**</span><span class="sxs-lookup"><span data-stu-id="954e6-110">**Questions about Password Reset**</span></span>](#password-reset)
* [<span data-ttu-id="954e6-111">**Frågor om ändring av lösenord**</span><span class="sxs-lookup"><span data-stu-id="954e6-111">**Questions about Password Change**</span></span>](#password-change)
* [<span data-ttu-id="954e6-112">**Frågor om lösenordshantering rapporter**</span><span class="sxs-lookup"><span data-stu-id="954e6-112">**Questions about password management Reports**</span></span>](#password-management-reports)
* [<span data-ttu-id="954e6-113">**Frågor om tillbakaskrivning av lösenord**</span><span class="sxs-lookup"><span data-stu-id="954e6-113">**Questions about password writeback**</span></span>](#password-writeback)

## <a name="password-reset-registration"></a><span data-ttu-id="954e6-114">Registrering för lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="954e6-114">Password reset registration</span></span>
* <span data-ttu-id="954e6-115">**F: kan användarna registrera sina egna data för återställning av lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-115">**Q:  Can my users register their own password reset data?**</span></span>

  > <span data-ttu-id="954e6-116">**S:** Ja, så länge återställning av lösenord är aktiverat och de licensierade kan de gå toohello registrering för lösenordsåterställning portalen på http://aka.ms/ssprsetup tooregister sina autentiseringsinformationen.</span><span class="sxs-lookup"><span data-stu-id="954e6-116">**A:** Yes, as long as password reset is enabled and they are licensed, they can go toohello Password Reset Registration portal at http://aka.ms/ssprsetup tooregister their authentication information.</span></span> <span data-ttu-id="954e6-117">Användare kan också registrera genom att gå åtkomstpanelen för toohello på http://myapps.microsoft.com, fliken för hello-profil och klicka på hello registrering för lösenordsåterställning alternativet.</span><span class="sxs-lookup"><span data-stu-id="954e6-117">Users can also register by going toohello access panel at http://myapps.microsoft.com, clicking hello profile tab, and clicking hello Register for Password Reset option.</span></span>
  >
  >
* <span data-ttu-id="954e6-118">**F: kan jag definiera data om återställning av lösenord för åt mina användare?**</span><span class="sxs-lookup"><span data-stu-id="954e6-118">**Q:  Can I define password reset data on behalf of my users?**</span></span>

  > <span data-ttu-id="954e6-119">**S:** Ja, kan du göra det med Azure AD Connect PowerShell hello [Azure-portalen](https://portal.azure.com), eller hello administrationsportalen för Office.</span><span class="sxs-lookup"><span data-stu-id="954e6-119">**A:** Yes, you can do so with Azure AD Connect, PowerShell, hello [Azure portal](https://portal.azure.com), or hello Office Admin portal.</span></span> <span data-ttu-id="954e6-120">Mer information finns i artikeln hello [Data som används av Azure AD Självbetjäning för återställning av lösenord](active-directory-passwords-data.md).</span><span class="sxs-lookup"><span data-stu-id="954e6-120">For more information, see hello article [Data used by Azure AD Self-Service Password Reset](active-directory-passwords-data.md).</span></span>
  >
  >
* <span data-ttu-id="954e6-121">**F: kan jag synkroniserar data om säkerhetsfrågor lokalt?**</span><span class="sxs-lookup"><span data-stu-id="954e6-121">**Q:  Can I synchronize data for security questions from on premises?**</span></span>

  > <span data-ttu-id="954e6-122">**S:** detta inte är möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="954e6-122">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="954e6-123">**F: kan användarna registrera data så att andra användare inte kan se dessa data?**</span><span class="sxs-lookup"><span data-stu-id="954e6-123">**Q:  Can my users register data in such a way that other users cannot see this data?**</span></span>

  > <span data-ttu-id="954e6-124">**S:** Ja, när användare registrerar data med hjälp av hello återställa portalen för Lösenordsregistrering sparas i privata autentisering fält som endast visas av globala administratörer och hello användare.</span><span class="sxs-lookup"><span data-stu-id="954e6-124">**A:** Yes, when users register data using hello Password Reset Registration Portal it is saved into private authentication fields that are only visible by Global Administrators and hello user.</span></span>
    >
    > [!NOTE]
    > <span data-ttu-id="954e6-125">Om en **Azure administratörskontot** registrerar sina telefonnummer för autentisering som den också fylls i hello mobiltelefon fältet och är synliga.</span><span class="sxs-lookup"><span data-stu-id="954e6-125">If an **Azure Administrator account** registers their authentication phone number it is also populated into hello mobile phone field and is visible.</span></span>
    >
  >
  >
* <span data-ttu-id="954e6-126">**F: Mina användare har toobe registrerad innan de kan använda lösenordsåterställning?**</span><span class="sxs-lookup"><span data-stu-id="954e6-126">**Q:  Do my users have toobe registered before they can use password reset?**</span></span>

  > <span data-ttu-id="954e6-127">**S:** Nej, om du definierar tillräckligt med autentiseringsinformation för åt användare har inte tooregister.</span><span class="sxs-lookup"><span data-stu-id="954e6-127">**A:** No, if you define enough authentication information on their behalf, users do not have tooregister.</span></span> <span data-ttu-id="954e6-128">Lösenordsåterställning fungerar så länge data som lagras i hello relevanta fält i hello directory är korrekt formaterat.</span><span class="sxs-lookup"><span data-stu-id="954e6-128">Password reset works as long as you have properly formatted data stored in hello appropriate fields in hello directory.</span></span>
  >
  >
* <span data-ttu-id="954e6-129">**F: kan jag synkronisera eller ange hello telefon för autentisering, e-autentisering eller autentisering telefon fält åt mina användare?**</span><span class="sxs-lookup"><span data-stu-id="954e6-129">**Q:  Can I synchronize or set hello Authentication Phone, Authentication Email or Alternate Authentication Phone fields on behalf of my users?**</span></span>

  > <span data-ttu-id="954e6-130">**S:** detta inte är möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="954e6-130">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="954e6-131">**F: hur portalen för registrering av hello känner vilka alternativ tooshow Mina användare?**</span><span class="sxs-lookup"><span data-stu-id="954e6-131">**Q:  How does hello registration portal know which options tooshow my users?**</span></span>

  > <span data-ttu-id="954e6-132">**S:** hello lösenordsåterställning portalen för registrering av endast visar hello alternativ som du har aktiverat för dina användare.</span><span class="sxs-lookup"><span data-stu-id="954e6-132">**A:** hello password reset registration portal only shows hello options that you have enabled for your users.</span></span> <span data-ttu-id="954e6-133">Dessa alternativ finns under hello avsnitt princip för lösenordsåterställning för användare i din katalog konfigurera fliken. Detta innebär till exempel att om du inte aktiverar säkerhetsfrågor sedan användare inte som kan tooregister för det alternativet.</span><span class="sxs-lookup"><span data-stu-id="954e6-133">These options are found under hello User Password Reset Policy section of your directory’s Configure tab. For example, this means that if you do not enable security questions, then users are not able tooregister for that option.</span></span>
  >
  >
* <span data-ttu-id="954e6-134">**F: när en användare anses vara registrerad?**</span><span class="sxs-lookup"><span data-stu-id="954e6-134">**Q:  When is a user considered registered?**</span></span>

  > <span data-ttu-id="954e6-135">**S:** en användare anses registrerade för SSPR när de har registrerat minst hello **antal metoder krävs tooreset** som du har angett i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="954e6-135">**A:** A user is considered registered for SSPR when they have registered at least hello **Number of methods required tooreset** that you have set in hello [Azure portal](https://portal.azure.com).</span></span>
  >
  >
## <a name="password-reset"></a><span data-ttu-id="954e6-136">Lösenordsåterställning</span><span class="sxs-lookup"><span data-stu-id="954e6-136">Password reset</span></span>
* <span data-ttu-id="954e6-137">**F: hur länge vänta bör tooreceive en e-post, SMS eller telefonsamtal från lösenordsåterställning?**</span><span class="sxs-lookup"><span data-stu-id="954e6-137">**Q:  How long should I wait tooreceive an email, SMS, or phone call from password reset?**</span></span>

  > <span data-ttu-id="954e6-138">**S:** e-post, SMS-meddelanden och samtal ska komma in under en minut med hello normal fallet 5-20 sekunder.</span><span class="sxs-lookup"><span data-stu-id="954e6-138">**A:** Email, SMS messages, and phone calls should arrive in under one minute, with hello normal case being 5-20 seconds.</span></span>
    ><span data-ttu-id="954e6-139">Om du inte har fått hello-meddelande i den här tidsperiod:</span><span class="sxs-lookup"><span data-stu-id="954e6-139">If you do not receive hello notification in this time frame:</span></span>
        > * <span data-ttu-id="954e6-140">Kontrollera mappen för skräppost.</span><span class="sxs-lookup"><span data-stu-id="954e6-140">Check your junk folder.</span></span>
        > * <span data-ttu-id="954e6-141">Kontrollera hello eller en e-kontaktas är hello något du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="954e6-141">Check hello number or email being contacted is hello one you expect.</span></span>
        > * <span data-ttu-id="954e6-142">Kontrollera hello autentiseringsdata i hello directory är korrekt formaterad.</span><span class="sxs-lookup"><span data-stu-id="954e6-142">Check hello authentication data in hello directory is correctly formatted.</span></span>
                >     * <span data-ttu-id="954e6-143">Exempel: ”+ 1 4255551234” eller ”user@contoso.com”</span><span class="sxs-lookup"><span data-stu-id="954e6-143">Example: "+1 4255551234" or "user@contoso.com"</span></span>
  >
  >
* <span data-ttu-id="954e6-144">**F: på vilka språk som stöds av lösenordsåterställning?**</span><span class="sxs-lookup"><span data-stu-id="954e6-144">**Q:  What languages are supported by password reset?**</span></span>

  > <span data-ttu-id="954e6-145">**S:** hello lösenordsåterställning UI, SMS-meddelanden och röst anrop är lokaliserade till hello samma språk som stöds i Office 365.</span><span class="sxs-lookup"><span data-stu-id="954e6-145">**A:** hello password reset UI, SMS messages, and voice calls are localized in hello same languages that are supported in Office 365.</span></span>
  >
  >
* <span data-ttu-id="954e6-146">**F: vilka delar av hello lösenord Återställ hämta märkta när jag har angett organisationens företagsanpassning i min katalog har konfigurera fliken?**</span><span class="sxs-lookup"><span data-stu-id="954e6-146">**Q:  What parts of hello password reset experience get branded when I set organizational branding in my directory’s configure tab?**</span></span>

  > <span data-ttu-id="954e6-147">**S:** hello-lösenordsåterställning, portal visar din organisations logotyp och ger dig tooconfigure hello Kontakta din administratör länken toopoint tooa anpassade e-postadress eller URL.</span><span class="sxs-lookup"><span data-stu-id="954e6-147">**A:** hello password reset portal shows your organizational logo and allows you tooconfigure hello Contact your administrator link toopoint tooa custom email or URL.</span></span> <span data-ttu-id="954e6-148">E-postmeddelandet som skickas av lösenordsåterställning innehåller organisationens logotyp, färger, namn i hello brödtext hello e-post och anpassa namn.</span><span class="sxs-lookup"><span data-stu-id="954e6-148">Any email that gets sent by password reset includes your organization’s logo, colors, name in hello body of hello email, and customized from name.</span></span>
  >
  >
* <span data-ttu-id="954e6-149">**F: hur kan jag för att informera användarna om var toogo tooreset sina lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-149">**Q:  How can I educate my users about where toogo tooreset their passwords?**</span></span>

  > <span data-ttu-id="954e6-150">**S:** du kan skicka dina användare toohttps://passwordreset.microsoftonline.com direkt eller instruera tooclick hello **kan inte komma åt ditt kontolänken** finns på alla arbets- eller Skol-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="954e6-150">**A:** You can send your users toohttps://passwordreset.microsoftonline.com directly, or you can instruct them tooclick hello **Can’t access your account link** found on any Work or School sign-in page.</span></span> <span data-ttu-id="954e6-151">Du kan också publicera dessa länkar i en lättillgänglig tooyour plats-användare.</span><span class="sxs-lookup"><span data-stu-id="954e6-151">You can also publish these links in a place easily accessible tooyour users.</span></span>
  >
  >
* <span data-ttu-id="954e6-152">**F: kan jag använda den här sidan från en mobil enhet?**</span><span class="sxs-lookup"><span data-stu-id="954e6-152">**Q:  Can I use this page from a mobile device?**</span></span>

  > <span data-ttu-id="954e6-153">**S:** Ja, den här sidan fungerar på mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="954e6-153">**A:** Yes, this page works on mobile devices.</span></span>
  >
  >
* <span data-ttu-id="954e6-154">**F: du stöder upplåsning lokala active directory-konton när användarna återställa sina lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-154">**Q:  Do you support unlocking local active directory accounts when users reset their passwords?**</span></span>

  > <span data-ttu-id="954e6-155">**S:** Ja, när en användare återställer sitt lösenord och tillbakaskrivning av lösenord har distribuerats med Azure AD Connect, användarens konto låses automatiskt när de återställa sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="954e6-155">**A:** Yes, when a user resets their password and password writeback has been deployed using Azure AD Connect, that user’s account is automatically unlocked when they reset their password.</span></span>
  >
  >
* <span data-ttu-id="954e6-156">**F: hur kan jag integrera lösenordsåterställning direkt till min användarens inloggning Skrivbordsmiljö?**</span><span class="sxs-lookup"><span data-stu-id="954e6-156">**Q:  How can I integrate password reset directly into my user’s desktop sign-in experience?**</span></span>

  > <span data-ttu-id="954e6-157">**S:** om du är en Azure AD Premium-kund kan du kan installera Microsoft Identity Manager utan extra kostnad och distribuera hello lokala lösenord Återställ lösning toomeet det här kravet.</span><span class="sxs-lookup"><span data-stu-id="954e6-157">**A:** If you are an Azure AD Premium customer, you can install Microsoft Identity Manager at no additional cost and deploy hello on-premises password reset solution toomeet this requirement.</span></span>
  >
  >
* <span data-ttu-id="954e6-158">**F: kan jag ange olika säkerhetsfrågor för olika språk?**</span><span class="sxs-lookup"><span data-stu-id="954e6-158">**Q:  Can I set different security questions for different locales?**</span></span>

  > <span data-ttu-id="954e6-159">**S:** detta inte är möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="954e6-159">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="954e6-160">**F: hur många frågor kan vi konfigurera för hello säkerhetsfrågor autentiseringsalternativet?**</span><span class="sxs-lookup"><span data-stu-id="954e6-160">**Q:  How many questions can we configure for hello Security Questions authentication option?**</span></span>

  > <span data-ttu-id="954e6-161">**S:** kan du konfigurera too20 anpassade säkerhetsfrågor i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="954e6-161">**A:** You can configure up too20 custom security questions in hello [Azure portal](https://portal.azure.com).</span></span>
  >
  >
* <span data-ttu-id="954e6-162">**F: hur lång tid kanske säkerhetsfrågor?**</span><span class="sxs-lookup"><span data-stu-id="954e6-162">**Q:  How long may security questions be?**</span></span>

  > <span data-ttu-id="954e6-163">**S:** säkerhetsfrågor kan innehålla mellan 3 och 200 tecken.</span><span class="sxs-lookup"><span data-stu-id="954e6-163">**A:** Security questions may be between 3 and 200 characters long.</span></span>
  >
  >
* <span data-ttu-id="954e6-164">**F: hur lång tid kanske svar toosecurity frågor?**</span><span class="sxs-lookup"><span data-stu-id="954e6-164">**Q:  How long may answers toosecurity questions be?**</span></span>

  > <span data-ttu-id="954e6-165">**S:** svar får inte vara 3 too40 tecken.</span><span class="sxs-lookup"><span data-stu-id="954e6-165">**A:** Answers may be 3 too40 characters long.</span></span>
  >
  >
* <span data-ttu-id="954e6-166">**F: dubblettsvar toosecurity frågor avvisas?**</span><span class="sxs-lookup"><span data-stu-id="954e6-166">**Q:  Are duplicate answers toosecurity questions rejected?**</span></span>

  > <span data-ttu-id="954e6-167">**S:** Ja, vi avvisa dubblettsvar toosecurity frågor.</span><span class="sxs-lookup"><span data-stu-id="954e6-167">**A:** Yes, we reject duplicate answers toosecurity questions.</span></span>
  >
  >
* <span data-ttu-id="954e6-168">**F: kan en användare registrera hello samma säkerhetsfråga mer än en gång?**</span><span class="sxs-lookup"><span data-stu-id="954e6-168">**Q:  May a user register hello same security question more than once?**</span></span>

  > <span data-ttu-id="954e6-169">**S:** Nej, när en användare som registrerar en fråga, de kan inte registrera dig för att fråga en andra gång.</span><span class="sxs-lookup"><span data-stu-id="954e6-169">**A:** No, once a user registers a particular question, they may not register for that question a second time.</span></span>
  >
  >
* <span data-ttu-id="954e6-170">**F: är det möjligt tooset minimigräns av säkerhetsfrågor för registrering och återställning av?**</span><span class="sxs-lookup"><span data-stu-id="954e6-170">**Q:  Is it possible tooset a minimum limit of security questions for registration and reset?**</span></span>

  > <span data-ttu-id="954e6-171">**S:** Ja, en gräns kan anges för registrering och en annan för återställning.</span><span class="sxs-lookup"><span data-stu-id="954e6-171">**A:** Yes, one limit can be set for registration and another for reset.</span></span> <span data-ttu-id="954e6-172">3-5 säkerhetsfrågor kan krävas för registrering och 3-5 kan krävas för återställning.</span><span class="sxs-lookup"><span data-stu-id="954e6-172">3-5 security questions may be required for registration and 3-5 may be required for reset.</span></span>
  >
  >
* <span data-ttu-id="954e6-173">**F: om en användare har registrerat mer än hello maximalt antal frågor krävs tooreset, hur säkerhetsfrågor väljs under återställning?**</span><span class="sxs-lookup"><span data-stu-id="954e6-173">**Q:  If a user has registered more than hello maximum number of questions required tooreset, how are security questions selected during reset?**</span></span>

  > <span data-ttu-id="954e6-174">**S:** N säkerhet frågorna väljs slumpmässigt hello Totalt antal frågor en användare har registrerats för, där N är hello **antalet frågor nödvändiga tooreset**.</span><span class="sxs-lookup"><span data-stu-id="954e6-174">**A:** N security questions are selected at random out of hello total number of questions a user has registered for, where N is hello **Number of questions required tooreset**.</span></span> <span data-ttu-id="954e6-175">Till exempel om en användare har 5 säkerhetsfrågor registrerade, men endast 3 är obligatoriska tooreset, är 3 av hello 5 väljas slumpmässigt och uppvisas vid återställning.</span><span class="sxs-lookup"><span data-stu-id="954e6-175">For example, if a user has 5 security questions registered, but only 3 are required tooreset, 3 of hello 5 are selected randomly and presented at reset.</span></span> <span data-ttu-id="954e6-176">Om hello användare hämtar hello svar toohello frågor fel, återkommer hello markeringen processen tooprevent fråga hammering.</span><span class="sxs-lookup"><span data-stu-id="954e6-176">If hello user gets hello answers toohello questions wrong, hello selection process reoccurs tooprevent question hammering.</span></span>
  >
  >
* <span data-ttu-id="954e6-177">**F: du hindrar användare från att försöka lösenordsåterställning många gånger under en kort tidsperiod?**</span><span class="sxs-lookup"><span data-stu-id="954e6-177">**Q:  Do you prevent users from attempting password reset many times in a short time period?**</span></span>

  > <span data-ttu-id="954e6-178">**S:** Ja, det finns inbyggda i lösenord Återställ tooprotect från missbruk säkerhetsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="954e6-178">**A:** Yes, there are security features built into password reset tooprotect from misuse.</span></span> <span data-ttu-id="954e6-179">Användare kan bara försök 5 Återställ lösenordsförsök inom en timme innan är utelåst under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="954e6-179">Users may only try 5 password reset attempts within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="954e6-180">Användare kan bara försöker toovalidate ett telefonnummer 5 gånger inom en timme innan är utelåst under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="954e6-180">Users may only try toovalidate a phone number 5 times within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="954e6-181">Användare kan bara försöker en enda autentiseringsmetod 5 gånger inom en timme innan är utelåst under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="954e6-181">Users may only try a single authentication method 5 times within an hour before being locked out for 24 hours.</span></span>
  >
  >
* <span data-ttu-id="954e6-182">**F: för hur länge är hello e-post och SMS engångskod giltiga?**</span><span class="sxs-lookup"><span data-stu-id="954e6-182">**Q:  For how long are hello email and SMS one-time passcode valid?**</span></span>

  > <span data-ttu-id="954e6-183">**S:** hello sessioners livstid för återställning av lösenord är 105 minuter.</span><span class="sxs-lookup"><span data-stu-id="954e6-183">**A:** hello session lifetime for password reset is 105 minutes.</span></span> <span data-ttu-id="954e6-184">Från början hello hello lösenordsåterställning åtgärden, hello användaren har 105 minuter tooreset sitt lösenord.</span><span class="sxs-lookup"><span data-stu-id="954e6-184">From hello beginning of hello password reset operation, hello user has 105 minutes tooreset their password.</span></span> <span data-ttu-id="954e6-185">hello är e-post och SMS engångskod ogiltiga när den här tiden har löpt ut.</span><span class="sxs-lookup"><span data-stu-id="954e6-185">hello email and SMS one-time passcode are invalid after this time period expires.</span></span>
  >
  >

## <a name="password-change"></a><span data-ttu-id="954e6-186">Ändra lösenordet</span><span class="sxs-lookup"><span data-stu-id="954e6-186">Password change</span></span>
* <span data-ttu-id="954e6-187">**F: bör användarna var toochange sina lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-187">**Q:  Where should my users go toochange their passwords?**</span></span>

  > <span data-ttu-id="954e6-188">**S:** användare kan ändra sina lösenord överallt där de ser sina profilbild eller ikonen (precis som i hello övre högra hörnet på sina [Office 365](https://portal.office.com) eller [åtkomstpanelen](https://myapps.microsoft.com) inträffar.</span><span class="sxs-lookup"><span data-stu-id="954e6-188">**A:** Users may change their passwords anywhere they see their profile picture or icon (like in hello upper right corner of their [Office 365](https://portal.office.com) or [Access Panel](https://myapps.microsoft.com) experiences.</span></span> <span data-ttu-id="954e6-189">Användare kan ändra sina lösenord från hello [profil åtkomstpanelsidan](https://account.activedirectory.windowsazure.com/r#/profile).</span><span class="sxs-lookup"><span data-stu-id="954e6-189">Users may change their passwords from hello [Access Panel profile page](https://account.activedirectory.windowsazure.com/r#/profile).</span></span> <span data-ttu-id="954e6-190">Användare kan också vara och toochange sina lösenord automatiskt vid hello Azure AD inloggningssida om sina lösenord har gått ut.</span><span class="sxs-lookup"><span data-stu-id="954e6-190">Users may also be asked toochange their passwords automatically at hello Azure AD sign-in screen if their passwords have expired.</span></span> <span data-ttu-id="954e6-191">Slutligen användare kan navigera toohello [Azure AD-lösenord ändra Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) direkt om de vill toochange sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="954e6-191">Finally, users may navigate toohello [Azure AD Password Change Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) directly if they wish toochange their passwords.</span></span>
  >
  >
* <span data-ttu-id="954e6-192">**F: kan användarna meddelas i hello Office-portalen när sina lokala lösenord upphör att gälla?**</span><span class="sxs-lookup"><span data-stu-id="954e6-192">**Q:  Can my users be notified in hello Office Portal when their on-premises password expires?**</span></span>

  > <span data-ttu-id="954e6-193">**S:** detta är idag möjligt om du använder AD FS genom att följa hello anvisningarna här: [skicka lösenord princip anspråk med AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="954e6-193">**A:** This is possible today if you are using ADFS by following hello instructions here: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span></span> <span data-ttu-id="954e6-194">Om du använder hash-synkronisering av lösenord, är det inte möjligt i dag.</span><span class="sxs-lookup"><span data-stu-id="954e6-194">If you are using password hash synchronization, this is not possible today.</span></span> <span data-ttu-id="954e6-195">Detta beror på att vi inte synkronisera lösenordsprinciper lokalt, så det inte är möjligt för oss toopost upphör att gälla meddelanden toocloud inträffar.</span><span class="sxs-lookup"><span data-stu-id="954e6-195">This is because we do not sync password policies from on-premises, so it is not possible for us toopost expiry notifications toocloud experiences.</span></span> <span data-ttu-id="954e6-196">I båda fallen kan också för[meddela användare vars lösenord om tooexpire med hjälp av PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="954e6-196">In either case, it is also possible too[notify users whose passwords are about tooexpire by using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span></span>
  >
  >

## <a name="password-management-reports"></a><span data-ttu-id="954e6-197">Rapporter för lösenord</span><span class="sxs-lookup"><span data-stu-id="954e6-197">Password management reports</span></span>
* <span data-ttu-id="954e6-198">**F: hur lång tid tar det för data tooshow in på rapporter hello lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-198">**Q:  How long does it take for data tooshow up on hello password management reports?**</span></span>

  > <span data-ttu-id="954e6-199">**S:** Data ska visas på hello lösenord rapporter inom 5-10 minuter.</span><span class="sxs-lookup"><span data-stu-id="954e6-199">**A:** Data should appear on hello password management reports within 5-10 minutes.</span></span> <span data-ttu-id="954e6-200">Den vissa instanser som det kan ta upp tooan timme tooappear.</span><span class="sxs-lookup"><span data-stu-id="954e6-200">It some instances it may take up tooan hour tooappear.</span></span>
  >
  >
* <span data-ttu-id="954e6-201">**F: hur filtrera rapporter hello lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-201">**Q:  How can I filter hello password management reports?**</span></span>

  > <span data-ttu-id="954e6-202">**S:** du kan filtrera rapporter hello lösenord genom att klicka på hello små förstoringsglas toohello extremt höger av hello kolumnetiketterna hello övre delen av hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="954e6-202">**A:** You can filter hello password management reports by clicking hello small magnifying glass toohello extreme right of hello column labels, near hello top of hello report.</span></span> <span data-ttu-id="954e6-203">Om du vill toodo bättre filtrering kan du hämta hello rapporten tooexcel och skapa en pivottabell.</span><span class="sxs-lookup"><span data-stu-id="954e6-203">If you want toodo richer filtering, you can download hello report tooexcel and create a pivot table.</span></span>
  >
  >
* <span data-ttu-id="954e6-204">**F: Vad är hello högsta antalet händelser lagras i rapporter hello lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-204">**Q: What is hello maximum number of events are stored in hello password management reports?**</span></span>

  > <span data-ttu-id="954e6-205">**S:** in too75, 000 lösenord återställning eller lösenord Återställ registrering händelser lagras i hello lösenord rapporter, utsträckning säkerhetskopiera too30 dagar.</span><span class="sxs-lookup"><span data-stu-id="954e6-205">**A:** Up too75,000 password reset or password reset registration events are stored in hello password management reports, spanning back up too30 days.</span></span>  <span data-ttu-id="954e6-206">Vi arbetar tooexpand detta number tooinclude fler händelser.</span><span class="sxs-lookup"><span data-stu-id="954e6-206">We are working tooexpand this number tooinclude more events.</span></span>
  >
  >
* <span data-ttu-id="954e6-207">**F: hur långt tillbaka går hello lösenord rapporter?**</span><span class="sxs-lookup"><span data-stu-id="954e6-207">**Q:  How far back do hello password management reports go?**</span></span>

  > <span data-ttu-id="954e6-208">**S:** hello lösenordshantering rapporterar Visa åtgärder som sker inom hello senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="954e6-208">**A:** hello password management reports show operations occurring within hello last 30 days.</span></span> <span data-ttu-id="954e6-209">För tillfället, om du behöver tooarchive data, kan du hämta hello rapporter med jämna mellanrum och spara dem på en annan plats.</span><span class="sxs-lookup"><span data-stu-id="954e6-209">For now, if you need tooarchive this data, you can download hello reports periodically and save them in a separate location.</span></span>
  >
  >
* <span data-ttu-id="954e6-210">**F: finns det ett maximalt antal rader som kan visas på rapporter hello lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-210">**Q:  Is there a maximum number of rows that can appear on hello password management reports?**</span></span>

  > <span data-ttu-id="954e6-211">**S:** Ja, högst 75 000 rader kan visas på något av hello lösenord rapporter, om de som visas i hello användargränssnitt eller håller på att hämtas.</span><span class="sxs-lookup"><span data-stu-id="954e6-211">**A:** Yes, a maximum of 75,000 rows may appear on either of hello password management reports, whether they are being shown in hello UI or being downloaded.</span></span>
  >
  >
* <span data-ttu-id="954e6-212">**F: finns det en API-tooaccess hello lösenord återställning eller registrering rapporteringsdata?**</span><span class="sxs-lookup"><span data-stu-id="954e6-212">**Q:  Is there an API tooaccess hello password reset or registration reporting data?**</span></span>

  > <span data-ttu-id="954e6-213">**S:** Ja, hello finns följande dokumentation toolearn hur du kan komma åt hello lösenord återställa reporting dataström.</span><span class="sxs-lookup"><span data-stu-id="954e6-213">**A:** Yes, see hello following documentation toolearn how you can access hello password reset reporting data stream.</span></span>  <span data-ttu-id="954e6-214">[Lär dig hur tooaccess lösenordsåterställning rapporteringshändelser programmässigt](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span><span class="sxs-lookup"><span data-stu-id="954e6-214">[Learn how tooaccess password reset reporting events programmatically](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span></span>
  >
  >

## <a name="password-writeback"></a><span data-ttu-id="954e6-215">Tillbakaskrivning av lösenord</span><span class="sxs-lookup"><span data-stu-id="954e6-215">Password writeback</span></span>
* <span data-ttu-id="954e6-216">**F: hur fungerar tillbakaskrivning av lösenord hello bakgrunden?**</span><span class="sxs-lookup"><span data-stu-id="954e6-216">**Q:  How does password writeback work behind hello scenes?**</span></span>

  > <span data-ttu-id="954e6-217">**S:** finns [hur tillbakaskrivning av lösenord fungerar](active-directory-passwords-writeback.md) för en förklaring om vad som händer när du aktiverar tillbakaskrivning av lösenord och hur data som flödar genom hello system tillbaka till din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="954e6-217">**A:** See [How password writeback works](active-directory-passwords-writeback.md) for an explanation of what happens when you enable password writeback, and how data flows through hello system back into your on-premises environment.</span></span>
  >
  >
* <span data-ttu-id="954e6-218">**F: hur lång tid tar tillbakaskrivning av lösenord toowork?  Finns det en synkronisering fördröjning som med synkronisering av lösenords-hash?**</span><span class="sxs-lookup"><span data-stu-id="954e6-218">**Q:  How long does password writeback take toowork?  Is there a synchronization delay like with password hash sync?**</span></span>

  > <span data-ttu-id="954e6-219">**S:** tillbakaskrivning av lösenord är snabbmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="954e6-219">**A:** Password writeback is instant.</span></span> <span data-ttu-id="954e6-220">Det är en synkron pipeline som fungerar helt annorlunda än hash-synkronisering av lösenord.</span><span class="sxs-lookup"><span data-stu-id="954e6-220">It is a synchronous pipeline that works fundamentally differently than password hash synchronization.</span></span> <span data-ttu-id="954e6-221">Tillbakaskrivning av lösenord kan användare tooget realtid feedback om hello genomförandet av sitt lösenord återställa eller ändra igen.</span><span class="sxs-lookup"><span data-stu-id="954e6-221">Password writeback allows users tooget real-time feedback about hello success of their password reset or change operation.</span></span> <span data-ttu-id="954e6-222">hello Genomsnittlig tid för en lyckad tillbakaskrivning av lösenord är under 500 ms.</span><span class="sxs-lookup"><span data-stu-id="954e6-222">hello average time for a successful writeback of a password is under 500 ms.</span></span>
  >
  >
* <span data-ttu-id="954e6-223">**F: hur påverkas mitt konto/molnåtkomst om mitt lokalt konto har inaktiverats?**</span><span class="sxs-lookup"><span data-stu-id="954e6-223">**Q:  If my on-premises account is disabled, how is my cloud account/access affected?**</span></span>

  > <span data-ttu-id="954e6-224">**S:** om ditt lokala ID inaktiveras ditt moln-ID-/ access inaktiveras även vid hello nästa synkroniseringsintervall via AAD Connect byt standard är var 30: e minut.</span><span class="sxs-lookup"><span data-stu-id="954e6-224">**A:** If your on-premises ID is disabled, your cloud ID/access will also be disabled at hello next sync interval via AAD Connect byt default this is every 30 minutes.</span></span>
  >
  >
* <span data-ttu-id="954e6-225">**F: om mitt konto lokalt är begränsad av en lokal Active Directory-lösenordsprincip, SSPR lyder under den här principen när jag ändrar hello lösenord?**</span><span class="sxs-lookup"><span data-stu-id="954e6-225">**Q:  If my on-premises account is constrained by an on-premises Active Directory password policy, does SSPR obey this policy when I change hello password?**</span></span>

  > <span data-ttu-id="954e6-226">**S:** Ja, SSPR förlitar sig på och som följer av hello lokala AD-lösenordsprincip, inklusive vanliga lösenordsprinciper för AD i domänen, samt några definierade detaljerade lösenordsprinciper riktade tooa som anges av användaren.</span><span class="sxs-lookup"><span data-stu-id="954e6-226">**A:** Yes, SSPR relies on and abides by hello on-premises AD password policy, including typical AD domain password policy, as well as any defined fine grained password policies targeted tooa given user.</span></span>
  >
  >
* <span data-ttu-id="954e6-227">**F: vilka typer av konton fungerar tillbakaskrivning av lösenord för?**</span><span class="sxs-lookup"><span data-stu-id="954e6-227">**Q:  What types of accounts does password writeback work for?**</span></span>

  > <span data-ttu-id="954e6-228">**S:** tillbakaskrivning av lösenord fungerar för federerat och lösenord Hash synkroniserade användare.</span><span class="sxs-lookup"><span data-stu-id="954e6-228">**A:** Password writeback works for Federated and Password Hash Synchronized users.</span></span>
  >
  >
* <span data-ttu-id="954e6-229">**F: tillbakaskrivning av lösenord kan genomdriva lösenordsprinciper för min domän?**</span><span class="sxs-lookup"><span data-stu-id="954e6-229">**Q:  Does password writeback enforce my domain’s password policies?**</span></span>

  > <span data-ttu-id="954e6-230">**S:** Ja, tillbakaskrivning av lösenord tillämpar ålder för lösenord, historik, komplexitet, filter och andra begränsningar som du kan infördes för lösenord i den lokala domänen.</span><span class="sxs-lookup"><span data-stu-id="954e6-230">**A:** Yes, password writeback enforces password age, history, complexity, filters, and any other restriction you may put in place on passwords in your local domain.</span></span>
  >
  >
* <span data-ttu-id="954e6-231">**F: är tillbakaskrivning av lösenord säker?  Hur vet jag att jag inte hämta över?**</span><span class="sxs-lookup"><span data-stu-id="954e6-231">**Q:  Is password writeback secure?  How can I be sure I won’t get hacked?**</span></span>

  > <span data-ttu-id="954e6-232">**S:** Ja, tillbakaskrivning av lösenord är säker.</span><span class="sxs-lookup"><span data-stu-id="954e6-232">**A:** Yes, password writeback is secure.</span></span> <span data-ttu-id="954e6-233">tooread mer om hello fyra säkerhetslagren implementeras av hello lösenord tillbakaskrivning av tjänsten, kolla hello [lösenord tillbakaskrivning säkerhetsmodell](active-directory-passwords-writeback.md#password-writeback-security-model) avsnitt i hur tillbakaskrivning av lösenord fungerar.</span><span class="sxs-lookup"><span data-stu-id="954e6-233">tooread more about hello four layers of security implemented by hello password writeback service, check out hello [Password writeback security model](active-directory-passwords-writeback.md#password-writeback-security-model) section in How password writeback works.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="954e6-234">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="954e6-234">Next steps</span></span>

<span data-ttu-id="954e6-235">hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD</span><span class="sxs-lookup"><span data-stu-id="954e6-235">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="954e6-236">[**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD</span><span class="sxs-lookup"><span data-stu-id="954e6-236">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="954e6-237">[**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering</span><span class="sxs-lookup"><span data-stu-id="954e6-237">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="954e6-238">[**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering</span><span class="sxs-lookup"><span data-stu-id="954e6-238">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="954e6-239">[**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här</span><span class="sxs-lookup"><span data-stu-id="954e6-239">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="954e6-240">[**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.</span><span class="sxs-lookup"><span data-stu-id="954e6-240">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="954e6-241">[**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner</span><span class="sxs-lookup"><span data-stu-id="954e6-241">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="954e6-242">[**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord</span><span class="sxs-lookup"><span data-stu-id="954e6-242">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="954e6-243">[**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?</span><span class="sxs-lookup"><span data-stu-id="954e6-243">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="954e6-244">[**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="954e6-244">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="954e6-245">[**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR</span><span class="sxs-lookup"><span data-stu-id="954e6-245">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>
