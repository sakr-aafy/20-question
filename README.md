# 📚 Réponses Détaillées - Questions Techniques

Ce document contient les **réponses complètes et détaillées** aux 20 questions techniques.

---

## 🟢 Réponses aux Questions Faciles

### 1. Python - Création de liste
**Compétence** : Programmation - Python

**Question** : En Python, comment créer une liste contenant les nombres 1, 2 et 3 ?

**Réponse** :
```python
ma_liste = [1, 2, 3]
```

---

### 2. JavaScript - Événement clic
**Compétence** : Programmation - JavaScript

**Question** : En JavaScript, comment ajouter un événement clic sur un bouton qui affiche une alerte "Clique !" ?

**Réponse** :
```html
<button id="monBouton">Cliquez-moi</button>
<script>
document.getElementById("monBouton").addEventListener("click", () => {
  alert("Clique !");
});
</script>
```

---

### 3. Java - Classe simple
**Compétence** : Programmation - Java

**Question** : En Java, comment créer une classe simple nommée "Voiture" avec un attribut "marque" de type String ?

**Réponse** :
```java
public class Voiture {
    private String marque;

    public Voiture(String marque) {
        this.marque = marque;
    }
}
```

---

### 4. React.js - Affichage de variable
**Compétence** : Développement Web - Front-end : React.js

**Question** : En React.js, comment afficher une variable "nom" dans un paragraphe avec JSX ?

**Réponse** :
```jsx
function Profil() {
  const nom = "Alice";
  return <p>Bonjour, {nom} !</p>;
}
```

---

### 5. CSS - Texte en gras
**Compétence** : Développement Web - Front-end : CSS

**Question** : En CSS, comment rendre un texte en gras ?

**Réponse** :
```css
.texte-gras {
  font-weight: bold;
}
```
ou simplement `font-weight: 700;`

---

### 6. Node.js - Serveur Express
**Compétence** : Développement Web - Back-end : Node.js

**Question** : En Node.js avec Express, comment démarrer un serveur qui écoute sur le port 3000 ?

**Réponse** :
```javascript
const express = require('express');
const app = express();

app.listen(3000, () => {
  console.log('Serveur démarré sur le port 3000');
});
```

---

### 7. PostgreSQL - Création de table
**Compétence** : Base de données : PostgreSQL

**Question** : En PostgreSQL, comment créer une table "clients" avec une colonne "id" de type entier et une colonne "nom" de type texte ?

**Réponse** :
```sql
CREATE TABLE clients (
    id SERIAL PRIMARY KEY,
    nom TEXT NOT NULL
);
```

---

### 8. FastAPI - Modèle Pydantic
**Compétence** : Machine Learning & Intelligence Artificielle - FastAPI

**Question** : En FastAPI, comment définir un modèle Pydantic simple avec un champ "nom" de type string ?

**Réponse** :
```python
from pydantic import BaseModel

class Utilisateur(BaseModel):
    nom: str
```

---

### 9. Docker - Conteneur Nginx
**Compétence** : DevOps & Outils - Docker

**Question** : En Docker, comment lancer un conteneur Nginx qui expose le port 80 de la machine hôte ?

**Réponse** :
```bash
docker run -d -p 80:80 --name mon-nginx nginx
```

---

### 10. GitHub - Création de branche
**Compétence** : DevOps & Outils - GitHub

**Question** : En GitHub, comment créer une nouvelle branche nommée "feature/login" à partir de "main" ?

**Réponse** :

En ligne de commande :
```bash
git checkout main
git pull
git checkout -b feature/login
```

Ou directement dans l'interface GitHub → Branches → New branch.

---

## 🔴 Réponses aux Questions Difficiles

### 11. TypeScript - Type utilitaire DeepPartial
**Compétence** : Programmation - TypeScript

**Question** : En TypeScript, comment créer un type utilitaire "DeepPartial<T>" qui rend toutes les propriétés d'un objet et de ses sous-objets optionnelles, même imbriqués profondément ?

