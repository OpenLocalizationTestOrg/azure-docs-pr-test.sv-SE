---
title: aaaSecure appar och resurser i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toolock ned appar och resurser i Azure RemoteApp"
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
ms.openlocfilehash: 26325826e92855a12a0087f19a3e32cbe1116449
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-apps-and-resources-in-azure-remoteapp"></a><span data-ttu-id="bc986-103">Skydda appar och resurser i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="bc986-103">Secure apps and resources in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bc986-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="bc986-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="bc986-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="bc986-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="bc986-106">Azure RemoteApp ger användarna åtkomst toocentrally-hanterade Windows-appar, vilket gör att du kan styra vad användarna kan och inte kan göra.</span><span class="sxs-lookup"><span data-stu-id="bc986-106">Azure RemoteApp provides users access toocentrally-managed Windows apps, which lets you control what your users can and can't do.</span></span>  <span data-ttu-id="bc986-107">Detta är särskilt användbart när hello användare ansluter från en ohanterad enhet (till exempel sina personliga Macbook) och du vill toocontrol hello användaråtkomst eller upplevelse.</span><span class="sxs-lookup"><span data-stu-id="bc986-107">This is particularly useful when hello user is connecting from an unmanaged device (like their personal Macbook) and you want toocontrol hello user access or experience.</span></span>

<span data-ttu-id="bc986-108">Om du använder Active Directory för autentisering av användare och du vill att tooprevent användarna från att kopiera data från en app, kan du konfigurera en grupprincip för Remote Desktop tooblock användarna att kopiera data.</span><span class="sxs-lookup"><span data-stu-id="bc986-108">For example, if you are using Active Directory for user authentication and you want tooprevent your users from copying data out of an app, you can configure a Remote Desktop Group Policy tooblock users from copying data.</span></span>

<span data-ttu-id="bc986-109">Ett annat exempel är om du vill tooblock internet-åtkomst för en viss app i samlingen.</span><span class="sxs-lookup"><span data-stu-id="bc986-109">Another example is if you want tooblock internet access for a particular app in your collection.</span></span> <span data-ttu-id="bc986-110">Du kan skapa en regel för Windows-brandväggen att block hello åtkomst när du skapar hello avbildning för samlingen.</span><span class="sxs-lookup"><span data-stu-id="bc986-110">You can create a Windows Firewall rule that blocks hello access when you create hello image for your collection.</span></span>

## <a name="implementation-options"></a><span data-ttu-id="bc986-111">Implementeringsalternativ för</span><span class="sxs-lookup"><span data-stu-id="bc986-111">Implementation options</span></span>
  <span data-ttu-id="bc986-112">Här följer hello viktiga implementeringsalternativ som kan användas enskilt eller tillsammans efter behov:</span><span class="sxs-lookup"><span data-stu-id="bc986-112">Here are hello key implementation options, which can be used individually or in tandem as needed:</span></span>

