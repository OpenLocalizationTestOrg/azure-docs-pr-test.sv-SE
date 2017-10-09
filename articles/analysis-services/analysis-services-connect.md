---
title: aaaConnect tooAzure Analysis Services | Microsoft Docs
description: "Lär dig hur tooconnect tooand hämta data från en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 5df94492feb48034f156b72e83e1009683988fc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-analysis-services-server"></a>Ansluta tooan Azure Analysis Services-server

Den här artikeln beskriver anslutande tooa server med hjälp av datamodellering och av hanteringsprogram som SQL Server Management Studio (SSMS) eller SQL Server Data Tools (SSDT). Eller med klienten reporting program som Microsoft Excel, Power BI Desktop eller anpassade program. Anslutningar tooAzure Analysis Services använder HTTPS.

## <a name="client-libraries"></a>Klientbibliotek
[Hämta senaste hello-klientbibliotek](analysis-services-data-providers.md)

Alla anslutningar tooa server, oavsett typ, kräver uppdaterade AMO och ADOMD.NET OLEDB libraries tooconnect tooand klientgränssnitt med en Analysis Services-server. För SSMS, SSDT, Excel 2016 och Power BI hello senaste klientbibliotek installeras eller uppdateras med månatliga versioner. I vissa fall kan är det dock ett program kan inte ha hello senaste. När principer fördröjning uppdateringar eller uppdateringar för Office 365 finns till exempel på hello uppskjuten kanal.

## <a name="server-name"></a>servernamn

När du skapar en Analysis Services-server i Azure kan ange du ett unikt namn och hello område där hello server är toobe som skapats. När du anger hello servernamnet i en anslutning är hello namngivningsschemat för servern:

```
<protocol>://<region>/<servername>
```
 Där protokollet är sträng **asazure**, region är hello Uri där hello servern skapades (till exempel westus.asazure.windows.net) och servernamn är serverns unika inom hello region hello namn.

### <a name="get-hello-server-name"></a>Hämta hello-servernamn
I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hello hela servernamn. Om andra användare i din organisation ansluter toothis server för, kan du dela det här Servernamnet med dem. När du anger ett servernamn måste hello hela sökvägen användas.

![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Anslutningssträng

När du ansluter tooAzure hello Analysis Services med hjälp av objektet Tabellmodell, Använd hello efter anslutning strängformat:

###### <a name="integrated-azure-active-directory-authentication"></a>Azure Active Directory-integrerad autentisering
Integrerad autentisering hämtar hello Azure Active Directory cacheminne om de är tillgängliga. Om inte, hello Azure inloggningen visas.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-autentisering med användarnamn och lösenord

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows-autentisering (integrerad säkerhet)
Använd hello Windows-konto som kör hello aktuella processen.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Ansluta med en ODC-fil
Användare kan ansluta tooan Azure Analysis Services-servern med hjälp av en fil Office Data Connection (.odc) med äldre versioner av Excel. Det finns fler toolearn [skapa en fil Office Data Connection (.odc)](analysis-services-odc.md).


## <a name="next-steps"></a>Nästa steg
[Ansluta till Excel](analysis-services-connect-excel.md)    
[Ansluta med Powerbi](analysis-services-connect-pbi.md)   
[Hantera servern](analysis-services-manage.md)   

