---
title: Skydda appar och resurser i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur du spärra appar och resurser i Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7fbade87-a453-426d-bfa5-c72227ea83cd
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 1c052906788f0f4fe4ca9fd6d3af63336245174a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a><span data-ttu-id="756c7-103">Skydda appar och resurser i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="756c7-103">Secure apps and resources in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="756c7-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="756c7-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="756c7-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="756c7-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="756c7-106">Azure RemoteApp ger användare åtkomst till centralt hanterade Windows-appar som kan du styra vad användarna kan och inte kan göra.</span><span class="sxs-lookup"><span data-stu-id="756c7-106">Azure RemoteApp provides users access to centrally-managed Windows apps, which lets you control what your users can and can't do.</span></span>  <span data-ttu-id="756c7-107">Detta är särskilt användbart när användaren ansluter från en ohanterad enhet (till exempel sina personliga Macbook) och du vill styra användaråtkomst eller upplevelse.</span><span class="sxs-lookup"><span data-stu-id="756c7-107">This is particularly useful when the user is connecting from an unmanaged device (like their personal Macbook) and you want to control the user access or experience.</span></span>

<span data-ttu-id="756c7-108">Om du använder Active Directory för autentisering av användare och du vill att användarna inte att kopiera data från en app, kan du till exempel konfigurera en Remote Desktop-Grupprincip att blockera användare från att kopiera data.</span><span class="sxs-lookup"><span data-stu-id="756c7-108">For example, if you are using Active Directory for user authentication and you want to prevent your users from copying data out of an app, you can configure a Remote Desktop Group Policy to block users from copying data.</span></span>

<span data-ttu-id="756c7-109">Ett annat exempel är om du vill blockera internet-åtkomst för en viss app i samlingen.</span><span class="sxs-lookup"><span data-stu-id="756c7-109">Another example is if you want to block internet access for a particular app in your collection.</span></span> <span data-ttu-id="756c7-110">Du kan skapa en regel för Windows-brandväggen som blockerar åtkomst när du skapar avbildningen för samlingen.</span><span class="sxs-lookup"><span data-stu-id="756c7-110">You can create a Windows Firewall rule that blocks the access when you create the image for your collection.</span></span>

## <a name="implementation-options"></a><span data-ttu-id="756c7-111">Implementeringsalternativ för</span><span class="sxs-lookup"><span data-stu-id="756c7-111">Implementation options</span></span>
  <span data-ttu-id="756c7-112">Här är implementeringsalternativen viktiga som kan användas enskilt eller tillsammans efter behov:</span><span class="sxs-lookup"><span data-stu-id="756c7-112">Here are the key implementation options, which can be used individually or in tandem as needed:</span></span>

