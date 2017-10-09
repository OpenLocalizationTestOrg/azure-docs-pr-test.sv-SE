---
title: "aaaLearn hur toomanage AzureML web services med hjälp av API-hantering | Microsoft Docs"
description: "En guide som visar hur toomanage AzureML web services med hjälp av API-hantering."
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
ms.openlocfilehash: 6e764fbfd71be6cc908a1c8d3d8889969fc651a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-how-toomanage-azureml-web-services-using-api-management"></a><span data-ttu-id="a5938-104">Lär dig hur toomanage AzureML web services med hjälp av API-hantering</span><span class="sxs-lookup"><span data-stu-id="a5938-104">Learn how toomanage AzureML web services using API Management</span></span>
## <a name="overview"></a><span data-ttu-id="a5938-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="a5938-105">Overview</span></span>
<span data-ttu-id="a5938-106">Den här guiden visar hur tooquickly komma igång med API Management toomanage AzureML-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="a5938-106">This guide shows you how tooquickly get started using API Management toomanage your AzureML web services.</span></span>

## <a name="what-is-azure-api-management"></a><span data-ttu-id="a5938-107">Vad är Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="a5938-107">What is Azure API Management?</span></span>
<span data-ttu-id="a5938-108">Azure API Management är en Azure-tjänst som låter dig hantera REST API-slutpunkter genom att definiera användaråtkomst, begränsning och instrumentpanelen för övervakning.</span><span class="sxs-lookup"><span data-stu-id="a5938-108">Azure API Management is an Azure service that lets you manage your REST API endpoints by defining user access, usage throttling, and dashboard monitoring.</span></span> <span data-ttu-id="a5938-109">Klicka på [här](https://azure.microsoft.com/services/api-management/) information om Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="a5938-109">Click [here](https://azure.microsoft.com/services/api-management/) for details on Azure API Management.</span></span> <span data-ttu-id="a5938-110">Klicka på [här](../api-management/api-management-get-started.md) för en översikt över hur tooget igång med Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="a5938-110">Click [here](../api-management/api-management-get-started.md) for a guide on how tooget started with Azure API Management.</span></span> <span data-ttu-id="a5938-111">Den här andra guiden som den här guiden bygger på omfattar flera ämnen, t.ex. meddelande konfigurationer, nivå prissättning, svar hantering, autentisering av användare, skapa produkter, developer prenumerationer och användning dashboarding.</span><span class="sxs-lookup"><span data-stu-id="a5938-111">This other guide, which this guide is based on, covers more topics, including notification configurations, tier pricing, response handling, user authentication, creating products, developer subscriptions, and usage dashboarding.</span></span>

## <a name="what-is-azureml"></a><span data-ttu-id="a5938-112">Vad är AzureML?</span><span class="sxs-lookup"><span data-stu-id="a5938-112">What is AzureML?</span></span>
<span data-ttu-id="a5938-113">AzureML är en Azure-tjänst för machine learning som gör att du tooeasily skapa, distribuera och dela avancerade Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="a5938-113">AzureML is an Azure service for machine learning that enables you tooeasily build, deploy, and share advanced analytics solutions.</span></span> <span data-ttu-id="a5938-114">Klicka på [här](https://azure.microsoft.com/services/machine-learning/) information om AzureML.</span><span class="sxs-lookup"><span data-stu-id="a5938-114">Click [here](https://azure.microsoft.com/services/machine-learning/) for details on AzureML.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5938-115">Krav</span><span class="sxs-lookup"><span data-stu-id="a5938-115">Prerequisites</span></span>
<span data-ttu-id="a5938-116">toocomplete den här guiden behöver du:</span><span class="sxs-lookup"><span data-stu-id="a5938-116">toocomplete this guide, you need:</span></span>

* <span data-ttu-id="a5938-117">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a5938-117">An Azure account.</span></span> <span data-ttu-id="a5938-118">Om du inte har ett Azure-konto, klickar du på [här](https://azure.microsoft.com/pricing/free-trial/) mer information om hur toocreate ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="a5938-118">If you don’t have an Azure account, click [here](https://azure.microsoft.com/pricing/free-trial/) for details on how toocreate a free trial account.</span></span>
* <span data-ttu-id="a5938-119">AzureML-konto.</span><span class="sxs-lookup"><span data-stu-id="a5938-119">An AzureML account.</span></span> <span data-ttu-id="a5938-120">Om du inte har en AzureML-konto, klickar du på [här](https://studio.azureml.net/) mer information om hur toocreate ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="a5938-120">If you don’t have an AzureML account, click [here](https://studio.azureml.net/) for details on how toocreate a free trial account.</span></span>
* <span data-ttu-id="a5938-121">hello arbetsytan, tjänst och api_key för en AzureML-experiment som distribueras som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a5938-121">hello workspace, service, and api_key for an AzureML experiment deployed as a web service.</span></span> <span data-ttu-id="a5938-122">Klicka på [här](machine-learning-create-experiment.md) för information om hur toocreate en AzureML experiment.</span><span class="sxs-lookup"><span data-stu-id="a5938-122">Click [here](machine-learning-create-experiment.md) for details on how toocreate an AzureML experiment.</span></span> <span data-ttu-id="a5938-123">Klicka på [här](machine-learning-publish-a-machine-learning-web-service.md) för information om hur toodeploy en AzureML experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a5938-123">Click [here](machine-learning-publish-a-machine-learning-web-service.md) for details on how toodeploy an AzureML experiment as a web service.</span></span> <span data-ttu-id="a5938-124">Bilaga A har också instruktioner för hur toocreate och testa en enkel AzureML experimentera och distribuera det som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a5938-124">Alternately, Appendix A has instructions for how toocreate and test a simple AzureML experiment and deploy it as a web service.</span></span>

## <a name="create-an-api-management-instance"></a><span data-ttu-id="a5938-125">Skapa en API Management-instans</span><span class="sxs-lookup"><span data-stu-id="a5938-125">Create an API Management instance</span></span>
<span data-ttu-id="a5938-126">Nedan finns hello steg för att använda API Management toomanage AzureML webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a5938-126">Below are hello steps for using API Management toomanage your AzureML web service.</span></span> <span data-ttu-id="a5938-127">Först skapa en instans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a5938-127">First create a service instance.</span></span> <span data-ttu-id="a5938-128">Logga in toohello [klassiska portalen](https://manage.windowsazure.com/) och på **ny** > **Apptjänster** > **API Management**  >  **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a5938-128">Log in toohello [Classic Portal](https://manage.windowsazure.com/) and click **New** > **App Services** > **API Management** > **Create**.</span></span>

![Skapa instans](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-instance.png)

<span data-ttu-id="a5938-130">Ange ett unikt **URL**.</span><span class="sxs-lookup"><span data-stu-id="a5938-130">Specify a unique **URL**.</span></span> <span data-ttu-id="a5938-131">Den här guiden använder **demoazureml** – du behöver toochoose någonting annat.</span><span class="sxs-lookup"><span data-stu-id="a5938-131">This guide uses **demoazureml** – you will need toochoose something different.</span></span> <span data-ttu-id="a5938-132">Välj önskad hello **prenumeration** och **Region** för service-instans.</span><span class="sxs-lookup"><span data-stu-id="a5938-132">Choose hello desired **Subscription** and **Region** for your service instance.</span></span> <span data-ttu-id="a5938-133">När du har gjort dina val klickar du på knappen för nästa hello.</span><span class="sxs-lookup"><span data-stu-id="a5938-133">After making your selections, click hello next button.</span></span>

![skapa-tjänsten-1](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-1.png)

<span data-ttu-id="a5938-135">Ange ett värde för hello **organisationsnamn**.</span><span class="sxs-lookup"><span data-stu-id="a5938-135">Specify a value for hello **Organization Name**.</span></span> <span data-ttu-id="a5938-136">Den här guiden använder **demoazureml** – du behöver toochoose någonting annat.</span><span class="sxs-lookup"><span data-stu-id="a5938-136">This guide uses **demoazureml** – you will need toochoose something different.</span></span> <span data-ttu-id="a5938-137">Ange din e-postadress i hello **administratör e-post** fältet.</span><span class="sxs-lookup"><span data-stu-id="a5938-137">Enter your email address in hello **administrator e-mail** field.</span></span> <span data-ttu-id="a5938-138">E-postadressen används för meddelanden från hello API Management-systemet.</span><span class="sxs-lookup"><span data-stu-id="a5938-138">This email address is used for notifications from hello API Management system.</span></span>

![skapa-tjänsten-2](./media/machine-learning-manage-web-service-endpoints-using-api-management/create-service-2.png)

<span data-ttu-id="a5938-140">Klicka på hello kryssrutan toocreate service-instans.</span><span class="sxs-lookup"><span data-stu-id="a5938-140">Click hello check box toocreate your service instance.</span></span> <span data-ttu-id="a5938-141">*Den tar toothirty minuter för en ny service toobe skapade*.</span><span class="sxs-lookup"><span data-stu-id="a5938-141">*It takes up toothirty minutes for a new service toobe created*.</span></span>

## <a name="create-hello-api"></a><span data-ttu-id="a5938-142">Skapa hello-API</span><span class="sxs-lookup"><span data-stu-id="a5938-142">Create hello API</span></span>
<span data-ttu-id="a5938-143">När du har skapat hello tjänstinstans är hello nästa steg toocreate hello API.</span><span class="sxs-lookup"><span data-stu-id="a5938-143">Once hello service instance is created, hello next step is toocreate hello API.</span></span> <span data-ttu-id="a5938-144">Ett API består av en uppsättning åtgärder som kan anropas från ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="a5938-144">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="a5938-145">API: et är via proxy tooexisting webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="a5938-145">API operations are proxied tooexisting web services.</span></span> <span data-ttu-id="a5938-146">Den här guiden skapar API: er som proxy toohello befintliga AzureML RRS och BES webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="a5938-146">This guide creates APIs that proxy toohello existing AzureML RRS and BES web services.</span></span>

<span data-ttu-id="a5938-147">API: er skapas och konfigureras från hello API publisher-portalen som öppnas via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5938-147">APIs are created and configured from hello API publisher portal, which is accessed through hello Azure Classic Portal.</span></span> <span data-ttu-id="a5938-148">tooreach hello publisher portal, väljer din service-instans.</span><span class="sxs-lookup"><span data-stu-id="a5938-148">tooreach hello publisher portal, select your service instance.</span></span>

![Välj tjänstinstans](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-service-instance.png)

<span data-ttu-id="a5938-150">Klicka på **hantera** i hello klassiska Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a5938-150">Click **Manage** in hello Azure Classic Portal for your API Management service.</span></span>

![hantera service](./media/machine-learning-manage-web-service-endpoints-using-api-management/manage-service.png)

<span data-ttu-id="a5938-152">Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **lägga till API**.</span><span class="sxs-lookup"><span data-stu-id="a5938-152">Click **APIs** from hello **API Management** menu on hello left, and then click **Add API**.</span></span>

![management-API-menyn](./media/machine-learning-manage-web-service-endpoints-using-api-management/api-management-menu.png)

<span data-ttu-id="a5938-154">Typen **AzureML Demo API** som hello **Web API-namnet**.</span><span class="sxs-lookup"><span data-stu-id="a5938-154">Type **AzureML Demo API** as hello **Web API name**.</span></span> <span data-ttu-id="a5938-155">Typen **https://ussouthcentral.services.azureml.net** som hello **-webbtjänstens URL**.</span><span class="sxs-lookup"><span data-stu-id="a5938-155">Type **https://ussouthcentral.services.azureml.net** as hello **Web service URL**.</span></span> <span data-ttu-id="a5938-156">Typen **azureml-demo** som hello **URL för Web API-suffix**.</span><span class="sxs-lookup"><span data-stu-id="a5938-156">Type **azureml-demo** as hello **Web API URL suffix**.</span></span> <span data-ttu-id="a5938-157">Kontrollera **HTTPS** som hello **URL för Web API** schema.</span><span class="sxs-lookup"><span data-stu-id="a5938-157">Check **HTTPS** as hello **Web API URL** scheme.</span></span> <span data-ttu-id="a5938-158">Välj **Starter** som **produkter**.</span><span class="sxs-lookup"><span data-stu-id="a5938-158">Select **Starter** as **Products**.</span></span> <span data-ttu-id="a5938-159">När du är klar klickar du på **spara** toocreate hello API.</span><span class="sxs-lookup"><span data-stu-id="a5938-159">When finished, click **Save** toocreate hello API.</span></span>

![Lägg till-nya-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-hello-operations"></a><span data-ttu-id="a5938-161">Lägga till hello åtgärder</span><span class="sxs-lookup"><span data-stu-id="a5938-161">Add hello operations</span></span>
<span data-ttu-id="a5938-162">Klicka på **lägga till åtgärden** tooadd operations toothis API.</span><span class="sxs-lookup"><span data-stu-id="a5938-162">Click **Add operation** tooadd operations toothis API.</span></span>

![lägga till åtgärden](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-operation.png)

<span data-ttu-id="a5938-164">Hej **nya åtgärden** fönster visas och hello **signatur** fliken väljs som standard.</span><span class="sxs-lookup"><span data-stu-id="a5938-164">hello **New operation** window will be displayed and hello **Signature** tab will be selected by default.</span></span>

## <a name="add-rrs-operation"></a><span data-ttu-id="a5938-165">Lägg till RR-åtgärd</span><span class="sxs-lookup"><span data-stu-id="a5938-165">Add RRS Operation</span></span>
<span data-ttu-id="a5938-166">Först skapa en åtgärd för hello AzureML RRS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a5938-166">First create an operation for hello AzureML RRS service.</span></span> <span data-ttu-id="a5938-167">Välj **POST** som hello **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="a5938-167">Select **POST** as hello **HTTP verb**.</span></span> <span data-ttu-id="a5938-168">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} / execute? api-version = {apiversion} & information = {information}** som hello **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-168">Type **/workspaces/{workspace}/services/{service}/execute?api-version={apiversion}&details={details}** as hello **URL template**.</span></span> <span data-ttu-id="a5938-169">Typen **Resursposter köra** som hello **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="a5938-169">Type **RRS Execute** as hello **Display name**.</span></span>

![Lägg till RR-åtgärden-signaturer](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

<span data-ttu-id="a5938-171">Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="a5938-171">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="a5938-172">Klicka på **spara** toosave den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a5938-172">Click **Save** toosave this operation.</span></span>

![Lägg till-RR-åtgärden-svar](./media/machine-learning-manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a><span data-ttu-id="a5938-174">Lägga till BES-åtgärder</span><span class="sxs-lookup"><span data-stu-id="a5938-174">Add BES Operations</span></span>
<span data-ttu-id="a5938-175">Skärmbilder anges inte för hello BES åtgärder som de är mycket lik toothose för att lägga till hello Resursposter igen.</span><span class="sxs-lookup"><span data-stu-id="a5938-175">Screenshots are not included for hello BES operations as they are very similar toothose for adding hello RRS operation.</span></span>

### <a name="submit-but-not-start-a-batch-execution-job"></a><span data-ttu-id="a5938-176">Skicka (men inte startar) ett Batch Execution-jobb</span><span class="sxs-lookup"><span data-stu-id="a5938-176">Submit (but not start) a Batch Execution job</span></span>
<span data-ttu-id="a5938-177">Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API.</span><span class="sxs-lookup"><span data-stu-id="a5938-177">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="a5938-178">Välj **POST** för hello **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="a5938-178">Select **POST** for hello **HTTP verb**.</span></span> <span data-ttu-id="a5938-179">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} / jobb? api-version = {apiversion}** för hello **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-179">Type **/workspaces/{workspace}/services/{service}/jobs?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="a5938-180">Typen **BES skicka** för hello **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="a5938-180">Type **BES Submit** for hello **Display name**.</span></span> <span data-ttu-id="a5938-181">Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="a5938-181">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="a5938-182">Klicka på **spara** toosave den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a5938-182">Click **Save** toosave this operation.</span></span>

### <a name="start-a-batch-execution-job"></a><span data-ttu-id="a5938-183">Starta ett jobb i Batch Execution</span><span class="sxs-lookup"><span data-stu-id="a5938-183">Start a Batch Execution job</span></span>
<span data-ttu-id="a5938-184">Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API.</span><span class="sxs-lookup"><span data-stu-id="a5938-184">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="a5938-185">Välj **POST** för hello **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="a5938-185">Select **POST** for hello **HTTP verb**.</span></span> <span data-ttu-id="a5938-186">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid} / start? api-version = {apiversion}** för hello **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-186">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}/start?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="a5938-187">Typen **BES starta** för hello **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="a5938-187">Type **BES Start** for hello **Display name**.</span></span> <span data-ttu-id="a5938-188">Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="a5938-188">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="a5938-189">Klicka på **spara** toosave den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a5938-189">Click **Save** toosave this operation.</span></span>

### <a name="get-hello-status-or-result-of-a-batch-execution-job"></a><span data-ttu-id="a5938-190">Hämta hello status eller resultatet av ett Batch Execution-jobb</span><span class="sxs-lookup"><span data-stu-id="a5938-190">Get hello status or result of a Batch Execution job</span></span>
<span data-ttu-id="a5938-191">Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API.</span><span class="sxs-lookup"><span data-stu-id="a5938-191">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="a5938-192">Välj **hämta** för hello **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="a5938-192">Select **GET** for hello **HTTP verb**.</span></span> <span data-ttu-id="a5938-193">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid}? api-version = {apiversion}** för hello **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-193">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="a5938-194">Typen **BES Status** för hello **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="a5938-194">Type **BES Status** for hello **Display name**.</span></span> <span data-ttu-id="a5938-195">Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="a5938-195">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="a5938-196">Klicka på **spara** toosave den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a5938-196">Click **Save** toosave this operation.</span></span>

### <a name="delete-a-batch-execution-job"></a><span data-ttu-id="a5938-197">Ta bort en Batch Execution-jobb</span><span class="sxs-lookup"><span data-stu-id="a5938-197">Delete a Batch Execution job</span></span>
<span data-ttu-id="a5938-198">Klicka på **lägga till åtgärden** tooadd hello AzureML BES åtgärden toohello API.</span><span class="sxs-lookup"><span data-stu-id="a5938-198">Click **add operation** tooadd hello AzureML BES operation toohello API.</span></span> <span data-ttu-id="a5938-199">Välj **ta bort** för hello **HTTP-verbet**.</span><span class="sxs-lookup"><span data-stu-id="a5938-199">Select **DELETE** for hello **HTTP verb**.</span></span> <span data-ttu-id="a5938-200">Typen **/workspaces/ {arbetsytan} /services/ {tjänsten} /jobs/ {jobid}? api-version = {apiversion}** för hello **URL mallen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-200">Type **/workspaces/{workspace}/services/{service}/jobs/{jobid}?api-version={apiversion}** for hello **URL template**.</span></span> <span data-ttu-id="a5938-201">Typen **BES ta bort** för hello **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="a5938-201">Type **BES Delete** for hello **Display name**.</span></span> <span data-ttu-id="a5938-202">Klicka på **svar** > **lägga till** på hello vänster och välj **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="a5938-202">Click **Responses** > **ADD** on hello left and select **200 OK**.</span></span> <span data-ttu-id="a5938-203">Klicka på **spara** toosave den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a5938-203">Click **Save** toosave this operation.</span></span>

## <a name="call-an-operation-from-hello-developer-portal"></a><span data-ttu-id="a5938-204">Anropa en åtgärd från hello Developer-portalen</span><span class="sxs-lookup"><span data-stu-id="a5938-204">Call an operation from hello Developer Portal</span></span>
<span data-ttu-id="a5938-205">Åtgärder kan anropas direkt från hello Developer-portalen, vilket ger ett bekvämt sätt tooview och testa hello driften av ett API.</span><span class="sxs-lookup"><span data-stu-id="a5938-205">Operations can be called directly from hello Developer portal, which provides a convenient way tooview and test hello operations of an API.</span></span> <span data-ttu-id="a5938-206">I den här guiden steg du anropar hello **Resursposter köra** metod som har lagts till toohello **AzureML Demo API**.</span><span class="sxs-lookup"><span data-stu-id="a5938-206">In this guide step you will call hello **RRS Execute** method that was added toohello **AzureML Demo API**.</span></span> <span data-ttu-id="a5938-207">Klicka på **Developer-portalen** hello-menyn på hello uppifrån höger om hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="a5938-207">Click **Developer portal** from hello menu at hello top right of hello Classic Portal.</span></span>

![Developer-portalen](./media/machine-learning-manage-web-service-endpoints-using-api-management/developer-portal.png)

<span data-ttu-id="a5938-209">Klicka på **API: er** hello översta menyn och klicka sedan på **AzureML Demo API** toosee hello-åtgärder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="a5938-209">Click **APIs** from hello top menu, and then click **AzureML Demo API** toosee hello operations available.</span></span>

![demoazureml-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/demoazureml-api.png)

<span data-ttu-id="a5938-211">Välj **Resursposter köra** för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a5938-211">Select **RRS Execute** for hello operation.</span></span> <span data-ttu-id="a5938-212">Klicka på **prova**.</span><span class="sxs-lookup"><span data-stu-id="a5938-212">Click **Try It**.</span></span>

![Försök it](./media/machine-learning-manage-web-service-endpoints-using-api-management/try-it.png)

<span data-ttu-id="a5938-214">De begäranparametrarna, Skriv din **arbetsytan**, **service**, **2.0** för hello **apiversion**, och **true**för hello **information**.</span><span class="sxs-lookup"><span data-stu-id="a5938-214">For Request parameters, type your **workspace**,  **service**, **2.0** for hello **apiversion**, and  **true** for hello **details**.</span></span> <span data-ttu-id="a5938-215">Du kan hitta din **arbetsytan** och **service** hello AzureML web service instrumentpanelen (finns **testa hello webbtjänsten** i bilaga A).</span><span class="sxs-lookup"><span data-stu-id="a5938-215">You can find your **workspace** and **service** in hello AzureML web service dashboard (see **Test hello web service** in Appendix A).</span></span>

<span data-ttu-id="a5938-216">Huvuden för begäran, klickar du på **Lägg till sidhuvud** och skriv **Content-Type** och **application/json**, klicka på **Lägg till sidhuvud** och skriv **auktorisering** och **ägar <YOUR AZUREML SERVICE API-KEY>** .</span><span class="sxs-lookup"><span data-stu-id="a5938-216">For Request headers, click **Add header** and type **Content-Type** and **application/json**, then click **Add header** and type **Authorization** and **Bearer <YOUR AZUREML SERVICE API-KEY>**.</span></span> <span data-ttu-id="a5938-217">Du hittar din **api-nyckel** hello AzureML web service instrumentpanelen (se **testa hello-webbtjänsten** i bilaga A).</span><span class="sxs-lookup"><span data-stu-id="a5938-217">You can find your **api key** in hello AzureML web service dashboard (see **Test hello web service** in Appendix A).</span></span>

<span data-ttu-id="a5938-218">Typen **{”indata”: {”input1”: {”ColumnNames”: [”Col2”], ”värden”: [[”det här är en bra dag”]]}}, ”GlobalParameters”: {}}** för hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="a5938-218">Type **{"Inputs": {"input1": {"ColumnNames": ["Col2"], "Values": [["This is a good day"]]}}, "GlobalParameters": {}}** for hello request body.</span></span>

![azureml-demo-api](./media/machine-learning-manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

<span data-ttu-id="a5938-220">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="a5938-220">Click **Send**.</span></span>

![Skicka](./media/machine-learning-manage-web-service-endpoints-using-api-management/send.png)

<span data-ttu-id="a5938-222">När en åtgärd har anropats hello developer-portalen visar hello **begärda URL: en** från hello backend-tjänst hello **svarsstatusen**, hello **svarshuvuden**, och alla **svar innehåll**.</span><span class="sxs-lookup"><span data-stu-id="a5938-222">After an operation is invoked, hello developer portal displays hello **Requested URL** from hello back-end service, hello **Response status**, hello **Response headers**, and any **Response content**.</span></span>

![Response-status](./media/machine-learning-manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a><span data-ttu-id="a5938-224">Bilaga A – skapa och testa en enkel AzureML webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="a5938-224">Appendix A - Creating and testing a simple AzureML web service</span></span>
### <a name="creating-hello-experiment"></a><span data-ttu-id="a5938-225">Skapa hello experiment</span><span class="sxs-lookup"><span data-stu-id="a5938-225">Creating hello experiment</span></span>
<span data-ttu-id="a5938-226">Nedan finns hello steg för att skapa ett enkelt experiment AzureML och distribuera den som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a5938-226">Below are hello steps for creating a simple AzureML experiment and deploying it as a web service.</span></span> <span data-ttu-id="a5938-227">Hej web service tar som indata för en kolumn med valfri text och returnerar en uppsättning funktioner som visas i form av heltal.</span><span class="sxs-lookup"><span data-stu-id="a5938-227">hello web service takes as input a column of arbitrary text and returns a set of features represented as integers.</span></span> <span data-ttu-id="a5938-228">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5938-228">For example:</span></span>

| <span data-ttu-id="a5938-229">Text</span><span class="sxs-lookup"><span data-stu-id="a5938-229">Text</span></span> | <span data-ttu-id="a5938-230">Hashformaterats Text</span><span class="sxs-lookup"><span data-stu-id="a5938-230">Hashed Text</span></span> |
| --- | --- |
| <span data-ttu-id="a5938-231">Det här är en bra dag</span><span class="sxs-lookup"><span data-stu-id="a5938-231">This is a good day</span></span> |<span data-ttu-id="a5938-232">1 1 2 2 0 2 0 1</span><span class="sxs-lookup"><span data-stu-id="a5938-232">1 1 2 2 0 2 0 1</span></span> |

<span data-ttu-id="a5938-233">Börja med en webbläsare, navigera till: [https://studio.azureml.net/](https://studio.azureml.net/) och ange dina autentiseringsuppgifter toolog i.</span><span class="sxs-lookup"><span data-stu-id="a5938-233">First, using a browser of your choice, navigate to: [https://studio.azureml.net/](https://studio.azureml.net/) and enter your credentials toolog in.</span></span> <span data-ttu-id="a5938-234">Skapa sedan ett nytt tomt experiment.</span><span class="sxs-lookup"><span data-stu-id="a5938-234">Next, create a new blank experiment.</span></span>

![Sök-experiment-mallar](./media/machine-learning-manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

<span data-ttu-id="a5938-236">Byt namn på den för**SimpleFeatureHashingExperiment**.</span><span class="sxs-lookup"><span data-stu-id="a5938-236">Rename it too**SimpleFeatureHashingExperiment**.</span></span> <span data-ttu-id="a5938-237">Expandera **sparade datauppsättningar** och dra **Book granskningar från Amazon** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="a5938-237">Expand **Saved Datasets** and drag **Book Reviews from Amazon** onto your experiment.</span></span>

![enkel-funktionen-hash-experiment](./media/machine-learning-manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

<span data-ttu-id="a5938-239">Expandera **Dataomvandling** och **manipulering** och dra **Välj kolumner i datauppsättning** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="a5938-239">Expand **Data Transformation** and **Manipulation** and drag **Select Columns in Dataset** onto your experiment.</span></span> <span data-ttu-id="a5938-240">Ansluta **Book granskningar från Amazon** för**Välj kolumner i datauppsättning**.</span><span class="sxs-lookup"><span data-stu-id="a5938-240">Connect **Book Reviews from Amazon** too**Select Columns in Dataset**.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/project-columns.png)

<span data-ttu-id="a5938-242">Klicka på **Välj kolumner i datauppsättning** och klicka sedan på **starta kolumnväljaren** och välj **Col2**.</span><span class="sxs-lookup"><span data-stu-id="a5938-242">Click **Select Columns in Dataset** and then click **Launch column selector** and select **Col2**.</span></span> <span data-ttu-id="a5938-243">Klicka på hello markering tooapply ändringarna.</span><span class="sxs-lookup"><span data-stu-id="a5938-243">Click hello checkmark tooapply these changes.</span></span>

![select-columns](./media/machine-learning-manage-web-service-endpoints-using-api-management/select-columns.png)

<span data-ttu-id="a5938-245">Expandera **textanalys** och dra **funktions-hashning** till hello experiment.</span><span class="sxs-lookup"><span data-stu-id="a5938-245">Expand **Text Analytics** and drag **Feature Hashing** onto hello experiment.</span></span> <span data-ttu-id="a5938-246">Ansluta **Välj kolumner i datauppsättning** för**hash-funktionen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-246">Connect **Select Columns in Dataset** too**Feature Hashing**.</span></span>

![ansluta projektkolumner](./media/machine-learning-manage-web-service-endpoints-using-api-management/connect-project-columns.png)

<span data-ttu-id="a5938-248">Typen **3** för hello **Hashing bitsize**.</span><span class="sxs-lookup"><span data-stu-id="a5938-248">Type **3** for hello **Hashing bitsize**.</span></span> <span data-ttu-id="a5938-249">Detta skapar 8 (23) kolumner.</span><span class="sxs-lookup"><span data-stu-id="a5938-249">This will create 8 (23) columns.</span></span>

![hash-bitsize](./media/machine-learning-manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

<span data-ttu-id="a5938-251">Nu vill du kanske tooclick **kör** tootest hello experiment.</span><span class="sxs-lookup"><span data-stu-id="a5938-251">At this point, you may want tooclick **Run** tootest hello experiment.</span></span>

![Kör](./media/machine-learning-manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a><span data-ttu-id="a5938-253">Skapa en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="a5938-253">Create a web service</span></span>
<span data-ttu-id="a5938-254">Nu skapa en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a5938-254">Now create a web service.</span></span> <span data-ttu-id="a5938-255">Expandera **webbtjänsten** och dra **indata** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="a5938-255">Expand **Web Service** and drag **Input** onto your experiment.</span></span> <span data-ttu-id="a5938-256">Ansluta **indata** för**hash-funktionen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-256">Connect **Input** too**Feature Hashing**.</span></span> <span data-ttu-id="a5938-257">Också dra **utdata** på experimentet.</span><span class="sxs-lookup"><span data-stu-id="a5938-257">Also drag **output** onto your experiment.</span></span> <span data-ttu-id="a5938-258">Ansluta **utdata** för**hash-funktionen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-258">Connect **Output** too**Feature Hashing**.</span></span>

![utdata-till-funktionen för-hashing](./media/machine-learning-manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

<span data-ttu-id="a5938-260">Klicka på **publicerar webbtjänst**.</span><span class="sxs-lookup"><span data-stu-id="a5938-260">Click **Publish web service**.</span></span>

![Publicera webbtjänst](./media/machine-learning-manage-web-service-endpoints-using-api-management/publish-web-service.png)

<span data-ttu-id="a5938-262">Klicka på **Ja** toopublish hello experiment.</span><span class="sxs-lookup"><span data-stu-id="a5938-262">Click **Yes** toopublish hello experiment.</span></span>

![Ja för att publicera](./media/machine-learning-manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-hello-web-service"></a><span data-ttu-id="a5938-264">Testa hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="a5938-264">Test hello web service</span></span>
<span data-ttu-id="a5938-265">En AzureML-webbtjänst består av RSS-(begäranden/svar-tjänsten) och slutpunkter för BES (batch execution service).</span><span class="sxs-lookup"><span data-stu-id="a5938-265">An AzureML web service consists of RSS (request/response service) and BES (batch execution service) endpoints.</span></span> <span data-ttu-id="a5938-266">RSS är för synkron körning.</span><span class="sxs-lookup"><span data-stu-id="a5938-266">RSS is for synchronous execution.</span></span> <span data-ttu-id="a5938-267">BES är asynkron jobbkörningen.</span><span class="sxs-lookup"><span data-stu-id="a5938-267">BES is for asynchronous job execution.</span></span> <span data-ttu-id="a5938-268">tootest webbtjänst med hello Python-exempelkälla nedan, du kan behöva toodownload och installera hello Azure SDK för Python (se: [hur tooinstall Python](../python-how-to-install.md)).</span><span class="sxs-lookup"><span data-stu-id="a5938-268">tootest your web service with hello sample Python source below, you may need toodownload and install hello Azure SDK for Python (see: [How tooinstall Python](../python-how-to-install.md)).</span></span>

<span data-ttu-id="a5938-269">Du måste också hello **arbetsytan**, **service**, och **api_key** av experimentet för hello-exempelkälla nedan.</span><span class="sxs-lookup"><span data-stu-id="a5938-269">You will also need hello **workspace**, **service**, and **api_key** of your experiment for hello sample source below.</span></span> <span data-ttu-id="a5938-270">Du kan hitta hello arbetsytan och tjänsten genom att klicka på antingen **frågor och svar** eller **Batch Execution** för experimentet hello web service instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a5938-270">You can find hello workspace and service by clicking either **Request/Response** or **Batch Execution** for your experiment in hello web service dashboard.</span></span>

![Sök-arbetsytan-och-tjänst](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

<span data-ttu-id="a5938-272">Du kan hitta hello **api_key** genom att klicka på experimentet hello web service instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a5938-272">You can find hello **api_key** by clicking your experiment in hello web service dashboard.</span></span>

![Sök-api-nyckel](./media/machine-learning-manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a><span data-ttu-id="a5938-274">Testa Resursposter slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a5938-274">Test RRS endpoint</span></span>
##### <a name="test-button"></a><span data-ttu-id="a5938-275">Knappen Testa</span><span class="sxs-lookup"><span data-stu-id="a5938-275">Test button</span></span>
<span data-ttu-id="a5938-276">Ett enkelt sätt tootest hello RR-slutpunkten är tooclick **Test** på instrumentpanelen för hello web service.</span><span class="sxs-lookup"><span data-stu-id="a5938-276">An easy way tootest hello RRS endpoint is tooclick **Test** on hello web service dashboard.</span></span>

![Test](./media/machine-learning-manage-web-service-endpoints-using-api-management/test.png)

<span data-ttu-id="a5938-278">Typen **detta är en bra dag** för **col2**.</span><span class="sxs-lookup"><span data-stu-id="a5938-278">Type **This is a good day** for **col2**.</span></span> <span data-ttu-id="a5938-279">Klicka på hello markering.</span><span class="sxs-lookup"><span data-stu-id="a5938-279">Click hello checkmark.</span></span>

![Ange data](./media/machine-learning-manage-web-service-endpoints-using-api-management/enter-data.png)

<span data-ttu-id="a5938-281">Du ser något som liknar</span><span class="sxs-lookup"><span data-stu-id="a5938-281">You will see something like</span></span>

![exempel på-utdata](./media/machine-learning-manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a><span data-ttu-id="a5938-283">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="a5938-283">Sample Code</span></span>
<span data-ttu-id="a5938-284">Ett annat sätt tootest din RRS är från din klientkod.</span><span class="sxs-lookup"><span data-stu-id="a5938-284">Another way tootest your RRS is from your client code.</span></span> <span data-ttu-id="a5938-285">Om du klickar på **frågor och svar** på hello instrumentpanelen och rulla toohello längst ned i visas exempelkod för C#, Python och R. Du kan även se hello syntax för hello Resursposter förfrågan, inklusive hello Begärd URI, rubriker och brödtext.</span><span class="sxs-lookup"><span data-stu-id="a5938-285">If you click **Request/response** on hello dashboard and scroll toohello bottom, you will see sample code for C#, Python, and R. You will also see hello syntax of hello RRS request, including hello request URI, headers, and body.</span></span>

<span data-ttu-id="a5938-286">Den här guiden visar en fungerande Python exempel.</span><span class="sxs-lookup"><span data-stu-id="a5938-286">This guide shows a working Python example.</span></span> <span data-ttu-id="a5938-287">Du behöver toomodify med hello **arbetsytan**, **service**, och **api_key** av experimentet.</span><span class="sxs-lookup"><span data-stu-id="a5938-287">You will need toomodify it with hello **workspace**, **service**, and **api_key** of your experiment.</span></span>

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
        print("hello request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a><span data-ttu-id="a5938-288">Testa BES slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a5938-288">Test BES endpoint</span></span>
<span data-ttu-id="a5938-289">Klicka på **Batch-körningen** hello instrumentpanelen och rulla toohello längst ned.</span><span class="sxs-lookup"><span data-stu-id="a5938-289">Click **Batch execution** on hello dashboard and scroll toohello bottom.</span></span> <span data-ttu-id="a5938-290">Du kommer att se exempelkod för C#, Python och R. Du kan även se hello syntaxen för hello BES begäranden toosubmit ett jobb, starta ett jobb, hämta hello status eller resultatet av ett jobb och ta bort ett jobb.</span><span class="sxs-lookup"><span data-stu-id="a5938-290">You will see sample code for C#, Python, and R. You will also see hello syntax of hello BES requests toosubmit a job, start a job, get hello status or results of a job, and delete a job.</span></span>

<span data-ttu-id="a5938-291">Den här guiden visar en fungerande Python exempel.</span><span class="sxs-lookup"><span data-stu-id="a5938-291">This guide shows a working Python example.</span></span> <span data-ttu-id="a5938-292">Du behöver toomodify med hello **arbetsytan**, **service**, och **api_key** av experimentet.</span><span class="sxs-lookup"><span data-stu-id="a5938-292">You need toomodify it with hello **workspace**, **service**, and **api_key** of your experiment.</span></span> <span data-ttu-id="a5938-293">Dessutom måste toomodify hello **lagringskontonamnet**, **lagringskontonyckel**, och **namnet på lagringsbehållaren**.</span><span class="sxs-lookup"><span data-stu-id="a5938-293">Additionally, you need toomodify hello **storage account name**, **storage account key**, and **storage container name**.</span></span> <span data-ttu-id="a5938-294">Slutligen måste toomodify hello platsen för hello **indatafilen** och hello plats för hello **utdatafilen**.</span><span class="sxs-lookup"><span data-stu-id="a5938-294">Lastly, you will need toomodify hello location of hello **input file** and hello location of hello **output file**.</span></span>

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH hello API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH hello LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH hello LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("hello request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading hello result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written toohello file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("hello results for " + outputName + " are available at hello following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "hello results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading hello input tooblob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded hello input tooblob storage"
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
    print("Submitting hello job...")
    # submit hello job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove hello enclosing double-quotes
    print("Job ID: " + job_id)
    # start hello job
    print("Starting hello job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking hello job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in hello follwing code
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
