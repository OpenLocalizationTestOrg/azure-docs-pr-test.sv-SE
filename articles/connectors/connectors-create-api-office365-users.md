---
title: "Lägg till användare av Office 365-kopplingen i Logic Apps | Microsoft Docs"
description: "Översikt över användare av Office 365-kopplingen med REST API-parametrar"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 330f733440932a769eb0fe6031cd0d947a820080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-users-connector"></a><span data-ttu-id="cdb35-103">Kom igång med användare av Office 365-kopplingen</span><span class="sxs-lookup"><span data-stu-id="cdb35-103">Get started with the Office 365 Users connector</span></span>
<span data-ttu-id="cdb35-104">Ansluta till Office 365-användare få profiler och Sök efter användare.</span><span class="sxs-lookup"><span data-stu-id="cdb35-104">Connect to Office 365 Users to get profiles, search users, and more.</span></span> <span data-ttu-id="cdb35-105">Med Office 365-användare kan du:</span><span class="sxs-lookup"><span data-stu-id="cdb35-105">With Office 365 Users, you can:</span></span>

* <span data-ttu-id="cdb35-106">Skapa ditt företag flödet som baseras på de data som du får från Office 365-användare.</span><span class="sxs-lookup"><span data-stu-id="cdb35-106">Build your business flow based on the data you get from Office 365 Users.</span></span> 
* <span data-ttu-id="cdb35-107">Använd åtgärder som hämta direktrapporter, hämta en chef användarprofil och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="cdb35-107">Use actions that get direct reports, get a manager's user profile, and more.</span></span> <span data-ttu-id="cdb35-108">De här åtgärderna få svar och utdata gör tillgängligt för andra åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cdb35-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="cdb35-109">Till exempel få en persons direktrapporter ta den här informationen och uppdatera SQL Azure-databasen.</span><span class="sxs-lookup"><span data-stu-id="cdb35-109">For example, get a person's direct reports, and then take this information and update a SQL Azure database.</span></span> 

<span data-ttu-id="cdb35-110">Du kan komma igång genom att skapa en logikapp nu, se [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="cdb35-110">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-office-365-users"></a><span data-ttu-id="cdb35-111">Skapa en anslutning till Office 365-användare</span><span class="sxs-lookup"><span data-stu-id="cdb35-111">Create a connection to Office 365 Users</span></span>
<span data-ttu-id="cdb35-112">När du lägger till den här anslutningen dina logic apps kan måste du logga in på ditt konto för Office 365-användare och Tillåt logikappar att ansluta till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="cdb35-112">When you add this connector to your logic apps, you must sign-in to your Office 365 Users account and allow logic apps to connect to your account.</span></span>

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

<span data-ttu-id="cdb35-113">När du skapar anslutningen kan ange du egenskaper för Office 365-användare, t.ex. användar-ID.</span><span class="sxs-lookup"><span data-stu-id="cdb35-113">After you create the connection, you enter the Office 365 Users properties, like the user ID.</span></span> <span data-ttu-id="cdb35-114">Den **REST API-referensen** i det här avsnittet beskriver dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="cdb35-114">The **REST API reference** in this topic describes these properties.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="cdb35-115">Connector-specifik information</span><span class="sxs-lookup"><span data-stu-id="cdb35-115">Connector-specific details</span></span>

<span data-ttu-id="cdb35-116">Visa alla utlösare och åtgärder som definierats i swagger och även se några gränser i den [connector information](/connectors/officeusers/).</span><span class="sxs-lookup"><span data-stu-id="cdb35-116">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/officeusers/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="cdb35-117">Flera kopplingar</span><span class="sxs-lookup"><span data-stu-id="cdb35-117">More connectors</span></span>
<span data-ttu-id="cdb35-118">Gå tillbaka till den [API: er listan](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="cdb35-118">Go back to the [APIs list](apis-list.md).</span></span>