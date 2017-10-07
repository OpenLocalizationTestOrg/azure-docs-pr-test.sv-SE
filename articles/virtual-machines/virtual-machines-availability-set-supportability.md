---
title: "aaaSupportability för att lägga till virtuella datorer i Azure tooan befintlig tillgänglighetsuppsättning | Microsoft Docs"
description: "Support för att lägga till virtuella datorer i Azure tooan befintlig tillgänglighetsuppsättning."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/15/2017
ms.author: delhan
ms.openlocfilehash: dc2bd86b916f1d1a0a0d4c9e870df829434c96b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supportability-of-adding-azure-vms-tooan-existing-availability-set"></a>Support för att lägga till virtuella datorer i Azure tooan befintlig tillgänglighetsuppsättning

Du kan ibland uppstå begränsningar när du lägger till nya virtuella datorer (VM) tooan befintlig tillgänglighetsuppsättning. hello följande diagram information som du kan blanda i virtuell dator-serie hello samma tillgänglighetsuppsättning.

Här är hello support matrisen toomix olika typer av virtuella datorer:

Serie & Tillgänglighetsuppsättning|Andra VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Första virtuella dator|||||||
|A||OKEJ|OKEJ|OKEJ|OKEJ|OKEJ|
|Av2||OKEJ|OKEJ|OKEJ|OKEJ|OKEJ|
|D||OKEJ|OKEJ|OKEJ|OKEJ|OKEJ|
|Dv2||OKEJ|OKEJ|OKEJ|OKEJ|OKEJ|
|Dv3||OKEJ|OKEJ|OKEJ|OKEJ|OKEJ|

Alla serier hittades inte i samma tillgänglighetsuppsättning eftersom de kräver att en specifik maskinvara hello.
