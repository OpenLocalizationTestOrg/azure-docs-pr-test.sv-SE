---
title: "Lär dig att hantera AzureML-webbtjänster med hjälp av API-hantering | Microsoft Docs"
description: "En guide som visar hur du hanterar AzureML-webbtjänster som använder API-hantering."
keywords: Machine learning api-hantering
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 65eff3f4971f79886a840bb19bf76aaab48878de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a><span data-ttu-id="24285-104">Lär dig hur du hanterar AzureML-webbtjänster som använder API Management</span><span class="sxs-lookup"><span data-stu-id="24285-104">Learn how to manage AzureML web services using API Management</span></span>
## <a name="overview"></a><span data-ttu-id="24285-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="24285-105">Overview</span></span>
<span data-ttu-id="24285-106">Den här guiden visar hur du Kom snabbt igång med API-hantering för att hantera AzureML-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="24285-106">This guide shows you how to quickly get started using API Management to manage your AzureML web services.</span></span>

## <a name="what-is-azure-api-management"></a><span data-ttu-id="24285-107">Vad är Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="24285-107">What is Azure API Management?</span></span>
<span data-ttu-id="24285-108">Azure API Management är en Azure-tjänst som låter dig hantera REST API-slutpunkter genom att definiera användaråtkomst, begränsning och instrumentpanelen för övervakning.</span><span class="sxs-lookup"><span data-stu-id="24285-108">Azure API Management is an Azure service that lets you manage your REST API endpoints by defining user access, usage throttling, and dashboard monitoring.</span></span> <span data-ttu-id="24285-109">Klicka på [här](https://azure.microsoft.com/services/api-management/) information om Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="24285-109">Click [here](https://azure.microsoft.com/services/api-management/) for details on Azure API Management.</span></span> <span data-ttu-id="24285-110">Klicka på [här](../api-management/api-management-get-started.md) en guide om hur du kommer igång med Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="24285-110">Click [here](../api-management/api-management-get-started.md) for a guide on how to get started with Azure API Management.</span></span> <span data-ttu-id="24285-111">Den här andra guiden som den här guiden bygger på omfattar flera ämnen, t.ex. meddelande konfigurationer, nivå prissättning, svar hantering, autentisering av användare, skapa produkter, developer prenumerationer och användning dashboarding.</span><span class="sxs-lookup"><span data-stu-id="24285-111">This other guide, which this guide is based on, covers more topics, including notification configurations, tier pricing, response handling, user authentication, creating products, developer subscriptions, and usage dashboarding.</span></span>

## <a name="what-is-azureml"></a><span data-ttu-id="24285-112">Vad är AzureML?</span><span class="sxs-lookup"><span data-stu-id="24285-112">What is AzureML?</span></span>
<span data-ttu-id="24285-113">AzureML är en Azure-tjänst för maskininlärning där du kan enkelt skapa, distribuera och dela avancerade Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="24285-113">AzureML is an Azure service for machine learning that enables you to easily build, deploy, and share advanced analytics solutions.</span></span> <span data-ttu-id="24285-114">Klicka på [här](https://azure.microsoft.com/services/machine-learning/) information om AzureML.</span><span class="sxs-lookup"><span data-stu-id="24285-114">Click [here](https://azure.microsoft.com/services/machine-learning/) for details on AzureML.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24285-115">Krav</span><span class="sxs-lookup"><span data-stu-id="24285-115">Prerequisites</span></span>
<span data-ttu-id="24285-116">Du behöver följande för att slutföra den här guiden:</span><span class="sxs-lookup"><span data-stu-id="24285-116">To complete this guide, you need:</span></span>

* <span data-ttu-id="24285-117">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="24285-117">An Azure account.</span></span> <span data-ttu-id="24285-118">Om du inte har ett Azure-konto, klickar du på [här](https://azure.microsoft.com/pricing/free-trial/) mer information om hur du skapar ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="24285-118">If you don’t have an Azure account, click [here](https://azure.microsoft.com/pricing/free-trial/) for details on how to create a free trial account.</span></span>
* <span data-ttu-id="24285-119">AzureML-konto.</span><span class="sxs-lookup"><span data-stu-id="24285-119">An AzureML account.</span></span> <span data-ttu-id="24285-120">Om du inte har en AzureML-konto, klickar du på [här](https://studio.azureml.net/) mer information om hur du skapar ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="24285-120">If you don’t have an AzureML account, click [here](https://studio.azureml.net/) for details on how to create a free trial account.</span></span>
* <span data-ttu-id="24285-121">Arbetsytan, tjänst och api_key för en AzureML-experiment som distribueras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="24285-121">The workspace, service, and api_key for an AzureML experiment deployed as a web service.</span></span> <span data-ttu-id="24285-122">Klicka på [här](machine-learning-create-experiment.md) mer information om hur du skapar ett AzureML-experiment.</span><span class="sxs-lookup"><span data-stu-id="24285-122">Click [here](machine-learning-create-experiment.md) for details on how to create an AzureML experiment.</span></span> <span data-ttu-id="24285-123">Klicka på [här](machine-learning-publish-a-machine-learning-web-service.md) för information om hur du distribuerar en AzureML experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="24285-123">Click [here](machine-learning-publish-a-machine-learning-web-service.md) for details on how to deploy an AzureML experiment as a web service.</span></span> <span data-ttu-id="24285-124">Bilaga A har också anvisningar för hur du skapar och testar ett enkelt experiment AzureML och distribuera det som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="24285-124">Alternately, Appendix A has instructions for how to create and test a simple AzureML experiment and deploy it as a web service.</span></span>

## <a name="create-an-api-management-instance"></a><span data-ttu-id="24285-125">Skapa en API Management-instans</span><span class="sxs-lookup"><span data-stu-id="24285-125">Create an API Management instance</span></span>
<span data-ttu-id="24285-126">Nedan visas stegen för att hantera din AzureML-webbtjänsten med hjälp av API-hantering.</span><span class="sxs-lookup"><span data-stu-id="24285-126">Below are the steps for using API Management to manage your AzureML web service.</span></span> <span data-ttu-id="24285-127">Först skapa en instans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="24285-127">First create a service instance.</span></span> <span data-ttu-id="24285-128">Logga in på den [klassiska portalen](https://manage.windowsazure.com/) och på **ny** > **Apptjänster** > **API Management** > **skapa**.</span><span class="sxs-lookup"><span data-stu-id="24285-128">Log in to the [Classic Portal](https://manage.windowsazure.com/) and click **New** > **App Services** > **API Management** > **Create**.</span></span>

![Skapa instans](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

<span data-ttu-id="24285-130">Ange ett unikt **URL**.</span><span class="sxs-lookup"><span data-stu-id="24285-130">Specify a unique **URL**.</span></span> <span data-ttu-id="24285-131">Den här guiden använder **demoazureml** – måste du välja något annat.</span><span class="sxs-lookup"><span data-stu-id="24285-131">This guide uses **demoazureml** – you will need to choose something different.</span></span> <span data-ttu-id="24285-132">Välj önskad **prenumeration** och **region** för din tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="24285-132">Choose the desired **Subscription** and **Region** for your service instance.</span></span> <span data-ttu-id="24285-133">Klicka på Nästa när du har gjort dina val.</span><span class="sxs-lookup"><span data-stu-id="24285-133">After making your selections, click the next button.</span></span>

![skapa-tjänsten-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

<span data-ttu-id="24285-135">Ange ett värde för den **organisationsnamn**.</span><span class="sxs-lookup"><span data-stu-id="24285-135">Specify a value for the **Organization Name**.</span></span> <span data-ttu-id="24285-136">Den här guiden använder **demoazureml** – måste du välja något annat.</span><span class="sxs-lookup"><span data-stu-id="24285-136">This guide uses **demoazureml** – you will need to choose something different.</span></span> <span data-ttu-id="24285-137">Ange din e-postadress i den **administratör e-post** fältet.</span><span class="sxs-lookup"><span data-stu-id="24285-137">Enter your email address in the **administrator e-mail** field.</span></span> <span data-ttu-id="24285-138">Den här e-postadressen används för meddelanden från API Management-systemet.</span><span class="sxs-lookup"><span data-stu-id="24285-138">This email address is used for notifications from the API Management system.</span></span>

![skapa-tjänsten-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

<span data-ttu-id="24285-140">Markera kryssrutan om du vill skapa en tjänstinstans.</span><span class="sxs-lookup"><span data-stu-id="24285-140">Click the check box to create your service instance.</span></span> <span data-ttu-id="24285-141">*Det tar upp till 30 minuter innan en ny tjänst skapas*.</span><span class="sxs-lookup"><span data-stu-id="24285-141">*It takes up to thirty minutes for a new service to be created*.</span></span>

## <a name="create-the-api"></a><span data-ttu-id="24285-142">Skapa API: et</span><span class="sxs-lookup"><span data-stu-id="24285-142">Create the API</span></span>
<span data-ttu-id="24285-143">När instansen för tjänsten har skapats, är nästa steg att skapa API: et.</span><span class="sxs-lookup"><span data-stu-id="24285-143">Once the service instance is created, the next step is to create the API.</span></span> <span data-ttu-id="24285-144">Ett API består av en uppsättning åtgärder som kan anropas från ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="24285-144">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="24285-145">API-åtgärder körs via en proxy till befintliga webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="24285-145">API operations are proxied to existing web services.</span></span> <span data-ttu-id="24285-146">Den här guiden skapar API: er proxyinställningarna så att de befintliga AzureML RRS och BES-webbtjänsterna.</span><span class="sxs-lookup"><span data-stu-id="24285-146">This guide creates APIs that proxy to the existing AzureML RRS and BES web services.</span></span>

<span data-ttu-id="24285-147">API: er skapas och konfigureras från publisher-portalen API som öppnas via den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="24285-147">APIs are created and configured from the API publisher portal, which is accessed through the Azure Classic Portal.</span></span> <span data-ttu-id="24285-148">Välj service-instans för att nå publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="24285-148">To reach the publisher portal, select your service instance.</span></span>

![Välj tjänstinstans](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

<span data-ttu-id="24285-150">Klicka på **hantera** i den klassiska Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="24285-150">Click **Manage** in the Azure Classic Portal for your API Management service.</span></span>

![hantera service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

<span data-ttu-id="24285-152">Klicka på **API: er** från den **API Management** menyn till vänster och klicka sedan på **lägga till API**.</span><span class="sxs-lookup"><span data-stu-id="24285-152">Click **APIs** from the **API Management** menu on the left, and then click **Add API**.</span></span>

![management-API-menyn](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

<span data-ttu-id="24285-154">Typen **AzureML Demo API** som den **Web API-namnet**.</span><span class="sxs-lookup"><span data-stu-id="24285-154">Type **AzureML Demo API** as the **Web API name**.</span></span> <span data-ttu-id="24285-155">Typen **https://ussouthcentral.services.azureml.net** som den **-webbtjänstens URL**.</span><span class="sxs-lookup"><span data-stu-id="24285-155">Type **https://ussouthcentral.services.azureml.net** as the **Web service URL**.</span></span> <span data-ttu-id="24285-156">Typen **azureml-demo** som den **URL för Web API-suffix**.</span><span class="sxs-lookup"><span data-stu-id="24285-156">Type **azureml-demo** as the **Web API URL suffix**.</span></span> <span data-ttu-id="24285-157">Kontrollera **HTTPS** som den **URL för Web API** schema.</span><span class="sxs-lookup"><span data-stu-id="24285-157">Check **HTTPS** as the **Web API URL** scheme.</span></span> <span data-ttu-id="24285-158">Välj **Starter** som **produkter**.</span><span class="sxs-lookup"><span data-stu-id="24285-158">Select **Starter** as **Products**.</span></span> <span data-ttu-id="24285-159">När du är klar klickar du på **spara** att skapa API: et.</span><span class="sxs-lookup"><span data-stu-id="24285-159">When finished, click **Save** to create the API.</span></span>

![Lägg till-nya-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-the-operations"></a><span data-ttu-id="24285-161">Lägg till åtgärder</span><span class="sxs-lookup"><span data-stu-id="24285-161">Add the operations</span></span>
<span data-ttu-id="24285-162">Klicka på **lägga till åtgärden** att lägga till åtgärder i detta API.</span><span class="sxs-lookup"><span data-stu-id="24285-162">Click **Add operation** to add operations to this API.</span></span>

![lägga till åtgärden](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

<span data-ttu-id="24285-164">Den **nya åtgärden** visas och **signatur** fliken väljs som standard.</span><span class="sxs-lookup"><span data-stu-id="24285-164">The **New operation** window will be displayed and the **Signature** tab will be selected by default.</span></span>

## <a name="add-rrs-operation"></a><span data-ttu-id="24285-165">Lägg till RR-åtgärd</span><span class="sxs-lookup"><span data-stu-id="24285-165">Add RRS Operation</span></span>
<span data-ttu-id="24285-166">Först skapa en åtgärd för AzureML RRS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="24285-166">First create an operation for the AzureML RRS service.</span></span> <span data-ttu-id="24285-167">Välj **POST** som den **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="24285-167">Select **POST** as the **HTTP verb**.</span></span> <span data-ttu-id="24285-168">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} / execute? api-version = {apiversion} & information = {information}** som den **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="24285-168">Type **/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}** as the **URL template**.</span></span> <span data-ttu-id="24285-169">Typen **Resursposter köra** som den **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="24285-169">Type **RRS Execute** as the **Display name**.</span></span>

![Lägg till RR-åtgärden-signaturer](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

<span data-ttu-id="24285-171">Klicka på **svar** > **lägga till** till vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="24285-171">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="24285-172">Klicka på **spara** att spara den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24285-172">Click **Save** to save this operation.</span></span>

![Lägg till-RR-åtgärden-svar](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a><span data-ttu-id="24285-174">Lägga till BES-åtgärder</span><span class="sxs-lookup"><span data-stu-id="24285-174">Add BES Operations</span></span>
<span data-ttu-id="24285-175">Skärmbilder ingår inte för BES-åtgärder som de är mycket lik dem för att lägga till åtgärden Resursposter.</span><span class="sxs-lookup"><span data-stu-id="24285-175">Screenshots are not included for the BES operations as they are very similar to those for adding the RRS operation.</span></span>

### <a name="submit-but-not-start-a-batch-execution-job"></a><span data-ttu-id="24285-176">Skicka (men inte startar) ett Batch Execution-jobb</span><span class="sxs-lookup"><span data-stu-id="24285-176">Submit (but not start) a Batch Execution job</span></span>
<span data-ttu-id="24285-177">Klicka på **lägga till åtgärden** att lägga till åtgärden AzureML BES-API: et.</span><span class="sxs-lookup"><span data-stu-id="24285-177">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="24285-178">Välj **POST** för den **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="24285-178">Select **POST** for the **HTTP verb**.</span></span> <span data-ttu-id="24285-179">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} / jobb? api-version = {apiversion}** för den **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="24285-179">Type **/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="24285-180">Typen **BES skicka** för den **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="24285-180">Type **BES Submit** for the **Display name**.</span></span> <span data-ttu-id="24285-181">Klicka på **svar** > **lägga till** till vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="24285-181">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="24285-182">Klicka på **spara** att spara den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24285-182">Click **Save** to save this operation.</span></span>

### <a name="start-a-batch-execution-job"></a><span data-ttu-id="24285-183">Starta ett jobb i Batch Execution</span><span class="sxs-lookup"><span data-stu-id="24285-183">Start a Batch Execution job</span></span>
<span data-ttu-id="24285-184">Klicka på **lägga till åtgärden** att lägga till åtgärden AzureML BES-API: et.</span><span class="sxs-lookup"><span data-stu-id="24285-184">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="24285-185">Välj **POST** för den **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="24285-185">Select **POST** for the **HTTP verb**.</span></span> <span data-ttu-id="24285-186">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid} / start? api-version = {apiversion}** för den **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="24285-186">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="24285-187">Typen **BES starta** för den **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="24285-187">Type **BES Start** for the **Display name**.</span></span> <span data-ttu-id="24285-188">Klicka på **svar** > **lägga till** till vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="24285-188">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="24285-189">Klicka på **spara** att spara den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24285-189">Click **Save** to save this operation.</span></span>

### <a name="get-the-status-or-result-of-a-batch-execution-job"></a><span data-ttu-id="24285-190">Hämta status eller resultatet av ett Batch Execution-jobb</span><span class="sxs-lookup"><span data-stu-id="24285-190">Get the status or result of a Batch Execution job</span></span>
<span data-ttu-id="24285-191">Klicka på **lägga till åtgärden** att lägga till åtgärden AzureML BES-API: et.</span><span class="sxs-lookup"><span data-stu-id="24285-191">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="24285-192">Välj **hämta** för den **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="24285-192">Select **GET** for the **HTTP verb**.</span></span> <span data-ttu-id="24285-193">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid}? api-version = {apiversion}** för den **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="24285-193">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="24285-194">Typen **BES Status** för den **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="24285-194">Type **BES Status** for the **Display name**.</span></span> <span data-ttu-id="24285-195">Klicka på **svar** > **lägga till** till vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="24285-195">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="24285-196">Klicka på **spara** att spara den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24285-196">Click **Save** to save this operation.</span></span>

### <a name="delete-a-batch-execution-job"></a><span data-ttu-id="24285-197">Ta bort en Batch Execution-jobb</span><span class="sxs-lookup"><span data-stu-id="24285-197">Delete a Batch Execution job</span></span>
<span data-ttu-id="24285-198">Klicka på **lägga till åtgärden** att lägga till åtgärden AzureML BES-API: et.</span><span class="sxs-lookup"><span data-stu-id="24285-198">Click **add operation** to add the AzureML BES operation to the API.</span></span> <span data-ttu-id="24285-199">Välj **ta bort** för den **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="24285-199">Select **DELETE** for the **HTTP verb**.</span></span> <span data-ttu-id="24285-200">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid}? api-version = {apiversion}** för den **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="24285-200">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for the **URL template**.</span></span> <span data-ttu-id="24285-201">Typen **BES ta bort** för den **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="24285-201">Type **BES Delete** for the **Display name**.</span></span> <span data-ttu-id="24285-202">Klicka på **svar** > **lägga till** till vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="24285-202">Click **Responses** > **ADD** on the left and select **200 OK**.</span></span> <span data-ttu-id="24285-203">Klicka på **spara** att spara den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24285-203">Click **Save** to save this operation.</span></span>

## <a name="call-an-operation-from-the-developer-portal"></a><span data-ttu-id="24285-204">Anropa en åtgärd från utvecklarportalen</span><span class="sxs-lookup"><span data-stu-id="24285-204">Call an operation from the Developer Portal</span></span>
<span data-ttu-id="24285-205">Åtgärder kan anropas direkt från Developer-portalen, vilket ger ett bekvämt sätt att visa och testa driften av ett API.</span><span class="sxs-lookup"><span data-stu-id="24285-205">Operations can be called directly from the Developer portal, which provides a convenient way to view and test the operations of an API.</span></span> <span data-ttu-id="24285-206">I det här steget i guiden kommer du anropar den **Resursposter köra** metod som har lagts till i den **AzureML Demo API**.</span><span class="sxs-lookup"><span data-stu-id="24285-206">In this guide step you will call the **RRS Execute** method that was added to the **AzureML Demo API**.</span></span> <span data-ttu-id="24285-207">Klicka på **Developer-portalen** på menyn längst upp i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="24285-207">Click **Developer portal** from the menu at the top right of the Classic Portal.</span></span>

![Developer-portalen](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

<span data-ttu-id="24285-209">Klicka på **API: er** från den översta menyn och klicka sedan på **AzureML Demo API** att se åtgärderna som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="24285-209">Click **APIs** from the top menu, and then click **AzureML Demo API** to see the operations available.</span></span>

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

<span data-ttu-id="24285-211">Välj **Resursposter köra** för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24285-211">Select **RRS Execute** for the operation.</span></span> <span data-ttu-id="24285-212">Klicka på **prova**.</span><span class="sxs-lookup"><span data-stu-id="24285-212">Click **Try It**.</span></span>

![Försök it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

<span data-ttu-id="24285-214">De begäranparametrarna, Skriv din **arbetsytan**, **service**, **2.0** för den **apiversion**, och **SANT** för den **information**.</span><span class="sxs-lookup"><span data-stu-id="24285-214">For Request parameters, type your **workspace**,  **service**, **2.0** for the **apiversion**, and  **true** for the **details**.</span></span> <span data-ttu-id="24285-215">Du hittar din **arbetsytan** och **service** i instrumentpanelen för AzureML web service (se **testa webbtjänsten** i bilaga A).</span><span class="sxs-lookup"><span data-stu-id="24285-215">You can find your **workspace** and **service** in the AzureML web service dashboard (see **Test the web service** in Appendix A).</span></span>

<span data-ttu-id="24285-216">Huvuden för begäran, klickar du på **Lägg till sidhuvud** och skriv **Content-Type** och **application/json**, klicka på **Lägg till sidhuvud** och skriv **auktorisering** och **ägar <YOUR AZUREML SERVICE API-KEY>** .</span><span class="sxs-lookup"><span data-stu-id="24285-216">For Request headers, click **Add header** and type **Content-Type** and **application/json**, then click **Add header** and type **Authorization** and **Bearer <YOUR AZUREML SERVICE API-KEY>**.</span></span> <span data-ttu-id="24285-217">Du kan hitta din **api-nyckel** i instrumentpanelen för AzureML web service (se **testa webbtjänsten** i bilaga A).</span><span class="sxs-lookup"><span data-stu-id="24285-217">You can find your **api key** in the AzureML web service dashboard (see **Test the web service** in Appendix A).</span></span>

<span data-ttu-id="24285-218">Typen **{”indata”: {”input1”: {”ColumnNames”: [”Col2”], ”värden”: [[”det här är en bra dag”]]}}, ”GlobalParameters”: {}}** för begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="24285-218">Type **{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}** for the request body.</span></span>

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

<span data-ttu-id="24285-220">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="24285-220">Click **Send**.</span></span>

![Skicka](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

<span data-ttu-id="24285-222">När en åtgärd har anropats developer-portalen visar den **begärda URL: en** från backend-tjänsten, den **svarsstatusen**, **svarshuvuden**, och en **svar innehåll**.</span><span class="sxs-lookup"><span data-stu-id="24285-222">After an operation is invoked, the developer portal displays the **Requested URL** from the back-end service, the **Response status**, the **Response headers**, and any **Response content**.</span></span>

![Response-status](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a><span data-ttu-id="24285-224">Bilaga A – skapa och testa en enkel AzureML webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="24285-224">Appendix A - Creating and testing a simple AzureML web service</span></span>
### <a name="creating-the-experiment"></a><span data-ttu-id="24285-225">Skapa experimentet</span><span class="sxs-lookup"><span data-stu-id="24285-225">Creating the experiment</span></span>
<span data-ttu-id="24285-226">Nedan följer du stegen för att skapa ett enkelt experiment AzureML och distribuera den som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="24285-226">Below are the steps for creating a simple AzureML experiment and deploying it as a web service.</span></span> <span data-ttu-id="24285-227">Tar för web-tjänst som indata för en kolumn med valfri text och returnerar en uppsättning funktioner som visas i form av heltal.</span><span class="sxs-lookup"><span data-stu-id="24285-227">The web service takes as input a column of arbitrary text and returns a set of features represented as integers.</span></span> <span data-ttu-id="24285-228">Exempel:</span><span class="sxs-lookup"><span data-stu-id="24285-228">For example:</span></span>

| <span data-ttu-id="24285-229">Text</span><span class="sxs-lookup"><span data-stu-id="24285-229">Text</span></span> | <span data-ttu-id="24285-230">Hashformaterats Text</span><span class="sxs-lookup"><span data-stu-id="24285-230">Hashed Text</span></span> |
| --- | --- |
| <span data-ttu-id="24285-231">Det här är en bra dag</span><span class="sxs-lookup"><span data-stu-id="24285-231">This is a good day</span></span> |<span data-ttu-id="24285-232">1 1 2 2 0 2 0 1</span><span class="sxs-lookup"><span data-stu-id="24285-232">1 1 2 2 0 2 0 1</span></span> |

<span data-ttu-id="24285-233">Börja med en webbläsare, navigera till: [https://studio.azureml.net/](https://studio.azureml.net/) och ange dina autentiseringsuppgifter för att logga in.</span><span class="sxs-lookup"><span data-stu-id="24285-233">First, using a browser of your choice, navigate to: [https://studio.azureml.net/](https://studio.azureml.net/) and enter your credentials to log in.</span></span> <span data-ttu-id="24285-234">Skapa sedan ett nytt tomt experiment.</span><span class="sxs-lookup"><span data-stu-id="24285-234">Next, create a new blank experiment.</span></span>

![Sök-experiment-mallar](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

<span data-ttu-id="24285-236">Byt namn på den till **SimpleFeatureHashingExperiment**.</span><span class="sxs-lookup"><span data-stu-id="24285-236">Rename it to **SimpleFeatureHashingExperiment**.</span></span> <span data-ttu-id="24285-237">Expandera **sparade datauppsättningar** och dra **Book granskningar från Amazon** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-237">Expand **Saved Datasets** and drag **Book Reviews from Amazon** onto your experiment.</span></span>

![enkel-funktionen-hash-experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

<span data-ttu-id="24285-239">Expandera **Dataomvandling** och **manipulering** och dra **Välj kolumner i datauppsättning** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-239">Expand **Data Transformation** and **Manipulation** and drag **Select Columns in Dataset** onto your experiment.</span></span> <span data-ttu-id="24285-240">Ansluta **bok granskningar från Amazon** till **Välj kolumner i datauppsättning**.</span><span class="sxs-lookup"><span data-stu-id="24285-240">Connect **Book Reviews from Amazon** to **Select Columns in Dataset**.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

<span data-ttu-id="24285-242">Klicka på **Välj kolumner i datauppsättning** och klicka sedan på **starta kolumnväljaren** och välj **Col2**.</span><span class="sxs-lookup"><span data-stu-id="24285-242">Click **Select Columns in Dataset** and then click **Launch column selector** and select **Col2**.</span></span> <span data-ttu-id="24285-243">Klicka på bockmarkeringen för att tillämpa ändringarna.</span><span class="sxs-lookup"><span data-stu-id="24285-243">Click the checkmark to apply these changes.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

<span data-ttu-id="24285-245">Expandera **textanalys** och dra **funktions-hashning** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-245">Expand **Text Analytics** and drag **Feature Hashing** onto the experiment.</span></span> <span data-ttu-id="24285-246">Ansluta **Välj kolumner i datauppsättning** till **hash-funktionen**.</span><span class="sxs-lookup"><span data-stu-id="24285-246">Connect **Select Columns in Dataset** to **Feature Hashing**.</span></span>

![ansluta projektkolumner](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

<span data-ttu-id="24285-248">Typen **3** för den **Hashing bitsize**.</span><span class="sxs-lookup"><span data-stu-id="24285-248">Type **3** for the **Hashing bitsize**.</span></span> <span data-ttu-id="24285-249">Detta skapar 8 (23) kolumner.</span><span class="sxs-lookup"><span data-stu-id="24285-249">This will create 8 (23) columns.</span></span>

![hash-bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

<span data-ttu-id="24285-251">Du kan nu vill klickar du på **kör** att testa experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-251">At this point, you may want to click **Run** to test the experiment.</span></span>

![Kör](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a><span data-ttu-id="24285-253">Skapa en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="24285-253">Create a web service</span></span>
<span data-ttu-id="24285-254">Nu skapa en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="24285-254">Now create a web service.</span></span> <span data-ttu-id="24285-255">Expandera **webbtjänsten** och dra **indata** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-255">Expand **Web Service** and drag **Input** onto your experiment.</span></span> <span data-ttu-id="24285-256">Ansluta **indata** till **hash-funktionen**.</span><span class="sxs-lookup"><span data-stu-id="24285-256">Connect **Input** to **Feature Hashing**.</span></span> <span data-ttu-id="24285-257">Också dra **utdata** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-257">Also drag **output** onto your experiment.</span></span> <span data-ttu-id="24285-258">Ansluta **utdata** till **hash-funktionen**.</span><span class="sxs-lookup"><span data-stu-id="24285-258">Connect **Output** to **Feature Hashing**.</span></span>

![utdata-till-funktionen för-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

<span data-ttu-id="24285-260">Klicka på **publicerar webbtjänst**.</span><span class="sxs-lookup"><span data-stu-id="24285-260">Click **Publish web service**.</span></span>

![Publicera webbtjänst](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

<span data-ttu-id="24285-262">Klicka på **Ja** att publicera experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-262">Click **Yes** to publish the experiment.</span></span>

![Ja för att publicera](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-the-web-service"></a><span data-ttu-id="24285-264">Testa webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="24285-264">Test the web service</span></span>
<span data-ttu-id="24285-265">En AzureML-webbtjänst består av RSS-(begäranden/svar-tjänsten) och slutpunkter för BES (batch execution service).</span><span class="sxs-lookup"><span data-stu-id="24285-265">An AzureML web service consists of RSS (request/response service) and BES (batch execution service) endpoints.</span></span> <span data-ttu-id="24285-266">RSS är för synkron körning.</span><span class="sxs-lookup"><span data-stu-id="24285-266">RSS is for synchronous execution.</span></span> <span data-ttu-id="24285-267">BES är asynkron jobbkörningen.</span><span class="sxs-lookup"><span data-stu-id="24285-267">BES is for asynchronous job execution.</span></span> <span data-ttu-id="24285-268">Om du vill testa webbtjänsten med exempel Python källan nedan, du kan behöva hämta och installera Azure SDK för Python (se: [hur du installerar Python](../python-how-to-install.md)).</span><span class="sxs-lookup"><span data-stu-id="24285-268">To test your web service with the sample Python source below, you may need to download and install the Azure SDK for Python (see: [How to install Python](../python-how-to-install.md)).</span></span>

<span data-ttu-id="24285-269">Du måste också den **arbetsytan**, **service**, och **api_key** av experimentet för källan exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="24285-269">You will also need the **workspace**, **service**, and **api_key** of your experiment for the sample source below.</span></span> <span data-ttu-id="24285-270">Du kan hitta arbetsytan och tjänsten genom att klicka på antingen **frågor och svar** eller **Batch Execution** för experimentet web service-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="24285-270">You can find the workspace and service by clicking either **Request/Response** or **Batch Execution** for your experiment in the web service dashboard.</span></span>

![Sök-arbetsytan-och-tjänst](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

<span data-ttu-id="24285-272">Du hittar den **api_key** genom att klicka på experimentet web service-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="24285-272">You can find the **api_key** by clicking your experiment in the web service dashboard.</span></span>

![Sök-api-nyckel](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a><span data-ttu-id="24285-274">Testa Resursposter slutpunkt</span><span class="sxs-lookup"><span data-stu-id="24285-274">Test RRS endpoint</span></span>
##### <a name="test-button"></a><span data-ttu-id="24285-275">Knappen Testa</span><span class="sxs-lookup"><span data-stu-id="24285-275">Test button</span></span>
<span data-ttu-id="24285-276">Ett enkelt sätt att testa RR-slutpunkten är att klicka på **testa** på instrumentpanelen web service.</span><span class="sxs-lookup"><span data-stu-id="24285-276">An easy way to test the RRS endpoint is to click **Test** on the web service dashboard.</span></span>

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

<span data-ttu-id="24285-278">Typen **detta är en bra dag** för **col2**.</span><span class="sxs-lookup"><span data-stu-id="24285-278">Type **This is a good day** for **col2**.</span></span> <span data-ttu-id="24285-279">Klicka på bockmarkeringen.</span><span class="sxs-lookup"><span data-stu-id="24285-279">Click the checkmark.</span></span>

![Ange data](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

<span data-ttu-id="24285-281">Du ser något som liknar</span><span class="sxs-lookup"><span data-stu-id="24285-281">You will see something like</span></span>

![exempel på-utdata](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a><span data-ttu-id="24285-283">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="24285-283">Sample Code</span></span>
<span data-ttu-id="24285-284">Ett annat sätt att testa din RRS är från din klientkod.</span><span class="sxs-lookup"><span data-stu-id="24285-284">Another way to test your RRS is from your client code.</span></span> <span data-ttu-id="24285-285">Om du klickar på **frågor och svar** på instrumentpanelen och rullar du längst ned visas exempelkod för C#, Python och R. Du kan även se syntaxen för RRS-förfrågan, inklusive URI, rubriker och brödtext.</span><span class="sxs-lookup"><span data-stu-id="24285-285">If you click **Request/response** on the dashboard and scroll to the bottom, you will see sample code for C#, Python, and R. You will also see the syntax of the RRS request, including the request URI, headers, and body.</span></span>

<span data-ttu-id="24285-286">Den här guiden visar en fungerande Python exempel.</span><span class="sxs-lookup"><span data-stu-id="24285-286">This guide shows a working Python example.</span></span> <span data-ttu-id="24285-287">Du behöver ändra den med den **arbetsytan**, **service**, och **api_key** av experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-287">You will need to modify it with the **workspace**, **service**, and **api_key** of your experiment.</span></span>

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a><span data-ttu-id="24285-288">Testa BES slutpunkt</span><span class="sxs-lookup"><span data-stu-id="24285-288">Test BES endpoint</span></span>
<span data-ttu-id="24285-289">Klicka på **Batch-körningen** på instrumentpanelen och rullar du längst ned.</span><span class="sxs-lookup"><span data-stu-id="24285-289">Click **Batch execution** on the dashboard and scroll to the bottom.</span></span> <span data-ttu-id="24285-290">Du kommer att se exempelkod för C#, Python och R. Du kan även se syntaxen för BES-förfrågningar till skickar ett jobb, starta ett jobb, status eller resultatet av ett jobb och ta bort ett jobb.</span><span class="sxs-lookup"><span data-stu-id="24285-290">You will see sample code for C#, Python, and R. You will also see the syntax of the BES requests to submit a job, start a job, get the status or results of a job, and delete a job.</span></span>

<span data-ttu-id="24285-291">Den här guiden visar en fungerande Python exempel.</span><span class="sxs-lookup"><span data-stu-id="24285-291">This guide shows a working Python example.</span></span> <span data-ttu-id="24285-292">Du behöver ändra den med den **arbetsytan**, **service**, och **api_key** av experimentet.</span><span class="sxs-lookup"><span data-stu-id="24285-292">You need to modify it with the **workspace**, **service**, and **api_key** of your experiment.</span></span> <span data-ttu-id="24285-293">Dessutom måste du ändra den **lagringskontonamnet**, **lagringskontonyckel**, och **namnet på lagringsbehållaren**.</span><span class="sxs-lookup"><span data-stu-id="24285-293">Additionally, you need to modify the **storage account name**, **storage account key**, and **storage container name**.</span></span> <span data-ttu-id="24285-294">Till sist ska du behöver ändra platsen för den **indatafilen** och platsen för den **utdatafilen**.</span><span class="sxs-lookup"><span data-stu-id="24285-294">Lastly, you will need to modify the location of the **input file** and the location of the **output file**.</span></span>

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
