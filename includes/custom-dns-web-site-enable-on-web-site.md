Du måste koppla dem till ditt webbprogram när hello posterna för domännamnet har spridits. Använd hello följande steg tooenable hello domännamn med hjälp av webbläsaren.

> [!NOTE]
> Det kan ta lite tid för TXT-poster som skapats i hello föregående steg toopropagate via hello DNS-systemet. Du kan inte lägga till hello domännamnet för tooyour webbprogrammet förrän hello TXT-posten har spridits. Om du använder en A-post, kan du inte lägga till hello ett webbprogram för poster domain name tooyour tills du skapade i föregående steg i hello hello TXT-posten har spridits.
> 
> Du kan använda en tjänst som <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> tooverify som hello TXT-posten är tillgänglig.
> 
> 

1. Öppna i din webbläsare hello [Azure Portal](https://portal.azure.com).
2. I hello **Web Apps** , klicka hello namnet på ditt webbprogram, och välj sedan **anpassade domäner**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. I hello **anpassade domäner** bladet, klickar du på **lägga till värdnamnet**.
4. Använd hello **värdnamn** text rutorna tooenter hello domän namn tooassociate med det här webbprogrammet.
   
    ![](./media/custom-dns-web-site/add-custom-domain.png)
5. Klicka på **Validera**.
6. När du klickar på **verifiera** Azure kommer startar domänverifiering arbetsflöde. Detta kommer att söka efter ägarskap för domänen som värdnamn tillgänglighet och rapporten slutfört eller detaljerade fel med normativ guidence på hur toofix hello fel.    

Du bör nu vara hello kan tooenter domännamn i webbläsaren och se att det har tar du tooyour webbprogram.

