# DALI-AccessStadium

Installation
OS X & Linux:

Per scaricare e installare SICStus Prolog (è necessario), seguire le istruzioni al link https://sicstus.sics.se/download4.html. Adesso è possibile clonare il repository ed eseguire il comando per far partire il programma:

 git clone https://github.com/lucadisab/DALI-AccessStadium/
 cd DALI-project/application/
 bash startmas.sh

Le finestre che si apriranno: Prolog LINDA server (active_server_wi.pl) Prolog FIPA client (active_user_wi.pl) 1 instanza di DALI per ogni agente (active_dali_wi.pl)
Organizzazione delle cartelle

La cartella DALI-project contiene due sottocartelle application e src. Per poter eseguire il progetto è necessario andare in application e da li far partire il termina con il seguente comando 'bash startmas.sh'.

Istruzioni per l'avvio 

1. Una volta clonata la repository accedere alla cartella DALI-AccessStadium/application da terminale ed eseguire bash startmas.sh;
2. Mandare un messaggio al primo agente dicendogli di poter raggiungere il tornello per il controllo del biglietto;
3. Insert name of addressee: agent1.
4. Insert From: user.
5. Insert message: send_message(tornelli_aperti,user).


Descrizione del mas.

3 tipi di agenti: <br>
-tornello, agente che ha il compito di far entrare gli agenti nello stadio controllando che siano in possesso del biglietto e basandosi sulla loro priorità;<br>
-bigliettaio, agente nel botteghino che vende i biglietti a chi non ne è in possesso;<br>
-agente(n, per n=1,..,4), agente n-esimo che vuole entrare allo stadio.

Lista eventi esterni, interni ed azioni per tipo di agente:

Eventi Esterni ed azioni tornello:<br>
-richiesta_ingressoE(Id,Priorita): attraverso questo evento l’agente tornello viene svegliato e inizia ad eleggere chi tra gli agenti ha priorità di entrare rispetto ad altri, riceve in input l’Id dell’agente e la priorità;
<br>-fai_entrareA: contiene l’algoritmo distribuito che permette l’ingresso con priorità più alta (=0)  a chi ha già il biglietto e priorità più bassa (=1) a chi lo deve comprare;
-entratoE: evento che rimuove dal database l’ordine fatto dall’agente che è entrato.<br>
Eventi Interni tornello:<br>
-condizione_tornelloI: Il tornello rivede il proprio stato e se è libero, comunica che è pronto a ricevere.

Eventi Esterni ed azioni bigliettaio:<br>
-richiesta_acquistoE(Id_agente): evento con cui il bigliettaio annuncia che sta vendendo un biglietto all’agente.<br>
Eventi Interni bigliettaio:<br>
-condizione_bigliettaioI: Il bigliettaio è pronto a vendere i biglietti e lo comunica agli agenti con vendi_bigliettoA.

Eventi Esterni ed azioni agente(n, per n=1,..,4):<br>
-tornelli_apertiE: Evento che sveglia l’agente e gli dice di andare ai tornelli;<br>
-vai_tornelloA: Questa azione è resa possibile se il biglietto è stato comprato altrimenti l’agente deve preoccuparsi di comprare il biglietto e quindi svegliare l’agente bigliettaio con compra_bigliettoA;
<br>-compra_bigliettoA: sveglia l’agente bigliettaio per acquistare il biglietto;
<br>-biglietto_acquistatoE: una volta acquistato il biglietto l’agente può andare al tornello (biglietto_comprato adesso è true);
<br>-accedi_allo_stadioE: l’agente può entrare allo stadio una volta che ha ricevuto il permesso.


Considerazioni e progetti futuri:
Il sistema da noi proposto rispecchia uno use case di pochi agenti ma il sistema può essere esteso a casi reali con un numero grande di agenti. 
In tal caso si può pensare di estendere l’agente tornello con un evento interno che ad una certa ora (ad esempio l’inizio dello spettacolo) chiuda il tornello e non permetta più di entrare.
Si può inoltre pensare di estendere l’agente bigliettaio con un evento interno che controlli il numero di biglietti venduti e, quando tutti i biglietti sono stati venduti, si stoppa la vendita dei biglietti informando gli agenti che ad esempio lo spettacolo è “sold out”.

Descrizione del Sequence diagram

Nel sequence diagram illustriamo la sequenza di interazioni tra gli oggetti, necessari per realizzare lo use case.
L’oggetto Utente avvisa che i tornelli sono aperti: a questo punto l’agente1 viene svegliato e successivamente vengono svegliati l’agente2, l’agente3 e infine l’agente4.
Tutti gli agenti tentano l’accesso al tornello svegliando l’agente tornello per entrare allo stadio ma vengono vincolati dalla condizione ‘hai il biglietto?’ descritta dall’  AlternativeCombinedFragment ‘Alt’ che specifica cosa avviene se si verifica la condizione o se non si verifica la condizione, ovvero se si ha il biglietto oppure no.
Concettualmente finchè l’agente non avrà il biglietto non potrà andare al tornello ed andare allo stadio. Affinchè però l’accesso sia consentito a tutti l’agente che non ha il biglietto può svegliare il bigliettaio e andare a comprare il biglietto. Appena gli viene dato il biglietto può entrare nello stadio.


![StadiumSequenceDiagram](https://user-images.githubusercontent.com/45095731/181846800-b7fd4e44-2dbd-41a6-81d7-ef6a88baf748.png)

![Screenshot-esecuzione](https://user-images.githubusercontent.com/45095731/181846851-315eb345-fe36-4200-b864-373109b634b9.png)
