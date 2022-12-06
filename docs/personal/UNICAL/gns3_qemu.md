# Guida GNS3 con QEMU

## Prerequisiti

- Almeno 8GB di ram
- CPU quad core o superiore
- VT-x e/o HyperV abilitato nel BIOS
- Extra:
  - Linux: Supporto KVM, vedi [qui](#linux-controllare-il-supporto-kvm)

E' preferibile utilizzare una distro debian/ubuntu-based.

### Test effettuati

Questa guida è stata testata con successo su:

- Ryzen 3 5300U, 8GB RAM, Pop_OS! 22.04 LTS
- Ryzen 5 PRO 5xxx, 8GB RAM, Ubuntu 20.04 LTS

### Linux - Controllare il supporto KVM

Aprire un terminale, e digitare `lsmod | grep kvm`.

Output previsto:

- CPU Intel: 2 entry: `kvm` e `kvm_intel`
- CPU AMD: 2 entry: `kvm` e `kvm_amd`

## Steps

- Disinstalla, se presenti, Virtualbox o VMWare, e rifai il setup iniziale di GNS3. Previene eventuali errori con il server di GNS3.
- Scarica il file zip contenente l'hard disk virtuale, scompattalo e posizionalo in una cartella a tua scelta.
- Apri GNS3 e recati su Edit, poi su Preferences.
- Apri la scheda "QEMU":
  - Seleziona "Enable hardware acceleration" e "Require hardware acceleration"
- Apri la scheda "Qemu VMs", e clicca sul tasto "New".
- Qui dovrai inserire i vari parametri della macchina virtuale:
  - Nome: a scelta :)
    - Nota: NON selezionare "This is a legacy ASA VM"
  - Qemu binary: seleziona "qemu-system-x86_64" o "qemu-system-x86_64w", preferibilmente versione "4.2.1" o superiore.
  - RAM: 256MB (o superiore)
  - Console type: seleziona "none", troverai come default "telnet"
  - Disk image:
    - Seleziona "New image", e clicca sul tasto "browse".
    - Seleziona il file scaricato in precedenza, e attendi il suo caricamento.
      - Nota: potrebbe volerci un po' per caricare l'immagine, attendi anche se la schermata è freezata.
    - Alla richiesta "Would you like to copy ..." seleziona "Yes".
- A VM creata, clicca su "Edit". Da qui, vai a:
  - Scheda general settings:
    - On close: seleziona "Send the shutdown signal (ACPI)"
    - Abilita "Auto start console"
  - Scheda network:
    - Cambia il numero di adapters da 1 a 4
  - Scheda advanced:
    - Se non abilitato, abilita "Use as a linked base VM"
- Salva, chiudi le impostazioni

### Note

1. All'avvio delle VM, ci sarà qualche secondo di schermo nero. Dopodichè dovrebbe apparire l'interfaccia di login.
   1. Username e password: root root
   2. Se l'emulazione sembra "lenta" anche nell'utilizzo, l'accelerazione hardware è mancante. [Assicurati di aver KVM attivo](#linux-controllare-il-supporto-kvm).
2. E' altamente consigliato spegnere le VM cliccando sul tasto "STOP" dentro GNS3, anzichè chiudere le finestre di QEMU. Questo permette alle VM uno spegnimento "non forzato", in modo da salvaguardare la vita e il contenuto dell'hard disk virtuale.
