---
title: "Utdata för Stream Analytics Data Lake Store | Microsoft Docs"
description: Konfigurationen av autentisering och auktorisering av ett Azure Data Lake Store i ett Stream Analytics-jobb
keywords: 
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ea5baafa-0054-4c70-973a-6a3a8c6eaffc
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 3d867df3ef875d5cc41de418c3d1d269ff751fda
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="stream-analytics-data-lake-store-output"></a><span data-ttu-id="7fa64-103">Stream Analytics Data Lake Store-utdata</span><span class="sxs-lookup"><span data-stu-id="7fa64-103">Stream Analytics Data Lake Store output</span></span>
<span data-ttu-id="7fa64-104">Stream Analytics-jobb stöder flera metoder för utdata, en som en [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="7fa64-104">Stream Analytics jobs support several output methods, one being an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span> <span data-ttu-id="7fa64-105">Azure Data Lake Store är en företagsomfattande storskalig lagringsplats för analytiska arbetsbelastningar för stordata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-105">Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.</span></span> <span data-ttu-id="7fa64-106">Data Lake Store kan du lagra data med en storlek, typ och införandet hastighet för drifts- och undersökande analyser.</span><span class="sxs-lookup"><span data-stu-id="7fa64-106">Data Lake Store enables you to store data of any size, type and ingestion speed for operational and exploratory analytics.</span></span>

## <a name="authorize-a-data-lake-store-account"></a><span data-ttu-id="7fa64-107">Godkänna ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="7fa64-107">Authorize a Data Lake Store account</span></span>
1. <span data-ttu-id="7fa64-108">När Data Lake Store är markerad som utdata i Azure-portalen, uppmanas du att tillåta användning av din befintliga Data Lake Store eller begära åtkomst till Data Lake Store via den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="7fa64-108">When Data Lake Store is selected as an output in the Azure portal, you will be prompted to authorize use of your existing Data Lake Store or to request access to the Data Lake Store via the Classic Portal.</span></span>
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. <span data-ttu-id="7fa64-109">Om du redan har åtkomst till Data Lake Store, klicka på ”Verifiera nu” och under en kort tid en sida visas som anger ”omdirigera tillstånd”.</span><span class="sxs-lookup"><span data-stu-id="7fa64-109">If you already have access to Data Lake Store, click “Authorize Now” and for a brief time a page will pop up indicating “Redirecting to authorization”.</span></span> <span data-ttu-id="7fa64-110">Sidan stängs automatiskt och visas med sidan som du kan konfigurera Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-110">The page will automatically close and you will be presented with the page that would allow you to configure the Data Lake Store output.</span></span>

<span data-ttu-id="7fa64-111">Om du inte har registrerat dig för Data Lake Store kan du följa länken ”logga nu” för att initiera begäran eller följa den [igång instruktioner](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7fa64-111">If you have not signed up for Data Lake Store, you can follow the “Sign up now” link to initiate the request, or follow the [get started instructions](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="configure-the-data-lake-store-output-properties"></a><span data-ttu-id="7fa64-112">Konfigurera egenskaper för Data Lake Store-utdata</span><span class="sxs-lookup"><span data-stu-id="7fa64-112">Configure the Data Lake Store output properties</span></span>
<span data-ttu-id="7fa64-113">När du har Data Lake Store-kontot autentiseras kan du konfigurera egenskaper för Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-113">Once you have the Data Lake Store account authenticated, you can configure the properties for your Data Lake Store output.</span></span> <span data-ttu-id="7fa64-114">Tabellen nedan är listan över egenskapsnamn och deras beskrivning för att konfigurera din Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-114">The table below is the list of property names and their description to configure your Data Lake Store output.</span></span>

<table>
<tbody>
<tr>
<td><span data-ttu-id="7fa64-115"><B>EGENSKAPSNAMN</B></span><span class="sxs-lookup"><span data-stu-id="7fa64-115"><B>PROPERTY NAME</B></span></span></td>
<td><span data-ttu-id="7fa64-116"><B>BESKRIVNING</B></span><span class="sxs-lookup"><span data-stu-id="7fa64-116"><B>DESCRIPTION</B></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-117">Kolumnalias</span><span class="sxs-lookup"><span data-stu-id="7fa64-117">Output Alias</span></span></td>
<td><span data-ttu-id="7fa64-118">Detta är ett eget namn som används i frågor för att dirigera utdata till denna Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="7fa64-118">This is a friendly name used in queries to direct the query output to this Data Lake Store.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-119">Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="7fa64-119">Data Lake Store Account</span></span></td>
<td><span data-ttu-id="7fa64-120">Namnet på det lagringskonto där du skickar din utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-120">The name of the storage account where you are sending your output.</span></span> <span data-ttu-id="7fa64-121">Visas en lista med Data Lake Store-konton som den inloggade användaren har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="7fa64-121">You will be presented with a list of Data Lake Store accounts  the logged in user has access to.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-122">Prefixet sökvägar [<I>valfria</I>]</span><span class="sxs-lookup"><span data-stu-id="7fa64-122">Path Prefix Pattern [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="7fa64-123">Den filsökväg som används för att skriva filer i den angivna Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7fa64-123">The file path used to write your files within the specified Data Lake Store Account.</span></span> <BR><span data-ttu-id="7fa64-124">{date} {time}</span><span class="sxs-lookup"><span data-stu-id="7fa64-124">{date}, {time}</span></span><BR><span data-ttu-id="7fa64-125">Exempel 1: mapp1/logs / {date} / {time}</span><span class="sxs-lookup"><span data-stu-id="7fa64-125">Example 1: folder1/logs/{date}/{time}</span></span><BR><span data-ttu-id="7fa64-126">Exempel 2: mapp1/logs / {date}</span><span class="sxs-lookup"><span data-stu-id="7fa64-126">Example 2: folder1/logs/{date}</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-127">Datumformat [<I>valfria</I>]</span><span class="sxs-lookup"><span data-stu-id="7fa64-127">Date Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="7fa64-128">Du kan välja datumformat där filerna ordnas om datumtoken används i sökvägen till prefix.</span><span class="sxs-lookup"><span data-stu-id="7fa64-128">If the date token is used in the prefix path, you can select the date format in which your files are organized.</span></span> <span data-ttu-id="7fa64-129">Exempel: ÅÅÅÅ/MM/DD</span><span class="sxs-lookup"><span data-stu-id="7fa64-129">Example: YYYY/MM/DD</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-130">Tidsformat [<I>valfria</I>]</span><span class="sxs-lookup"><span data-stu-id="7fa64-130">Time Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="7fa64-131">Ange tidsformat där filerna ordnas om tid token används i sökvägen till prefix.</span><span class="sxs-lookup"><span data-stu-id="7fa64-131">If the time token is used in the prefix path, specify the time format in which your files are organized.</span></span> <span data-ttu-id="7fa64-132">Det enda värdet som stöds är för närvarande HH.</span><span class="sxs-lookup"><span data-stu-id="7fa64-132">Currently the only supported value is HH.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-133">Händelsen serialiseringsformat</span><span class="sxs-lookup"><span data-stu-id="7fa64-133">Event Serialization Format</span></span></td>
<td><span data-ttu-id="7fa64-134">Serialiseringsformat för utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-134">Serialization format for output data.</span></span> <span data-ttu-id="7fa64-135">JSON-, CSV- och Avro stöds.</span><span class="sxs-lookup"><span data-stu-id="7fa64-135">JSON, CSV, and Avro are supported.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-136">Encoding</span><span class="sxs-lookup"><span data-stu-id="7fa64-136">Encoding</span></span></td>
<td><span data-ttu-id="7fa64-137">Om CSV- eller JSON-format, måste kodning anges.</span><span class="sxs-lookup"><span data-stu-id="7fa64-137">If CSV or JSON format, an encoding must be specified.</span></span> <span data-ttu-id="7fa64-138">UTF-8 är det enda kodformat som stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="7fa64-138">UTF-8 is the only supported encoding format at this time.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-139">Avgränsare</span><span class="sxs-lookup"><span data-stu-id="7fa64-139">Delimiter</span></span></td>
<td><span data-ttu-id="7fa64-140">Gäller endast för CSV-serialisering.</span><span class="sxs-lookup"><span data-stu-id="7fa64-140">Only applicable for CSV serialization.</span></span> <span data-ttu-id="7fa64-141">Stream Analytics stöder ett antal olika avgränsare för serialisering CSV.</span><span class="sxs-lookup"><span data-stu-id="7fa64-141">Stream Analytics supports a number of common delimiters for serializing CSV data.</span></span> <span data-ttu-id="7fa64-142">Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck.</span><span class="sxs-lookup"><span data-stu-id="7fa64-142">Supported values are comma, semicolon, space, tab and vertical bar.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7fa64-143">Format</span><span class="sxs-lookup"><span data-stu-id="7fa64-143">Format</span></span></td>
<td><span data-ttu-id="7fa64-144">Gäller endast för JSON-serialisering.</span><span class="sxs-lookup"><span data-stu-id="7fa64-144">Only applicable for JSON serialization.</span></span> <span data-ttu-id="7fa64-145">Radseparering innebär att utdata formateras genom att varje JSON-objekt avgränsas med en ny rad.</span><span class="sxs-lookup"><span data-stu-id="7fa64-145">Line separated specifies that the output will be formatted by having each JSON object separated by a new line.</span></span> <span data-ttu-id="7fa64-146">Matrisen anger att utdata formateras som en matris av JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="7fa64-146">Array specifies that the output will be formatted as an array of JSON objects.</span></span></td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a><span data-ttu-id="7fa64-147">Förnya auktorisering för Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7fa64-147">Renew Data Lake Store Authorization</span></span>
<span data-ttu-id="7fa64-148">För närvarande finns en begränsning där autentiseringstoken måste uppdateras manuellt efter 90 dagar för alla jobb med Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-148">Currently, there is a limitation where the authentication token needs to be manually refreshed every 90 days for all jobs with Data Lake Store output.</span></span> <span data-ttu-id="7fa64-149">Du måste också återautentisera ditt Data Lake Store-konto om du har ändrat ditt lösenord eftersom utskriftsjobbet skapades eller senast autentiserad.</span><span class="sxs-lookup"><span data-stu-id="7fa64-149">You will also need to re-authenticate your Data Lake Store account if you have changed your password since your job was created or last authenticated.</span></span> <span data-ttu-id="7fa64-150">Ett symtom på det här problemet är inga jobbutdata och ett fel i loggarna åtgärden som anger behovet av återauktorisering.</span><span class="sxs-lookup"><span data-stu-id="7fa64-150">A symptom of this issue is no job output and an error in the Operation Logs indicating need for re-authorization.</span></span>

<span data-ttu-id="7fa64-151">Lös problemet genom att stoppa körs jobbet och gå till din Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="7fa64-151">To resolve this issue, stop your running job and go to your Data Lake Store output.</span></span> <span data-ttu-id="7fa64-152">Klicka på länken ”förnya auktorisering” och under en kort tid en sida visas som anger ”omdirigera tillstånd..”.</span><span class="sxs-lookup"><span data-stu-id="7fa64-152">Click the “Renew authorization” link, and for a brief time a page will pop up indicating “Redirecting to authorization..”.</span></span> <span data-ttu-id="7fa64-153">Sidan stängs automatiskt och om detta lyckas visar ”tillstånd har förnyats”.</span><span class="sxs-lookup"><span data-stu-id="7fa64-153">The page will automatically close and if successful, will indicate “Authorization has been successfully renewed”.</span></span> <span data-ttu-id="7fa64-154">Du sedan behöver klicka på ”Spara” längst ned på sidan och kan fortsätta genom att starta om jobbet från stoppats senast att undvika dataförlust.</span><span class="sxs-lookup"><span data-stu-id="7fa64-154">You then need to click “Save” at the bottom of the page, and can proceed by restarting your job from the Last Stopped Time to avoid data loss.</span></span>

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

