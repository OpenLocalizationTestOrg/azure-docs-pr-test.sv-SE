---
title: "aaaAzure Active Directory-enhetshantering vanliga frågor och svar | Microsoft Docs"
description: "Azure Active Directory enhetshantering vanliga frågor och svar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 000eb6a930187e13cb24cf628793afd06813be23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-device-management-faq"></a><span data-ttu-id="af37e-103">Azure Active Directory-enhetshantering vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="af37e-103">Azure Active Directory device management FAQ</span></span>

<span data-ttu-id="af37e-104">**F: jag registrerade nyligen hello enhet. Varför ser inte hello enhet under Mina användarinformation i hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="af37e-104">**Q: I registered hello device recently. Why can’t I see hello device under my user info in hello Azure portal?**</span></span>

<span data-ttu-id="af37e-105">**S:** Windows 10-enheter som är ansluten till domänen med automatisk enhetsregistrering visas inte under hello användarinformation.</span><span class="sxs-lookup"><span data-stu-id="af37e-105">**A:** Windows 10 devices that are domain-joined with automatic device registration do not show up under hello USER info.</span></span>
<span data-ttu-id="af37e-106">Toouse PowerShell toosee måste alla enheter.</span><span class="sxs-lookup"><span data-stu-id="af37e-106">You need toouse PowerShell toosee all devices.</span></span> 

<span data-ttu-id="af37e-107">Endast visas hello följande enheter under hello användarinformation:</span><span class="sxs-lookup"><span data-stu-id="af37e-107">Only hello following devices are listed under hello USER info:</span></span>

- <span data-ttu-id="af37e-108">Alla personliga enheter som inte är ansluten enterprise</span><span class="sxs-lookup"><span data-stu-id="af37e-108">All personal devices that are not enterprise joined</span></span> 
- <span data-ttu-id="af37e-109">Alla icke - Windows 10 / Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="af37e-109">All non-Windows 10 / Windows Server 2016</span></span> 
- <span data-ttu-id="af37e-110">Alla Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="af37e-110">All non-Windows devices</span></span> 

---

<span data-ttu-id="af37e-111">**F: Varför kan jag inte se alla hello-enheter som registrerats i Azure Active Directory i hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="af37e-111">**Q: Why can I not see all hello devices registered in Azure Active Directory in hello Azure portal?**</span></span> 

<span data-ttu-id="af37e-112">**S:** för närvarande finns ingen sätt toosee alla registrerade enheter i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="af37e-112">**A:** Currently, there is no way toosee all registered devices in hello Azure portal.</span></span> <span data-ttu-id="af37e-113">Du kan använda Azure PowerShell toofind alla enheter.</span><span class="sxs-lookup"><span data-stu-id="af37e-113">You can use Azure PowerShell toofind all devices.</span></span> <span data-ttu-id="af37e-114">Mer information finns i hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="af37e-114">For more details, see hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span></span>

--- 

<span data-ttu-id="af37e-115">**F: hur vet jag vilka hello enheten registreringstillstånd klientens hello är?**</span><span class="sxs-lookup"><span data-stu-id="af37e-115">**Q: How do I know what hello device registration state of hello client is?**</span></span>

<span data-ttu-id="af37e-116">**S:** registreringstillstånd för hello enhet beror på:</span><span class="sxs-lookup"><span data-stu-id="af37e-116">**A:** hello device registration state depends on:</span></span>

- <span data-ttu-id="af37e-117">Vilka hello-enheten är</span><span class="sxs-lookup"><span data-stu-id="af37e-117">What hello device is</span></span>
- <span data-ttu-id="af37e-118">Hur den har registrerats</span><span class="sxs-lookup"><span data-stu-id="af37e-118">How it was registered</span></span> 
- <span data-ttu-id="af37e-119">Information om eventuella relaterade tooit.</span><span class="sxs-lookup"><span data-stu-id="af37e-119">Any details related tooit.</span></span> 
 

---