**Réponse** :
```typescript
type DeepPartial<T> = T extends object
  ? { [K in keyof T]?: DeepPartial<T[K]> }
  : T;
```

Cette implémentation récursive rend chaque niveau de propriétés optionnel.

---

### 12. C# - Pattern AsyncLazy
**Compétence** : Programmation - C#

**Question** : En C#, comment implémenter un pattern "AsyncLazy<T>" qui permet une initialisation asynchrone paresseuse thread-safe sans utiliser de bibliothèque externe ?

**Réponse** :
```csharp
public class AsyncLazy<T>
{
    private readonly Lazy<Task<T>> _lazy;

    public AsyncLazy(Func<Task<T>> factory)
    {
        _lazy = new Lazy<Task<T>>(factory, true); // true = thread-safe
    }

    public Task<T> Value => _lazy.Value;
}
```

**Usage** :
```csharp
var lazy = new AsyncLazy<MyType>(async () => await LoadAsync());
```

---

### 13. Next.js - Middleware d'authentification
**Compétence** : Développement Web - Front-end : Next.js

**Question** : En Next.js 14 (App Router), comment implémenter un middleware qui redirige les utilisateurs non authentifiés vers /login tout en préservant l'URL d'origine et en gérant les routes dynamiques ?

**Réponse** :

Dans `middleware.ts` :
```typescript
import { NextResponse, NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const token = request.cookies.get('token')?.value;
  const protectedPaths = ['/dashboard', '/profile'];

  if (protectedPaths.some(path => request.nextUrl.pathname.startsWith(path)) && !token) {
    const loginUrl = new URL('/login', request.url);
    loginUrl.searchParams.set('callbackUrl', request.nextUrl.pathname);
    return NextResponse.redirect(loginUrl);
  }
  return NextResponse.next();
}
```

---

### 14. Spring Boot - Validation personnalisée
**Compétence** : Développement Web - Back-end : Spring Boot

**Question** : En Spring Boot, comment configurer une validation personnalisée avec un ConstraintValidator qui vérifie qu'une liste contient au moins un élément non null et respecte une règle métier complexe ?

**Réponse** :

Créer une annotation + validator :
```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = NonEmptyListValidator.class)
public @interface NonEmptyList {
    String message() default "La liste ne doit pas être vide";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}

public class NonEmptyListValidator implements ConstraintValidator<NonEmptyList, List<?>> {
    @Override
    public boolean isValid(List<?> list, ConstraintValidatorContext context) {
        return list != null 
            && list.stream().noneMatch(Objects::isNull) 
            && /* règle métier personnalisée */;
    }
}
```

---

### 15. Django - Signal personnalisé avec Celery
**Compétence** : Développement Web - Back-end : Django

**Question** : En Django, comment implémenter un signal personnalisé qui déclenche une tâche Celery asynchrone uniquement si un champ spécifique d'un modèle a changé lors d'une mise à jour ?

**Réponse** :
```python
from django.db.models.signals import pre_save
from django.dispatch import receiver

@receiver(pre_save, sender=MonModele)
def check_champ_modifie(sender, instance, **kwargs):
    if instance.pk:
        old = MonModele.objects.get(pk=instance.pk)
        if old.champ_cible != instance.champ_cible:
            ma_tache_celery.delay(instance.id)
```

---

### 16. MongoDB - Aggregation Pipeline avancée
**Compétence** : Base de données : MongoDB

**Question** : En MongoDB, comment écrire une aggregation pipeline qui groupe des documents par date (jour), calcule une moyenne conditionnelle, et effectue un $lookup uniquement sur les groupes ayant plus de 10 éléments ?

**Réponse** :
```javascript
[
  { 
    $group: {
      _id: { $dateToString: { format: "%Y-%m-%d", date: "$date" } },
      count: { $sum: 1 },
      avgValue: { $avg: { $cond: [{ $gt: ["$value", 0] }, "$value", null] } }
    }
  },
  { 
    $match: { count: { $gt: 10 } } 
  },
  { 
    $lookup: {
      from: "autreCollection",
      localField: "_id",
      foreignField: "dateStr",
      as: "details"
    }
  }
]
```

