---
title: "Lägg till Oracle-databas-koppling i dina Logic Apps i Azure | Microsoft Docs"
description: "Använda anslutningstjänsten Oracle-databas i en logikapp"
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
ms.openlocfilehash: cc64441617eb5e7d5e70c1cf5c491a672428bc51
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-the-oracle-database-connector"></a><span data-ttu-id="7696c-103">Kom igång med kopplingen Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="7696c-103">Get started with the Oracle Database connector</span></span>

<span data-ttu-id="7696c-104">Med kopplingen Oracle-databas kan skapa du organisationens arbetsflöden som använder data i den befintliga databasen.</span><span class="sxs-lookup"><span data-stu-id="7696c-104">Using the Oracle Database connector, you create organizational workflows that use data in your existing database.</span></span> <span data-ttu-id="7696c-105">Den här anslutningen kan ansluta till en lokal Oracle-databas eller en virtuell Azure-dator med Oracle-databas som är installerad.</span><span class="sxs-lookup"><span data-stu-id="7696c-105">This connector can connect to an on-premises Oracle Database, or an Azure virtual machine with Oracle Database installed.</span></span> <span data-ttu-id="7696c-106">Med den här anslutningen kan du:</span><span class="sxs-lookup"><span data-stu-id="7696c-106">With this connector, you can:</span></span>

* <span data-ttu-id="7696c-107">Skapa ditt arbetsflöde genom att lägga till en ny kund till en databas för kunder eller uppdatera en ordning i en order-databas.</span><span class="sxs-lookup"><span data-stu-id="7696c-107">Build your workflow by adding a new customer to a customers database, or updating an order in an orders database.</span></span>
* <span data-ttu-id="7696c-108">Använd åtgärder för att hämta en rad med data, infoga en ny rad och även ta bort.</span><span class="sxs-lookup"><span data-stu-id="7696c-108">Use actions to get a row of data, insert a new row, and even delete.</span></span> <span data-ttu-id="7696c-109">Till exempel när en post har skapats i Dynamics CRM Online (en utlösare), sedan infoga en rad i en Oracle-databas (en åtgärd).</span><span class="sxs-lookup"><span data-stu-id="7696c-109">For example, when a record is created in Dynamics CRM Online (a trigger), then insert a row in an Oracle Database (an action).</span></span> 

<span data-ttu-id="7696c-110">Det här avsnittet visar hur du använder kopplingen Oracle-databas i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="7696c-110">This topic shows you how to use the Oracle Database connector in a logic app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7696c-111">Krav</span><span class="sxs-lookup"><span data-stu-id="7696c-111">Prerequisites</span></span>

* <span data-ttu-id="7696c-112">Oracle-versioner stöds:</span><span class="sxs-lookup"><span data-stu-id="7696c-112">Supported Oracle versions:</span></span> 
    * <span data-ttu-id="7696c-113">Oracle 9 och senare</span><span class="sxs-lookup"><span data-stu-id="7696c-113">Oracle 9 and later</span></span>
    * <span data-ttu-id="7696c-114">Oracle-klientprogramvaran 8.1.7 eller senare</span><span class="sxs-lookup"><span data-stu-id="7696c-114">Oracle client software 8.1.7 and later</span></span>