<span data-ttu-id="af37e-120">**F: Varför är en enhet som jag har tagit bort hello Azure-portalen eller med Windows PowerShell fortfarande visas som har registrerats?**</span><span class="sxs-lookup"><span data-stu-id="af37e-120">**Q: Why is a device I have deleted in hello Azure portal or using Windows PowerShell still listed as registered?**</span></span>

<span data-ttu-id="af37e-121">**S:** detta är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="af37e-121">**A:** This is by design.</span></span> <span data-ttu-id="af37e-122">hello enheten har inte åtkomst tooresources i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="af37e-122">hello device will not have access tooresources in hello cloud.</span></span> <span data-ttu-id="af37e-123">Om du vill tooremove hello enhet och registrera igen, måste en manuell åtgärd vara toobe som vidtagits hello enhet.</span><span class="sxs-lookup"><span data-stu-id="af37e-123">If you want tooremove hello device and register again, a manual action must be toobe taken on hello device.</span></span> 

<span data-ttu-id="af37e-124">För Windows 10 och Windows Server 2016 som är lokala AD-domänansluten:</span><span class="sxs-lookup"><span data-stu-id="af37e-124">For Windows 10 and Windows Server 2016 that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="af37e-125">Öppna hello kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="af37e-125">Open hello command prompt as an administrator.</span></span>

2.  <span data-ttu-id="af37e-126">Typ`dsregcmd.exe /debug /leave`</span><span class="sxs-lookup"><span data-stu-id="af37e-126">Type `dsregcmd.exe /debug /leave`</span></span>

3.  <span data-ttu-id="af37e-127">Logga ut och logga in tootrigger hello schemalagd aktivitet som registrerar hello enheten igen.</span><span class="sxs-lookup"><span data-stu-id="af37e-127">Sign out and sign in tootrigger hello scheduled task that registers hello device again.</span></span> 

<span data-ttu-id="af37e-128">För andra Windows-plattformar som är lokala AD-domänansluten:</span><span class="sxs-lookup"><span data-stu-id="af37e-128">For other Windows platforms that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="af37e-129">Öppna hello kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="af37e-129">Open hello command prompt as an administrator.</span></span>
2.  <span data-ttu-id="af37e-130">Skriv `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span><span class="sxs-lookup"><span data-stu-id="af37e-130">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span></span>
3.  <span data-ttu-id="af37e-131">Skriv `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span><span class="sxs-lookup"><span data-stu-id="af37e-131">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span></span>

---

<span data-ttu-id="af37e-132">**F: Varför visas dubblettenhet poster i Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="af37e-132">**Q: Why do I see duplicate device entries in Azure portal?**</span></span>

<span data-ttu-id="af37e-133">**S:**</span><span class="sxs-lookup"><span data-stu-id="af37e-133">**A:**</span></span>

-   <span data-ttu-id="af37e-134">För Windows 10 och Windows Server 2016, om de upprepade försök toounjoin och ansluta igen hello samma enhet, kan det finnas dubbla poster.</span><span class="sxs-lookup"><span data-stu-id="af37e-134">For Windows 10 and Windows Server 2016, if they are repeated attempts toounjoin and re-join hello same device, there might be duplicate entries.</span></span> 

-   <span data-ttu-id="af37e-135">Om du har använt Lägg till arbets- eller Skolkonto, de windowsanvändare som använder Lägg till arbets- eller Skolkonto skapar en ny enhetspost med hello samma enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="af37e-135">If you have used Add Work or School Account, each windows user who uses Add Work or School Account will create a new device record with hello same device name.</span></span>

-   <span data-ttu-id="af37e-136">Andra Windows-plattformar som är lokala AD som ingår i domänen med hjälp av automatisk registrering skapar en ny enhetspost med hello samma enhetsnamn för varje domänanvändare som loggar in hello enhet.</span><span class="sxs-lookup"><span data-stu-id="af37e-136">Other Windows platforms that are on-premises AD domain-joined using automatic registration will create a new device record with hello same device name for each domain user who logs into hello device.</span></span> 