---

### 17. Llama 3 - Déploiement avec llama.cpp
**Compétence** : Machine Learning & Intelligence Artificielle

**Question** : Comment déployer un modèle Llama 3 quantizé (GGUF) avec llama.cpp dans une API FastAPI, en utilisant le streaming de tokens et en limitant la consommation mémoire via offloading de couches ?

**Réponse** :

Utiliser `llama-cpp-python` avec `n_gpu_layers` pour offloading :
```python
from fastapi import FastAPI
from llama_cpp import Llama
from fastapi.responses import StreamingResponse

app = FastAPI()

llm = Llama(
    model_path="llama3-8b-q4.gguf", 
    n_gpu_layers=20,  # Offloading de 20 couches sur GPU
    n_ctx=4096
)

@app.post("/generate")
async def generate(prompt: str):
    def stream():
        for token in llm(prompt, stream=True):
            yield token["choices"][0]["text"]
    return StreamingResponse(stream(), media_type="text/event-stream")
```

---

### 18. Kubernetes - Ingress avancé
**Compétence** : DevOps & Outils - Kubernetes

**Question** : En Kubernetes, comment configurer un Ingress avec TLS termination, rewrite rules, rate limiting via annotation, et canary deployment basé sur header ?

**Réponse** :

Avec NGINX Ingress :
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: advanced-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/limit-rps: "10"
    nginx.ingress.kubernetes.io/canary: "true"
    nginx.ingress.kubernetes.io/canary-by-header: "x-canary"
    nginx.ingress.kubernetes.io/canary-by-header-value: "true"
spec:
  tls:
  - hosts: 
    - example.com
    secretName: tls-secret
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

---

### 19. GitLab CI/CD - Pipeline dynamique
**Compétence** : DevOps & Outils - GitLab

**Question** : Dans GitLab CI/CD, comment mettre en place un pipeline dynamique (dynamic child pipelines) généré à partir d'un script Python qui détecte les services modifiés et lance uniquement les jobs nécessaires ?

**Réponse** :

Créer un job qui génère un fichier YAML dynamique :
```yaml
generate_pipeline:
  script:
    - python detect_changes.py > generated-pipeline.yml
  artifacts:
    paths: 
      - generated-pipeline.yml

trigger_child:
  trigger:
    include:
      - artifact: generated-pipeline.yml
        job: generate_pipeline
  strategy: depend
```

---

### 20. Angular - Garde de route CanDeactivate
**Compétence** : Développement Web - Front-end : Angular

**Question** : En Angular, comment implémenter un garde de route (CanDeactivate) qui empêche la navigation si un formulaire est modifié, avec confirmation personnalisée via un MatDialog, et gestion des cas de rafraîchissement de page ?

**Réponse** :
```typescript
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { MatDialog } from '@angular/material/dialog';
import { HostListener } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class UnsavedChangesGuard implements CanDeactivate<FormComponent> {
  constructor(private dialog: MatDialog) {}

  async canDeactivate(component: FormComponent): Promise<boolean> {
    if (!component.form.dirty) return true;
    
    const dialogRef = this.dialog.open(ConfirmDialogComponent, {
      data: { message: 'Vous avez des modifications non sauvegardées. Voulez-vous vraiment quitter ?' }
    });
    
    return await dialogRef.afterClosed().toPromise();
  }
}

// Dans le composant FormComponent
export class FormComponent {
  @HostListener('window:beforeunload', ['$event'])
  unloadNotification($event: any): void {
    if (this.form.dirty) {
      $event.returnValue = true;
    }
  }
}
```

---

## 📊 Statistiques

- **Total de questions** : 20
- **Questions faciles** : 10
- **Questions difficiles** : 10
- **Technologies couvertes** : 20+
- **Langages de programmation** : 10+

---

**Créé le** : Janvier 2026  
**Licence** : MIT
