	Excel file a számításokkal: https://docs.google.com/spreadsheets/d/1Z4R3-6jwl1uDZT865t_YVpSVFB3wFbcu7YvxOZjA5Lw/edit#gid=0


![[Pasted image 20221025113505.png]]

## Működés
t_be ideig kapcsoló zár: L árama lineárisan nő, U_C nő, D1 dióda zárt
t_ki ideig kapcsoló nyitva: D1 dióda nyit, rajta keresztül L árama lineárisan csökken, U_C csökken

## Tekercsre vonatkozó számítások
A következő alapegyenletből következően ki lehet számítani az áramok változását:

$$U_l = \frac{di}{dt}L$$
t_be ideig: 

$$\Delta i_{L} = \frac{U_l \times t_{be}}{L} = \frac{(U_{be} - U{ki}) \times t_{be}}{L}$$
t_ki ideig: 

$$\Delta i_{L} = \frac{U_l \times t_{ki}}{L} = \frac{(U_{ki} + U{D}) \times t_{ki}}{L} \approx \frac{U_{ki} \times t_{ki}}{L}$$
Így a következő a tekercs áramának időfüggvénye:

![[Pasted image 20221025113525.png]]

Ha a kettőt kiegyenlítjük egymással, akkor megkapjuk a bekapcsolási időt:

$$  \frac{(U_{be} - U{ki}) \times t_{be}}{L} = \frac{U_{ki} \times t_{ki}}{L} = \frac{U_{ki} \times (T-t_{be})}{L}$$
Innen:

$$ \delta = \frac{t_{be}}{T} = \frac{U_{ki}}{U_{be}}$$

Hogy folytonos módban maradjunk a $\Delta i_L$-nek nem szabad elérni az $I_{ki}$ értékét, ebből kiszámítható egy minimum induktivitás:

$$ L_{min} = \frac{(U_{be} - U{ki}) \times t_{be}}{I_{ki}} = \frac{U_{ki} \times t_{ki}}{I_{ki}}$$

## Kondenzátorra vonatkozó számítások
$$ I_C(t) = I_L(t) - I_{ki} $$
Ha ez az áram pozitív akkor a kondenzátor töltődik, ha negatív akkor ürül. Így a kondenzátor feszültségének is van egy hullámzása. A következő képpen határoztuk meg ennek az értékét:

t = $0$-ban a kondenzátor áramának értéke $-\Delta i_L$
t = $t_{be}$-ben $+\Delta i_L$
t = $T$-ben pedig újra $-\Delta i_L$

Mivel a növekedés lineáris és szimmetrikus tartományon történik, ezért az áram t = $\frac{t_{be}}{2}$ és $t_{be}+\frac{t_{ki}}{2}$ időpontokban nulla.
Ugyanezekben a pontokban éri el a hullámzásának a minimum és a maximum értékét.
t = $\frac{t_{be}}{2}$-ben $U_c = U_{ki} - \Delta U_C$ 
t = $\frac{t_{be}}{2}$-ben pedig $U_c = U_{ki} + \Delta U_C$
Ez a két időpont között $$\Delta Q = \frac{ \Delta i_L \times (\frac{t_be}{2} + \frac{t_ki}{2})}{2} = \frac{ \Delta i_L \times T}{4} = \frac{ \Delta i_L}{4f}$$
A két időpont között felírható a következő egyenlet a töltés-feszültség-kapacitás kapcsolatával:
$$ -\Delta U_C + \frac{\Delta Q}{C} = +\Delta U_C$$

Így kaphatunk egy minimum értéket a kapacitásra, hiszen ha nagyobb a kapacitás akkor ugyanakkora töltéstől kevesebb feszültségre töltődik, így kisebb lesz a feszültségének hullámzása is:
$$ C_{min} = \frac{\Delta Q}{2 \Delta U_C} = \frac{\Delta i_L}{8f \Delta U_C} = \frac{\Delta i_L}{4f U_{pp}}$$

