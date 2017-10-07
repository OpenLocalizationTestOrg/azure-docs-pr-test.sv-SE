---
title: "stöd för aaaCORS i App Service | Microsoft Docs"
description: "Lär dig hur toouse CORS stöd i Azure Apptjänst."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: alkarche
ms.openlocfilehash: c229378b75840bc0f7b2eefc3df3031233f9b494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-api-app-from-javascript-using-cors"></a><span data-ttu-id="d18a8-103">Använda en API-app från JavaScript med CORS</span><span class="sxs-lookup"><span data-stu-id="d18a8-103">Consume an API app from JavaScript using CORS</span></span>
<span data-ttu-id="d18a8-104">Apptjänst har inbyggt stöd för [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), vilket gör att JavaScript-klienter toomake mellan domäner anropar tooAPIs som finns i API apps.</span><span class="sxs-lookup"><span data-stu-id="d18a8-104">App Service offers built-in support for [Cross Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), which enables JavaScript clients toomake cross-domain calls tooAPIs that are hosted in API apps.</span></span> <span data-ttu-id="d18a8-105">Apptjänst kan du konfigurera CORS åtkomst tooyour API utan att skriva någon kod i ditt API.</span><span class="sxs-lookup"><span data-stu-id="d18a8-105">App Service lets you configure CORS access tooyour API without writing any code in your API.</span></span>

<span data-ttu-id="d18a8-106">Den här artikeln innehåller två avsnitt:</span><span class="sxs-lookup"><span data-stu-id="d18a8-106">This article contains two sections:</span></span>

