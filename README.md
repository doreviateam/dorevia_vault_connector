# 🔐 Dorevia Vault Connector

**Version** : 1.0.0  
**Compatibilité** : Odoo 18 CE  
**Auteur** : [Doreviateam](https://github.com/doreviateam)  
**Licence** : AGPL-3

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
