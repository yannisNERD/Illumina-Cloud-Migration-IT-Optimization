### Projet Illumina – Migration Cloud & Optimisation IT

## 📑 Table des matières
1. [Contexte](#-contexte)
2. [Périmètre du Projet](#-périmètre-du-projet)
3. [Analyse de l’Existant](#-analyse-de-lexistant)
4. [Conception de l’Architecture](#-conception-de-larchitecture)
5. [Plan & Mise en Œuvre](#-plan--mise-en-œuvre)
6. [Suivi & Maintenance](#-suivi--maintenance)
7. [Résultats & Bénéfices Attendus](#-résultats--bénéfices-attendus)
8. [Conclusion](#-conclusion)
9. [Structure du Dépôt](#-structure-du-dépôt)

---

## 📌 Contexte

Illumina, leader dans le séquençage génomique, souhaite moderniser et sécuriser son infrastructure informatique afin de soutenir ses activités de recherche et développement en intelligence artificielle. Notre société, SAYA, spécialisée en migration de données et supervision réseau, est chargée de mener ce projet en partenariat avec AWS.

---

## 📦 Périmètre du Projet
Architecture cible intégrant :
- **Équilibreur de charge**
- **VM + outils d’automatisation/gestion**
- **Bases de données**
- **Zone de supervision** (SIEM/CloudWatch)
- **Poste admin sécurisé**
- **VPN & connexion privée**
- **Réseau local segmenté**
- **DNS/TLS**
- **Zabbix, ELK**
- **GitLab CI/CD**
- **Ansible**
- **Sauvegardes & réplication**

---

## 🔍 Analyse de l’Existant
**Constats principaux :**
- **Scalabilité limitée** : ressources statiques, incapacité à absorber les pics de charge.
- **Haute disponibilité non garantie** : absence de redondance et d’équilibrage.
- **Sécurité insuffisante** : segmentation inexistante, chiffrement inégal, IAM faible.
- **Surveillance morcelée** : métriques/logs non centralisés.
- **Peu d’automatisation** : déploiements manuels, CI/CD minimal.

**Objectif :** Remédier à ces limites par une infrastructure sécurisée, évolutive, observable et automatisée.

---


## 🛠 Conception de l’Architecture

### Réseau & Accès
- **VPN OVPN + Connexion privée** : accès administré et chiffré aux environnements, isolation des flux sensibles (admin, sauvegardes, DB).
- **Segmentation réseau (VPC/subnets)** : séparation public/privé, app/data, sauts d’admin contrôlés via bastion.
- **DNS/TLS** : résolution interne/privée + certificats managés ; tout trafic applicatif en TLS.

### Sécurité
- **Périmètre & micro-segmentation** : pare-feu/WAF et règles SG/NACL minimales ; flux deny-by-default.
- **SIEM + CloudWatch** : centralisation logs/événements, corrélation, détection précoce.
- **Poste admin durci** : compte dédié, MFA, sauts contrôlés, sessions auditées.

### Calcul & Orchestration
- **VM + outils d’automatisation** : standardisation des images (bake/AMI), configuration as code avec Ansible, immutable infra sur les briques critiques.
- **Équilibreur de charge** : haute disponibilité, *health checks*.

### Données
- **Bases de données** : services managés/hautes performances, chiffrement au repos/en transit, sauvegardes planifiées, réplication multi-AZ.       

### Observabilité
- **Zabbix** : supervision métriques.
- **ELK** : ingestion et visualisation des logs.
- **CloudWatch** : métriques cloud & alerting.

### Industrialisation
- **GitLab CI/CD** : pipelines build/test/deploy.
- **Ansible** : configuration idempotente.

### Continuité d’activité
- **Sauvegardes & réplications** : politiques 3-2-1, rétention, tests de restauration réguliers ; réplication des serveurs essentiels explicitement recommandée dans la conclusion.

---

## 📋 Plan & Mise en Œuvre

### Étape 1 — Réseau & Sécurité
- **Tâches** : VPC/subnets, routage, SG/NACL ; DNS/TLS ; VPN OVPN ; bastion & poste admin.
- **Livrables** : schéma réseau, PKI/certificats actifs, accès VPN par rôle, runbook d’accès.
- **Succès** : latence conforme, port scans négatifs hors surfaces prévues, accès admin tracé.

### Étape 2 — Observabilité
- **Tâches** : déployer Zabbix (hôtes/agents/templates), ELK (pipelines Logstash, indexation, tableaux) + CloudWatch (métriques/alertes).
- **Livrables** : tableaux de bord disponibilité, règles d’alerting (seuils CPU, mémoire, erreurs 5xx, latence), on-call défini.
- **Succès** : incident simulé détecté < 2 min, alerte reçue par l’équipe SRE.

### Étape 3 — Automatisation & CI/CD
- **Tâches** : Ansible (rôles, playbooks), inventories par env ; GitLab CI/CD (lint, tests, scans SAST/DAST, build artefacts, déploiement).
- **Livrables** : .gitlab-ci.yml, dossiers ansible/ (roles, group_vars, host_vars), release notes automatiques.
- **Succès** : pipeline green < 15 min, rollback one-click, conformité taggée par environnement.

### Étape 4 — Plateforme d’Exécution
- **Tâches** : images VM standardisées, équilibreur de charge, déploiements rolling/blue-green, accès applicatifs via TLS, segmentation réseaux logiques.
- **Livrables** : AMI/Images signées, health checks, runbook de déploiement/rollback.
- **Succès** : indisponibilité < 1 min sur upgrade, error budget respecté.

### Étape 5 — Données & Sauvegardes
- **Tâches** : bases de données (chiffrement, parameter groups), sauvegarde planifiée, réplication multi-AZ, tests de restauration.
- **Livrables** : politiques de rétention, preuves de restauration (RTO/RPO), documentation PRA.
- **Succès** : restauration validée dans le RTO cible ; pertes ≤ RPO.

### Étape 6 — Démonstrations & Validation
- Pipeline CI/CD complet, supervision, accès intranet validés.

---

## 🔄 Suivi & Maintenance
- **Opérations récurrentes** : mises à jour, patching via Ansible, revue des alertes/SLI/SLO, tests PRA trimestriels.
- **Amélioration continue** : post-mortems, capacity planning, optimisation coûts cloud (droitsizing, réservations).
- **Sécurité** : revues IAM périodiques, rotation des secrets, audits de configuration.

---

## 🚀 Résultats & Bénéfices Attendus
**Sécurité renforcée**, **haute disponibilité**, **scalabilité**, **agilité** et **flexibilité** sont les gains clés.
Les recommandations incluent l’**ajout de pare-feu**, la **segmentation** des serveurs/services, l’**intégration d’une solution de supervision** et **la réplication des serveurs essentiels**.
Bénéfices opérationnels : **détection proactive des incidents**, **automatisation** des transferts/déploiements (moins d’erreurs humaines), **réduction des coûts** (paiement à l’usage) et **diminution de la charge de gestion** pour se concentrer sur le cœur métier.

---

## 🏁 Conclusion
La cible proposée transforme l’existant en une plateforme **sécurisée par défaut**, **observable**, **hautement disponible** et **automatisée**. Le socle réseau **(DNS/TLS, VPN, segmentation)**, la **chaîne CI/CD GitLab** + **Ansible**, la **supervision Zabbix/ELK/CloudWatch** et **la stratégie de sauvegarde & réplication** adressent directement les limites actuelles et alignent l’infrastructure sur les exigences du secteur (qualité, performance, traçabilité). Les mesures de **pare-feu + SIEM + segmentation + réplication** énumérées dans la présentation sont centrales pour atteindre ces objectifs, tout en optimisant les coûts via le modèle cloud.

---