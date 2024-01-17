Det här är en samling sensorer för att få ytterligare informaton om intäkter/besparing, samt beräkning av break-even för din solcellsanläggning.  
Ursprungligen var syftet med detta bara för att kunna se värdet på den egenförbrukade elen, men det 

## Disclaimer
Den beräkning som görs för break-even baseras på värdet av såld el, samt egenförbrukningen under en rullande 12-månadersperiod.  
Stora variationer av elpriset från år till år kommer alltså påverka datumet.

Ett någorlunda rättvist värde fås först efter att dessa sensorer använts under minst 12 månader.  
Innan dess fungerar allt, men du kommer att få stora svängningar i beräkningen av datum, samt ett VÄDLIGT missvisande datum.  
Det blir bättre med tiden..

Denna beräkning blir endast korrekt om du köper och säljer el till timspot.
## Grundkrav
Exemplet nedan baseras på sensorerna som finns tillgängliga från en Huawei växelriktare med smartmätare.  
Smartmätaren är en förutsättning i det här fallet, då den gör att du i realtid kan mäta hur mycket el som exporteras ut på elnätet.  
Jag använder HACS-integrartionen https://github.com/wlcrs/huawei_solar, molnintegrationen till FusionSolar är inte testad.

Har du ingen smartmätare inkopplad kan du förmodligen få ut motsvarande värden med exempelvis en Tibber pulse, p1reader, eller någon liknande pryl som läser import/export genom P1-porten på din elmätare.  
I så fall behöver du justera namnen på ingående sensorer så att dom stämmer överens med din anläggning.

Har du redan några av sensorerna för att mäta export, egenförbrukning och kostnader nedan så använder du givetvis dom.

### Värdering av egenförbrukad el
Hur man väljer att värdera egenförbrukad el finns det många varianter på och vad som rätt eller fel är högst individuellt.  
Jag har valt att värdera den till samma pris som om jag skulle behövt köpa motsvarande från mitt elbolag.  
Det är ju det verkliga alternativet om jag inte hade haft solcellsanläggningen.  
Alltså; aktuellt spotpris + överföring + skatt.

Anser du att den egenförbrukade elen ska värderas annorlunda behöver du justera "sensor.electricity_import_price"

### Sensorer som behövs
Ingående sensorer som jag använder:  
- sensor.inverter_daily_yeild  
Daglig produktion mätt i kWh från solcellsanläggningen

- sensor.power_meter_exported  
Total export till elnätet mätt i kWh

- sensor.nordpool_kwh_se3_sek_4_10_025  
Aktuellt spotpris från Nordpool, pris inkl. moms utan några tillägg eller avgifter

## Detta får du:
### Break-even
- sensor.solar_investment  
Räknare som startar på minus. Den utgår från din anläggnings kostnad och tickar mot 0, vid break-even slår den över till positivt värde och visar vad du verkligen tjänat på investeringen

- sensor.solar_investment_predicted_break_even  
Uppskattat datum för break-even

### Kostnader
- sensor.electricity_grid_import_price  
Kostnad för köpt el, totalt med ev. påslag, skatter och nätavgifter 

- sensor.electricity_grid_export_price  
Ersättning för såld el, inklusive nätnytta. Skatteavdraget på 60 öre är inte med här.
### Egenförbrukad el
Mätare för egenförbrukad el i kWh, samt värdering.  
Dom som krävs är per timme och total, övriga är valfria att lägga till.  
- sensor.inverter_hourly_yield_self_consumption
- sensor.inverter_total_yield_self_consumption
- sensor.inverter_hourly_yield_self_consumption_earnings
- sensor.inverter_total_yield_self_consumption_earnings

Valfria (dag, månad, år):  
- sensor.inverter_daily_yield_self_consumption
- sensor.inverter_monthly_yield_self_consumption
- sensor.inverter_yearly_yield_self_consumption
- sensor.inverter_daily_yield_self_consumption_earnings
- sensor.inverter_monthly_yield_self_consumption_earnings
- sensor.inverter_yearly_yield_self_consumption_earnings

### Såld el
Mätare för såld el i kWh, samt ersättning.  
Dom som krävs är per timme och total, övriga är valfria att lägga till.  
- sensor.power_meter_exported_hourly
- sensor.power_meter_exported_hourly_earnings
- sensor.power_meter_exported_total_earnings

Valfria(dag, månad, år):  
- sensor.power_meter_exported_daily
- sensor.power_meter_exported_monthly
- sensor.power_meter_exported_yearly
- sensor.power_meter_exported_daily_earnings
- sensor.power_meter_exported_monthly_earnings
- sensor.power_meter_exported_yearly_earnings

### Summa av egenförbrukad och såld el
Summering av värdet för egenförbrukad och såld el  

Dessa krävs:  
- sensor.solar_earnings_total (värde av såld och egenförbrukad el, totalt)
- sensor.solar_earnings_12m (värde av såld och egenförbrukad el rullande 12 månader)
- sensor.solar_earnings_12m_oldest (hjälpare för att kunna utföra korrekt beräkning innan det gått 12 månader. Kan tas bort efter 12 månader, men då måste också sensorn som beräknar datum justeras.)

Valfria(timme, dag, månad, år):  
- sensor.solar_earnings_hourly
- sensor.solar_earnings_daily
- sensor.solar_earnings_monthly
- sensor.solar_earnings_yearly
