# Android-Calc

## 📱 Projet : Calculatrice Android avec Jetpack Compose

### 🎯 Objectif du projet

Développer une application Android de calculatrice simple et fonctionnelle en utilisant **Jetpack Compose**. Cette calculatrice doit permettre d'effectuer les quatre opérations arithmétiques de base :
- ✅ Addition (+)
- ✅ Soustraction (-)
- ✅ Multiplication (×)
- ✅ Division (÷)

---

## 🚀 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

1. **Android Studio** (version Electric Eel ou supérieure recommandée)
   - Téléchargement : https://developer.android.com/studio

2. **JDK 11 ou supérieur**
   - Généralement inclus avec Android Studio

3. **Connaissances de base** :
   - Kotlin (syntaxe de base, variables, fonctions)
   - Concepts Android (Activity, composables)
   - Notions d'interface utilisateur

---

## 📋 Étapes de développement

### 1️⃣ Configuration du projet

1. **Créer un nouveau projet Android Studio** :
   - Ouvrir Android Studio
   - Sélectionner "New Project"
   - Choisir "Empty Activity" avec Compose
   - Nommer le projet : `AndroidCalc`
   - Package name : `com.example.androidcalc` (ou votre propre package)
   - Language : Kotlin
   - Minimum SDK : API 24 (Android 7.0) ou supérieur
   - Build configuration language : Kotlin DSL

2. **Vérifier les dépendances** :
   
   Dans le fichier `build.gradle.kts` (Module :app), assurez-vous d'avoir :
   ```kotlin
   dependencies {
       implementation("androidx.core:core-ktx:1.12.0")
       implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.7.0")
       implementation("androidx.activity:activity-compose:1.8.2")
       implementation(platform("androidx.compose:compose-bom:2024.02.00"))
       implementation("androidx.compose.ui:ui")
       implementation("androidx.compose.ui:ui-graphics")
       implementation("androidx.compose.ui:ui-tooling-preview")
       implementation("androidx.compose.material3:material3")
   }
   ```

### 2️⃣ Architecture de l'application

Votre calculatrice doit suivre cette structure :

```
app/
├── src/main/java/com/example/androidcalc/
│   ├── MainActivity.kt          # Point d'entrée de l'application
│   ├── CalculatorViewModel.kt   # Logique métier (optionnel mais recommandé)
│   └── ui/
│       ├── CalculatorScreen.kt  # Interface principale
│       └── components/
│           ├── DisplayScreen.kt # Affichage des nombres
│           └── CalculatorButton.kt # Boutons de la calculatrice
```

### 3️⃣ Fonctionnalités à implémenter

#### **Interface utilisateur (UI)**

Votre calculatrice doit avoir :

1. **Un écran d'affichage** :
   - Affiche les nombres saisis
   - Affiche le résultat des calculs
   - Taille de texte adaptative

2. **Des boutons** :
   - Chiffres : 0-9
   - Opérations : +, -, ×, ÷
   - Bouton égal (=) pour calculer le résultat
   - Bouton Clear (C) pour effacer
   - Bouton de suppression (⌫) pour effacer le dernier chiffre (optionnel)

3. **Disposition suggérée** :
   ```
   [  Écran d'affichage  ]
   [  7  ] [  8  ] [  9  ] [ ÷ ]
   [  4  ] [  5  ] [  6  ] [ × ]
   [  1  ] [  2  ] [  3  ] [ - ]
   [  C  ] [  0  ] [  =  ] [ + ]
   ```

#### **Logique de calcul**

Implémentez la logique pour :

1. **Saisie des nombres** :
   - Permettre la saisie de nombres entiers et décimaux
   - Gérer les nombres à plusieurs chiffres

