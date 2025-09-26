# Cours Détaillé sur Vue 3 avec JavaScript et TypeScript - Mises à Jour de 2025

Bonjour ! Ce cours se concentre sur les directives essentielles de Vue 3, en utilisant JavaScript (JS) et TypeScript (TS). En 2025, Vue 3 reste stable avec des améliorations continues dans l'écosystème (comme les mises à jour de Vite 6, Nuxt 4 et une meilleure intégration TypeScript pour une inférence de types plus précise dans les composants). Cependant, les directives de base comme `v-if`, `v-for`, `v-bind`, `v-on` et `v-model` n'ont pas subi de changements majeurs. Elles bénéficient d'une meilleure support TS pour les props et les états réactifs, rendant le code plus robuste et facile à maintenir dans les grands projets.

Je vais expliquer chaque concept en détail, avec des petits exemples séparés (en JS et TS où pertinent), et des exemples de la vie réelle intégrés dans le même contexte (par exemple, dans une application de gestion de tâches ou un site e-commerce). Les exemples seront courts et isolés pour une meilleure compréhension.

## 9. Directives Essentielles : `v-if`, `v-for`, `v-bind`, `v-on`

Les directives sont des attributs spéciaux dans les templates Vue qui ajoutent du comportement réactif. Elles commencent par `v-` et réagissent aux changements de données.

### Contrôle du Rendu : `v-if`, `v-else`, `v-show`

- **Explication** : `v-if` rend un élément conditionnellement (il est ajouté/supprimé du DOM si la condition est vraie/fausse). `v-else` s'utilise avec `v-if` pour un bloc alternatif. `v-show` cache/montre l'élément via CSS (`display: none`) sans le supprimer du DOM, ce qui est plus performant pour des toggles fréquents. En 2025, avec TS, ces directives bénéficient d'une meilleure vérification des types pour les conditions booléennes.

- **Petit Exemple Séparé en JS** :
  ```vue
  <template>
    <div v-if="isVisible">Contenu visible</div>
    <div v-else>Contenu alternatif</div>
    <div v-show="isVisible">Contenu affiché/masqué</div>
  </template>

  <script>
  export default {
    data() {
      return { isVisible: true };
    }
  };
  </script>
  ```
  Ici, si `isVisible` est `true`, le premier div apparaît ; sinon, le second. Le troisième est toujours dans le DOM mais masqué si `false`.

- **Petit Exemple Séparé en TS** (avec Composition API pour une meilleure intégration TS) :
  ```vue
  <template>
    <div v-if="isVisible">Contenu visible</div>
    <div v-else>Contenu alternatif</div>
    <div v-show="isVisible">Contenu affiché/masqué</div>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  const isVisible = ref<boolean>(true);
  </script>
  ```
  TS assure que `isVisible` est un booléen, évitant les erreurs.

- **Exemple de la Vie Réelle dans le Même Contexte** : Dans une app de gestion de stock e-commerce, `v-if` peut afficher un bouton "Acheter" seulement si le produit est en stock (`v-if="stock > 0"`). Si non, `v-else` montre "Rupture de stock". `v-show` masque une section de détails produit lors d'un toggle rapide (comme un accordion), sans recharger le DOM, pour une meilleure performance sur mobile.

### Listes Réactives : `v-for`

- **Explication** : `v-for` itère sur un tableau ou un objet pour rendre des éléments dynamiquement. Utilisez `:key` pour une performance optimale (Vue tracke les changements). En 2025, avec TS, `v-for` supporte mieux les types génériques pour les listes typées.

- **Petit Exemple Séparé en JS** :
  ```vue
  <template>
    <ul>
      <li v-for="(item, index) in items" :key="index">{{ item }}</li>
    </ul>
  </template>

  <script>
  export default {
    data() {
      return { items: ['Tâche 1', 'Tâche 2'] };
    }
  };
  </script>
  ```
  Cela rend une liste `<li>` pour chaque élément dans `items`.

- **Petit Exemple Séparé en TS** :
  ```vue
  <template>
    <ul>
      <li v-for="(item, index) in items" :key="index">{{ item }}</li>
    </ul>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  const items = ref<string[]>(['Tâche 1', 'Tâche 2']);
  </script>
  ```
  TS définit `items` comme un tableau de strings.

- **Exemple de la Vie Réelle dans le Même Contexte** : Dans l'app e-commerce, `v-for` liste les produits d'un panier (`v-for="product in cart"`). Chaque item montre nom, prix, et un bouton supprimer. Si le panier change (ajout/suppression), la liste se met à jour réactivement, comme sur Amazon où le panier refresh sans recharger la page.

### Liaison Bidirectionnelle : `v-model`

- **Explication** : `v-model` crée une liaison two-way entre un input et une variable data. Elle gère les mises à jour automatiques. En 2025, TS améliore le support pour les modèles typés, comme avec des interfaces.

