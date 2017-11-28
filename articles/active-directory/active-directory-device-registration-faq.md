---
title: "Azure Active Directory automatisk enhetsregistrering vanliga frågor och svar | Microsoft Docs"
description: "Automatisk enhetsregistrering med Azure Active Directory vanliga frågor och svar."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 29751239ae2a26cd7b07ddd0d8a8e706d4056b68
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-automatic-device-registration-faq"></a><span data-ttu-id="3a4c8-103">Vanliga frågor och svar om automatisk enhetsregistrering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3a4c8-103">Azure Active Directory automatic device registration FAQ</span></span>

<span data-ttu-id="3a4c8-104">**F: jag registrerade nyligen enheten. Varför ser inte enheten under Mina användarinformation i Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-104">**Q: I registered the device recently. Why can’t I see the device under my user info in the Azure portal?**</span></span>

<span data-ttu-id="3a4c8-105">**S:** Windows 10-enheter som är ansluten till domänen med automatisk enhetsregistrering visas inte under användarinformation.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-105">**A:** Windows 10 devices that are domain-joined with automatic device registration do not show up under the USER info.</span></span>
<span data-ttu-id="3a4c8-106">Du måste använda PowerShell för att se alla enheter.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-106">You need to use PowerShell to see all devices.</span></span> 

<span data-ttu-id="3a4c8-107">Följande enheter visas under informationen som användaren:</span><span class="sxs-lookup"><span data-stu-id="3a4c8-107">Only the following devices are listed under the USER info:</span></span>

- <span data-ttu-id="3a4c8-108">Alla personliga enheter som inte är ansluten enterprise</span><span class="sxs-lookup"><span data-stu-id="3a4c8-108">All personal devices that are not enterprise joined</span></span> 
- <span data-ttu-id="3a4c8-109">Alla icke - Windows 10 / Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3a4c8-109">All non-Windows 10 / Windows Server 2016</span></span> 
- <span data-ttu-id="3a4c8-110">Alla Windows-enheter</span><span class="sxs-lookup"><span data-stu-id="3a4c8-110">All non-Windows devices</span></span> 

---

<span data-ttu-id="3a4c8-111">**F: Varför kan jag inte se alla enheter som har registrerats i Azure Active Directory i Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-111">**Q: Why can I not see all the devices registered in Azure Active Directory in the Azure portal?**</span></span> 

