---
title: "viktig information för aaaAzure Data Catalog | Microsoft Docs"
description: "Viktig information för Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 661826f66020ba72a885c6b14522b53c8b469d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-release-notes"></a>Azure Data Catalog viktig information
## <a name="notes-for-hello-november-20-2015-release-of-azure-data-catalog"></a>Information om hello 20 November 2015-versionen av Azure Data Catalog
### <a name="opening-data-sources-in-power-bi-desktop"></a>Öppna datakällor i Power BI Desktop
När du använder hello ”öppna i Power BI Desktop” alternativet från hello **Azure Data Catalog** Företagsportalen kan användare kan stöta på något av två problem i hello Power BI Desktop-program:

* En dialogruta med hello rubriken ”tooOpen dokument” visas
* hello Power BI Desktop-programmet öppnas men hello filen visas toobe tom

För varje situation hello problemet kan lösas genom att hämta och installera hello senaste versionen av Power BI Desktop från [PowerBI.com](https://powerbi.com).

## <a name="notes-for-hello-november-13-2015-release-of-azure-data-catalog"></a>Information om hello 13 November 2015-versionen av Azure Data Catalog
### <a name="registering-and-connecting-tooteradata"></a>Registrera och ansluta tooTeradata
När du ansluter tooTeradata datakällor användare måste ha installerat hello rätt Teradata ODBC-drivrutin som matchar hello bitness (32-bitars eller 64-bitars) hello programvara som används.

Från och med den här ADC utgivningsdatum, hello senaste [Teradata ODBC-drivrutin för windows (version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) är kompatibel med Office 2013, men inte med Office 2016.

## <a name="notes-for-hello-july-13-2015-release-of-azure-data-catalog"></a>Information om hello 13 juli 2015-versionen av Azure Data Catalog
### <a name="registering-and-connecting-toooracle-database"></a>Registrera och ansluta tooOracle databas
När ansluter tooOracle databasanvändare data sources måste ha installerat hello rätt Oracle drivrutiner som matchar hello bitness (32-bitars eller 64-bitars) hello programvara som används.

* När du registrerar Oracle-datakällor på en dator med 32-bitars Windows kommer hello 32-bitars Oracle drivrutiner att användas
* När du registrerar Oracle-datakällor på en dator som kör 64-bitars Windows kommer hello 64-bitars Oracle drivrutiner att användas
* När du ansluter tooOracle datakällor på en dator som kör hello 32-bitars version av Microsoft Office Excel, kommer inklusive på 64-bitars Windows hello 32-bitars Oracle drivrutiner att användas
* När du ansluter tooOracle datakällor på en dator som kör hello 64-bitarsversionen av Microsoft Office Excel kommer hello 64-bitars Oracle drivrutiner att användas

### <a name="registering-and-connecting-toosql-server-reporting-services"></a>Registrera och ansluta tooSQL Server Reporting Services
Stöd för SQL Server Reporting Services (SSRS) datakällor är för närvarande begränsad tooNative läge servrar. Stöd för SSRS i SharePoint-läge ska läggas till i en senare version.

### <a name="opening-data-assets-in-excel"></a>Öppna datatillgångar i Excel
När du öppnar datatillgångar i Microsoft Excel från hello **Azure Data Catalog** Företagsportalen kan användare uppmanas med en **säkerhetsmeddelande om Microsoft Excel** dialogrutan. Detta är standard, förväntat beteende och användare kan välja **aktivera** toocontinue.

Mer information finns i [aktivera eller inaktivera säkerhetsvarningar om länkar och filer från misstänkta webbplatser](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy- och konfiguration och data för registrering av datakälla
Användare kan stöta på en situation där de kan logga in på toohello Azure Data Catalog-portalen, men när de försöker toolog på toohello registreringsverktyget för datakällor de får ett felmeddelande som hindrar dem från att logga in.

Det finns två möjliga orsaker till detta problem:

**Orsak 1: Active Directory Federation Services configuration** hello registreringsverktyget för datakällor använder formulärautentisering toovalidate användarinloggningar mot Active Directory. För lyckad inloggning måste formulär för autentisering aktiveras i hello Global autentiseringsprincip av en Active Directory-administratör.

I vissa situationer kan kan det här felet inträffa bara när hello användaren på hello företagets nätverk, eller endast när hello användaren ansluter från hello utanför företagets nätverk. hello Global autentiseringsprincip tillåter autentisering metoder toobe aktiveras separat för intranät och extranät-anslutningar. Inloggningsfel kan uppstå om formulär för autentisering inte är aktiverat för hello nätverket från vilken hello användaren ansluter.

Mer information finns i [konfigurera autentiseringsprinciper](https://technet.microsoft.com/library/dn486781.aspx).

**Orsak 2: Proxy-nätverkskonfigurationen** om hello företagsnätverket använder en proxyserver, hello registreringsverktyget kanske inte kan tooconnect tooAzure Active Directory via hello proxy. Användare kan se till att hello registreringsverktyget genom att redigera hello verktyget konfigurationsfil, lägger till toohello filen:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


toolocate hello RegistrationTool.exe.config filen starta hello registreringsverktyget och sedan öppna hello Aktivitetshanteraren verktyget. På fliken hello information i Aktivitetshanteraren, högerklicka på RegistrationTool.exe och välj Öppna filplats hello popup-menyn.
