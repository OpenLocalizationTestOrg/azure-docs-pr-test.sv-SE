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
# <a name="sign-ins-after-multiple-failures"></a>Inloggningar efter flera fel
Den här rapporten visar användare som har loggat in efter flera efterföljande misslyckade logga inloggningsförsök. Möjliga orsaker är:

* Användaren har glömt sitt lösenord</li><li>Användaren är drabbade lyckade lösenord för att gissa brute force attack

Resultaten från den här rapporten visar antalet misslyckade försök i följd inloggning gjorda innan den lyckas inloggningen och en tidsstämpel som är associerade med den första lycka inloggningen.

**Rapportera inställningar**: du kan konfigurera det minsta antalet på varandra följande loggar för misslyckade inloggningsförsök som måste inträffa innan den kan visas i rapporten. När du gör ändringar i den här inställningen är det viktigt att notera att inte tillämpas dessa ändringar till alla befintliga Misslyckad inloggning moduler som för närvarande visas i den befintliga rapporten. De kommer att användas för alla framtida inloggningar. I den här rapporten kan bara ändras av licensierade administratörer.

![Inloggningar efter flera fel](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

