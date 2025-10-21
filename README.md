# Android-Calc - Calculatrice avec Jetpack Compose

## 📱 Objectif du Projet

Créer une application Android de calculatrice fonctionnelle utilisant Jetpack Compose qui effectue les quatre opérations arithmétiques de base :
- ➕ **Addition**
- ➖ **Soustraction** 
- ✖️ **Multiplication**
- ➗ **Division**

Ce projet est conçu pour les étudiants débutants en développement Android souhaitant apprendre les bases de Compose.

---

## 🎯 Prérequis

Avant de commencer, assurez-vous d'avoir installé :

1. **Android Studio** (version Arctic Fox ou ultérieure)
   - Téléchargement : https://developer.android.com/studio
   
2. **JDK** (Java Development Kit) version 11 ou supérieure
   - Généralement inclus avec Android Studio

3. **Connaissances de base** :
   - Langage Kotlin (variables, fonctions, conditions)
   - Concepts de base d'Android (Activity, état)

---

## 🚀 Étape 1 : Créer le Projet Android

### 1.1 Configuration initiale

1. Ouvrez **Android Studio**
2. Cliquez sur **"New Project"**
3. Sélectionnez **"Empty Compose Activity"** (ou "Empty Activity" avec Compose)
4. Configurez votre projet :
   - **Name** : `Android-Calc` ou `CalculatriceCompose`
   - **Package name** : `com.example.calculatrice` (ou votre propre nom)
   - **Language** : Kotlin
   - **Minimum SDK** : API 21 ou supérieur
5. Cliquez sur **"Finish"**

### 1.2 Vérifier les dépendances

Ouvrez le fichier `build.gradle.kts` (Module: app) et vérifiez que vous avez les dépendances Compose :

```kotlin
dependencies {
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.2")
    implementation("androidx.activity:activity-compose:1.8.0")
    implementation(platform("androidx.compose:compose-bom:2023.10.01"))
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.ui:ui-graphics")
    implementation("androidx.compose.ui:ui-tooling-preview")
    implementation("androidx.compose.material3:material3")
}
```

---

## 🏗️ Étape 2 : Architecture de l'Application

### 2.1 Structure du code

Votre calculatrice aura besoin de :

1. **Un état** : pour stocker l'affichage et les valeurs
2. **Une interface utilisateur** : boutons et écran d'affichage
3. **Une logique de calcul** : traiter les opérations

### 2.2 Créer le modèle de données

Créez un fichier `CalculatorState.kt` dans votre package :

```kotlin
package com.example.calculatrice

data class CalculatorState(
    val displayValue: String = "0",
    val operand1: Double? = null,
    val operand2: Double? = null,
    val operation: Operation? = null
)

enum class Operation {
    ADDITION,
    SOUSTRACTION,
    MULTIPLICATION,
    DIVISION
}
```

---

## 🎨 Étape 3 : Créer l'Interface Utilisateur avec Compose

### 3.1 Comprendre la mise en page

Une calculatrice typique contient :
- **Un écran d'affichage** (en haut) : affiche les nombres et résultats
- **Une grille de boutons** (en bas) : 
  - Chiffres 0-9
  - Opérations : +, -, ×, ÷
  - Boutons spéciaux : C (effacer), = (égal)

### 3.2 Code de base pour l'UI

Dans votre fichier `MainActivity.kt`, créez la structure de base :

```kotlin
package com.example.calculatrice

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import androidx.compose.ui.Alignment
import androidx.compose.ui.text.style.TextAlign

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    CalculatorScreen()
                }
            }
        }
    }
}

@Composable
fun CalculatorScreen() {
    var state by remember { mutableStateOf(CalculatorState()) }
    
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        // Écran d'affichage
        DisplayScreen(value = state.displayValue)
        
        // Grille de boutons
        CalculatorButtons(
            onNumberClick = { number -> /* TODO */ },
            onOperationClick = { operation -> /* TODO */ },
            onEqualsClick = { /* TODO */ },
            onClearClick = { state = CalculatorState() }
        )
    }
}

@Composable
fun DisplayScreen(value: String) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .height(120.dp),
        colors = CardDefaults.cardColors(
            containerColor = MaterialTheme.colorScheme.primaryContainer
        )
    ) {
        Box(
            modifier = Modifier.fillMaxSize(),
            contentAlignment = Alignment.CenterEnd
        ) {
            Text(
                text = value,
                fontSize = 48.sp,
                modifier = Modifier.padding(16.dp),
                textAlign = TextAlign.End,
                maxLines = 1
            )
        }
    }
}
```

### 3.3 Créer les boutons de la calculatrice

Ajoutez cette fonction Composable pour la grille de boutons :

