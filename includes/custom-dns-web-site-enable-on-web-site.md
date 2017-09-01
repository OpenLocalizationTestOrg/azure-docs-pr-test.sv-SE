När posterna för domännamnet har spridits måste du koppla dem till ditt webbprogram. Använd följande steg för att aktivera domännamn med hjälp av webbläsaren.

> [!NOTE]
> Det kan ta lite tid för TXT-poster som skapats i föregående steg och spridas via DNS-systemet. Du kan inte lägga till domännamnet för din webbapp tills TXT-posten har spridits. Om du använder en A-post kan du inte lägga till domännamnet för en post ditt webbprogram tills du skapade i föregående steg TXT-posten har spridits.
> 
> Du kan använda en tjänst som <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> att verifiera att TXT-posten är tillgänglig.
> 
> 

1. I webbläsaren och öppna den [Azure Portal](https://portal.azure.com).
2. I den **Web Apps** , klicka på namnet på ditt webbprogram, och välj sedan **anpassade domäner**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. I den **anpassade domäner** bladet, klickar du på **lägga till värdnamnet**.
4. Använd den **värdnamn** textrutor för att ange de domännamn som ska associeras med det här webbprogrammet.
   
    ![](./media/custom-dns-web-site/add-custom-domain.png)
5. Klicka på **Validera**.
6. När du klickar på **verifiera** Azure kommer startar domänverifiering arbetsflöde. Detta kommer att söka efter ägarskap för domänen som värdnamn tillgänglighet och rapporten slutfört eller detaljerade fel med normativ guidence om hur du åtgärdar felet.    

Nu ska du kunna ange det anpassade domännamnet i din webbläsare och se att det har tar dig till ditt webbprogram.

