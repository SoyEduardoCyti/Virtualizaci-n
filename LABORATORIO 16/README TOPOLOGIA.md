# Lab 16 - Docker Networking
## Topología de Red

┌─────────────────────────────────────────────┐
│              HOST (Ubuntu VM)               │
│                                             │
│  ┌──────────────────────────────────────┐   │
│  │        red-negocio (bridge)          │   │
│  │         172.18.0.0/16                │   │
│  │                                      │   │
│  │  ┌─────────────┐  ┌──────────────┐   │   │
│  │  │   mi-db     │  │   alpine     │   │   │
│  │  │  (MariaDB)  │  │  (Frontend)  │   │   │
│  │  │ 172.18.0.2  │  │ 172.18.0.3   │   │   │
│  │  └──────┬──────┘  └──────┬───────┘   │   │
│  │         └────────────────┘           │   │
│  │              ping mi-db ✅          │   │
│  │         (DNS embebido Docker)        │   │
│  └──────────────────────────────────────┘   │
│                    │ NAT                    │
└────────────────────┼────────────────────────┘
                     │
                  Internet

## Conceptos aplicados
- Red SDN personalizada con driver bridge
- Service Discovery por nombre de contenedor
- DNS embebido de Docker
- Aislamiento de red entre contenedores