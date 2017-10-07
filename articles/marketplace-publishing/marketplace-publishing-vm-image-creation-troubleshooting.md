---
title: aaaHow tootroubleshoot vanliga problem vid skapandet av VHD | Microsoft Docs
description: "Felsökning av svaren toocommon frågor och problem under skapande av virtuell Hårddisk."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: e4ff09a979bdf575badff2d33f2299abb17c947d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-common-issues-encountered-during-vhd-creation"></a>Hur vanliga problem med tootroubleshoot påträffades under skapande av virtuell Hårddisk
Den här artikeln är angivna toohelp ett Azure Marketplace utgivaren och/eller medadministratör som kan ha ett problem eller har frågor när du publicerar eller hantera sina virtuella solution(s).

1. Hur ändrar jag hello namnet på hello värden?
   
    När du har skapat VM kan inte användare uppdatera hello namnet på hello-värden.
2. Hur hello tooreset Fjärrskrivbordstjänsten eller dess inloggningslösenord?
   
   * [Referens för Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Referens för Linux VM](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Hur toogenerate nya ssh-certifikat?
   
   Se toohello länk: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Hur tooconfigure ett öppna VPN-certifikat?
   
   Se toohello länk: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Vad är hello Supportpolicy för att köra Microsoft server-program i hello Microsoft Azure virtuell miljö (infrastructure-as-a-service)?
   
   Se toohello länk: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Har ingen unik identifierare i virtuella datorer?
   
   Azure kodar Azure VM unika ID på varje virtuell dator. Mer information finns i den här bloggen och dokumentationen.
7. På en virtuell dator hur kan jag hantera hello tillägget för anpassat skript i hello startaktivitet?
   
   Se toohello länk: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Hur toocreate som en virtuell dator från Azure portal med hello hello VHD som är upp toopremium storage?
   
   Vi stöder inte den här funktionen ännu.
9. Är en 32-bitars-app som stöds i hello Azure Marketplace?
   
   Mer information om hello Supportpolicy finns toohello länk: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Varje gång jag skulle toocreate en bild från min virtuella hårddiskar, visas felmeddelandet hello ”. Virtuell Hårddisk är redan registrerad med avbildningslagringsplats som resursen hello ”i PowerShell. Skapade inte någon bild innan eller hittade I en bild med det här namnet i Azure. Hur lösa problemet?
    
    Vanligtvis inträffa om användaren hello etableras en virtuell dator från den här virtuella Hårddisken och det finns ett lås på den virtuella Hårddisken. Kontrollera att det finns ingen VM allokerats från den här virtuella Hårddisken. Om du hello fel fortfarande kvar måste du höja ett supportärende med hjälp av den här länken eller från hello Publicera portalen angående detta (information ges i hello svar för frågan 11).
