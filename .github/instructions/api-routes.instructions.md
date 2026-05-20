---
applyTo: "src/routes/**"
---
Conventions pour les routes API :
- Toujours valider les entrées en début de handler
- Format réponse succès : { data: ... }
- Format réponse erreur : { error: "message explicite" }
- Status codes : 200 (ok), 201 (created), 400 (bad input), 404 (not found), 500 (server error)
- Toujours wrapper dans try/catch
- Logger les erreurs avec console.error avant de les retourner
