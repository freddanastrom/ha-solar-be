# ha-solar-be
Det här är en samling av sensorer för att få ytterligare informaton om intäkter/besparing och beräkning av break-even för din solcellsanläggning.

## Disclaimer
Den beräkning som görs för break-even baseras på värdet av såld el, samt egenförbrukningen under en rullande 12-månadersperiod.  
Stora variationer av elpriset kommer alltså påverka datumet.

Ett någorlunda rättvist värde fås först efter att dessa sensorer använts under minst 12 månader.  
Innan dess kommer du att få stora svängningar i beräkningen, samt ett VÄDLIGT missvisande datum. Det blir bättre med tiden..

## Grundkrav
Exemplet nedan baseras på sensorerna som finns tillgängliga från en Huawei växelriktare med smartmätare.  
Smartmätaren är en förutsättning i det här fallet, då den gör att du i realtid kan se hur mycket el som exporteras ut på elnätet.  
Jag använder HACS-integrartionen https://github.com/wlcrs/huawei_solar, molnintegrationen till FusionSolar är inte testad.

Har du ingen smartmätare inkopplad kan du förmodligen få ut motsvarande värden med exempelvis en Tibber pulse, p1reader, eller någon liknande pryl som läsar import/export genom P1-porten på din elmätare.  
I så fall behöver du justera namnen på ingående sensorer som stämmer överens med din anläggning.

Denna beräkning blir endast korrekt om du både köper och säljer el till timspot.

### Värdering av egenförbrukad el
Hur man väljer att värdera egenförbrukad el finns det många varianter på och vad som rätt eller fel är högst individuellt.  
Jag har valt att värdera den till samma pris som om jag skulle behövt köpa motsvarande från mitt elbolag. Det är ju det verkliga alternativet om jag inte hade haft solcellsanläggningen.  
Alltså; aktuellt spotpris + överföring + skatt.

Anser du att den egenförbrukade elen ska värderas annorlunda behöver du justera "sensor.inverter_hourly_yield_self_consumption_earnings"

## Sensorer
Ingående sensorer som jag använder:  
- sensor.inverter_daily_yeild (daglig produktion mätt i kWh från solcellsanläggningen)
- sensor.power_meter_exported (total export till elnätet mätt i kWh)
- sensor.nordpool_kwh_se3_sek_4_10_025 (aktuellt spotpris från Nordpool, pris inkl. moms utan nätavgifter, skatter, nätnytta eller liknande)

## Detta får du:
### Break-even
- sensor.solar_investment (räknare som utgår från din anläggnings kostnad och tickar mot 0, vid break-even ökar den och visar vad du verkligen tjänat på investeringen)
- sensor.solar_investment_predicted_break_even (uppskattat datum för break-even)

### Egenförbrukad el
- sensor.inverter_hourly_yield_self_consumption (mätare för egenförbrukad el, timme)
- sensor.inverter_hourly_yield_self_consumption_earnings (värde för egenförbrukad el, timme)
- sensor.inverter_total_yield_self_consumption (mätare för egenförbrukad el, total sedan start)
- sensor.inverter_total_yield_self_consumption_earnings (värde för egenförbrukad el, totalt sedan start)

Dessa ingår också, men är inget krav och kan tas bort om så önskas.  
- sensor.inverter_daily_yield_self_consumption (mätare för egenförbrukad el, daglig, ej nödvändig och kan uteslutas)
- sensor.inverter_daily_yield_self_consumption_earnings (värde för egenförbrukad el, daglig, ej nödvändig och kan uteslutas)
- sensor.inverter_monthly_yield_self_consumption (mätare för egenförbrukad el, månatlig, ej nödvändig och kan uteslutas)
- sensor.inverter_monthly_yield_self_consumption_earnings (värde för egenförbrukad el, månatlig, ej nödvändig och kan uteslutas)
- sensor.inverter_yearly_yield_self_consumption (mätare för egenförbrukad el, årlig, ej nödvändig och kan uteslutas)
- sensor.inverter_yearly_yield_self_consumption_earnings (värde för egenförbrukad el, årlig, ej nödvändig och kan uteslutas)

### Såld el
- sensor.power_meter_exported_hourly (mätare för såld el, timme)
- sensor.power_meter_exported_hourly_earnings (värde för såld el, timme)
- sensor.power_meter_exported_total_earnings (totalt värde av såld el)

Dessa ingår också, men är inget krav och kan tas bort om så önskas.  
- sensor.inverter_daily_yield_self_consumption (mätare för egenförbrukad el, daglig, ej nödvändig och kan uteslutas)
- sensor.inverter_daily_yield_self_consumption_earnings (värde för egenförbrukad el, daglig, ej nödvändig och kan uteslutas)
- sensor.inverter_monthly_yield_self_consumption (mätare för egenförbrukad el, månatlig, ej nödvändig och kan uteslutas)
- sensor.inverter_monthly_yield_self_consumption_earnings (värde för egenförbrukad el, månatlig, ej nödvändig och kan uteslutas)
- sensor.inverter_yearly_yield_self_consumption (mätare för egenförbrukad el, årlig, ej nödvändig och kan uteslutas)
- sensor.inverter_yearly_yield_self_consumption_earnings (värde för egenförbrukad el, årlig, ej nödvändig och kan uteslutas)

### Summa av egenförbrukad och såld el
- sensor.solar_earnings_hourly (värde av såld och egenförbrukad el, timme)
- sensor.solar_earnings_total (värde av såld och egenförbrukad el, totalt)
- sensor.solar_earnings_12m (värde av såld och egenförbrukad el rullande 12 månader)
- sensor.solar_earnings_12m_oldest (hjälpare för att kunna utföra korrekt beräkning innan det gått 12 månader. Kan tas bort efter 12 månader, men då måste också sensorn som beräknar datum justeras.)

Dessa ingår också, men är inget krav och kan tas bort om så önskas.  
- sensor.solar_earnings_daily (värde av såld och egenförbrukad el, daglig, ej nödvändig och kan uteslutas)
- sensor.solar_earnings_monthly (värde av såld och egenförbrukad el, månatlig, ej nödvändig och kan uteslutas)
- sensor.solar_earnings_yearly (värde av såld och egenförbrukad el, årlig, ej nödvändig och kan uteslutas)