<template>
  <div class="app-container">
  <div class="autocomplete-container">
    <div
      ref="input"
      class="input"
      contenteditable="true"
      @input="handleInput"
      @keydown="handleKeyDown"
      style="height: 20rem; border: 1px solid #ccc;padding: 10px; white-space: pre-line;"
    ></div>
    <ul v-if="showSuggestions" class="autocomplete-results">
      <li v-for="contact in suggestions" :key="contact.id">
        <a
          @click="replaceWith(contact.name, contact.email)"
          href="#"
          class="contact-link"
        >
          <span class="pill">
            <img
              src="https://peaku.co/img/logo-blue.png"
              alt="Logo"
              class="avatar"
            />
            <span class="name">{{ contact.name }}</span>
          </span>
        </a>
        <span class="email">{{ contact.email }}</span>
      </li>
    </ul>
  </div>
  </div>
</template>

<script>
import params from '../data/data.js';

export default {
  data() {
    return {
      inputValue: '',
      suggestions: [],
      showSuggestions: false,
    };
  },
  mounted() {
    this.loadTemplate(); // Carga la plantilla automáticamente al montar el componente
  },
  methods: {
    loadTemplate() {
      const candidateName = params.candidate_name;
      const job = params.job;
      const businessPhone = params.logged_in_bussiness_phone;
      const businessEmail = params.logged_in_bussiness_email;
      const offerUrl = params.offer_url;

      const template = `Hola <a href="#" class="pill">
          <img src="https://peaku.co/img/logo-blue.png" alt="Logo" class="avatar" />
          <span class="name">${candidateName}</span>
        </a>,\n\nQuiero invitarte a una entrevista para la vacante '${job}'.\nContactame en:\n  telefono: '${businessPhone}'\n  email: '${businessEmail}'\n\nAdemás, puedes ver más info de la vacante <a href="${offerUrl}">${job}</a>.\n\nSaludos.`;

      this.$refs.input.innerHTML = template;
    },
    handleInput() {
      // No se necesita verificación de longitud, esta función carga la plantilla automáticamente
      // Resto del código del método handleInput...
    },
    handleKeyDown(event) {
      if (event.key === 'Escape') {
        this.suggestions = [];
        this.showSuggestions = false;
      }
    },
    replaceWith(name, email) {
      const inputText = this.$refs.input.innerText.trim();
      const words = inputText.split(' ');
      words[words.length - 1] = `<a href="mailto:${email}" style="background-color: #bebfc5;padding:5px;border-radius:8px; text-decoration: none; color: #fff;">
          <img src="https://peaku.co/img/logo-blue.png" alt="Logo" class="avatar" />
          <span class="name">${name}</span>
        </a>`;
      const updatedText = words.join(' ');

      this.$refs.input.innerHTML = updatedText;

      const range = document.createRange();
      range.selectNodeContents(this.$refs.input);
      range.collapse(false);
      const selection = window.getSelection();
      selection.removeAllRanges();
      selection.addRange(range);

      this.$refs.input.focus();
      this.$refs.input.dispatchEvent(new Event('input'));

      this.suggestions = [];
      this.showSuggestions = false;
    },
  },
};
</script>

<style>
.app-container {
  /* Ajusta la altura del contenedor de la aplicación según tu preferencia */
  height: 700px; /* Por ejemplo, aquí se establece una altura de 400px */
}

.autocomplete-container {
  position: relative;
  width: 100%;
  height: 100%; /* Hacemos que ocupe toda la altura disponible del contenedor de la aplicación */
  resize: none !important;
}
.pill {
  display: inline-flex;
  align-items: center;
  background-color: #e0e0e0;
  border-radius: 20px;
  padding: 8px 12px; 
  margin: 0 2px;
  color: #333;
  font-size: 14px; 
  text-decoration: none;
}

.avatar {
  width: 20px;
  height: 20px;
  margin-right: 4px;
  border-radius: 50%;
}

.contact-link:hover .pill {
  background-color: #ccc;
}

/* Estilos para los hiperenlaces */
a {
  cursor: pointer; /* Cambiamos el cursor a "pointer" para indicar que el enlace es clickeable */
  text-decoration: none; /* Quitamos el subrayado por defecto de los hiperenlaces */
}

/* Estilos para los hiperenlaces al pasar el mouse por encima */
a:hover {
  text-decoration: underline; /* Agregamos un subrayado al pasar el mouse por encima del enlace */
}
/* Estilos de tu componente */
</style>











  
  
  
  
  