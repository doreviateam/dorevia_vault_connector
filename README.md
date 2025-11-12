# 🔐 Dorevia Vault Connector

**Version** : 1.0.0  
**Compatibilité** : Odoo 18 CE  
**Auteur** : [Doreviateam](https://github.com/doreviateam)  
**Licence** : AGPL-3 *(à confirmer selon stratégie Open Source)*  

---

## 🎯 Objectif

Le module **`dorevia_vault_connector`** assure la **connexion sécurisée et automatisée** entre **Odoo** et le service **[Dorevia Vault](https://vault.doreviateam.com)** afin que chaque document validé (facture, devis, avoir, etc.) soit :

1. **Vaulté** → transmis et stocké dans le coffre-fort documentaire,  
2. **Signé** → scellé avec empreinte cryptographique (SHA256 + JWS),  
3. **Vérifiable** → consultable et auditable depuis le Ledger Dorevia Vault.

Ce connecteur met en œuvre la règle des **3 V : Validé → Vaulté → Vérifiable**.

---

## 🧩 Architecture Fonctionnelle

| Côté Odoo | Côté Vault |
|------------|------------|
| Module `dorevia_vault_connect` | API `/api/v1/documents` |
| Authentification via Token ou JWS | Vérification et stockage |
| Hook sur `account.move` | Insertion + Ledger |
| Champs `vault_state`, `vault_id` | Preuve d’intégrité JWS |

---

## ⚙️ Installation

```bash
# Depuis ton répertoire addons
cd /opt/odoo_18/projects
git clone https://github.com/doreviateam/dorevia_vault_connector.git
```

Active ensuite le module dans **Applications > Mise à jour de la liste > Installer `Dorevia Vault Connector`**.

---

## 🧠 Configuration

Paramètres système (`Paramètres techniques > Paramètres système`) :

| Clé | Description | Exemple |
|------|--------------|----------|
| `dorevia.vault.url` | URL du service Vault | `https://vault.doreviateam.com` |
| `dorevia.vault.token` | Token d’accès API | `eyJhbGciOiJIUzI1Ni...` |

---

## 🚀 Flux typique (Factur-X)

1. Validation d’une facture (`action_post`)  
2. Génération du PDF + hash SHA256  
3. Envoi du payload JSON + fichier au Vault  
4. Réponse :
   ```json
   {
     "status": "ok",
     "vault_id": "VAULT-XYZ-0001",
     "proof": "JWS_signature"
   }
   ```
5. Mise à jour du statut dans Odoo :
   - `vault_state = vaulted`
   - `vault_id`, `proof_url` enregistrés  

---

## 🧾 Roadmap

| Sprint | Objectif | Statut |
|--------|-----------|--------|
| S1 | Connector Core (Auth + Push Factur-X) | 🔄 En cours |
| S2 | Ledger Feedback + Logs | ⏳ |
| S3 | UI & Cron retry | ⏳ |
| S4 | Packaging + Tests | ⏳ |

---

## 🧱 Structure du module

```
dorevia_vault_connector/
├── __manifest__.py
├── __init__.py
├── models/
│   └── account_move.py
├── controllers/
│   └── main.py
├── data/
│   └── ir_config_parameter.xml
├── security/
│   └── ir.model.access.csv
└── views/
    └── account_move_views.xml
```

---

## 🛠️ Dépendances

- `odoo>=18.0`
- `requests`
- `pyjwt`
- `cryptography`

---

## 📜 Licence
AGPL-3 © 2025 [Doreviateam](https://doreviateam.com)

---

## ❤️ Contributeurs

- **David Baron** — Concepteur & AMOA  
- **Doreviateam** — Équipe technique & intégration  
