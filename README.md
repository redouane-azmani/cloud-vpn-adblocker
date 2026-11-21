# Cloud Private VPN & DNS Ad-Blocker (Azure / Terraform / Docker)

Déploiement automatisé d'une passerelle VPN chiffrée (**WireGuard**) couplée à un résolveur DNS filtrant (**AdGuard Home**) et à une stack d'observabilité (**Prometheus + Grafana**) sur Microsoft Azure.

## Architecture
- **Client :** Téléphone, Tablette et PC connectés via tunnel chiffré WireGuard (UDP/51820)
- **Réseau Azure :** VNet `10.0.0.0/16`, sous-réseau `10.0.1.0/24`, pare-feu NSG (ports `22/TCP` et `51820/UDP` uniquement)
- **Conteneurs Docker (réseau privé `172.20.0.0/24`) :**
  - `172.20.0.2` : AdGuard Home (Filtrage DNS)
  - `172.20.0.3` : WireGuard (Passerelle VPN)
  - `172.20.0.4` : Node Exporter (Métriques système)
  - `172.20.0.5` : Prometheus (Collecte des métriques)
  - `172.20.0.6` : Grafana (Tableaux de bord)

## Déploiement
1. **Infrastructure :** `cd terraform && terraform init && terraform apply`
2. **Services :** `cd docker && docker compose up -d`
3. **Clients VPN :** `docker exec -it wireguard /app/show-peer telephone`
