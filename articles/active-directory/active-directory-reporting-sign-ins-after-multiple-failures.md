---
title: Inloggningar efter flera fel
description: "En rapport som visar användare som har loggat in efter flera efterföljande misslyckade logga inloggningsförsök."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: femila
editor: 
ms.assetid: e4ec1a39-9c20-418f-8a75-6497d0117176
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: e55e0145adbdb1f41a8b8753d5555f20e96bf161
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-after-multiple-failures"></a><span data-ttu-id="afdc0-103">Inloggningar efter flera fel</span><span class="sxs-lookup"><span data-stu-id="afdc0-103">Sign-ins after multiple failures</span></span>
<span data-ttu-id="afdc0-104">Den här rapporten visar användare som har loggat in efter flera efterföljande misslyckade logga inloggningsförsök.</span><span class="sxs-lookup"><span data-stu-id="afdc0-104">This report indicates users who have successfully signed in after multiple consecutive failed sign in attempts.</span></span> <span data-ttu-id="afdc0-105">Möjliga orsaker är:</span><span class="sxs-lookup"><span data-stu-id="afdc0-105">Possible causes include:</span></span>

* <span data-ttu-id="afdc0-106">Användaren har glömt sitt lösenord</span><span class="sxs-lookup"><span data-stu-id="afdc0-106">User had forgotten their password</span></span></li><li><span data-ttu-id="afdc0-107">Användaren är drabbade lyckade lösenord för att gissa brute force attack</span><span class="sxs-lookup"><span data-stu-id="afdc0-107">User is the victim of a successful password guessing brute force attack</span></span>

<span data-ttu-id="afdc0-108">Resultaten från den här rapporten visar antalet misslyckade försök i följd inloggning gjorda innan den lyckas inloggningen och en tidsstämpel som är associerade med den första lycka inloggningen.</span><span class="sxs-lookup"><span data-stu-id="afdc0-108">Results from this report will show you the number of consecutive failed sign-in attempts made prior to the successful sign-in and a timestamp associated with the first successful sign-in.</span></span>

<span data-ttu-id="afdc0-109">**Rapportera inställningar**: du kan konfigurera det minsta antalet på varandra följande loggar för misslyckade inloggningsförsök som måste inträffa innan den kan visas i rapporten.</span><span class="sxs-lookup"><span data-stu-id="afdc0-109">**Report Settings**: You can configure the minimum number of consecutive failed sign in attempts that must occur before it can be displayed in the report.</span></span> <span data-ttu-id="afdc0-110">När du gör ändringar i den här inställningen är det viktigt att notera att inte tillämpas dessa ändringar till alla befintliga Misslyckad inloggning moduler som för närvarande visas i den befintliga rapporten.</span><span class="sxs-lookup"><span data-stu-id="afdc0-110">When you make changes to this setting it is important to note that these changes will not be applied to any existing failed sign ins that currently show up in your existing report.</span></span> <span data-ttu-id="afdc0-111">De kommer att användas för alla framtida inloggningar.</span><span class="sxs-lookup"><span data-stu-id="afdc0-111">However, they will be applied to all future sign-ins.</span></span> <span data-ttu-id="afdc0-112">I den här rapporten kan bara ändras av licensierade administratörer.</span><span class="sxs-lookup"><span data-stu-id="afdc0-112">Changes to this report can only be made by licensed admins.</span></span>

![Inloggningar efter flera fel](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

