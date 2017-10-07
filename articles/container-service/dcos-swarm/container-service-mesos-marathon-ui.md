---
title: "aaaManage Azure DC/OS-kluster med Marathon-Gränssnittet | Microsoft Docs"
description: "Distribuera behållare tooan Azure Container Service-klustertjänsten med hello Marathons webbgränssnitt."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: a90180e1b4763e6d2ddfa699ed4b7269f209f728
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-container-service-dcos-cluster-through-hello-marathon-web-ui"></a>Hantera en Azure Container Service DC/OS-klustret via hello Marathons webbgränssnitt
DC/OS erbjuder en miljö för att distribuera och skala klustrade arbetsbelastningar samtidigt abstrahera hello underliggande maskinvara. Utöver DC/OS finns det ett ramverk som hanterar schemaläggning och beräkning av arbetsbelastningar.

Även om ramverk är tillgängliga för många populära arbetsbelastningar beskriver det här dokumentet hur tooget igång med att distribuera behållare med Marathon. 


## <a name="prerequisites"></a>Krav
Innan du börjar med de här exemplen behöver du ett DC/OS-kluster som har konfigurerats i Azure Container Service. Du måste också toohave fjärranslutningar toothis klustret. Mer information om dessa objekt finns i hello följande artiklar:

* [Distribuera ett Azure Container Service-kluster](container-service-deployment.md)
* [Ansluta tooan Azure Container Service-kluster](../container-service-connect.md)

> [!NOTE]
> Den här artikeln förutsätter att du använder tunneltrafik toohello DC/OS-klustret via en lokal port 80.
>

## <a name="explore-hello-dcos-ui"></a>Utforska hello DC/OS-gränssnitt
Med en tunnel SSH (Secure Shell) [upprätta](../container-service-connect.md), bläddra toohttp://localhost/. Detta laddar hello DC/OS-webbgränssnittet och visar information om hello klustret, till exempel använda resurser, aktiva agenter och tjänster som körs.

![DC/OS-gränssnitt:](./media/container-service-mesos-marathon-ui/dcos2.png)

## <a name="explore-hello-marathon-ui"></a>Utforska hello Marathon-Gränssnittet
toosee hello Användargränssnittet för Marathon Bläddra toohttp://localhost/marathon. Från den här skärmbilden kan starta du en ny behållare eller ett annat program på hello Azure Container Service DC/OS-klustret. Du kan även se information om att köra behållare och program.  

![Gränssnittet i Marathon](./media/container-service-mesos-marathon-ui/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Distribuera en Docker-formaterad behållare
toodeploy en ny behållare med Marathon, klickar du på **skapa program**, och ange följande information i hello formuläret flikar hello:

| Fält | Värde |
| --- | --- |
| ID |nginx |
| Minne | 32 |
| Bild |nginx |
| Nätverk |Bryggad |
| Värdport |80 |
| Protokoll |TCP |

![Nytt programgränssnitt – allmänt](./media/container-service-mesos-marathon-ui/dcos4.png)

![Nytt programgränssnitt – Dockerbehållare](./media/container-service-mesos-marathon-ui/dcos5.png)

![Nytt programgränssnitt – portar och identifiering av tjänst](./media/container-service-mesos-marathon-ui/dcos6.png)

Om du vill toostatically mappa hello behållaren port tooa port på hello-agenten måste toouse JSON-läget. toodo så växla hello guiden för nya program för**JSON-läget** med hjälp av hello växla. Ange följande inställningen under hello hello `portMappings` avsnitt i hello programmets definition. Det här exemplet Binder port 80 på hello behållaren tooport 80 av hello DC/OS-agenten. När du har gjort den här ändringen kan du växla ur guiden ur JSON-läget.

```none
"hostPort": 80,
```

![Nytt programgränssnitt – port 80-exempel](./media/container-service-mesos-marathon-ui/dcos13.png)

Om du vill tooenable hälsokontroller, ange en sökväg på hello **hälsa kontrollerar** fliken.

![Nytt programanvändargränssnitt – hälsokontroller](./media/container-service-mesos-marathon-ui/dcos_healthcheck.png)

hello DC/OS-klustret distribueras med privata och offentliga agenter. För hello klustret toobe kan tooaccess program från hello Internet behöver du toodeploy hello program tooa offentlig agent. Välj toodo därför hello **valfritt** fliken hello nytt program guiden och ange **slave_public** för hello **accepterade resursroller**.

Klicka på **Skapa program**.

![Nytt programgränssnitt – inställning av offentlig agent](./media/container-service-mesos-marathon-ui/dcos14.png)

Tillbaka på hello huvudsidan för Marathon ser du hello Distributionsstatus för hello behållare. Inledningsvis visas statusen **Distribuerar**. Efter en lyckad distribution hello status ändras för**kör**.

![Marathon-huvudsidans gränssnitt 0 behållarens distributionsstatus](./media/container-service-mesos-marathon-ui/dcos7.png)

När du växlar tillbaka toohello DC/OS-webbgränssnittet (http://localhost/) ser du att en aktivitet (i det här fallet en Docker-formaterad behållare) körs på hello DC/OS-klustret.

![DC/OS-webbgränssnitt – aktivitet som körs på klustret hello](./media/container-service-mesos-marathon-ui/dcos8.png)

toosee hello nod i klustret som hello aktiviteten körs på, klicka på hello **noder** fliken.

![DC/OS-webbgränssnitt – klusternod](./media/container-service-mesos-marathon-ui/dcos9.png)

## <a name="reach-hello-container"></a>Nå hello behållare

I det här exemplet körs hello programmet på en offentlig agent-nod. Du når programmet hello från hello internet genom att bläddra toohello agent FQDN för hello kluster: `http://[DNSPREFIX]agents.[REGION].cloudapp.azure.com`, där:

* **DNSPREFIX** är hello DNS-prefix som du angav när du har distribuerat hello klustret.
* **REGION** hello region där resursgruppen finns.

    ![Nginx från Internet](./media/container-service-mesos-marathon-ui/nginx.png)


## <a name="next-steps"></a>Nästa steg
* [Arbeta med DC/OS och Marathon API hello](container-service-mesos-marathon-rest.md)

* Ingående om hello Azure Container Service med Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
