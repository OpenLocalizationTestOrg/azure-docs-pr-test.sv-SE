Varje blobb i Azure-lagring måste befinna sig i en behållare. hello behållaren utgör en del av blobbnamnet hello. Till exempel `mycontainer` är hello namnet på hello-behållaren i de här exempelblobb URI: er:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Ett behållarnamn måste vara ett giltigt DNS-namn, förväntad toohello följande namngivningsregler:

1. Behållarnamn måste inledas med en bokstav eller siffra och får bara innehålla bokstäver, siffror, och hello streck (-).
2. Varje bindestreck (-) måste föregås och följas av en bokstav eller siffra. Flera bindestreck i följd är inte tillåtna i behållarnamn.
3. Alla bokstäver i ett behållarnamn måste vara gemener.
4. Behållarnamn måste vara mellan 3 och 63 tecken långa.

> [!IMPORTANT]
> Observera att hello namnet på en behållare måste alltid vara gemener. Om du inkluderar en versal i ett behållarnamn, eller något annat sätt kränka hello behållaren namngivningsregler, kan det hända att ett 400-fel (felaktig begäran). 
> 
> 

