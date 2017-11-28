---
title: "aaaGet igång med Microsoft Power BI Embedded"
description: "Power BI Embedded lägger till interaktiva Power BI-rapporter i dina Business Intelligence-appar"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: ebb550cb4eba761dde3c23e4dd0314fc885817e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a><span data-ttu-id="bb80d-103">Komma igång med Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="bb80d-103">Get started with Microsoft Power BI Embedded</span></span>

<span data-ttu-id="bb80d-104">**Power BI Embedded** är en Azure-tjänst som gör programmet utvecklare tooadd interaktiva Power BI-rapporter till sina egna appar.</span><span class="sxs-lookup"><span data-stu-id="bb80d-104">**Power BI Embedded** is an Azure service that enables application developers tooadd interactive Power BI reports into their own applications.</span></span> <span data-ttu-id="bb80d-105">**Power BI Embedded** fungerar tillsammans med befintliga program utan att behöva designa eller ändra hello hur användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="bb80d-105">**Power BI Embedded** works with existing applications without needing redesign or changing hello way users sign in.</span></span>

<span data-ttu-id="bb80d-106">Resurser för **Microsoft Power BI Embedded** tillhandahålls via hello [Azure ARM-API: er](https://msdn.microsoft.com/library/mt712306.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb80d-106">Resources for **Microsoft Power BI Embedded** are provisioned through hello [Azure ARM APIs](https://msdn.microsoft.com/library/mt712306.aspx).</span></span> <span data-ttu-id="bb80d-107">I det här fallet hello resursen du etablera är en **Power BI-Arbetsytesamling**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-107">In this case, hello resource you provision is a **Power BI Workspace Collection**.</span></span>

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a><span data-ttu-id="bb80d-108">Skapa en arbetsytesamling</span><span class="sxs-lookup"><span data-stu-id="bb80d-108">Create a workspace collection</span></span>

<span data-ttu-id="bb80d-109">En **Arbetsytesamling** är hello översta Azure-resurs och en behållare för hello-innehåll som ska bäddas in i ditt program.</span><span class="sxs-lookup"><span data-stu-id="bb80d-109">A **Workspace Collection** is hello top-level Azure resource and a container for hello content that will be embedded in your application.</span></span> <span data-ttu-id="bb80d-110">En **arbetsytesamling** kan skapas på två sätt:</span><span class="sxs-lookup"><span data-stu-id="bb80d-110">A **Workspace Collection** can be created in two ways::</span></span>

* <span data-ttu-id="bb80d-111">Manuellt med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bb80d-111">Manually using hello Azure Portal</span></span>
* <span data-ttu-id="bb80d-112">Genom programmering med hello Azure Resource Manager API: er</span><span class="sxs-lookup"><span data-stu-id="bb80d-112">Programmatically using hello Azure Resource Manager(ARM) APIs</span></span>

<span data-ttu-id="bb80d-113">Låt oss gå igenom hello steg toobuild en **Arbetsytesamling** med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb80d-113">Let's walk through hello steps toobuild a **Workspace Collection** using hello Azure Portal.</span></span>

1. <span data-ttu-id="bb80d-114">Öppna och logga in på **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bb80d-114">Open and sign into **Azure Portal**: [http://portal.azure.com](http://portal.azure.com).</span></span>
2. <span data-ttu-id="bb80d-115">Klicka på **+ ny** på hello övre panelen.</span><span class="sxs-lookup"><span data-stu-id="bb80d-115">Click **+ New** on hello top panel.</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. <span data-ttu-id="bb80d-116">Under **Data + analys** klickar du på **Power BI Embedded**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-116">Under **Data + Analytics** click **Power BI Embedded**.</span></span>
4. <span data-ttu-id="bb80d-117">På hello **Arbetsytesamlingsbladet**, ange information om hello krävs.</span><span class="sxs-lookup"><span data-stu-id="bb80d-117">On hello **Workspace Collection Blade**, enter hello required information.</span></span> <span data-ttu-id="bb80d-118">Mer information om **priser** finns i [Priser för Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).</span><span class="sxs-lookup"><span data-stu-id="bb80d-118">For **Pricing**, see [Power BI Embedded pricing](http://go.microsoft.com/fwlink/?LinkID=760527).</span></span>
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. <span data-ttu-id="bb80d-119">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-119">Click **Create**.</span></span>

<span data-ttu-id="bb80d-120">Hej **Arbetsytesamling** tar några ögonblick tooprovision.</span><span class="sxs-lookup"><span data-stu-id="bb80d-120">hello **Workspace Collection** will take a few moments tooprovision.</span></span> <span data-ttu-id="bb80d-121">När du är klar, vidarebefordras du toohello **Arbetsytesamlingsbladet**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-121">When completed, you'll be taken toohello **Workspace Collection Blade**.</span></span>

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

<span data-ttu-id="bb80d-122">Hej **skapas bladet** innehåller hello information du behöver toocall hello API: er som skapar arbetsytor och distribuera innehåll toothem.</span><span class="sxs-lookup"><span data-stu-id="bb80d-122">hello **Creation Blade** contains hello information you need toocall hello APIs that create workspaces and deploy content toothem.</span></span>

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a><span data-ttu-id="bb80d-123">Visa API-åtkomstnycklar för Power BI</span><span class="sxs-lookup"><span data-stu-id="bb80d-123">View Power BI API access keys</span></span>

<span data-ttu-id="bb80d-124">En av hello viktigaste delar av information som behövs för toocall hello Power BI REST API: er är hello **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-124">One of hello most important pieces of information needed toocall hello Power BI REST APIs are hello **Access Keys**.</span></span> <span data-ttu-id="bb80d-125">Detta är att använda toogenerate hello **apptoken** som har använt tooauthenticate API-begäranden.</span><span class="sxs-lookup"><span data-stu-id="bb80d-125">These are used toogenerate hello **app tokens** that are used tooauthenticate your API requests.</span></span> <span data-ttu-id="bb80d-126">tooview din **åtkomstnycklar**, klickar du på **åtkomstnycklar** på hello **inställningsbladet**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-126">tooview your **Access Keys**, click **Access Keys** on hello **Settings Blade**.</span></span> <span data-ttu-id="bb80d-127">Mer information om **apptoken**, finns i [Autentisering och auktorisering med Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="bb80d-127">For more about **app tokens**, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

   ![](media/power-bi-embedded-get-started/access-keys.png)

<span data-ttu-id="bb80d-128">Du ser att du har två nycklar.</span><span class="sxs-lookup"><span data-stu-id="bb80d-128">You'll'notice that you have two keys.</span></span>

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

<span data-ttu-id="bb80d-129">Kopiera nycklarna och lagra dem på ett säkert sätt i din app.</span><span class="sxs-lookup"><span data-stu-id="bb80d-129">Copy these keys and store them securely in your application.</span></span> <span data-ttu-id="bb80d-130">Det är mycket viktigt att du hanterar dessa nycklar precis som ett lösenord, eftersom de kommer ger åtkomst till tooall hello innehåll i din **Arbetsytesamling**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-130">It's very important that you treat these keys as you would a password, because they'll provide access tooall hello content in your **Workspace Collection**.</span></span>

<span data-ttu-id="bb80d-131">Det finns två nycklar listade men bara en i taget behövs.</span><span class="sxs-lookup"><span data-stu-id="bb80d-131">While two keys are listed, only one key is needed at a particular time.</span></span> <span data-ttu-id="bb80d-132">hello andra nyckeln tillhandahålls så att du regelbundet kan återskapa nycklar utan att avbryta toohello-tjänsten för dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="bb80d-132">hello second key is provided so you can periodically regenerate keys without interrupting access toohello service.</span></span>

<span data-ttu-id="bb80d-133">Nu när du har en Power BI-instans för din app och **åtkomstnycklar** kan du importera en rapport till din egen app.</span><span class="sxs-lookup"><span data-stu-id="bb80d-133">Now that you have an instance of Power BI for your application, and **Access Keys**, you can import a report into your own app.</span></span> <span data-ttu-id="bb80d-134">Innan du lära dig hur tooimport en rapport, hello nästa avsnitt beskrivs skapar Powerbi-datauppsättningar och rapporter tooembed i en app.</span><span class="sxs-lookup"><span data-stu-id="bb80d-134">Before you learn how tooimport a report, hello next section describes creating Power BI datasets and reports tooembed into an app.</span></span>

## <a name="working-with-workspaces"></a><span data-ttu-id="bb80d-135">Arbeta med arbetsytor</span><span class="sxs-lookup"><span data-stu-id="bb80d-135">Working with workspaces</span></span>

<span data-ttu-id="bb80d-136">När du har skapat din arbetsytesamling måste toocreate en arbetsyta som ska innehålla dina rapporter och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="bb80d-136">After you have created your workspace collection, you will need toocreate a workspace that will house your reports and datasets.</span></span> <span data-ttu-id="bb80d-137">toocreate en arbetsyta, behöver du toouse hello [Post arbetsyta REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb80d-137">toocreate a workspace, you will need toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-tooembed-into-an-app-using-power-bi-desktop"></a><span data-ttu-id="bb80d-138">Så här skapar du Power BI-datauppsättningar och rapporter tooembed i en app med Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="bb80d-138">Create Power BI datasets and reports tooembed into an app using Power BI Desktop</span></span>

<span data-ttu-id="bb80d-139">Nu när du har skapat en instans av Power BI för ditt program och har **åtkomstnycklar**, behöver du toocreate hello Power BI-datauppsättningar och rapporter som du vill tooembed.</span><span class="sxs-lookup"><span data-stu-id="bb80d-139">Now that you have created an instance of Power BI for your application, and have **Access Keys**, you will need toocreate hello Power BI datasets and reports that you want tooembed.</span></span> <span data-ttu-id="bb80d-140">Datauppsättningar och rapporter kan skapas med hjälp av **Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-140">Datasets and reports can be created by using **Power BI Desktop**.</span></span> <span data-ttu-id="bb80d-141">Du kan hämta [Power BI Desktop kostnadsfritt](https://go.microsoft.com/fwlink/?LinkId=521662).</span><span class="sxs-lookup"><span data-stu-id="bb80d-141">You can download [Power BI Desktop for free](https://go.microsoft.com/fwlink/?LinkId=521662).</span></span> <span data-ttu-id="bb80d-142">Eller, tooquickly komma igång, kan du hämta hello [exempel på detaljhandelsanalys PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span><span class="sxs-lookup"><span data-stu-id="bb80d-142">Or, tooquickly get started, you can download hello [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547).</span></span>

> [!NOTE]
> <span data-ttu-id="bb80d-143">Mer om hur toolearn toouse **Power BI Desktop**, se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span><span class="sxs-lookup"><span data-stu-id="bb80d-143">toolearn more about how toouse **Power BI Desktop**, see [Getting Started with Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).</span></span>

<span data-ttu-id="bb80d-144">Med **Power BI Desktop**, du ansluta tooyour datakällan genom att importera en kopia av hello data till **Power BI Desktop** eller ansluta direkt toohello data datakällan **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-144">With **Power BI Desktop**, you connect tooyour data source by importing a copy of hello data into **Power BI Desktop** or connecting directly toohello data source using **DirectQuery**.</span></span>

<span data-ttu-id="bb80d-145">Här följer hello skillnaderna mellan **importera** och **DirectQuery**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-145">Here are hello differences between using **Import** and **DirectQuery**.</span></span>

| <span data-ttu-id="bb80d-146">Importera</span><span class="sxs-lookup"><span data-stu-id="bb80d-146">Import</span></span> | <span data-ttu-id="bb80d-147">DirectQuery</span><span class="sxs-lookup"><span data-stu-id="bb80d-147">DirectQuery</span></span> |
| --- | --- |
| <span data-ttu-id="bb80d-148">Tabeller, kolumner *och data* importeras eller kopieras till **Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-148">Tables, columns, *and data* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="bb80d-149">När du arbetar med visualiseringar ställer **Power BI Desktop** frågar en kopia av hello data.</span><span class="sxs-lookup"><span data-stu-id="bb80d-149">As you work with visualizations, **Power BI Desktop** queries a copy of hello data.</span></span> <span data-ttu-id="bb80d-150">toosee ändringar uppstod toohello underliggande data, du måste uppdatera eller importera en fullständig, aktuell datauppsättning igen.</span><span class="sxs-lookup"><span data-stu-id="bb80d-150">toosee any changes that occurred toohello underlying data, you must refresh, or import, a complete, current dataset again.</span></span> |<span data-ttu-id="bb80d-151">Endast *tabeller och kolumner* importeras eller kopieras till **Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="bb80d-151">Only *tables and columns* are imported or copied into **Power BI Desktop**.</span></span> <span data-ttu-id="bb80d-152">När du arbetar med visualiseringar ställer **Power BI Desktop** frågor hello underliggande datakällan, vilket innebär att du alltid visar aktuella data.</span><span class="sxs-lookup"><span data-stu-id="bb80d-152">As you work with visualizations, **Power BI Desktop** queries hello underlying data source, which means you're always viewing current data.</span></span> |

<span data-ttu-id="bb80d-153">Mer information om anslutande tooa datakälla finns [Anslut tooa datakällan](power-bi-embedded-connect-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="bb80d-153">For more about connecting tooa data source, see [Connect tooa data source](power-bi-embedded-connect-datasource.md).</span></span>

<span data-ttu-id="bb80d-154">När du har sparat ditt arbete i **Power BI Desktop** skapas en PBIX-fil.</span><span class="sxs-lookup"><span data-stu-id="bb80d-154">After you save your work in **Power BI Desktop**, a PBIX file is created.</span></span> <span data-ttu-id="bb80d-155">Den här filen innehåller rapporten.</span><span class="sxs-lookup"><span data-stu-id="bb80d-155">This file contains your report.</span></span> <span data-ttu-id="bb80d-156">Dessutom, om du importerar data hello innehåller PBIX hello fullständiga datauppsättningen eller om du använder **DirectQuery**, hello PBIX innehåller bara ett datauppsättningsschema.</span><span class="sxs-lookup"><span data-stu-id="bb80d-156">In addition, if you import data hello PBIX contains hello complete dataset, or if you use **DirectQuery**, hello PBIX contains just a dataset schema.</span></span> <span data-ttu-id="bb80d-157">Du distribuerar hello PBIX genom programmering i arbetsytan med hello [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb80d-157">You programmatically deploy hello PBIX into your workspace using hello [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="bb80d-158">**Power BI Embedded** har ytterligare API: er toochange hello-server och databas som din datauppsättning pekar tooand uppsättning autentiseringsuppgifter för tjänstekontot som datauppsättningen hello använder tooconnect tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="bb80d-158">**Power BI Embedded** has additional APIs toochange hello server and database that your dataset is pointing tooand set a service account credential that hello dataset will use tooconnect tooyour database.</span></span> <span data-ttu-id="bb80d-159">Se [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) och [Datakälla för korrigeringsgateway](https://msdn.microsoft.com/library/mt711498.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb80d-159">See [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) and [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).</span></span>

## <a name="create-power-bi-datasets-and-reports-using-apis"></a><span data-ttu-id="bb80d-160">Skapa Power BI-datauppsättningar och -rapporter med hjälp av API:er</span><span class="sxs-lookup"><span data-stu-id="bb80d-160">Create Power BI datasets and reports using APIs</span></span>

### <a name="datsets"></a><span data-ttu-id="bb80d-161">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="bb80d-161">Datsets</span></span>

<span data-ttu-id="bb80d-162">Du kan skapa datauppsättningar i Power BI Embedded med hello REST API.</span><span class="sxs-lookup"><span data-stu-id="bb80d-162">You can create datasets within Power BI Embedded using hello REST API.</span></span> <span data-ttu-id="bb80d-163">Du kan sedan skicka data till datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="bb80d-163">You can then push data into your dataset.</span></span> <span data-ttu-id="bb80d-164">Detta ger dig toowork med data utan hello behovet av Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="bb80d-164">This allows you toowork with data without hello need of Power BI Desktop.</span></span> <span data-ttu-id="bb80d-165">Mer information finns i [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx) (Lägga upp datauppsättningar).</span><span class="sxs-lookup"><span data-stu-id="bb80d-165">For more information, see [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx).</span></span>

### <a name="reports"></a><span data-ttu-id="bb80d-166">Rapporter</span><span class="sxs-lookup"><span data-stu-id="bb80d-166">Reports</span></span>

<span data-ttu-id="bb80d-167">Du kan skapa en rapport från en datamängd direkt i ditt program med hjälp av hello JavaScript API.</span><span class="sxs-lookup"><span data-stu-id="bb80d-167">You can create a report from a dataset directly in your application using hello JavaScript API.</span></span> <span data-ttu-id="bb80d-168">Mer information finns i [Skapa en ny rapport från en datauppsättning i Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span><span class="sxs-lookup"><span data-stu-id="bb80d-168">For more information, see [Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="bb80d-169">Se även</span><span class="sxs-lookup"><span data-stu-id="bb80d-169">See Also</span></span>

[<span data-ttu-id="bb80d-170">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="bb80d-170">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="bb80d-171">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="bb80d-171">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="bb80d-172">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="bb80d-172">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
<span data-ttu-id="bb80d-173">[Skapa en ny rapport från en datauppsättning i Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Spara rapporter](power-bi-embedded-save-reports.md)</span><span class="sxs-lookup"><span data-stu-id="bb80d-173">[Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md)
[Save reports](power-bi-embedded-save-reports.md)</span></span>  
[<span data-ttu-id="bb80d-174">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="bb80d-174">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="bb80d-175">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="bb80d-175">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="bb80d-176">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="bb80d-176">More questions?</span></span> [<span data-ttu-id="bb80d-177">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="bb80d-177">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

