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
# <a name="sign-ins-after-multiple-failures"></a>Inloggningar efter flera fel
Den här rapporten visar användare som har loggat in efter flera efterföljande misslyckade logga inloggningsförsök. Möjliga orsaker är:

* Användaren har glömt sitt lösenord</li><li>Användaren är hello offer för en lyckad nyckelsökningsangrepp att gissa lösenord

Resultaten från den här rapporten visar hello av antalet misslyckade försök i följd inloggning gjorts tidigare toohello lyckad inloggning och en tidsstämpel som är associerade med hello första lyckade inloggning.

**Rapportera inställningar**: du kan konfigurera hello minsta antal på varandra följande loggar för misslyckade inloggningsförsök som måste inträffa innan den kan visas i hello rapporten. När du ändrar toothis som du anger är viktiga toonote att ändringarna inte tillämpade tooany befintliga misslyckade inloggningar som för närvarande visas i den befintliga rapporten. De kommer dock tillämpade tooall framtida inloggningar. Rapport om toothis kan endast göras av licensierade administratörer.

![Inloggningar efter flera fel](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

