<template>
  <div>
    <textarea v-model="inputValue" @input="handleInput" @keydown="handleKeyDown"></textarea>
    <ul v-if="showSuggestions" class="autocomplete-results">
      <li v-for="contact in suggestions" :key="contact.id" @click="selectContact(contact)">
        {{ contact.name }} ({{ contact.email }})
      </li>
    </ul>
  </div>
</template>

<style>
.autocomplete-results {
    position: absolute;
    z-index: 1;
    list-style-type: none;
    background-color: #fff;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
    border-radius: 4px;
    padding: 0;
    margin: 0;
    max-height: 200px;
    overflow-y: auto;
}

.autocomplete-results li {
    padding: 8px 16px;
    cursor: pointer;
}

.autocomplete-results li:hover {
    background-color: #f1f1f1;
}

.autocomplete-results li:last-child {
    border-bottom-left-radius: 4px;
    border-bottom-right-radius: 4px;
}
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
  mounted() {
    document.addEventListener('input', this.handleInput);
    document.addEventListener('keydown', this.handleKeyDown);
  },
  beforeDestroy() {
    document.removeEventListener('input', this.handleInput);
    document.removeEventListener('keydown', this.handleKeyDown);
  },
  methods: {
    async handleInput() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        const contacts = await response.json();

        const inputText = this.inputValue.trim();
        const words = inputText.split(' ');
        const lastWord = words[words.length - 1];

        if (inputText.includes('@') || lastWord.length >= 3) {
          let filteredContacts = [];

          if (lastWord.startsWith('@')) {
            const username = lastWord.slice(1).toLowerCase();
            filteredContacts = contacts.filter(contact =>
              contact.username.toLowerCase().startsWith(username)
            );
          } else if (lastWord.length >= 3) {
            const searchChars = lastWord.slice(0, 3).toLowerCase();
            filteredContacts = contacts.filter(contact =>
              contact.name.toLowerCase().startsWith(searchChars)
            );
          }

          this.suggestions = filteredContacts;
          this.showSuggestions = true;
        } else {
          this.suggestions = [];
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
    selectContact(contact) {
      console.log('Contact selected:', contact);

      const inputText = this.inputValue.trim();
      const words = inputText.split(' ');
      const lastWord = words[words.length - 1];

      if (lastWord.startsWith('@')) {
        const updatedText = inputText.slice(0, -lastWord.length) + contact.email + ' ';
        this.inputValue = updatedText;
      } else {
        this.inputValue = inputText + ' ' + contact.name;
      }

      this.suggestions = [];
      this.showSuggestions = false;
    },
  },
};
</script>