<span data-ttu-id="3a4c8-112">**S:** för närvarande, det går inte att visa alla registrerade enheter i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-112">**A:** Currently, there is no way to see all registered devices in the Azure portal.</span></span> <span data-ttu-id="3a4c8-113">Du kan använda Azure PowerShell för att hitta alla enheter.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-113">You can use Azure PowerShell to find all devices.</span></span> <span data-ttu-id="3a4c8-114">Mer information finns i [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-114">For more details, see the [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span></span>

--- 

<span data-ttu-id="3a4c8-115">**F: hur vet jag Enhetsstatus för registrering av klienten är?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-115">**Q: How do I know what the device registration state of the client is?**</span></span>

<span data-ttu-id="3a4c8-116">**S:** registreringstillstånd enhet beror på:</span><span class="sxs-lookup"><span data-stu-id="3a4c8-116">**A:** The device registration state depends on:</span></span>

- <span data-ttu-id="3a4c8-117">Vad enheten är</span><span class="sxs-lookup"><span data-stu-id="3a4c8-117">What the device is</span></span>
- <span data-ttu-id="3a4c8-118">Hur den har registrerats</span><span class="sxs-lookup"><span data-stu-id="3a4c8-118">How it was registered</span></span> 
- <span data-ttu-id="3a4c8-119">Alla information som rör den.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-119">Any details related to it.</span></span> 
 

---

<span data-ttu-id="3a4c8-120">**F: Varför är en enhet som jag har tagits bort i Azure-portalen eller med Windows PowerShell fortfarande listad som har registrerats?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-120">**Q: Why is a device I have deleted in the Azure portal or using Windows PowerShell still listed as registered?**</span></span>

<span data-ttu-id="3a4c8-121">**S:** detta är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-121">**A:** This is by design.</span></span> <span data-ttu-id="3a4c8-122">Enheten har inte åtkomst till resurser i molnet.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-122">The device will not have access to resources in the cloud.</span></span> <span data-ttu-id="3a4c8-123">Om du vill ta bort enheten och registrera igen måste vara en manuell åtgärd som ska vidtas på enheten.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-123">If you want to remove the device and register again, a manual action must be to be taken on the device.</span></span> 

<span data-ttu-id="3a4c8-124">För Windows 10 och Windows Server 2016 som är lokala AD-domänansluten:</span><span class="sxs-lookup"><span data-stu-id="3a4c8-124">For Windows 10 and Windows Server 2016 that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="3a4c8-125">Öppna Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-125">Open the command prompt as an administrator.</span></span>

2.  <span data-ttu-id="3a4c8-126">Typ`dsregcmd.exe /debug /leave`</span><span class="sxs-lookup"><span data-stu-id="3a4c8-126">Type `dsregcmd.exe /debug /leave`</span></span>

3.  <span data-ttu-id="3a4c8-127">Logga ut och logga in att utlösa den schemalagda aktiviteten som registrerar enheten igen.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-127">Sign out and sign in to trigger the scheduled task that registers the device again.</span></span> 

<span data-ttu-id="3a4c8-128">För andra Windows-plattformar som är lokala AD-domänansluten:</span><span class="sxs-lookup"><span data-stu-id="3a4c8-128">For other Windows platforms that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="3a4c8-129">Öppna Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-129">Open the command prompt as an administrator.</span></span>
2.  <span data-ttu-id="3a4c8-130">Skriv `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-130">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span></span>
3.  <span data-ttu-id="3a4c8-131">Skriv `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-131">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span></span>

---

<span data-ttu-id="3a4c8-132">**F: Varför visas dubblettenhet poster i Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-132">**Q: Why do I see duplicate device entries in Azure portal?**</span></span>

<span data-ttu-id="3a4c8-133">**S:**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-133">**A:**</span></span>

-   <span data-ttu-id="3a4c8-134">För Windows 10 och Windows Server 2016, om de är upprepade försök att koppla från och ansluta igen samma enhet, kan det finnas dubbla poster.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-134">For Windows 10 and Windows Server 2016, if they are repeated attempts to unjoin and re-join the same device, there might be duplicate entries.</span></span> 

-   <span data-ttu-id="3a4c8-135">Om du har använt Lägg till arbets- eller Skolkonto, skapas en ny enhetspost med samma enhetsnamn varje windowsanvändare som använder Lägg till arbets- eller Skolkonto.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-135">If you have used Add Work or School Account, each windows user who uses Add Work or School Account will create a new device record with the same device name.</span></span>

-   <span data-ttu-id="3a4c8-136">Andra Windows-plattformar som är lokala AD som ingår i domänen med hjälp av automatisk registrering skapar en ny enhetspost med samma enhetsnamn för varje domänanvändare som loggar in på enheten.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-136">Other Windows platforms that are on-premises AD domain-joined using automatic registration will create a new device record with the same device name for each domain user who logs into the device.</span></span> 

-   <span data-ttu-id="3a4c8-137">En AADJ-dator som har rensats, installeras igen och ansluten igen med samma namn kommer att visas som en annan post med samma enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-137">An AADJ machine that has been wiped, re-installed and re-joined with the same name, will show up as another record with the same device name.</span></span>

---

<span data-ttu-id="3a4c8-138">**F: Varför kan en användare fortfarande komma åt resurser från en enhet som jag har inaktiverats i Azure portal?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-138">**Q: Why can a user still access resources from a device I have disabled in the Azure portal?**</span></span>

<span data-ttu-id="3a4c8-139">**S:** det kan ta upp till en timme innan en återkalla som ska användas.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-139">**A:** It can take up to an hour for a revoke to be applied.</span></span>

>[!Note] 
><span data-ttu-id="3a4c8-140">För förlorade enheter rekommenderar vi att rensa enheten så att användare inte tillgång till enheten.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-140">For lost devices, we recommend wiping the device to ensure that users cannot access the device.</span></span> <span data-ttu-id="3a4c8-141">Mer information finns i [registrera enheter för hantering i Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="3a4c8-141">For more details, see [Enroll devices for management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span></span> 


---

<span data-ttu-id="3a4c8-142">**F: Varför Mina användare ser inte ”du kan hämta det här”?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-142">**Q: Why do my users see “You can’t get there from here”?**</span></span>

<span data-ttu-id="3a4c8-143">**S:** om du har konfigurerat vissa regler för villkorlig åtkomst för att kräva tillstånd för en specifik enhet och enheten inte uppfyller kriterierna som användare blockeras och detta meddelande.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-143">**A:** If you have configured certain conditional access rules to require a specific device state and the device does not meet the criteria, users are blocked and see this message.</span></span> <span data-ttu-id="3a4c8-144">Utvärdera reglerna och se till att enheten kan uppfyller kriterierna för att undvika det här meddelandet.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-144">Please evaluate the rules and ensure that the device is able to meet the criteria to avoid this message.</span></span>

---


<span data-ttu-id="3a4c8-145">**F: jag finns enheten under användarinformation i Azure-portalen och kan se status som har registrerats på klienten. Kan jag in korrekt för att använda villkorlig åtkomst?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-145">**Q: I see the device record under the USER info in the Azure portal and can see the state as registered on the client. Am I setup correctly for using conditional access?**</span></span>

<span data-ttu-id="3a4c8-146">**S:** enhetspost (deviceID) och tillståndet på Azure-portalen måste matcha klienten och utvärdering kriterier för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-146">**A:** The device record (deviceID) and state on the Azure portal must match the client and meet any evaluation criteria for conditional access.</span></span> <span data-ttu-id="3a4c8-147">Mer information finns i [Kom igång med Azure Active Directory Device Registration](active-directory-device-registration.md).</span><span class="sxs-lookup"><span data-stu-id="3a4c8-147">For more details, see [Get started with Azure Active Directory Device Registration](active-directory-device-registration.md).</span></span>

---

<span data-ttu-id="3a4c8-148">**F: Varför visas meddelandet ”användarnamnet eller lösenordet är felaktigt” för en enhet som jag bara har anslutit till Azure AD?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-148">**Q: Why do I get a "username or password is incorrect" message for a device I have just joined to Azure AD?**</span></span>

<span data-ttu-id="3a4c8-149">**S:** vanliga orsaker till det här scenariot är:</span><span class="sxs-lookup"><span data-stu-id="3a4c8-149">**A:** Common reasons for this scenario are:</span></span>

- <span data-ttu-id="3a4c8-150">Användarens autentiseringsuppgifter är inte längre giltig.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-150">Your user credentials are no longer valid.</span></span>

- <span data-ttu-id="3a4c8-151">Datorn kan inte kommunicera med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-151">Your computer is unable to communicate with Azure Active Directory.</span></span> <span data-ttu-id="3a4c8-152">Sök efter eventuella problem med nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-152">Check for any network connectivity issues.</span></span>

- <span data-ttu-id="3a4c8-153">Azure AD Join kraven har inte uppfyllts.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-153">The Azure AD Join pre-requisites were not met.</span></span> <span data-ttu-id="3a4c8-154">Se till att du har följt stegen för att [utöka molnfunktioner till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3a4c8-154">Please ensure that you have followed the steps for [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span>  

- <span data-ttu-id="3a4c8-155">Federerad inloggningar kräver federationsservern för att stödja en aktiv WS-Trust-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-155">Federated logins requires your federation server to support a WS-Trust active endpoint.</span></span> 

---

<span data-ttu-id="3a4c8-156">**F: Varför visas den ”Oops... Det uppstod ett fel”! dialogrutan när jag försöker Anslut min dator?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-156">**Q: Why do I see the “Oops… an error occurred!" dialog when I try do join my PC?**</span></span>

<span data-ttu-id="3a4c8-157">**S:** detta beror på hur du konfigurerar Azure Active Directory-registrering med Intune.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-157">**A:** This is a result of setting up Azure Active Directory enrollment with Intune.</span></span> <span data-ttu-id="3a4c8-158">Mer information finns i [konfigurera enhetshantering för Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span><span class="sxs-lookup"><span data-stu-id="3a4c8-158">For more details, see [Set up Windows device management](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span></span>  

---

<span data-ttu-id="3a4c8-159">**F: varför min försök att ansluta en dator inte även om jag inte får någon information om fel?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-159">**Q: Why did my attempt to join a PC fail although I didn't get any error information?**</span></span>

<span data-ttu-id="3a4c8-160">**S:** en trolig orsak är att användaren är inloggad på enheten med det inbyggda administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-160">**A:** A likely cause is that the user is logged in to the device using the built-in administrator account.</span></span> <span data-ttu-id="3a4c8-161">Skapa ett annat lokalt konto innan du använder Azure Active Directory Join för att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="3a4c8-161">Please create a different local account before using Azure Active Directory Join to complete the setup.</span></span> 

---

<span data-ttu-id="3a4c8-162">**F: var kan jag hitta instruktioner för installationen av automatisk enhetsregistrering?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-162">**Q: Where can I find instructions for the setup of automatic device registration?**</span></span>

<span data-ttu-id="3a4c8-163">**S:** detaljerade instruktioner finns [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="3a4c8-163">**A:** For detailed instructions, see [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

---

<span data-ttu-id="3a4c8-164">**F: var kan jag hitta felsökning information om automatisk enhetsregistrering?**</span><span class="sxs-lookup"><span data-stu-id="3a4c8-164">**Q: Where can I find troubleshooting information about the automatic device registration?**</span></span>

<span data-ttu-id="3a4c8-165">**S:** information om felsökning, se:</span><span class="sxs-lookup"><span data-stu-id="3a4c8-165">**A:** For troubleshooting information, see:</span></span>

- [<span data-ttu-id="3a4c8-166">Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="3a4c8-166">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)

- [<span data-ttu-id="3a4c8-167">Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD för äldre Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="3a4c8-167">Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