1. <span data-ttu-id="bc986-113">Om RemoteApp-samlingen är ansluten till en domän kan du använda någon [Grupprincip](https://technet.microsoft.com/library/cc725828.aspx) (med undantag för hello av hello inaktivt och koppla från timeout principer beskrivs [här](../azure-subscription-service-limits.md)).</span><span class="sxs-lookup"><span data-stu-id="bc986-113">If your RemoteApp collection is domain joined you can enforce any [Group Policy](https://technet.microsoft.com/library/cc725828.aspx) (with hello exception of hello Idle and Disconnect timeout policies described [here](../azure-subscription-service-limits.md)).</span></span>
2. <span data-ttu-id="bc986-114">Som ett alternativt tooGroup principen (om samlingen är inte ansluten till en domän eller om du inte har rätt behörighet för hello i AD) kan du konfigurera [lokala principer](https://technet.microsoft.com/library/cc775702.aspx) i mallavbildningen.</span><span class="sxs-lookup"><span data-stu-id="bc986-114">As an alternative tooGroup Policy (if your collection is not domain joined or you don't have hello right privileges in AD), you can configure [Local Polices](https://technet.microsoft.com/library/cc775702.aspx) into your template image.</span></span>  <span data-ttu-id="bc986-115">Observera att gruppen principer trumf lokala principer när det finns en konflikt.</span><span class="sxs-lookup"><span data-stu-id="bc986-115">Note that group polices trump local policies when there is a conflict.</span></span>
3. <span data-ttu-id="bc986-116">Vissa inställningar för OS-appen kan inte konfigureras via Grupprincip, men kan vara via registernyckeln hello [RegEdit verktyget](remoteapp-hybridtrouble.md) vid konfiguration av mallavbildningen.</span><span class="sxs-lookup"><span data-stu-id="bc986-116">Some OS/app settings are not configurable via policy, but can be via registry key using hello [RegEdit tool](remoteapp-hybridtrouble.md) while configuring your template image.</span></span>
4. <span data-ttu-id="bc986-117">Du kan använda [Windows-brandväggen](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) toocontrol network access tooand från hello datorn där hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="bc986-117">You can use [Windows Firewall](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) toocontrol network access tooand from hello machine where hello app is running.</span></span> <span data-ttu-id="bc986-118">Kontrollera att du inte blockerar hello URL: er och portar som anges här.</span><span class="sxs-lookup"><span data-stu-id="bc986-118">Just make sure you don't block hello URLs and ports defined here.</span></span>
5. <span data-ttu-id="bc986-119">Du kan använda [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) toocontrol vilka program och filer användare kan köra.</span><span class="sxs-lookup"><span data-stu-id="bc986-119">You can use [AppLocker](https://technet.microsoft.com/library/hh831440.aspx) toocontrol which applications and files users can run.</span></span> <span data-ttu-id="bc986-120">Till exempel smarta användare kan ta reda på hur toorun program som du inte har publicerats men som är tillgängliga i hello avbildning som du använde toocreate hello samling - AppLocker kan blockera detta.</span><span class="sxs-lookup"><span data-stu-id="bc986-120">For example, savvy users can figure out how toorun applications that you did not publish but that are available in hello image you used toocreate hello collection - AppLocker can block this.</span></span>

## <a name="detailed-information"></a><span data-ttu-id="bc986-121">Detaljerad information</span><span class="sxs-lookup"><span data-stu-id="bc986-121">Detailed information</span></span>
* <span data-ttu-id="bc986-122">hello följande RDS principer är sannolikt toobe som är mest användbara:</span><span class="sxs-lookup"><span data-stu-id="bc986-122">hello following RDS policies are likely toobe most useful:</span></span>
  * [<span data-ttu-id="bc986-123">Enhet och resurs</span><span class="sxs-lookup"><span data-stu-id="bc986-123">Device and Resource Redirection</span></span>](https://technet.microsoft.com/library/ee791794.aspx)
  * [<span data-ttu-id="bc986-124">Omdirigering av skrivare</span><span class="sxs-lookup"><span data-stu-id="bc986-124">Printer Redirection</span></span>](https://technet.microsoft.com/library/ee791784.aspx)
  * <span data-ttu-id="bc986-125">[Profiler](https://technet.microsoft.com/library/ee791865.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc986-125">[Profiles](https://technet.microsoft.com/library/ee791865.aspx).</span></span>
* <span data-ttu-id="bc986-126">Observera att konfigurera omdirigeringar via hello RemoteApp PowerShell-modulen (som visas [här](remoteapp-redirection.md)) förlitar sig på hello datorn tooenforce hello klientprinciper, så om säkerheten är hello primära mål ska du tooenforce hello principen via Hej mallen bild lokala principen eller via en Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="bc986-126">Note that configuring redirections via hello RemoteApp PowerShell module (as seen [here](remoteapp-redirection.md)) relies on hello client machine tooenforce hello policy, so if security is hello primary objective you'll want tooenforce hello policy via hello template image local policy or via group policy.</span></span>
* <span data-ttu-id="bc986-127">[Principer för Windows Server 2012 R2](https://technet.microsoft.com/library/hh831791.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc986-127">[Windows Server 2012 R2 policies](https://technet.microsoft.com/library/hh831791.aspx).</span></span>
* <span data-ttu-id="bc986-128">[Office 2013 principerna](https://technet.microsoft.com/library/cc178969.aspx) (inklusive [hur toocustomize hello Office-verktygsfältet](https://technet.microsoft.com/library/cc179143.aspx)).</span><span class="sxs-lookup"><span data-stu-id="bc986-128">[Office 2013 polices](https://technet.microsoft.com/library/cc178969.aspx) (including [how toocustomize hello Office toolbar](https://technet.microsoft.com/library/cc179143.aspx)).</span></span>

