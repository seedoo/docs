# Ambiente di sviluppo

Questa guida ha lo scopo di illustrare gli step necessari per configurare un ambiente completo per lo sviluppo di Seedoo.

Dopo aver installato le dipendenze di sistema indicate nella guida di installazione, occorre eseguire i passi descritti nei paragrafi seguenti.

## Cartella dedicata per Seedoo

Creare una directory dedicata al progetto nella HOME dell'utente:

```bash
mkdir -p ~/git/seedoo
```

Tutti i comandi che seguono nella guida sono relativi alla cartella appena creata. Occorre quindi spostarsi all'interno della directory:

```bash
cd ~/git/seedoo
```

## Ottenere il codice

Il primo step per iniziare a sviluppare è ottenere il codice sorgente delle varie componenti.

### Seedoo

Clonare il repository principale da Github:

```bash
git clone https://github.com/seedoo/seedoo.git
```

Clonare i repository aggiuntivi necessari per il funzionamento di Seedoo:

```bash
git clone https://github.com/seedoo/seedoo-attivita.git
git clone https://github.com/seedoo/l10n-italy.git
```

Se necessario, clonare eventuali repository aggiuntivi contenenti moduli custom.

### Odoo Community Backports

Seedoo è un'applicazione basata su Odoo. Occorre quindi clonare il repository relativo alla versione di Odoo sviluppata dalla Community, branch 8.0. Successivamente effettuare il checkout del repository al commit corretto indicato nel repository principale (https://github.com/seedoo/seedoo):

```bash
git clone -b 8.0 https://github.com/oca/ocb.git
cd ocb
git checkout <commit-hash>
```

## Creazione VirtualEnv Python

Seedoo è scritto in python e richiede una serie di librerie dedicate. Creare quindi un virtualenv dedicato:

```bash
virtualenv venv
```

In seguito è necessario installare le librerie python richieste. Ogni repository contiene, se necessario, un file chiamato `requirements.txt` con l'elenco delle dipendenze in formato adatto per il comando `pip`.

Installare le dipendenze di base di Odoo:

```bash
./venv/bin/pip install -r ./ocb/requirements.txt 
```

Ripetere il comando per tutti i repository che necessitano di dipendenze aggiuntive (`oca_requirements.txt` **non** è un file di dipendenze python):

```bash
./venv/bin/pip install -r ./seedoo/requirements.txt 
```

## Apertura progetto su PyCharm

### Cartella principale

Dopo aver avviato PyCharm, selezionare il comando **Open** ed aprire la cartella `~/git/seedoo/seedoo`.

### Aggiunta Document Root

In seguito, aprire le impostazioni progetto dal menu `File`-> `Settings` -> `Project` -> `Project Structure`.

Aggiungere tramite il pulsante `Add Content Root` tutte le seguenti directory:

- `seedoo-attivita`
- `l10n-italy`
- `ocb`

### Impostazione cartelle sorgenti

Per fare in modo che PyCharm riconosca correttamente il codice occorre selezionare le directory da considerare come sorgenti.

Selezionando le diverse Content Root, impostare le seguenti cartelle come `Soruces`:

- `seedoo`: solo la sotto-cartella `core`
- `seedoo-attivita`: root directory
- `l10n-italy`: root directory
- `ocb`: root directory

Occorre inoltre impostare come `Sources` le root directory di ogni repository aggiuntivo.

### Configurazione di avvio

Per avviare Seedoo all'interno di PyCharm occorre creare una nuova configurazione nel menu apposito con i seguenti parametri:

- Script: `~/git/seedoo/ocb/openerp-server`
- Python Interpeter: `~/git/seedoo/venv/bin/python`
- Interpeter Options: *vuoto*
- Working directory: `~/git/seedoo/`
- Disabilitare `Add content roots to PYTHONPATH`
- Disabilitare `Add source roots to PYTHONPATH`
- Abilitare `Single instance only`

Impostare come `Script parameters` i seguenti parametri:

```
--addons-path=ocb/openerp/addons,ocb/addons,seedoo/core,seedoo-attivita,l10n-italy
--xmlrpc-port=8069
--db_host=127.0.0.1
--db_port=5432
--db_user=seedoo
--db_password=seedoo
```

Se sono presenti repository aggiuntivi occorre aggiungere le relative directory al parametro `--addons-path`.

## Riepilogo

Al termine delle operaizoni all'interno della directory `~/git/seedoo` si troveranno le seguenti cartelle:

- `seedoo`: Repository principale
- `seedoo-attivita`: Modulo aggiuntivo
- `l10n-italy`: Modulo aggiuntivo
- `ocb`: Odoo versione Community
- `venv`: Python Virtualenv

