---
title: "aaaAdd anpassade domäner och SSL tooan Azure web app | Microsoft Docs"
description: "Lär dig hur tooprepare ditt Azure web app toogo produktion genom att lägga till din företagsanpassning. Mappa hello domänen namnet (alternativa domän) tooyour webbapp och skydda den med ett anpassat SSL-certifikat."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 03/29/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2679ed8b2dbbeba0b128c1a3ec01148f97c35342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-domain-and-ssl-tooan-azure-web-app"></a><span data-ttu-id="194e9-104">Lägg till anpassade domäner och SSL tooan Azure webbapp</span><span class="sxs-lookup"><span data-stu-id="194e9-104">Add custom domain and SSL tooan Azure web app</span></span>

<span data-ttu-id="194e9-105">Den här kursen visar hur tooquickly mappa en anpassad domän namn tooyour Azure webbapp och skydda den med ett anpassat SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="194e9-105">This tutorial shows you how tooquickly map a custom domain name tooyour Azure web app and then secure it with a custom SSL certificate.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="194e9-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="194e9-106">Before you begin</span></span>

<span data-ttu-id="194e9-107">Innan du kör det här exemplet installerar hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) lokalt.</span><span class="sxs-lookup"><span data-stu-id="194e9-107">Before running this sample, install hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) locally.</span></span>

<span data-ttu-id="194e9-108">Du måste också administrativ åtkomst toohello DNS-konfigurationssidan för respektive domän-providern.</span><span class="sxs-lookup"><span data-stu-id="194e9-108">You also need administrative access toohello DNS configuration page for your respective domain provider.</span></span> <span data-ttu-id="194e9-109">Till exempel tooadd `www.contoso.com`, behöver du toobe kan tooconfigure DNS-poster för `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="194e9-109">For example, tooadd `www.contoso.com`, you need toobe able tooconfigure DNS entries for `contoso.com`.</span></span>

<span data-ttu-id="194e9-110">Slutligen måste en giltig. PFX-filen _och_ lösenordet för hello SSL-certifikat som du vill tooupload och bind.</span><span class="sxs-lookup"><span data-stu-id="194e9-110">Lastly, you need a valid .PFX file _and_ its password for hello SSL certificate you want tooupload and bind.</span></span> <span data-ttu-id="194e9-111">SSL-certifikatet ska vara konfigurerade toosecure hello domännamn du vill använda.</span><span class="sxs-lookup"><span data-stu-id="194e9-111">This SSL certificate should be configured toosecure hello custom domain name you want.</span></span> <span data-ttu-id="194e9-112">I ovanstående exempel hello, SSL-certifikat ska skydda `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="194e9-112">In hello above example, your SSL certificate should secure `www.contoso.com`.</span></span> 

## <a name="step-1---create-an-azure-web-app"></a><span data-ttu-id="194e9-113">Steg 1 – skapa ett Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="194e9-113">Step 1 - Create an Azure web app</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="194e9-114">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="194e9-114">Log in tooAzure</span></span>

<span data-ttu-id="194e9-115">Vi kan nu gå toouse hello Azure CLI 2.0 i ett terminalfönster toocreate hello resurser behövs toohost vår Node.js-app i Azure.</span><span class="sxs-lookup"><span data-stu-id="194e9-115">We are now going toouse hello Azure CLI 2.0 in a terminal window toocreate hello resources needed toohost our Node.js app in Azure.</span></span>  <span data-ttu-id="194e9-116">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="194e9-116">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="194e9-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="194e9-117">Create a resource group</span></span>   
<span data-ttu-id="194e9-118">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="194e9-118">Create a resource group with hello [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="194e9-119">En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. webbappar, databaser och lagringskonton) distribueras och hanteras i.</span><span class="sxs-lookup"><span data-stu-id="194e9-119">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

<span data-ttu-id="194e9-120">toosee vilka möjliga värden du kan använda för `---location`, använda hello `az appservice list-locations` Azure CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="194e9-120">toosee what possible values you can use for `---location`, use hello `az appservice list-locations` Azure CLI command.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="194e9-121">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="194e9-121">Create an App Service plan</span></span>