* <span data-ttu-id="7696c-115">Installera den lokala datagatewayen.</span><span class="sxs-lookup"><span data-stu-id="7696c-115">Install the on-premises data gateway.</span></span> <span data-ttu-id="7696c-116">[Ansluta till lokala data från logikappar](../logic-apps/logic-apps-gateway-connection.md) innehåller stegen.</span><span class="sxs-lookup"><span data-stu-id="7696c-116">[Connect to on-premises data from logic apps](../logic-apps/logic-apps-gateway-connection.md) lists the steps.</span></span> <span data-ttu-id="7696c-117">En gateway krävs för att ansluta till en lokal Oracle-databas eller en virtuell Azure-dator med Oracle DB installerat.</span><span class="sxs-lookup"><span data-stu-id="7696c-117">The gateway is required to connect to an on-premises Oracle Database, or an Azure VM with Oracle DB installed.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="7696c-118">Lokala datagateway fungerar som en brygga och ger en säker dataöverföring mellan lokala data (data som inte är i molnet) och dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="7696c-118">The on-premises data gateway acts as a bridge, and provides a secure data transfer between on-premises data (data that is not in the cloud) and your logic apps.</span></span> <span data-ttu-id="7696c-119">Samma gateway kan användas med flera tjänster och flera datakällor.</span><span class="sxs-lookup"><span data-stu-id="7696c-119">The same gateway can be used with multiple services, and multiple data sources.</span></span> <span data-ttu-id="7696c-120">Så kan du bara behöver installera gatewayen en gång.</span><span class="sxs-lookup"><span data-stu-id="7696c-120">So, you may only need to install the gateway once.</span></span>

