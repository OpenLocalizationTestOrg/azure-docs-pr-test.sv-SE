Du lägger till taggar tooyour Azure-resurser toologically organisera dem efter kategorier. Varje tagg består av ett namn och ett värde. Du kan till exempel använda hello namn ”miljö” och hello värdet ”produktion” tooall hello resurser i produktionen. Utan taggen kan det vara svårt att identifiera om en resurs är avsedd för utveckling, testning eller produktion. "Miljö" och "produktion" är bara ett exempel. Du kan definiera hello namn och värden som gör hello bäst för att ordna din prenumeration.

När du har tillämpat taggar kan du hämta alla hello resurser i din prenumeration med samma taggnamn och värde. Taggar aktivera du tooretrieve relaterade resurser som finns i olika resursgrupper. Den här metoden är användbar när du behöver tooorganize resurser för fakturerings- eller hantering.

hello följande begränsningar gäller tootags:

* Varje resurs eller resursgrupp kan innehålla upp till 15 taggnamn-/taggvärdepar. Den här begränsningen gäller endast tootags tillämpas direkt toohello resursgrupp eller resurs. En resursgrupp kan innehålla många resurser som var och en har 15 taggnamn-/taggvärdepar. 
* hello taggnamn är begränsad too512 tecken och hello Taggvärdet är begränsad too256 tecken. Hello taggnamn är begränsad too128 tecken för storage-konton och hello Taggvärdet är begränsad too256 tecken.
* Märkningar toohello resursgruppen ärvs inte av hello resurser i resursgruppen. 

Om du har mer än 15 värden som du behöver tooassociate med en resurs kan du använda en JSON-sträng för hello taggvärde. hello JSON-strängen kan innehålla flera värden som är kopplade tooa enda taggnamn. Den här artikeln visar ett exempel för att tilldela en JSON-strängen toohello tagg.
