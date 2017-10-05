---
title: "Felsökning av automatisk registrering av Azure AD-domän domänanslutna datorer för klienter med äldre Windows | Microsoft Docs"
description: "Felsökning av automatisk registrering av Azure AD-domän domänanslutna datorer för äldre Windows-klienter."
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
ms.openlocfilehash: a7c8ef4c59c53c21258f0c61963d8f994a3946ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad-for-windows-down-level-clients"></a><span data-ttu-id="a8ed1-103">Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD för äldre Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="a8ed1-103">Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients</span></span> 

<span data-ttu-id="a8ed1-104">Det här avsnittet gäller endast för följande klienter:</span><span class="sxs-lookup"><span data-stu-id="a8ed1-104">This topic is applicable only to the following clients:</span></span> 

- <span data-ttu-id="a8ed1-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="a8ed1-105">Windows 7</span></span> 
- <span data-ttu-id="a8ed1-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="a8ed1-106">Windows 8.1</span></span> 
- <span data-ttu-id="a8ed1-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="a8ed1-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="a8ed1-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="a8ed1-108">Windows Server 2012</span></span> 
- <span data-ttu-id="a8ed1-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="a8ed1-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="a8ed1-110">Windows 10 eller Windows Server 2016 finns [felsökning automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10 och Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span><span class="sxs-lookup"><span data-stu-id="a8ed1-110">For Windows 10 or Windows Server 2016, see [Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span></span>

<span data-ttu-id="a8ed1-111">Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a8ed1-111">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md).</span></span>
 
<span data-ttu-id="a8ed1-112">Det här avsnittet ger dig felsökningsanvisningar om hur du löser problem.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-112">This topic provides you with troubleshooting guidance on how to resolve potential issues.</span></span>  
<span data-ttu-id="a8ed1-113">Några saker att Observera för lyckade resultat:</span><span class="sxs-lookup"><span data-stu-id="a8ed1-113">Some things to note for successful outcomes:</span></span> 

- <span data-ttu-id="a8ed1-114">Registrering av dessa klienter på Azure AD är per användare/enhet.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-114">Registration of these clients on Azure AD is per user/device.</span></span> <span data-ttu-id="a8ed1-115">Exempel: om jdoe och jharnett loggar du in på den här enheten, skapas en separat registrering (DeviceID) för dessa användare på fliken info för användaren.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-115">As an example: If jdoe and jharnett log in to this device, a separate registration (DeviceID) is created for each of these users in the USER info tab.</span></span>  

- <span data-ttu-id="a8ed1-116">Registrering av dessa klienter direkt är konfigurerad för att försöka vid inloggning eller låsa/låsa upp och det dröja 5 minuter innan detta blir utlöst med hjälp av en uppgift i Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-116">Registration of these clients out of the box is configured to try at either logon or lock/unlock and there could be 5-minute delay that this is triggered using a Task Scheduler task.</span></span> 

- <span data-ttu-id="a8ed1-117">Installera om operativsystemet eller en manuell avregistrera och registrera kan skapa en ny registrering på Azure AD och leder till flera poster information på fliken användare i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-117">A re-install of the operating system or a manual un-register and re-register may create a new registration on Azure AD and will result in multiple entries under the USER info tab in the Azure portal.</span></span> 


## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="a8ed1-118">Steg 1: Hämta registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="a8ed1-118">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="a8ed1-119">**Kontrollera registreringsstatus:**</span><span class="sxs-lookup"><span data-stu-id="a8ed1-119">**To verify the registration status:**</span></span>  

1. <span data-ttu-id="a8ed1-120">Öppna Kommandotolken som administratör</span><span class="sxs-lookup"><span data-stu-id="a8ed1-120">Open the command prompt as an administrator</span></span> 

2. <span data-ttu-id="a8ed1-121">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="a8ed1-121">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="a8ed1-122">Detta kommando visar en dialogruta där du får mer information om status för anslutning till.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-122">This command displays a dialog box that provides you with more details about the join status.</span></span>

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-registration-status"></a><span data-ttu-id="a8ed1-124">Steg 2: Utvärdera registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="a8ed1-124">Step 2: Evaluate the registration status</span></span> 

<span data-ttu-id="a8ed1-125">Om inte kopplingen lyckades ger dialogrutan dig information om problem som har uppstått.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-125">If the join was not successful, the dialog box provides you with details about the issue that has occured.</span></span>

<span data-ttu-id="a8ed1-126">**De flesta vanliga problem är:**</span><span class="sxs-lookup"><span data-stu-id="a8ed1-126">**The most common issues are:**</span></span>

- <span data-ttu-id="a8ed1-127">En felkonfigurerad AD FS eller Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8ed1-127">A misconfigured AD FS or Azure AD</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="a8ed1-129">Du är inte inloggad som domänanvändare</span><span class="sxs-lookup"><span data-stu-id="a8ed1-129">You are not signed on as a domain user</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="a8ed1-131">En kvot har uppnåtts</span><span class="sxs-lookup"><span data-stu-id="a8ed1-131">A quota has been reached</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="a8ed1-133">Tjänsten svarar inte</span><span class="sxs-lookup"><span data-stu-id="a8ed1-133">The service is not responding</span></span> 

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="a8ed1-135">Du kan också hitta statusinformation i Loggboken under **program och tjänster Log\Microsoft-Arbetsplatsanslutning**.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-135">You can also find the status information in the event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="a8ed1-136">**De vanligaste orsakerna till misslyckade registrering är:**</span><span class="sxs-lookup"><span data-stu-id="a8ed1-136">**The most common causes for a failed registration are:**</span></span> 

- <span data-ttu-id="a8ed1-137">Datorn finns inte på det interna nätverket eller en VPN-anslutning utan anslutning till en lokal AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-137">Your computer is not on the organization’s internal network or a VPN without connection to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="a8ed1-138">Du är inloggad på datorn med ett lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-138">You are logged on to your computer with a local computer account.</span></span> 

- <span data-ttu-id="a8ed1-139">Konfigurationsproblem för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a8ed1-139">Service configuration issues:</span></span> 

  - <span data-ttu-id="a8ed1-140">Federationsservern har konfigurerats för att stödja **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-140">The federation server has been configured to support **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="a8ed1-141">Det finns inget tjänstanslutningspunkten-objekt som pekar på ett verifierat domännamn i Azure AD i AD-skog där datorn tillhör.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-141">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to.</span></span>

  - <span data-ttu-id="a8ed1-142">En användare har nått gränsen på enheter.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-142">A user has reached the limit of devices.</span></span> <span data-ttu-id="a8ed1-143">Se till att komma igång med Azure Active Directory Device Registration.</span><span class="sxs-lookup"><span data-stu-id="a8ed1-143">Please see Get Started with Azure Active Directory Device Registration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8ed1-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8ed1-144">Next steps</span></span>

<span data-ttu-id="a8ed1-145">Mer information finns i [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="a8ed1-145">For more information, see the [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
