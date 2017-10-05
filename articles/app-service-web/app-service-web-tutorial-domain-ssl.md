---
title: "Lägga till anpassade domäner och SSL i en Azure-webbapp | Microsoft Docs"
description: "Lär dig hur du förbereder din Azure-webbapp gå produktionen genom att lägga till din företagsanpassning. Mappa det anpassade domännamnet (alternativa domänen) till din webbapp och skydda den med ett anpassat SSL-certifikat."
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
ms.openlocfilehash: c9d00f678b6257a8aafb35acd2d5a2292703a2dc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="add-custom-domain-and-ssl-to-an-azure-web-app"></a><span data-ttu-id="fce61-104">Lägga till anpassade domäner och SSL i en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="fce61-104">Add custom domain and SSL to an Azure web app</span></span>

<span data-ttu-id="fce61-105">Den här kursen visar hur du snabbt mappa ett anpassat domännamn i Azure-webbappen och skydda den med ett anpassat SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="fce61-105">This tutorial shows you how to quickly map a custom domain name to your Azure web app and then secure it with a custom SSL certificate.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="fce61-106">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fce61-106">Before you begin</span></span>

<span data-ttu-id="fce61-107">Innan du kör det här exemplet måste du installera den [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) lokalt.</span><span class="sxs-lookup"><span data-stu-id="fce61-107">Before running this sample, install the [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) locally.</span></span>

<span data-ttu-id="fce61-108">Du måste också administrativ åtkomst till sidan DNS-konfiguration för respektive domän-providern.</span><span class="sxs-lookup"><span data-stu-id="fce61-108">You also need administrative access to the DNS configuration page for your respective domain provider.</span></span> <span data-ttu-id="fce61-109">Till exempel för att lägga till `www.contoso.com`, du behöver för att kunna konfigurera DNS-poster för `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="fce61-109">For example, to add `www.contoso.com`, you need to be able to configure DNS entries for `contoso.com`.</span></span>

<span data-ttu-id="fce61-110">Slutligen måste en giltig. PFX-filen _och_ lösenordet för SSL-certifikatet som du vill ladda upp och binda.</span><span class="sxs-lookup"><span data-stu-id="fce61-110">Lastly, you need a valid .PFX file _and_ its password for the SSL certificate you want to upload and bind.</span></span> <span data-ttu-id="fce61-111">SSL-certifikatet ska konfigureras för att skydda det anpassade domännamnet som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="fce61-111">This SSL certificate should be configured to secure the custom domain name you want.</span></span> <span data-ttu-id="fce61-112">I exemplet ovan SSL-certifikat ska skydda `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="fce61-112">In the above example, your SSL certificate should secure `www.contoso.com`.</span></span> 

## <a name="step-1---create-an-azure-web-app"></a><span data-ttu-id="fce61-113">Steg 1 – skapa ett Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="fce61-113">Step 1 - Create an Azure web app</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="fce61-114">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="fce61-114">Log in to Azure</span></span>

<span data-ttu-id="fce61-115">Nu ska vi använda Azure CLI 2.0 i ett terminalfönster för att skapa de resurser som behövs för att använda Azure som värd för Node.js-appen.</span><span class="sxs-lookup"><span data-stu-id="fce61-115">We are now going to use the Azure CLI 2.0 in a terminal window to create the resources needed to host our Node.js app in Azure.</span></span>  <span data-ttu-id="fce61-116">Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="fce61-116">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="fce61-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="fce61-117">Create a resource group</span></span>   
<span data-ttu-id="fce61-118">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="fce61-118">Create a resource group with the [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="fce61-119">En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. webbappar, databaser och lagringskonton) distribueras och hanteras i.</span><span class="sxs-lookup"><span data-stu-id="fce61-119">An Azure resource group is a logical container into which Azure resources like web apps, databases and storage accounts are deployed and managed.</span></span> 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

