---
applyTo: "tests/**"
---
Conventions de tests :
- Utiliser describe/it (pas test())
- Noms de tests en français
- Tester les cas limites : null, undefined, tableau vide, string vide
- Ajouter un test d'intégration si le service appelle un autre service
- Format : describe("NomDuModule", () => { it("devrait faire X quand Y", ...) })
