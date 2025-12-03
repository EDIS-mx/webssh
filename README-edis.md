# WebSSH con Token Routing

Fork con soporte para `--hosts` y autenticación por token server-side.

## Instalacion

```bash
python3 -m venv venv
venv/bin/pip install -e .
```

## Hosts file

```
# token hostname port username keypath passphrase
sol          10.11.0.1   22 admin /etc/webssh/keys/id_ed25519_sol   mipassphrase
minsbreakout 10.11.0.202 22 root  /etc/webssh/keys/id_ed25519_inwow otrapassphrase
```

## Generar keys

```bash
sudo mkdir -p /etc/webssh/keys
sudo ssh-keygen -t ed25519 -f /etc/webssh/keys/id_ed25519_sol -N "mipassphrase"
sudo chown $USER /etc/webssh/keys/*
```

Copiar pubkey al dispositivo:
```bash
cat /etc/webssh/keys/id_ed25519_sol.pub | ssh admin@10.11.0.1 "cat >> ~/.ssh/authorized_keys"
```

## Uso

```bash
venv/bin/wssh --hosts=./webssh-hosts --address=127.0.0.1 --port=8022
```

## Arquitectura

```
Browser (xterm.js) -> nginx (secure_link) -> webssh (token) -> device:22
```
