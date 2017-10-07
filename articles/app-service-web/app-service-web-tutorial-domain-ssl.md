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
# <a name="add-custom-domain-and-ssl-tooan-azure-web-app"></a>Lägg till anpassade domäner och SSL tooan Azure webbapp

Den här kursen visar hur tooquickly mappa en anpassad domän namn tooyour Azure webbapp och skydda den med ett anpassat SSL-certifikat. 

## <a name="before-you-begin"></a>Innan du börjar

Innan du kör det här exemplet installerar hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) lokalt.

Du måste också administrativ åtkomst toohello DNS-konfigurationssidan för respektive domän-providern. Till exempel tooadd `www.contoso.com`, behöver du toobe kan tooconfigure DNS-poster för `contoso.com`.

Slutligen måste en giltig. PFX-filen _och_ lösenordet för hello SSL-certifikat som du vill tooupload och bind. SSL-certifikatet ska vara konfigurerade toosecure hello domännamn du vill använda. I ovanstående exempel hello, SSL-certifikat ska skydda `www.contoso.com`. 

## <a name="step-1---create-an-azure-web-app"></a>Steg 1 – skapa ett Azure-webbapp

### <a name="log-in-tooazure"></a>Logga in tooAzure

Vi kan nu gå toouse hello Azure CLI 2.0 i ett terminalfönster toocreate hello resurser behövs toohost vår Node.js-app i Azure.  Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar. 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a>Skapa en resursgrupp   
Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create). En Azure-resursgrupp är en logisk behållare som Azure-resurser (t.ex. webbappar, databaser och lagringskonton) distribueras och hanteras i. 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

toosee vilka möjliga värden du kan använda för `---location`, använda hello `az appservice list-locations` Azure CLI-kommando.

## <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

hello följande exempel skapas en apptjänstplan med namnet `myAppServicePlan` med hello **grundläggande** prisnivån.

Skapa AZ programtjänstplan--namnet myAppServicePlan--resursgrupp myResourceGroup--sku B1

När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel. 

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

