---
title: aaaConfigure autentisering med Amazon Web Services | Microsoft Docs
description: "Den här artikeln beskriver hur toocreate och validera en AWS-autentiseringsuppgift för runbooks i Azure Automation hantera AWS-resurser."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: aws-autentisering, konfigurera aws
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: magoedte
ms.openlocfilehash: 6edaa000c1b206d80fe64b18c729dac124849070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Autentisera runbooks med Amazon Web Services (AWS)
Du kan automatisera vanliga uppgifter med resurser i Amazon Web Services (AWS) med hjälp av Automation-runbooks i Azure.  Du kan automatisera många aktiviteter i AWS med hjälp av Automation-runbooks precis som du kan göra med resurser i Azure.  Du behöver bara två saker:

* En AWS-prenumeration och en uppsättning autentiseringsuppgifter.  Mer specifikt din åtkomstnyckel  och hemliga nyckel för AWS.  Mer information finns i hello artikel [med hjälp av autentiseringsuppgifter för AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* En Azure-prenumeration och ett Automation-konto.  Mer information om hur du skapar ett Azure Automation-konto Läs hello artikel [konfigurera Azure kör som-konto](automation-sec-configure-azure-runas-account.md).  

tooauthenticate med AWS, du måste ange en uppsättning AWS autentiseringsuppgifter tooauthenticate dina runbooks som körs från Azure Automation. Om du redan har ett Automation-konto som har skapats och du vill toouse som tooauthenticate med AWS, kan du följa hello stegen i följande avsnitt hello.  Om du vill toodedicated ett konto för runbooks targetting AWS resurser måste du först skapa en ny [Automation kör som-konto](automation-sec-configure-azure-runas-account.md) (hoppa över hello alternativet toocreate ett huvudnamn för tjänsten) och följ sedan hello stegen nedan.

## <a name="configure-automation-account"></a>Konfigurera ett Automation-konto
För Azure Automation toocommunicate med AWS, ska du först behöver tooretrieve AWS-autentiseringsuppgifter och lagra dem som resurser i Azure Automation.  Utföra hello följa stegen i hello AWS dokumentet [hantera Åtkomstnycklarna för ditt konto AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) toocreate en åtkomstnyckel och kopiera hello **åtkomst nyckel-ID** och **hemlighet åtkomstnyckel** (du kan också hämta nyckelfilen-toostore den någonstans säker).

När du har skapat och kopiera dina nycklar för AWS-säkerhet, måste toocreate en autentiseringstillgång med en Azure Automation-konto toosecurely lagrar dem och hänvisa dem med dina runbooks.  Hjälp hello åtgärderna i hello **att skapa en ny autentiseringstillgång** i hello [tillgångar i Azure Automation-autentiseringen](automation-credentials.md) artikel och ange hello följande information:

1. I hello **namn** ange **AWScred** eller ett lämpligt värde efter din namngivning standarder.  
2. I hello **användarnamn** skriver din **åtkomst-ID** och **hemlighet åtkomstnyckeln** i hello **lösenord** och **bekräfta lösenordet** rutan.   

## <a name="next-steps"></a>Nästa steg
* Reivew hello lösning artikel [automatisera distributionen av en virtuell dator i Amazon Web Services](automation-scenario-aws-deployment.md) toolearn hur toocreate runbooks tooautomate uppgifter i AWS.

