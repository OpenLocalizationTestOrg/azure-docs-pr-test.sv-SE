---
title: aaaSign moduler efter flera fel
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
ms.openlocfilehash: 48d137dc3abf65287cb3b9ba8a6ff10340f6741f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-ins-after-multiple-failures"></a><span data-ttu-id="b40c2-103">Inloggningar efter flera fel</span><span class="sxs-lookup"><span data-stu-id="b40c2-103">Sign-ins after multiple failures</span></span>
<span data-ttu-id="b40c2-104">Den här rapporten visar användare som har loggat in efter flera efterföljande misslyckade logga inloggningsförsök.</span><span class="sxs-lookup"><span data-stu-id="b40c2-104">This report indicates users who have successfully signed in after multiple consecutive failed sign in attempts.</span></span> <span data-ttu-id="b40c2-105">Möjliga orsaker är:</span><span class="sxs-lookup"><span data-stu-id="b40c2-105">Possible causes include:</span></span>

* <span data-ttu-id="b40c2-106">Användaren har glömt sitt lösenord</span><span class="sxs-lookup"><span data-stu-id="b40c2-106">User had forgotten their password</span></span></li><li><span data-ttu-id="b40c2-107">Användaren är hello offer för en lyckad nyckelsökningsangrepp att gissa lösenord</span><span class="sxs-lookup"><span data-stu-id="b40c2-107">User is hello victim of a successful password guessing brute force attack</span></span>

<span data-ttu-id="b40c2-108">Resultaten från den här rapporten visar hello av antalet misslyckade försök i följd inloggning gjorts tidigare toohello lyckad inloggning och en tidsstämpel som är associerade med hello första lyckade inloggning.</span><span class="sxs-lookup"><span data-stu-id="b40c2-108">Results from this report will show you hello number of consecutive failed sign-in attempts made prior toohello successful sign-in and a timestamp associated with hello first successful sign-in.</span></span>

<span data-ttu-id="b40c2-109">**Rapportera inställningar**: du kan konfigurera hello minsta antal på varandra följande loggar för misslyckade inloggningsförsök som måste inträffa innan den kan visas i hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="b40c2-109">**Report Settings**: You can configure hello minimum number of consecutive failed sign in attempts that must occur before it can be displayed in hello report.</span></span> <span data-ttu-id="b40c2-110">När du ändrar toothis som du anger är viktiga toonote att ändringarna inte tillämpade tooany befintliga misslyckade inloggningar som för närvarande visas i den befintliga rapporten.</span><span class="sxs-lookup"><span data-stu-id="b40c2-110">When you make changes toothis setting it is important toonote that these changes will not be applied tooany existing failed sign ins that currently show up in your existing report.</span></span> <span data-ttu-id="b40c2-111">De kommer dock tillämpade tooall framtida inloggningar. Rapport om toothis kan endast göras av licensierade administratörer.</span><span class="sxs-lookup"><span data-stu-id="b40c2-111">However, they will be applied tooall future sign-ins. Changes toothis report can only be made by licensed admins.</span></span>

![Inloggningar efter flera fel](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

