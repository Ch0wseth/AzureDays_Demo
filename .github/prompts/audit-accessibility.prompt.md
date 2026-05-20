---
description: "Auditer l'accessibilité d'une page avec Playwright"
---
# Audit Accessibilité

Utilise le navigateur Playwright pour auditer l'accessibilité :

1. Naviguer vers http://localhost:3000
2. Prendre un screenshot
3. Vérifier :
   - Tous les boutons ont un aria-label ou un texte visible
   - Les images ont un attribut alt
   - Le contraste des couleurs est suffisant (ratio 4.5:1 minimum)
   - La navigation au clavier fonctionne (tabindex logique)
   - Les formulaires ont des labels associés
4. Retourner un rapport au format :
   - ✅ Conforme / ❌ Non conforme pour chaque critère
   - Screenshot annoté si possible
