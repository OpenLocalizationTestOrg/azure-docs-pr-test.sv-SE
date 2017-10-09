---
title: "aaaUse Virtuella Azure-agenter för kontinuerlig integrering med Jenkins."
description: Azure VM-agenter som Jenkins slaves.
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 2388e6919d0280372166fbd325d80dafb00d7550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Använda virtuella Azure-agenter för kontinuerlig integrering med Jenkins.

Den här snabbstarten visar hur hello toouse Jenkins Azure VM agenter plugin-programmet toocreate en agent för Linux (Ubuntu) av på begäran i Azure.

## <a name="prerequisites"></a>Krav

toocomplete denna Snabbstart:

* Om du inte redan har en Jenkins master, du kan starta med hello [Lösningsmall](install-jenkins-solution-template.md) 
* Se för[skapa ett Azure-tjänstens huvudnamn med Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) om du inte redan har en Azure-tjänstens huvudnamn.

## <a name="install-azure-vm-agents-plugin"></a>Installera Plugin-program för Azure VM-agenter

Om du startar från hello [Lösningsmall](install-jenkins-solution-template.md), hello Azure VM-agenten plugin-programmet är installerat i hello Jenkins master.

I annat fall installerar hello **Azure VM agenter** plugin-programmet från inom hello Jenkins instrumentpanelen.

## <a name="configure-hello-plugin"></a>Konfigurera hello plugin-program

* Inom hello Jenkins instrumentpanelen, klickar du på **Jenkins hantera -> Konfigurera System ->**. Rulla toohello längst ned på sidan för hello och hitta hello avsnitt med hello nedrullningsbara **lägga till nya molntjänster**. Hello-menyn, Välj **Microsoft Azure VM agenter**
* Välj ett befintligt konto hello Azure-autentiseringsuppgifterna listrutan.  tooadd en ny **Microsoft Azure Service Principal** ange hello följande värden: prenumerations-ID, klient-ID, Klienthemligheten och OAuth 2.0-Token för slutpunkt.

![Azure-autentiseringsuppgifter](./media/jenkins-azure-vm-agents/service-principal.png)

* Klicka på **verifiera konfigurationen** toomake att hello profilkonfigurationen är korrekt.
* Spara hello konfigurationen och fortsätta toohello nästa steg.

## <a name="template-configuration"></a>Mallkonfiguration

### <a name="general-configuration"></a>Allmän konfiguration
Konfigurera en mall för Använd toodefine Virtuella Azure-agenten. 

* Klicka på **Lägg till** tooadd en mall. 
* Ge din nya mall ett nytt namn. 
* Ange hello etiketten ”ubuntu”. Den här etiketten används under konfigurationen av hello jobb.
* Välj önskad region för hello hello kombinationsruta.
* Välj hello önskade VM-storlek.
* Ange kontonamn för hello Azure Storage eller lämna den tom toouse hello standardnamnet ”jenkinsarmst”.
* Ange hello kvarhållningstiden i minuter. Den här inställningen definierar hello antal minuter som Jenkins kan vänta innan du tar bort en inaktiv agent automatiskt. Ange 0 om du inte vill att inaktiva agenter toobe tas bort automatiskt.

![Allmän konfiguration](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Bildkonfiguration

toocreate linuxagenten (Ubuntu), Välj **bild referens** och Använd hello efter konfigurationen som exempel. Se för[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) för hello senaste Azure stöds bilder.

* Bildutgivare: Canonical
* Bild-erbjudande: UbuntuServer
* Bild-Sku: 14.04.5-LTS
* Bildversion: senaste
* OS-typ: Linux
* Startmetod: SSH
* Uppge administratörsautentiseringsuppgifter
* Ange följande för initieringsskriptet för virtuell dator:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Bildkonfiguration](./media/jenkins-azure-vm-agents/image-config.png)

* Klicka på **Kontrollera mallen** tooverify hello konfiguration.
* Klicka på **Spara**.

## <a name="create-a-job-in-jenkins"></a>Skapa ett jobb i Jenkins

* Inom hello Jenkins instrumentpanelen, klickar du på **nytt objekt**. 
* Ange ett namn och välj **Freestyle-projekt** och sedan på **OK**.
* I hello **allmänna** fliken, markerar ”begränsa där du kan köra project” och typ ”ubuntu” i etikettuttrycket. Nu kan du se ”ubuntu” i hello listrutan.
* Klicka på **Spara**.

![Ställ in jobb](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Skapa det nya projektet

* Gå tillbaka toohello Jenkins instrumentpanelen.
* Högerklicka på hello nytt jobb som du har skapat, klicka på **skapa nu**. En version har startats. 
* När hello build är klar, går för**konsolen utdata**. Du ser att hello build utfördes via fjärranslutning på Azure.

![Konsolutdata](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Referens

* Azure Friday-video: [kontinuerlig integration med Jenkins med virtuella Azure-agenter](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Supportalternativ och konfigurationsinformation: [Azure VM-agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 

