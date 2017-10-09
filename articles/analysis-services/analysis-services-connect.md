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
# <a name="connect-tooan-azure-analysis-services-server"></a><span data-ttu-id="3cc54-103">Ansluta tooan Azure Analysis Services-server</span><span class="sxs-lookup"><span data-stu-id="3cc54-103">Connect tooan Azure Analysis Services server</span></span>

<span data-ttu-id="3cc54-104">Den här artikeln beskriver anslutande tooa server med hjälp av datamodellering och av hanteringsprogram som SQL Server Management Studio (SSMS) eller SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="3cc54-104">This article describes connecting tooa server by using data modeling and management applications like SQL Server Management Studio (SSMS) or SQL Server Data Tools (SSDT).</span></span> <span data-ttu-id="3cc54-105">Eller med klienten reporting program som Microsoft Excel, Power BI Desktop eller anpassade program.</span><span class="sxs-lookup"><span data-stu-id="3cc54-105">Or, with client reporting applications like Microsoft Excel, Power BI Desktop, or custom applications.</span></span> <span data-ttu-id="3cc54-106">Anslutningar tooAzure Analysis Services använder HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3cc54-106">Connections tooAzure Analysis Services use HTTPS.</span></span>

## <a name="client-libraries"></a><span data-ttu-id="3cc54-107">Klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="3cc54-107">Client libraries</span></span>
[<span data-ttu-id="3cc54-108">Hämta senaste hello-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="3cc54-108">Get hello latest Client libraries</span></span>](analysis-services-data-providers.md)

<span data-ttu-id="3cc54-109">Alla anslutningar tooa server, oavsett typ, kräver uppdaterade AMO och ADOMD.NET OLEDB libraries tooconnect tooand klientgränssnitt med en Analysis Services-server.</span><span class="sxs-lookup"><span data-stu-id="3cc54-109">All connections tooa server, regardless of type, require updated AMO, ADOMD.NET, and OLEDB client libraries tooconnect tooand interface with an Analysis Services server.</span></span> <span data-ttu-id="3cc54-110">För SSMS, SSDT, Excel 2016 och Power BI hello senaste klientbibliotek installeras eller uppdateras med månatliga versioner.</span><span class="sxs-lookup"><span data-stu-id="3cc54-110">For SSMS, SSDT, Excel 2016, and Power BI, hello latest client libraries are installed or updated with monthly releases.</span></span> <span data-ttu-id="3cc54-111">I vissa fall kan är det dock ett program kan inte ha hello senaste.</span><span class="sxs-lookup"><span data-stu-id="3cc54-111">However, in some cases, it's possible an application may not have hello latest.</span></span> <span data-ttu-id="3cc54-112">När principer fördröjning uppdateringar eller uppdateringar för Office 365 finns till exempel på hello uppskjuten kanal.</span><span class="sxs-lookup"><span data-stu-id="3cc54-112">For example, when policies delay updates, or Office 365 updates are on hello Deferred Channel.</span></span>

## <a name="server-name"></a><span data-ttu-id="3cc54-113">servernamn</span><span class="sxs-lookup"><span data-stu-id="3cc54-113">Server name</span></span>

<span data-ttu-id="3cc54-114">När du skapar en Analysis Services-server i Azure kan ange du ett unikt namn och hello område där hello server är toobe som skapats.</span><span class="sxs-lookup"><span data-stu-id="3cc54-114">When you create an Analysis Services server in Azure, you specify a unique name and hello region where hello server is toobe created.</span></span> <span data-ttu-id="3cc54-115">När du anger hello servernamnet i en anslutning är hello namngivningsschemat för servern:</span><span class="sxs-lookup"><span data-stu-id="3cc54-115">When specifying hello server name in a connection, hello server naming scheme is:</span></span>

```
<protocol>://<region>/<servername>
```
 <span data-ttu-id="3cc54-116">Där protokollet är sträng **asazure**, region är hello Uri där hello servern skapades (till exempel westus.asazure.windows.net) och servernamn är serverns unika inom hello region hello namn.</span><span class="sxs-lookup"><span data-stu-id="3cc54-116">Where protocol is string **asazure**, region is hello Uri where hello server was created (for example, westus.asazure.windows.net) and servername is hello name of your unique server within hello region.</span></span>

