---
title: aaaAssign grupper tooAzure AD-appar | Microsoft Docs
description: "Hur tooimplement gruppera tilldelning för Azure-program."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: 086619df09c13bf259afc3128d45ed804b99e519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-azure-active-directory-groups-tooan-application"></a>Tilldela Azure Active Directory-grupper tooan program
Innan du kan tilldela användare och grupper tooan program, måste du begära Användartilldelning. toolearn hur toorequire Användartilldelning finns hello [kräver Användartilldelning](active-directory-applications-guiding-developers-requiring-user-assignment.md) artikel.

Den här artikeln förutsätter att du redan har skapat grupper i hello active directory som du använder för det här programmet.

## <a name="assigning-groups-tooan-application"></a>Tilldela grupper tooan program
1. Logga in toohello Azure-portalen med ett administratörskonto.
2. Klicka på hello **alla objekt** objekt i hello huvudmenyn.
3. Välja hello-katalog som du använder för hello program.
4. Klicka på hello **program** fliken.
5. Välj hello programmet hello listan över program som är associerade med den här katalogen.
6. Klicka på hello **användare och grupper** fliken.
7. Filter hello listan över grupper i active directory med hjälp av hello **grupper** listrutan.
8. Välj hello grupp.
9. Klicka på **TILLDELA**.
10. Klicka på **Ja** när du tillfrågas.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
