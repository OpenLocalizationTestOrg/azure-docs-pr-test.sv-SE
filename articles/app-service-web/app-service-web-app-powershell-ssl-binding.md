---
title: "aaaSSL certifikat-bindning med hjälp av PowerShell"
description: "Lär dig hur toobind SSL-certifikat tooyour webbprogram med hjälp av PowerShell."
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
ms.openlocfilehash: 82f0e7c796da99ab50f69f3638ef64d55a94fc8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a><span data-ttu-id="9f497-103">Azure App Service SSL-Certifikatbindning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f497-103">Azure App Service SSL Certificate Binding using PowerShell</span></span>
<span data-ttu-id="9f497-104">Hello-versionen av Microsoft Azure PowerShell version 1.1.0 har lagts till en ny cmdlet som ger hello användaren hello möjlighet toobind befintliga eller nya SSL-certifikat tooan befintliga Web App.</span><span class="sxs-lookup"><span data-stu-id="9f497-104">With hello release of Microsoft Azure PowerShell version 1.1.0 a new cmdlet has been added that would give hello user hello ability toobind existing or new SSL certificates tooan existing Web App.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="9f497-105">toolearn om hur du använder Azure Resource Manager baserat Azure PowerShell-cmdlets toomanage Web Apps-Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="9f497-105">toolearn about using Azure Resource Manager based Azure PowerShell cmdlets toomanage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="uploading-and-binding-a-new-ssl-certificate"></a><span data-ttu-id="9f497-106">Ladda upp och binda ett nytt SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="9f497-106">Uploading and Binding a new SSL certificate</span></span>
<span data-ttu-id="9f497-107">Scenario: hello användaren vill toobind ett SSL-certifikat tooone av sin webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9f497-107">Scenario: hello user would like toobind an SSL certificate tooone of his web apps.</span></span>

<span data-ttu-id="9f497-108">Veta hello resursgruppens namn som innehåller hello webbprogram, hello webbprogrammets namn, hello certifikat PFX sökväg på hello användaren datorn hello lösenordet för hello certifikatet och hello anpassade värdnamnet kan vi använda hello följande PowerShell-kommandot toocreate som SSL-bindning:</span><span class="sxs-lookup"><span data-stu-id="9f497-108">Knowing hello resource group name that contains hello web app, hello web app name, hello certificate .pfx file path on hello user machine, hello password for hello certificate, and hello custom hostname, we can use hello following PowerShell command toocreate that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

<span data-ttu-id="9f497-109">Observera att du, innan du lägger till en SSL-bindning tooa webbapp måste ha ett värdnamn (anpassad domän) som redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="9f497-109">Note that before adding a SSL binding tooa web app, you must have a host name (custom domain) already configured.</span></span> <span data-ttu-id="9f497-110">Om hello värdnamn inte är konfigurerad så att du får ett fel 'hostname' inte finns när du kör New-AzureRmWebAppSSLBinding.</span><span class="sxs-lookup"><span data-stu-id="9f497-110">If hello host name is not configured , then you will get an error 'hostname' does not exist while running  New-AzureRmWebAppSSLBinding.</span></span> <span data-ttu-id="9f497-111">Du kan lägga till ett värdnamn direkt från hello-portalen eller med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9f497-111">You can add a hostname directly from hello portal or using Azure PowerShell.</span></span> <span data-ttu-id="9f497-112">hello kan följande PowerShell-fragment vara tooconfigure hello värdnamn innan du kör New-AzureRmWebAppSSLBinding.</span><span class="sxs-lookup"><span data-stu-id="9f497-112">hello following PowerShell snippet can be tooconfigure hello hostname before running New-AzureRmWebAppSSLBinding.</span></span>   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

