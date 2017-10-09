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
# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App Service SSL-Certifikatbindning med hjälp av PowerShell
Hello-versionen av Microsoft Azure PowerShell version 1.1.0 har lagts till en ny cmdlet som ger hello användaren hello möjlighet toobind befintliga eller nya SSL-certifikat tooan befintliga Web App.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

toolearn om hur du använder Azure Resource Manager baserat Azure PowerShell-cmdlets toomanage Web Apps-Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Ladda upp och binda ett nytt SSL-certifikat
Scenario: hello användaren vill toobind ett SSL-certifikat tooone av sin webbprogram.

Veta hello resursgruppens namn som innehåller hello webbprogram, hello webbprogrammets namn, hello certifikat PFX sökväg på hello användaren datorn hello lösenordet för hello certifikatet och hello anpassade värdnamnet kan vi använda hello följande PowerShell-kommandot toocreate som SSL-bindning:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Observera att du, innan du lägger till en SSL-bindning tooa webbapp måste ha ett värdnamn (anpassad domän) som redan har konfigurerats. Om hello värdnamn inte är konfigurerad så att du får ett fel 'hostname' inte finns när du kör New-AzureRmWebAppSSLBinding. Du kan lägga till ett värdnamn direkt från hello-portalen eller med hjälp av Azure PowerShell. hello kan följande PowerShell-fragment vara tooconfigure hello värdnamn innan du kör New-AzureRmWebAppSSLBinding.   

    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   

Det är viktigt toounderstand som hello cmdlet Set-AzureRmWebApp skriver över hello värdnamn för hello webbprogrammet. Därför hello ovan PowerShell fragment att lägga till toohello befintlig lista över hello värdnamn för hello webbprogrammet.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Ladda upp och binda ett SSL-certifikat
Scenario: hello användaren vill toobind tooone för en tidigare överförda SSL-certifikatet av sin webbprogram.

Vi kan ha hello listan över certifikat redan överförda tooa specifika resursgrupp med hjälp av följande kommando hello

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Observera att hello certifikat är lokala tooa specifik plats och grupp, hello användare behöver toore-överföringen hello certifikat om hello konfigurerade webbprogram finns på en annan plats och resursgrupp andra som som hello behövs certifikat 

Veta hello resursgruppens namn som innehåller hello webbapp hello webbprogrammets namn, hello tumavtryck för certifikat och hello anpassade värdnamnet kan vi använda hello följande PowerShell-kommandot toocreate som SSL-bindning:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Om du tar bort en befintlig SSL-bindning
Scenario: hello användaren vill toodelete en befintlig SSL-bindning.

Veta hello resursgruppens namn som innehåller hello webbapp hello webbprogrammets namn och hello anpassade värdnamnet kan vi använda hello följande PowerShell-kommandot tooremove som SSL-bindning:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Observera att om hello bort SSL-bindning har hello senast bindning med certifikatet på den platsen kan som standard hello certifikat tas bort, om hello användaren vill tookeep hello certifikatet han kan använda hello DeleteCertificate alternativet tookeep hello certifikat

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Referenser
* [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
* [Introduktion tooApp-miljö](app-service-app-service-environment-intro.md)
* [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md)

