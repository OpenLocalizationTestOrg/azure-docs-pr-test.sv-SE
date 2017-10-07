---
title: "aaaExample Azure Infrastructure genomgången | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för att distribuera en exempel-infrastruktur i Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7032b586-e4e5-4954-952f-fdfc03fc1980
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd6b6e904404bea0b5be37dfce6d60039daae636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-azure-infrastructure-walkthrough-for-windows-vms"></a>Exemplet Azure-infrastrukturen genomgång för virtuella Windows-datorer

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln beskriver hur bygga ut infrastruktur för ett exempel. Vi i detalj utformar en infrastruktur för en enkel onlinebutik som sammanför alla hello riktlinjer och beslut runt namngivningskonventioner, tillgänglighetsuppsättningar, virtuella nätverk och belastningsutjämnare och faktiskt distribuerar virtuella datorer (VM).

## <a name="example-workload"></a>Exempel arbetsbelastning
Adventure Works Cycles vill toobuild en onlinebutik program i Azure som består av:

* Två IIS-servrar som kör hello klienten klientdelen i en webbnivå
* Två IIS-servrar bearbetar data och order i en program-nivå
* Två Microsoft SQL Server-instanser med AlwaysOn-Tillgänglighetsgrupper (två SQL-servrar och en majoritet nod vittne) för att lagra produktdata och order i en databasnivå
* Två Active Directory-domänkontrollanter för kundkonton och leverantörer i en nivå för autentisering
* Alla hello-servrar finns i två undernät:
  * en frontend-undernät för hello webbservrar 
  * en backend-undernät för hello programservrar, SQL-kluster och domänkontrollanter

![Diagram över olika nivåer för infrastruktur](./media/infrastructure-example/example-tiers.png)

Inkommande säker webbtrafik måste vara belastningsutjämnad bland hello webbservrar som kunder Bläddra hello onlinebutik. Ordna bearbetning trafik i hello form av HTTP-begäranden från hello web servrar måste balanseras mellan hello programservrar. Dessutom måste hello infrastrukturen utformas för hög tillgänglighet.

resulterande hello-design måste innehålla:

* Ett konto och Azure-prenumeration
* En enskild resursgrupp
* Azure Managed Disks
* Ett virtuellt nätverk med två undernät
* Tillgänglighetsuppsättningar för hello virtuella datorer med en liknande roll
* Virtuella datorer

Alla hello ovan följer dessa namngivningsregler:

* Adventure Works Cycles använder **[IT arbetsbelastning]-[plats]-[Azure resurs]** som ett prefix
  * I det här exemplet ”**azos**” (Azure online Store) är hello arbetsbelastning namn och ”**använder**” (östra USA 2) är hello plats
* Virtuella nätverk använder AZOS-Använd-VN**[antal]**
* Tillgänglighetsuppsättningar använder azos-Använd-som-**[roll]**
* Namn på virtuella datorer använda azos-Använd-vm -**[vmname]**

## <a name="azure-subscriptions-and-accounts"></a>Azure-prenumerationer och konton
Adventure Works Cycles använder sin Enterprise-prenumeration med namnet Adventure Works Enterprise prenumeration, tooprovide fakturering för den här IT-arbetsbelastning.

## <a name="storage"></a>Lagring
Adventure Works Cycles fastställt att de ska använda Azure hanterade diskar. När du skapar virtuella datorer används både tillgängligt lagringsutrymme lagringsnivåer:

* **Standardlagring** för hello webbservrar, programservrar och domänkontrollanter och deras datadiskar.
* **Premium-lagring** för hello SQL Server-datorer och deras datadiskar.

## <a name="virtual-network-and-subnets"></a>Virtuellt nätverk och undernät
Eftersom hello virtuellt nätverk inte behöver pågående anslutning toohello Adventure arbete Cycles lokalt nätverk valt de endast molnbaserad virtuella nätverk.

De skapade ett virtuellt nätverk som endast molnbaserad med hello följande inställningar med hjälp av hello Azure-portalen:

* Namn: AZOS-Använd-VN01
* Plats: Östra USA 2
* Virtuellt nätverks-adressutrymme: 10.0.0.0/8
* Första undernätet:
  * Namn: klientdel
  * Adressutrymmet: 10.0.1.0/24
* Andra undernät:
  * Namn: BackEnd
  * Adressutrymmet: 10.0.2.0/24

## <a name="availability-sets"></a>Tillgänglighetsuppsättningar
toomaintain hög tillgänglighet för alla fyra nivåer av deras onlinebutik Adventure Works Cycles bestämt på fyra tillgänglighetsuppsättningar:

* **azos används som web** för hello webbservrar
* **azos används som appen** för hello programservrar
* **azos Använd som sql** för hello SQL-servrar
* **azos används som dc** hello-domänkontrollanter

## <a name="virtual-machines"></a>Virtuella datorer
Adventure Works Cycles valt hello följande namn för sina virtuella Azure-datorer:

* **azos-användning-vm-web01** för hello första webbserver
* **azos-användning-vm-web02** för hello andra webbservern
* **azos-användning-vm-app01** för hello första programserver
* **azos-användning-vm-app02** för hello andra programserver
* **azos-användning-vm-sql01** för hello första SQL Server-server i hello klustret
* **azos-användning-vm-sql02** för hello andra SQL Server-server i hello klustret
* **azos-användning-vm-dc01** för hello första domänkontrollant
* **azos-användning-vm-dc02** för hello andra domänkontrollanten

Här är hello resulterande konfigurationen.

![Sista infrastruktur för distribuerade i Azure](./media/infrastructure-example/example-config.png)

Den här konfigurationen omfattar:

* Endast molnbaserad virtuellt nätverk med två undernät (klient- och Servergränssnitten)
* Azure-hanterade diskar med både Standard- och Premium-diskar
* Fyra tillgänglighetsuppsättningar, ett för varje nivå av hello onlinebutik
* hello virtuella datorer för hello fyra nivåer
* En extern belastningsutjämnad uppsättning för HTTPS-baserade webbtrafik från hello Internet toohello webbservrar
* Ett internt belastningsutjämnade uppsättningen okrypterad webbtrafik från hello servrar toohello webbprogramservrar
* En enskild resursgrupp