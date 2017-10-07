---
title: aaaUse hello SharePoint Server-anslutningen i dina Logic Apps | Microsoft Docs
description: "Komma igång med hello hello SharePoint Server-anslutningen i dina Logic apps"
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
ms.openlocfilehash: 3b814f42611e4971ff5c94ae3b021829217911dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-connector"></a><span data-ttu-id="d6284-103">Kom igång med hello SharePoint-koppling</span><span class="sxs-lookup"><span data-stu-id="d6284-103">Get started with hello SharePoint connector</span></span>
<span data-ttu-id="d6284-104">hello SharePoint Connector ger ett sätt toowork med listor i SharePoint.</span><span class="sxs-lookup"><span data-stu-id="d6284-104">hello SharePoint Connector provides an way toowork with Lists on SharePoint.</span></span>

<span data-ttu-id="d6284-105">Kom igång genom att skapa en logikapp; Se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d6284-105">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-toosharepoint"></a><span data-ttu-id="d6284-106">Skapa en anslutning tooSharePoint</span><span class="sxs-lookup"><span data-stu-id="d6284-106">Create a connection tooSharePoint</span></span>
<span data-ttu-id="d6284-107">toouse Hej SharePoint Connector, du först skapa en **anslutning** ange hello information för dessa egenskaper:</span><span class="sxs-lookup"><span data-stu-id="d6284-107">toouse hello SharePoint Connector , you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="d6284-108">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d6284-108">Property</span></span> | <span data-ttu-id="d6284-109">Krävs</span><span class="sxs-lookup"><span data-stu-id="d6284-109">Required</span></span> | <span data-ttu-id="d6284-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d6284-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d6284-111">Token</span><span class="sxs-lookup"><span data-stu-id="d6284-111">Token</span></span> |<span data-ttu-id="d6284-112">Ja</span><span class="sxs-lookup"><span data-stu-id="d6284-112">Yes</span></span> |<span data-ttu-id="d6284-113">Ange autentiseringsuppgifter för SharePoint</span><span class="sxs-lookup"><span data-stu-id="d6284-113">Provide SharePoint Credentials</span></span> |

<span data-ttu-id="d6284-114">tooconnect för**SharePoint**, ange din identitet (användarnamn och lösenord, autentiseringsuppgifter för smartkort, etc.) tooSharePoint.</span><span class="sxs-lookup"><span data-stu-id="d6284-114">tooconnect too**SharePoint**, enter your identity (username and password, smart card credentials, etc.) tooSharePoint.</span></span> <span data-ttu-id="d6284-115">När du har autentiserats, kan du fortsätta toouse hello SharePoint connector i din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d6284-115">Once you've been authenticated, you can proceed toouse hello SharePoint connector  in your logic app.</span></span> 

<span data-ttu-id="d6284-116">Följ dessa steg toosign i SharePoint toocreate hello-anslutning när på hello designer av din logikapp, **anslutning** för användning i din logikapp:</span><span class="sxs-lookup"><span data-stu-id="d6284-116">While on hello designer of your logic app, follow these steps toosign into SharePoint toocreate hello connection **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="d6284-117">Ange SharePoint i sökrutan hello och vänta tills hello Sök tooreturn alla poster med SharePoint i hello namn:</span><span class="sxs-lookup"><span data-stu-id="d6284-117">Enter SharePoint in hello search box and wait for hello search tooreturn all entries with SharePoint in hello name:</span></span>   
   ![Konfigurera SharePoint][1]  
2. <span data-ttu-id="d6284-119">Välj **SharePoint - när en fil har skapats**</span><span class="sxs-lookup"><span data-stu-id="d6284-119">Select **SharePoint - When a file is created**</span></span>   
3. <span data-ttu-id="d6284-120">Välj **logga in tooSharePoint**:</span><span class="sxs-lookup"><span data-stu-id="d6284-120">Select **Sign in tooSharePoint**:</span></span>   
   <span data-ttu-id="d6284-121">![Konfigurera SharePoint][2]</span><span class="sxs-lookup"><span data-stu-id="d6284-121">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="d6284-122">Ge din SharePoint-autentiseringsuppgifter toosign i tooauthenticate SharePoint</span><span class="sxs-lookup"><span data-stu-id="d6284-122">Provide your SharePoint credentials toosign in tooauthenticate with SharePoint</span></span>   
   ![Konfigurera SharePoint][3]     
5. <span data-ttu-id="d6284-124">När hello autentisering har slutförts kommer du att omdirigerade tooyour logik app toocomplete den genom att konfigurera SharePoint- **när en fil skapas** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6284-124">After hello authentication completes you'll be redirected tooyour logic app toocomplete it by configuring SharePoint's **When a file is created** dialog.</span></span>          
   <span data-ttu-id="d6284-125">![Konfigurera SharePoint][4]</span><span class="sxs-lookup"><span data-stu-id="d6284-125">![Configure SharePoint][4]</span></span>  
6. <span data-ttu-id="d6284-126">Du kan sedan lägga till andra utlösare och åtgärder som du behöver toocomplete din logikapp.</span><span class="sxs-lookup"><span data-stu-id="d6284-126">You can then add other triggers and actions that you need toocomplete your logic app.</span></span>   
7. <span data-ttu-id="d6284-127">Spara ditt arbete genom att välja **spara** hello menyraden ovan.</span><span class="sxs-lookup"><span data-stu-id="d6284-127">Save your work by selecting **Save** on hello menu bar above.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="d6284-128">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="d6284-128">Connector-specific details</span></span>

<span data-ttu-id="d6284-129">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="d6284-129">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sharepoint/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="d6284-130">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="d6284-130">More connectors</span></span>
<span data-ttu-id="d6284-131">Gå tillbaka toohello [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="d6284-131">Go back toohello [APIs list](apis-list.md).</span></span>

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
