---
title: aaaLearn hur toouse hello FTP-anslutningen i logikappar | Microsoft Docs
description: "Skapa logikappar med Azure App service. Ansluta tooFTP server toomanage dina filer. Du kan utföra olika åtgärder, till exempel ladda upp, uppdatera, hämta och ta bort filer i FTP-servern."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d83c55fe-eb59-4b7b-a5ec-afac5c772616
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/22/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a7020df2005ebb34fc569627ae0516b8528cc7a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-ftp-connector"></a><span data-ttu-id="b7fc4-105">Kom igång med hello FTP-anslutningen</span><span class="sxs-lookup"><span data-stu-id="b7fc4-105">Get started with hello FTP connector</span></span>
<span data-ttu-id="b7fc4-106">Använd hello FTP-anslutningen toomonitor, hantera och skapa filer på en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-106">Use hello FTP connector toomonitor, manage and create files on an  FTP server.</span></span> 

<span data-ttu-id="b7fc4-107">toouse [alla anslutningar](apis-list.md), måste du först toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-107">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="b7fc4-108">Du kan komma igång med [att skapa en logikapp nu](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b7fc4-108">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-tooftp"></a><span data-ttu-id="b7fc4-109">Ansluta tooFTP</span><span class="sxs-lookup"><span data-stu-id="b7fc4-109">Connect tooFTP</span></span>
<span data-ttu-id="b7fc4-110">Innan din logikapp kan komma åt någon tjänst, måste du först toocreate en *anslutning* toohello service.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-110">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="b7fc4-111">En [anslutning](connectors-overview.md) tillhandahåller anslutningen mellan en logikapp och en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-111">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-tooftp"></a><span data-ttu-id="b7fc4-112">Skapa en anslutning tooFTP</span><span class="sxs-lookup"><span data-stu-id="b7fc4-112">Create a connection tooFTP</span></span>
> [!INCLUDE [Steps toocreate a connection tooFTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a><span data-ttu-id="b7fc4-113">Använda en FTP-utlösare</span><span class="sxs-lookup"><span data-stu-id="b7fc4-113">Use a FTP trigger</span></span>
<span data-ttu-id="b7fc4-114">En utlösare är en händelse som definierats i en logikapp används toostart hello i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-114">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="b7fc4-115">[Mer information om utlösare](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="b7fc4-115">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="b7fc4-116">hello FTP-anslutningen kräver en FTP-server som kan nås från hello Internet och är konfigurerad toooperate med PASSIVT läge.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-116">hello FTP connector requires an FTP server that  is accessible from hello Internet and is configured toooperate with PASSIVE mode.</span></span> <span data-ttu-id="b7fc4-117">Hello FTP-anslutningen är också **inte kompatibel med implicit FTPS (FTP över SSL)**.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-117">Also, hello FTP connector is **not compatible with implicit FTPS (FTP over SSL)**.</span></span> <span data-ttu-id="b7fc4-118">hello FTP-anslutningen har endast stöd för explicit FTPS (FTP över SSL).</span><span class="sxs-lookup"><span data-stu-id="b7fc4-118">hello FTP connector only supports explicit FTPS (FTP over SSL).</span></span>  
> 
> 

<span data-ttu-id="b7fc4-119">I det här exemplet ska jag visa hur toouse hello **FTP - när en fil har lagts till eller ändrats** utlöser tooinitiate en logik app arbetsflödet när en fil har lagts till eller ändrats på en FTP-server.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-119">In this example, I will show you how toouse hello **FTP - When a file is added or modified** trigger tooinitiate a logic app workflow when a file is added to, or modified on, an FTP server.</span></span> <span data-ttu-id="b7fc4-120">Du kan använda den här utlösaren toomonitor en FTP-mapp för nya filer som representerar order från kunder i ett enterprise-exempel.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-120">In an enterprise example, you could use this trigger toomonitor an FTP folder for new files that represent orders from customers.</span></span>  <span data-ttu-id="b7fc4-121">Du kan sedan använda en åtgärd för FTP-anslutningen som **hämta filinnehåll** tooget hello innehållet i hello ordning för vidare bearbetning och lagring i order-databas.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-121">You could then use an FTP connector action such as **Get file content** tooget hello contents of hello order for further processing and storage in your orders database.</span></span>

1. <span data-ttu-id="b7fc4-122">Ange *ftp* i hello sökrutan på hello logic apps designer väljer du sedan hello **FTP - när en fil har lagts till eller ändrats** utlösare</span><span class="sxs-lookup"><span data-stu-id="b7fc4-122">Enter *ftp* in hello search box on hello logic apps designer then select hello **FTP - When a file is added or modified**  trigger</span></span>   
   <span data-ttu-id="b7fc4-123">![FTP-utlösarbild 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-123">![FTP trigger image 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)</span></span>  
   <span data-ttu-id="b7fc4-124">Hej **när en fil har lagts till eller ändrats** kontrollen öppnas</span><span class="sxs-lookup"><span data-stu-id="b7fc4-124">hello **When a file is added or modified** control opens up</span></span>  
   <span data-ttu-id="b7fc4-125">![Bild 2 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-125">![FTP trigger image 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)</span></span>  
2. <span data-ttu-id="b7fc4-126">Välj hello **...**  finns på hello höger sida av hello kontroll.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-126">Select hello **...** located on hello right side of hello control.</span></span> <span data-ttu-id="b7fc4-127">Då öppnas hello mappen väljarkontrollen</span><span class="sxs-lookup"><span data-stu-id="b7fc4-127">This opens hello folder picker control</span></span>  
   <span data-ttu-id="b7fc4-128">![Bild 3 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-128">![FTP trigger image 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)</span></span>  
3. <span data-ttu-id="b7fc4-129">Välj hello  **>**  (HÖGERPIL) och bläddra toofind hello mappen som du vill toomonitor för nya eller ändrade filer.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-129">Select hello **>** (right arrow) and browse toofind hello folder that you want toomonitor for new or modified files.</span></span> <span data-ttu-id="b7fc4-130">Välj hello mapp och notera hello mapp visas nu i hello **mappen** kontroll.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-130">Select hello folder and notice hello folder is now displayed in hello **Folder** control.</span></span>  
   <span data-ttu-id="b7fc4-131">![Bild 4 till FTP-utlösare](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-131">![FTP trigger image 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)</span></span>   

<span data-ttu-id="b7fc4-132">Din logikapp har nu konfigurerats med en utlösare som börjar en körning av hello andra utlösare och åtgärder i hello arbetsflödet när en fil ändras eller skapas i hello viss FTP-mapp.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-132">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow when a file is either modified or created in hello specific FTP folder.</span></span> 

> [!NOTE]
> <span data-ttu-id="b7fc4-133">Det måste innehålla minst en utlösare och en åtgärd för en logik app toobe funktionella.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-133">For a logic app toobe functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="b7fc4-134">Hello åtgärderna i hello nästa avsnitt tooadd en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-134">Follow hello steps in hello next section tooadd an action.</span></span>  
> 
> 

## <a name="use-a-ftp-action"></a><span data-ttu-id="b7fc4-135">Använda en FTP-åtgärd</span><span class="sxs-lookup"><span data-stu-id="b7fc4-135">Use a FTP action</span></span>
<span data-ttu-id="b7fc4-136">En åtgärd är en åtgärd som utförs av hello arbetsflöde som definierats i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-136">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="b7fc4-137">[Mer information om åtgärder](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="b7fc4-137">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="b7fc4-138">Nu när du har lagt till en utlösare, följer du dessa steg tooadd en åtgärd som kommer att få hello innehållet i hello nya eller ändrade filen som hittas med hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-138">Now that you have added a trigger, follow these steps tooadd an action that will get hello contents of hello new or modified file found by hello trigger.</span></span>    

1. <span data-ttu-id="b7fc4-139">Välj **+ nytt steg** tooadd hello hello åtgärd tooget hello innehållet i hello fil på hello FTP-server</span><span class="sxs-lookup"><span data-stu-id="b7fc4-139">Select **+ New step** tooadd hello hello action tooget hello contents of hello file on hello FTP server</span></span>  
2. <span data-ttu-id="b7fc4-140">Välj hello **lägga till en åtgärd** länk.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-140">Select hello **Add an action** link.</span></span>  
   <span data-ttu-id="b7fc4-141">![Bild 1 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-1.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-141">![FTP action image 1](./media/connectors-create-api-ftp/ftp-action-1.png)</span></span>  
3. <span data-ttu-id="b7fc4-142">Ange *FTP* toosearch för alla åtgärder relaterade tooFTP.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-142">Enter *FTP* toosearch for all actions related tooFTP.</span></span>
4. <span data-ttu-id="b7fc4-143">Välj **FTP - hämta filinnehåll** som hello åtgärd tootake när en ny eller ändrad fil hittas i hello FTP-mappen.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-143">Select **FTP - Get file content**  as hello action tootake when a new or modified file is found in hello FTP folder.</span></span>      
   <span data-ttu-id="b7fc4-144">![Bild 2 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-2.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-144">![FTP action image 2](./media/connectors-create-api-ftp/ftp-action-2.png)</span></span>  
   <span data-ttu-id="b7fc4-145">Hej **hämta filinnehåll** styra öppnas.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-145">hello **Get file content** control opens.</span></span> <span data-ttu-id="b7fc4-146">**Obs**: du kommer att tillfrågas tooauthorize din logik app tooaccess FTP-servern konto om du inte har gjort det tidigare.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-146">**Note**: you will be prompted tooauthorize your logic app tooaccess your FTP server account if you have not done so previously.</span></span>  
   <span data-ttu-id="b7fc4-147">![Bild 3 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-3.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-147">![FTP action image 3](./media/connectors-create-api-ftp/ftp-action-3.png)</span></span>   
5. <span data-ttu-id="b7fc4-148">Välj hello **filen** kontrollen (hello tomt utrymme finns under **filen***).</span><span class="sxs-lookup"><span data-stu-id="b7fc4-148">Select hello **File** control (hello white space located below **FILE***).</span></span> <span data-ttu-id="b7fc4-149">Här kan kan du använda någon av hello olika egenskaper från hello nya eller ändrade filen som hittas på hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-149">Here, you can use any of hello various properties from hello new or modified file found on hello FTP server.</span></span>  
6. <span data-ttu-id="b7fc4-150">Välj hello **filen innehåll** alternativet.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-150">Select hello **File content** option.</span></span>  
   <span data-ttu-id="b7fc4-151">![Bild 4 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-4.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-151">![FTP action image 4](./media/connectors-create-api-ftp/ftp-action-4.png)</span></span>   
7. <span data-ttu-id="b7fc4-152">hello kontroll uppdateras, som anger att hello **FTP - hämta filinnehåll** åtgärd får hello *filen innehåll* hello nya eller ändrade filen på hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-152">hello control is updated, indicating that hello **FTP - Get file content** action will get hello *file content* of hello new or modified file on hello FTP server.</span></span>      
   <span data-ttu-id="b7fc4-153">![Bild 5 till FTP-åtgärd](./media/connectors-create-api-ftp/ftp-action-5.png)</span><span class="sxs-lookup"><span data-stu-id="b7fc4-153">![FTP action image 5](./media/connectors-create-api-ftp/ftp-action-5.png)</span></span>     
8. <span data-ttu-id="b7fc4-154">Spara ditt arbete och lägger till en fil toohello FTP-mappen tootest arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-154">Save your work then add a file toohello FTP folder tootest your workflow.</span></span>    

<span data-ttu-id="b7fc4-155">Nu hello logikapp har konfigurerats med en utlösare toomonitor en mapp på en FTP-servern och initiera hello arbetsflödet när den hittar en ny fil eller en fil på hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-155">At this point, hello logic app has been configured with a trigger toomonitor a folder on an FTP server and initiate hello workflow when it finds either a new file or a modified file on hello FTP server.</span></span> 

<span data-ttu-id="b7fc4-156">Hej logikapp har också konfigurerats med en åtgärd tooget hello innehållet i hello nya eller ändrade filen.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-156">hello logic app also has been configured with an action tooget hello contents of hello new or modified file.</span></span>

<span data-ttu-id="b7fc4-157">Nu kan du lägga till en annan åtgärd som hello [SQL Server - infogningsraden](connectors-create-api-sqlazure.md) åtgärd tooinsert hello innehållet i hello nya eller ändrade filen till en SQL-databastabell.</span><span class="sxs-lookup"><span data-stu-id="b7fc4-157">You can now add another action such as hello [SQL Server - insert row](connectors-create-api-sqlazure.md) action tooinsert hello contents of hello new or modified file into a SQL database table.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="b7fc4-158">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="b7fc4-158">Connector-specific details</span></span>

<span data-ttu-id="b7fc4-159">Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/ftpconnector/).</span><span class="sxs-lookup"><span data-stu-id="b7fc4-159">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/ftpconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b7fc4-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7fc4-160">Next Steps</span></span>
[<span data-ttu-id="b7fc4-161">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="b7fc4-161">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