<span data-ttu-id="194e9-122">Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="194e9-122">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="194e9-123">hello följande exempel skapas en apptjänstplan med namnet `myAppServicePlan` med hello **grundläggande** prisnivån.</span><span class="sxs-lookup"><span data-stu-id="194e9-123">hello following example creates an App Service plan named `myAppServicePlan` using hello **Basic** pricing tier.</span></span>

<span data-ttu-id="194e9-124">Skapa AZ programtjänstplan--namnet myAppServicePlan--resursgrupp myResourceGroup--sku B1</span><span class="sxs-lookup"><span data-stu-id="194e9-124">az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1</span></span>

<span data-ttu-id="194e9-125">När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="194e9-125">When hello App Service plan has been created, hello Azure CLI shows information similar toohello following example.</span></span> 

```json 
{ 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "kind": "app", 
    "location": "West Europe", 
    "sku": { 
    "capacity": 1, 
    "family": "B", 
    "name": "B1", 
    "tier": "Basic" 
    }, 
    "status": "Ready", 
    "type": "Microsoft.Web/serverfarms" 
} 
``` 

## <a name="create-a-web-app"></a><span data-ttu-id="194e9-126">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="194e9-126">Create a web app</span></span>

<span data-ttu-id="194e9-127">Nu när en App Service-plan har skapats kan du skapa en webbapp inom hello `myAppServicePlan` App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="194e9-127">Now that an App Service plan has been created, create a web app within hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="194e9-128">hello web app ger du är en värd utrymme toodeploy koden som innehåller en URL för du tooview hello distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="194e9-128">hello web app gives your a hosting space toodeploy your code as well as provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="194e9-129">Använd hello [az apptjänst web skapa](/cli/azure/appservice/web#create) kommandot toocreate hello-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="194e9-129">Use hello [az appservice web create](/cli/azure/appservice/web#create) command toocreate hello web app.</span></span> 

<span data-ttu-id="194e9-130">I hello kommandot nedan ersätta ditt eget unikt appnamn där du ser hello `<app_name>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="194e9-130">In hello command below, please substitute your own unique app name where you see hello `<app_name>` placeholder.</span></span> <span data-ttu-id="194e9-131">Detta unika namn används under hello hello standarddomännamnet för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="194e9-131">This unique name will be used as hello part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="194e9-132">Du kan mappa ett anpassat webbprogram för DNS-posten toohello senare innan du exponera tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="194e9-132">You can later map any custom DNS entry toohello web app before you expose it tooyour users.</span></span> 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

<span data-ttu-id="194e9-133">När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="194e9-133">When hello web app has been created, hello Azure CLI shows information similar toohello following example.</span></span> 

```json 
{ 
    "clientAffinityEnabled": true, 
    "defaultHostName": "<app_name>.azurewebsites.net", 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>", 
    "isDefaultContainer": null, 
    "kind": "app", 
    "location": "West Europe", 
    "name": "<app_name>", 
    "repositorySiteName": "<app_name>", 
    "reserved": true, 
    "resourceGroup": "myResourceGroup", 
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "state": "Running", 
    "type": "Microsoft.Web/sites", 
} 
```

<span data-ttu-id="194e9-134">Från hello JSON-utdata `defaultHostName` visar standarddomännamnet för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="194e9-134">From hello JSON output, `defaultHostName` shows your web app's default domain name.</span></span> <span data-ttu-id="194e9-135">Navigera toothis adress i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="194e9-135">In your browser, navigate toothis address.</span></span>

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a><span data-ttu-id="194e9-137">Steg 2 – konfigurera DNS-mappning</span><span class="sxs-lookup"><span data-stu-id="194e9-137">Step 2 - Configure DNS mapping</span></span>

<span data-ttu-id="194e9-138">I det här steget kan du lägga till en mappning från en anpassad domän tooyour webbprogram standarddomännamnet `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="194e9-138">In this step, you add a mapping from a custom domain tooyour web app's default domain name, `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="194e9-139">Normalt utför du det här steget i domänen leverantörens webbplats.</span><span class="sxs-lookup"><span data-stu-id="194e9-139">Typically, you perform this step in your domain provider's website.</span></span> <span data-ttu-id="194e9-140">Varje domänregistrator webbplats är något annorlunda, så bör du kontakta din leverantör dokumentation.</span><span class="sxs-lookup"><span data-stu-id="194e9-140">Every domain registrar's website is slightly different, so you should consult your provider's documentation.</span></span> <span data-ttu-id="194e9-141">Här är några allmänna riktlinjer.</span><span class="sxs-lookup"><span data-stu-id="194e9-141">However, here are some general guidelines.</span></span> 

### <a name="navigate-tootoodns-management-page"></a><span data-ttu-id="194e9-142">Navigera tootooDNS sidan för hantering</span><span class="sxs-lookup"><span data-stu-id="194e9-142">Navigate tootooDNS management page</span></span>

<span data-ttu-id="194e9-143">Logga först in tooyour domänregistrators webbplats.</span><span class="sxs-lookup"><span data-stu-id="194e9-143">First, log in tooyour domain registrar's website.</span></span>  

<span data-ttu-id="194e9-144">Leta sedan upp hello sidan för hantering av DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="194e9-144">Then, find hello page for managing DNS records.</span></span> <span data-ttu-id="194e9-145">Leta efter länkar eller delar av hello plats med etiketten **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="194e9-145">Look for links or areas of hello site labeled **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="194e9-146">Du kan ofta hitta hello länken genom att visa din kontoinformation och söker efter en länk som **min domäner**.</span><span class="sxs-lookup"><span data-stu-id="194e9-146">Often, you can find hello link by viewing your account information, and then looking for a link such as **My domains**.</span></span>

<span data-ttu-id="194e9-147">När du hittar den här sidan kan du leta efter en länk där du kan lägga till eller redigera DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="194e9-147">Once you find this page, look for a link that lets you add or edit DNS records.</span></span> <span data-ttu-id="194e9-148">Detta kan bero på en **zonfilen** eller **DNS-poster** länken, eller en **avancerad konfiguration** länk.</span><span class="sxs-lookup"><span data-stu-id="194e9-148">This might be a **Zone file** or **DNS Records** link, or an **Advanced configuration** link.</span></span>

### <a name="create-a-cname-record"></a><span data-ttu-id="194e9-149">Skapa en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="194e9-149">Create a CNAME record</span></span>

<span data-ttu-id="194e9-150">Lägg till en CNAME-post som mappar hello önskad underdomän namn tooyour webbprogram standarddomännamnet (`<app_name>.azurewebsites.net`, där `<app_name>` är unikt namn för din app).</span><span class="sxs-lookup"><span data-stu-id="194e9-150">Add a CNAME record that maps hello desired subdomain name tooyour web app's default domain name (`<app_name>.azurewebsites.net`, where `<app_name>` is your app's unique name).</span></span>