2. **Opérations** :
   - Addition : `a + b`
   - Soustraction : `a - b`
   - Multiplication : `a × b`
   - Division : `a ÷ b`
   - Gérer la division par zéro (afficher un message d'erreur)

3. **Calcul du résultat** :
   - Calculer et afficher le résultat lorsque l'utilisateur appuie sur "="
   - Permettre d'enchaîner plusieurs opérations

4. **Réinitialisation** :
   - Bouton Clear (C) pour tout effacer et recommencer

### 4️⃣ Guide d'implémentation Jetpack Compose

#### **Exemple de structure de base**

```kotlin
@Composable
fun CalculatorScreen() {
    // État de la calculatrice
    var display by remember { mutableStateOf("0") }
    var currentNumber by remember { mutableStateOf("") }
    var operation by remember { mutableStateOf<String?>(null) }
    var previousNumber by remember { mutableStateOf<Double?>(null) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        // Écran d'affichage
        DisplayScreen(display = display)
        
        // Grille de boutons
        CalculatorButtons(
            onNumberClick = { /* Logique pour les chiffres */ },
            onOperationClick = { /* Logique pour les opérations */ },
            onEqualsClick = { /* Logique pour calculer */ },
            onClearClick = { /* Logique pour effacer */ }
        )
    }
}
```

#### **Composable pour un bouton**

```kotlin
@Composable
fun CalculatorButton(
    text: String,
    onClick: () -> Unit,
    modifier: Modifier = Modifier
) {
    Button(
        onClick = onClick,
        modifier = modifier
            .aspectRatio(1f)
            .padding(4.dp)
    ) {
        Text(
            text = text,
            fontSize = 24.sp
        )
    }
}
```

### 5️⃣ Gestion de l'état avec ViewModel (Optionnel mais recommandé)

Pour une meilleure architecture, créez un `CalculatorViewModel` :

```kotlin
class CalculatorViewModel : ViewModel() {
    private val _displayText = mutableStateOf("0")
    val displayText: State<String> = _displayText
    
    fun onNumberClick(number: String) {
        // Logique d'ajout de chiffre
    }
    
    fun onOperationClick(operation: String) {
        // Logique de sélection d'opération
    }
    
    fun onEqualsClick() {
        // Logique de calcul
    }
    
    fun onClearClick() {
        // Logique de réinitialisation
    }
}
```

---

## 🧪 Tests et validation

### Scénarios de test à vérifier :

1. ✅ **Addition simple** : 5 + 3 = 8
2. ✅ **Soustraction** : 10 - 4 = 6
3. ✅ **Multiplication** : 7 × 6 = 42
4. ✅ **Division** : 20 ÷ 4 = 5
5. ✅ **Division par zéro** : 5 ÷ 0 = Erreur
6. ✅ **Opérations enchaînées** : 5 + 3 - 2 = 6
7. ✅ **Nombres décimaux** : 5.5 + 2.3 = 7.8
8. ✅ **Bouton Clear** : Efface tout et remet à 0

### Comment tester :

1. **Lancer l'application** :
   - Connecter un appareil Android ou lancer un émulateur
   - Cliquer sur le bouton "Run" (▶️) dans Android Studio

2. **Tester manuellement** :
   - Essayer tous les scénarios ci-dessus
   - Vérifier que l'interface est réactive
   - S'assurer qu'il n'y a pas de crash

---

## 🎨 Améliorations optionnelles

Une fois la calculatrice de base fonctionnelle, vous pouvez ajouter :

1. **Design amélioré** :
   - Thème sombre/clair
   - Couleurs personnalisées pour les boutons
   - Animations sur les clics

2. **Fonctionnalités avancées** :
   - Bouton de suppression (⌫) pour effacer le dernier chiffre
   - Pourcentage (%)
   - Changement de signe (+/-)
   - Historique des calculs

3. **Gestion des erreurs** :
   - Messages d'erreur personnalisés
   - Limitation du nombre de chiffres
   - Validation des entrées

---

## 📚 Ressources utiles

### Documentation officielle :
- [Jetpack Compose Basics](https://developer.android.com/jetpack/compose/tutorial)
- [State in Compose](https://developer.android.com/jetpack/compose/state)
- [Material Design 3](https://developer.android.com/jetpack/compose/designsystems/material3)

### Tutoriels recommandés :
- [Compose Tutorial - Android Developers](https://developer.android.com/courses/pathways/compose)
- [Kotlin Documentation](https://kotlinlang.org/docs/home.html)

### Concepts clés à comprendre :
- `@Composable` : Annotation pour créer des composants UI
- `remember` : Mémoriser l'état entre les recompositions
- `mutableStateOf` : Créer un état modifiable
- `Column` / `Row` : Disposition verticale/horizontale
- `Modifier` : Personnaliser l'apparence et le comportement

---

## 💡 Conseils pour les débutants

1. **Commencez simple** : 
   - Créez d'abord l'interface avec des boutons statiques
   - Puis ajoutez la logique progressivement

2. **Testez fréquemment** :
   - Lancez l'application après chaque modification importante
   - Utilisez `Log.d()` pour déboguer

3. **Décomposez le problème** :
   - Créez des fonctions séparées pour chaque opération
   - Utilisez des composables réutilisables

4. **Gérez les cas limites** :
   - Division par zéro
   - Nombres très grands
   - Entrées invalides

5. **Demandez de l'aide** :
   - Consultez la documentation
   - Utilisez Stack Overflow
   - Discutez avec vos camarades

---

## 🏁 Critères de réussite

Votre projet sera considéré comme réussi si :

- ✅ L'application compile sans erreur
- ✅ Les quatre opérations de base fonctionnent correctement
- ✅ L'interface est claire et utilisable
- ✅ Le bouton Clear réinitialise la calculatrice
- ✅ La division par zéro est gérée proprement
- ✅ Le code est organisé et commenté
- ✅ L'application ne crash pas pendant l'utilisation

---

## 📝 Livrables attendus

1. **Code source complet** :
   - Tous les fichiers Kotlin nécessaires
   - Fichiers de configuration Gradle
   - Ressources (si applicable)

2. **Documentation** :
   - Commentaires dans le code
   - README avec instructions d'installation (optionnel)

3. **APK** ou **démonstration vidéo** :
   - Fichier APK compilé, ou
   - Vidéo montrant l'application en fonctionnement

---

## 🤝 Contribution et questions

Si vous rencontrez des difficultés ou avez des questions :
- Consultez d'abord la documentation Android
- Recherchez sur Stack Overflow
- Demandez à votre enseignant ou tuteur

Bon courage et bon développement ! 🚀