-   <span data-ttu-id="af37e-137">En AADJ-dator som har rensats kan installeras igen och nytt ansluts till hello samma namn, visas som en ny post med hello samma enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="af37e-137">An AADJ machine that has been wiped, re-installed and re-joined with hello same name, will show up as another record with hello same device name.</span></span>

---

<span data-ttu-id="af37e-138">**F: Varför kan en användare fortfarande komma åt resurser från en enhet som jag har inaktiverats i hello Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="af37e-138">**Q: Why can a user still access resources from a device I have disabled in hello Azure portal?**</span></span>

<span data-ttu-id="af37e-139">**S:** kan det ta upp tooan timmen för en återkalla toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="af37e-139">**A:** It can take up tooan hour for a revoke toobe applied.</span></span>

>[!Note] 
><span data-ttu-id="af37e-140">För förlorade enheter rekommenderar vi att rensa hello enheten tooensure att användare inte kan komma åt hello enhet.</span><span class="sxs-lookup"><span data-stu-id="af37e-140">For lost devices, we recommend wiping hello device tooensure that users cannot access hello device.</span></span> <span data-ttu-id="af37e-141">Mer information finns i [registrera enheter för hantering i Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="af37e-141">For more details, see [Enroll devices for management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span></span> 


---

<span data-ttu-id="af37e-142">**F: Varför Mina användare ser inte ”du kan hämta det här”?**</span><span class="sxs-lookup"><span data-stu-id="af37e-142">**Q: Why do my users see “You can’t get there from here”?**</span></span>

<span data-ttu-id="af37e-143">**S:** om du har konfigurerat vissa villkorlig åtkomst regler toorequire tillstånd för en specifik enhet och hello enheten uppfyller inte kraven för hello kan användare blockeras och finns i det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="af37e-143">**A:** If you have configured certain conditional access rules toorequire a specific device state and hello device does not meet hello criteria, users are blocked and see this message.</span></span> <span data-ttu-id="af37e-144">Kontrollera utvärdera hello regler och se till att hello-enheten är kan toomeet hello kriterier tooavoid det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="af37e-144">Please evaluate hello rules and ensure that hello device is able toomeet hello criteria tooavoid this message.</span></span>

---


<span data-ttu-id="af37e-145">**F: jag finns hello enhetspost under hello användarinformation i hello Azure-portalen och kan se hello tillstånd som har registrerats på hello-klienten. Kan jag in korrekt för att använda villkorlig åtkomst?**</span><span class="sxs-lookup"><span data-stu-id="af37e-145">**Q: I see hello device record under hello USER info in hello Azure portal and can see hello state as registered on hello client. Am I setup correctly for using conditional access?**</span></span>

<span data-ttu-id="af37e-146">**S:** hello enhetspost (deviceID) och tillstånd på hello Azure-portalen måste matcha hello klienten och utvärdering kriterier för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="af37e-146">**A:** hello device record (deviceID) and state on hello Azure portal must match hello client and meet any evaluation criteria for conditional access.</span></span> <span data-ttu-id="af37e-147">Mer information finns i [Kom igång med Azure Active Directory Device Registration](active-directory-device-registration.md).</span><span class="sxs-lookup"><span data-stu-id="af37e-147">For more details, see [Get started with Azure Active Directory Device Registration](active-directory-device-registration.md).</span></span>

---

<span data-ttu-id="af37e-148">**F: Varför visas meddelandet ”användarnamnet eller lösenordet är felaktigt” för en enhet har jag bara ansluten tooAzure AD?**</span><span class="sxs-lookup"><span data-stu-id="af37e-148">**Q: Why do I get a "username or password is incorrect" message for a device I have just joined tooAzure AD?**</span></span>

<span data-ttu-id="af37e-149">**S:** vanliga orsaker till det här scenariot är:</span><span class="sxs-lookup"><span data-stu-id="af37e-149">**A:** Common reasons for this scenario are:</span></span>

- <span data-ttu-id="af37e-150">Användarens autentiseringsuppgifter är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="af37e-150">Your user credentials are no longer valid.</span></span>

- <span data-ttu-id="af37e-151">Datorn är toocommunicate med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="af37e-151">Your computer is unable toocommunicate with Azure Active Directory.</span></span> <span data-ttu-id="af37e-152">Sök efter eventuella problem med nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="af37e-152">Check for any network connectivity issues.</span></span>

- <span data-ttu-id="af37e-153">hello Azure AD Join förutsättningar har inte uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="af37e-153">hello Azure AD Join pre-requisites were not met.</span></span> <span data-ttu-id="af37e-154">Se till att du har följt hello steg för [utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="af37e-154">Please ensure that you have followed hello steps for [Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span>  

- <span data-ttu-id="af37e-155">Federerad inloggning kräver din federation server toosupport en aktiv WS-Trust-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="af37e-155">Federated logins requires your federation server toosupport a WS-Trust active endpoint.</span></span> 

---

<span data-ttu-id="af37e-156">**F: Varför visas hello ”Oops... Det uppstod ett fel”! dialogrutan när jag försöker Anslut min dator?**</span><span class="sxs-lookup"><span data-stu-id="af37e-156">**Q: Why do I see hello “Oops… an error occurred!" dialog when I try do join my PC?**</span></span>

<span data-ttu-id="af37e-157">**S:** detta beror på hur du konfigurerar Azure Active Directory-registrering med Intune.</span><span class="sxs-lookup"><span data-stu-id="af37e-157">**A:** This is a result of setting up Azure Active Directory enrollment with Intune.</span></span> <span data-ttu-id="af37e-158">Mer information finns i [konfigurera enhetshantering för Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span><span class="sxs-lookup"><span data-stu-id="af37e-158">For more details, see [Set up Windows device management](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span></span>  

---

<span data-ttu-id="af37e-159">**F: Varför har min försök toojoin en dator misslyckas även om jag inte får någon information om fel?**</span><span class="sxs-lookup"><span data-stu-id="af37e-159">**Q: Why did my attempt toojoin a PC fail although I didn't get any error information?**</span></span>

<span data-ttu-id="af37e-160">**S:** en trolig orsak är att hello användaren är inloggad toohello enhet med hjälp av hello inbyggda administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="af37e-160">**A:** A likely cause is that hello user is logged in toohello device using hello built-in administrator account.</span></span> <span data-ttu-id="af37e-161">Skapa ett annat lokalt konto innan du använder Azure Active Directory Join toocomplete hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="af37e-161">Please create a different local account before using Azure Active Directory Join toocomplete hello setup.</span></span> 

---

<span data-ttu-id="af37e-162">**F: var kan jag hitta instruktioner för hello installationsprogrammet för automatisk enhetsregistrering?**</span><span class="sxs-lookup"><span data-stu-id="af37e-162">**Q: Where can I find instructions for hello setup of automatic device registration?**</span></span>

<span data-ttu-id="af37e-163">**S:** detaljerade instruktioner finns [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="af37e-163">**A:** For detailed instructions, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

---

<span data-ttu-id="af37e-164">**F: var kan jag hitta felsökning information om automatisk enhetsregistrering för hello?**</span><span class="sxs-lookup"><span data-stu-id="af37e-164">**Q: Where can I find troubleshooting information about hello automatic device registration?**</span></span>

<span data-ttu-id="af37e-165">**S:** information om felsökning, se:</span><span class="sxs-lookup"><span data-stu-id="af37e-165">**A:** For troubleshooting information, see:</span></span>

- [<span data-ttu-id="af37e-166">Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="af37e-166">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)

- [<span data-ttu-id="af37e-167">Felsökning av automatisk registrering av domän anslutna datorer tooAzure AD för äldre Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="af37e-167">Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

