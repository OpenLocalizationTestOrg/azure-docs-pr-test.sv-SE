---
title: aaaConnect tooan lokal SAP-system i Azure Logic Apps | Microsoft Docs
description: "Ansluta tooan lokal SAP system från logik app-arbetsflödes hello lokala datagateway"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 594ec5fed337398bf931d396684630ee9f907d2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-on-premises-sap-system-from-logic-apps-with-hello-sap-connector"></a><span data-ttu-id="db9c9-103">Ansluta tooan lokal SAP system från logikappar med hello SAP connector</span><span class="sxs-lookup"><span data-stu-id="db9c9-103">Connect tooan on-premises SAP system from logic apps with hello SAP connector</span></span> 

<span data-ttu-id="db9c9-104">hello lokala datagateway kan du toomanage data och åtkomst till säker resurser som finns lokalt.</span><span class="sxs-lookup"><span data-stu-id="db9c9-104">hello on-premises data gateway enables you toomanage data, and securely access resources that are on-premises.</span></span> <span data-ttu-id="db9c9-105">Det här avsnittet visar hur du kan ansluta logic apps tooan lokal SAP-systemet.</span><span class="sxs-lookup"><span data-stu-id="db9c9-105">This topic shows how you can connect logic apps tooan on-premises SAP system.</span></span> <span data-ttu-id="db9c9-106">I det här exemplet logikappen begär en IDOC över HTTP och skickar tillbaka hello-svar.</span><span class="sxs-lookup"><span data-stu-id="db9c9-106">In this example, your logic app requests an IDOC over HTTP and sends hello response back.</span></span>    

> [!NOTE]
> <span data-ttu-id="db9c9-107">Aktuella begränsningar:</span><span class="sxs-lookup"><span data-stu-id="db9c9-107">Current limitations:</span></span> 
> - <span data-ttu-id="db9c9-108">Din logikapp timeout om alla steg som krävs för hello svar inte slutförs inom hello [tidsgränsen för begäran](./logic-apps-limits-and-config.md).</span><span class="sxs-lookup"><span data-stu-id="db9c9-108">Your logic app times out if all steps required for hello response don't finish within hello [request timeout limit](./logic-apps-limits-and-config.md).</span></span> <span data-ttu-id="db9c9-109">I det här scenariot kan begäranden blockeras.</span><span class="sxs-lookup"><span data-stu-id="db9c9-109">In this scenario, requests might get blocked.</span></span> 
> - <span data-ttu-id="db9c9-110">hello filväljaren visar inte alla tillgängliga hello-fält.</span><span class="sxs-lookup"><span data-stu-id="db9c9-110">hello file picker does not display all hello available fields.</span></span> <span data-ttu-id="db9c9-111">I det här scenariot kan du manuellt lägga till sökvägar.</span><span class="sxs-lookup"><span data-stu-id="db9c9-111">In this scenario, you can manually add paths.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db9c9-112">Krav</span><span class="sxs-lookup"><span data-stu-id="db9c9-112">Prerequisites</span></span>

