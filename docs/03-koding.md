
# Koding av kategoriske variabler
I tabellen på s. kan du se at vi har en tabell med tre kolonner: en kolonne for hver variabel vi har i vårt datasett. Variabelen **gruppe** er en kategorisk vaiabel som har to ulike verdier: "ett.sett" og "tre.sett". Dette er de to gruppene som vi skal teste om er forskjellige. I programmeringsverdenen kalles disse denne typen data for et tekstobjekt, "strings" (python/javascript) eller "characters" (R). På norsk kalles disse verdiene for ord. Uansett navn er problemet at vi ikke kan putte ord inn i en statistisk modell; vi er nødt til å representere denne kategoriske vaiabelen med tallverdier. Det er flere måter å gjøre dette på, men de forskjellige måtene gir ulik resultat. Derfor må vi vie en god del tid på dette. Vi går gjennom to måter å gjøre dette på.

## Dummykoding
En vanlig metode kalles **dummykoding** eller **treatment-koding**. Den går ut på å lage en eller flere variabler med 0 og 1 som de to mulige verdiene. Antall variabler vi trenger avhenger av antall grupper vi vil sammenligne. Siden vårt datasett kun inneholder to grupper, så trenger vi kun en variabel. Vi kan den ene gruppen og den andre 1. Hovedregelen er at vi gir 0 til baselinegruppe og 1 til den eksperimentelle gruppen. Vi gir derfor 0 til 1.sett-gruppen og 1 til 3.sett-gruppen. Gjør dette før du går videre.



I R og Jamovi kan du gjøre det med følgende if/else statement. I R kan du bruke følgende kode:


```r
#lager et nytt objekt som heter dummykodet.dat
dummykodet.dat <- dat %>%
  # her lager jeg en ny kolonne som heter dummykoder. If gruppe == 'ett.sett', gi verdien 0, else gi de 1.
  mutate(dummykodet = if_else(gruppe == "ett.sett", 0, 1))
```
I jamovi ville jeg sett følgende video: https://www.youtube.com/watch?v=iITxK27LfZk

Table: (\#tab:unnamed-chunk-4)Dummy koding

| individ|gruppe   |     rm| dummykodet|
|-------:|:--------|------:|----------:|
|       1|tre.sett | 40.467|          1|
|       2|tre.sett | 49.072|          1|
|       3|tre.sett | 47.941|          1|
|       4|tre.sett | 44.514|          1|
|       5|tre.sett | 52.288|          1|
|       6|tre.sett | 40.018|          1|
|       7|tre.sett | 49.484|          1|
|       8|tre.sett | 29.210|          1|
|       9|tre.sett | 40.593|          1|
|      10|tre.sett | 37.587|          1|
|      11|tre.sett | 35.427|          1|
|      12|tre.sett | 42.494|          1|
|      13|ett.sett | 17.706|          0|
|      14|ett.sett | 17.072|          0|
|      15|ett.sett | 18.268|          0|
|      16|ett.sett | 25.426|          0|
|      17|ett.sett | 32.703|          0|
|      18|ett.sett | 19.102|          0|
|      19|ett.sett | 22.238|          0|
|      20|ett.sett | 22.271|          0|
|      21|ett.sett | 26.179|          0|
|      22|ett.sett | 20.349|          0|
|      23|ett.sett | 23.528|          0|
|      24|ett.sett | 17.960|          0|
## Kontrastkoding
Kontrastkoding er et alternativ til dummykoding. Det er en regel som er viktig å følge for å ha en gyldig kontrastkode, og det er at summen av kontrastkodene blir 0. For eksempel er -0.5 og 0.5 gyldige kontrastkoder fordi summen av disse blir 0. Det samme er -10 og +10. 0 og 1 derimot, slik vi har med en dummykodet variabel, er ikke er en gyldig kontrastkode fordi summen av disse blir 1. **Hvilke verdier vi velger å bruke på vår kontrastkodede variabel betyr ingenting for den statistiske test vi gjennomfører, men gjør at vi må fortolke resultatene litt forskjellig**. Med en kontrastkode på +10 og -10 er det en 20 enhets forskjell, mens det ved +0.5 og -0.5 kun er <input class='solveme nospaces' size='1' data-answer='["1"]'/> enhet forskjell. 



```r
#lager et nytt objekt som heter dummykodet.dat
kontrastkodet.dat <- dummykodet.dat %>%
  # her lager jeg en ny kolonne som heter kontrastkodet. If gruppe == 'ett.sett', gi verdien -0.5, else gi de +0.5
  mutate(kontrastkodet = if_else(gruppe == "ett.sett", -0.5, +0.5)
         )
```


Table: (\#tab:unnamed-chunk-6)Kontrastkoding

| individ|gruppe   |     rm| dummykodet| kontrastkodet|
|-------:|:--------|------:|----------:|-------------:|
|       1|tre.sett | 40.467|          1|           0.5|
|       2|tre.sett | 49.072|          1|           0.5|
|       3|tre.sett | 47.941|          1|           0.5|
|       4|tre.sett | 44.514|          1|           0.5|
|       5|tre.sett | 52.288|          1|           0.5|
|       6|tre.sett | 40.018|          1|           0.5|
|       7|tre.sett | 49.484|          1|           0.5|
|       8|tre.sett | 29.210|          1|           0.5|
|       9|tre.sett | 40.593|          1|           0.5|
|      10|tre.sett | 37.587|          1|           0.5|
|      11|tre.sett | 35.427|          1|           0.5|
|      12|tre.sett | 42.494|          1|           0.5|
|      13|ett.sett | 17.706|          0|          -0.5|
|      14|ett.sett | 17.072|          0|          -0.5|
|      15|ett.sett | 18.268|          0|          -0.5|
|      16|ett.sett | 25.426|          0|          -0.5|
|      17|ett.sett | 32.703|          0|          -0.5|
|      18|ett.sett | 19.102|          0|          -0.5|
|      19|ett.sett | 22.238|          0|          -0.5|
|      20|ett.sett | 22.271|          0|          -0.5|
|      21|ett.sett | 26.179|          0|          -0.5|
|      22|ett.sett | 20.349|          0|          -0.5|
|      23|ett.sett | 23.528|          0|          -0.5|
|      24|ett.sett | 17.960|          0|          -0.5|
Spørsmålet dere sikkert lurer på er hvorfor vi dummykoder og kontrastkoder gruppe-variabelen vår. Det korte svaret er at vo gjør det fordi vi skal se at disse to måtene å kode på produserer forskjellige svar. 




