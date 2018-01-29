---
title: Kvottyper i stacken Azure | Microsoft Docs
description: "Granska de olika kvoten typerna som är tillgängliga för tjänster och resurser i Azure-stacken."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 8/23/2017
ms.author: erikje
ms.openlocfilehash: d9bb048ece32bf5b34e05d7459488aa0f24d0d44
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/25/2017
---
# <a name="quota-types-in-azure-stack"></a>Kvottyper i Azure-stacken

*Gäller för: Azure Stack integrerat system och Azure-stacken Development Kit*

[Kvoter](azure-stack-plan-offer-quota-overview.md#plans) definiera gränserna för resurser som en användare-prenumeration kan etablera eller använda. En kvot kan exempelvis tillåta en användare att skapa upp till fem virtuella datorer. Varje resurs kan ha sin egen typer av kvoter.

## <a name="compute-quota-types"></a>Beräkna kvottyper
| **Typ** | **Standardvärde** | **Beskrivning** |
| --- | --- | --- |
| Max antal virtuella datorer | 20 | Maximalt antal virtuella datorer som en prenumeration kan skapa i den här platsen. |
| Max antal virtuella kärnor | 50 | Det högsta antalet kärnor som skapar en prenumeration på den här platsen (till exempel en A3 VM har fyra kärnor). |
| Max antal tillgänglighetsuppsättningar | 10 | Maximalt antal tillgänglighetsuppsättningar som kan skapas på den här platsen. |
| Anger högsta antal virtuella datorn | 20 | Det maximala antalet skalningsuppsättningar i virtuella datorer som kan skapas på den här platsen. |

> [!NOTE]
> Beräkna kvoter inte tillämpas i den här tekniska förhandsversionen.
> 
> 

## <a name="storage-quota-types"></a>Kvoten lagringstyper
| **Objektet** | **Standardvärde** | **Beskrivning** |
| --- | --- | --- |
| Maximal kapacitet (GB) |500 |Totalt antal lagringskapacitet som kan användas av en prenumeration på den här platsen. |
| Totalt antal storage-konton |20 |Maximalt antal lagringskonton som skapar en prenumeration på den här platsen. |

## <a name="network-quota-types"></a>Typer av nätverk kvot
| **Objektet** | **Standardvärde** | **Beskrivning** |
| --- | --- | --- |
| Maximalt antal offentliga IP-adresser |50 |Det maximala antalet offentliga IP-adresser som en prenumeration kan skapa i den här platsen. |
| Maximalt antal virtuella nätverk |50 |Maximalt antal virtuella nätverk som skapar en prenumeration på den här platsen. |
| Maximalt antal virtuella nätverks-gateway |1 |Maximalt antal virtuella nätverks-gateway (VPN-gateway) som skapar en prenumeration på den här platsen. |
| Max nätverksanslutningar |2 |Maximalt antal nätverksanslutningar (point-to-point eller plats-till-plats) som en prenumeration kan skapa över alla virtuella nätverks-gateway på den här platsen. |
| Max belastningsutjämnare |50 |Det maximala antalet belastningsutjämnare som skapar en prenumeration på den här platsen. |
| Maximalt antal nätverkskort |100 |Det maximala antalet nätverksgränssnitt som skapar en prenumeration på den här platsen. |
| Maximalt antal nätverkssäkerhetsgrupper |50 |Maximalt antal nätverkssäkerhetsgrupper som skapar en prenumeration på den här platsen. |

## <a name="view-an-existing-quota"></a>Visa en befintlig kvot
1. Klicka på **fler tjänster** > **Resursproviders**.
2. Välj tjänsten med den kvot som du vill visa.
3. Klicka på **kvoter**, och välj den kvot som du vill visa.

## <a name="next-steps"></a>Nästa steg
[Lär dig mer om planer, erbjudanden och kvoter.](azure-stack-plan-offer-quota-overview.md)

[Skapa kvoter medan du skapar en plan.](azure-stack-create-plan.md)
