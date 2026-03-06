# Progetto Trascrizione Lessico: Manuale Operativo

Per facilitare il processo di trascrizione e correzione del lessico, il progetto utilizza una struttura delle directories fissa. La struttura è composta da tre cartelle principali chiamate: Originals, Recorrected, Standardized. All’interno di ogni cartella ci sono delle sottocartelle, ogni sottocartella avrà come titolo la segnatura del manoscritto. All’interno della segnatura sarà presente un file .txt per ogni lettera, la nomenclatura del file txt è fissa e può avere a seconda della lettera uno dei seguenti valori:

```bash
01alpha.txt 06zeta.txt  11lambda.txt  16pi.txt  21phi.txt 02beta.txt  07eta.txt 12mu.txt 17rho.txt  22chi.txt 03gamma.txt 08theta.txt 13nu.txt  18sigma.txt 23psi.txt  04delta.txt 09iota.txt  14xi.txt  19tau.txt 24omega.txt 05epsilon.txt 10kappa.txt 15omicron.txt 20upsilon.txt   
```

```bash
. 
├── Originals/ 
│   ├── A= Vallic. E 11/ 
│   │   └── 13nu.txt 
│   ├── A= Vallic. E 11 B/ 
│   │   └── 13nu.txt 
│   └── g1 (Λ)= BML Plut. 59.16/ 
│       ├── 01alpha.txt 
│       └── 02beta.txt 
├── Recorrected/ 
│   ├── A= Vallic. E 11/ 
│   │   └── 13nu.txt 
│   ├── A= Vallic. E 11 B/ 
│   │   └── 13nu.txt 
│   └── g1 (Λ)= BML Plut. 59.16/ 
│       ├── 01alpha.txt 
│       └── 02beta.txt 
└── Standardized/ 
    ├── A= Vallic. E 11/ 
    │   └── 13nu.txt 
    ├── A= Vallic. E 11 B/ 
    │   └── 13nu.txt 
    └── g1 (Λ)= BML Plut. 59.16/ 
        ├── 01alpha.txt 
        └── 02beta.txt 
```


## Procedura


### Procedura sintetica
1. Copio cartella Template su Original.
2. Elimino le lettere che non intendo compilare
3. Faccio la trascrizione sul file relativo alla lettera per esempio 01alpha.txt (vedi criteri sotto).
4. (eseguo script di validazione)
5. Copio la trascrizione (per esempio 01alpha.txt) sulla cartella Reccorrected
6. Faccio la correzione sul file copiato (vedi criteri sotto)
7. Copio il file corretto (per esempio 01alpha.txt) sulla cartella Standardized
8. Standardizzo il file (per esempio 01alpha.txt) secondo le norme.

(se è presente un intervento scrittorio successivo)

9. Duplico la raccolta relativa la manoscritto sulla cartella Originals e rinonimo la cartella (per esempio A= Vallic. E 11 -> A= Vallic. E 1 B).
10. Ripeto punti 3-8

Inizialmente si crea una cartella con il nome del manoscritto (es. A= Vallic. E 11 ) nella cartella `Originals`.

(posso sia copiare l'intera cartella `Template` e rinominarla o incollare la lettera che si vuole fare dalla cartella `Template` in una cartella creata da noi su `Originals`.

Per ogni lettera, si compilano i metadati richiesti:

```
# METADATA format: Dublin Core (dc:https://www.dublincore.org/specifications/dublin-core/dcmi-terms/elements11/) 
# dc:creator: Scattolin, Paolo (Cognome, Nome) 
# dc:date: 2025-08-08 (la data di compilazione) 
# dc:source: Leiden University Library, Manuscript VGQ 63 
# dc:coverage: fol. 5r-12v. (i fogli dove si trova il testo trascritto) 
# dc:relation: Diktyon ID 38170 (la relazione con l’ID di Diktiyon) 
# dc:description: qualche nota generale 
```

** Per il momento si è deciso di agire solo sul lemma non su l'interpretamentum, le cartelle Standardized e Recorrected
contengono il lemma original seguito da duepunti e dalla sua correzione. **

Nella cartella Originals si compie una trascrizione diplomatica, senza applicare correzioni. Si trascrive il testo greco compresi errori di accento e spirito, itacismo, etc.

Ad ogni lemma seguono: uno spazio i due punti e un altro spazio seguito dall’interpretatamentum,non si pone punto alla fine:

```
ναίων : οἰκων 
```

## Note e commenti del trascrittore 

Il trascrittore può commentare con note di trascrizione creando una nuova riga che inizia con un cancelletto e uno spazio:

```bash
ναίων : οἰκων
ναίοντα : οἰκοῦντα 
ναιϊν : τῶν δύο 
# f.45v questo è un commento non verrà considerato dal software. 
ναι μήν : ὄντως δή 
ναίχι : ναί ἀττικῶς 
# note: non riesco a leggere questo 
ναίεσθαι : πορεύεσθαι
```

È fortemente consigliabile che tutti i commenti siano presenti nella trascrizione diplomatica (cartella Originals) e che si propaghino nella versione ricorretta ed in quella standardizzata, questo permette una corrispondenza tra le righe dei file. 

### Commenti generici
I commenti generici vanno introdotti con # note: per esempio:   

```
# note: non sono riuscito a leggere bene  
```

### Inizio del folgio

Inizio del foglio: 
```
# f.45v 
```
Ogni qualvolta si inizia a trascrivere un foglio si segnala l’inizio del foglio con `f.<numero_della_carta><recto o verso>`, in modo che la seguente espressione regolare sia valida: 

```re
^# f\.\d+[rv] 
```

### Correzioni
Se il copista o una mano successiva ha fatto qualche correzione al testo si procederà nel seguente modo, verrà copiata la versione non corretta, preceduta da un commento: 

```
# cor 1 = ναίχι : ναί ἀττικῶς -> ναίχ : ναί ἀτικῶς 
ναίχ : ναί ἀτικῶς 
```

Il commento seguente il seguente pattern:
`# cor {identificativo mano} = {voce originale} -> {voce ricorretta}` 
Secondo il seguente pattern regex: 

```re
^# cor \d+ = .+ : .+ \-\> .+ : .+$ 
```

{identificativo mano} può essere utilizzato per disambiguare correzioni appartenenti a mani differenti per esempio:
`# cor 2 = ναίχι : ναί ἀττικῶς`
Può essere usato per identificare una seconda mano con un ulteriore intervento correttorio.

Nel caso di più di una correzzione nello stesso lemma si utilizzano due commenti separati.

```
# cor 1 = ξαίνω : νήθω -> ξαίνω : νήθω σορεύω
# cor 2 = ξαίνω : νήθω σορεύω -> ξαίνω : νήθω σορεύω καλοσ
ξαίνω : νήθω
```

Questi dati possono essere utilizzati per creare una nuova versione del manoscritto (es. A= Vallic. E 11 B) nella cartella Originals copiando (e quindi nella cartella al cui interno sarà presente la lettera del manoscritto originario con le correzioni della seconda mano.

Questa procedura potrebbe essere automatizzata, quindi per il momento si consiglia di scrivere solo le correzioni.

Queste cartelle verranno poi copiate nella cartella Recorrected dove si applicheranno le correzioni di errori grammaticali.

Infine, le cartelle contenenti i file ricorretti verranno a loro volta copiate nella cartella Standardized dove i file verranno standardizzati secondo le regole di standardizzazione definite dal progetto.





