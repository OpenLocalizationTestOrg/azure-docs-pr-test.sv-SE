---
title: Anslut till Azure Analysis Services | Microsoft Docs
description: "Lär dig mer om att ansluta till och hämta data från en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: d6cafc72f74dc0ec891591d3311f93ba1649f922
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/02/2017
---
# <a name="connect-to-an-azure-analysis-services-server"></a>Ansluta till en Azure Analysis Services-server

Den här artikeln beskriver ansluter till en server med hjälp av datamodellering och av hanteringsprogram som SQL Server Management Studio (SSMS) eller SQL Server Data Tools (SSDT). Eller med klienten reporting program som Microsoft Excel, Power BI Desktop eller anpassade program. Anslutningar till Azure Analysis Services använder HTTPS.

## <a name="client-libraries"></a>Klientbibliotek
[Hämta senaste klientbibliotek](analysis-services-data-providers.md)

Alla anslutningar till en server, oavsett typ, kräver uppdaterade AMO och ADOMD.NET OLEDB klientbibliotek att ansluta till och gränssnitt med en Analysis Services-server. För SSMS, SSDT, Excel 2016 och Power BI, senaste klientbibliotek installeras eller uppdateras med månatliga versioner. I vissa fall kan är det dock ett program kan inte ha den senaste versionen. När principer fördröjning uppdateras eller uppdateringar för Office 365 är till exempel på uppskjuten kanalen.

## <a name="server-name"></a>servernamn

När du skapar en Analysis Services-server i Azure kan ange du ett unikt namn och den region där servern är skapas. När du anger servernamnet i en anslutning är namngivningsschemat server:

```
<protocol>://<region>/<servername>
```
 Där protokollet är sträng **asazure**, region är URI: N där servern skapades (till exempel westus.asazure.windows.net) och servername är namnet på servern unikt för regionen.

### <a name="get-the-server-name"></a>Hämta namnet på servern
I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hela servernamnet. Om andra användare i din organisation ansluter till den här servern för, kan du dela det här Servernamnet med dem. När du anger ett servernamn måste hela sökvägen användas.

![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Anslutningssträng

När du ansluter till Azure Analysis Services format med hjälp av objektet Tabellmodellen, Använd följande anslutningssträng:

###### <a name="integrated-azure-active-directory-authentication"></a>Azure Active Directory-integrerad autentisering
Integrerad autentisering hämtar cache för Azure Active Directory-autentiseringsuppgifter om det är tillgängligt. Om inte, visas fönstret Azure-inloggning.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-autentisering med användarnamn och lösenord

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows-autentisering (integrerad säkerhet)
Använd Windows-kontot som kör den aktuella processen.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Ansluta med en ODC-fil
Med äldre versioner av Excel kan användarna ansluta till en Azure Analysis Services-server med hjälp av en fil Office Data Connection (.odc). Läs mer i [skapa en fil Office Data Connection (.odc)](analysis-services-odc.md).


## <a name="next-steps"></a>Nästa steg
[Ansluta till Excel](analysis-services-connect-excel.md)    
[Ansluta med Powerbi](analysis-services-connect-pbi.md)   
[Hantera servern](analysis-services-manage.md)   

