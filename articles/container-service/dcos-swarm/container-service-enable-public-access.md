---
title: "aaaEnable access tooAzure DC/OS-behållaren appen | Microsoft Docs"
description: "Hur tooenable offentliga åt tooDC/OS-behållare i Azure Container Service."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 1ba251ba5a176a6a5e1c7831655164e380a62b27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-public-access-tooan-azure-container-service-application"></a>Aktivera allmän åtkomst tooan Azure Container Service-program
Alla DC/OS-behållare i hello ACS [offentlig agent poolen](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) är automatiskt synliga toohello internet. Som standard portarna **80**, **443**, **8080** öppnas, och (offentlig)-behållare som lyssnar på de portarna som är tillgängliga. Den här artikeln visar hur tooopen fler portar för dina program i Azure Container Service.

## <a name="open-a-port-portal"></a>Öppna en port (portal)
Först måste vi vill tooopen hello-port.

1. Logga in toohello portal.
2. Hitta hello resursgrupp som du har distribuerat hello Azure Container Service till.
3. Välj hello-agentens belastningsutjämnare (som heter liknande för**XXXX-agent-lb-XXXX**).
   
    ![Azure container service-belastningsutjämnare](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Klicka på **avsökningar** och sedan **lägga till**.
   
    ![Azure container service-belastningsutjämnaren avsökningar](./media/container-service-enable-public-access/add-probe.png)
5. Fyll i hello avsökningen formuläret och klicka på **OK**.
   
   | Fält | Beskrivning |
   | --- | --- |
   | Namn |Ett beskrivande namn på hello avsökning. |
   | Port |hello behållaren tootest hello-port. |
   | Sökväg |(När i HTTP-läge) hello relativa webbplats sökvägen tooprobe. HTTPS stöds inte. |
   | intervall |hello tidslängd mellan avsökningen försök i sekunder. |
   | Tröskelvärde för ohälsosamt värde |Antal på varandra följande avsökningen försöker innan hello behållaren feltillstånd. |
6. Tillbaka hello egenskaper för hello-agentens belastningsutjämnare, klickar du på **belastningsutjämningsregler** och sedan **Lägg till**.
   
    ![Azure container service regler för inläsning av belastningsutjämnare](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Fyll i formuläret om hello belastningen belastningsutjämnaren och klicka på **OK**.
   
   | Fält | Beskrivning |
   | --- | --- |
   | Namn |Ett beskrivande namn på hello belastningsutjämnaren. |
   | Port |hello offentliga inkommande port. |
   | Backend-port |hello interna offentlig port av hello behållaren tooroute trafik till. |
   | Serverdelspool |hello behållare i den här poolen kommer att hello mål för den här belastningsutjämnaren. |
   | Avsökningen |Hej avsökningen används toodetermine om ett mål i hello **serverdelspool** är felfri. |
   | Persistence för session |Anger hur trafik från en klient ska hanteras för hello varaktighet hello-sessionen.<br><br>**Ingen**: efterföljande förfrågningar från samma klient kan hanteras av alla behållare hello.<br>**Klienten IP**: efterföljande förfrågningar från hello samma klient-IP hanteras av hello samma behållare.<br>**Klienten IP och protokoll**: efterföljande förfrågningar från samma klient-IP- och protokollkombination hanteras av hello hello samma behållare. |
   | Tidsgränsen för inaktivitet |(TCP) I minuter, hello tid tookeep en TCP/HTTP-klient öppna utan *keep-alive* meddelanden. |

## <a name="add-a-security-rule-portal"></a>Lägg till en säkerhetsregel (portal)
Därefter måste tooadd en anslutningssäkerhetsregel som dirigerar trafik från våra öppna port hello-brandväggen.

1. Logga in toohello portal.
2. Hitta hello resursgrupp som du har distribuerat hello Azure Container Service till.
3. Välj hello **offentliga** agent nätverkssäkerhetsgruppen (som heter liknande för**XXXX-agent-offentliga-nsg-XXXX**).
   
    ![Azure container service-nätverkssäkerhetsgrupp](./media/container-service-enable-public-access/agent-nsg.png)
4. Välj **inkommande säkerhetsregler** och sedan **Lägg till**.
   
    ![Azure container service regler för nätverkssäkerhetsgrupper](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Fyll i hello brandväggen regeln tooallow den offentliga porten och klicka på **OK**.
   
   | Fält | Beskrivning |
   | --- | --- |
   | Namn |Ett beskrivande namn på hello brandväggsregel. |
   | Prioritet |Prioritet rangordningen för hello regeln. hello lägre hello nummer hello högre hello prioritet. |
   | Källa |Begränsa hello inkommande IP-adressintervallet toobe tillåts eller nekas av den här regeln. Använd **alla** toonot ange en begränsning. |
   | Tjänst |Välj en uppsättning fördefinierade tjänster som denna regel gäller. Använd annars **anpassad** toocreate egna. |
   | Protokoll |Begränsa trafik utifrån **TCP** eller **UDP**. Använd **alla** toonot ange en begränsning. |
   | Portintervall |När **Service** är **anpassade**, anger hello portintervall som påverkas av regeln. Du kan använda en enskild port, till exempel **80**, eller ett intervall som **1024 1500**. |
   | Åtgärd |Tillåt eller neka trafik som uppfyller hello villkor. |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om hello skillnaden mellan [offentliga och privata DC/OS-agenterna](container-service-dcos-agents.md).

Läs mer om [hantera DC/OS-behållare](container-service-mesos-marathon-ui.md).

