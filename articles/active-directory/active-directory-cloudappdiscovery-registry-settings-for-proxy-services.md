---
title: "aaaCloud App Discovery-registerinställningar för Proxy-tjänster | Microsoft Docs"
description: "hello syftet med det här avsnittet är tooprovide du med hello steg du behöver tooperform tooset hello krävs port på hello-datorer som kör hello Cloud App Discovery-agenten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bb1fe20016459160b4f67cb0125b1781a0260c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery-registerinställningar för Proxy-tjänster
Hello Cloud App Discovery-agenten är som standard konfigurerade toouse endast hello portarna 80 eller 443. Om du planerar att installera Cloud App Discovery i en miljö med en proxyserver som använder en anpassad port (varken 80 eller 443), måste tooconfigure agenter-toouse denna port. hello konfigurationen baseras på en registernyckel.

hello syftet med det här avsnittet är tooprovide du med hello steg du behöver tooperform tooset hello krävs port på hello-datorer som kör hello Cloud App Discovery-agenten.

**toomodify hello port som används av hello datorn som kör hello Cloud App Discovery-agenten, utför hello följande steg:**

1. Starta hello Registereditorn. <br> ![Kör](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Navigera tooor skapa hello följande registernyckel: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint** 
3. Skapa en ny **multisträng** värde med namnet **portar**. ![Ny](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. tooopen hello **redigera multisträng** dialogrutan, dubbelklicka på värdet för hello-portar.
5. Skriv hello följande värden i hello värdet data textruta och lägga till alla anpassade portar som används av din organisation: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Redigera multisträng](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Klicka på **OK** tooclose hello **redigera multisträng** dialogrutan.

**Ytterligare resurser**

* [Hur kan identifiera ej sanktionerade molnappar som används inom organisationen](active-directory-cloudappdiscovery-whatis.md) 