<span data-ttu-id="9f497-113">Det är viktigt toounderstand som hello cmdlet Set-AzureRmWebApp skriver över hello värdnamn för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9f497-113">It is important toounderstand that hello Set-AzureRmWebApp cmdlet overwrites hello hostnames for hello web app.</span></span> <span data-ttu-id="9f497-114">Därför hello ovan PowerShell fragment att lägga till toohello befintlig lista över hello värdnamn för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9f497-114">Hence hello above PowerShell snippet is appending toohello existing list of hello host names for hello web app.</span></span>  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a><span data-ttu-id="9f497-115">Ladda upp och binda ett SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="9f497-115">Uploading and Binding an existing SSL certificate</span></span>
<span data-ttu-id="9f497-116">Scenario: hello användaren vill toobind tooone för en tidigare överförda SSL-certifikatet av sin webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9f497-116">Scenario: hello user would like toobind a previously uploaded SSL certificate tooone of his web apps.</span></span>

<span data-ttu-id="9f497-117">Vi kan ha hello listan över certifikat redan överförda tooa specifika resursgrupp med hjälp av följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="9f497-117">We can get hello list of certificates already uploaded tooa specific resource group by using hello following command</span></span>

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

<span data-ttu-id="9f497-118">Observera att hello certifikat är lokala tooa specifik plats och grupp, hello användare behöver toore-överföringen hello certifikat om hello konfigurerade webbprogram finns på en annan plats och resursgrupp andra som som hello behövs certifikat</span><span class="sxs-lookup"><span data-stu-id="9f497-118">Note that hello certificates are local tooa specific location and resource group, hello user need toore-upload hello certificate if hello configured web app is in a different location and resource group other that that of hello needed certificate</span></span> 

<span data-ttu-id="9f497-119">Veta hello resursgruppens namn som innehåller hello webbapp hello webbprogrammets namn, hello tumavtryck för certifikat och hello anpassade värdnamnet kan vi använda hello följande PowerShell-kommandot toocreate som SSL-bindning:</span><span class="sxs-lookup"><span data-stu-id="9f497-119">Knowing hello resource group name that contains hello web app, hello web app name, hello certificate thumbprint, and hello custom hostname, we can use hello following PowerShell command toocreate that SSL binding:</span></span>

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a><span data-ttu-id="9f497-120">Om du tar bort en befintlig SSL-bindning</span><span class="sxs-lookup"><span data-stu-id="9f497-120">Deleting an existing SSL binding</span></span>
<span data-ttu-id="9f497-121">Scenario: hello användaren vill toodelete en befintlig SSL-bindning.</span><span class="sxs-lookup"><span data-stu-id="9f497-121">Scenario: hello user would like toodelete an existing SSL binding.</span></span>

<span data-ttu-id="9f497-122">Veta hello resursgruppens namn som innehåller hello webbapp hello webbprogrammets namn och hello anpassade värdnamnet kan vi använda hello följande PowerShell-kommandot tooremove som SSL-bindning:</span><span class="sxs-lookup"><span data-stu-id="9f497-122">Knowing hello resource group name that contains hello web app, hello web app name, and hello custom hostname, we can use hello following PowerShell command tooremove that SSL binding:</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

<span data-ttu-id="9f497-123">Observera att om hello bort SSL-bindning har hello senast bindning med certifikatet på den platsen kan som standard hello certifikat tas bort, om hello användaren vill tookeep hello certifikatet han kan använda hello DeleteCertificate alternativet tookeep hello certifikat</span><span class="sxs-lookup"><span data-stu-id="9f497-123">Note that if hello removed SSL binding was hello last binding using that certificate in that location, by default hello certificate will be deleted, if hello user want tookeep hello certificate he can use hello DeleteCertificate option tookeep hello certificate</span></span>

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a><span data-ttu-id="9f497-124">Referenser</span><span class="sxs-lookup"><span data-stu-id="9f497-124">References</span></span>
* [<span data-ttu-id="9f497-125">Azure Resource Manager baserat PowerShell-kommandon för Azure Web App</span><span class="sxs-lookup"><span data-stu-id="9f497-125">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="9f497-126">Introduktion tooApp-miljö</span><span class="sxs-lookup"><span data-stu-id="9f497-126">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="9f497-127">Använda Azure PowerShell med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9f497-127">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

