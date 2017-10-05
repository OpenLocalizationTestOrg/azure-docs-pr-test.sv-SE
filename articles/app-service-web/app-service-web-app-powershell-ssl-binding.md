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
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Service SSL-Certifikatbindning med hjälp av PowerShell
Med versionen av Microsoft Azure PowerShell version 1.1.0 en ny cmdlet lagts till som skulle ge användaren möjlighet att binda befintliga eller nya SSL-certifikat till en befintlig Webbapp.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Läs om hur du använder Azure Resource Manager baserade Azure PowerShell-cmdlets för att hantera ditt webbprogram Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Ladda upp och binda ett nytt SSL-certifikat
Scenario: Användaren vill binda ett SSL-certifikat till en av sina webbprogram.

Veta resursgruppens namn som innehåller webbappen, webbprogramnamnet, certifikat PFX sökvägen till filen på användarens dator, lösenordet för certifikatet och det anpassade värdnamnet kan vi använda följande PowerShell-kommando för att skapa den SSL-bindningen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Observera att du måste ha ett värdnamn (anpassad domän) som redan har konfigurerats för att innan du lägger till en SSL-bindning till en webbapp. Om värdnamnet inte har konfigurerats så att du får ett fel 'hostname' inte finns när du kör New-AzureRmWebAppSSLBinding. Du kan lägga till ett värdnamn direkt från portalen eller med hjälp av Azure PowerShell. Följande PowerShell-fragment kan vara att konfigurera värdnamnet innan du kör New-AzureRmWebAppSSLBinding.   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

Det är viktigt att förstå att cmdlet Set-AzureRmWebApp skriver över värdnamn för webbprogrammet. Därför ovan PowerShell fragment att lägga till i den befintliga listan med värdnamn för webbprogrammet.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Ladda upp och binda ett SSL-certifikat
Scenario: Användaren vill binda ett tidigare överförda SSL-certifikat till en av sina webbprogram.

Vi kan hämta listan över certifikat som redan har överförts till en viss resursgrupp med hjälp av följande kommando

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Observera att certifikat som är lokala för en specifik plats och resursgruppen, användaren behöver ladda upp certifikatet igen om det konfigurerade webbprogrammet finns på en annan plats och resursgrupp andra som som nödvändiga certifikat 

Veta resursgruppens namn som innehåller webbappen, webbprogramnamnet, tumavtrycket för certifikatet och det anpassade värdnamnet kan vi använda följande PowerShell-kommando för att skapa den SSL-bindningen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Om du tar bort en befintlig SSL-bindning
Scenario: Användaren vill ta bort en befintlig SSL-bindning.

Vi vet resursgruppens namn som innehåller webbappen, webbprogramnamnet och det anpassade värdnamnet, kan du använda följande PowerShell-kommando att ta bort den SSL-bindningen:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Observera att om den borttagna SSL-bindningen har den senaste bindning med certifikatet i den plats som standard certifikatet kommer att tas bort, om användaren vill behålla det certifikat som han kan använda alternativet DeleteCertificate att certifikatet

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Referenser
* [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
* [Introduktion till App Service-miljöer](app-service-app-service-environment-intro.md)
* [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)

