---
title: "Använda SharePoint Server-anslutningen i dina Logic Apps | Microsoft Docs"
description: "Komma igång med den SharePoint-Server-anslutningen i dina Logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-connector"></a><span data-ttu-id="8a875-103">Kom igång med SharePoint-koppling</span><span class="sxs-lookup"><span data-stu-id="8a875-103">Get started with the SharePoint connector</span></span>
<span data-ttu-id="8a875-104">SharePoint-kopplingen innehåller ett sätt att arbeta med listor i SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8a875-104">The SharePoint Connector provides an way to work with Lists on SharePoint.</span></span>

<span data-ttu-id="8a875-105">Kom igång genom att skapa en logikapp; Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="8a875-105">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-sharepoint"></a><span data-ttu-id="8a875-106">Skapa en anslutning till SharePoint</span><span class="sxs-lookup"><span data-stu-id="8a875-106">Create a connection to SharePoint</span></span>
<span data-ttu-id="8a875-107">Om du vill använda SharePoint-anslutningstjänsten måste du först skapa en **anslutning** ange detaljer för dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8a875-107">To use the SharePoint Connector , you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="8a875-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8a875-108">Property</span></span> | <span data-ttu-id="8a875-109">Krävs</span><span class="sxs-lookup"><span data-stu-id="8a875-109">Required</span></span> | <span data-ttu-id="8a875-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a875-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8a875-111">Token</span><span class="sxs-lookup"><span data-stu-id="8a875-111">Token</span></span> |<span data-ttu-id="8a875-112">Ja</span><span class="sxs-lookup"><span data-stu-id="8a875-112">Yes</span></span> |<span data-ttu-id="8a875-113">Ange autentiseringsuppgifter för SharePoint</span><span class="sxs-lookup"><span data-stu-id="8a875-113">Provide SharePoint Credentials</span></span> |

<span data-ttu-id="8a875-114">Att ansluta till **SharePoint**, ange din identitet (användarnamn och lösenord, smartkort autentiseringsuppgifter osv) till SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8a875-114">To connect to **SharePoint**, enter your identity (username and password, smart card credentials, etc.) to SharePoint.</span></span> <span data-ttu-id="8a875-115">När du har autentiserats, kan du fortsätta att använda SharePoint-kopplingen i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="8a875-115">Once you've been authenticated, you can proceed to use the SharePoint connector  in your logic app.</span></span> 

<span data-ttu-id="8a875-116">Följ dessa steg att logga in på SharePoint att skapa anslutningen på designer av din logikapp **anslutning** för användning i din logikapp:</span><span class="sxs-lookup"><span data-stu-id="8a875-116">While on the designer of your logic app, follow these steps to sign into SharePoint to create the connection **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="8a875-117">Ange SharePoint i sökrutan och vänta tills sökning för att returnera alla poster med SharePoint i namnet:</span><span class="sxs-lookup"><span data-stu-id="8a875-117">Enter SharePoint in the search box and wait for the search to return all entries with SharePoint in the name:</span></span>   
   ![Konfigurera SharePoint][1]  
2. <span data-ttu-id="8a875-119">Välj **SharePoint - när en fil har skapats**</span><span class="sxs-lookup"><span data-stu-id="8a875-119">Select **SharePoint - When a file is created**</span></span>   
3. <span data-ttu-id="8a875-120">Välj **logga in på SharePoint**:</span><span class="sxs-lookup"><span data-stu-id="8a875-120">Select **Sign in to SharePoint**:</span></span>   
   <span data-ttu-id="8a875-121">![Konfigurera SharePoint][2]</span><span class="sxs-lookup"><span data-stu-id="8a875-121">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="8a875-122">Ange din SharePoint-autentiseringsuppgifter för att logga in att autentisera med SharePoint</span><span class="sxs-lookup"><span data-stu-id="8a875-122">Provide your SharePoint credentials to sign in to authenticate with SharePoint</span></span>   
   ![Konfigurera SharePoint][3]     
5. <span data-ttu-id="8a875-124">När autentiseringen har slutförts ska du omdirigeras till din logikapp för att slutföra den genom att konfigurera SharePoint- **när en fil skapas** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8a875-124">After the authentication completes you'll be redirected to your logic app to complete it by configuring SharePoint's **When a file is created** dialog.</span></span>          
   <span data-ttu-id="8a875-125">![Konfigurera SharePoint][4]</span><span class="sxs-lookup"><span data-stu-id="8a875-125">![Configure SharePoint][4]</span></span>  
6. <span data-ttu-id="8a875-126">Du kan sedan lägga till andra utlösare och åtgärder som behövs för att slutföra din logikapp.</span><span class="sxs-lookup"><span data-stu-id="8a875-126">You can then add other triggers and actions that you need to complete your logic app.</span></span>   
7. <span data-ttu-id="8a875-127">Spara ditt arbete genom att välja **spara** på menyraden ovan.</span><span class="sxs-lookup"><span data-stu-id="8a875-127">Save your work by selecting **Save** on the menu bar above.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="8a875-128">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="8a875-128">Connector-specific details</span></span>

<span data-ttu-id="8a875-129">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="8a875-129">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sharepoint/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="8a875-130">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="8a875-130">More connectors</span></span>
<span data-ttu-id="8a875-131">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="8a875-131">Go back to the [APIs list](apis-list.md).</span></span>

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
