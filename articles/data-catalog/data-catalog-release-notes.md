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
# <a name="azure-data-catalog-release-notes"></a><span data-ttu-id="a4fb7-103">Azure Data Catalog viktig information</span><span class="sxs-lookup"><span data-stu-id="a4fb7-103">Azure Data Catalog release notes</span></span>
## <a name="notes-for-hello-november-20-2015-release-of-azure-data-catalog"></a><span data-ttu-id="a4fb7-104">Information om hello 20 November 2015-versionen av Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="a4fb7-104">Notes for hello November 20, 2015 release of Azure Data Catalog</span></span>
### <a name="opening-data-sources-in-power-bi-desktop"></a><span data-ttu-id="a4fb7-105">Öppna datakällor i Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="a4fb7-105">Opening Data Sources in Power BI Desktop</span></span>
<span data-ttu-id="a4fb7-106">När du använder hello ”öppna i Power BI Desktop” alternativet från hello **Azure Data Catalog** Företagsportalen kan användare kan stöta på något av två problem i hello Power BI Desktop-program:</span><span class="sxs-lookup"><span data-stu-id="a4fb7-106">When using hello "Open in Power BI Desktop" option from hello **Azure Data Catalog** portal, users may encounter one of two problems in hello Power BI Desktop application:</span></span>

* <span data-ttu-id="a4fb7-107">En dialogruta med hello rubriken ”tooOpen dokument” visas</span><span class="sxs-lookup"><span data-stu-id="a4fb7-107">A dialog box with hello title "Unable tooOpen Document" is displayed</span></span>
* <span data-ttu-id="a4fb7-108">hello Power BI Desktop-programmet öppnas men hello filen visas toobe tom</span><span class="sxs-lookup"><span data-stu-id="a4fb7-108">hello Power BI Desktop application opens, but hello file appears toobe empty</span></span>

