# Paso 3 - Reglas de firewall por defecto en Google Cloud
# Comando: gcloud compute firewall-rules list
# Usuario: angel@angel-VMware-Virtual-Platform
#
# Puertos abiertos por defecto:
#   - ICMP              ? Ping (diagnóstico de conectividad entre hosts)
#   - TCP 0-65535       ? Todo el tráfico interno entre VMs del mismo proyecto
#   - TCP 3389          ? RDP (escritorio remoto Windows)
#   - TCP 22            ? SSH (conexión remota segura a máquinas virtuales)

NAME                    NETWORK  DIRECTION  PRIORITY  ALLOW                         DENY  DISABLED
default-allow-icmp      default  INGRESS    65534     icmp                                False
default-allow-internal  default  INGRESS    65534     tcp:0-65535,udp:0-65535,icmp        False
default-allow-rdp       default  INGRESS    65534     tcp:3389                            False
default-allow-ssh       default  INGRESS    65534     tcp:22                              False