---
title: aaaAccess Azure AD App Proxy appar i team | Microsoft Docs
description: "Använda Azure AD Application Proxy tooaccess lokalt programmet via Microsoft-Teams."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 13c36e43ae6349df09272e308ad4f40451cbbeb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a><span data-ttu-id="81910-103">Åtkomst till lokala program via Microsoft-Teams</span><span class="sxs-lookup"><span data-stu-id="81910-103">Access your on-premises applications through Microsoft Teams</span></span>

<span data-ttu-id="81910-104">Azure Active Directory Application Proxy ger enkel inloggning tooon lokala program oavsett var du befinner dig och Microsoft-Teams förenklar samverkande arbetet på ett ställe.</span><span class="sxs-lookup"><span data-stu-id="81910-104">Azure Active Directory Application Proxy gives you single sign-on tooon-premises applications no matter where you are, and Microsoft Teams streamlines your collaborative efforts in one place.</span></span> <span data-ttu-id="81910-105">Integrera hello innebär två tillsammans att användarna kan vara produktiva med sina medarbetare i en situation.</span><span class="sxs-lookup"><span data-stu-id="81910-105">Integrating hello two together means that your users can be productive with their teammates in any situation.</span></span> 

<span data-ttu-id="81910-106">Användarna kan lägga till molnet appar tootheir team kanaler [med hjälp av flikarna](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), men vad händer om SharePoint-webbplatsen eller alla använder Verktyg för planering finns lokalt?</span><span class="sxs-lookup"><span data-stu-id="81910-106">Your users can add cloud apps tootheir Teams channels [using tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), but what happens if that SharePoint site or planning tool they all use is hosted on-premises?</span></span> <span data-ttu-id="81910-107">Application Proxy är hello lösning.</span><span class="sxs-lookup"><span data-stu-id="81910-107">Application Proxy is hello solution.</span></span> <span data-ttu-id="81910-108">De kan lägga till appar som publicerats via Application Proxy tootheir hello kanaler som använder samma externa URL: er använder de alltid tooaccess sina appar via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="81910-108">They can add apps published through Application Proxy tootheir channels using hello same external URLs they always use tooaccess their apps remotely.</span></span> <span data-ttu-id="81910-109">Och eftersom Application Proxy som autentiserar via Azure Active Directory, hello samma enkel inloggning utför via.</span><span class="sxs-lookup"><span data-stu-id="81910-109">And because Application Proxy authenticates through Azure Active Directory, hello same single sign-on experience carries through.</span></span>


## <a name="install-hello-application-proxy-connector-and-publish-your-app"></a><span data-ttu-id="81910-110">Installera hello Application Proxy connector och publicera en app</span><span class="sxs-lookup"><span data-stu-id="81910-110">Install hello Application Proxy connector and publish your app</span></span>

<span data-ttu-id="81910-111">Om du inte redan har gjort [konfigurera programproxyn för din klient och installera hello connector](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="81910-111">If you haven't already, [configure Application Proxy for your tenant and install hello connector](active-directory-application-proxy-enable.md).</span></span> <span data-ttu-id="81910-112">Sedan [publicera programmet lokalt](application-proxy-publish-azure-portal.md) för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="81910-112">Then, [publish your on-premises application](application-proxy-publish-azure-portal.md) for remote access.</span></span> <span data-ttu-id="81910-113">När du publicerar hello app, anteckna hello extern URL eftersom dina slutanvändare behöver informationen när de lägger till hello app tooTeams.</span><span class="sxs-lookup"><span data-stu-id="81910-113">When you're publishing hello app, make note of hello external URL because your end users need that information when they add hello app tooTeams.</span></span>

<span data-ttu-id="81910-114">Om du redan har dina appar som publiceras men inte kommer ihåg sina externa URL: er, söker du efter dem i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="81910-114">If you already have your apps published but don't remember their external URLs, look them up in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="81910-115">Logga in och sedan navigera för**Azure Active Directory** > **företagsprogram** > **alla program** > Välj appen > **Programproxy**.</span><span class="sxs-lookup"><span data-stu-id="81910-115">Sign in, then navigate too**Azure Active Directory** > **Enterprise applications** > **All applications** > select your app > **Application proxy**.</span></span>

## <a name="add-your-app-tooteams"></a><span data-ttu-id="81910-116">Lägg till app-tooTeams</span><span class="sxs-lookup"><span data-stu-id="81910-116">Add your app tooTeams</span></span>

<span data-ttu-id="81910-117">När du publicerar hello appen via Application Proxy kan du låta dina användare veta att de kan lägga till den som en flik direkt i deras team-kanaler.</span><span class="sxs-lookup"><span data-stu-id="81910-117">Once you publish hello app through Application Proxy, let your users know that they can add it as a tab directly in their Teams channels.</span></span> <span data-ttu-id="81910-118">Ha dem följande tre steg:</span><span class="sxs-lookup"><span data-stu-id="81910-118">Have them follow these three steps:</span></span>

1. <span data-ttu-id="81910-119">Navigera toohello team kanaler där du vill att tooadd den här appen och markera  **+**  tooadd en flik.</span><span class="sxs-lookup"><span data-stu-id="81910-119">Navigate toohello Teams channel where you want tooadd this app and select **+** tooadd a tab.</span></span>

   ![Välj Lägg till en flik](./media/application-proxy-teams/add-tab.png)

2. <span data-ttu-id="81910-121">Välj **webbplats** från hello alternativ på fliken.</span><span class="sxs-lookup"><span data-stu-id="81910-121">Select **Website** from hello tab options.</span></span>

   ![Lägga till en webbplats](./media/application-proxy-teams/website.png)

3. <span data-ttu-id="81910-123">Namnge hello-fliken och ange hello URL toohello Application Proxy externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="81910-123">Give hello tab a name and set hello URL toohello Application Proxy external URL.</span></span> 

   ![Konfigurera fliknamn och en URL](./media/application-proxy-teams/tab-name-url.png)

<span data-ttu-id="81910-125">När en medlem i ett team lägger hello-fliken, visas den för alla hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="81910-125">Once one member of a team adds hello tab, it shows up for everyone in hello channel.</span></span> <span data-ttu-id="81910-126">Alla användare som har åtkomst toohello app hämta enkel inloggning med hello autentiseringsuppgifter de använder för Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="81910-126">Any users who have access toohello app get single sign-on access with hello credentials they use for Microsoft Teams.</span></span> <span data-ttu-id="81910-127">Alla användare som inte har åtkomst toohello app hello-fliken i team visas, men är spärrat tills du ge dem behörighet toohello lokalt app och hello Azure portal publicerade versionen av hello app.</span><span class="sxs-lookup"><span data-stu-id="81910-127">Any users who don't have access toohello app will see hello tab in Teams, but are blocked until you give them permissions toohello on-premises app and hello Azure portal published version of hello app.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="81910-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81910-128">Next steps</span></span>

- <span data-ttu-id="81910-129">Lär dig hur för[publicera lokala SharePoint-webbplatser](application-proxy-enable-remote-access-sharepoint.md) med Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="81910-129">Learn how too[publish on-premises SharePoint sites](application-proxy-enable-remote-access-sharepoint.md) with Application Proxy.</span></span>
- <span data-ttu-id="81910-130">Konfigurera din apps toouse [anpassade domäner](active-directory-application-proxy-custom-domains.md) för sina externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="81910-130">Configure your apps toouse [custom domains](active-directory-application-proxy-custom-domains.md) for their external URL.</span></span> 
