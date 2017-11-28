---
title: "aaaTroubleshooting hello automatisk registrering av Azure AD-domän domänanslutna datorer för klienter med äldre Windows | Microsoft Docs"
description: "Felsöka hello automatisk registrering av Azure AD-domän domänanslutna datorer för äldre Windows-klienter."
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
ms.openlocfilehash: 84fe666576f13de09d1eaa5692517d45a4dbeebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad-for-windows-down-level-clients"></a><span data-ttu-id="baffc-103">Felsökning av automatisk registrering av domän anslutna datorer tooAzure AD för äldre Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="baffc-103">Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients</span></span> 

<span data-ttu-id="baffc-104">Det här avsnittet är tillämpliga endast toohello följande klienter:</span><span class="sxs-lookup"><span data-stu-id="baffc-104">This topic is applicable only toohello following clients:</span></span> 

- <span data-ttu-id="baffc-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="baffc-105">Windows 7</span></span> 
- <span data-ttu-id="baffc-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="baffc-106">Windows 8.1</span></span> 
- <span data-ttu-id="baffc-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="baffc-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="baffc-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="baffc-108">Windows Server 2012</span></span> 
- <span data-ttu-id="baffc-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="baffc-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="baffc-110">Windows 10 eller Windows Server 2016 finns [felsökning automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span><span class="sxs-lookup"><span data-stu-id="baffc-110">For Windows 10 or Windows Server 2016, see [Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md).</span></span>

<span data-ttu-id="baffc-111">Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="baffc-111">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md).</span></span>
 
<span data-ttu-id="baffc-112">Det här avsnittet ger dig felsökningsanvisningar på hur tooresolve potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="baffc-112">This topic provides you with troubleshooting guidance on how tooresolve potential issues.</span></span>  
<span data-ttu-id="baffc-113">Vissa saker toonote för lyckade resultat:</span><span class="sxs-lookup"><span data-stu-id="baffc-113">Some things toonote for successful outcomes:</span></span> 

- <span data-ttu-id="baffc-114">Registrering av dessa klienter på Azure AD är per användare/enhet.</span><span class="sxs-lookup"><span data-stu-id="baffc-114">Registration of these clients on Azure AD is per user/device.</span></span> <span data-ttu-id="baffc-115">Exempel: om jdoe och jharnett loggar du in toothis enhet, skapas en separat registrering (DeviceID) för dessa användare i fliken för hello användaren information.</span><span class="sxs-lookup"><span data-stu-id="baffc-115">As an example: If jdoe and jharnett log in toothis device, a separate registration (DeviceID) is created for each of these users in hello USER info tab.</span></span>  

- <span data-ttu-id="baffc-116">Registrering av dessa klienter out of box hello är konfigurerade tootry vid inloggning eller låsa/låsa upp det dröja 5 minuter innan detta blir utlöst med hjälp av en uppgift i Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="baffc-116">Registration of these clients out of hello box is configured tootry at either logon or lock/unlock and there could be 5-minute delay that this is triggered using a Task Scheduler task.</span></span> 

- <span data-ttu-id="baffc-117">Installera om hello operativsystem eller en manuell avregistrera och registrera kan skapa en ny registrering på Azure AD och leder till att flera poster under fliken hello användaren information i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="baffc-117">A re-install of hello operating system or a manual un-register and re-register may create a new registration on Azure AD and will result in multiple entries under hello USER info tab in hello Azure portal.</span></span> 


## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="baffc-118">Steg 1: Hämta hello registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="baffc-118">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="baffc-119">**tooverify hello registreringsstatus:**</span><span class="sxs-lookup"><span data-stu-id="baffc-119">**tooverify hello registration status:**</span></span>  

1. <span data-ttu-id="baffc-120">Öppna hello Kommandotolken som administratör</span><span class="sxs-lookup"><span data-stu-id="baffc-120">Open hello command prompt as an administrator</span></span> 

2. <span data-ttu-id="baffc-121">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="baffc-121">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="baffc-122">Detta kommando visar en dialogruta där du får mer information om status för hello-koppling.</span><span class="sxs-lookup"><span data-stu-id="baffc-122">This command displays a dialog box that provides you with more details about hello join status.</span></span>

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-registration-status"></a><span data-ttu-id="baffc-124">Steg 2: Utvärdera hello registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="baffc-124">Step 2: Evaluate hello registration status</span></span> 

<span data-ttu-id="baffc-125">Om inte hello koppling lyckades ger hello dialogrutan dig information om hello problem som har uppstått.</span><span class="sxs-lookup"><span data-stu-id="baffc-125">If hello join was not successful, hello dialog box provides you with details about hello issue that has occured.</span></span>

<span data-ttu-id="baffc-126">**hello de flesta vanliga problem är:**</span><span class="sxs-lookup"><span data-stu-id="baffc-126">**hello most common issues are:**</span></span>

- <span data-ttu-id="baffc-127">En felkonfigurerad AD FS eller Azure AD</span><span class="sxs-lookup"><span data-stu-id="baffc-127">A misconfigured AD FS or Azure AD</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="baffc-129">Du är inte inloggad som domänanvändare</span><span class="sxs-lookup"><span data-stu-id="baffc-129">You are not signed on as a domain user</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="baffc-131">En kvot har uppnåtts</span><span class="sxs-lookup"><span data-stu-id="baffc-131">A quota has been reached</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="baffc-133">hello-tjänsten svarar inte</span><span class="sxs-lookup"><span data-stu-id="baffc-133">hello service is not responding</span></span> 

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="baffc-135">Du kan också hitta hello statusinformation i hello händelselogg under **program och tjänster Log\Microsoft-Arbetsplatsanslutning**.</span><span class="sxs-lookup"><span data-stu-id="baffc-135">You can also find hello status information in hello event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="baffc-136">**hello de vanligaste orsakerna till misslyckade registrering är:**</span><span class="sxs-lookup"><span data-stu-id="baffc-136">**hello most common causes for a failed registration are:**</span></span> 

- <span data-ttu-id="baffc-137">Datorn är inte på hello företags interna nätverk eller en VPN-anslutning utan anslutning tooan lokala AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="baffc-137">Your computer is not on hello organization’s internal network or a VPN without connection tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="baffc-138">Du är inloggad på tooyour dator med ett lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="baffc-138">You are logged on tooyour computer with a local computer account.</span></span> 

- <span data-ttu-id="baffc-139">Konfigurationsproblem för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="baffc-139">Service configuration issues:</span></span> 

  - <span data-ttu-id="baffc-140">hello federationsservern har konfigurerats toosupport **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="baffc-140">hello federation server has been configured toosupport **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="baffc-141">Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör.</span><span class="sxs-lookup"><span data-stu-id="baffc-141">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to.</span></span>

  - <span data-ttu-id="baffc-142">En användare har nått hello gränsen på enheter.</span><span class="sxs-lookup"><span data-stu-id="baffc-142">A user has reached hello limit of devices.</span></span> <span data-ttu-id="baffc-143">Se till att komma igång med Azure Active Directory Device Registration.</span><span class="sxs-lookup"><span data-stu-id="baffc-143">Please see Get Started with Azure Active Directory Device Registration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="baffc-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="baffc-144">Next steps</span></span>

<span data-ttu-id="baffc-145">Mer information finns i hello [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="baffc-145">For more information, see hello [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