* <span data-ttu-id="d18a8-107">Hej [hur tooconfigure CORS](#corsconfig) förklaras i allmänna hur tooconfigure CORS för API-app, webbapp eller mobilapp.</span><span class="sxs-lookup"><span data-stu-id="d18a8-107">hello [How tooconfigure CORS](#corsconfig) section explains in general how tooconfigure CORS for any API app, web app, or mobile app.</span></span> <span data-ttu-id="d18a8-108">Det gäller även tooall ramverk som stöds av Apptjänsten, inklusive .NET, Node.js och Java.</span><span class="sxs-lookup"><span data-stu-id="d18a8-108">It applies equally tooall frameworks that are supported by App Service, including .NET, Node.js, and Java.</span></span> 
* <span data-ttu-id="d18a8-109">Från och med hello [fortsätter hello .NET komma igång-Självstudier](#tutorialstart) avsnittet hello artikeln är en genomgång som visar CORS-stöd genom att bygga vidare på vad du gjorde i [hello första API Apps komma igång-kursen ](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d18a8-109">Starting with hello [Continuing hello .NET getting-started tutorials](#tutorialstart) section, hello article is a tutorial that demonstrates CORS support by building on what you did in [hello first API Apps getting started tutorial](app-service-api-dotnet-get-started.md).</span></span> 

## <span data-ttu-id="d18a8-110"><a id="corsconfig"></a>Hur tooconfigure CORS i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d18a8-110"><a id="corsconfig"></a> How tooconfigure CORS in Azure App Service</span></span>
<span data-ttu-id="d18a8-111">Du kan konfigurera CORS i hello Azure-portalen eller med hjälp av [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) verktyg.</span><span class="sxs-lookup"><span data-stu-id="d18a8-111">You can configure CORS in hello Azure portal or by using [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tools.</span></span>

#### <a name="configure-cors-in-hello-azure-portal"></a><span data-ttu-id="d18a8-112">Konfigurera CORS i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d18a8-112">Configure CORS in hello Azure portal</span></span>
1. <span data-ttu-id="d18a8-113">I en webbläsare går toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d18a8-113">In a browser go toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d18a8-114">Klicka på **Apptjänster**, och klicka sedan på hello namnet på din API-app.</span><span class="sxs-lookup"><span data-stu-id="d18a8-114">Click **App Services**, and then click hello name of your API app.</span></span>
   
    ![Välj API-app i portalen](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="d18a8-116">I hello **inställningar** bladet som öppnas toohello höger om hello **API-app** bladet, hitta hello **API** avsnittet och klicka sedan på **CORS**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-116">In hello **Settings** blade that opens toohello right of hello **API app** blade, find hello **API** section, and then click **CORS**.</span></span>
   
   ![Välj CORS i bladet Inställningar](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="d18a8-118">Ange hello URL eller URL: er som du vill tooallow JavaScript-anrop toocome från i hello textruta.</span><span class="sxs-lookup"><span data-stu-id="d18a8-118">In hello text box enter hello URL or URLs that you want tooallow JavaScript calls toocome from.</span></span>

    <span data-ttu-id="d18a8-119">Om du har distribuerat JavaScript programmet tooa webbappen med namnet todolistangular anger du ”https://todolistangular.azurewebsites.net”.</span><span class="sxs-lookup"><span data-stu-id="d18a8-119">For example, if you deployed your JavaScript application tooa web app named todolistangular, enter "https://todolistangular.azurewebsites.net".</span></span> <span data-ttu-id="d18a8-120">Alternativt kan ange du en asterisk (*) toospecify att alla ursprungsdomäner accepteras.</span><span class="sxs-lookup"><span data-stu-id="d18a8-120">As an alternative, you can enter an asterisk (*) toospecify that all origin domains are accepted.</span></span>


1. <span data-ttu-id="d18a8-121">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-121">Click **Save**.</span></span>
   
   ![Klicka på Spara](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="d18a8-123">När du klickar på **spara**, hello API-appen att acceptera JavaScript-anrop från hello angivna URL: er.</span><span class="sxs-lookup"><span data-stu-id="d18a8-123">After you click **Save**, hello API app will accept JavaScript calls from hello specified URLs.</span></span>

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a><span data-ttu-id="d18a8-124">Konfigurera CORS med hjälp av Azure Resource Manager-verktyg</span><span class="sxs-lookup"><span data-stu-id="d18a8-124">Configure CORS by using Azure Resource Manager tools</span></span>
<span data-ttu-id="d18a8-125">Du kan också konfigurera CORS för API-app med hjälp av [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) i kommandoradsverktyg som [Azure PowerShell](/powershell/azureps-cmdlets-docs) och hello [Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="d18a8-125">You can also configure CORS for an API app by using [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) in command line tools such as [Azure PowerShell](/powershell/azureps-cmdlets-docs) and hello [Azure CLI](../cli-install-nodejs.md).</span></span> 

<span data-ttu-id="d18a8-126">Ett exempel på en Azure Resource Manager-mall som anger CORS-egenskapen för hello öppna hello [azuredeploy.JSON i hello databasen för den här kursens exempelprogram](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="d18a8-126">For an example of an Azure Resource Manager template that sets hello CORS property, open hello [azuredeploy.json file in hello repository for this tutorial's sample application](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json).</span></span> <span data-ttu-id="d18a8-127">Hitta hello avsnitt i hello-mall som ser ut som följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="d18a8-127">Find hello section of hello template that looks like hello following example:</span></span>

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <span data-ttu-id="d18a8-128"><a id="tutorialstart"></a>Fortsätter hello .NET komma igång-kursen</span><span class="sxs-lookup"><span data-stu-id="d18a8-128"><a id="tutorialstart"></a> Continuing hello .NET getting-started tutorial</span></span>
<span data-ttu-id="d18a8-129">Om du följer hello Node.js eller Java Kom igång-serien för API apps har du slutfört hello komma igång-serien.</span><span class="sxs-lookup"><span data-stu-id="d18a8-129">If you are following hello Node.js or Java getting-started series for API apps, you have completed hello getting started series.</span></span> <span data-ttu-id="d18a8-130">Hoppa över toohello [nästa steg](#next-steps) avsnittet toofind förslag på ytterligare utbildning om API Apps.</span><span class="sxs-lookup"><span data-stu-id="d18a8-130">Skip toohello [Next steps](#next-steps) section toofind suggestions for further learning about API Apps.</span></span>

<span data-ttu-id="d18a8-131">hello resten av den här artikeln är en förlängning av hello Kom igång-serien för .NET och förutsätter att du har slutfört [hello första självstudierna](app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d18a8-131">hello remainder of this article is a continuation of hello .NET getting-started series and assumes that you successfully completed [hello first tutorial](app-service-api-dotnet-get-started.md).</span></span>

## <a name="deploy-hello-todolistangular-project-tooa-new-web-app"></a><span data-ttu-id="d18a8-132">Distribuera hello ToDoListAngular-projektet tooa nytt webbprogram</span><span class="sxs-lookup"><span data-stu-id="d18a8-132">Deploy hello ToDoListAngular project tooa new web app</span></span>
<span data-ttu-id="d18a8-133">I [hello första självstudierna](app-service-api-dotnet-get-started.md), du har skapat en mellannivå-API-app och en API-appen på datanivå.</span><span class="sxs-lookup"><span data-stu-id="d18a8-133">In [hello first tutorial](app-service-api-dotnet-get-started.md), you created a middle tier API app and a data tier API app.</span></span> <span data-ttu-id="d18a8-134">I den här självstudiekursen skapar du en enda sida program (SPA) webbapp som anrop hello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="d18a8-134">In this tutorial you create a single-page application (SPA) web app that calls hello middle tier API app.</span></span> <span data-ttu-id="d18a8-135">Hello SPA toowork har tooenable CORS på hello mellannivå-API-app.</span><span class="sxs-lookup"><span data-stu-id="d18a8-135">For hello SPA toowork you have tooenable CORS on hello middle tier API app.</span></span> 

<span data-ttu-id="d18a8-136">I hello [exempelappen todolist](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular-projektet är en enkel AngularJS-klient som anropar hello mellannivå ToDoListAPI Web API-projekt.</span><span class="sxs-lookup"><span data-stu-id="d18a8-136">In hello [ToDoList sample application](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), hello ToDoListAngular project is a simple AngularJS client that calls hello middle tier ToDoListAPI Web API project.</span></span> <span data-ttu-id="d18a8-137">Hej JavaScript-kod i hello *app/scripts/todoListSvc.js* anropar hello-API med hjälp av hello AngularJS HTTP-leverantören.</span><span class="sxs-lookup"><span data-stu-id="d18a8-137">hello JavaScript code in hello *app/scripts/todoListSvc.js* file calls hello API by using hello AngularJS HTTP provider.</span></span> 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-hello-todolistangular-project"></a><span data-ttu-id="d18a8-138">Skapa ett nytt webbprogram för hello ToDoListAngular-projektet</span><span class="sxs-lookup"><span data-stu-id="d18a8-138">Create a new web app for hello ToDoListAngular project</span></span>
<span data-ttu-id="d18a8-139">Hej proceduren toocreate en ny App Service webbapp och distribuera ett projekt tooit är liknande toowhat som du såg för [att skapa och distribuera en API-app i hello första kursen i den här serien](app-service-api-dotnet-get-started.md#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="d18a8-139">hello procedure toocreate a new App Service web app and deploy a project tooit is similar toowhat you saw for [creating and deploying an API app in hello first tutorial in this series](app-service-api-dotnet-get-started.md#createapiapp).</span></span> <span data-ttu-id="d18a8-140">hello bara skillnaden är den typ av app hello är **Webbapp** i stället för **API-App**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-140">hello only difference is that hello app type is **Web App** instead of **API App**.</span></span>  <span data-ttu-id="d18a8-141">Skärmdumpar av dialogrutorna hello finns</span><span class="sxs-lookup"><span data-stu-id="d18a8-141">For screen shots of hello dialogs, see</span></span> 

1. <span data-ttu-id="d18a8-142">I **Solution Explorer**, högerklicka på hello ToDoListAngular-projektet och klicka sedan på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-142">In **Solution Explorer**, right-click hello ToDoListAngular project, and then click **Publish**.</span></span>
2. <span data-ttu-id="d18a8-143">I hello **profil** för hello **Publicera webbplats** guiden, klickar du på **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-143">In hello **Profile** tab of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="d18a8-144">I hello **Apptjänst** dialogrutan klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-144">In hello **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="d18a8-145">I hello **värd** för hello **skapa Apptjänst** dialogrutan Ange en **Webbprogramnamnet** som är unikt i hello *azurewebsites.net* domän.</span><span class="sxs-lookup"><span data-stu-id="d18a8-145">In hello **Hosting** tab of hello **Create App Service** dialog box, enter a **Web App Name** that is unique in hello *azurewebsites.net* domain.</span></span> 
5. <span data-ttu-id="d18a8-146">Välj hello Azure **prenumeration** du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="d18a8-146">Choose hello Azure **Subscription** you want toowork with.</span></span>
6. <span data-ttu-id="d18a8-147">I hello **resursgruppen** listrutan Välj hello samma resursgrupp som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d18a8-147">In hello **Resource Group** drop-down list, choose hello same resource group you created earlier.</span></span>
7. <span data-ttu-id="d18a8-148">I hello **Apptjänstplan** listrutan Välj hello samma plan som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d18a8-148">In hello **App Service Plan** drop-down list, choose hello same plan you created earlier.</span></span> 
8. <span data-ttu-id="d18a8-149">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-149">Click **Create**.</span></span>
   
    <span data-ttu-id="d18a8-150">Visual Studio skapar hello webbprogram, skapas en publiceringsprofil för den och visar hello **anslutning** steg i hello **Publicera webbplats** guiden.</span><span class="sxs-lookup"><span data-stu-id="d18a8-150">Visual Studio creates hello web app, creates a publish profile for it, and displays hello **Connection** step of hello **Publish Web** wizard.</span></span>
   
    <span data-ttu-id="d18a8-151">Klicka inte på **Publicera** ännu.</span><span class="sxs-lookup"><span data-stu-id="d18a8-151">Don't click **Publish** yet.</span></span> <span data-ttu-id="d18a8-152">I följande avsnitt hello, konfigurerar du hello ny web app toocall hello mellannivå-API-app som körs i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="d18a8-152">In hello following section, you configure hello new web app toocall hello middle tier API app that is running in App Service.</span></span> 

### <a name="set-hello-middle-tier-url-in-web-app-settings"></a><span data-ttu-id="d18a8-153">Ange URL för hello mellannivå i inställningarna för webbappen</span><span class="sxs-lookup"><span data-stu-id="d18a8-153">Set hello middle tier URL in web app settings</span></span>
1. <span data-ttu-id="d18a8-154">Gå toohello [Azure-portalen](https://portal.azure.com/), och sedan gå toohello **Web App** bladet för hello webbprogrammet som du skapade toohost hello TodoListAngular (klientdelen)-projektet.</span><span class="sxs-lookup"><span data-stu-id="d18a8-154">Go toohello [Azure portal](https://portal.azure.com/), and then navigate toohello **Web App** blade for hello web app that you created toohost hello TodoListAngular (front end) project.</span></span>
2. <span data-ttu-id="d18a8-155">Klicka på **Inställningar > Programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-155">Click **Settings > Application Settings**.</span></span>
3. <span data-ttu-id="d18a8-156">I hello **appinställningar** lägger du till hello följande nyckel och värde:</span><span class="sxs-lookup"><span data-stu-id="d18a8-156">In hello **App settings** section, add hello following key and value:</span></span>
   
   | <span data-ttu-id="d18a8-157">Nyckel</span><span class="sxs-lookup"><span data-stu-id="d18a8-157">Key</span></span> | <span data-ttu-id="d18a8-158">Värde</span><span class="sxs-lookup"><span data-stu-id="d18a8-158">Value</span></span> | <span data-ttu-id="d18a8-159">Exempel</span><span class="sxs-lookup"><span data-stu-id="d18a8-159">Example</span></span> |
   | --- | --- | --- |
   | <span data-ttu-id="d18a8-160">toDoListAPIURL</span><span class="sxs-lookup"><span data-stu-id="d18a8-160">toDoListAPIURL</span></span> |<span data-ttu-id="d18a8-161">https://{namnet på ditt API på mellannivå}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="d18a8-161">https://{your middle tier API app name}.azurewebsites.net</span></span> |<span data-ttu-id="d18a8-162">https://todolistapi0121.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="d18a8-162">https://todolistapi0121.azurewebsites.net</span></span> |
4. <span data-ttu-id="d18a8-163">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-163">Click **Save**.</span></span>
   
    <span data-ttu-id="d18a8-164">När hello koden körs i Azure åsidosätter det här värdet hello localhost-URL som är i hello *Web.config* fil.</span><span class="sxs-lookup"><span data-stu-id="d18a8-164">When hello code runs in Azure, this value overrides hello localhost URL that is in hello *Web.config* file.</span></span> 
   
    <span data-ttu-id="d18a8-165">hello-koden som hämtar Inställningsvärdet hello finns i *index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d18a8-165">hello code that gets hello setting value is in *index.cshtml*:</span></span>
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    <span data-ttu-id="d18a8-166">Hej koden i *todoListSvc.js* använder hello inställningen:</span><span class="sxs-lookup"><span data-stu-id="d18a8-166">hello code in *todoListSvc.js* uses hello setting:</span></span>
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-hello-todolistangular-web-project-toohello-new-web-app"></a><span data-ttu-id="d18a8-167">Distribuera hello ToDoListAngular web project toohello nytt webbprogram</span><span class="sxs-lookup"><span data-stu-id="d18a8-167">Deploy hello ToDoListAngular web project toohello new web app</span></span>
* <span data-ttu-id="d18a8-168">I Visual Studio i hello **anslutning** steg i hello **Publicera webbplats** guiden, klickar du på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-168">In Visual Studio, in hello **Connection** step of hello **Publish Web** wizard, click **Publish**.</span></span>
  
   <span data-ttu-id="d18a8-169">Visual Studio distribuerar hello ToDoListAngular-projektet toohello ny webbapp och öppnar en webbläsare toohello URL av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d18a8-169">Visual Studio deploys hello ToDoListAngular project toohello new web app and opens a browser toohello URL of hello web app.</span></span> 

### <a name="test-hello-application-without-cors-enabled"></a><span data-ttu-id="d18a8-170">Testa programmet hello utan att CORS är aktiverat</span><span class="sxs-lookup"><span data-stu-id="d18a8-170">Test hello application without CORS enabled</span></span>
1. <span data-ttu-id="d18a8-171">Öppna hello konsolfönstret i webbläsarens utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="d18a8-171">In your browser Developer Tools, open hello Console window.</span></span>
2. <span data-ttu-id="d18a8-172">I hello webbläsarfönster som visar hello AngularJS UI, klickar du på hello **tooDo lista** länk.</span><span class="sxs-lookup"><span data-stu-id="d18a8-172">In hello browser window that displays hello AngularJS UI, click hello **tooDo List** link.</span></span>
   
    <span data-ttu-id="d18a8-173">hello JavaScript-kod försöker toocall hello mellannivå-API-app, men hello anropet misslyckas eftersom hello klientdelen körs i en annan domän än hello tillbaka slutet.</span><span class="sxs-lookup"><span data-stu-id="d18a8-173">hello JavaScript code tries toocall hello middle tier API app, but hello call fails because hello front end is running in a different domain than hello back end.</span></span> <span data-ttu-id="d18a8-174">konsolfönster för hello webbläsarens utvecklingsverktyg visar ett felmeddelande för cross-origin.</span><span class="sxs-lookup"><span data-stu-id="d18a8-174">hello browser's Developer Tools Console window shows a cross-origin error message.</span></span>
   
    ![Felmeddelande för cross-origin](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-hello-middle-tier-api-app"></a><span data-ttu-id="d18a8-176">Konfigurera CORS för API-appen för hello mellannivå</span><span class="sxs-lookup"><span data-stu-id="d18a8-176">Configure CORS for hello middle tier API app</span></span>
<span data-ttu-id="d18a8-177">I det här avsnittet kan du konfigurera hello CORS-inställningen i Azure för hello mellannivå API-appen ToDoListAPI.</span><span class="sxs-lookup"><span data-stu-id="d18a8-177">In this section, you configure hello CORS setting in Azure for hello middle tier ToDoListAPI API app.</span></span> <span data-ttu-id="d18a8-178">Den här inställningen tillåter hello mellannivå API app tooreceive JavaScript-anrop från hello webbapp som du skapade för hello ToDoListAngular-projektet.</span><span class="sxs-lookup"><span data-stu-id="d18a8-178">This setting will allow hello middle tier API app tooreceive JavaScript calls from hello web app that you created for hello ToDoListAngular project.</span></span>

1. <span data-ttu-id="d18a8-179">I en webbläsare går du toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d18a8-179">In a browser, go toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d18a8-180">Klicka på **Apptjänster**, och klicka sedan på hello ToDoListAPI (mellannivå) API-app.</span><span class="sxs-lookup"><span data-stu-id="d18a8-180">Click **App Services**, and then click hello ToDoListAPI (middle tier) API app.</span></span>
   
    ![Välj API-app i portalen](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. <span data-ttu-id="d18a8-182">I hello **inställningar** bladet som öppnas toohello höger om hello **API-app** bladet, hitta hello **API** avsnittet och klicka sedan på **CORS**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-182">In hello **Settings** blade that opens toohello right of hello **API app** blade, find hello **API** section, and then click **CORS**.</span></span>
   
   ![Välj CORS i portalen](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. <span data-ttu-id="d18a8-184">Ange hello URL för webbappen ToDoListAngular (klientdelen) för hello hello i textrutan.</span><span class="sxs-lookup"><span data-stu-id="d18a8-184">In hello text box, enter hello URL for hello ToDoListAngular (front end) web app.</span></span> <span data-ttu-id="d18a8-185">Till exempel tillåta anrop från hello URL Om du har distribuerat hello ToDoListAngular-projektet tooa webbprogram med namnet todolistangular0121 `https://todolistangular0121.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="d18a8-185">For example, if you deployed hello ToDoListAngular project tooa web app named todolistangular0121, allow calls from hello URL `https://todolistangular0121.azurewebsites.net`.</span></span>
   
   <span data-ttu-id="d18a8-186">Alternativt kan ange du en asterisk (*) toospecify att alla ursprungsdomäner accepteras.</span><span class="sxs-lookup"><span data-stu-id="d18a8-186">As an alternative, you can enter an asterisk (*) toospecify that all origin domains are accepted.</span></span>
5. <span data-ttu-id="d18a8-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d18a8-187">Click **Save**.</span></span>
   
   ![Klicka på Spara](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   <span data-ttu-id="d18a8-189">När du klickar på **spara**, hello API-appen att acceptera JavaScript-anrop från hello specificerat URL: en.</span><span class="sxs-lookup"><span data-stu-id="d18a8-189">After you click **Save**, hello API app will accept JavaScript calls from hello specified URL.</span></span> <span data-ttu-id="d18a8-190">I den här skärmdumpen kommer hello API-appen ToDoListAPI0223 acceptera JavaScript-klientanrop från hello webbappen todolistangular.</span><span class="sxs-lookup"><span data-stu-id="d18a8-190">In this screen shot, hello ToDoListAPI0223 API app will accept JavaScript client calls from hello ToDoListAngular web app.</span></span>

### <a name="test-hello-application-with-cors-enabled"></a><span data-ttu-id="d18a8-191">Testa programmet hello med CORS aktiverat</span><span class="sxs-lookup"><span data-stu-id="d18a8-191">Test hello application with CORS enabled</span></span>
* <span data-ttu-id="d18a8-192">Öppna en webbläsare toohello HTTPS-URL för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d18a8-192">Open a browser toohello HTTPS URL of hello web app.</span></span> 
  
    <span data-ttu-id="d18a8-193">Det här programmet hello kan du visa, lägga till, redigera och ta bort arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d18a8-193">This time hello application lets you view, add, edit, and delete to-do items.</span></span> 
  
    ![sidan för tooDo lista för exempelapp](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a><span data-ttu-id="d18a8-195">Apptjänst-CORS kontra Web API CORS</span><span class="sxs-lookup"><span data-stu-id="d18a8-195">App Service CORS versus Web API CORS</span></span>
<span data-ttu-id="d18a8-196">I Web API-projekt kan du installera hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-paketet toospecify i koden vilka domäner som din API ska ta emot JavaScript-anrop.</span><span class="sxs-lookup"><span data-stu-id="d18a8-196">In a Web API project, you can install hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package toospecify in code which domains your API will accept JavaScript calls from.</span></span>

<span data-ttu-id="d18a8-197">Stöd för Web API CORS är mer flexibelt än stöd för Apptjänst-CORS.</span><span class="sxs-lookup"><span data-stu-id="d18a8-197">Web API CORS support is more flexible than App Service CORS support.</span></span> <span data-ttu-id="d18a8-198">Till exempel kan du i koden ange olika accepterade ursprung för olika åtgärdsmetoder, medan du anger en uppsättning accepterade ursprung för alla metoder i en API-app när det gäller Apptjänst-CORS.</span><span class="sxs-lookup"><span data-stu-id="d18a8-198">For example, in code you can specify different accepted origins for different action methods, while for App Service CORS you specify one set of accepted origins for all of an API app's methods.</span></span>

> [!NOTE]
> <span data-ttu-id="d18a8-199">Försök inte toouse både Web API CORS och Apptjänst CORS i en API-app.</span><span class="sxs-lookup"><span data-stu-id="d18a8-199">Don't try toouse both Web API CORS and App Service CORS in one API app.</span></span> <span data-ttu-id="d18a8-200">Apptjänst-CORS har företräde och Web API CORS har ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="d18a8-200">App Service CORS will take precedence and Web API CORS will have no effect.</span></span> <span data-ttu-id="d18a8-201">Till exempel om du aktiverar en ursprungsdomän i Apptjänst och aktiverar alla ursprungsdomäner i Web API-koden, Azure API-app bara ska ta emot samtal från hello domän du angav i Azure.</span><span class="sxs-lookup"><span data-stu-id="d18a8-201">For example, if you enable one origin domain in App Service, and enable all origin domains in your Web API code, your Azure API app will only accept calls from hello domain you specified in Azure.</span></span>
> 
> 

### <a name="how-tooenable-cors-in-web-api-code"></a><span data-ttu-id="d18a8-202">Hur tooenable CORS i Web API-koden</span><span class="sxs-lookup"><span data-stu-id="d18a8-202">How tooenable CORS in Web API code</span></span>
<span data-ttu-id="d18a8-203">hello följande steg sammanfattar hello process för att aktivera stöd för Web API CORS.</span><span class="sxs-lookup"><span data-stu-id="d18a8-203">hello following steps summarize hello process for enabling Web API CORS support.</span></span> <span data-ttu-id="d18a8-204">Mer information finns i [Aktivera cross-origin-begäranden i ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).</span><span class="sxs-lookup"><span data-stu-id="d18a8-204">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).</span></span>

1. <span data-ttu-id="d18a8-205">I ett Web API-projekt installerar hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="d18a8-205">In a Web API project, install hello [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet package.</span></span>
2. <span data-ttu-id="d18a8-206">Inkludera en `config.EnableCors()` kodrad i hello **registrera** metod för hello **WebApiConfig** klassen som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d18a8-206">Include a `config.EnableCors()` line of code in hello **Register** method of hello **WebApiConfig** class, as in hello following example.</span></span> 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // hello following line enables you toocontrol CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. <span data-ttu-id="d18a8-207">I Web API-kontrollanten lägger du till en `using` -instruktion för hello `System.Web.Http.Cors` namnområde och Lägg till hello `EnableCors` attributet toohello klass eller tooindividual åtgärden kontrollantmetoder.</span><span class="sxs-lookup"><span data-stu-id="d18a8-207">In your Web API controller, add a `using` statement for hello `System.Web.Http.Cors` namespace, and add hello `EnableCors` attribute toohello controller class or tooindividual action methods.</span></span> <span data-ttu-id="d18a8-208">I följande exempel hello, gäller CORS-stödet toohello hela kontrollanten.</span><span class="sxs-lookup"><span data-stu-id="d18a8-208">In hello following example, CORS support applies toohello entire controller.</span></span>
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a><span data-ttu-id="d18a8-209">Använda Azure API Management med API Apps</span><span class="sxs-lookup"><span data-stu-id="d18a8-209">Using Azure API Management with API Apps</span></span>
<span data-ttu-id="d18a8-210">Om du använder Azure API Management med en API-app måste du konfigurera CORS i API Management i stället för i hello API-app.</span><span class="sxs-lookup"><span data-stu-id="d18a8-210">If you use Azure API Management with an API app, configure CORS in API Management instead of in hello API app.</span></span> <span data-ttu-id="d18a8-211">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="d18a8-211">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="d18a8-212">Översikt över Azure API Management (video: CORS börjar vid 12:10)</span><span class="sxs-lookup"><span data-stu-id="d18a8-212">Azure API Management Overview (video: CORS starts at 12:10)</span></span>](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [<span data-ttu-id="d18a8-213">Korsdomänprinciper för API Management</span><span class="sxs-lookup"><span data-stu-id="d18a8-213">API Management cross domain policies</span></span>](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a><span data-ttu-id="d18a8-214">Felsökning</span><span class="sxs-lookup"><span data-stu-id="d18a8-214">Troubleshooting</span></span>
<span data-ttu-id="d18a8-215">Om du stöter på problem när du går igenom den här kursen får du några felsökningsidéer här.</span><span class="sxs-lookup"><span data-stu-id="d18a8-215">In case you run into a problem while going through this tutorial, here are some troubleshooting ideas.</span></span>

* <span data-ttu-id="d18a8-216">Kontrollera att du använder hello senaste versionen av hello [Azure SDK för .NET för Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="d18a8-216">Make sure that you're using hello latest version of hello [Azure SDK for .NET for Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="d18a8-217">Kontrollera att du har angett `https` i hello CORS-inställningen och se till att du använder `https` toorun hello frontend-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d18a8-217">Make sure that you entered `https` in hello CORS setting, and make sure that you're using `https` toorun hello front-end web app.</span></span>
* <span data-ttu-id="d18a8-218">Kontrollera att du har angett hello CORS-inställningen i API-appen för hello mellannivå, inte i hello frontend-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d18a8-218">Make sure that you entered hello CORS setting in hello middle tier API app, not in hello front-end web app.</span></span>
* <span data-ttu-id="d18a8-219">Om du konfigurerar CORS i både programkoden och Azure App Service, Observera att hello Apptjänst-CORS-inställning åsidosätter vad du gör i programkoden.</span><span class="sxs-lookup"><span data-stu-id="d18a8-219">If you're configuring CORS in both application code and Azure App Service, note that hello App Service CORS setting will override whatever you're doing in application code.</span></span> 

<span data-ttu-id="d18a8-220">toolearn mer om Visual Studio-funktioner som förenklar felsökningen, se [felsöka Azure Apptjänst-appar i Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="d18a8-220">toolearn more about Visual Studio features that simplify troubleshooting, see [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d18a8-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d18a8-221">Next steps</span></span>
<span data-ttu-id="d18a8-222">I den här artikeln har sett du hur tooenable Apptjänst-CORS stöder så att klientens JavaScript-kod kan anropa ett API i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="d18a8-222">In this article, you saw how tooenable App Service CORS support so that client JavaScript code can call an API in a different domain.</span></span> <span data-ttu-id="d18a8-223">Mer om API apps, läsa hello toolearn [introduktion tooauthentication i App Service](../app-service/app-service-authentication-overview.md), och sedan gå toohello [användarautentisering för API apps](app-service-api-dotnet-user-principal-auth.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="d18a8-223">toolearn more about API apps, read hello [introduction tooauthentication in App Service](../app-service/app-service-authentication-overview.md), and then go toohello [user authentication for API apps](app-service-api-dotnet-user-principal-auth.md) tutorial.</span></span>

