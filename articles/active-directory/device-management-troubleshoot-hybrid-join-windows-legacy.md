---
title: "Felsökning av hybrid Azure Active Directory-anslutna enheter äldre | Microsoft Docs"
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
ms.openlocfilehash: 715fca79e488ae3759926181c244a42026f4a554
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a><span data-ttu-id="2ed29-103">Felsökning av hybrid Azure Active Directory-anslutna äldre enheter</span><span class="sxs-lookup"><span data-stu-id="2ed29-103">Troubleshooting hybrid Azure Active Directory joined down-level devices</span></span> 

<span data-ttu-id="2ed29-104">Det här avsnittet gäller endast för följande enheter:</span><span class="sxs-lookup"><span data-stu-id="2ed29-104">This topic is applicable only to the following devices:</span></span> 

- <span data-ttu-id="2ed29-105">Windows 7</span><span class="sxs-lookup"><span data-stu-id="2ed29-105">Windows 7</span></span> 
- <span data-ttu-id="2ed29-106">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="2ed29-106">Windows 8.1</span></span> 
- <span data-ttu-id="2ed29-107">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="2ed29-107">Windows Server 2008 R2</span></span> 
- <span data-ttu-id="2ed29-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="2ed29-108">Windows Server 2012</span></span> 
- <span data-ttu-id="2ed29-109">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="2ed29-109">Windows Server 2012 R2</span></span> 
 

<span data-ttu-id="2ed29-110">Windows 10 eller Windows Server 2016 finns [felsökning hybrid Azure Active Directory-anslutna enheter för Windows 10 och Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).</span><span class="sxs-lookup"><span data-stu-id="2ed29-110">For Windows 10 or Windows Server 2016, see [Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices](device-management-troubleshoot-hybrid-join-windows-current.md).</span></span>

<span data-ttu-id="2ed29-111">Det här avsnittet förutsätter att du har [konfigurerade hybrid Azure Active Directory-anslutna enheter](device-management-hybrid-azuread-joined-devices-setup.md) till stöd för följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="2ed29-111">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) to support the following scenarios:</span></span>

- <span data-ttu-id="2ed29-112">Enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="2ed29-112">Device-based conditional access</span></span>

- [<span data-ttu-id="2ed29-113">Enterprise centrala inställningar</span><span class="sxs-lookup"><span data-stu-id="2ed29-113">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="2ed29-114">Windows Hello för företag</span><span class="sxs-lookup"><span data-stu-id="2ed29-114">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md) 





<span data-ttu-id="2ed29-115">Det här avsnittet ger dig felsökningsanvisningar om hur du löser problem.</span><span class="sxs-lookup"><span data-stu-id="2ed29-115">This topic provides you with troubleshooting guidance on how to resolve potential issues.</span></span>  

<span data-ttu-id="2ed29-116">**Vad du bör känna till:**</span><span class="sxs-lookup"><span data-stu-id="2ed29-116">**What you should know:**</span></span> 

- <span data-ttu-id="2ed29-117">Det maximala antalet enheter per användare är enhetscentrerad.</span><span class="sxs-lookup"><span data-stu-id="2ed29-117">The maximum number of devices per user is device-centric.</span></span> <span data-ttu-id="2ed29-118">Till exempel om *jdoe* och *jharnett* logga in på en enhet, en separat registrering (DeviceID) skapas för var och en av dem i den **användaren** fliken information.</span><span class="sxs-lookup"><span data-stu-id="2ed29-118">For example, if *jdoe* and *jharnett* sign-in to a device, a separate registration (DeviceID) is created for each of them in the **USER** info tab.</span></span>  

- <span data-ttu-id="2ed29-119">Registreringen / koppling för enheter som har konfigurerats för att utföra ett försök vid inloggning eller Lås / Lås upp.</span><span class="sxs-lookup"><span data-stu-id="2ed29-119">The initial registration / join of devices is configured to perform an attempt at either logon or lock / unlock.</span></span> <span data-ttu-id="2ed29-120">Det dröja 5 minuter innan utlöses av en uppgift i Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="2ed29-120">There could be 5-minute delay triggered by a task scheduler task.</span></span> 

- <span data-ttu-id="2ed29-121">En ominstallation av operativsystemet eller en manuell unregister och registrera kan skapa en ny registrering på Azure AD och resulterar i flera poster information på fliken användare i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ed29-121">A reinstall of the operating system or a manual unregister and re-register may create a new registration on Azure AD and results in multiple entries under the USER info tab in the Azure portal.</span></span> 


## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="2ed29-122">Steg 1: Hämta registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="2ed29-122">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="2ed29-123">**Kontrollera registreringsstatus:**</span><span class="sxs-lookup"><span data-stu-id="2ed29-123">**To verify the registration status:**</span></span>  

1. <span data-ttu-id="2ed29-124">Öppna Kommandotolken som administratör</span><span class="sxs-lookup"><span data-stu-id="2ed29-124">Open the command prompt as an administrator</span></span> 

2. <span data-ttu-id="2ed29-125">Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span><span class="sxs-lookup"><span data-stu-id="2ed29-125">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`</span></span>

<span data-ttu-id="2ed29-126">Detta kommando visar en dialogruta där du får mer information om status för anslutning till.</span><span class="sxs-lookup"><span data-stu-id="2ed29-126">This command displays a dialog box that provides you with more details about the join status.</span></span>

![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a><span data-ttu-id="2ed29-128">Steg 2: Utvärdera hybrid Azure AD join-status</span><span class="sxs-lookup"><span data-stu-id="2ed29-128">Step 2: Evaluate the hybrid Azure AD join status</span></span> 

<span data-ttu-id="2ed29-129">Om inte Azure AD-anslutning hybrid lyckades ger dialogrutan dig information om problem som har uppstått.</span><span class="sxs-lookup"><span data-stu-id="2ed29-129">If the hybrid Azure AD join was not successful, the dialog box provides you with details about the issue that has occurred.</span></span>

<span data-ttu-id="2ed29-130">**De flesta vanliga problem är:**</span><span class="sxs-lookup"><span data-stu-id="2ed29-130">**The most common issues are:**</span></span>

- <span data-ttu-id="2ed29-131">En felkonfigurerad AD FS eller Azure AD</span><span class="sxs-lookup"><span data-stu-id="2ed29-131">A misconfigured AD FS or Azure AD</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- <span data-ttu-id="2ed29-133">Du är inte inloggad som domänanvändare</span><span class="sxs-lookup"><span data-stu-id="2ed29-133">You are not signed on as a domain user</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- <span data-ttu-id="2ed29-135">En kvot har uppnåtts</span><span class="sxs-lookup"><span data-stu-id="2ed29-135">A quota has been reached</span></span>

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- <span data-ttu-id="2ed29-137">Tjänsten svarar inte</span><span class="sxs-lookup"><span data-stu-id="2ed29-137">The service is not responding</span></span> 

    ![Anslut till arbetsplatsen för Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

<span data-ttu-id="2ed29-139">Du kan också hitta statusinformation i Loggboken under **program och tjänster Log\Microsoft-Arbetsplatsanslutning**.</span><span class="sxs-lookup"><span data-stu-id="2ed29-139">You can also find the status information in the event log under **Applications and Services Log\Microsoft-Workplace Join**.</span></span>
  
<span data-ttu-id="2ed29-140">**De vanligaste orsakerna till en misslyckad hybrid Azure AD-koppling är:**</span><span class="sxs-lookup"><span data-stu-id="2ed29-140">**The most common causes for a failed hybrid Azure AD join are:**</span></span> 

- <span data-ttu-id="2ed29-141">Datorn finns inte på det interna nätverket eller en VPN-anslutning utan anslutning till en lokal AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="2ed29-141">Your computer is not on the organization’s internal network or a VPN without connection to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="2ed29-142">Du är inloggad på datorn med ett lokalt datorkonto.</span><span class="sxs-lookup"><span data-stu-id="2ed29-142">You are logged on to your computer with a local computer account.</span></span> 

- <span data-ttu-id="2ed29-143">Konfigurationsproblem för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="2ed29-143">Service configuration issues:</span></span> 

  - <span data-ttu-id="2ed29-144">Federationsservern har konfigurerats för att stödja **WIAORMULTIAUTHN**.</span><span class="sxs-lookup"><span data-stu-id="2ed29-144">The federation server has been configured to support **WIAORMULTIAUTHN**.</span></span> 

  - <span data-ttu-id="2ed29-145">Det finns inget tjänstanslutningspunkten-objekt som pekar på ett verifierat domännamn i Azure AD i AD-skog där datorn tillhör.</span><span class="sxs-lookup"><span data-stu-id="2ed29-145">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to.</span></span>

  - <span data-ttu-id="2ed29-146">En användare har nått gränsen på enheter.</span><span class="sxs-lookup"><span data-stu-id="2ed29-146">A user has reached the limit of devices.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2ed29-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ed29-147">Next steps</span></span>

<span data-ttu-id="2ed29-148">Frågor, finns det [enhetshantering vanliga frågor och svar](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="2ed29-148">For questions, see the [device management FAQ](device-management-faq.md)</span></span>  