```kotlin
@Composable
fun CalculatorButtons(
    onNumberClick: (String) -> Unit,
    onOperationClick: (Operation) -> Unit,
    onEqualsClick: () -> Unit,
    onClearClick: () -> Unit
) {
    Column(
        modifier = Modifier.fillMaxWidth(),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        // Ligne 1 : 7, 8, 9, ÷
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            CalculatorButton("7", Modifier.weight(1f)) { onNumberClick("7") }
            CalculatorButton("8", Modifier.weight(1f)) { onNumberClick("8") }
            CalculatorButton("9", Modifier.weight(1f)) { onNumberClick("9") }
            CalculatorButton("÷", Modifier.weight(1f)) { 
                onOperationClick(Operation.DIVISION) 
            }
        }
        
        // Ligne 2 : 4, 5, 6, ×
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            CalculatorButton("4", Modifier.weight(1f)) { onNumberClick("4") }
            CalculatorButton("5", Modifier.weight(1f)) { onNumberClick("5") }
            CalculatorButton("6", Modifier.weight(1f)) { onNumberClick("6") }
            CalculatorButton("×", Modifier.weight(1f)) { 
                onOperationClick(Operation.MULTIPLICATION) 
            }
        }
        
        // Ligne 3 : 1, 2, 3, -
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            CalculatorButton("1", Modifier.weight(1f)) { onNumberClick("1") }
            CalculatorButton("2", Modifier.weight(1f)) { onNumberClick("2") }
            CalculatorButton("3", Modifier.weight(1f)) { onNumberClick("3") }
            CalculatorButton("-", Modifier.weight(1f)) { 
                onOperationClick(Operation.SOUSTRACTION) 
            }
        }
        
        // Ligne 4 : C, 0, =, +
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            CalculatorButton("C", Modifier.weight(1f)) { onClearClick() }
            CalculatorButton("0", Modifier.weight(1f)) { onNumberClick("0") }
            CalculatorButton("=", Modifier.weight(1f)) { onEqualsClick() }
            CalculatorButton("+", Modifier.weight(1f)) { 
                onOperationClick(Operation.ADDITION) 
            }
        }
    }
}

@Composable
fun CalculatorButton(
    text: String,
    modifier: Modifier = Modifier,
    onClick: () -> Unit
) {
    Button(
        onClick = onClick,
        modifier = modifier.height(80.dp),
        colors = ButtonDefaults.buttonColors(
            containerColor = MaterialTheme.colorScheme.secondary
        )
    ) {
        Text(
            text = text,
            fontSize = 24.sp
        )
    }
}
```

---

## 🧮 Étape 4 : Implémenter la Logique de Calcul

### 4.1 Gérer les clics sur les chiffres

Modifiez la fonction `CalculatorScreen()` pour gérer les événements :

```kotlin
@Composable
fun CalculatorScreen() {
    var state by remember { mutableStateOf(CalculatorState()) }
    
    // Fonction pour gérer les clics sur les chiffres
    fun onNumberClick(number: String) {
        state = state.copy(
            displayValue = if (state.displayValue == "0") {
                number
            } else {
                state.displayValue + number
            }
        )
    }
    
    // Fonction pour gérer les opérations
    fun onOperationClick(operation: Operation) {
        val currentValue = state.displayValue.toDoubleOrNull() ?: 0.0
        state = state.copy(
            operand1 = currentValue,
            operation = operation,
            displayValue = "0"
        )
    }
    
    // Fonction pour calculer le résultat
    fun onEqualsClick() {
        val operand1 = state.operand1 ?: return
        val operand2 = state.displayValue.toDoubleOrNull() ?: return
        val operation = state.operation ?: return
        
        val result = when (operation) {
            Operation.ADDITION -> operand1 + operand2
            Operation.SOUSTRACTION -> operand1 - operand2
            Operation.MULTIPLICATION -> operand1 * operand2
            Operation.DIVISION -> {
                if (operand2 == 0.0) {
                    // Gestion de la division par zéro
                    return
                }
                operand1 / operand2
            }
        }
        
        state = CalculatorState(displayValue = result.toString())
    }
    
    // UI
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp)
    ) {
        DisplayScreen(value = state.displayValue)
        
        CalculatorButtons(
            onNumberClick = { number -> onNumberClick(number) },
            onOperationClick = { operation -> onOperationClick(operation) },
            onEqualsClick = { onEqualsClick() },
            onClearClick = { state = CalculatorState() }
        )
    }
}
```

### 4.2 Améliorer le formatage du résultat

Pour afficher des nombres propres sans décimales inutiles :

```kotlin
fun formatResult(value: Double): String {
    return if (value % 1.0 == 0.0) {
        value.toInt().toString()
    } else {
        String.format("%.2f", value)
    }
}
```

Utilisez cette fonction dans `onEqualsClick()` :

```kotlin
state = CalculatorState(displayValue = formatResult(result))
```

---

## 🧪 Étape 5 : Tester votre Application

### 5.1 Tests de base à effectuer

1. **Test des chiffres** :
   - Cliquez sur les chiffres 0-9
   - Vérifiez qu'ils s'affichent correctement

2. **Test de l'addition** :
   - Tapez `5 + 3 =`
   - Résultat attendu : `8`