<span data-ttu-id="a4fb7-109">För varje situation hello problemet kan lösas genom att hämta och installera hello senaste versionen av Power BI Desktop från [PowerBI.com](https://powerbi.com).</span><span class="sxs-lookup"><span data-stu-id="a4fb7-109">For each situation, hello problem can be resolved by downloading and installing hello latest version of Power BI Desktop from [PowerBI.com](https://powerbi.com).</span></span>

## <a name="notes-for-hello-november-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="a4fb7-110">Information om hello 13 November 2015-versionen av Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="a4fb7-110">Notes for hello November 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-tooteradata"></a><span data-ttu-id="a4fb7-111">Registrera och ansluta tooTeradata</span><span class="sxs-lookup"><span data-stu-id="a4fb7-111">Registering and connecting tooTeradata</span></span>
<span data-ttu-id="a4fb7-112">När du ansluter tooTeradata datakällor användare måste ha installerat hello rätt Teradata ODBC-drivrutin som matchar hello bitness (32-bitars eller 64-bitars) hello programvara som används.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-112">When connecting tooTeradata data sources users must have installed hello correct Teradata ODBC driver that match hello bitness (32-bit or 64-bit) of hello software being used.</span></span>

<span data-ttu-id="a4fb7-113">Från och med den här ADC utgivningsdatum, hello senaste [Teradata ODBC-drivrutin för windows (version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) är kompatibel med Office 2013, men inte med Office 2016.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-113">As of this ADC release date, hello most recent [Teradata ODBC driver for windows ( version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatible with Office 2013, but not with Office 2016.</span></span>

## <a name="notes-for-hello-july-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="a4fb7-114">Information om hello 13 juli 2015-versionen av Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="a4fb7-114">Notes for hello July 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-toooracle-database"></a><span data-ttu-id="a4fb7-115">Registrera och ansluta tooOracle databas</span><span class="sxs-lookup"><span data-stu-id="a4fb7-115">Registering and connecting tooOracle Database</span></span>
<span data-ttu-id="a4fb7-116">När ansluter tooOracle databasanvändare data sources måste ha installerat hello rätt Oracle drivrutiner som matchar hello bitness (32-bitars eller 64-bitars) hello programvara som används.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-116">When connecting tooOracle Database data sources users must have installed hello correct Oracle drivers that match hello bitness (32-bit or 64-bit) of hello software being used.</span></span>

* <span data-ttu-id="a4fb7-117">När du registrerar Oracle-datakällor på en dator med 32-bitars Windows kommer hello 32-bitars Oracle drivrutiner att användas</span><span class="sxs-lookup"><span data-stu-id="a4fb7-117">When registering Oracle data sources on a computer running 32-bit Windows, hello 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="a4fb7-118">När du registrerar Oracle-datakällor på en dator som kör 64-bitars Windows kommer hello 64-bitars Oracle drivrutiner att användas</span><span class="sxs-lookup"><span data-stu-id="a4fb7-118">When registering Oracle data sources on a computer running 64-bit Windows, hello 64-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="a4fb7-119">När du ansluter tooOracle datakällor på en dator som kör hello 32-bitars version av Microsoft Office Excel, kommer inklusive på 64-bitars Windows hello 32-bitars Oracle drivrutiner att användas</span><span class="sxs-lookup"><span data-stu-id="a4fb7-119">When connecting tooOracle data sources using Excel on a computer running hello 32-bit version of Microsoft Office, including on 64-bit Windows, hello 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="a4fb7-120">När du ansluter tooOracle datakällor på en dator som kör hello 64-bitarsversionen av Microsoft Office Excel kommer hello 64-bitars Oracle drivrutiner att användas</span><span class="sxs-lookup"><span data-stu-id="a4fb7-120">When connecting tooOracle data sources using Excel on a computer running hello 64-bit version of Microsoft Office, hello 64-bit Oracle drivers will be used</span></span>

### <a name="registering-and-connecting-toosql-server-reporting-services"></a><span data-ttu-id="a4fb7-121">Registrera och ansluta tooSQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="a4fb7-121">Registering and connecting tooSQL Server Reporting Services</span></span>
<span data-ttu-id="a4fb7-122">Stöd för SQL Server Reporting Services (SSRS) datakällor är för närvarande begränsad tooNative läge servrar.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-122">Support for SQL Server Reporting Services (SSRS) data sources is currently limited tooNative Mode servers only.</span></span> <span data-ttu-id="a4fb7-123">Stöd för SSRS i SharePoint-läge ska läggas till i en senare version.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-123">Support for SSRS in SharePoint mode will be added in a later release.</span></span>

### <a name="opening-data-assets-in-excel"></a><span data-ttu-id="a4fb7-124">Öppna datatillgångar i Excel</span><span class="sxs-lookup"><span data-stu-id="a4fb7-124">Opening data assets in Excel</span></span>
<span data-ttu-id="a4fb7-125">När du öppnar datatillgångar i Microsoft Excel från hello **Azure Data Catalog** Företagsportalen kan användare uppmanas med en **säkerhetsmeddelande om Microsoft Excel** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-125">When opening data assets in Microsoft Excel from hello **Azure Data Catalog** portal, users may be prompted with a **Microsoft Excel Security Notice** dialog box.</span></span> <span data-ttu-id="a4fb7-126">Detta är standard, förväntat beteende och användare kan välja **aktivera** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-126">This is standard, expected behavior, and users can select **Enable** toocontinue.</span></span>

<span data-ttu-id="a4fb7-127">Mer information finns i [aktivera eller inaktivera säkerhetsvarningar om länkar och filer från misstänkta webbplatser](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span><span class="sxs-lookup"><span data-stu-id="a4fb7-127">For more information, see [Enable or disable security alerts about links and files from suspicious websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span></span>

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a><span data-ttu-id="a4fb7-128">Proxy- och konfiguration och data för registrering av datakälla</span><span class="sxs-lookup"><span data-stu-id="a4fb7-128">Proxy and policy configuration and data source registration</span></span>
<span data-ttu-id="a4fb7-129">Användare kan stöta på en situation där de kan logga in på toohello Azure Data Catalog-portalen, men när de försöker toolog på toohello registreringsverktyget för datakällor de får ett felmeddelande som hindrar dem från att logga in.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-129">Users may encounter a situation where they can log on toohello Azure Data Catalog portal, but when they attempt toolog on toohello data source registration tool they encounter an error message that prevents them from logging on.</span></span>

<span data-ttu-id="a4fb7-130">Det finns två möjliga orsaker till detta problem:</span><span class="sxs-lookup"><span data-stu-id="a4fb7-130">There are two potential causes for this problem behavior:</span></span>

<span data-ttu-id="a4fb7-131">**Orsak 1: Active Directory Federation Services configuration** hello registreringsverktyget för datakällor använder formulärautentisering toovalidate användarinloggningar mot Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-131">**Cause 1: Active Directory Federation Services configuration** hello data source registration tool uses Forms Authentication toovalidate user logons against Active Directory.</span></span> <span data-ttu-id="a4fb7-132">För lyckad inloggning måste formulär för autentisering aktiveras i hello Global autentiseringsprincip av en Active Directory-administratör.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-132">For successful logon, Forms Authentication must be enabled in hello Global Authentication Policy by an Active Directory administrator.</span></span>

<span data-ttu-id="a4fb7-133">I vissa situationer kan kan det här felet inträffa bara när hello användaren på hello företagets nätverk, eller endast när hello användaren ansluter från hello utanför företagets nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-133">In some situations, this error behavior may occur only when hello user is on hello company network, or only when hello user is connecting from outside hello company network.</span></span> <span data-ttu-id="a4fb7-134">hello Global autentiseringsprincip tillåter autentisering metoder toobe aktiveras separat för intranät och extranät-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-134">hello Global Authentication Policy allows authentication methods toobe enabled separately for intranet and extranet connections.</span></span> <span data-ttu-id="a4fb7-135">Inloggningsfel kan uppstå om formulär för autentisering inte är aktiverat för hello nätverket från vilken hello användaren ansluter.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-135">Logon errors may occur if Forms Authentication is not enabled for hello network from which hello user is connecting.</span></span>

<span data-ttu-id="a4fb7-136">Mer information finns i [konfigurera autentiseringsprinciper](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="a4fb7-136">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>

<span data-ttu-id="a4fb7-137">**Orsak 2: Proxy-nätverkskonfigurationen** om hello företagsnätverket använder en proxyserver, hello registreringsverktyget kanske inte kan tooconnect tooAzure Active Directory via hello proxy.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-137">**Cause 2: Network proxy configuration** If hello corporate network uses a proxy server, hello registration tool may not be able tooconnect tooAzure Active Directory through hello proxy.</span></span> <span data-ttu-id="a4fb7-138">Användare kan se till att hello registreringsverktyget genom att redigera hello verktyget konfigurationsfil, lägger till toohello filen:</span><span class="sxs-lookup"><span data-stu-id="a4fb7-138">Users can ensure that hello registration tool by editing hello tool’s configuration file, adding this section toohello file:</span></span>

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


<span data-ttu-id="a4fb7-139">toolocate hello RegistrationTool.exe.config filen starta hello registreringsverktyget och sedan öppna hello Aktivitetshanteraren verktyget.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-139">toolocate hello RegistrationTool.exe.config file, launch hello registration tool, and then open hello Windows Task Manager utility.</span></span> <span data-ttu-id="a4fb7-140">På fliken hello information i Aktivitetshanteraren, högerklicka på RegistrationTool.exe och välj Öppna filplats hello popup-menyn.</span><span class="sxs-lookup"><span data-stu-id="a4fb7-140">On hello Details tab in Task manager, right-click on RegistrationTool.exe and choose Open file location from hello pop-up menu.</span></span>