<span data-ttu-id="194e9-151">För hello `www.contoso.com` exempelvis skapa en CNAME-post som mappar hello `www` värdnamn för`<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="194e9-151">For hello `www.contoso.com` example, you create a CNAME that maps hello `www` hostname too`<app_name>.azurewebsites.net`.</span></span>

## <a name="step-3---configure-hello-custom-domain-on-your-web-app"></a><span data-ttu-id="194e9-152">Steg 3 – Konfigurera hello anpassade domäner på ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="194e9-152">Step 3 - Configure hello custom domain on your web app</span></span>

<span data-ttu-id="194e9-153">När du har konfigurerat hello hostname mappning i domänen leverantörens webbplats, är du redo tooconfigure hello anpassade domäner på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="194e9-153">When you finish configuring hello hostname mapping in your domain provider's website, you're ready tooconfigure hello custom domain on your web app.</span></span> <span data-ttu-id="194e9-154">Använd hello [az apptjänst web config hostname lägga till](/cli/azure/appservice/web/config/hostname#add) kommandot tooadd den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="194e9-154">Use hello [az appservice web config hostname add](/cli/azure/appservice/web/config/hostname#add) command tooadd this configuration.</span></span> 

<span data-ttu-id="194e9-155">I hello kommandot nedan, ersätta `<app_name>` med unika namn och < your_custom_domain > med hello fullständigt kvalificerade domännamn (t.ex. `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="194e9-155">In hello command below, please substitute `<app_name>` with your unique app name, and <your_custom_domain> with hello fully qualified custom domain name (e.g. `www.contoso.com`).</span></span> 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

