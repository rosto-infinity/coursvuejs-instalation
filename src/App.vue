<template>
  <div class="multi-step-form">
    <h2>Étape {{ currentStep }} sur 3</h2>

    <!-- ÉTAPE 1 : Infos perso -->
    <div v-if="currentStep === 1">
      <h3>Informations personnelles</h3>
      <div>
        <label>Nom :</label>
        <input v-model="formData.name" type="text" placeholder="Votre nom" />
        <p v-if="errors.name" class="error">{{ errors.name }}</p>
      </div>
      <div>
        <label>Âge :</label>
        <input v-model.number="formData.age" type="number" min="13" />
        <p v-if="errors.age" class="error">{{ errors.age }}</p>
      </div>
    </div>

    <!-- ÉTAPE 2 : Coordonnées -->
    <div v-else-if="currentStep === 2">
      <h3>Coordonnées</h3>
      <div>
        <label>Email :</label>
        <input v-model="formData.email" type="email" placeholder="email@exemple.com" />
        <p v-if="errors.email" class="error">{{ errors.email }}</p>
      </div>
      <div>
        <label>Téléphone (optionnel) :</label>
        <input v-model="formData.phone" type="tel" placeholder="06 12 34 56 78" />
      </div>
    </div>

    <!-- ÉTAPE 3 : Résumé -->
    <div v-else-if="currentStep === 3">
      <h3>Résumé de votre profil</h3>
      <ul>
        <li><strong>Nom :</strong> {{ formData.name }}</li>
        <li><strong>Âge :</strong> {{ formData.age }}</li>
        <li><strong>Email :</strong> {{ formData.email }}</li>
        <li v-if="formData.phone"><strong>Téléphone :</strong> {{ formData.phone }}</li>
      </ul>
    </div>

    <!-- Boutons de navigation -->
    <div class="form-actions">
      <button 
        v-if="currentStep > 1" 
        @click="currentStep--"
      >
        Précédent
      </button>
      <button 
        v-if="currentStep < 3" 
        @click="goToNextStep"
        :disabled="!isCurrentStepValid"
      >
        Suivant
      </button>
      <button 
        v-else 
        @click="submitForm"
      >
        Soumettre
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

// Typage des données
interface FormData {
  name: string;
  age: number | null;
  email: string;
  phone: string;
}

// Données initiales
const formData = ref<FormData>({
  name: '',
  age: null,
  email: '',
  phone: ''
});

const currentStep = ref<number>(1);
const errors = ref<Record<string, string>>({});

// Validation par étape
const validateStep = (step: number): boolean => {
  errors.value = {};

  if (step === 1) {
    if (!formData.value.name.trim()) {
      errors.value.name = 'Le nom est requis.';
      return false;
    }
    if (!formData.value.age || formData.value.age < 13) {
      errors.value.age = 'Vous devez avoir au moins 13 ans.';
      return false;
    }
  }

  if (step === 2) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(formData.value.email)) {
      errors.value.email = 'Email invalide.';
      return false;
    }
  }

  return true; // valide
};

// Computed : est-ce que l'étape courante est valide ?
const isCurrentStepValid = computed(() => {
  // On ne bloque pas l'étape 3 (résumé)
  if (currentStep.value === 3) return true;
  return validateStep(currentStep.value);
});

// Passer à l'étape suivante (avec validation)
const goToNextStep = () => {
  if (validateStep(currentStep.value)) {
    currentStep.value++;
  }
};

const submitForm = () => {
  alert('✅ Profil créé avec succès !');
  console.log('Données finales :', formData.value);
};
</script>

<style scoped>
.error {
  color: red;
  font-size: 0.875em;
  margin-top: 4px;
}
.form-actions {
  margin-top: 20px;
}
.form-actions button {
  margin-right: 10px;
  padding: 8px 16px;
}
</style>