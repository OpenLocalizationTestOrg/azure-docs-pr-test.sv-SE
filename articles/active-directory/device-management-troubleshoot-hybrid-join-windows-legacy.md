---
title: "aaaTroubleshooting hybrid Azure Active Directory-anslutna enheter för äldre | Microsoft Docs"
description: "Felsökning av hybrid Azure Active Directory-anslutna äldre enheter."
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
ms.openlocfilehash: edd56b89579fac6b427732902284ad9c568b87b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a><span data-ttu-id="c8e3a-103">Felsökning av hybrid Azure Active Directory-anslutna äldre enheter</span><span class="sxs-lookup"><span data-stu-id="c8e3a-103">Troubleshooting hybrid Azure Active Directory joined down-level devices</span></span> 

<span data-ttu-id="c8e3a-104">Det här avsnittet är tillämpliga endast toohello följande enheter:</span><span class="sxs-lookup"><span data-stu-id="c8e3a-104">This topic is applicable only toohello following devices:</span></span> 

- <span data-ttu-id="c8e3a-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="c8e3a-105">Windows 7</span></span> 
- <span data-ttu-id="c8e3a-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="c8e3a-106">Windows 8.1</span></span> 
- <span data-ttu-id="c8e3a-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="c8e3a-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="c8e3a-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="c8e3a-108">Windows Server 2012</span></span> 
- <span data-ttu-id="c8e3a-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c8e3a-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="c8e3a-110">Windows 10 eller Windows Server 2016 finns [felsökning hybrid Azure Active Directory-anslutna enheter för Windows 10 och Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).</span><span class="sxs-lookup"><span data-stu-id="c8e3a-110">For Windows 10 or Windows Server 2016, see [Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices](device-management-troubleshoot-hybrid-join-windows-current.md).</span></span>

