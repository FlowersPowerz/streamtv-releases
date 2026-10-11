# Registratore

Un computer sempre acceso registra al posto del telefono, anche quando il telefono è spento. Le registrazioni si
riguardano dall'app, su qualunque dispositivo dell'account.

Serve una macchina con Docker sempre accesa nella rete di casa: un NAS, un mini PC o un Raspberry Pi 4/5.
L'immagine è per x86 e ARM.

## Installazione

Crea un file `compose.yaml`, sostituendo l'indirizzo con quello della macchina:

```yaml
services:
  recorder:
    image: ghcr.io/flowerspowerz/recorder:latest
    container_name: recorder
    restart: unless-stopped
    stop_grace_period: 30s
    ports: ["8190:8080", "8191:8091"]
    environment:
      RECORDER_PUBLIC_URL: http://192.168.1.50:8190/
      RECORDER_NAME: casa
      TZ: Europe/Rome
    volumes: ["data:/data"]
volumes:
  data:
```

Poi, nella stessa cartella: `docker compose up -d`. Sui NAS con un'interfaccia per Docker (Synology Container
Manager, Unraid, Portainer, CasaOS) lo stesso file si incolla come «progetto» o «stack».

## Collegamento

1. Apri `http://<indirizzo della macchina>:8191` dal browser: compare un codice di otto caratteri, valido 10 minuti.
   È lo stesso che si legge nei log del container. Se è scaduto, premi «Nuovo codice».
2. Nell'app: **Impostazioni → Registrazioni → Codice del registratore**, inserisci il codice.

Il collegamento si salva sull'account, cifrato: gli altri dispositivi lo ricevono da soli.

## Uso

Dalla guida, «Registra» su un programma futuro. Il registratore lo prende qualche minuto prima dell'inizio. A fine
programma la registrazione compare in On Demand.

## La pagina

`http://<indirizzo della macchina>:8191` mostra le registrazioni in corso, in programma, pronte e non riuscite: le
pronte si eliminano, le altre si annullano. Dopo il collegamento dice «Collegato all'account»; «Nuovo codice» serve
per collegare un altro account.

Impostazioni: nome, registrazioni contemporanee per playlist, eliminazione automatica dopo N giorni (0 = mai).
Salvare riavvia il registratore; il nome non si cambia durante una registrazione. I valori restano nel volume `data`
e prevalgono su `RECORDER_NAME` e `RECORDER_MAX_PER_PLAYLIST`.

## Destinazione

Le registrazioni stanno nel volume `data`, oppure su un servizio esterno: **Destinazione → Cambia** sulla pagina.
WebDAV e cartella di rete (SMB) chiedono indirizzo e credenziali; Dropbox e OneDrive un accesso: «Apri la pagina di
accesso», consenso, poi si incolla nella pagina l'indirizzo che il browser apre e non carica (inizia con
`http://localhost:53682/`). «Verifica e salva» prova a scrivere nella cartella e riavvia il registratore. L'app non
cambia nulla: stesso indirizzo, stesse credenziali. Le registrazioni già fatte restano nella destinazione precedente;
«Torna alla cartella del registratore» le mostra di nuovo. Se all'avvio il servizio non risponde, il registratore usa la
propria cartella e la pagina lo segnala.

## Note

- **Indirizzo.** `RECORDER_PUBLIC_URL` è l'indirizzo con cui i dispositivi raggiungono le registrazioni: deve
  funzionare dal telefono. Fuori casa serve una VPN (WireGuard, Tailscale) o un indirizzo pubblico.
- **Porta 8191.** Chiunque la apra vede il codice e gestisce registrazioni, impostazioni e destinazione: non va esposta su Internet.
- **Playlist.** Il registratore scarica la playlist M3U del canale: la macchina deve raggiungere il fornitore.
- **Nome.** `RECORDER_NAME` è il nome iniziale con cui l'app indica il registratore; poi si cambia dalla pagina.
- **Aggiornamento.** `docker compose pull && docker compose up -d`. Le registrazioni e le credenziali restano nel
  volume `data`.

---

# Recorder

An always-on computer records instead of the phone, even while the phone is off. Recordings play back in the
app, on any device of the account.

It needs an always-on machine with Docker on the home network: a NAS, a mini PC or a Raspberry Pi 4/5. The image
is built for x86 and ARM.

## Install

Save the `compose.yaml` above, replacing the address with the machine's own, then run `docker compose up -d` in
the same folder. On a NAS with a Docker interface (Synology Container Manager, Unraid, Portainer, CasaOS), paste
the same file as a "project" or "stack".

## Linking

1. Open `http://<machine address>:8191` in a browser: it shows an eight-character code, valid for 10 minutes. The
   container log shows the same one. If it has expired, press "New code".
2. In the app: **Settings → Recordings → Recorder code**, enter the code.

The link is saved to the account, encrypted: the other devices receive it on their own.

## Use

From the guide, "Record" a future programme. The recorder takes it a few minutes before it starts. When the
programme ends, the recording appears in On Demand.

## The page

`http://<machine address>:8191` lists the recordings under way, scheduled, ready and failed: ready ones can be
deleted, the others cancelled. Once linked it says "Linked to the account"; "New code" links another account.

Settings: name, simultaneous recordings per playlist, automatic deletion after N days (0 = never). Saving restarts
the recorder; the name cannot change during a recording. The values stay in the `data` volume and take precedence
over `RECORDER_NAME` and `RECORDER_MAX_PER_PLAYLIST`.

## Destination

Recordings live in the `data` volume, or on an external service: **Destination → Change** on the page. WebDAV and
network share (SMB) ask for address and credentials; Dropbox and OneDrive for a sign-in: "Open the sign-in page",
consent, then paste into the page the address the browser opens and cannot load (it starts with
`http://localhost:53682/`). "Check and save" tries a write in the folder and restarts the recorder. Nothing changes
in the app: same address, same credentials. Existing recordings stay in the previous destination; "Back to the
recorder folder" shows them again. If the service does not answer at startup, the recorder uses its own folder and
the page says so.

## Notes

- **Address.** `RECORDER_PUBLIC_URL` is the address devices use to reach the recordings: it must work from the
  phone. Away from home this needs a VPN (WireGuard, Tailscale) or a public address.
- **Port 8191.** Anyone who opens it sees the code and manages recordings, settings and destination: do not expose it to the Internet.
- **Playlists.** The recorder downloads the channel's M3U playlist: the machine must reach the provider.
- **Name.** `RECORDER_NAME` is the initial name the app uses for the recorder; the page changes it afterwards.
- **Updates.** `docker compose pull && docker compose up -d`. Recordings and credentials stay in the `data` volume.