### <a name="get-hello-server-name"></a><span data-ttu-id="3cc54-117">Hämta hello-servernamn</span><span class="sxs-lookup"><span data-stu-id="3cc54-117">Get hello server name</span></span>
<span data-ttu-id="3cc54-118">I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hello hela servernamn.</span><span class="sxs-lookup"><span data-stu-id="3cc54-118">In **Azure portal** > server > **Overview** > **Server name**, copy hello entire server name.</span></span> <span data-ttu-id="3cc54-119">Om andra användare i din organisation ansluter toothis server för, kan du dela det här Servernamnet med dem.</span><span class="sxs-lookup"><span data-stu-id="3cc54-119">If other users in your organization are connecting toothis server too, you can share this server name with them.</span></span> <span data-ttu-id="3cc54-120">När du anger ett servernamn måste hello hela sökvägen användas.</span><span class="sxs-lookup"><span data-stu-id="3cc54-120">When specifying a server name, hello entire path must be used.</span></span>

![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a><span data-ttu-id="3cc54-122">Anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="3cc54-122">Connection string</span></span>

<span data-ttu-id="3cc54-123">När du ansluter tooAzure hello Analysis Services med hjälp av objektet Tabellmodell, Använd hello efter anslutning strängformat:</span><span class="sxs-lookup"><span data-stu-id="3cc54-123">When connecting tooAzure Analysis Services using hello Tabular Object Model, use hello following connection string formats:</span></span>

###### <a name="integrated-azure-active-directory-authentication"></a><span data-ttu-id="3cc54-124">Azure Active Directory-integrerad autentisering</span><span class="sxs-lookup"><span data-stu-id="3cc54-124">Integrated Azure Active Directory authentication</span></span>
<span data-ttu-id="3cc54-125">Integrerad autentisering hämtar hello Azure Active Directory cacheminne om de är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="3cc54-125">Integrated authentication picks up hello Azure Active Directory credential cache if available.</span></span> <span data-ttu-id="3cc54-126">Om inte, hello Azure inloggningen visas.</span><span class="sxs-lookup"><span data-stu-id="3cc54-126">If not, hello Azure login window is shown.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a><span data-ttu-id="3cc54-127">Azure Active Directory-autentisering med användarnamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="3cc54-127">Azure Active Directory authentication with username and password</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a><span data-ttu-id="3cc54-128">Windows-autentisering (integrerad säkerhet)</span><span class="sxs-lookup"><span data-stu-id="3cc54-128">Windows authentication (Integrated security)</span></span>
<span data-ttu-id="3cc54-129">Använd hello Windows-konto som kör hello aktuella processen.</span><span class="sxs-lookup"><span data-stu-id="3cc54-129">Use hello Windows account running hello current process.</span></span>

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a><span data-ttu-id="3cc54-130">Ansluta med en ODC-fil</span><span class="sxs-lookup"><span data-stu-id="3cc54-130">Connect using an .odc file</span></span>
<span data-ttu-id="3cc54-131">Användare kan ansluta tooan Azure Analysis Services-servern med hjälp av en fil Office Data Connection (.odc) med äldre versioner av Excel.</span><span class="sxs-lookup"><span data-stu-id="3cc54-131">With older versions of Excel, users can connect tooan Azure Analysis Services server by using an Office Data Connection (.odc) file.</span></span> <span data-ttu-id="3cc54-132">Det finns fler toolearn [skapa en fil Office Data Connection (.odc)](analysis-services-odc.md).</span><span class="sxs-lookup"><span data-stu-id="3cc54-132">toolearn more, see [Create an Office Data Connection (.odc) file](analysis-services-odc.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="3cc54-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3cc54-133">Next steps</span></span>
<span data-ttu-id="3cc54-134">[Ansluta till Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="3cc54-134">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
<span data-ttu-id="3cc54-135">[Ansluta med Powerbi](analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="3cc54-135">[Connect with Power BI](analysis-services-connect-pbi.md) </span></span>  
[<span data-ttu-id="3cc54-136">Hantera servern</span><span class="sxs-lookup"><span data-stu-id="3cc54-136">Manage your server</span></span>](analysis-services-manage.md)   

