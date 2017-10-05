---
title: "Begränsningar i Azure-databas för MySQL | Microsoft Docs"
description: "Beskriver begränsningar i förhandsversionen i Azure-databas för MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c61d70897b66c2ffee819ac98c38ab75000db907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>Begränsningar i Azure-databas för MySQL (förhandsgranskning)
Azure-databasen för MySQL-tjänsten är tillgänglig som förhandsversion. I följande avsnitt beskrivs kapacitet och funktionella gränser i databastjänsten för.

## <a name="service-tier-maximums"></a>Tjänsten nivå maxkapacitet
Azure MySQL-databas har flera tjänstnivåer som du kan välja mellan när du skapar en server. Mer information finns i [förstå vad som är tillgängliga i varje tjänstnivå](concepts-service-tiers.md).  

Det finns ett maximalt antal anslutningar, enheter för beräkning och lagring i varje tjänstnivå under förhandsgranskningen tjänsten enligt följande: 

|                            |                   |
| :------------------------- | :---------------- |
| **Högsta antal anslutningar**        |                   |
| Grundläggande 50 beräknings-enheter     | 50-anslutningar    |
| Grundläggande 100 beräknings-enheter    | 100-anslutningar   |
| Standardenheter 100 beräkning | 200-anslutningar   |
| Standardenheter 200 beräkning | 300 anslutningar   |
| Standardenheter 400 beräkning | 400-anslutningar   |
| Standard 800 beräknings-enheter | 500-anslutningar   |
| **Max beräknings-enheter**      |                   |
| Grundläggande tjänstenivå         | 100 enheter för beräkning |
| Standardtjänstenivå      | 800 beräknings-enheter |
| **Maximalt lagringsutrymme**            |                   |
| Grundläggande tjänstenivå         | 1 TB              |
| Standardtjänstenivå      | 1 TB              |

När för många anslutningar har uppnåtts, får du följande fel:
> FEL 1040 (08004): För många anslutningar

## <a name="preview-functional-limitations"></a>Funktionella begränsningar i förhandsversionen:
### <a name="scale-operations"></a>Skalningsåtgärder:
1.  Dynamisk skalning av servrar över tjänstnivåer stöds inte för närvarande. Att växla mellan tjänstnivåerna Basic och Standard.
2.  Dynamisk på begäran ökningen av lagring i förväg skapade server stöds inte för närvarande.
3.  Minska lagringsstorleken för server stöds inte.

### <a name="server-version-upgrades"></a>Serveruppgraderingarna version:
- Automatisk migrering mellan större database engine versioner stöds inte för närvarande.

### <a name="subscription-management"></a>Prenumerationshantering av:
- Dynamiskt flytta förskapade servrar över prenumeration och resursgrupp stöds inte för närvarande.

### <a name="point-in-time-restore"></a>Punkt-i-tid-återställning:
1.  Är inte tillåtet att återställa till olika tjänstnivå och/eller Compute enheter och lagringsstorlek.
2.  Återställa en släppt server stöds inte.

## <a name="next-steps"></a>Nästa steg:
[Vad är tillgängliga i varje tjänstnivå](concepts-service-tiers.md)
[versioner som stöds av MySQL-databas](concepts-supported-versions.md)
