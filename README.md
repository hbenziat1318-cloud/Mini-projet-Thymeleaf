# Mini-projet-Thymeleaf
[DOSIER DU TP](https://drive.google.com/drive/u/0/home)
# DOCUMENTATION DU PROJET - GESTION IMMOBILIÈRE

## 1. Description du projet

### Contexte fonctionnel
Application de **gestion locative immobilière** permettant aux agences immobilières de gérer leur parc de biens, leurs locataires et les contrats de location.

### Objectif de l'application
Digitaliser et simplifier la gestion des locations immobilières avec un système complet de suivi des biens, locataires et contrats.

### Public cible / cas d'usage
- **Agences immobilières**
- **Propriétaires bailleurs**
- **Gestionnaires de patrimoine immobilier**

### Ce que l'application permet concrètement
"Une application web complète permettant de gérer un parc immobilier locatif, du suivi des biens à la gestion des baux en passant par le suivi des locataires."

---

## 2. Architecture technique

### 2.1 Stack technologique
- **Backend** : Spring Boot 3.2.0, Spring Data JPA/Hibernate 6.3.1
- **Frontend** : Thymeleaf 3.1.2, HTML5, CSS3, Bootstrap 5.1.3
- **Base de données** : MySQL 8.0 (Développement) 
- **Build** : Maven 3.9+
- **Java** : JDK 17

### 2.2 Structure du code
```
gestion-immobilier/
├── src/main/java/com/gestionimmobilier/
│   ├── GestionImmobilierApplication.java
│   ├── controller/
│   │   ├── BienController.java
│   │   ├── LocataireController.java
│   │   ├── BailController.java
│   │   └── DashboardController.java
│   ├── entity/
│   │   ├── Bien.java
│   │   ├── Locataire.java
│   │   └── Bail.java
│   ├── repository/
│   │   ├── BienRepository.java
│   │   ├── LocataireRepository.java
│   │   └── BailRepository.java
│   ├── service/
│   │   ├── BienService.java
│   │   ├── LocataireService.java
│   │   ├── BailService.java
│   │   └── StatistiqueService.java
│   └── dto/
│       └── StatistiqueDto.java
├── src/main/resources/
│   ├── templates/
│   │   ├── biens/
│   │   ├── locataires/
│   │   ├── baux/
│   │   └── dashboard/
│   ├── static/
│   │   ├── css/
│   │   ├── js/
│   │   └── images/
│   └── application.properties
└── pom.xml
```

### 2.3 Diagramme d'architecture
```
┌─────────┐    HTTP    ┌─────────────┐    Appel    ┌──────────┐    JPA    ┌────────────┐
│Navigateur│───────────▶│ Controller  │─────────────▶│ Service  │───────────▶│ Repository │
└─────────┘   Request  └─────────────┘   Métier    └──────────┘   Query   └────────────┘
     │                                           │                          │
     │ Response                                  │                          │ DB Access
     │ Thymeleaf                                 │ Result                   │
     ▼                                           ▼                          ▼
┌─────────┐    Model    ┌─────────────┐   Return   ┌──────────┐   Entity   ┌────────────┐
│  Vue    │◄────────────│ Controller  │◄───────────│ Service  │◄───────────│ Repository │
│Thymeleaf│             └─────────────┘   Object   └──────────┘   Object   └────────────┘
└─────────┘                                                               │
                                                                          ▼
                                                                   ┌────────────┐
                                                                   │  MySQL /   │
                                                                   │    │
                                                                   └────────────┘
```

---

## 3. Fonctionnalités principales

### CRUD Complet
- **Biens** : Ajouter, modifier, supprimer, consulter les propriétés
- **Locataires** : Gestion complète des profils locataires
- **Baux** : Création, résiliation, consultation des contrats

### Recherche & Filtrage
- **Biens** : Par ville, type, disponibilité
- **Locataires** : Par nom, CIN
- **Baux** : Par statut, période

### Tableau de bord & Statistiques
- **Taux d'occupation** : Pourcentage de biens loués
- **Chiffre d'affaires** : Loyers mensuels perçus
- **Indicateurs clés** : Biens disponibles, baux actifs

### Gestion des statuts
- **Biens** : Disponible / Non disponible
- **Baux** : ACTIF / TERMINE / RESILIE

---

## 4. Modèle de données

### 4.1 Entités principales

#### Bien
```java
- id: Long
- reference: String (unique)
- ville: String
- type: String (APPARTEMENT, MAISON, BUREAU)
- loyerMensuel: BigDecimal
- disponible: Boolean
```

#### Locataire
```java
- id: Long
- nom: String
- cin: String (unique)
- revenu: BigDecimal
- email: String
- telephone: String
```

#### Bail
```java
- id: Long
- dateDebut: LocalDate
- dateFin: LocalDate
- statut: String (ACTIF, TERMINE, RESILIE)
- depotGarantie: BigDecimal
- bien: Bien (@ManyToOne)
- locataire: Locataire (@ManyToOne)
```

### 4.2 Relations
- **Bail → Bien** : `@ManyToOne` (Un bail concerne un bien)
- **Bail → Locataire** : `@ManyToOne` (Un bail concerne un locataire)
- **Bien → Bail** : `@OneToMany` (Un bien peut avoir plusieurs baux dans le temps)

### 4.3 Configuration base de données
```properties
# MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/gestion_immobilier
spring.datasource.username=root
spring.datasource.password=

# H2 (Alternative)
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=

# Génération automatique des tables
spring.jpa.hibernate.ddl-auto=update
```

---

## 5. Lancer le projet

### 5.1 Prérequis
- **Java** : JDK 17 ou supérieur
- **Maven** : 3.9+
- **MySQL** : 8.0+ (optionnel)
- **Navigateur web** moderne

### 5.2 Installation
```bash
# Cloner le projet
git clone https://github.com/votre-username/gestion-immobilier.git

# Se positionner dans le dossier
cd gestion-immobilier

# Lancer l'application
mvn spring-boot:run

# Ou compiler d'abord
mvn clean compile
mvn spring-boot:run
```

### 5.3 Accès à l'application
- **Application principale** : http://localhost:8080
- **Tableau de bord** : http://localhost:8080/
- **Gestion des biens** : http://localhost:8080/biens
- **Gestion des locataires** : http://localhost:8080/locataires
- **Gestion des baux** : http://localhost:8080/baux

---

## 6. Jeu de données initial

### Chargement automatique via `DataLoader.java`
```java
@Component
public class DataLoader implements CommandLineRunner {
    // Données de test créées au démarrage
}
```

### Exemples de données
```sql
-- Biens
INSERT INTO biens (reference, ville, type, loyer_mensuel, disponible) VALUES
('APP-PAR-001', 'Paris', 'APPARTEMENT', 750.00, true),
('MAI-LYO-001', 'Lyon', 'MAISON', 1200.00, true);

-- Locataires  
INSERT INTO locataires (nom, cin, revenu) VALUES
('Dupont Martin', 'AB123456', 2500.00),
('Bernard Sophie', 'CD789012', 3000.00);

-- Baux
INSERT INTO baux (date_debut, date_fin, statut, depot_garantie, bien_id, locataire_id) VALUES
('2024-01-01', '2024-12-31', 'ACTIF', 750.00, 1, 1);
```

---

## 7. Démonstration Vidéo

### Lien vers la démonstration
📹 **[(https://drive.google.com/drive/folders/11To6nvQhSndndzdvqRfC7aIOafdstVbF?usp=sharing)]** 

### Contenu de la démonstration
1. **Navigation générale** dans l'application
2. **Création d'un bien immobilier**
3. **Ajout d'un locataire**
4. **Création d'un bail de location**
5. **Recherche et filtrage** des biens par ville
6. **Consultation du tableau de bord** avec statistiques
7. **Visualisation des baux actifs**

---

## 8. Auteurs /Encadrement

### Étudiant
- **Nom** : [BENZIAT Hana]
- **Email** : h.benziat1318@uca.ac.ma
- 
### Encadrement
- **Module** : Développement d'Applications avec Spring Boot
- **Encadré par:** Mr.lachgar
- **Établissement** : Ens-Marrakech
- **Année académique** : 2025-2026

### Technologies maîtrisées
- ✅ Spring Boot & Spring MVC
- ✅ Spring Data JPA & Hibernate
- ✅ Thymeleaf & Bootstrap
- ✅ Architecture MVC & Design Patterns
- ✅ Gestion de bases de données relationnelles


---
