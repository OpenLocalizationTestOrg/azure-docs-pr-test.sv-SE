När hello posterna för domännamnet har spridits bör du vara kan toouse din webbläsare tooverify som ditt domännamn kan vara används tooaccess webbappen i Azure App Service.

> [!NOTE]
> Det kan ta lite tid för din CNAME toopropagate via hello DNS-systemet. Du kan använda en tjänst som <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> tooverify som hello CNAME är tillgänglig.
> 
> 

Om du inte redan har lagts ditt webbprogram som en Traffic Manager-slutpunkt måste du göra detta innan namnmatchning fungerar som hello domänen namnet vägar tooTraffic Manager. Traffic Manager dirigerar sedan tooyour webbprogram. Använd hello information i [Lägg till eller ta bort slutpunkter](../articles/traffic-manager/traffic-manager-endpoints.md) tooadd ditt webbprogram som en slutpunkt i Traffic Manager-profilen.

> [!NOTE]
> Om webbappen inte visas när du lägger till en slutpunkt, kontrollera att den är konfigurerad för **Standard** läge för App Service-plan. Du måste använda **Standard** läge för webbappen i ordning toowork med Traffic Manager.
> 
> 

1. Öppna i din webbläsare hello [Azure Portal](https://portal.azure.com).
2. I hello **Web Apps** klickar du på hello namnet på ditt webbprogram, Välj **inställningar**, och välj sedan **anpassade domäner**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. I hello **anpassade domäner** bladet, klickar du på **lägga till värdnamnet**.
4. Använd hello **värdnamn** text rutorna tooenter hello Traffic Manager domain name tooassociate med det här webbprogrammet.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Klicka på **verifiera** toosave hello domänkonfigurationen namn.
6. När du klickar på **verifiera** Azure kommer startar domänverifiering arbetsflöde. Detta kommer att söka efter ägarskap för domänen som värdnamn tillgänglighet och rapporten slutfört eller detaljerade fel med normativ guidence på hur toofix hello fel.    
7. När valideringen har lyckats **lägga till värdnamnet** knappen ska aktiveras och du kommer att kunna toohello tilldela värdnamn. Gå nu tooyour domännamn i en webbläsare. Du bör nu se din app körs med ditt domännamn. 
   
   När konfigurationen är klar visas hello domännamn i hello **domännamn** avsnitt av ditt webbprogram.

Du bör nu vara kan tooenter hello Traffic Manager domännamn/användarnamn i webbläsaren och se att det har tar du tooyour webbprogram.

