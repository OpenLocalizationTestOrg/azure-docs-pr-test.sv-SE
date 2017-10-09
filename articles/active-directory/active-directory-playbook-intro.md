---
title: aaaAzure Active Directory PoC Playbook introduktion | Microsoft Docs
description: "Utforska och snabbt implementera scenarier för identitets- och åtkomsthantering"
services: active-directory
keywords: Azure active directory, playbook, konceptbevis, PoC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 655524e9692de46e831fc68e1636e6c20d41958f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a>Azure Active Directory bevis på koncept Playbook: introduktion

Den här artikeln innehåller riktlinjer tooexplore olika Azure AD-funktioner i ett konceptbevis (PoC). hello avsedd målgruppen för det här dokumentet är identitet arkitekter, IT-proffs och systemintegrerare.

## <a name="how-toouse-this-playbook"></a>Hur toouse denna Playbook

1. Använd hello [tema](active-directory-playbook-ingredients.md#theme) avsnittet och välj hello områden av intresse baserat på dina behov.  
2. Omfång hello PoC genom att välja hello-scenarier som överensstämmer med dina affärsmål. hello kortare hello bättre. Vi rekommenderar att du gör som kort och koncis som möjligt tooconvey hello värde toohello intressenter och minimerar hello komplexitet toorealize den.  
3. Använd hello [implementering](active-directory-playbook-implementation.md) avsnittet toounderstand hello scenarier och vad betyder de för din miljö. I varje scenario beskrivs hur tooset den (vi kallar [byggblock](active-directory-playbook-building-blocks.md)), och hur toonavigate hello scenarier. 
4. Varje byggblock förklarar hello förutsättningar, samt en ungefärliga tid toocomplete. Detta kan hjälpa dig under hello planeringsprocessen. 
5. Baserat på 1 till 3 ovan, definiera hello [miljö](active-directory-playbook-ingredients.md#environment) i vilka tooexecute. Vi rekommenderar att toostrive för en produktion miljö tooget en bra känsla hello upplevelse för användarna. 
6. När med motstridiga krav, använder du matrisen användbara förhållandet 
   * Tema till Central visar värde  
   * Jämn tooprepare tooset upp och tooexecute hello-scenarier 
   * Minimal tid tooexecute hello målscenarier 
   * Som nära tooproduction som möjligt i din begränsningar 

>[!NOTE]
> I den här artikeln visas vissa specifika tredjepartsprogram och produkter som exempel i informationssyfte. Azure AD stöder tusentals program i vår [programgalleriet](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) som du kan använda baserat på dina behov och miljö. 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]