---
title: SSL-certifikat-bindning med PowerShell
description: "Lär dig mer om att binda SSL-certifikat till ditt webbprogram med hjälp av PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 8ce32508-e982-48d3-b878-0e526afda537
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: a1fcc618fb0c68778e39cc227368a60b008f9401
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a><span data-ttu-id="3f4d9-103">Azure App Service SSL-Certifikatbindning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f4d9-103">Azure App Service SSL Certificate Binding using PowerShell</span></span>
<span data-ttu-id="3f4d9-104">Med versionen av Microsoft Azure PowerShell version 1.1.0 en ny cmdlet lagts till som skulle ge användaren möjlighet att binda befintliga eller nya SSL-certifikat till en befintlig Webbapp.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-104">With the release of Microsoft Azure PowerShell version 1.1.0 a new cmdlet has been added that would give the user the ability to bind existing or new SSL certificates to an existing Web App.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="3f4d9-105">Läs om hur du använder Azure Resource Manager baserade Azure PowerShell-cmdlets för att hantera ditt webbprogram Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="3f4d9-105">To learn about using Azure Resource Manager based Azure PowerShell cmdlets to manage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="uploading-and-binding-a-new-ssl-certificate"></a><span data-ttu-id="3f4d9-106">Ladda upp och binda ett nytt SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="3f4d9-106">Uploading and Binding a new SSL certificate</span></span>
<span data-ttu-id="3f4d9-107">Scenario: Användaren vill binda ett SSL-certifikat till en av sina webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-107">Scenario: The user would like to bind an SSL certificate to one of his web apps.</span></span>

<span data-ttu-id="3f4d9-108">Veta resursgruppens namn som innehåller webbappen, webbprogramnamnet, certifikat PFX sökvägen till filen på användarens dator, lösenordet för certifikatet och det anpassade värdnamnet kan vi använda följande PowerShell-kommando för att skapa den SSL-bindningen:</span><span class="sxs-lookup"><span data-stu-id="3f4d9-108">Knowing the resource group name that contains the web app, the web app name, the certificate .pfx file path on the user machine, the password for the certificate, and the custom hostname, we can use the following PowerShell command to create that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

<span data-ttu-id="3f4d9-109">Observera att du måste ha ett värdnamn (anpassad domän) som redan har konfigurerats för att innan du lägger till en SSL-bindning till en webbapp.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-109">Note that before adding a SSL binding to a web app, you must have a host name (custom domain) already configured.</span></span> <span data-ttu-id="3f4d9-110">Om värdnamnet inte har konfigurerats så att du får ett fel 'hostname' inte finns när du kör New-AzureRmWebAppSSLBinding.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-110">If the host name is not configured , then you will get an error 'hostname' does not exist while running  New-AzureRmWebAppSSLBinding.</span></span> <span data-ttu-id="3f4d9-111">Du kan lägga till ett värdnamn direkt från portalen eller med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-111">You can add a hostname directly from the portal or using Azure PowerShell.</span></span> <span data-ttu-id="3f4d9-112">Följande PowerShell-fragment kan vara att konfigurera värdnamnet innan du kör New-AzureRmWebAppSSLBinding.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-112">The following PowerShell snippet can be to configure the hostname before running New-AzureRmWebAppSSLBinding.</span></span>   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

<span data-ttu-id="3f4d9-113">Det är viktigt att förstå att cmdlet Set-AzureRmWebApp skriver över värdnamn för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-113">It is important to understand that the Set-AzureRmWebApp cmdlet overwrites the hostnames for the web app.</span></span> <span data-ttu-id="3f4d9-114">Därför ovan PowerShell fragment att lägga till i den befintliga listan med värdnamn för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-114">Hence the above PowerShell snippet is appending to the existing list of the host names for the web app.</span></span>  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a><span data-ttu-id="3f4d9-115">Ladda upp och binda ett SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="3f4d9-115">Uploading and Binding an existing SSL certificate</span></span>
<span data-ttu-id="3f4d9-116">Scenario: Användaren vill binda ett tidigare överförda SSL-certifikat till en av sina webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-116">Scenario: The user would like to bind a previously uploaded SSL certificate to one of his web apps.</span></span>

<span data-ttu-id="3f4d9-117">Vi kan hämta listan över certifikat som redan har överförts till en viss resursgrupp med hjälp av följande kommando</span><span class="sxs-lookup"><span data-stu-id="3f4d9-117">We can get the list of certificates already uploaded to a specific resource group by using the following command</span></span>

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

<span data-ttu-id="3f4d9-118">Observera att certifikat som är lokala för en specifik plats och resursgruppen, användaren behöver ladda upp certifikatet igen om det konfigurerade webbprogrammet finns på en annan plats och resursgrupp andra som som nödvändiga certifikat</span><span class="sxs-lookup"><span data-stu-id="3f4d9-118">Note that the certificates are local to a specific location and resource group, the user need to re-upload the certificate if the configured web app is in a different location and resource group other that that of the needed certificate</span></span> 

<span data-ttu-id="3f4d9-119">Veta resursgruppens namn som innehåller webbappen, webbprogramnamnet, tumavtrycket för certifikatet och det anpassade värdnamnet kan vi använda följande PowerShell-kommando för att skapa den SSL-bindningen:</span><span class="sxs-lookup"><span data-stu-id="3f4d9-119">Knowing the resource group name that contains the web app, the web app name, the certificate thumbprint, and the custom hostname, we can use the following PowerShell command to create that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a><span data-ttu-id="3f4d9-120">Om du tar bort en befintlig SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="3f4d9-120">Deleting an existing SSL binding</span></span>
<span data-ttu-id="3f4d9-121">Scenario: Användaren vill ta bort en befintlig SSL-bindning.</span><span class="sxs-lookup"><span data-stu-id="3f4d9-121">Scenario: The user would like to delete an existing SSL binding.</span></span>

<span data-ttu-id="3f4d9-122">Vi vet resursgruppens namn som innehåller webbappen, webbprogramnamnet och det anpassade värdnamnet, kan du använda följande PowerShell-kommando att ta bort den SSL-bindningen:</span><span class="sxs-lookup"><span data-stu-id="3f4d9-122">Knowing the resource group name that contains the web app, the web app name, and the custom hostname, we can use the following PowerShell command to remove that SSL binding:</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

<span data-ttu-id="3f4d9-123">Observera att om den borttagna SSL-bindningen har den senaste bindning med certifikatet i den plats som standard certifikatet kommer att tas bort, om användaren vill behålla det certifikat som han kan använda alternativet DeleteCertificate att certifikatet</span><span class="sxs-lookup"><span data-stu-id="3f4d9-123">Note that if the removed SSL binding was the last binding using that certificate in that location, by default the certificate will be deleted, if the user want to keep the certificate he can use the DeleteCertificate option to keep the certificate</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a><span data-ttu-id="3f4d9-124">Referenser</span><span class="sxs-lookup"><span data-stu-id="3f4d9-124">References</span></span>
* [<span data-ttu-id="3f4d9-125">Azure Resource Manager baserat PowerShell-kommandon för Azure Web App</span><span class="sxs-lookup"><span data-stu-id="3f4d9-125">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="3f4d9-126">Introduktion till App Service-miljöer</span><span class="sxs-lookup"><span data-stu-id="3f4d9-126">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="3f4d9-127">Använda Azure PowerShell med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3f4d9-127">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

