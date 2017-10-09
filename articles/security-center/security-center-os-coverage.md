---
title: aaaSupported plattformar i Azure Security Center | Microsoft Docs
description: "Det här dokumentet innehåller en lista över Windows- och Linux operatings system som stöds i Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: f73e04970749271fc9d75836f4f468b0a4953f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="supported-platforms-in-azure-security-center"></a>Plattformar som stöds i Azure Security Center
Tillstånd säkerhetsövervakning och rekommendationer är tillgängliga för virtuella datorer (VM) med hjälp av både hello klassiska och Resource Manager distributionsmodellerna.

> [!NOTE]
> Mer information om hello [klassisk och Resource Manager distributionsmodellerna](../azure-classic-rm.md) för Azure-resurser.
>
>

## <a name="supported-platforms-for-windows-vms"></a>Plattformar som stöds för virtuella Windows-datorer
Windows operativsystem:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

> [!NOTE]
>
* OS säkerhetsproblem bedömningar är ännu inte tillgängliga för Windows Server 2016.
* Crash analysis identifieringar stöds endast för Windows Server 2012 och Windows Server 2012 R2.
>
>

## <a name="supported-platforms-for-linux-vms"></a>Plattformar som stöds för virtuella Linux-datorer
Operativsystem som stöds Linux:

* Versioner av Ubuntu 12.04, 14.04, 16.04, 16.10
* Debian versioner 7, 8
* CentOS version 6. \*, 7.*
* Red Hat Enterprise Linux (RHEL) version 6. \*, 7.*
* SUSE Linux Enterprise Server (SLES) versioner 11 SP4 +, 12.*
* Oracle Linux-version 6. \*, 7.*

> [!NOTE]
> Virtuella beteendeanalys är ännu inte tillgängliga för Linux-operativsystem.
>
>

## <a name="vms-and-cloud-services"></a>Virtuella datorer och molntjänster
Virtuella datorer som körs i en molntjänst stöds också. Endast cloud services webb- och arbetsroller roller som körs i produktion fack övervakas. toolearn mer information om Molntjänsten, se [översikt över molntjänster](../cloud-services/cloud-services-choose-me.md).

## <a name="next-steps"></a>Nästa steg

- [Planera för Azure Security Center och handboken](security-center-planning-and-operations-guide.md) – Lär dig hur tooplan och förstå hello design överväganden tooadopt Azure Security Center
- [Säkerhetsaviseringar efter typ i Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-alerts-type.md#virtual-machine-behavioral-analysis) – Lär dig mer om virtuella beteendeanalys och krascher dump minnesanalys i Security Center
- [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten
- [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – hittar du blogginlägg om säkerhet och Azure kompatibilitet
