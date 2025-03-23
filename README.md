# Junsun-Backlight-Dimming
A brief guide on how to dim the lowest backlight setting on a Junsun car radio.


Il problema più grande di queste autoradio è la luminosità minima che di notte ti distrugge le diottrie rimaste. È praticamente impossibile fare retromarcia con la radio accesa, per questo ho deciso di modificarla per poter diminuire la luminosità minima, pur mantendo inalterata quella massima. 

Una volta aperta la radio ed arrivati all'unico PCB a bordo, è sufficiente individuare l'integrato che genera la tensione di alimentazione per la retroilluminazione. Nel mio caso si trovava dal lato opposto del connettore ed era un Boost converter per pilotare i led partendo da una tensione più bassa. La sigla riportata sopra è S87Cd81, marcatura corrispondente all'integrato STI9287. (https://www.lcsc.com/datasheet/lcsc_datasheet_2409021801_TMI-STI9287_C842664.pdf)

![image](https://github.com/user-attachments/assets/4fe1fdde-6723-4a34-9b6d-11807659afa0)

Dal datasheet emerge che l'integrato regola la corrente nei led mantenendo 202 mV ai capi della resistenza R1. Nel mio caso R1 era implementata con due resistenze 0805 in parallelo, una da 1 ohm e una da 2.7 ohm, per un totale di 0.73 ohm. Con questa configurazione la corrente alla luminosità massima è di 280 mA (Con una tensione sui led di circa 18 V).

Ho quindi rimosso le due resistenze, mettendo al posto di quella da 2.7 ohm una da 5.1 ohm in modo da garantirmi una luminosità minima abbastanza bassa (circa 40 mA, secondo me si può scendere ancora ma già così è decisamente migliorata la situazione) mentre quella da 1 ohm è rimasta in posizione da un capo, mentre l'altro capo è stato sollevato e connesso al drain di un MOSFET (BSS138).
Questo Mosfet ha poi il source a massa e il gate connesso, tramite una resistenza da 10K all'alimentazione dell'STI9287. Un altro mosfet a canale N è messo con il drain al gate del primo mosfet, il source a massa e la base connessa al pin di alimentazione della retroilluminazione della pulsantiera del volume. In questo modo, finche i fari dell'auto restano spenti, il secondo MOS è interdetto e la resistenza da 10K porta in conduzione il primo, mettendo in parallelo la resistenza da 1 ohm con quella da 5,1, consentendo una luminosità massima simile a quella originale (si può fare uguale calcolando la giusta resistenza da mettere in parallelo a quella da 5,1 ma non mi interessava). Quando invece si accendono i fari, la pulsantiera si illumina e con lei si accende anche il secondo mos, portando il gate a 0 del primo, spengendolo. Così facendo la resistenza da 1ohm viene scollegata e resta solo quella da 5,1 ohm a stabilire la corrente massima, ulteriormente diminuita dal PWM che la radio fornisce all'integrato per abbassare la luminostià come faceva prima.

Qui di seguito uno schema di massima del circuito:

![image](https://github.com/user-attachments/assets/97a084c4-54bf-4883-8984-2f3784c74a86)



Questa è la situazione terminata ![image](https://github.com/user-attachments/assets/bed99c34-5e94-4b0f-9684-e96dbb095825)

![image](https://github.com/user-attachments/assets/6c4c4429-d171-413d-bb81-b3139ebd3200)