- **Petit Exemple Séparé en JS** :
  ```vue
  <template>
    <input v-model="message" />
    <p>{{ message }}</p>
  </template>

  <script>
  export default {
    data() {
      return { message: '' };
    }
  };
  </script>
  ```
  Tapez dans l'input, et le paragraphe se met à jour.

- **Petit Exemple Séparé en TS** :
  ```vue
  <template>
    <input v-model="message" />
    <p>{{ message }}</p>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  const message = ref<string>('');
  </script>
  ```
  TS force `message` à être une string.

- **Exemple de la Vie Réelle dans le Même Contexte** : Dans un formulaire de recherche e-commerce, `v-model` lie un input de recherche à une variable `searchQuery`. À mesure que l'utilisateur tape, les résultats filtrés s'affichent en temps réel (combiné avec `v-for`), comme la barre de recherche sur un site comme Etsy.

### Liaison : `v-bind` (ou `:` shorthand)

- **Explication** : `v-bind` lie un attribut HTML à une expression JS/TS. Utile pour classes, styles, src, etc. dynamiques.

- **Petit Exemple Séparé en JS** :
  ```vue
  <template>
    <img :src="imageUrl" />
  </template>

  <script>
  export default {
    data() {
      return { imageUrl: 'https://example.com/image.jpg' };
    }
  };
  </script>
  ```
  L'URL de l'image est dynamique.

- **Petit Exemple Séparé en TS** :
  ```vue
  <template>
    <img :src="imageUrl" />
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  const imageUrl = ref<string>('https://example.com/image.jpg');
  </script>
  ```

- **Exemple de la Vie Réelle dans le Même Contexte** : Dans l'e-commerce, `v-bind` lie une classe CSS pour highlighter un produit en promotion (`:class="{ promo: isOnSale }"`), changeant la couleur si `isOnSale` est true.

### Événements : `v-on` (ou `@` shorthand)

- **Explication** : `v-on` écoute les événements DOM et appelle des méthodes.

- **Petit Exemple Séparé en JS** :
  ```vue
  <template>
    <button @click="increment">Cliquez</button>
    <p>{{ count }}</p>
  </template>

  <script>
  export default {
    data() {
      return { count: 0 };
    },
    methods: { increment() { this.count++; } }
  };
  </script>
  ```

- **Petit Exemple Séparé en TS** :
  ```vue
  <template>
    <button @click="increment">Cliquez</button>
    <p>{{ count }}</p>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  const count = ref<number>(0);
  const increment = () => { count.value++; };
  </script>
  ```

- **Exemple de la Vie Réelle dans le Même Contexte** : Dans le panier e-commerce, `@click` sur un bouton "Ajouter au panier" appelle une méthode qui met à jour le panier.

### Modificateurs d'Événements et de Liaison

- **Explication** : Modificateurs comme `.prevent` pour `v-on` (empêche le comportement par défaut), ou `.lazy` pour `v-model` (met à jour sur change, pas input). Pour `v-bind`, `.camel` convertit en camelCase.

- **Petit Exemple Séparé en JS/TS** (commun) :
  ```vue
  <template>
    <form @submit.prevent="submitForm"> <!-- Empêche refresh page -->
      <input v-model.lazy="name" /> <!-- Met à jour sur change -->
    </form>
  </template>

  <script setup>
  import { ref } from 'vue';
  const name = ref('');
  const submitForm = () => { console.log(name.value); };
  </script>
  ```

- **Exemple de la Vie Réelle dans le Même Contexte** : Dans un formulaire de login e-commerce, `@submit.prevent` évite le refresh, et `v-model.trim` nettoie les espaces blancs du mot de passe.

## Atelier Pratique : Créer une Liste de Tâches Simple

Utilisons tout ça pour une todo list réactive.

- **Code Complet en TS** (utilisant Composition API) :
  ```vue
  <template>
    <div>
      <input v-model="newTask" placeholder="Nouvelle tâche" />
      <button @click="addTask">Ajouter</button>
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          {{ task }}
          <button @click="removeTask(index)">Supprimer</button>
        </li>
      </ul>
      <div v-if="tasks.length === 0">Aucune tâche</div>
      <div v-else>Tâches en cours : {{ tasks.length }}</div>
      <div v-show="showDetails">Détails masqués/affichés</div>
    </div>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue';
  const newTask = ref<string>('');
  const tasks = ref<string[]>([]);
  const showDetails = ref<boolean>(true);

  const addTask = () => {
    if (newTask.value) {
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  };

  const removeTask = (index: number) => {
    tasks.value.splice(index, 1);
  };
  </script>
  ```

- **Explication Pratique** : Ajoutez une tâche via `v-model` et `@click`. Liste avec `v-for`. Condition avec `v-if`/`v-else`. Toggle détails avec `v-show`. Dans la vie réelle, c'est comme une app de productivité (ex: Todoist) pour gérer des tâches quotidiennes, avec réactivité instantanée.

Testez ce code dans un projet Vue 3 ! Si vous avez des questions, demandez.