---
title: aaaAdd hello Oracle-databas connector i dina Logic Apps i Azure | Microsoft Docs
description: "Använd hello Oracle-databas kopplingen i en logikapp"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: 8a802a6c4782e210ff71848614152cb46ba5d651
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-oracle-database-connector"></a><span data-ttu-id="6ce2e-103">Kom igång med hello Oracle Database connector</span><span class="sxs-lookup"><span data-stu-id="6ce2e-103">Get started with hello Oracle Database connector</span></span>

<span data-ttu-id="6ce2e-104">Med hello Oracle Database connector kan skapa du organisationens arbetsflöden som använder data i den befintliga databasen.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-104">Using hello Oracle Database connector, you create organizational workflows that use data in your existing database.</span></span> <span data-ttu-id="6ce2e-105">Den här anslutningen kan ansluta tooan lokal Oracle-databas eller en virtuell Azure-dator med Oracle-databas har installerats.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-105">This connector can connect tooan on-premises Oracle Database, or an Azure virtual machine with Oracle Database installed.</span></span> <span data-ttu-id="6ce2e-106">Med den här anslutningen kan du:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-106">With this connector, you can:</span></span>

* <span data-ttu-id="6ce2e-107">Skapa ditt arbetsflöde genom att lägga till en ny databas för kunden tooa kunder eller uppdatera en ordning i en order-databas.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-107">Build your workflow by adding a new customer tooa customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="6ce2e-108">Använd åtgärder tooget en rad med data, infoga en ny rad och även ta bort.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-108">Use actions tooget a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="6ce2e-109">Till exempel när en post har skapats i Dynamics CRM Online (en utlösare), sedan infoga en rad i en Oracle-databas (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="6ce2e-109">For example, when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Oracle Database (an action).</span></span> 

<span data-ttu-id="6ce2e-110">Det här avsnittet visar hur toouse hello connector för Oracle-databas i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-110">This topic shows you how toouse hello Oracle Database connector in a logic app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6ce2e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="6ce2e-111">Prerequisites</span></span>

* <span data-ttu-id="6ce2e-112">Oracle-versioner stöds:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-112">Supported Oracle versions:</span></span> 
    * <span data-ttu-id="6ce2e-113">Oracle 9 och senare</span><span class="sxs-lookup"><span data-stu-id="6ce2e-113">Oracle 9 and later</span></span>
    * <span data-ttu-id="6ce2e-114">Oracle-klientprogramvaran 8.1.7 eller senare</span><span class="sxs-lookup"><span data-stu-id="6ce2e-114">Oracle client software 8.1.7 and later</span></span>

* <span data-ttu-id="6ce2e-115">Installera hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-115">Install hello on-premises data gateway.</span></span> <span data-ttu-id="6ce2e-116">[Ansluta tooon lokala data från logikappar](../logic-apps/logic-apps-gateway-connection.md) visar hello steg.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-116">[Connect tooon-premises data from logic apps](../logic-apps/logic-apps-gateway-connection.md) lists hello steps.</span></span> <span data-ttu-id="6ce2e-117">hello-gateway är obligatoriska tooconnect tooan lokal Oracle-databas eller en virtuell Azure-dator med Oracle DB installerad.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-117">hello gateway is required tooconnect tooan on-premises Oracle Database, or an Azure VM with Oracle DB installed.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="6ce2e-118">hello lokala datagateway fungerar som en brygga och ger en säker dataöverföring mellan lokala data (data som inte är i hello moln) och dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-118">hello on-premises data gateway acts as a bridge, and provides a secure data transfer between on-premises data (data that is not in hello cloud) and your logic apps.</span></span> <span data-ttu-id="6ce2e-119">hello samma gateway kan användas med flera tjänster och flera datakällor.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-119">hello same gateway can be used with multiple services, and multiple data sources.</span></span> <span data-ttu-id="6ce2e-120">Därför kanske behöver du bara tooinstall hello gateway en gång.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-120">So, you may only need tooinstall hello gateway once.</span></span>

* <span data-ttu-id="6ce2e-121">Installera hello Oracle-klient på hello datorn där du installerade hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-121">Install hello Oracle Client on hello machine where you installed hello on-premises data gateway.</span></span> <span data-ttu-id="6ce2e-122">Tänk tooinstall hello 64-bitars Oracle Data Provider för .NET från Oracle:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-122">Be sure tooinstall hello 64-bit Oracle Data Provider for .NET from Oracle:</span></span>  

  [<span data-ttu-id="6ce2e-123">64-bitars ODAC 12c version 4 (12.1.0.2.4) för Windows x64</span><span class="sxs-lookup"><span data-stu-id="6ce2e-123">64-bit ODAC 12c Release 4 (12.1.0.2.4) for Windows x64</span></span>](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > <span data-ttu-id="6ce2e-124">Om hello Oracle-klienten inte är installerad, uppstår ett fel när du toocreate eller Använd hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-124">If hello Oracle client is not installed, an error occurs when you try toocreate or use hello connection.</span></span> <span data-ttu-id="6ce2e-125">Se hello vanliga fel i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-125">See hello common errors in this topic.</span></span>


## <a name="add-hello-connector"></a><span data-ttu-id="6ce2e-126">Lägg till hello-koppling</span><span class="sxs-lookup"><span data-stu-id="6ce2e-126">Add hello connector</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6ce2e-127">Den här anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-127">This connector does not have any triggers.</span></span> <span data-ttu-id="6ce2e-128">Det finns endast åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-128">It has only actions.</span></span> <span data-ttu-id="6ce2e-129">Så när du skapar din logikapp, lägga till en annan utlösare toostart din logikapp som **schema - upprepning**, eller **begäranden / svar - svar**.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-129">So when you create your logic app, add another trigger toostart your logic app, such as **Schedule - Recurrence**, or **Request / Response - Response**.</span></span> 

1. <span data-ttu-id="6ce2e-130">I hello [Azure-portalen](https://portal.azure.com), skapa en tom logikapp.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-130">In hello [Azure portal](https://portal.azure.com), create a blank logic app.</span></span>

2. <span data-ttu-id="6ce2e-131">Hello början av din logikapp, Välj hello **begäranden / svar - begäran** utlösare:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-131">At hello start of your logic app, select hello **Request / Response - Request** trigger:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. <span data-ttu-id="6ce2e-132">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-132">Select **Save**.</span></span> <span data-ttu-id="6ce2e-133">När du sparar genereras automatiskt en begärd URL.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-133">When you save, a request URL is automatically generated.</span></span> 

4. <span data-ttu-id="6ce2e-134">Välj **nytt steg**, och välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-134">Select **New step**, and select **Add an action**.</span></span> <span data-ttu-id="6ce2e-135">Skriv i `oracle` toosee hello tillgängliga åtgärder:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-135">Type in `oracle` toosee hello available actions:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > <span data-ttu-id="6ce2e-136">Det är också hello snabbaste sättet toosee hello utlösare och åtgärder som är tillgängliga för alla anslutningar.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-136">This is also hello quickest way toosee hello triggers and actions available for any connector.</span></span> <span data-ttu-id="6ce2e-137">Ange i en del av hello Kopplingsnamn `oracle`.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-137">Type in part of hello connector name, such as `oracle`.</span></span> <span data-ttu-id="6ce2e-138">hello designer visas alla utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-138">hello designer lists any triggers and any actions.</span></span> 

5. <span data-ttu-id="6ce2e-139">Välj någon av hello åtgärder, exempelvis **Oracle-databasen - Get-raden**.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-139">Select one of hello actions, such as **Oracle Database - Get row**.</span></span> <span data-ttu-id="6ce2e-140">Välj **Anslut via lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-140">Select **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="6ce2e-141">Ange servernamnet för hello Oracle, autentiseringsmetod, användarnamn, lösenord och väljer hello gateway:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-141">Enter hello Oracle server name, authentication method, username, password, and select hello gateway:</span></span>

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. <span data-ttu-id="6ce2e-142">När du är ansluten, markera en tabell från listan hello och ange hello rad-ID tooyour tabell.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-142">Once connected, select a table from hello list, and enter hello row ID tooyour table.</span></span> <span data-ttu-id="6ce2e-143">Du behöver tooknow hello identifierare toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-143">You need tooknow hello identifier toohello table.</span></span> <span data-ttu-id="6ce2e-144">Om du inte vet administratören Oracle DB och få hello utdata från `select * from yourTableName`.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-144">If you don't know, contact your Oracle DB administrator, and get hello output from `select * from yourTableName`.</span></span> <span data-ttu-id="6ce2e-145">Det här ger du hello identifierbar information som du behöver tooproceed.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-145">This gives you hello identifiable information you need tooproceed.</span></span>

    <span data-ttu-id="6ce2e-146">I följande exempel hello, returneras jobbdata från en databas för personal:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-146">In hello following example, job data is being returned from a Human Resources database:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. <span data-ttu-id="6ce2e-147">I nästa steg, du kan använda någon av hello andra kopplingar toobuild arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-147">In this next step, you can use any of hello other connectors toobuild your workflow.</span></span> <span data-ttu-id="6ce2e-148">Om du vill ha tootest hämta data från Oracle och skicka ett e-postmeddelande med hello Oracle data med hjälp av en av hello skicka e-kopplingar, sådan Office 365 eller Gmail.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-148">If you want tootest getting data from Oracle, then send yourself an email with hello Oracle data using one of hello send email connectors, such Office 365 or Gmail.</span></span> <span data-ttu-id="6ce2e-149">Använd dynamiska hello-token från hello Oracle tabell toobuild hello `Subject` och `Body` av din e-post:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-149">Use hello dynamic tokens from hello Oracle table toobuild hello `Subject` and `Body` of your email:</span></span>

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. <span data-ttu-id="6ce2e-150">**Spara** din logikapp och välj sedan **kör**.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-150">**Save** your logic app, and then select **Run**.</span></span> <span data-ttu-id="6ce2e-151">Stäng designern för hello och titta på hello körs historik för hello status.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-151">Close hello designer, and look at hello runs history for hello status.</span></span> <span data-ttu-id="6ce2e-152">Välj raden för hello-meddelande om den inte.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-152">If it fails, select hello failed message row.</span></span> <span data-ttu-id="6ce2e-153">hello designer öppnas och visar vilka steg misslyckades och visar hello felinformation.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-153">hello designer opens, and shows you which step failed, and also shows hello error information.</span></span> <span data-ttu-id="6ce2e-154">Om det lyckas, bör du få ett e-postmeddelande med hello information som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-154">If it succeeds, then you should receive an email with hello information you added.</span></span>


### <a name="workflow-ideas"></a><span data-ttu-id="6ce2e-155">Arbetsflöde för uppslag</span><span class="sxs-lookup"><span data-stu-id="6ce2e-155">Workflow ideas</span></span>

* <span data-ttu-id="6ce2e-156">Du vill toomonitor hello #oracle hashtaggar och placera hello tweets i en databas så att de kan efterfrågas och används i andra program.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-156">You want toomonitor hello #oracle hashtag, and put hello tweets in a database so they can be queried, and used within other applications.</span></span> <span data-ttu-id="6ce2e-157">Lägg till hello i en logikapp `Twitter - When a new tweet is posted` utlösa och ange hello **#oracle** hashtaggar.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-157">In a logic app, add hello `Twitter - When a new tweet is posted` trigger, and enter hello **#oracle** hashtag.</span></span> <span data-ttu-id="6ce2e-158">Lägg sedan till hello `Oracle Database - Insert row` åtgärder, och välj din tabell:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-158">Then, add hello `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* <span data-ttu-id="6ce2e-159">Meddelanden skickas tooa Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-159">Messages are sent tooa Service Bus queue.</span></span> <span data-ttu-id="6ce2e-160">Du vill tooget dessa meddelanden och placera dem i en databas.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-160">You want tooget these messages, and put them in a database.</span></span> <span data-ttu-id="6ce2e-161">Lägg till hello i en logikapp `Service Bus - when a message is received in a queue` utlösare och välj hello kön.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-161">In a logic app, add hello `Service Bus - when a message is received in a queue` trigger, and select hello queue.</span></span> <span data-ttu-id="6ce2e-162">Lägg sedan till hello `Oracle Database - Insert row` åtgärder, och välj din tabell:</span><span class="sxs-lookup"><span data-stu-id="6ce2e-162">Then, add hello `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a><span data-ttu-id="6ce2e-163">Vanliga fel</span><span class="sxs-lookup"><span data-stu-id="6ce2e-163">Common errors</span></span>

#### <a name="error-cannot-reach-hello-gateway"></a><span data-ttu-id="6ce2e-164">**Fel**: Det går inte att nå hello Gateway</span><span class="sxs-lookup"><span data-stu-id="6ce2e-164">**Error**: Cannot reach hello Gateway</span></span>

<span data-ttu-id="6ce2e-165">**Orsak**: hello lokala datagateway är inte kan tooconnect toohello moln.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-165">**Cause**: hello on-premises data gateway is not able tooconnect toohello cloud.</span></span> 

<span data-ttu-id="6ce2e-166">**Minskning**: Kontrollera att din gateway körs på hello lokala dator där du har installerat och att den kan ansluta toohello internet.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-166">**Mitigation**: Make sure your gateway is running on hello on-premises machine where you installed it, and that it can connect toohello internet.</span></span>  <span data-ttu-id="6ce2e-167">Vi rekommenderar att du inte installerar hello gateway på en dator som kan stängas av eller i viloläge.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-167">We recommend not installing hello gateway on a computer that may be turned off or sleep.</span></span> <span data-ttu-id="6ce2e-168">Du kan också starta om hello lokala data gateway-tjänsten (PBIEgwService).</span><span class="sxs-lookup"><span data-stu-id="6ce2e-168">You can also restart hello on-premises data gateway service (PBIEgwService).</span></span>

#### <a name="error-hello-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-tooinstall-hello-official-provider"></a><span data-ttu-id="6ce2e-169">**Fel**: hello-providern som används är inaktuell: ' System.Data.OracleClient kräver Oracle klientprogramversionen 8.1.7 eller senare ”..</span><span class="sxs-lookup"><span data-stu-id="6ce2e-169">**Error**: hello provider being used is deprecated: 'System.Data.OracleClient requires Oracle client software version 8.1.7 or greater.'.</span></span> <span data-ttu-id="6ce2e-170">Besök [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello officiella leverantören.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-170">Please visit [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello official provider.</span></span>

<span data-ttu-id="6ce2e-171">**Orsak**: hello Oracle-klient SDK inte har installerats på datorn hello där hello lokala datagateway körs.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-171">**Cause**: hello Oracle client SDK is not installed on hello machine where hello on-premises data gateway is running.</span></span>  

<span data-ttu-id="6ce2e-172">**Lösning**: hämta och installera hello Oracle klient-SDK på hello samma dator som hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-172">**Resolution**: Download and install hello Oracle client SDK on hello same computer as hello on-premises data gateway.</span></span>

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a><span data-ttu-id="6ce2e-173">**Fel**: tabellen [tabellnamn] definierar inte några viktiga kolumner</span><span class="sxs-lookup"><span data-stu-id="6ce2e-173">**Error**: Table '[Tablename]' does not define any key columns</span></span>

<span data-ttu-id="6ce2e-174">**Orsak**: hello tabellen inte har någon primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-174">**Cause**: hello table does not have any primary key.</span></span>  

<span data-ttu-id="6ce2e-175">**Lösning**: hello Oracle Database connector kräver att en tabell med en primärnyckelkolumn används.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-175">**Resolution**: hello Oracle Database connector requires that a table with a primary key column be used.</span></span>

#### <a name="currently-not-supported"></a><span data-ttu-id="6ce2e-176">För närvarande stöds inte</span><span class="sxs-lookup"><span data-stu-id="6ce2e-176">Currently not supported</span></span>

* <span data-ttu-id="6ce2e-177">Vyer och lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="6ce2e-177">Views and stored procedures</span></span> 
* <span data-ttu-id="6ce2e-178">En tabell med sammansatta nycklar</span><span class="sxs-lookup"><span data-stu-id="6ce2e-178">Any table with composite keys</span></span>
* <span data-ttu-id="6ce2e-179">Kapslade objekttyper i tabeller</span><span class="sxs-lookup"><span data-stu-id="6ce2e-179">Nested object types in tables</span></span>
 
## <a name="connector-specific-details"></a><span data-ttu-id="6ce2e-180">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="6ce2e-180">Connector-specific details</span></span>

<span data-ttu-id="6ce2e-181">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/oracle/).</span><span class="sxs-lookup"><span data-stu-id="6ce2e-181">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/oracle/).</span></span> 

## <a name="get-some-help"></a><span data-ttu-id="6ce2e-182">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="6ce2e-182">Get some help</span></span>

<span data-ttu-id="6ce2e-183">Hej [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) är utmärkt placera tooask frågor, besvara frågor och se vad andra användare med Logic Apps gör.</span><span class="sxs-lookup"><span data-stu-id="6ce2e-183">hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is a great place tooask questions, answer questions, and see what other Logic Apps users are doing.</span></span> 

<span data-ttu-id="6ce2e-184">Du kan förbättra Logic Apps och kopplingar genom röstning och skicka dina idéer på [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="6ce2e-184">You can help improve Logic Apps and connectors by voting and submitting your ideas at [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span></span> 


## <a name="next-steps"></a><span data-ttu-id="6ce2e-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ce2e-185">Next steps</span></span>
<span data-ttu-id="6ce2e-186">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md), och utforska hello tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="6ce2e-186">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md), and explore hello available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
