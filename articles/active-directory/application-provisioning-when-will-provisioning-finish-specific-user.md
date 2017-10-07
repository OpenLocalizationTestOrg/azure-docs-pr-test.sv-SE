---
title: "aaaFind ut när en viss användare kommer att kunna tooaccess ett program | Microsoft Docs"
description: "Hur toofind ut när en ytterst viktigt användare vara kan tooaccess ett program du har konfigurerat för användaretablering med Azure AD"
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
ms.openlocfilehash: bb9520499dcc8bbbe6fae05c5238c8852815ea0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-tooaccess-an-application"></a>Ta reda på när en viss användare kommer att kunna tooaccess ett program
När du använder automatisk användaretablering med ett program, Azure AD automatiskt etablera och uppdatera användarkonton i en app baserat på sådant som [användar- och tilldelning](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) vid en schemalagd tid intervall, vanligtvis var 10 minuter.

## <a name="how-long-does-it-take"></a>Hur lång tid tar det?

hello tid det tar för en viss användare toobe etablerats beror huvudsakligen på huruvida en inledande ”full” sync redan har inträffat.

Hej första synkronisering mellan Azure AD och en app kan ta allt från 20 minuter tooseveral timmar, beroende på hello storleken på hello Azure AD-katalogen och hello antalet användare i omfånget för etablering. 

Efterföljande synkroniseringar efter hello inledande synkronisering vara snabbare (t.ex. inom 10 minuter) som hello Etablerar tjänsten lagrar vattenstämplar som representerar hello tillståndet för båda systemen efter hello inledande synkronisering, förbättra prestanda för efterföljande synkronisering.

## <a name="how-toocheck-hello-status-of-a-user"></a>Hur toocheck hello status för en användare

toosee hello Etableringsstatus för en vald användare se hello granskningsloggar i Azure AD.

hello etablering granskningsloggar kan nås i hello Azure-portalen i hello **Azure Active Directory &gt; Företagsappar &gt; \[programnamn\] &gt; granskningsloggarna**fliken. Filter hello loggar in hello **Kontoetablering** kategori tooonly finns hello etablering händelser för appen. Du kan söka efter användare baserat på hello ”matchande ID” som konfigurerats för dem i hello attributmappning. 

Om du har konfigurerat hello ”huvudnamn” eller ”e-postadress” som hello matchar attributet på hello Azure AD-sidan och hello användaren inte etablering har värdet till exempel ”audrey@contoso.com”, sedan Sök hello granskningsloggar för ”audrey@contoso.com” och granska sedan poster som returneras.

hello etablering audit loggar register alla hello åtgärder som utförs av hello etableras, inklusive:

* Hämta Azure AD för tilldelade användare som ingår i omfånget för etablering
* Frågar hello mål app hello befintliga användare
* Jämföra hello användarobjekt mellan hello system
* Lägga till, uppdatera eller inaktivera hello användarkonto i hello målsystemet baserat på hello jämförelse

## <a name="next-steps"></a>Nästa steg
[Automatisera Användaretablering och avetablering tooSaaS program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''
