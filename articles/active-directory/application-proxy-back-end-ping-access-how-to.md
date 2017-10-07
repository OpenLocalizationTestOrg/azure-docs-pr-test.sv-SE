---
title: aaaHow tooconfigure en programproxy programmet toouse PingAccess | Microsoft Docs
description: "Lär dig hur toouse PingAccess tooextend hello fördelarna med Application Proxy tooapplications med huvud-baserad autentisering"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: fa4c9747b7bf5a135425be270e4f7eadf95248fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application-toouse-pingaccess"></a><span data-ttu-id="11e3d-103">Hur tooconfigure en programproxy programmet toouse PingAccess</span><span class="sxs-lookup"><span data-stu-id="11e3d-103">How tooconfigure an Application Proxy application toouse PingAccess</span></span>

<span data-ttu-id="11e3d-104">Vår samarbete med PingAccess nu kan du tooextend hello fördelarna med Application Proxy tooapplications med huvud-baserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="11e3d-104">Our collaboration with PingAccess now allows you tooextend hello benefits of Application Proxy tooapplications using header-based authentication.</span></span> <span data-ttu-id="11e3d-105">Om dina program inte använder huvuden, finns våra [enkel inloggning dokumentationen](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) mer information om andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="11e3d-105">If your applications do not use headers, see our [Single Sign-On documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) for details on other options.</span></span>

## <a name="overview-of-steps-and-recommended-documents"></a><span data-ttu-id="11e3d-106">Översikt över stegen och rekommenderade dokument</span><span class="sxs-lookup"><span data-stu-id="11e3d-106">Overview of steps and recommended documents</span></span>

<span data-ttu-id="11e3d-107">tooconfigure ett program med PingAccess, det finns fyra steg:</span><span class="sxs-lookup"><span data-stu-id="11e3d-107">tooconfigure an application with PingAccess, there are four steps:</span></span>

1.  <span data-ttu-id="11e3d-108">Konfigurera Application Proxy-kopplingar</span><span class="sxs-lookup"><span data-stu-id="11e3d-108">Configure Application Proxy Connectors</span></span>

2.  <span data-ttu-id="11e3d-109">Skapa ett program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="11e3d-109">Create an Azure AD Application Proxy Application</span></span>

3.  <span data-ttu-id="11e3d-110">& Hämta konfigurera PingAccess</span><span class="sxs-lookup"><span data-stu-id="11e3d-110">Download & Configure PingAccess</span></span>

4.  <span data-ttu-id="11e3d-111">Konfigurera program för PingAccess</span><span class="sxs-lookup"><span data-stu-id="11e3d-111">Configure Applications in PingAccess</span></span>

<span data-ttu-id="11e3d-112">Mer information om var och en av de här stegen finns våra [enkel inloggning med rubriker dokumentation](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span><span class="sxs-lookup"><span data-stu-id="11e3d-112">For details on each of these steps, see our [Single Sign-On with Headers documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).</span></span>
