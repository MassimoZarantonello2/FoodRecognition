```markdown
# Progetto FoodX_251

## Descrizione
Questo progetto si concentra sull'analisi e la creazione di modelli complessi per il riconoscimento delle immagini utilizzando il dataset FoodX_251. All'interno del progetto sono stati esplorati diversi modelli di machine learning, analizzate le performance su set di dati deboli e gestiti vari aspetti relativi alla qualità delle immagini.

## Struttura del Progetto

- **`utils/`**: Contiene il file `requirements.txt` per configurare l'ambiente virtuale.
- **`dataset/`**: Contiene i dati necessari per il progetto. Scaricare e decomprimere i seguenti set di dati nella cartella:
  - `train_set`
  - `test_set`
  - `val_set_degraded`
- **`FoodX_251/`**: Include l'analisi dei dati, lo studio dei modelli semplici e la creazione del modello complesso.
- **`ground_truth/`**: Contiene i file di ground truth. Alcuni di essi sono stati modificati per adattarsi a immagini mancanti e semplificare i processi di split.
- **`models/`**: Contiene le classi relative ai modelli utilizzati e quelli scartati. La sottocartella `trained_models/` include i modelli già allenati. I file che terminano con `-1` si riferiscono ai modelli rumorosi.
- **`scripts/`**: Contiene le classi e gli script che sono stati fondamentali per il progetto.
- **`graphs/`**: Contiene i grafici dei training dei vari modelli.

## Installazione

1. **Crea un ambiente virtuale**:

   ```bash
   python -m venv nome_ambiente
   ```

2. **Attiva l'ambiente virtuale**:

   - Su Windows:
     ```bash
     nome_ambiente\Scripts\activate
     ```
   - Su Linux/MacOS:
     ```bash
     source nome_ambiente/bin/activate
     ```

3. **Installa le dipendenze**:

   Il file `requirements.txt` si trova nella cartella `utils`. Per installare tutte le dipendenze, esegui:

   ```bash
   pip install -r utils/requirements.txt
   ```

## Utilizzo

1. **Prepara i dati**: Scarica e decomprimi i seguenti set di dati nella cartella `dataset/`:
   - `train_set`
   - `test_set`
   - `val_set_degraded`

2. **Esegui l'analisi dei dati**: L'analisi dei dati è contenuta nel file `FoodX_251`. Questo file include anche lo studio sui modelli semplici e la creazione del modello complesso.

3. **Allenamento del modello**: I modelli sono contenuti nella cartella `models/`. All'interno della sottocartella `trained_models/` puoi trovare i modelli già allenati. Puoi anche allenare i modelli a partire dal codice sorgente presente nella cartella `scripts/`.

Ecco un piccolo manuale di istruzioni su come usare la classe `EnsambleModel`, che puoi aggiungere al `README.md`:

4. **Aggiornamento delle Immagini di Train e Allenamento Ciclico dell'Ensamble**

Lo script `ensamble_image_increment.py` gestisce l'aggiornamento delle immagini di addestramento e l'allenamento ciclico dell'ensamble. Questo processo consiste nell'aggiungere progressivamente nuove immagini nel dataset di training, migliorando così l'accuratezza del modello nel tempo.

#### Uso dello Script `ensamble_image_increment.py`

Per eseguire l'aggiornamento delle immagini di train e allenare l'ensamble in modalità ciclica, basta eseguire lo script. Lo script si occupa di:

1. Aggiungere nuove immagini al dataset di training.
2. Allenare nuovamente i modelli dell'ensamble su questo dataset aggiornato.

Esempio di esecuzione:

```bash
python ensamble_image_increment.py
```

```markdown
## Uso della classe `EnsambleModel`

La classe `EnsambleModel` permette di creare un modello ensemble, allenare i modelli individuali e fare previsioni utilizzando il peso combinato di ciascun modello.

### Inizializzazione dell'Ensemble

Per creare un oggetto della classe `EnsambleModel`, bisogna fornire i seguenti parametri:

- `models_name`: una lista con i nomi dei modelli da usare nell'ensamble.
- `pre_trained`: se impostato su `True`, carica i pesi pre-addestrati per i modelli. Se impostato su `False`, non vengono caricati pesi pre-addestrati.
- `models_weights`: una lista di pesi per ciascun modello. La somma dei pesi determina l'importanza di ciascun modello durante la previsione. Questo parametro è utile solo se `pre_trained` è `True`.
- `num_classes`: il numero di classi del dataset (default: 251).

Esempio di inizializzazione:

```python
ensemble = EnsambleModel(
    models_name=['resnet', 'efficientnet', 'vgg'],
    pre_trained=True,
    models_weights=[0.3, 0.4, 0.3],
    num_classes=251
)
```

### Allenamento dell'Ensemble

Per allenare i modelli dell'ensamble, utilizzare il metodo `train_ensamble()`. I parametri richiesti sono:

- `train_dataset`: il dataset di addestramento (un oggetto della classe `ImageDataset`).
- `lr`: il tasso di apprendimento.
- `num_epochs`: il numero di epoche per l'allenamento (default: 10).
- `lc`: parametri aggiuntivi per la gestione dell'allenamento, se necessario.

Esempio di allenamento:

```python
train_losses, val_losses, train_accuracies, val_accuracies = ensemble.train_ensamble(
    train_dataset=train_data,
    lr=0.001,
    num_epochs=10
)
```

Questo metodo allena i modelli specificati in `models_name` e restituisce le perdite e le accuratezze per il training e la validazione.

### Previsioni con l'Ensemble

Per fare delle previsioni sui dati, utilizzare il metodo `predict()`. I parametri richiesti sono:

- `image_dataset`: un oggetto della classe `ImageDataset` contenente le immagini per cui fare le previsioni.
- `lc`: parametri aggiuntivi per la gestione delle previsioni, se necessario.

Esempio di previsione:

```python
images_idx, images_label, predictions_confidences = ensemble.predict(image_dataset=test_data)
```

Il metodo restituisce tre liste:

- `images_idx`: gli ID delle immagini.
- `images_label`: le etichette predette.
- `predictions_confidences`: le probabilità associate alle etichette predette.

### Caricamento di Modelli Pre-addestrati

Se `pre_trained=True`, i modelli vengono caricati dai pesi pre-addestrati salvati nella cartella `./models/trained_models/`. Il nome del modello deve corrispondere al nome del file di pesi (ad esempio, `resnet_-1.pth` per il modello ResNet).

### Nota sui Modelli Rumorosi

I file che terminano con `-1` fanno riferimento a modelli allenati su dati rumorosi. Se non si desidera usare i modelli rumorosi, è possibile omettere questi file dai pesi pre-addestrati.
```

4. **Grafici di training**: I grafici dei training dei modelli sono contenuti nella cartella `graphs/`.