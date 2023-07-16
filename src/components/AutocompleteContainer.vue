<template>
    <div class="autocomplete-container">
      <div class="input-container">
        <div
          ref="input"
          class="input"
          contenteditable="true"
          @input="handleInput"
          @keydown="handleKeyDown"
        ></div>
      </div>
      <ul v-if="showSuggestions" class="autocomplete-results">
        <li v-for="contact in suggestions" :key="contact.id">
          <a
            @click="replaceWith(contact.name, contact.email)"
            href="#"
            class="contact-link"
          >
            {{ contact.name }}
          </a>
          <span class="email">{{ contact.email }}</span>
        </li>
      </ul>
    </div>
  </template>
  
  <style>
    .autocomplete-container {
        position: relative;
        width: 100%;
        height: 300px;
        resize: none !important;
    }
  /* Estilos CSS aquí */
  </style>
  
  <script>
  export default {
    data() {
      return {
        inputValue: '',
        suggestions: [],
        showSuggestions: false,
      };
    },
    methods: {
      async handleInput() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          const contacts = await response.json();
  
          const inputText = this.$refs.input.innerText.trim();
          const words = inputText.split(' ');
          const lastWord = words[words.length - 1];
  
          if (inputText.includes('@') || lastWord.length >= 3) {
            let filteredContacts = [];
  
            if (lastWord.startsWith('@')) {
              const username = lastWord.slice(1).toLowerCase();
              filteredContacts = contacts.filter((contact) =>
                contact.username.toLowerCase().startsWith(username)
              );
            } else if (lastWord.length >= 3) {
              const searchChars = lastWord.slice(0, 3).toLowerCase();
              filteredContacts = contacts.filter((contact) =>
                contact.name.toLowerCase().startsWith(searchChars) ||
                contact.email.toLowerCase().startsWith(searchChars)
              );
            }
  
            this.suggestions = filteredContacts;
            this.showSuggestions = true;
          } else {
            this.suggestions = [];
            this.showSuggestions = false;
          }
  
          if (lastWord.length === 4 && this.suggestions.length === 0) {
            this.showSuggestions = false;
          }
        } catch (error) {
          console.error('Error fetching contacts:', error);
        }
      },
      handleKeyDown(event) {
        if (event.key === 'Escape') {
          this.suggestions = [];
          this.showSuggestions = false;
        }
      },
      replaceWith(name, email) {
        const range = document.getSelection().getRangeAt(0);
        const mailtoLink = document.createElement('a');
        mailtoLink.href = 'mailto:' + email;
        mailtoLink.appendChild(document.createTextNode(name));
        range.deleteContents();
        range.insertNode(mailtoLink);
  
        this.$refs.input.focus();
        this.$refs.input.dispatchEvent(new Event('input'));
  
        this.suggestions = [];
        this.showSuggestions = false;
      },
    },
  };
  </script>
  

