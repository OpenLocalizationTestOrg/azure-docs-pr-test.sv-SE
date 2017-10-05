---
title: "Åtkomst till Azure AD App Proxy appar i team | Microsoft Docs"
description: "Använda Azure AD Application Proxy åtkomst till lokala program via Microsoft-Teams."
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
ms.openlocfilehash: 24e22d7314de536714a825cd7035d2cec2112278
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a><span data-ttu-id="3a95b-103">Åtkomst till lokala program via Microsoft-Teams</span><span class="sxs-lookup"><span data-stu-id="3a95b-103">Access your on-premises applications through Microsoft Teams</span></span>

<span data-ttu-id="3a95b-104">Azure Active Directory Application Proxy ger enkel inloggning till lokala program oavsett var du befinner dig och Microsoft-Teams förenklar samverkande arbetet på ett ställe.</span><span class="sxs-lookup"><span data-stu-id="3a95b-104">Azure Active Directory Application Proxy gives you single sign-on to on-premises applications no matter where you are, and Microsoft Teams streamlines your collaborative efforts in one place.</span></span> <span data-ttu-id="3a95b-105">Integrering av två innebär att användarna kan vara produktiva med sina medarbetare i en situation.</span><span class="sxs-lookup"><span data-stu-id="3a95b-105">Integrating the two together means that your users can be productive with their teammates in any situation.</span></span> 

<span data-ttu-id="3a95b-106">Användarna kan lägga till molnappar till deras team kanaler [med hjälp av flikarna](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), men vad händer om SharePoint-webbplatsen eller alla använder Verktyg för planering finns lokalt?</span><span class="sxs-lookup"><span data-stu-id="3a95b-106">Your users can add cloud apps to their Teams channels [using tabs](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US), but what happens if that SharePoint site or planning tool they all use is hosted on-premises?</span></span> <span data-ttu-id="3a95b-107">Application Proxy är lösningen.</span><span class="sxs-lookup"><span data-stu-id="3a95b-107">Application Proxy is the solution.</span></span> <span data-ttu-id="3a95b-108">De kan lägga till appar som publiceras via Application Proxy till deras kanaler som använder samma externa URL: er använder de alltid åtkomst till sina program via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="3a95b-108">They can add apps published through Application Proxy to their channels using the same external URLs they always use to access their apps remotely.</span></span> <span data-ttu-id="3a95b-109">Och eftersom Application Proxy som autentiserar via Azure Active Directory, har den samma enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3a95b-109">And because Application Proxy authenticates through Azure Active Directory, the same single sign-on experience carries through.</span></span>


## <a name="install-the-application-proxy-connector-and-publish-your-app"></a><span data-ttu-id="3a95b-110">Installera Application Proxy connector och publicera en app</span><span class="sxs-lookup"><span data-stu-id="3a95b-110">Install the Application Proxy connector and publish your app</span></span>