<span data-ttu-id="fce61-120">Om du vill se vilka värden som kan användas för `---location` använder du Azure CLI-kommandot `az appservice list-locations`.</span><span class="sxs-lookup"><span data-stu-id="fce61-120">To see what possible values you can use for `---location`, use the `az appservice list-locations` Azure CLI command.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="fce61-121">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="fce61-121">Create an App Service plan</span></span>

<span data-ttu-id="fce61-122">Skapa en App Service-plan med kommandot [az appservice plan create](/cli/azure/appservice/plan#create).</span><span class="sxs-lookup"><span data-stu-id="fce61-122">Create an App Service plan with the [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="fce61-123">I följande exempel skapas en apptjänstplan med namnet `myAppServicePlan` med hjälp av den **grundläggande** prisnivån.</span><span class="sxs-lookup"><span data-stu-id="fce61-123">The following example creates an App Service plan named `myAppServicePlan` using the **Basic** pricing tier.</span></span>

<span data-ttu-id="fce61-124">Skapa AZ programtjänstplan--namnet myAppServicePlan--resursgrupp myResourceGroup--sku B1</span><span class="sxs-lookup"><span data-stu-id="fce61-124">az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1</span></span>

<span data-ttu-id="fce61-125">När programtjänstplanen har skapats, Azure CLI visar information liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fce61-125">When the App Service plan has been created, the Azure CLI shows information similar to the following example.</span></span> 

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

## <a name="create-a-web-app"></a><span data-ttu-id="fce61-126">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="fce61-126">Create a web app</span></span>

<span data-ttu-id="fce61-127">Nu när en App Service-plan har skapats kan du skapa en webbapp i `myAppServicePlan` App Service-planen.</span><span class="sxs-lookup"><span data-stu-id="fce61-127">Now that an App Service plan has been created, create a web app within the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="fce61-128">Web app kan du är värd kan du distribuera din kod som innehåller en URL som du kan visa det distribuerade programmet.</span><span class="sxs-lookup"><span data-stu-id="fce61-128">The web app gives your a hosting space to deploy your code as well as provides a URL for you to view the deployed application.</span></span> <span data-ttu-id="fce61-129">Använd kommandot [az appservice web create](/cli/azure/appservice/web#create) för att skapa webbappen.</span><span class="sxs-lookup"><span data-stu-id="fce61-129">Use the [az appservice web create](/cli/azure/appservice/web#create) command to create the web app.</span></span> 

<span data-ttu-id="fce61-130">I det här kommandot nedan ersätta ditt eget unikt appnamn där du ser den `<app_name>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="fce61-130">In the command below, please substitute your own unique app name where you see the `<app_name>` placeholder.</span></span> <span data-ttu-id="fce61-131">Detta unika namn används som en del av standarddomännamnet för webbprogram, så att namnet måste vara unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="fce61-131">This unique name will be used as the part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="fce61-132">Du kan senare mappa en anpassad DNS-post till webbappen innan du exponerar den för användarna.</span><span class="sxs-lookup"><span data-stu-id="fce61-132">You can later map any custom DNS entry to the web app before you expose it to your users.</span></span> 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

<span data-ttu-id="fce61-133">När webbappen har skapats visas information av Azure CLI. Informationen ser ut ungefär som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fce61-133">When the web app has been created, the Azure CLI shows information similar to the following example.</span></span> 

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

<span data-ttu-id="fce61-134">Från JSON-utdata `defaultHostName` visar standarddomännamnet för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fce61-134">From the JSON output, `defaultHostName` shows your web app's default domain name.</span></span> <span data-ttu-id="fce61-135">Navigera till den här adressen i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fce61-135">In your browser, navigate to this address.</span></span>

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a><span data-ttu-id="fce61-137">Steg 2 – konfigurera DNS-mappning</span><span class="sxs-lookup"><span data-stu-id="fce61-137">Step 2 - Configure DNS mapping</span></span>

<span data-ttu-id="fce61-138">I det här steget kan du lägga till en mappning från en anpassad domän till ditt webbprogram standarddomännamnet `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="fce61-138">In this step, you add a mapping from a custom domain to your web app's default domain name, `<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="fce61-139">Normalt utför du det här steget i domänen leverantörens webbplats.</span><span class="sxs-lookup"><span data-stu-id="fce61-139">Typically, you perform this step in your domain provider's website.</span></span> <span data-ttu-id="fce61-140">Varje domänregistrator webbplats är något annorlunda, så bör du kontakta din leverantör dokumentation.</span><span class="sxs-lookup"><span data-stu-id="fce61-140">Every domain registrar's website is slightly different, so you should consult your provider's documentation.</span></span> <span data-ttu-id="fce61-141">Här är några allmänna riktlinjer.</span><span class="sxs-lookup"><span data-stu-id="fce61-141">However, here are some general guidelines.</span></span> 

### <a name="navigate-to-to-dns-management-page"></a><span data-ttu-id="fce61-142">Navigera till att DNS-sidan för hantering</span><span class="sxs-lookup"><span data-stu-id="fce61-142">Navigate to to DNS management page</span></span>

<span data-ttu-id="fce61-143">Först logga in till din domänregistrator webbplats.</span><span class="sxs-lookup"><span data-stu-id="fce61-143">First, log in to your domain registrar's website.</span></span>  

<span data-ttu-id="fce61-144">Hitta sedan sidan för hantering av DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="fce61-144">Then, find the page for managing DNS records.</span></span> <span data-ttu-id="fce61-145">Leta efter länkar eller områden på webbplatsen med namnet **domännamn**, **DNS**, eller **namn serverhantering**.</span><span class="sxs-lookup"><span data-stu-id="fce61-145">Look for links or areas of the site labeled **Domain Name**, **DNS**, or **Name Server Management**.</span></span> <span data-ttu-id="fce61-146">Ofta, kan du hitta länken genom att visa din kontoinformation och söker efter en länk som **min domäner**.</span><span class="sxs-lookup"><span data-stu-id="fce61-146">Often, you can find the link by viewing your account information, and then looking for a link such as **My domains**.</span></span>

<span data-ttu-id="fce61-147">När du hittar den här sidan kan du leta efter en länk där du kan lägga till eller redigera DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="fce61-147">Once you find this page, look for a link that lets you add or edit DNS records.</span></span> <span data-ttu-id="fce61-148">Detta kan bero på en **zonfilen** eller **DNS-poster** länken, eller en **avancerad konfiguration** länk.</span><span class="sxs-lookup"><span data-stu-id="fce61-148">This might be a **Zone file** or **DNS Records** link, or an **Advanced configuration** link.</span></span>

### <a name="create-a-cname-record"></a><span data-ttu-id="fce61-149">Skapa en CNAME-post</span><span class="sxs-lookup"><span data-stu-id="fce61-149">Create a CNAME record</span></span>

<span data-ttu-id="fce61-150">Lägg till en CNAME-post som mappar önskade underdomännamnet till ditt webbprogram standarddomännamnet (`<app_name>.azurewebsites.net`, där `<app_name>` är unikt namn för din app).</span><span class="sxs-lookup"><span data-stu-id="fce61-150">Add a CNAME record that maps the desired subdomain name to your web app's default domain name (`<app_name>.azurewebsites.net`, where `<app_name>` is your app's unique name).</span></span>

<span data-ttu-id="fce61-151">För den `www.contoso.com` exempelvis skapa en CNAME-post som mappar den `www` värdnamn till `<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="fce61-151">For the `www.contoso.com` example, you create a CNAME that maps the `www` hostname to `<app_name>.azurewebsites.net`.</span></span>

## <a name="step-3---configure-the-custom-domain-on-your-web-app"></a><span data-ttu-id="fce61-152">Steg 3 – Konfigurera anpassade domäner på ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="fce61-152">Step 3 - Configure the custom domain on your web app</span></span>

<span data-ttu-id="fce61-153">När du har konfigurerat hostname mappningen i domänen leverantörens webbplats, är du redo att konfigurera den anpassade domänen på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fce61-153">When you finish configuring the hostname mapping in your domain provider's website, you're ready to configure the custom domain on your web app.</span></span> <span data-ttu-id="fce61-154">Använd den [az apptjänst web config hostname lägga till](/cli/azure/appservice/web/config/hostname#add) kommando för att lägga till den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fce61-154">Use the [az appservice web config hostname add](/cli/azure/appservice/web/config/hostname#add) command to add this configuration.</span></span> 

<span data-ttu-id="fce61-155">I kommandot nedan ersätta `<app_name>` med unika namn och < your_custom_domain > med det fullständigt kvalificerade domännamnet (t.ex. `www.contoso.com`).</span><span class="sxs-lookup"><span data-stu-id="fce61-155">In the command below, please substitute `<app_name>` with your unique app name, and <your_custom_domain> with the fully qualified custom domain name (e.g. `www.contoso.com`).</span></span> 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

<span data-ttu-id="fce61-156">Den anpassade domänen nu mappas fullständigt till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fce61-156">The custom domain now is fully mapped to your web app.</span></span> <span data-ttu-id="fce61-157">Navigera till det anpassade domännamnet i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fce61-157">In your browser, navigate to the custom domain name.</span></span> <span data-ttu-id="fce61-158">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fce61-158">For example:</span></span>

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-to-your-web-app"></a><span data-ttu-id="fce61-160">Steg 4 – binda ett anpassat SSL-certifikat till ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="fce61-160">Step 4 - Bind a custom SSL certificate to your web app</span></span>

<span data-ttu-id="fce61-161">Nu har du ett Azure webbapp med domännamn som du vill använda i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="fce61-161">You now have an Azure web app, with the domain name you want in the browser's address bar.</span></span> <span data-ttu-id="fce61-162">Men om du navigerar till den `https://<your_custom_domain>` nu kan du få ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="fce61-162">However, if you navigate to the `https://<your_custom_domain>` now, you get a certificate error.</span></span> 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

<span data-ttu-id="fce61-164">Det här felet beror på att webbappen inte har en SSL-certifikat-bindning som matchar din anpassade domännamn.</span><span class="sxs-lookup"><span data-stu-id="fce61-164">This error occurs because your web app doesn't yet have an SSL certificate binding that matches your custom domain name.</span></span> <span data-ttu-id="fce61-165">Men du inte får ett felmeddelande om du navigerar till `https://<app_name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="fce61-165">However, you don't get an error if you navigate to `https://<app_name>.azurewebsites.net`.</span></span> <span data-ttu-id="fce61-166">Detta beror på att din app, samt alla appar i Azure App Service, skyddas med SSL-certifikatet för den `*.azurewebsites.net` jokerteckendomän som standard.</span><span class="sxs-lookup"><span data-stu-id="fce61-166">This is because your app, as well as all Azure App Service apps, is secured with the SSL certificate for the `*.azurewebsites.net` wildcard domain by default.</span></span> 

<span data-ttu-id="fce61-167">För att komma åt ditt webbprogram med ditt domännamn måste du binda SSL-certifikatet för den anpassade domänen till webbappen.</span><span class="sxs-lookup"><span data-stu-id="fce61-167">In order to access your web app by your custom domain name, you need to bind the SSL certificate for your custom domain to the web app.</span></span> <span data-ttu-id="fce61-168">Du kan göra det i det här steget.</span><span class="sxs-lookup"><span data-stu-id="fce61-168">You will do it in this step.</span></span> 

### <a name="upload-the-ssl-certificate"></a><span data-ttu-id="fce61-169">Överför ett SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="fce61-169">Upload the SSL certificate</span></span>

<span data-ttu-id="fce61-170">Överför SSL-certifikatet för den anpassade domänen till ditt webbprogram med hjälp av den [az apptjänst web config ssl överför](/cli/azure/appservice/web/config/ssl#upload) kommando.</span><span class="sxs-lookup"><span data-stu-id="fce61-170">Upload the SSL certificate for your custom domain to your web app by using the [az appservice web config ssl upload](/cli/azure/appservice/web/config/ssl#upload) command.</span></span>

<span data-ttu-id="fce61-171">I kommandot nedan ersätta `<app_name>` med din unikt appnamn `<path_to_ptx_file>` med sökvägen till den. PFX-filen och `<password>` med ditt certifikat lösenord.</span><span class="sxs-lookup"><span data-stu-id="fce61-171">In the command below, please substitute `<app_name>` with your unique app name, `<path_to_ptx_file>` with the path to your .PFX file, and `<password>` with your certificate's password.</span></span> 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

<span data-ttu-id="fce61-172">När certifikatet har överförts visar Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="fce61-172">When the certificate is uploaded, the Azure CLI shows information similar to the following example:</span></span>

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

<span data-ttu-id="fce61-173">Från JSON-utdata `thumbprint` visar överförda certifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="fce61-173">From the JSON output, `thumbprint` shows your uploaded certificate's thumbprint.</span></span> <span data-ttu-id="fce61-174">Kopiera värdet för nästa steg.</span><span class="sxs-lookup"><span data-stu-id="fce61-174">Copy its value for the next step.</span></span>

### <a name="bind-the-uploaded-ssl-certificate-to-the-web-app"></a><span data-ttu-id="fce61-175">Binda överförda SSL-certifikatet till webbappen</span><span class="sxs-lookup"><span data-stu-id="fce61-175">Bind the uploaded SSL certificate to the web app</span></span>

<span data-ttu-id="fce61-176">Ditt webbprogram har nu det anpassade domännamnet som du vill använda och har ett SSL-certifikat som skyddar den egna domänen.</span><span class="sxs-lookup"><span data-stu-id="fce61-176">Your web app now has the custom domain name you want, and it also has a SSL certificate that secures that custom domain.</span></span> <span data-ttu-id="fce61-177">Det enda som återstår att göra är att binda det överförda certifikatet till webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="fce61-177">The only thing left to do is to bind the uploaded certificate to the web app.</span></span> <span data-ttu-id="fce61-178">Du kan göra detta med hjälp av den [az apptjänst web config ssl-bindning](/cli/azure/appservice/web/config/ssl#bind) kommando.</span><span class="sxs-lookup"><span data-stu-id="fce61-178">You do this by using the [az appservice web config ssl bind](/cli/azure/appservice/web/config/ssl#bind) command.</span></span>

<span data-ttu-id="fce61-179">I kommandot nedan ersätta `<app_name>` med din unika namn och `<thumbprint-from-previous-output>` med tumavtrycket för certifikatet som du får från det föregående kommandot.</span><span class="sxs-lookup"><span data-stu-id="fce61-179">In the command below, please substitute `<app_name>` with your unique app name and `<thumbprint-from-previous-output>` with the certificate thumbprint that you get from the previous command.</span></span> 

<span data-ttu-id="fce61-180">AZ apptjänst web config ssl-bindning--name < programnamn >--resursgrupp myResourceGroup--tumavtryck < tumavtryck-från-tidigare-output >--ssl-typen SNI</span><span class="sxs-lookup"><span data-stu-id="fce61-180">az appservice web config ssl bind --name <app_name> --resource-group myResourceGroup --certificate-thumbprint <thumbprint-from-previous-output> --ssl-type SNI</span></span>

<span data-ttu-id="fce61-181">När certifikatet är bunden till ditt webbprogram, visar Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="fce61-181">When the certificate is bound to your web app, the Azure CLI shows information similar to the following example:</span></span>

<span data-ttu-id="fce61-182">{”availabilityState”: ”Normal”, ”clientAffinityEnabled”: värdet är true, ”clientCertEnabled”: false, ”cloningInfo”: null, ”containerSize”: 0, ”dailyMemoryTimeQuota”: 0, ”defaultHostName” ”: < programnamn >. azurewebsites.net”, ”enabled”: värdet är true ”, enabledHostNames ”: [” www.contoso.com ””, < programnamn >. azurewebsites.net ””, < programnamn >. scm.azurewebsites.net ”],” gatewaySiteName ”: null,” hostNameSslStates ”: [{” hostType ”:” Standard ”,” name ””: < programnamn >. azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” databas ”,” name ””: < programnamn >. scm.azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” Standard ”,” name ”:” www.contoso.com ”,” sslState ”:” SniEnabled ”,” tumavtryck ”:” 9FD1D2D06E2293673E2A8D1CA484A092BD016D00 ”,” toUpdate ”: null,” virtualIp ”: null}],” värdnamn ”: [” www.contoso.com ”,” < programnamn >. azurewebsites.NET ”],” hostNamesDisabled ”: false,” hostingEnvironmentProfile ”: null,” id ””: /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < programnamn > ””, isDefaultContainer ”: null” typ ”:” WebApp ”,” lastModifiedTimeUtc ””: 2017-03-29T14:36:18.803333 ”,” plats ”:” västra Europa ”,” maxNumberOfWorkers ”: null,” Mikrotjänster ”:” false ”,” namn ”:” < programnamn > ”,” outboundIpAddresses ””: 13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1 ”,” premiumAppDeployed ”: null,” repositorySiteName ”:” < programnamn > ”,” reserverade ”: false,” resourceGroup ”:” myResourceGroup ”,” scmSiteAlsoStopped ”: false,” serverFarmId ”: ”/ prenumerationer/00000000-0000-0000-0000-000000000000/resursgrupper/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan”, ”siteConfig”: null, ”slotSwapStatus”: null ”tillstånd”: ”körs”, ”suspendedTill”: null ”taggar”: null, ”targetSwapSlot”: null, ”trafficManagerHostNames”: null, ”typ”: ”Microsoft.Web/sites”, ”usageState”: ”Normal”}</span><span class="sxs-lookup"><span data-stu-id="fce61-182">{ "availabilityState": "Normal", "clientAffinityEnabled": true, "clientCertEnabled": false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "<app_name>.azurewebsites.net", "enabled": true, "enabledHostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net", "<app_name>.scm.azurewebsites.net" ], "gatewaySiteName": null, "hostNameSslStates": [ { "hostType": "Standard", "name": "<app_name>.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Repository", "name": "<app_name>.scm.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtualIp": null } ], "hostNames": [ "www.contoso.com", "<app_name>.azurewebsites.net" ], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s/<app_name>", "isDefaultContainer": null, "kind": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "location": "West Europe", "maxNumberOfWorkers": null, "microService": "false", "name": "<app_name>", "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "<app_name>", "reserved": false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": false, "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "state": "Running", "suspendedTill": null, "tags": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normal" }</span></span>

<span data-ttu-id="fce61-183">Navigera till HTTPS-slutpunkten för ditt anpassade domännamn i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fce61-183">In your browser, navigate to HTTPS endpoint of your custom domain name.</span></span> <span data-ttu-id="fce61-184">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fce61-184">For example:</span></span>

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a><span data-ttu-id="fce61-186">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="fce61-186">More resources</span></span>

<span data-ttu-id="fce61-187">[Köp och konfigurera ett anpassat domännamn i Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[köpa och konfigurera ett SSL-certifikat för Azure App Service](web-sites-purchase-ssl-web-site.md)</span><span class="sxs-lookup"><span data-stu-id="fce61-187">[Buy and Configure a custom domain name in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[Buy and Configure an SSL Certificate for your Azure App Service](web-sites-purchase-ssl-web-site.md)</span></span>