3. **Test de la soustraction** :
   - Tapez `10 - 4 =`
   - Résultat attendu : `6`

4. **Test de la multiplication** :
   - Tapez `7 × 6 =`
   - Résultat attendu : `42`

5. **Test de la division** :
   - Tapez `20 ÷ 4 =`
   - Résultat attendu : `5`

6. **Test de la division par zéro** :
   - Tapez `5 ÷ 0 =`
   - L'application ne devrait pas planter

7. **Test du bouton Clear** :
   - Après n'importe quelle opération, cliquez sur `C`
   - L'affichage devrait revenir à `0`

### 5.2 Exécuter l'application

1. Connectez un appareil Android ou lancez un émulateur
2. Cliquez sur le bouton **"Run"** (triangle vert) dans Android Studio
3. Attendez que l'application se compile et s'installe
4. Testez toutes les fonctionnalités

---

## 🎓 Étape 6 : Améliorations Possibles (Facultatif)

Une fois que votre calculatrice de base fonctionne, vous pouvez ajouter :

### 6.1 Fonctionnalités avancées

- **Décimales** : Ajouter un bouton `.` pour les nombres décimaux
- **Pourcentage** : Ajouter une opération de pourcentage
- **Mémoire** : Boutons M+, M-, MR pour stocker des valeurs
- **Historique** : Afficher l'historique des calculs
- **Thème sombre** : Permettre de basculer entre thème clair et sombre

### 6.2 Amélioration de l'UX

```kotlin
// Exemple : Support des décimales
fun onDecimalClick() {
    if (!state.displayValue.contains(".")) {
        state = state.copy(
            displayValue = state.displayValue + "."
        )
    }
}
```

### 6.3 Gestion d'erreurs améliorée

```kotlin
fun onEqualsClick() {
    // ... code existant ...
    
    val result = try {
        when (operation) {
            Operation.DIVISION -> {
                if (operand2 == 0.0) {
                    state = CalculatorState(displayValue = "Erreur")
                    return
                }
                operand1 / operand2
            }
            // ... autres opérations ...
        }
    } catch (e: Exception) {
        state = CalculatorState(displayValue = "Erreur")
        return
    }
    
    state = CalculatorState(displayValue = formatResult(result))
}
```

---

## 📚 Ressources Utiles

### Documentation officielle

- **Jetpack Compose** : https://developer.android.com/jetpack/compose
- **Tutoriels Compose** : https://developer.android.com/courses/pathways/compose
- **Kotlin Documentation** : https://kotlinlang.org/docs/home.html

### Concepts importants à comprendre

1. **State en Compose** : `remember` et `mutableStateOf`
2. **Composables** : Fonctions annotées avec `@Composable`
3. **Layouts** : `Column`, `Row`, `Box`
4. **Material Design 3** : Composants UI modernes

### Déboguer votre application

- Utilisez `Log.d()` pour afficher des messages de débogage :
  ```kotlin
  import android.util.Log
  
  Log.d("Calculator", "Current state: $state")
  ```

- Utilisez les **breakpoints** dans Android Studio pour arrêter l'exécution

---

## ✅ Checklist de Réussite

Avant de considérer votre projet terminé, assurez-vous que :

- [ ] L'application se compile sans erreurs
- [ ] L'interface utilisateur s'affiche correctement
- [ ] L'addition fonctionne correctement
- [ ] La soustraction fonctionne correctement
- [ ] La multiplication fonctionne correctement
- [ ] La division fonctionne correctement
- [ ] La division par zéro est gérée (ne plante pas)
- [ ] Le bouton Clear réinitialise l'affichage
- [ ] Les nombres s'affichent correctement
- [ ] Le résultat est formaté proprement (pas de décimales inutiles)

---

## 🆘 Problèmes Courants et Solutions

### Problème : L'application ne compile pas

**Solution** : Vérifiez que toutes les importations sont présentes :
```kotlin
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
```

### Problème : Les boutons ne répondent pas

**Solution** : Assurez-vous que les fonctions de callback sont bien passées et appelées dans `onClick`

### Problème : L'affichage ne se met pas à jour

**Solution** : Vérifiez que vous utilisez `remember { mutableStateOf() }` et `state.copy()` pour mettre à jour l'état

### Problème : Division par zéro plante l'application

**Solution** : Ajoutez une vérification avant la division :
```kotlin
if (operand2 == 0.0) {
    // Gérer l'erreur
    return
}
```

---

## 🎉 Conclusion

Félicitations ! Vous avez maintenant une calculatrice Android fonctionnelle utilisant Jetpack Compose.

Ce projet vous a permis d'apprendre :
- Les bases de Jetpack Compose
- La gestion d'état avec `State` et `remember`
- La création d'interfaces utilisateur avec des Composables
- L'implémentation de logique métier dans une application Android

N'hésitez pas à personnaliser votre calculatrice et à ajouter vos propres fonctionnalités !

---

## 📝 Licence

Ce projet est destiné à un usage éducatif pour les étudiants débutants en développement Android.