<span data-ttu-id="3a95b-111">Om du inte redan har gjort [konfigurera programproxyn för din klient och installera connector](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="3a95b-111">If you haven't already, [configure Application Proxy for your tenant and install the connector](active-directory-application-proxy-enable.md).</span></span> <span data-ttu-id="3a95b-112">Sedan [publicera programmet lokalt](application-proxy-publish-azure-portal.md) för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="3a95b-112">Then, [publish your on-premises application](application-proxy-publish-azure-portal.md) for remote access.</span></span> <span data-ttu-id="3a95b-113">När du publicerar appen, anteckna den externa URL eftersom dina slutanvändare behöver informationen när de lägger till appen team.</span><span class="sxs-lookup"><span data-stu-id="3a95b-113">When you're publishing the app, make note of the external URL because your end users need that information when they add the app to Teams.</span></span>

<span data-ttu-id="3a95b-114">Om du redan har dina appar som publiceras men inte kommer ihåg sina externa URL: er, söker du efter dem den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3a95b-114">If you already have your apps published but don't remember their external URLs, look them up in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3a95b-115">Logga in och sedan gå till **Azure Active Directory** > **företagsprogram** > **alla program** > Välj appen > **programproxy**.</span><span class="sxs-lookup"><span data-stu-id="3a95b-115">Sign in, then navigate to **Azure Active Directory** > **Enterprise applications** > **All applications** > select your app > **Application proxy**.</span></span>

## <a name="add-your-app-to-teams"></a><span data-ttu-id="3a95b-116">Lägg till din app i team</span><span class="sxs-lookup"><span data-stu-id="3a95b-116">Add your app to Teams</span></span>

<span data-ttu-id="3a95b-117">När du publicerar appen via Application Proxy kan du låta dina användare veta att de kan lägga till den som en flik direkt i deras team-kanaler.</span><span class="sxs-lookup"><span data-stu-id="3a95b-117">Once you publish the app through Application Proxy, let your users know that they can add it as a tab directly in their Teams channels.</span></span> <span data-ttu-id="3a95b-118">Ha dem följande tre steg:</span><span class="sxs-lookup"><span data-stu-id="3a95b-118">Have them follow these three steps:</span></span>

1. <span data-ttu-id="3a95b-119">Navigera till team kanalen där du vill lägga till den här appen och markera  **+**  att lägga till en flik.</span><span class="sxs-lookup"><span data-stu-id="3a95b-119">Navigate to the Teams channel where you want to add this app and select **+** to add a tab.</span></span>

   ![Välj Lägg till en flik](./media/application-proxy-teams/add-tab.png)

2. <span data-ttu-id="3a95b-121">Välj **webbplats** från Flikalternativ.</span><span class="sxs-lookup"><span data-stu-id="3a95b-121">Select **Website** from the tab options.</span></span>

   ![Lägga till en webbplats](./media/application-proxy-teams/website.png)

3. <span data-ttu-id="3a95b-123">Namnge fliken och ange URL-Adressen till Application Proxy externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="3a95b-123">Give the tab a name and set the URL to the Application Proxy external URL.</span></span> 

   ![Konfigurera fliknamn och en URL](./media/application-proxy-teams/tab-name-url.png)

<span data-ttu-id="3a95b-125">När en medlem i ett team lägger du till fliken, visas den för alla i kanalen.</span><span class="sxs-lookup"><span data-stu-id="3a95b-125">Once one member of a team adds the tab, it shows up for everyone in the channel.</span></span> <span data-ttu-id="3a95b-126">Alla användare som har åtkomst till appen hämta enkel inloggning med de autentiseringsuppgifter de använder för Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="3a95b-126">Any users who have access to the app get single sign-on access with the credentials they use for Microsoft Teams.</span></span> <span data-ttu-id="3a95b-127">Alla användare som inte har åtkomst till appen visas på fliken i team, men är spärrat tills du ge dem behörighet till appen lokalt och Azure portal publicerade versionen av appen.</span><span class="sxs-lookup"><span data-stu-id="3a95b-127">Any users who don't have access to the app will see the tab in Teams, but are blocked until you give them permissions to the on-premises app and the Azure portal published version of the app.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3a95b-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a95b-128">Next steps</span></span>

- <span data-ttu-id="3a95b-129">Lär dig hur du [publicera lokala SharePoint-webbplatser](application-proxy-enable-remote-access-sharepoint.md) med Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="3a95b-129">Learn how to [publish on-premises SharePoint sites](application-proxy-enable-remote-access-sharepoint.md) with Application Proxy.</span></span>
- <span data-ttu-id="3a95b-130">Konfigurera dina appar att använda [anpassade domäner](active-directory-application-proxy-custom-domains.md) för sina externa URL: en.</span><span class="sxs-lookup"><span data-stu-id="3a95b-130">Configure your apps to use [custom domains](active-directory-application-proxy-custom-domains.md) for their external URL.</span></span> 
