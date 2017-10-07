---
title: "aaaManage nätverk grupp flödet säkerhetsloggar med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage Nätverkssäkerhetsgruppen flöde loggar i Azure Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fb250337ab9d1a0c0d0d3569c00bc221dd102a3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-group-flow-logs-in-hello-azure-portal"></a>Hantera nätverket grupp flödet säkerhetsloggar i hello Azure-portalen

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST-API](network-watcher-nsg-flow-logging-rest.md)

Nätverket säkerhetsloggar grupp flöde är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en nätverkssäkerhetsgrupp. Loggarna flödet skrivs i JSON-format och innehåller viktig information, inklusive: 

- Utgående och inkommande flöden på grundval av per regel.
- hello NIC som hello flödet gäller.
- 5-tuppel information om hello flödet (källan/målet IP, källan/målet port och protokoll).
- Information om huruvida trafik tillåtas eller nekas.

## <a name="before-you-begin"></a>Innan du börjar

Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en instans av Nätverksbevakaren](network-watcher-create.md). hello scenariot förutsätter att du har en resursgrupp med en giltig virtuell dator.

## <a name="register-insights-provider"></a>Registrera providern insikter

För flöde loggning toowork, hello **Microsoft.Insights** provider måste vara registrerad. tooregister hello provider, vidta hello följande steg: 

1. Gå för**prenumerationer**, och välj sedan hello prenumeration som du vill tooenable flöde loggar. 
2. På hello **prenumeration** bladet väljer **Resursproviders**. 
3. Titta på hello lista över leverantörer och kontrollera att hello **microsoft.insights** providern är registrerad. Om inte, välj sedan **registrera**.

![Visa providers][providers]

## <a name="enable-flow-logs"></a>Aktivera flödet loggar

Dessa steg leder dig genom hello innebär att aktivera flödet loggar på en nätverkssäkerhetsgrupp.

### <a name="step-1"></a>Steg 1

Gå tooa Nätverksbevakaren instansen och välj sedan **NSG flöda loggar**.

![Översikt över flödet-loggar][1]

### <a name="step-2"></a>Steg 2

Välj en nätverkssäkerhetsgrupp hello-listan.

![Översikt över flödet-loggar][2]

### <a name="step-3"></a>Steg 3 

På hello **flöde loggar inställningar** bladet ange hello status för**på**, och konfigurera ett lagringskonto.  När du är klar väljer du **OK**. Välj sedan **spara**.

![Översikt över flödet-loggar][3]

## <a name="download-flow-logs"></a>Hämta flödet loggar

Flödet loggar sparas i ett lagringskonto. Hämta din flödet loggar tooview dem.

### <a name="step-1"></a>Steg 1

Välj toodownload flöde loggar **kan du hämta flödet loggar från konfigurerade lagringskonton**. Det här steget anser du tooa storage-konto där du kan välja vilka loggar toodownload.

![Flöda inställningarna för webbprogramloggar][4]

### <a name="step-2"></a>Steg 2

Gå toohello rätt storage-konto. Välj sedan **behållare** > **insights-log-networksecuritygroupflowevent**.

![Flöda inställningarna för webbprogramloggar][5]

### <a name="step-3"></a>Steg 3

Gå toohello plats hello flödet loggen markerar du den och välj sedan **hämta**.

![Flöda inställningarna för webbprogramloggar][6]

Information om hello struktur hello loggen finns [nätverk grupp flödet loggen Säkerhetsöversikt](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
