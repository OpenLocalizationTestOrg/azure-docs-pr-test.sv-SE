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
# <a name="add-custom-domain-and-ssl-to-an-azure-web-app"></a>Lägga till anpassade domäner och SSL i en Azure-webbapp

Den här kursen visar hur du snabbt mappa ett anpassat domännamn i Azure-webbappen och skydda den med ett anpassat SSL-certifikat. 

## <a name="before-you-begin"></a>Innan du börjar

Innan du kör det här exemplet måste du installera den [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) lokalt.

Du måste också administrativ åtkomst till sidan DNS-konfiguration för respektive domän-providern. Till exempel för att lägga till `www.contoso.com`, du behöver för att kunna konfigurera DNS-poster för `contoso.com`.

Slutligen måste en giltig. PFX-filen _och_ lösenordet för SSL-certifikatet som du vill ladda upp och binda. SSL-certifikatet ska konfigureras för att skydda det anpassade domännamnet som du vill använda. I exemplet ovan SSL-certifikat ska skydda `www.contoso.com`. 

## <a name="step-1---create-an-azure-web-app"></a>Steg 1 – skapa ett Azure-webbapp

### <a name="log-in-to-azure"></a>Logga in på Azure

Nu ska vi använda Azure CLI 2.0 i ett terminalfönster för att skapa de resurser som behövs för att använda Azure som värd för Node.js-appen.  Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen. 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a>Skapa en resursgrupp   
Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create). En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. webbappar, databaser och lagringskonton) distribueras och hanteras i. 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

Om du vill se vilka värden som kan användas för `---location` använder du Azure CLI-kommandot `az appservice list-locations`.

## <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