* <span data-ttu-id="7696c-121">Installera Oracle-klienten på datorn där du installerade lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="7696c-121">Install the Oracle Client on the machine where you installed the on-premises data gateway.</span></span> <span data-ttu-id="7696c-122">Glöm inte att installera 64-bitars Oracle Data Provider för .NET från Oracle:</span><span class="sxs-lookup"><span data-stu-id="7696c-122">Be sure to install the 64-bit Oracle Data Provider for .NET from Oracle:</span></span>  

  [<span data-ttu-id="7696c-123">64-bitars ODAC 12c version 4 (12.1.0.2.4) för Windows x64</span><span class="sxs-lookup"><span data-stu-id="7696c-123">64-bit ODAC 12c Release 4 (12.1.0.2.4) for Windows x64</span></span>](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > <span data-ttu-id="7696c-124">Om Oracle-klienten inte är installerad, inträffar ett fel vid försök att skapa eller använda anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7696c-124">If the Oracle client is not installed, an error occurs when you try to create or use the connection.</span></span> <span data-ttu-id="7696c-125">Finns de vanliga fel i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7696c-125">See the common errors in this topic.</span></span>


## <a name="add-the-connector"></a><span data-ttu-id="7696c-126">Lägg till anslutningen</span><span class="sxs-lookup"><span data-stu-id="7696c-126">Add the connector</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7696c-127">Den här anslutningen har inte utlösare.</span><span class="sxs-lookup"><span data-stu-id="7696c-127">This connector does not have any triggers.</span></span> <span data-ttu-id="7696c-128">Det finns endast åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7696c-128">It has only actions.</span></span> <span data-ttu-id="7696c-129">Så när du skapar din logikapp, lägga till en annan utlösare för att starta din logikapp, till exempel **schema - upprepning**, eller **begäranden / svar - svar**.</span><span class="sxs-lookup"><span data-stu-id="7696c-129">So when you create your logic app, add another trigger to start your logic app, such as **Schedule - Recurrence**, or **Request / Response - Response**.</span></span> 

1. <span data-ttu-id="7696c-130">I den [Azure-portalen](https://portal.azure.com), skapa en tom logikapp.</span><span class="sxs-lookup"><span data-stu-id="7696c-130">In the [Azure portal](https://portal.azure.com), create a blank logic app.</span></span>

2. <span data-ttu-id="7696c-131">I början av din logikapp, Välj den **begäranden / svar - begäran** utlösare:</span><span class="sxs-lookup"><span data-stu-id="7696c-131">At the start of your logic app, select the **Request / Response - Request** trigger:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. <span data-ttu-id="7696c-132">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7696c-132">Select **Save**.</span></span> <span data-ttu-id="7696c-133">När du sparar genereras automatiskt en begärd URL.</span><span class="sxs-lookup"><span data-stu-id="7696c-133">When you save, a request URL is automatically generated.</span></span> 

4. <span data-ttu-id="7696c-134">Välj **nytt steg**, och välj **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="7696c-134">Select **New step**, and select **Add an action**.</span></span> <span data-ttu-id="7696c-135">Skriv i `oracle` att se tillgängliga åtgärder:</span><span class="sxs-lookup"><span data-stu-id="7696c-135">Type in `oracle` to see the available actions:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > <span data-ttu-id="7696c-136">Detta är det snabbaste sättet att se utlösare och åtgärder som är tillgängliga för alla anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7696c-136">This is also the quickest way to see the triggers and actions available for any connector.</span></span> <span data-ttu-id="7696c-137">Ange i en del av namnet på anslutningen, till exempel `oracle`.</span><span class="sxs-lookup"><span data-stu-id="7696c-137">Type in part of the connector name, such as `oracle`.</span></span> <span data-ttu-id="7696c-138">Designern listar alla utlösare och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7696c-138">The designer lists any triggers and any actions.</span></span> 

5. <span data-ttu-id="7696c-139">Välj en av åtgärderna som **Oracle-databasen - Get-raden**.</span><span class="sxs-lookup"><span data-stu-id="7696c-139">Select one of the actions, such as **Oracle Database - Get row**.</span></span> <span data-ttu-id="7696c-140">Välj **Anslut via lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="7696c-140">Select **Connect via on-premises data gateway**.</span></span> <span data-ttu-id="7696c-141">Ange servernamnet för Oracle, autentiseringsmetod, användarnamn, lösenord och välj gatewayen:</span><span class="sxs-lookup"><span data-stu-id="7696c-141">Enter the Oracle server name, authentication method, username, password, and select the gateway:</span></span>

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. <span data-ttu-id="7696c-142">När du är ansluten, markera en tabell från listan och ange rad-ID för tabellen.</span><span class="sxs-lookup"><span data-stu-id="7696c-142">Once connected, select a table from the list, and enter the row ID to your table.</span></span> <span data-ttu-id="7696c-143">Du behöver veta identifierare i tabellen.</span><span class="sxs-lookup"><span data-stu-id="7696c-143">You need to know the identifier to the table.</span></span> <span data-ttu-id="7696c-144">Om du inte vet administratören Oracle-databas och hämta utdata från `select * from yourTableName`.</span><span class="sxs-lookup"><span data-stu-id="7696c-144">If you don't know, contact your Oracle DB administrator, and get the output from `select * from yourTableName`.</span></span> <span data-ttu-id="7696c-145">Detta ger dig identifierbar information du behöver för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="7696c-145">This gives you the identifiable information you need to proceed.</span></span>

    <span data-ttu-id="7696c-146">I följande exempel returneras jobbdata från en databas för personal:</span><span class="sxs-lookup"><span data-stu-id="7696c-146">In the following example, job data is being returned from a Human Resources database:</span></span> 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. <span data-ttu-id="7696c-147">Du kan använda någon av de andra kopplingarna för att skapa arbetsflödet i den här nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7696c-147">In this next step, you can use any of the other connectors to build your workflow.</span></span> <span data-ttu-id="7696c-148">Om du vill testa hämta data från Oracle sedan skicka dig ett e-postmeddelande med Oracle data med hjälp av en skicka e-kopplingar, sådan Office 365 eller Gmail.</span><span class="sxs-lookup"><span data-stu-id="7696c-148">If you want to test getting data from Oracle, then send yourself an email with the Oracle data using one of the send email connectors, such Office 365 or Gmail.</span></span> <span data-ttu-id="7696c-149">Använd dynamiska token från Oracle-tabell för att skapa den `Subject` och `Body` av din e-post:</span><span class="sxs-lookup"><span data-stu-id="7696c-149">Use the dynamic tokens from the Oracle table to build the `Subject` and `Body` of your email:</span></span>

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. <span data-ttu-id="7696c-150">**Spara** din logikapp och välj sedan **kör**.</span><span class="sxs-lookup"><span data-stu-id="7696c-150">**Save** your logic app, and then select **Run**.</span></span> <span data-ttu-id="7696c-151">Stäng designern och titta på körs historiken för status.</span><span class="sxs-lookup"><span data-stu-id="7696c-151">Close the designer, and look at the runs history for the status.</span></span> <span data-ttu-id="7696c-152">Välj raden meddelande om det misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7696c-152">If it fails, select the failed message row.</span></span> <span data-ttu-id="7696c-153">Designer öppnas och visar vilka steg misslyckades och visar även information om felet.</span><span class="sxs-lookup"><span data-stu-id="7696c-153">The designer opens, and shows you which step failed, and also shows the error information.</span></span> <span data-ttu-id="7696c-154">Om det lyckas, bör du få ett e-postmeddelande med information som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="7696c-154">If it succeeds, then you should receive an email with the information you added.</span></span>


### <a name="workflow-ideas"></a><span data-ttu-id="7696c-155">Arbetsflöde för uppslag</span><span class="sxs-lookup"><span data-stu-id="7696c-155">Workflow ideas</span></span>

* <span data-ttu-id="7696c-156">Du vill övervaka #oracle hashtaggar och placera tweets i en databas så att de kan efterfrågas och används i andra program.</span><span class="sxs-lookup"><span data-stu-id="7696c-156">You want to monitor the #oracle hashtag, and put the tweets in a database so they can be queried, and used within other applications.</span></span> <span data-ttu-id="7696c-157">Lägg till i en logikapp den `Twitter - When a new tweet is posted` utlösa och ange den **#oracle** hashtaggar.</span><span class="sxs-lookup"><span data-stu-id="7696c-157">In a logic app, add the `Twitter - When a new tweet is posted` trigger, and enter the **#oracle** hashtag.</span></span> <span data-ttu-id="7696c-158">Lägg sedan till den `Oracle Database - Insert row` åtgärder, och välj din tabell:</span><span class="sxs-lookup"><span data-stu-id="7696c-158">Then, add the `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* <span data-ttu-id="7696c-159">Meddelanden skickas till en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="7696c-159">Messages are sent to a Service Bus queue.</span></span> <span data-ttu-id="7696c-160">Du vill hämta dessa meddelanden och placera dem i en databas.</span><span class="sxs-lookup"><span data-stu-id="7696c-160">You want to get these messages, and put them in a database.</span></span> <span data-ttu-id="7696c-161">Lägg till i en logikapp den `Service Bus - when a message is received in a queue` utlösa och Välj kön.</span><span class="sxs-lookup"><span data-stu-id="7696c-161">In a logic app, add the `Service Bus - when a message is received in a queue` trigger, and select the queue.</span></span> <span data-ttu-id="7696c-162">Lägg sedan till den `Oracle Database - Insert row` åtgärder, och välj din tabell:</span><span class="sxs-lookup"><span data-stu-id="7696c-162">Then, add the `Oracle Database - Insert row` action, and select your table:</span></span>

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a><span data-ttu-id="7696c-163">Vanliga fel</span><span class="sxs-lookup"><span data-stu-id="7696c-163">Common errors</span></span>

#### <a name="error-cannot-reach-the-gateway"></a><span data-ttu-id="7696c-164">**Fel**: Det går inte att nå gatewayen</span><span class="sxs-lookup"><span data-stu-id="7696c-164">**Error**: Cannot reach the Gateway</span></span>

<span data-ttu-id="7696c-165">**Orsak**: lokala datagateway kan inte ansluta till molnet.</span><span class="sxs-lookup"><span data-stu-id="7696c-165">**Cause**: The on-premises data gateway is not able to connect to the cloud.</span></span> 

<span data-ttu-id="7696c-166">**Minskning**: Kontrollera att din gateway körs på den lokala datorn där du installerade dem och att den kan ansluta till internet.</span><span class="sxs-lookup"><span data-stu-id="7696c-166">**Mitigation**: Make sure your gateway is running on the on-premises machine where you installed it, and that it can connect to the internet.</span></span>  <span data-ttu-id="7696c-167">Vi rekommenderar att du inte installerar gateway på en dator som kan stängas av eller i viloläge.</span><span class="sxs-lookup"><span data-stu-id="7696c-167">We recommend not installing the gateway on a computer that may be turned off or sleep.</span></span> <span data-ttu-id="7696c-168">Du kan också starta om tjänsten lokala data gateway (PBIEgwService).</span><span class="sxs-lookup"><span data-stu-id="7696c-168">You can also restart the on-premises data gateway service (PBIEgwService).</span></span>

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a><span data-ttu-id="7696c-169">**Fel**: den leverantör som används är föråldrad: ' System.Data.OracleClient kräver Oracle klientprogramversionen 8.1.7 eller senare ”..</span><span class="sxs-lookup"><span data-stu-id="7696c-169">**Error**: The provider being used is deprecated: 'System.Data.OracleClient requires Oracle client software version 8.1.7 or greater.'.</span></span> <span data-ttu-id="7696c-170">Besök [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) att installera den officiella leverantören.</span><span class="sxs-lookup"><span data-stu-id="7696c-170">Please visit [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) to install the official provider.</span></span>

<span data-ttu-id="7696c-171">**Orsak**: Oracle-klient-SDK inte är installerad på datorn där lokala datagateway körs.</span><span class="sxs-lookup"><span data-stu-id="7696c-171">**Cause**: The Oracle client SDK is not installed on the machine where the on-premises data gateway is running.</span></span>  

<span data-ttu-id="7696c-172">**Lösning**: hämta och installera Oracle klient-SDK på samma dator som lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="7696c-172">**Resolution**: Download and install the Oracle client SDK on the same computer as the on-premises data gateway.</span></span>

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a><span data-ttu-id="7696c-173">**Fel**: tabellen [tabellnamn] definierar inte några viktiga kolumner</span><span class="sxs-lookup"><span data-stu-id="7696c-173">**Error**: Table '[Tablename]' does not define any key columns</span></span>

<span data-ttu-id="7696c-174">**Orsak**: tabellen inte har någon primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="7696c-174">**Cause**: The table does not have any primary key.</span></span>  

<span data-ttu-id="7696c-175">**Lösning**: connector i Oracle-databasen kräver att en tabell med en primärnyckelkolumn används.</span><span class="sxs-lookup"><span data-stu-id="7696c-175">**Resolution**: The Oracle Database connector requires that a table with a primary key column be used.</span></span>

#### <a name="currently-not-supported"></a><span data-ttu-id="7696c-176">För närvarande stöds inte</span><span class="sxs-lookup"><span data-stu-id="7696c-176">Currently not supported</span></span>

* <span data-ttu-id="7696c-177">Vyer och lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="7696c-177">Views and stored procedures</span></span> 
* <span data-ttu-id="7696c-178">En tabell med sammansatta nycklar</span><span class="sxs-lookup"><span data-stu-id="7696c-178">Any table with composite keys</span></span>
* <span data-ttu-id="7696c-179">Kapslade objekttyper i tabeller</span><span class="sxs-lookup"><span data-stu-id="7696c-179">Nested object types in tables</span></span>
 
## <a name="connector-specific-details"></a><span data-ttu-id="7696c-180">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="7696c-180">Connector-specific details</span></span>

<span data-ttu-id="7696c-181">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/oracle/).</span><span class="sxs-lookup"><span data-stu-id="7696c-181">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/oracle/).</span></span> 

## <a name="get-some-help"></a><span data-ttu-id="7696c-182">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="7696c-182">Get some help</span></span>

<span data-ttu-id="7696c-183">Den [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) är det bra att ställa frågor, besvara frågor och se vad andra användare med Logic Apps gör.</span><span class="sxs-lookup"><span data-stu-id="7696c-183">The [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) is a great place to ask questions, answer questions, and see what other Logic Apps users are doing.</span></span> 

<span data-ttu-id="7696c-184">Du kan förbättra Logic Apps och kopplingar genom röstning och skicka dina idéer på [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="7696c-184">You can help improve Logic Apps and connectors by voting and submitting your ideas at [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish).</span></span> 


## <a name="next-steps"></a><span data-ttu-id="7696c-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7696c-185">Next steps</span></span>
<span data-ttu-id="7696c-186">[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md), och utforska de tillgängliga kopplingarna i Logic Apps på vår [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="7696c-186">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md), and explore the available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>
