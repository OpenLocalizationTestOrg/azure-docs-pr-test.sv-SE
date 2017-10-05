---
title: "Cloud App Discovery-registerinställningar för Proxy-tjänster | Microsoft Docs"
description: "Syftet med det här avsnittet är att förse dig med de steg som du behöver utföra för att ställa in den begärda porten på datorer som kör Cloud App Discovery-agenten."
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
ms.openlocfilehash: ea15dc9a9f20a296e622c8fb1011f7ee99de3e99
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery-registerinställningar för Proxy-tjänster
Cloud App Discovery-agenten har konfigurerats för att använda endast portarna 80 eller 443 som standard. Om du planerar att installera Cloud App Discovery i en miljö med en proxyserver som använder en anpassad port (varken 80 eller 443), måste du konfigurera dina agenter för att använda den här porten. Konfigurationen är baserad på en registernyckel.

Syftet med det här avsnittet är att förse dig med de steg som du behöver utföra för att ställa in den begärda porten på datorer som kör Cloud App Discovery-agenten.

**Utför följande steg om du vill ändra den port som används av datorn som kör Cloud App Discovery-agenten:**

1. Starta Registereditorn. <br> ![Kör](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Navigera till eller skapa följande registernyckel: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint** 
3. Skapa en ny **multisträng** värde med namnet **portar**. ![Ny](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Öppna den **redigera multisträng** dialogrutan, dubbelklicka på värdet portar.
5. Ange följande värden i textrutan värde data och Lägg till alla anpassade portar som används av din organisation: <br><br>
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
6. Klicka på **OK** att stänga den **redigera multisträng** dialogrutan.

**Ytterligare resurser**

* [Hur kan identifiera ej sanktionerade molnappar som används inom organisationen](active-directory-cloudappdiscovery-whatis.md) 