Nu när en App Service-plan har skapats kan du skapa en webbapp inom hello `myAppServicePlan` App Service-plan. hello web app ger du är en värd utrymme toodeploy koden som innehåller en URL för du tooview hello distribuerat program. Använd hello [az apptjänst web skapa](/cli/azure/appservice/web#create) kommandot toocreate hello-webbprogram. 

I hello kommandot nedan ersätta ditt eget unikt appnamn där du ser hello `<app_name>` platshållare. Detta unika namn används under hello hello standarddomännamnet för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure. Du kan mappa ett anpassat webbprogram för DNS-posten toohello senare innan du exponera tooyour användare. 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel. 

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

Från hello JSON-utdata `defaultHostName` visar standarddomännamnet för ditt webbprogram. Navigera toothis adress i webbläsaren.

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a>Steg 2 – konfigurera DNS-mappning

I det här steget kan du lägga till en mappning från en anpassad domän tooyour webbprogram standarddomännamnet `<app_name>.azurewebsites.net`. Normalt utför du det här steget i domänen leverantörens webbplats. Varje domänregistrator webbplats är något annorlunda, så bör du kontakta din leverantör dokumentation. Här är några allmänna riktlinjer. 

### <a name="navigate-tootoodns-management-page"></a>Navigera tootooDNS sidan för hantering

Logga först in tooyour domänregistrators webbplats.  

Leta sedan upp hello sidan för hantering av DNS-poster. Leta efter länkar eller delar av hello plats med etiketten **domännamn**, **DNS**, eller **namn serverhantering**. Du kan ofta hitta hello länken genom att visa din kontoinformation och söker efter en länk som **min domäner**.

När du hittar den här sidan kan du leta efter en länk där du kan lägga till eller redigera DNS-poster. Detta kan bero på en **zonfilen** eller **DNS-poster** länken, eller en **avancerad konfiguration** länk.

### <a name="create-a-cname-record"></a>Skapa en CNAME-post

Lägg till en CNAME-post som mappar hello önskad underdomän namn tooyour webbprogram standarddomännamnet (`<app_name>.azurewebsites.net`, där `<app_name>` är unikt namn för din app).

För hello `www.contoso.com` exempelvis skapa en CNAME-post som mappar hello `www` värdnamn för`<app_name>.azurewebsites.net`.

## <a name="step-3---configure-hello-custom-domain-on-your-web-app"></a>Steg 3 – Konfigurera hello anpassade domäner på ditt webbprogram

När du har konfigurerat hello hostname mappning i domänen leverantörens webbplats, är du redo tooconfigure hello anpassade domäner på ditt webbprogram. Använd hello [az apptjänst web config hostname lägga till](/cli/azure/appservice/web/config/hostname#add) kommandot tooadd den här konfigurationen. 

I hello kommandot nedan, ersätta `<app_name>` med unika namn och < your_custom_domain > med hello fullständigt kvalificerade domännamn (t.ex. `www.contoso.com`). 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

hello domänen är nu fullständigt mappade tooyour webbprogram. Navigera toohello domännamn i webbläsaren. Exempel:

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-tooyour-web-app"></a>Steg 4 – binda ett anpassat webbprogram för SSL-certifikat tooyour

Nu har du ett Azure webbapp med hello domännamn som du vill använda i hello webbläsarens adressfält. Men om du navigerar toohello `https://<your_custom_domain>` nu kan du få ett certifikat. 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

Det här felet beror på att webbappen inte har en SSL-certifikat-bindning som matchar din anpassade domännamn. Men du inte får ett felmeddelande om du navigerar för`https://<app_name>.azurewebsites.net`. Detta beror på att din app, samt alla appar i Azure App Service, skyddas med hello SSL-certifikat för hello `*.azurewebsites.net` jokerteckendomän som standard. 

I ordning tooaccess ditt webbprogram med ditt domännamn måste toobind hello SSL-certifikat för webbappen domänen toohello. Du kan göra det i det här steget. 

### <a name="upload-hello-ssl-certificate"></a>Överför hello SSL-certifikat

Överför hello SSL-certifikat för ditt anpassade domäner tooyour webbprogram med hjälp av hello [az apptjänst web config ssl överför](/cli/azure/appservice/web/config/ssl#upload) kommando.

I hello kommandot nedan, ersätta `<app_name>` med din unikt appnamn `<path_to_ptx_file>` med hello sökvägen tooyour. PFX-filen och `<password>` med ditt certifikat lösenord. 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

När hello certifikat har överförts visar hello Azure CLI information liknande toohello följande exempel:

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

Från hello JSON-utdata `thumbprint` visar överförda certifikatets tumavtryck. Kopiera värdet för hello nästa steg.

### <a name="bind-hello-uploaded-ssl-certificate-toohello-web-app"></a>Bind hello upp SSL-certifikat toohello webbprogram

Ditt webbprogram har nu hello domännamn som du vill använda och har ett SSL-certifikat som skyddar den egna domänen. hello är endast sak vänstra toodo toobind hello överförda certifikat toohello webbapp. Du kan göra detta med hjälp av hello [az apptjänst web config ssl-bindning](/cli/azure/appservice/web/config/ssl#bind) kommando.

I hello kommandot nedan, ersätta `<app_name>` med din unika namn och `<thumbprint-from-previous-output>` med hello tumavtrycket för certifikatet som du får från hello föregående kommando. 

AZ apptjänst web config ssl-bindning--name < programnamn >--resursgrupp myResourceGroup--tumavtryck < tumavtryck-från-tidigare-output >--ssl-typen SNI

När hello certifikat är bundet tooyour webbprogram, visar hello Azure CLI information liknande toohello följande exempel:

{”availabilityState”: ”Normal”, ”clientAffinityEnabled”: värdet är true, ”clientCertEnabled”: false, ”cloningInfo”: null, ”containerSize”: 0, ”dailyMemoryTimeQuota”: 0, ”defaultHostName” ”: < programnamn >. azurewebsites.net”, ”enabled”: värdet är true ”, enabledHostNames ”: [” www.contoso.com ””, < programnamn >. azurewebsites.net ””, < programnamn >. scm.azurewebsites.net ”],” gatewaySiteName ”: null,” hostNameSslStates ”: [{” hostType ”:” Standard ”,” name ””: < programnamn >. azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” databas ”,” name ””: < programnamn >. scm.azurewebsites.net ”,” sslState ”:” inaktiverad ”,” tumavtryck ”: null,” toUpdate ”: null,” virtualIp ”: null}, {” hostType ”:” Standard ”,” name ”:” www.contoso.com ”,” sslState ”:” SniEnabled ”,” tumavtryck ”:” 9FD1D2D06E2293673E2A8D1CA484A092BD016D00 ”,” toUpdate ”: null,” virtualIp ”: null}],” värdnamn ”: [” www.contoso.com ”,” < programnamn >. azurewebsites.NET ”],” hostNamesDisabled ”: false,” hostingEnvironmentProfile ”: null,” id ””: /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s / < programnamn > ””, isDefaultContainer ”: null” typ ”:” WebApp ”,” lastModifiedTimeUtc ””: 2017-03-29T14:36:18.803333 ”,” plats ”:” västra Europa ”,” maxNumberOfWorkers ”: null,” Mikrotjänster ”:” false ”,” namn ”:” < programnamn > ”,” outboundIpAddresses ””: 13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1 ”,” premiumAppDeployed ”: null,” repositorySiteName ”:” < programnamn > ”,” reserverade ”: false,” resourceGroup ”:” myResourceGroup ”,” scmSiteAlsoStopped ”: false,” serverFarmId ”: ”/ prenumerationer/00000000-0000-0000-0000-000000000000/resursgrupper/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan”, ”siteConfig”: null, ”slotSwapStatus”: null ”tillstånd”: ”körs”, ”suspendedTill”: null ”taggar”: null, ”targetSwapSlot”: null, ”trafficManagerHostNames”: null, ”typ”: ”Microsoft.Web/sites”, ”usageState”: ”Normal”}

Navigera tooHTTPS slutpunkten för ditt anpassade domännamn i webbläsaren. Exempel:

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a>Fler resurser

[Köp och konfigurera ett anpassat domännamn i Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[köpa och konfigurera ett SSL-certifikat för Azure App Service](web-sites-purchase-ssl-web-site.md)
