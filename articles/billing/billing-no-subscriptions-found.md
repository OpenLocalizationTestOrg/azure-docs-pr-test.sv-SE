---
title: "aaaNo prenumerationer påträffade fel när försöker toosign i tooAzure portal eller Azure kontocenter | Microsoft Docs"
description: "Är hello lösning på problemet som inga prenumerationer hittades fel uppstår när loggar in tooAzure portalen eller Azure kontocenter."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: genli
ms.openlocfilehash: def4d4a1f883dd948fe8132f2d85abc4c23ae624
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a><span data-ttu-id="d5543-103">Inga prenumerationer hittades fel i Azure-portalen eller Azure-kontocenter</span><span class="sxs-lookup"><span data-stu-id="d5543-103">No subscriptions found error in Azure portal or Azure account center</span></span>
<span data-ttu-id="d5543-104">Du kan få ett meddelande om ”inga prenumerationer hittades” när du försöker toosign i toohello [Azure-portalen](https://portal.azure.com/) eller hello [Azure kontocenter](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="d5543-104">You might receive a "No subscriptions found" error message when you try toosign in toohello [Azure portal](https://portal.azure.com/) or hello [Azure account center](https://account.windowsazure.com/Subscriptions).</span></span> <span data-ttu-id="d5543-105">Den här artikeln innehåller en lösning på problemet.</span><span class="sxs-lookup"><span data-stu-id="d5543-105">This article provides a solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="d5543-106">Symtom</span><span class="sxs-lookup"><span data-stu-id="d5543-106">Symptom</span></span>

<span data-ttu-id="d5543-107">När du försöker toosign i toohello [Azure-portalen](https://portal.azure.com/) eller hello [Azure kontocenter](https://account.windowsazure.com/Subscriptions), du får följande felmeddelande hello: ”inga prenumerationer hittades”.</span><span class="sxs-lookup"><span data-stu-id="d5543-107">When you try toosign in toohello [Azure portal](https://portal.azure.com/) or hello [Azure account center](https://account.windowsazure.com/Subscriptions), you receive hello following error message: "No subscriptions found".</span></span>

## <a name="cause"></a><span data-ttu-id="d5543-108">Orsak</span><span class="sxs-lookup"><span data-stu-id="d5543-108">Cause</span></span>

<span data-ttu-id="d5543-109">Det här problemet uppstår om ditt konto inte har tillräcklig behörighet.</span><span class="sxs-lookup"><span data-stu-id="d5543-109">This problem occurs if your account doesn’t have sufficient permissions.</span></span> 

## <a name="solution"></a><span data-ttu-id="d5543-110">Lösning</span><span class="sxs-lookup"><span data-stu-id="d5543-110">Solution</span></span>

<span data-ttu-id="d5543-111">Kontrollera att du logga in som administratör för hello korrekt.</span><span class="sxs-lookup"><span data-stu-id="d5543-111">Make sure that you log in as hello correct administrator.</span></span> <span data-ttu-id="d5543-112">En kontoadministratör kan komma åt endast hello Account Center.</span><span class="sxs-lookup"><span data-stu-id="d5543-112">An Account Administrator can access only hello Account Center.</span></span> <span data-ttu-id="d5543-113">Tjänsten administratörer (SA) och Medadministratörer (CA) har åtkomst behörighet endast toohello Azure-portalen eller hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5543-113">Service Administrators (SA) and Co-Administrators (CA) have access permission only toohello Azure portal or hello Azure classic portal.</span></span>

### <a name="scenario-1-error-message-is-received-in-hello-azure-portalhttpsportalazurecom"></a><span data-ttu-id="d5543-114">Scenario 1: Meddelande tas emot i hello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="d5543-114">Scenario 1: Error message is received in hello [Azure portal](https://portal.azure.com)</span></span>

<span data-ttu-id="d5543-115">toofix problemet:</span><span class="sxs-lookup"><span data-stu-id="d5543-115">toofix this issue:</span></span>

* <span data-ttu-id="d5543-116">Kontrollera att rätt Azure directory väljs genom att klicka på ditt konto på hello uppe till höger hello.</span><span class="sxs-lookup"><span data-stu-id="d5543-116">Make sure that hello correct Azure directory is selected by clicking your account at hello top right.</span></span>

  ![Välj hello katalog vid hello uppifrån höger om hello Azure-portalen](./media/billing-no-subscriptions-found/directory-switch.png)

* <span data-ttu-id="d5543-118">Om hello höger Azure-katalogen har valts men fortfarande felmeddelande hello, [ditt konto som ägare](billing-add-change-azure-subscription-administrator.md).</span><span class="sxs-lookup"><span data-stu-id="d5543-118">If hello right Azure directory is selected but you still receive hello error message, [have your account added as an Owner](billing-add-change-azure-subscription-administrator.md).</span></span>

### <a name="scenario-2-error-message-is-received-in-hello-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a><span data-ttu-id="d5543-119">Scenario 2: Felmeddelande tas emot i hello [Azure kontocenter](https://account.windowsazure.com/Subscriptions)</span><span class="sxs-lookup"><span data-stu-id="d5543-119">Scenario 2: Error message is received in hello [Azure account center](https://account.windowsazure.com/Subscriptions)</span></span>

<span data-ttu-id="d5543-120">Kontrollera om hello-konto som du använde hello kontoadministratör.</span><span class="sxs-lookup"><span data-stu-id="d5543-120">Check whether hello account that you used is hello Account Administrator.</span></span> <span data-ttu-id="d5543-121">tooverify som hello kontoadministratör, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="d5543-121">tooverify who hello Account Administrator is, follow these steps:</span></span>

1. <span data-ttu-id="d5543-122">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d5543-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d5543-123">På navmenyn hello väljer **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d5543-123">On hello Hub menu, select **Subscription**.</span></span>
3. <span data-ttu-id="d5543-124">Välj hello prenumeration som du vill toocheck och välj sedan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d5543-124">Select hello subscription that you want toocheck, and then select **Settings**.</span></span>
4. <span data-ttu-id="d5543-125">Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="d5543-125">Select **Properties**.</span></span> <span data-ttu-id="d5543-126">hello kontoadministratör för hello prenumerationen visas i hello **kontoadministratören** rutan.</span><span class="sxs-lookup"><span data-stu-id="d5543-126">hello account administrator of hello subscription is displayed in hello **Account Admin** box.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="d5543-127">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="d5543-127">Need help?</span></span> <span data-ttu-id="d5543-128">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="d5543-128">Contact support.</span></span>
<span data-ttu-id="d5543-129">Om du fortfarande behöver hjälp [supporten](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) tooget problemet lösas snabbt.</span><span class="sxs-lookup"><span data-stu-id="d5543-129">If you still need help, [contact support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) tooget your issue resolved quickly.</span></span> 