<span data-ttu-id="194e9-156">hello domänen är nu fullständigt mappade tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="194e9-156">hello custom domain now is fully mapped tooyour web app.</span></span> <span data-ttu-id="194e9-157">Navigera toohello domännamn i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="194e9-157">In your browser, navigate toohello custom domain name.</span></span> <span data-ttu-id="194e9-158">Exempel:</span><span class="sxs-lookup"><span data-stu-id="194e9-158">For example:</span></span>

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-tooyour-web-app"></a><span data-ttu-id="194e9-160">Steg 4 – binda ett anpassat webbprogram för SSL-certifikat tooyour</span><span class="sxs-lookup"><span data-stu-id="194e9-160">Step 4 - Bind a custom SSL certificate tooyour web app</span></span>

<span data-ttu-id="194e9-161">Nu har du ett Azure webbapp med hello domännamn som du vill använda i hello webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="194e9-161">You now have an Azure web app, with hello domain name you want in hello browser's address bar.</span></span> <span data-ttu-id="194e9-162">Men om du navigerar toohello `https://<your_custom_domain>` nu kan du få ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="194e9-162">However, if you navigate toohello `https://<your_custom_domain>` now, you get a certificate error.</span></span> 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

<span data-ttu-id="194e9-164">Det här felet beror på att webbappen inte har en SSL-certifikat-bindning som matchar din anpassade domännamn.</span><span class="sxs-lookup"><span data-stu-id="194e9-164">This error occurs because your web app doesn't yet have an SSL certificate binding that matches your custom domain name.</span></span> <span data-ttu-id="194e9-165">Men du inte får ett felmeddelande om du navigerar för`https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="194e9-165">However, you don't get an error if you navigate too`https://<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="194e9-166">Detta beror på att din app, samt alla appar i Azure App Service, skyddas med hello SSL-certifikat för hello `*.azurewebsites.net` jokerteckendomän som standard.</span><span class="sxs-lookup"><span data-stu-id="194e9-166">This is because your app, as well as all Azure App Service apps, is secured with hello SSL certificate for hello `*.azurewebsites.net` wildcard domain by default.</span></span> 

<span data-ttu-id="194e9-167">I ordning tooaccess ditt webbprogram med ditt domännamn måste toobind hello SSL-certifikat för webbappen domänen toohello.</span><span class="sxs-lookup"><span data-stu-id="194e9-167">In order tooaccess your web app by your custom domain name, you need toobind hello SSL certificate for your custom domain toohello web app.</span></span> <span data-ttu-id="194e9-168">Du kan göra det i det här steget.</span><span class="sxs-lookup"><span data-stu-id="194e9-168">You will do it in this step.</span></span> 

### <a name="upload-hello-ssl-certificate"></a><span data-ttu-id="194e9-169">Överför hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="194e9-169">Upload hello SSL certificate</span></span>

<span data-ttu-id="194e9-170">Överför hello SSL-certifikat för ditt anpassade domäner tooyour webbprogram med hjälp av hello [az apptjänst web config ssl överför](/cli/azure/appservice/web/config/ssl#upload) kommando.</span><span class="sxs-lookup"><span data-stu-id="194e9-170">Upload hello SSL certificate for your custom domain tooyour web app by using hello [az appservice web config ssl upload](/cli/azure/appservice/web/config/ssl#upload) command.</span></span>

<span data-ttu-id="194e9-171">I hello kommandot nedan, ersätta `<app_name>` med din unikt appnamn `<path_to_ptx_file>` med hello sökvägen tooyour. PFX-filen och `<password>` med ditt certifikat lösenord.</span><span class="sxs-lookup"><span data-stu-id="194e9-171">In hello command below, please substitute `<app_name>` with your unique app name, `<path_to_ptx_file>` with hello path tooyour .PFX file, and `<password>` with your certificate's password.</span></span> 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

<span data-ttu-id="194e9-172">När hello certifikat har överförts visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="194e9-172">When hello certificate is uploaded, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "cerBlob": null,
  "expirationDate": "2018-03-29T14:12:57+00:00",
  "friendlyName": "",
  "hostNames": [
    "www.contoso.com"
  ],
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/cert
ificates/9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "issueDate": "2017-03-29T14:12:57+00:00",
  "issuer": "www.contoso.com",
  "keyVaultId": null,
  "keyVaultSecretName": null,
  "keyVaultSecretStatus": "Initialized",
  "kind": null,
  "location": "West Europe",
  "name": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "password": null,
 "pfxBlob": null,
  "publicKeyHash": null,
  "resourceGroup": "myResourceGroup",
  "selfLink": null,
  "serverFarmId": null,
  "siteName": null,
  "subjectName": "www.contoso.com",
  "tags": null,
  "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00",
  "type": "Microsoft.Web/certificates",
  "valid": null
}
```

