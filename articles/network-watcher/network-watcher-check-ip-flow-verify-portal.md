---
title: Kontrollera aaaVerify trafik med Azure Network Watcher IP - Azure-portalen | Microsoft Docs
description: "Den här artikeln beskriver hur toocheck om trafik tooor från en virtuell dator tillåts eller nekas"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e0e3e9a8-70eb-409a-a744-0ce9deb27148
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: abf639f36d32f3416dd927e66b635267b746e62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-tooor-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Kontrollera om trafik som tillåts eller nekas tooor från en virtuell dator med IP-flöda Kontrollera en komponent i Azure Nätverksbevakaren

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-flöde verifiera är en funktion i Nätverksbevakaren som gör att du tooverify om trafik tillåts tooor från en virtuell dator. hello verifiering kan köras för inkommande eller utgående trafik. Det här scenariot är användbart tooget aktuella tillstånd om en virtuell dator kan prata tooan extern resurs eller en annan resurs. IP-flöde verifiera går att använda tooverify om Nätverkssäkerhetsgrupp (NSG)-regler är korrekt konfigurerade och felsöka flöden som blockeras av NSG-regler. En annan orsak till att använda IP flödet Kontrollera tooensure trafik som du vill blockerade blockeras korrekt av hello NSG.

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren eller har en befintlig instans av Nätverksbevakaren. hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.

## <a name="scenario"></a>Scenario

Det här scenariot använder IP-flöda Kontrollera tooverify om en virtuell dator kan prata tooanother datorn via port 443. Om hello trafik nekas returnerar hello säkerhetsregel som nekar trafiken. toolearn mer om IP-flöda Kontrollera finns [flöda Kontrollera översikt över IP](network-watcher-ip-flow-verify-overview.md)

### <a name="run-ip-flow-verify"></a>Kontrollera kör IP-flöde

Navigera tooyour Nätverksbevakaren och klicka på **IP-flöde Kontrollera**. Välj hello virtuell dator och gränssnitt som du vill tooverify trafik från. Ange ytterligare filtrering information och klickar på **Kontrollera**.

När du klickar på **Kontrollera**, hello flödet baserat på hello kriterier som du angav är markerad. hello resultatet är antingen **åtkomst tillåts** eller **åtkomst nekad**. Om åtkomst nekas hello Nätverkssäkerhetsgrupp (NSG) och säkerhetsregeln som blockerar trafik tillhandahålls. Om hello DOS-trafik är förväntat, lyckades hello regeln.

> [!NOTE]
> IP-flöde Kontrollera kräver att hello Virtuella datorresursen allokeras.

Som du ser i följande bild hello tilläts hello utgående HTTPS-trafik.

![IP-flöde Kontrollera översikt][1]

Som det visas i följande bild hello trafik ändras tooinbound och hello inkommande port ändras too123. Trafik nekas nu har hello-meddelande ”åtkomst nekad” angetts tillsammans med hello grupp och säkerhet nätverkssäkerhetsregeln som nekar hello trafik.

![IP-flöde resultat][2]

## <a name="next-steps"></a>Nästa steg

Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













