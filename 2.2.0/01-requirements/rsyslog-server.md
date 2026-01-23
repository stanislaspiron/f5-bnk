# External Rsyslog server deployment

## Requirements
My NFS server is deployed on Ubuntu 22.04
- 2 vCPU
-  2GB RAM

## RSyslog Installation

```bash
sudo apt install rsyslog -y
```

## RSyslog directory
Create **/var/log/remote** directory and change directory owner and access

```bash
sudo mkdir /var/log/remote
sudo chown syslog:adm /var/log/remote
sudo chmod 755 /var/log/remote
```


## RSyslog configuration for per-ip log file

Create the rsyslog tcp configuration file
```bash
cat <<EOF | sudo tee /etc/rsyslog.d/51-tcp-listener.conf > /dev/null
# Charge les modules nécessaires
module(load="imtcp")    # Entrées TCP
module(load="imuxsock") # Logs système locaux (par défaut)

# Active l’écoute sur le port 514 TCP
input(type="imtcp" port="514")


# Template pour nommer le fichier selon l'IP source
template(name="PerHostLogFile" type="string"
         string="/var/log/remote/%fromhost-ip%.log")

# Règle générale : tout ce qui vient d’un hôte distant → fichier dédié
if $fromhost-ip != "127.0.0.1" then {
        action(type="omfile" dynaFile="PerHostLogFile")
        stop
}
EOF
```

## Restart rsyslog

```bash
sudo systemctl restart rsyslog
```