A képlet valószínűleg jó, hiszen egy [Texas Instruments méretezési utasításban](https://www.ti.com/lit/an/slva477b/slva477b.pdf?ts=1666618206561&ref_url=https%253A%252F%252Fwww.google.com%252F)  is ez szerepel.

Ezt a képletet kicsit átalakítva megkapjuk hogy adott $C$ érték mellett mekkora lesz a feszültség ingadozás: $$U_{Cpp} = \frac{\Delta i_L}{4fC}$$
## Szabályozó IC-hez tartozó számítások
Az űrtechnikában elterjedt TI UCCx8C4x családból származó IC-t fogunk használni a szabályozáshoz a következő elrendezésben:

A tápegység rajza:
![[Pasted image 20221108184259.png]]

A szabályozási kör rajza:
![[Pasted image 20221108184234.png]]

### Röviden az IC működése:
A kapcsoló egy SR Latch kimenetére van kapcsolva, aminek az S lábára időnként egy oszcillátor egy pulzust küld, ez felel azért hogy a kapcsoló zárjon. A kapcsoló nyitásáért a Latch R lába felelős, egy komparátor van rákötve mely a tápegységben lévő söntellenállás áramából származó feszültséget és egy kontroll feszültséget hasonlít össze, és ha az áram meghalad egy bizonyos értéket akkor lekapcsol. Ezt a kontroll feszültséget a kimeneti feszültség leosztásából kapjuk.

### A paraméterek amikre méretezünk:
- bemeneti feszültség: $U_{be} = 48 V$
- kimeneti feszültség: $U_{ki} = 12 V$
- üzemi áram: $I_{norm} = 1 A$
- csúcsáram: $I_{max} = 2 A$
- a sönt árama egy áramtranszformátoron keresztül van mérve ami 50 vagy 100-as osztású $\rightarrow$ $CS_{ratio} = 50$ vagy $100$
- maximális kitöltési tényező: $\delta_{max} = 80 \%$
- frekvencia: $f = 200 kHz$
- a tápegységben lévő tekercs induktivitása: $L = 100 \micro H$
- a tápegységben lévő kondenzátor induktivitása: $C = 100 \micro F$
- a kimeneti feszültség leosztásában lévő felső ellenállás: $R_{20} = 9.1 k\Omega$

### Az áram mérésének méretezése:
A kapcsolás aminek a kimenete az IC CS lábára megy:
![[Pasted image 20221108191709.png]]
A CS láb működése: ha a rá kapcsolt feszültség $V_{CS}$ adatlapi értéknél nagyobb akkor a kapcsolót nyitja. 

Az adatlapból:
$V_{CS\_typ} = 1 V$
$V_{CS\_max} = 1.1 V$
$V_{CS\_min} = 0.9 V$

Az R1 ellenálláson folyható maximum áram: 
$$ I_{R1\_max} = I_{max} + \frac{\Delta i_L}{2} = 2.225 A$$
Így a B2 áramgenerátoron folyó maximális áram $CS_{ratio} = 100$ érték mellett: 
$$ I_{B2\_max} = \frac{ I_{R1\_max} }{ CS_{ratio} } = 22.25 mA$$

Így az R8 ellenállás értéke megkapható mivel ha azt szeretnénk hogy erre az áramra a legrosszabb esetben is biztos billenjen, ezért az $U_{CS\_max}$ feszültséget kell hogy elérje ekkor:
$$ R_8 = \frac{ U_{CS\_max} }{I_{B2\_max}} = 49.438 \Omega$$
Az ehhez legközelebbi standard ellenállás érték választásával: $R_8 = 51 \Omega$

### A kimeneti feszültség visszacsatolás méretezése:
A kapcsolás aminek a kimenete az IC FB lábára megy:
![[Pasted image 20221108191910.png]]

Az FB láb működése: egy hibaerősítő invertáló bemenetére van kötve, az erősítő nem inverteló bemenete pedig a belső $V_{REF}$ feszültségéből előállított $V_{FB}$ feszültségre van kötve, a kimenetéből pedig a $V_{CS}$ feszültség van előállítva. Az ellenállás osztóval azt szeretnénk elérni, hogy amikor a kimenet eléri az $U_{ki}$ feszültséget akkor az FB lábra kötött feszültség érje le vagy haladja meg az $U_{FB}$ feszültséget.
![[Pasted image 20221108192325.png]]

Az adatlapból:
$V_{FB\_typ} = 2.5 V$
$V_{FB\_max} = 2.55 V$
$V_{FB\_min} = 2.45 V$


$$ U_{ki} \times \frac{ R_{19} }{ R_{19} + R_{20} } = V_{FB} $$
Így $R_{19}$ a következő:
$$ R_{19} = R_{20} \times \frac{ V_{FB\_typ} }{ U_{ki} - V_{FB\_typ} } = 2.382 k\Omega$$
Az ehhez legközelebbi standard ellenállás érték választásával: $R_{19} = 2.4 k\Omega$

### Az oszcillátor tervezése
Az IC-ben lévő oszcillátor:
![[Pasted image 20221108193305.png]]
$R_{RT}$ és $C_{CT}$ elemeket nekünk kell méretezni, az IC-ben van egy invertáló hiszterézises komparátor, ami az adatlap szerint $V_{osc\_L} = 0.7V$ és $V_{osc\_H} = 3 V$ pontokban billen, és amikor negatív a kimenete akkor $I_{osc}$ árammal üríti az RC tagunkat.

Az adatlapból:
$I_{osc\_typ} = 8.4 mA$
$I_{osc\_max} = 9 mA$
$I_{osc\_min} = 7.7 mA$

A számításokhoz az egy tárolós rendszerek "magic" formuláját használtuk fel, ami:
$$ x(t) = végérték + (kezdetiérték - végérték) \times e^{ - \frac{t}{\tau} } $$

Az RC-tag töltése:
$$ V_{osc\_H} = V_{REF} + (V_{osc\_L} - V_{REF}) \times e^{ - \frac{t_{on}} {R_{RT}C_{CT}} } $$
Így a $t_{on}$:
$$ t_{on} = R_{RT}C_{CT} \times \ln{ \frac{V_{osc\_L} - V_{REF}} {V_{osc\_H} - V_{REF}} } $$

Az RC-tag ürítése:
$$ V_{osc\_L} = (V_{REF} - R_{RT}I_{osc}) + (V_{osc\_L} - (V_{REF} - R_{RT}I_{osc})) \times e^{ - \frac{t_{off}} {R_{RT}C_{CT}} }$$

Így a $t_{off}$:
$$ t_{off} = R_{RT}C_{CT} \times \ln {\frac{R_{RT}I_{osc} - (V_{REF} - V_{osc\_H})} {R_{RT}I_{osc} - (V_{REF} - V_{osc\_L})} } $$


Itt arra szeretnénk méretezni hogy legrosszabb esetben is maximum 80% legyen a kitöltése tényező - ez az az eset amikor a $V_{REF}$ a legkisebb és $I_{osc}$ a legnagyobb. A levezetések:
$$ \frac{t_{on}} {\delta_{max}} = \frac{t_{off}} {1-\delta_{max}} $$
$$ \frac{1-\delta_{max}}{\delta_{max}} t_{on} = t_{off} $$
$$ \frac{1-\delta_{max}}{\delta_{max}} \times R_{RT}C_{CT} \times \ln{ \frac{V_{osc\_L} - V_{REF}} {V_{osc\_H} - V_{REF}} } = R_{RT}C_{CT} \times \ln {\frac{R_{RT}I_{osc} - (V_{REF} - V_{osc\_H})} {R_{RT}I_{osc} - (V_{REF} - V_{osc\_L})}} $$
$R_{RT}C_{CT}$-vel lehet egyszerűsíteni, majd mind a két oldalt e-re emelem:

$$ (\frac{V_{osc\_L} - V_{REF}} {V_{osc\_H} - V_{REF}})^{ \frac{1-\delta_{max}}{\delta_{max}} } = \frac{R_{RT}I_{osc} - (V_{REF} - V_{osc\_H})} {R_{RT}I_{osc} - (V_{REF} - V_{osc\_L})}$$

A bal oldalt elnevezem k-nak, majd átrendezéssel megkapható az $R_{RT}$:

$$ k = (\frac{V_{osc\_L} - V_{REF\_min}} {V_{osc\_H} - V_{REF\_min}})^{ \frac{1-\delta_{max}}{\delta_{max}} } $$

$$ R_{RT} = \frac{1}{I_{osc\_max}}(V_{REF\_min}-\frac{kV_{osc\_L} - V_{osc\_H}}{k-1}) = 1.631 k\Omega $$

Az ehhez legközelebbi, ettől kisebb standard ellenállás érték választásával: 

$R_{RT} = 1.5 k\Omega$

És ebből meg lehet kapni az oszcillátorhoz szükséges $C_{CT}$-t:

$$ t_{on} = \delta_{max} T = \frac{\delta_{max}}{f} = R_{RT}C_{CT} \times \ln{ \frac{V_{osc\_L} - V_{REF\_min}} {V_{osc\_H} - V_{REF\_min}} }$$

$$ C_{CT} = \frac{\delta_{max}}{f R_{RT} \times \ln{ \frac{V_{osc\_L} - V_{REF\_min}} {V_{osc\_H} - V_{REF\_min}}} } = 3.09 nF $$

Az ehhez legközelebbi, ettől nagyobb standard kondenzátor érték választásával: 

$C_{CT} = 3.3 nF$