1. <span data-ttu-id="756c7-113">Om RemoteApp-samlingen är ansluten till en domän kan du använda någon [Grupprincip](https://technet.microsoft.com/library/cc725828.aspx) (med undantag för inaktivt och koppla från timeout principerna beskrivs [här](../azure-subscription-service-limits.md)).</span><span class="sxs-lookup"><span data-stu-id="756c7-113">If your RemoteApp collection is domain joined you can enforce any [Group Policy](https://technet.microsoft.com/library/cc725828.aspx) (with the exception of the Idle and Disconnect timeout policies described [here](../azure-subscription-service-limits.md)).</span></span>
2. <span data-ttu-id="756c7-114">Som ett alternativ till Grupprincip (om samlingen är inte ansluten till en domän eller om du inte har rätt privilegier i AD) kan du konfigurera [lokala principer](https://technet.microsoft.com/library/cc775702.aspx) i mallavbildningen.</span><span class="sxs-lookup"><span data-stu-id="756c7-114">As an alternative to Group Policy (if your collection is not domain joined or you don't have the right privileges in AD), you can configure [Local Polices](https://technet.microsoft.com/library/cc775702.aspx) into your template image.</span></span>  <span data-ttu-id="756c7-115">Observera att gruppen principer trumf lokala principer när det finns en konflikt.</span><span class="sxs-lookup"><span data-stu-id="756c7-115">Note that group polices trump local policies when there is a conflict.</span></span>
3. <span data-ttu-id="756c7-116">Vissa inställningar för OS-appen kan inte konfigureras via Grupprincip, men kan vara via registret med den [RegEdit verktyget](remoteapp-hybridtrouble.md) vid konfiguration av mallavbildningen.</span><span class="sxs-lookup"><span data-stu-id="756c7-116">Some OS/app settings are not configurable via policy, but can be via registry key using the [RegEdit tool](remoteapp-hybridtrouble.md) while configuring your template image.</span></span>
4. <span data-ttu-id="756c7-117">Du kan använda [Windows-brandväggen](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) nätverket att styra åtkomsten till och från den dator där programmet körs.</span><span class="sxs-lookup"><span data-stu-id="756c7-117">You can use [Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) to control network access to and from the machine where the app is running.</span></span> <span data-ttu-id="756c7-118">Kontrollera att du inte blockerar URL: er och portar som anges här.</span><span class="sxs-lookup"><span data-stu-id="756c7-118">Just make sure you don't block the URLs and ports defined here.</span></span>
5. <span data-ttu-id="756c7-119">Du kan använda [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) att styra vilka program och filer användare kan köra.</span><span class="sxs-lookup"><span data-stu-id="756c7-119">You can use [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) to control which applications and files users can run.</span></span> <span data-ttu-id="756c7-120">Smarta användare kan till exempel ta reda på hur att köra program att du inte har publicerats men som är tillgängliga i bilden som du använde för att skapa samlingen - AppLocker kan blockera detta.</span><span class="sxs-lookup"><span data-stu-id="756c7-120">For example, savvy users can figure out how to run applications that you did not publish but that are available in the image you used to create the collection - AppLocker can block this.</span></span>

## <a name="detailed-information"></a><span data-ttu-id="756c7-121">Detaljerad information</span><span class="sxs-lookup"><span data-stu-id="756c7-121">Detailed information</span></span>
* <span data-ttu-id="756c7-122">Följande RDS-principer är troligt att det mest användbara:</span><span class="sxs-lookup"><span data-stu-id="756c7-122">The following RDS policies are likely to be most useful:</span></span>
  * [<span data-ttu-id="756c7-123">Enhet och resurs</span><span class="sxs-lookup"><span data-stu-id="756c7-123">Device and Resource Redirection</span></span>](https://technet.microsoft.com/library/ee791794.aspx)
  * [<span data-ttu-id="756c7-124">Omdirigering av skrivare</span><span class="sxs-lookup"><span data-stu-id="756c7-124">Printer Redirection</span></span>](https://technet.microsoft.com/library/ee791784.aspx)
  * <span data-ttu-id="756c7-125">[Profiler](https://technet.microsoft.com/library/ee791865.aspx).</span><span class="sxs-lookup"><span data-stu-id="756c7-125">[Profiles](https://technet.microsoft.com/library/ee791865.aspx).</span></span>
* <span data-ttu-id="756c7-126">Observera att konfigurera omdirigeringar via RemoteApp PowerShell-modul (som visas [här](remoteapp-redirection.md)) förlitar sig på klientdatorn att genomdriva principer, så om säkerhet är det primära målet ska du tillämpa principen via mallavbildningen lokal policy eller via en Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="756c7-126">Note that configuring redirections via the RemoteApp PowerShell module (as seen [here](remoteapp-redirection.md)) relies on the client machine to enforce the policy, so if security is the primary objective you'll want to enforce the policy via the template image local policy or via group policy.</span></span>
* <span data-ttu-id="756c7-127">[Principer för Windows Server 2012 R2](https://technet.microsoft.com/library/hh831791.aspx).</span><span class="sxs-lookup"><span data-stu-id="756c7-127">[Windows Server 2012 R2 policies](https://technet.microsoft.com/library/hh831791.aspx).</span></span>
* <span data-ttu-id="756c7-128">[Office 2013 principerna](https://technet.microsoft.com/library/cc178969.aspx) (inklusive [hur du anpassar Office-verktygsfältet](https://technet.microsoft.com/library/cc179143.aspx)).</span><span class="sxs-lookup"><span data-stu-id="756c7-128">[Office 2013 polices](https://technet.microsoft.com/library/cc178969.aspx) (including [how to customize the Office toolbar](https://technet.microsoft.com/library/cc179143.aspx)).</span></span>