<span data-ttu-id="c8e3a-111">Det här avsnittet förutsätter att du har [konfigurerade hybrid Azure Active Directory-anslutna enheter](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="c8e3a-111">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello following scenarios:</span></span>

- <span data-ttu-id="c8e3a-112">Enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="c8e3a-112">Device-based conditional access</span></span>

- [<span data-ttu-id="c8e3a-113">Enterprise centrala inställningar</span><span class="sxs-lookup"><span data-stu-id="c8e3a-113">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="c8e3a-114">Windows Hello för företag</span><span class="sxs-lookup"><span data-stu-id="c8e3a-114">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md) 





<span data-ttu-id="c8e3a-115">Det här avsnittet ger dig felsökningsanvisningar på hur tooresolve potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-115">This topic provides you with troubleshooting guidance on how tooresolve potential issues.</span></span>  

<span data-ttu-id="c8e3a-116">**Vad du bör känna till:**</span><span class="sxs-lookup"><span data-stu-id="c8e3a-116">**What you should know:**</span></span> 

- <span data-ttu-id="c8e3a-117">hello högsta antalet enheter per användare är enhetscentrerad.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-117">hello maximum number of devices per user is device-centric.</span></span> <span data-ttu-id="c8e3a-118">Till exempel om *jdoe* och *jharnett* inloggning tooa enhet, skapas en separat registrering (DeviceID) för var och en av dem i hello **användaren** fliken information.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-118">For example, if *jdoe* and *jharnett* sign-in tooa device, a separate registration (DeviceID) is created for each of them in hello **USER** info tab.</span></span>  

- <span data-ttu-id="c8e3a-119">Hej inledande registrering / join av enheter som är konfigurerade tooperform ett försök vid inloggning eller Lås / Lås upp.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-119">hello initial registration / join of devices is configured tooperform an attempt at either logon or lock / unlock.</span></span> <span data-ttu-id="c8e3a-120">Det dröja 5 minuter innan utlöses av en uppgift i Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-120">There could be 5-minute delay triggered by a task scheduler task.</span></span> 

- <span data-ttu-id="c8e3a-121">En ominstallation av hello operativsystem eller en manuell unregister och registrera kan skapa en ny registrering på Azure AD och resulterar i flera poster i i hello användaren information fliken hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-121">A reinstall of hello operating system or a manual unregister and re-register may create a new registration on Azure AD and results in multiple entries under hello USER info tab in hello Azure portal.</span></span> 


## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="c8e3a-122">Steg 1: Hämta hello registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="c8e3a-122">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="c8e3a-123">**tooverify hello registreringsstatus:**</span><span class="sxs-lookup"><span data-stu-id="c8e3a-123">**tooverify hello registration status:**</span></span>  

1. <span data-ttu-id="c8e3a-124">Öppna hello Kommandotolken som administratör</span><span class="sxs-lookup"><span data-stu-id="c8e3a-124">Open hello command prompt as an administrator</span></span> 

2. <span data-ttu-id="c8e3a-125">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="c8e3a-125">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="c8e3a-126">Detta kommando visar en dialogruta där du får mer information om status för hello-koppling.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-126">This command displays a dialog box that provides you with more details about hello join status.</span></span>

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-hello-hybrid-azure-ad-join-status"></a><span data-ttu-id="c8e3a-128">Steg 2: Utvärdera hello hybrid Azure AD join status</span><span class="sxs-lookup"><span data-stu-id="c8e3a-128">Step 2: Evaluate hello hybrid Azure AD join status</span></span> 

<span data-ttu-id="c8e3a-129">Om inte hello hybrid Azure AD join lyckades ger hello dialogrutan dig information om hello problem som har uppstått.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-129">If hello hybrid Azure AD join was not successful, hello dialog box provides you with details about hello issue that has occurred.</span></span>

<span data-ttu-id="c8e3a-130">**hello de flesta vanliga problem är:**</span><span class="sxs-lookup"><span data-stu-id="c8e3a-130">**hello most common issues are:**</span></span>

- <span data-ttu-id="c8e3a-131">En felkonfigurerad AD FS eller Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8e3a-131">A misconfigured AD FS or Azure AD</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="c8e3a-133">Du är inte inloggad som domänanvändare</span><span class="sxs-lookup"><span data-stu-id="c8e3a-133">You are not signed on as a domain user</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="c8e3a-135">En kvot har uppnåtts</span><span class="sxs-lookup"><span data-stu-id="c8e3a-135">A quota has been reached</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="c8e3a-137">hello-tjänsten svarar inte</span><span class="sxs-lookup"><span data-stu-id="c8e3a-137">hello service is not responding</span></span> 

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="c8e3a-139">Du kan också hitta hello statusinformation i hello händelselogg under **program och tjänster Log\Microsoft-Arbetsplatsanslutning**.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-139">You can also find hello status information in hello event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="c8e3a-140">**hello de vanligaste orsakerna till en misslyckad hybrid Azure AD-anslutning är:**</span><span class="sxs-lookup"><span data-stu-id="c8e3a-140">**hello most common causes for a failed hybrid Azure AD join are:**</span></span> 

- <span data-ttu-id="c8e3a-141">Datorn är inte på hello företags interna nätverk eller en VPN-anslutning utan anslutning tooan lokala AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-141">Your computer is not on hello organization’s internal network or a VPN without connection tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="c8e3a-142">Du är inloggad på tooyour dator med ett lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-142">You are logged on tooyour computer with a local computer account.</span></span> 

- <span data-ttu-id="c8e3a-143">Konfigurationsproblem för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="c8e3a-143">Service configuration issues:</span></span> 

  - <span data-ttu-id="c8e3a-144">hello federationsservern har konfigurerats toosupport **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-144">hello federation server has been configured toosupport **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="c8e3a-145">Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-145">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to.</span></span>

  - <span data-ttu-id="c8e3a-146">En användare har nått hello gränsen på enheter.</span><span class="sxs-lookup"><span data-stu-id="c8e3a-146">A user has reached hello limit of devices.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c8e3a-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8e3a-147">Next steps</span></span>

<span data-ttu-id="c8e3a-148">Frågor, finns hello [enhetshantering vanliga frågor och svar](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="c8e3a-148">For questions, see hello [device management FAQ](device-management-faq.md)</span></span>  