<span data-ttu-id="194e9-173">Från hello JSON-utdata `thumbprint` visar överförda certifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="194e9-173">From hello JSON output, `thumbprint` shows your uploaded certificate's thumbprint.</span></span> <span data-ttu-id="194e9-174">Kopiera värdet för hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="194e9-174">Copy its value for hello next step.</span></span>

### <a name="bind-hello-uploaded-ssl-certificate-toohello-web-app"></a><span data-ttu-id="194e9-175">Bind hello upp SSL-certifikat toohello webbprogram</span><span class="sxs-lookup"><span data-stu-id="194e9-175">Bind hello uploaded SSL certificate toohello web app</span></span>

<span data-ttu-id="194e9-176">Ditt webbprogram har nu hello domännamn som du vill använda och har ett SSL-certifikat som skyddar den egna domänen.</span><span class="sxs-lookup"><span data-stu-id="194e9-176">Your web app now has hello custom domain name you want, and it also has a SSL certificate that secures that custom domain.</span></span> <span data-ttu-id="194e9-177">hello är endast sak vänstra toodo toobind hello överförda certifikat toohello webbapp.</span><span class="sxs-lookup"><span data-stu-id="194e9-177">hello only thing left toodo is toobind hello uploaded certificate toohello web app.</span></span> <span data-ttu-id="194e9-178">Du kan göra detta med hjälp av hello [az apptjänst web config ssl-bindning](/cli/azure/appservice/web/config/ssl#bind) kommando.</span><span class="sxs-lookup"><span data-stu-id="194e9-178">You do this by using hello [az appservice web config ssl bind](/cli/azure/appservice/web/config/ssl#bind) command.</span></span>

<span data-ttu-id="194e9-179">I hello kommandot nedan, ersätta `<app_name>` med din unika namn och `<thumbprint-from-previous-output>` med hello tumavtrycket för certifikatet som du får från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="194e9-179">In hello command below, please substitute `<app_name>` with your unique app name and `<thumbprint-from-previous-output>` with hello certificate thumbprint that you get from hello previous command.</span></span> 

<span data-ttu-id="194e9-180">AZ apptjänst web config ssl-bindning--name < programnamn >--resursgrupp myResourceGroup--tumavtryck < tumavtryck-från-tidigare-output >--ssl-typen SNI</span><span class="sxs-lookup"><span data-stu-id="194e9-180">az appservice web config ssl bind --name <app_name> --resource-group myResourceGroup --certificate-thumbprint <thumbprint-from-previous-output> --ssl-type SNI</span></span>

<span data-ttu-id="194e9-181">När hello certifikat är bundet tooyour webbprogram, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="194e9-181">When hello certificate is bound tooyour web app, hello Azure CLI shows information similar toohello following example:</span></span>

<span data-ttu-id="194e9-182">{”availabilityState”: ”Normal”, ”clientAffinityEnabled”: värdet är true, ”clientCertEnabled”: false, ”cloningInfo”: null, ”containerSize”: 0, ”dailyMemoryTimeQuota”: 0, ”defaultHostName” ”: < programnamn >. azurewebsites.net”, ”enabled”: värdet är true ”, enabledHostNames ”: [” www.contoso.com ””, < programnamn >. azurewebsites.net ””, < programnamn >. scm.azurewebsites.net ”],” gatewaySiteName ”: null,” hostNameSslStates ”: [{” hostType ”:” Standard ”,” name ””: < programnamn >. azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” databas ”,” name ””: < programnamn >. scm.azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” Standard ”,” name ”:” www.contoso.com ”,” sslState ”:” SniEnabled ”,” tumavtryck ”:” 9FD1D2D06E2293673E2A8D1CA484A092BD016D00 ”,” toUpdate ”: null,” virtualIp ”: null}],” värdnamn ”: [” www.contoso.com ”,” < programnamn >. azurewebsites.NET ”],” hostNamesDisabled ”: false,” hostingEnvironmentProfile ”: null,” id ””: /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < programnamn > ””, isDefaultContainer ”: null” typ ”:” WebApp ”,” lastModifiedTimeUtc ””: 2017-03-29T14:36:18.803333 ”,” plats ”:” västra Europa ”,” maxNumberOfWorkers ”: null,” Mikrotjänster ”:” false ”,” namn ”:” < programnamn > ”,” outboundIpAddresses ””: 13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1 ”,” premiumAppDeployed ”: null,” repositorySiteName ”:” < programnamn > ”,” reserverade ”: false,” resourceGroup ”:” myResourceGroup ”,” scmSiteAlsoStopped ”: false,” serverFarmId ”: ”/ prenumerationer/00000000-0000-0000-0000-000000000000/resursgrupper/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan”, ”siteConfig”: null, ”slotSwapStatus”: null ”tillstånd”: ”körs”, ”suspendedTill”: null ”taggar”: null, ”targetSwapSlot”: null, ”trafficManagerHostNames”: null, ”typ”: ”Microsoft.Web/sites”, ”usageState”: ”Normal”}</span><span class="sxs-lookup"><span data-stu-id="194e9-182">{ "availabilityState": "Normal", "clientAffinityEnabled": true, "clientCertEnabled": false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "<app_name>.azurewebsites.net", "enabled": true, "enabledHostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net", "<app_name>.scm.azurewebsites.net" ], "gatewaySiteName": null, "hostNameSslStates": [ { "hostType": "Standard", "name": "<app_name>.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Repository", "name": "<app_name>.scm.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtualIp": null } ], "hostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net" ], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s/<app_name>", "isDefaultContainer": null, "kind": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "location": "West Europe", "maxNumberOfWorkers": null, "microService": "false", "name": "<app_name>", "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "<app_name>", "reserved": false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": false, "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "state": "Running", "suspendedTill": null, "tags": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normal" }</span></span>

<span data-ttu-id="194e9-183">Navigera tooHTTPS slutpunkten för ditt anpassade domännamn i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="194e9-183">In your browser, navigate tooHTTPS endpoint of your custom domain name.</span></span> <span data-ttu-id="194e9-184">Exempel:</span><span class="sxs-lookup"><span data-stu-id="194e9-184">For example:</span></span>

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a><span data-ttu-id="194e9-186">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="194e9-186">More resources</span></span>

<span data-ttu-id="194e9-187">[Köp och konfigurera ett anpassat domännamn i Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[köpa och konfigurera ett SSL-certifikat för Azure App Service](web-sites-purchase-ssl-web-site.md)</span><span class="sxs-lookup"><span data-stu-id="194e9-187">[Buy and Configure a custom domain name in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[Buy and Configure an SSL Certificate for your Azure App Service](web-sites-purchase-ssl-web-site.md)</span></span>