Skapa en App Service-plan med kommandot [az appservice plan create](/cli/azure/appservice/plan#create). 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

I följande exempel skapas en apptjänstplan med namnet `myAppServicePlan` med hjälp av den **grundläggande** prisnivån.

Skapa AZ programtjänstplan--namnet myAppServicePlan--resursgrupp myResourceGroup--sku B1

När programtjänstplanen har skapats, Azure CLI visar information liknar följande exempel. 

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

## <a name="create-a-web-app"></a>Skapa en webbapp

Nu när en App Service-plan har skapats kan du skapa en webbapp i `myAppServicePlan` App Service-planen. Web app kan du är värd kan du distribuera din kod som innehåller en URL som du kan visa det distribuerade programmet. Använd kommandot [az appservice web create](/cli/azure/appservice/web#create) för att skapa webbappen. 

I det här kommandot nedan ersätta ditt eget unikt appnamn där du ser den `<app_name>` platshållare. Detta unika namn används som en del av standarddomännamnet för webbprogram, så att namnet måste vara unikt över alla program i Azure. Du kan senare mappa en anpassad DNS-post till webbappen innan du exponerar den för användarna. 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

När webbappen har skapats visas information av Azure CLI. Informationen ser ut ungefär som i följande exempel. 

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

Från JSON-utdata `defaultHostName` visar standarddomännamnet för ditt webbprogram. Navigera till den här adressen i din webbläsare.

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a>Steg 2 – konfigurera DNS-mappning

I det här steget kan du lägga till en mappning från en anpassad domän till ditt webbprogram standarddomännamnet `<app_name>.azurewebsites.net`. Normalt utför du det här steget i domänen leverantörens webbplats. Varje domänregistrator webbplats är något annorlunda, så bör du kontakta din leverantör dokumentation. Här är några allmänna riktlinjer. 

### <a name="navigate-to-to-dns-management-page"></a>Navigera till att DNS-sidan för hantering

Först logga in till din domänregistrator webbplats.  

Hitta sedan sidan för hantering av DNS-poster. Leta efter länkar eller områden på webbplatsen med namnet **domännamn**, **DNS**, eller **namn serverhantering**. Ofta, kan du hitta länken genom att visa din kontoinformation och söker efter en länk som **min domäner**.

När du hittar den här sidan kan du leta efter en länk där du kan lägga till eller redigera DNS-poster. Detta kan bero på en **zonfilen** eller **DNS-poster** länken, eller en **avancerad konfiguration** länk.

### <a name="create-a-cname-record"></a>Skapa en CNAME-post

Lägg till en CNAME-post som mappar önskade underdomännamnet till ditt webbprogram standarddomännamnet (`<app_name>.azurewebsites.net`, där `<app_name>` är unikt namn för din app).

För den `www.contoso.com` exempelvis skapa en CNAME-post som mappar den `www` värdnamn till `<app_name>.azurewebsites.net`.

## <a name="step-3---configure-the-custom-domain-on-your-web-app"></a>Steg 3 – Konfigurera anpassade domäner på ditt webbprogram

När du har konfigurerat hostname mappningen i domänen leverantörens webbplats, är du redo att konfigurera den anpassade domänen på ditt webbprogram. Använd den [az apptjänst web config hostname lägga till](/cli/azure/appservice/web/config/hostname#add) kommando för att lägga till den här konfigurationen. 

I kommandot nedan ersätta `<app_name>` med unika namn och < your_custom_domain > med det fullständigt kvalificerade domännamnet (t.ex. `www.contoso.com`). 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

Den anpassade domänen nu mappas fullständigt till ditt webbprogram. Navigera till det anpassade domännamnet i din webbläsare. Exempel:

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-to-your-web-app"></a>Steg 4 – binda ett anpassat SSL-certifikat till ditt webbprogram

Nu har du ett Azure webbapp med domännamn som du vill använda i webbläsarens adressfält. Men om du navigerar till den `https://<your_custom_domain>` nu kan du få ett certifikat. 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

Det här felet beror på att webbappen inte har en SSL-certifikat-bindning som matchar din anpassade domännamn. Men du inte får ett felmeddelande om du navigerar till `https://<app_name>.azurewebsites.net`. Detta beror på att din app, samt alla appar i Azure App Service, skyddas med SSL-certifikatet för den `*.azurewebsites.net` jokerteckendomän som standard. 

För att komma åt ditt webbprogram med ditt domännamn måste du binda SSL-certifikatet för den anpassade domänen till webbappen. Du kan göra det i det här steget. 

### <a name="upload-the-ssl-certificate"></a>Överför ett SSL-certifikat

Överför SSL-certifikatet för den anpassade domänen till ditt webbprogram med hjälp av den [az apptjänst web config ssl överför](/cli/azure/appservice/web/config/ssl#upload) kommando.

I kommandot nedan ersätta `<app_name>` med din unikt appnamn `<path_to_ptx_file>` med sökvägen till den. PFX-filen och `<password>` med ditt certifikat lösenord. 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

När certifikatet har överförts visar Azure CLI information liknar följande exempel:

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

Från JSON-utdata `thumbprint` visar överförda certifikatets tumavtryck. Kopiera värdet för nästa steg.

### <a name="bind-the-uploaded-ssl-certificate-to-the-web-app"></a>Binda överförda SSL-certifikatet till webbappen

Ditt webbprogram har nu det anpassade domännamnet som du vill använda och har ett SSL-certifikat som skyddar den egna domänen. Det enda som återstår att göra är att binda det överförda certifikatet till webbprogrammet. Du kan göra detta med hjälp av den [az apptjänst web config ssl-bindning](/cli/azure/appservice/web/config/ssl#bind) kommando.

I kommandot nedan ersätta `<app_name>` med din unika namn och `<thumbprint-from-previous-output>` med tumavtrycket för certifikatet som du får från det föregående kommandot. 

AZ apptjänst web config ssl-bindning--name < programnamn >--resursgrupp myResourceGroup--tumavtryck < tumavtryck-från-tidigare-output >--ssl-typen SNI

När certifikatet är bunden till ditt webbprogram, visar Azure CLI information liknar följande exempel:

{”availabilityState”: ”Normal”, ”clientAffinityEnabled”: värdet är true, ”clientCertEnabled”: false, ”cloningInfo”: null, ”containerSize”: 0, ”dailyMemoryTimeQuota”: 0, ”defaultHostName” ”: < programnamn >. azurewebsites.net”, ”enabled”: värdet är true ”, enabledHostNames ”: [” www.contoso.com ””, < programnamn >. azurewebsites.net ””, < programnamn >. scm.azurewebsites.net ”],” gatewaySiteName ”: null,” hostNameSslStates ”: [{” hostType ”:” Standard ”,” name ””: < programnamn >. azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” databas ”,” name ””: < programnamn >. scm.azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” Standard ”,” name ”:” www.contoso.com ”,” sslState ”:” SniEnabled ”,” tumavtryck ”:” 9FD1D2D06E2293673E2A8D1CA484A092BD016D00 ”,” toUpdate ”: null,” virtualIp ”: null}],” värdnamn ”: [” www.contoso.com ”,” < programnamn >. azurewebsites.NET ”],” hostNamesDisabled ”: false,” hostingEnvironmentProfile ”: null,” id ””: /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < programnamn > ””, isDefaultContainer ”: null” typ ”:” WebApp ”,” lastModifiedTimeUtc ””: 2017-03-29T14:36:18.803333 ”,” plats ”:” västra Europa ”,” maxNumberOfWorkers ”: null,” Mikrotjänster ”:” false ”,” namn ”:” < programnamn > ”,” outboundIpAddresses ””: 13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1 ”,” premiumAppDeployed ”: null,” repositorySiteName ”:” < programnamn > ”,” reserverade ”: false,” resourceGroup ”:” myResourceGroup ”,” scmSiteAlsoStopped ”: false,” serverFarmId ”: ”/ prenumerationer/00000000-0000-0000-0000-000000000000/resursgrupper/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan”, ”siteConfig”: null, ”slotSwapStatus”: null ”tillstånd”: ”körs”, ”suspendedTill”: null ”taggar”: null, ”targetSwapSlot”: null, ”trafficManagerHostNames”: null, ”typ”: ”Microsoft.Web/sites”, ”usageState”: ”Normal”}

Navigera till HTTPS-slutpunkten för ditt anpassade domännamn i din webbläsare. Exempel:

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a>Fler resurser

[Köp och konfigurera ett anpassat domännamn i Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[köpa och konfigurera ett SSL-certifikat för Azure App Service](web-sites-purchase-ssl-web-site.md)