- <span data-ttu-id="db9c9-113">Installera och konfigurera hello senaste [lokala datagateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="db9c9-113">Install and configure hello latest [on-premises data gateway](https://www.microsoft.com/download/details.aspx?id=53127) version 1.15.6150.1 or newer.</span></span> <span data-ttu-id="db9c9-114">[Hur tooconnect toohello lokala datagateway i en logikapp](http://aka.ms/logicapps-gateway) visar hello steg.</span><span class="sxs-lookup"><span data-stu-id="db9c9-114">[How tooconnect toohello on-premises data gateway in a logic app](http://aka.ms/logicapps-gateway) lists hello steps.</span></span> <span data-ttu-id="db9c9-115">hello gateway måste installeras på en lokal dator innan du kan fortsätta.</span><span class="sxs-lookup"><span data-stu-id="db9c9-115">hello gateway must be installed on an on-premises machine before you can proceed.</span></span>

- <span data-ttu-id="db9c9-116">Hämta och installera hello senaste SAP klientbiblioteket på hello datorn samma där du installerade hello datagateway.</span><span class="sxs-lookup"><span data-stu-id="db9c9-116">Download and install hello latest SAP client library on hello same machine where you installed hello data gateway.</span></span> <span data-ttu-id="db9c9-117">Använd någon av följande SAP versioner hello:</span><span class="sxs-lookup"><span data-stu-id="db9c9-117">Use any of hello following SAP versions:</span></span> 
    - <span data-ttu-id="db9c9-118">SAP-Server</span><span class="sxs-lookup"><span data-stu-id="db9c9-118">SAP Server</span></span>
        - <span data-ttu-id="db9c9-119">En SAP-Server som stöd hello .NET Connector (NCo) 3.0</span><span class="sxs-lookup"><span data-stu-id="db9c9-119">Any SAP Server that support hello .NET Connector (NCo) 3.0</span></span>
 
    - <span data-ttu-id="db9c9-120">SAP-klient</span><span class="sxs-lookup"><span data-stu-id="db9c9-120">SAP Client</span></span>
        - <span data-ttu-id="db9c9-121">SAP .NET Connector (NCo) 3.0</span><span class="sxs-lookup"><span data-stu-id="db9c9-121">SAP .NET Connector (NCo) 3.0</span></span>

## <a name="add-triggers-and-actions-for-connecting-tooyour-sap-system"></a><span data-ttu-id="db9c9-122">Lägg till utlösare och åtgärder för att ansluta tooyour SAP-system</span><span class="sxs-lookup"><span data-stu-id="db9c9-122">Add triggers and actions for connecting tooyour SAP system</span></span>

<span data-ttu-id="db9c9-123">hello SAP-koppling har åtgärder, men inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="db9c9-123">hello SAP connector has actions, but not triggers.</span></span> <span data-ttu-id="db9c9-124">Har vi toouse en annan utlösare hello början av hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="db9c9-124">So, we have toouse another trigger at hello start of hello workflow.</span></span> 

1. <span data-ttu-id="db9c9-125">Lägga till hello begäran och svar utlösare och välj sedan **nytt steg**.</span><span class="sxs-lookup"><span data-stu-id="db9c9-125">Add hello Request/Response trigger, and then select **New step**.</span></span>

2. <span data-ttu-id="db9c9-126">Välj **lägga till en åtgärd**, och välj sedan hello SAP-koppling genom att skriva `SAP` i sökfältet hello:</span><span class="sxs-lookup"><span data-stu-id="db9c9-126">Select **Add an action**, and then select hello SAP connector by typing `SAP` in hello search field:</span></span>    

     ![Välj SAP-programserver eller SAP Message Server](media/logic-apps-using-sap-connector/sap-action.png)

3. <span data-ttu-id="db9c9-128">Välj [ **SAP-programserver** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) eller [ **SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), baserat på din SAP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="db9c9-128">Select [**SAP Application Server**](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) or [**SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), based on your SAP setup.</span></span> <span data-ttu-id="db9c9-129">Om du inte har en befintlig anslutning, kan du ange toocreate en.</span><span class="sxs-lookup"><span data-stu-id="db9c9-129">If you don't have an existing connection, you are prompted toocreate one.</span></span>

   1. <span data-ttu-id="db9c9-130">Välj **Anslut via lokala datagateway**, och ange hello information för SAP-system:</span><span class="sxs-lookup"><span data-stu-id="db9c9-130">Select **Connect via on-premises data gateway**, and enter hello details for your SAP system:</span></span>   

       ![Lägg till anslutning sträng tooSAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. <span data-ttu-id="db9c9-132">Under **Gateway**, Välj en befintlig gateway eller tooinstall en ny gateway, Välj **installera gatewayen**.</span><span class="sxs-lookup"><span data-stu-id="db9c9-132">Under **Gateway**, select an existing gateway, or tooinstall a new gateway, select **Install Gateway**.</span></span>

        ![Installera en ny gateway](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. <span data-ttu-id="db9c9-134">När du har angett alla hello information, Välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="db9c9-134">After you enter all hello details, select **Create**.</span></span> 
   <span data-ttu-id="db9c9-135">Logic Apps konfigurerar och testar hello-anslutning, se till att hello anslutning fungerar korrekt.</span><span class="sxs-lookup"><span data-stu-id="db9c9-135">Logic Apps configures and tests hello connection, making sure that hello connection works properly.</span></span>

4. <span data-ttu-id="db9c9-136">Ange ett namn för din SAP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="db9c9-136">Enter a name for your SAP connection.</span></span>

5. <span data-ttu-id="db9c9-137">hello olika SAP-alternativ är nu tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="db9c9-137">hello different SAP options are now available.</span></span> <span data-ttu-id="db9c9-138">toofind IDOC kategori, Välj hello-listan.</span><span class="sxs-lookup"><span data-stu-id="db9c9-138">toofind your IDOC category, select from hello list.</span></span> <span data-ttu-id="db9c9-139">Eller manuellt ange i hello sökväg och välj hello HTTP-svar i hello **brödtext** fält:</span><span class="sxs-lookup"><span data-stu-id="db9c9-139">Or manually type in hello path, and select hello HTTP response in hello **body** field:</span></span>

     ![SAP-åtgärd](media/logic-apps-using-sap-connector/picture3.png)

6. <span data-ttu-id="db9c9-141">Lägg till hello åtgärd för att skapa en **HTTP-svar**.</span><span class="sxs-lookup"><span data-stu-id="db9c9-141">Add hello action for creating an **HTTP Response**.</span></span> <span data-ttu-id="db9c9-142">hello-svarsmeddelandet ska från hello SAP-utdata.</span><span class="sxs-lookup"><span data-stu-id="db9c9-142">hello response message should be from hello SAP output.</span></span>

7. <span data-ttu-id="db9c9-143">Spara din logikapp.</span><span class="sxs-lookup"><span data-stu-id="db9c9-143">Save your logic app.</span></span> <span data-ttu-id="db9c9-144">Testa den genom att skicka en IDOC via hello HTTP utlösaren URL.</span><span class="sxs-lookup"><span data-stu-id="db9c9-144">Test it by sending an IDOC through hello HTTP trigger URL.</span></span> <span data-ttu-id="db9c9-145">Vänta tills hello svar från hello logikapp efter hello IDOC skickas:</span><span class="sxs-lookup"><span data-stu-id="db9c9-145">After hello IDOC is sent, wait for hello response from hello logic app:</span></span>   

     > [!TIP]
     > <span data-ttu-id="db9c9-146">Kolla hur för[övervaka dina Logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="db9c9-146">Check out how too[monitor your Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).</span></span>

<span data-ttu-id="db9c9-147">Nu när hello SAP-anslutningen läggs tooyour logikapp kan du börja utforska andra funktioner:</span><span class="sxs-lookup"><span data-stu-id="db9c9-147">Now that hello SAP connector is added tooyour logic app, start exploring other functionalities:</span></span>

- <span data-ttu-id="db9c9-148">BAPI</span><span class="sxs-lookup"><span data-stu-id="db9c9-148">BAPI</span></span>
- <span data-ttu-id="db9c9-149">RFC</span><span class="sxs-lookup"><span data-stu-id="db9c9-149">RFC</span></span>

## <a name="get-help"></a><span data-ttu-id="db9c9-150">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="db9c9-150">Get help</span></span>

<span data-ttu-id="db9c9-151">tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="db9c9-151">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="db9c9-152">toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="db9c9-152">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="db9c9-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db9c9-153">Next steps</span></span>

- <span data-ttu-id="db9c9-154">Lär dig hur toovalidate, transformering och andra BizTalk-liknande funktioner i hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md).</span><span class="sxs-lookup"><span data-stu-id="db9c9-154">Learn how toovalidate, transform, and other BizTalk-like functions in hello [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).</span></span> 
- <span data-ttu-id="db9c9-155">[Ansluta tooon lokala data](../logic-apps/logic-apps-gateway-connection.md) från logikappar</span><span class="sxs-lookup"><span data-stu-id="db9c9-155">[Connect tooon-premises data](../logic-apps/logic-apps-gateway-connection.md) from logic apps</span></span>
