---
title: "aaaGet data med hjälp av hello Azure AD Reporting API med certifikat | Microsoft Docs"
description: "Förklarar hur toouse hello Azure AD Reporting API med certifikatet autentiseringsuppgifter tooget data från kataloger utan åtgärder från användaren."
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/24/2017
ms.author: ramical
ms.openlocfilehash: 00ddfaefe32ea6ae48f276c974a17ddcf84f7894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-data-using-hello-azure-ad-reporting-api-with-certificates"></a>Hämta data med certifikat hello Azure AD Reporting API
Den här artikeln beskrivs hur toouse hello Azure AD Reporting API med certifikatet autentiseringsuppgifter tooget data från kataloger utan åtgärder från användaren. 

## <a name="use-hello-azure-ad-reporting-api"></a>Använd hello Azure AD Reporting API 
Azure AD Reporting API kräver att du har slutfört hello följande steg:
 *  Installationskrav
 *  Ange hello certifikat i din app
 *  Hämta en åtkomsttoken
 *  Använd hello åtkomst-token toocall hello Graph API

Information om källkoden finns i [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Använda Report API-modulen). 

### <a name="install-prerequisites"></a>Installationskrav
Du behöver toohave Azure AD PowerShell V2 och AzureADUtils module installerad.

1. Hämta och installera Azure AD Powershell V2, hello instruktionerna på [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md).
2. Hämta hello verktyg för Azure AD webbplatsuppgradering modul från [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1). 
  Den här modulen tillhandahåller flera verktygs-cmdlets, däribland:
   * hello senaste versionen av ADAL med hjälp av Nuget
   * Åtkomsttoken från användare, programnycklar och certifikat med ADAL
   * Växlingsbara resultat för Graph API-hantering

**tooinstall hello verktyg för Azure AD webbplatsuppgradering modul:**

1. Skapa en katalog toosave hello verktyg modul (till exempel c:\azureAD) och hämta hello modul från GitHub.
2. Öppna ett PowerShell-session och gå toohello katalog som du nyss skapade. 
3. Importera hello-modulen och installera den i hello PowerShell modul-sökväg med hello installera AzureADUtilsModule cmdlet. 

hello session bör se ut ungefär toothis skärm:

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-hello-certificate-in-your-app"></a>Ange hello certifikat i din app
1. Om du redan har en app kan du hämta dess objekt-ID från hello Azure-portalen. 

  ![Azure Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Öppna ett PowerShell-session och Anslut tooAzure AD med hello Connect-AzureAD cmdlet.

  ![Azure Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Använd hello ny AzureADApplicationCertificateCredential cmdlet från AzureADUtils tooadd tooit en certifikat-autentiseringsuppgifter. 

>[!Note]
>Du behöver tooprovide hello programmet objekt-ID som du hämtat tidigare, samt hello certifikatobjekt (hämta detta med hjälp av hello Cert: enheten).
>


  ![Azure Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Hämta en åtkomsttoken

tooget ett åtkomsttoken, Använd hello Get-AzureADGraphAPIAccessTokenFromCert cmdlet från AzureADUtils. 

>[!NOTE]
>Du måste toouse hello program-ID i stället för hello objekt-ID som du använde i hello sista avsnittet.
>

 ![Azure Portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-hello-access-token-toocall-hello-graph-api"></a>Använd hello åtkomst-token toocall hello Graph API

Du kan nu skapa hello skript. Nedan visas ett exempel med hello Invoke-AzureADGraphAPIQuery cmdlet från hello AzureADUtils. Denna cmdlet hanterar flera växlingsbara resultat, och skickar sedan dessa resultat toohello PowerShell-pipeline. 

 ![Azure Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

Du är nu redo tooexport tooa CSV och spara tooa SIEM-system. Du kan också omsluta skriptet i en schemalagd aktivitet tooget Azure AD-data från din klient regelbundet utan toostore programnycklar i hello källkod. 

## <a name="next-steps"></a>Nästa steg
[hello grunderna i Azure Identitetshantering](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>



