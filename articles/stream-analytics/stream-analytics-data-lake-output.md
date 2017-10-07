---
title: aaaStream Analytics Data Lake Store utdata | Microsoft Docs
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
ms.openlocfilehash: 183cf51edb2e49ac3e42257e67a8077b95777258
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-data-lake-store-output"></a><span data-ttu-id="50265-103">Stream Analytics Data Lake Store-utdata</span><span class="sxs-lookup"><span data-stu-id="50265-103">Stream Analytics Data Lake Store output</span></span>
<span data-ttu-id="50265-104">Stream Analytics-jobb stöder flera metoder för utdata, en som en [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span><span class="sxs-lookup"><span data-stu-id="50265-104">Stream Analytics jobs support several output methods, one being an [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).</span></span> <span data-ttu-id="50265-105">Azure Data Lake Store är en företagsomfattande storskalig lagringsplats för analytiska arbetsbelastningar för stordata.</span><span class="sxs-lookup"><span data-stu-id="50265-105">Azure Data Lake Store is an enterprise-wide hyper-scale repository for big data analytic workloads.</span></span> <span data-ttu-id="50265-106">Data Lake Store kan du toostore data för alla storlek, typ och införandet hastighet för drifts- och undersökande analyser.</span><span class="sxs-lookup"><span data-stu-id="50265-106">Data Lake Store enables you toostore data of any size, type and ingestion speed for operational and exploratory analytics.</span></span>

## <a name="authorize-a-data-lake-store-account"></a><span data-ttu-id="50265-107">Godkänna ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="50265-107">Authorize a Data Lake Store account</span></span>
1. <span data-ttu-id="50265-108">När Data Lake Store är markerad som utdata i hello Azure-portalen, uppmanas du att tooauthorize användning av dina befintliga Data Lake Store eller toorequest åtkomst till toohello Data Lake Store via hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="50265-108">When Data Lake Store is selected as an output in hello Azure portal, you will be prompted tooauthorize use of your existing Data Lake Store or toorequest access toohello Data Lake Store via hello Classic Portal.</span></span>
   
   ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  
   
2. <span data-ttu-id="50265-109">Om du redan har åtkomst till tooData Lake Store och klicka på ”Verifiera nu” under en kort tid en sida visas som anger ”omdirigera tooauthorization”.</span><span class="sxs-lookup"><span data-stu-id="50265-109">If you already have access tooData Lake Store, click “Authorize Now” and for a brief time a page will pop up indicating “Redirecting tooauthorization”.</span></span> <span data-ttu-id="50265-110">hello sidan stängs automatiskt och visas med hello som skulle låta tooconfigure hello Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-110">hello page will automatically close and you will be presented with hello page that would allow you tooconfigure hello Data Lake Store output.</span></span>

<span data-ttu-id="50265-111">Om du inte har registrerat dig för Data Lake Store kan du följa hello ”logga nu” link tooinitiate hello begäran eller följa hello [igång instruktioner](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="50265-111">If you have not signed up for Data Lake Store, you can follow hello “Sign up now” link tooinitiate hello request, or follow hello [get started instructions](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

## <a name="configure-hello-data-lake-store-output-properties"></a><span data-ttu-id="50265-112">Konfigurera egenskaper för hello Data Lake Store-utdata</span><span class="sxs-lookup"><span data-stu-id="50265-112">Configure hello Data Lake Store output properties</span></span>
<span data-ttu-id="50265-113">När du har autentiserad hello Data Lake Store-konto kan konfigurera du hello egenskaper för Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-113">Once you have hello Data Lake Store account authenticated, you can configure hello properties for your Data Lake Store output.</span></span> <span data-ttu-id="50265-114">hello tabellen nedan är hello lista med egenskapsnamn och deras beskrivning tooconfigure din Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-114">hello table below is hello list of property names and their description tooconfigure your Data Lake Store output.</span></span>

<table>
<tbody>
<tr>
<td><span data-ttu-id="50265-115"><B>EGENSKAPSNAMN</B></span><span class="sxs-lookup"><span data-stu-id="50265-115"><B>PROPERTY NAME</B></span></span></td>
<td><span data-ttu-id="50265-116"><B>BESKRIVNING</B></span><span class="sxs-lookup"><span data-stu-id="50265-116"><B>DESCRIPTION</B></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-117">Kolumnalias</span><span class="sxs-lookup"><span data-stu-id="50265-117">Output Alias</span></span></td>
<td><span data-ttu-id="50265-118">Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="50265-118">This is a friendly name used in queries toodirect hello query output toothis Data Lake Store.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-119">Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="50265-119">Data Lake Store Account</span></span></td>
<td><span data-ttu-id="50265-120">hello namnet på hello storage-konto där du skickar din utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-120">hello name of hello storage account where you are sending your output.</span></span> <span data-ttu-id="50265-121">Visas med en lista över datasjölagerkonton hello inloggad användare har åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="50265-121">You will be presented with a list of Data Lake Store accounts  hello logged in user has access to.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-122">Prefixet sökvägar [<I>valfria</I>]</span><span class="sxs-lookup"><span data-stu-id="50265-122">Path Prefix Pattern [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="50265-123">Hej filen sökvägen toowrite filerna i hello angivna Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="50265-123">hello file path used toowrite your files within hello specified Data Lake Store Account.</span></span> <BR><span data-ttu-id="50265-124">{date} {time}</span><span class="sxs-lookup"><span data-stu-id="50265-124">{date}, {time}</span></span><BR><span data-ttu-id="50265-125">Exempel 1: mapp1/logs / {date} / {time}</span><span class="sxs-lookup"><span data-stu-id="50265-125">Example 1: folder1/logs/{date}/{time}</span></span><BR><span data-ttu-id="50265-126">Exempel 2: mapp1/logs / {date}</span><span class="sxs-lookup"><span data-stu-id="50265-126">Example 2: folder1/logs/{date}</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-127">Datumformat [<I>valfria</I>]</span><span class="sxs-lookup"><span data-stu-id="50265-127">Date Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="50265-128">Du kan välja hello datumformat där filerna ordnas om hello datumtoken används i hello prefix sökväg.</span><span class="sxs-lookup"><span data-stu-id="50265-128">If hello date token is used in hello prefix path, you can select hello date format in which your files are organized.</span></span> <span data-ttu-id="50265-129">Exempel: ÅÅÅÅ/MM/DD</span><span class="sxs-lookup"><span data-stu-id="50265-129">Example: YYYY/MM/DD</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-130">Tidsformat [<I>valfria</I>]</span><span class="sxs-lookup"><span data-stu-id="50265-130">Time Format [<I>optional</I>]</span></span></td>
<td><span data-ttu-id="50265-131">Ange hello tidsformat där filerna ordnas om hello tid token används i hello prefix sökväg.</span><span class="sxs-lookup"><span data-stu-id="50265-131">If hello time token is used in hello prefix path, specify hello time format in which your files are organized.</span></span> <span data-ttu-id="50265-132">Hello stöds endast värdet är för närvarande HH.</span><span class="sxs-lookup"><span data-stu-id="50265-132">Currently hello only supported value is HH.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-133">Händelsen serialiseringsformat</span><span class="sxs-lookup"><span data-stu-id="50265-133">Event Serialization Format</span></span></td>
<td><span data-ttu-id="50265-134">Serialiseringsformat för utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-134">Serialization format for output data.</span></span> <span data-ttu-id="50265-135">JSON-, CSV- och Avro stöds.</span><span class="sxs-lookup"><span data-stu-id="50265-135">JSON, CSV, and Avro are supported.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-136">Encoding</span><span class="sxs-lookup"><span data-stu-id="50265-136">Encoding</span></span></td>
<td><span data-ttu-id="50265-137">Om CSV- eller JSON-format, måste kodning anges.</span><span class="sxs-lookup"><span data-stu-id="50265-137">If CSV or JSON format, an encoding must be specified.</span></span> <span data-ttu-id="50265-138">UTF-8 är hello stöds endast kodningsformat just nu.</span><span class="sxs-lookup"><span data-stu-id="50265-138">UTF-8 is hello only supported encoding format at this time.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-139">Avgränsare</span><span class="sxs-lookup"><span data-stu-id="50265-139">Delimiter</span></span></td>
<td><span data-ttu-id="50265-140">Gäller endast för CSV-serialisering.</span><span class="sxs-lookup"><span data-stu-id="50265-140">Only applicable for CSV serialization.</span></span> <span data-ttu-id="50265-141">Stream Analytics stöder ett antal olika avgränsare för serialisering CSV.</span><span class="sxs-lookup"><span data-stu-id="50265-141">Stream Analytics supports a number of common delimiters for serializing CSV data.</span></span> <span data-ttu-id="50265-142">Värden som stöds är kommatecken, semikolon, blanksteg, TABB och vertikalstreck.</span><span class="sxs-lookup"><span data-stu-id="50265-142">Supported values are comma, semicolon, space, tab and vertical bar.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="50265-143">Format</span><span class="sxs-lookup"><span data-stu-id="50265-143">Format</span></span></td>
<td><span data-ttu-id="50265-144">Gäller endast för JSON-serialisering.</span><span class="sxs-lookup"><span data-stu-id="50265-144">Only applicable for JSON serialization.</span></span> <span data-ttu-id="50265-145">Radseparering innebär att hello utdata formateras genom att varje JSON-objekt avgränsas med en ny rad.</span><span class="sxs-lookup"><span data-stu-id="50265-145">Line separated specifies that hello output will be formatted by having each JSON object separated by a new line.</span></span> <span data-ttu-id="50265-146">Matrisen anger hello utdata formateras som en matris av JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="50265-146">Array specifies that hello output will be formatted as an array of JSON objects.</span></span></td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a><span data-ttu-id="50265-147">Förnya auktorisering för Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="50265-147">Renew Data Lake Store Authorization</span></span>
<span data-ttu-id="50265-148">För närvarande finns en begränsning där hello autentiseringstoken måste toobe manuellt uppdateras efter 90 dagar för alla jobb med Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-148">Currently, there is a limitation where hello authentication token needs toobe manually refreshed every 90 days for all jobs with Data Lake Store output.</span></span> <span data-ttu-id="50265-149">Du måste också toore-autentisera ditt Data Lake Store-konto om du har ändrat ditt lösenord eftersom utskriftsjobbet skapades eller senast autentiserad.</span><span class="sxs-lookup"><span data-stu-id="50265-149">You will also need toore-authenticate your Data Lake Store account if you have changed your password since your job was created or last authenticated.</span></span> <span data-ttu-id="50265-150">Ett symtom på det här problemet är inga jobbutdata och ett fel i hello Åtgärdsloggar som anger behovet av återauktorisering.</span><span class="sxs-lookup"><span data-stu-id="50265-150">A symptom of this issue is no job output and an error in hello Operation Logs indicating need for re-authorization.</span></span>

<span data-ttu-id="50265-151">tooresolve det här problemet stoppa körs jobbet och gå tooyour Data Lake Store-utdata.</span><span class="sxs-lookup"><span data-stu-id="50265-151">tooresolve this issue, stop your running job and go tooyour Data Lake Store output.</span></span> <span data-ttu-id="50265-152">Klicka på hello ”förnya auktorisering” länk och under en kort tid en sida visas som anger ”omdirigera tooauthorization..”.</span><span class="sxs-lookup"><span data-stu-id="50265-152">Click hello “Renew authorization” link, and for a brief time a page will pop up indicating “Redirecting tooauthorization..”.</span></span> <span data-ttu-id="50265-153">hello sidan stängs automatiskt och om detta lyckas visar ”tillstånd har förnyats”.</span><span class="sxs-lookup"><span data-stu-id="50265-153">hello page will automatically close and if successful, will indicate “Authorization has been successfully renewed”.</span></span> <span data-ttu-id="50265-154">Sedan måste tooclick ”spara” längst ned hello hello sida och kan fortsätta genom att starta om jobbet från hello stoppats senast tooavoid data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="50265-154">You then need tooclick “Save” at hello bottom of hello page, and can proceed by restarting your job from hello Last Stopped Time tooavoid data loss.</span></span>

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)

