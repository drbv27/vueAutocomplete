<!-- <template>
    <div class="autocomplete-container">
      <div class="input-container">
        <div
          ref="input"
          class="input"
          contenteditable="true"
          @input="handleInput"
          @keydown="handleKeyDown"
          style="height: 10rem; border: 1px solid #ccc;"
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
  </script> -->
  
<!--   <template>
    <div class="autocomplete-container">
      <div class="input-container">
        <div
          ref="input"
          class="input"
          contenteditable="true"
          @input="handleInput"
          @keydown="handleKeyDown"
          style="height: 10rem; border: 1px solid #ccc;"
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
      replaceWith(name) {
        const inputText = this.$refs.input.innerText.trim();
        const words = inputText.split(' ');
        words[words.length - 1] = name;
        const updatedText = words.join(' ');
  
        this.$refs.input.innerText = updatedText;
  
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
  </script> -->
  



  <template>
    <div class="autocomplete-container">
      <div class="input-container">
        <div
          ref="input"
          class="input"
          contenteditable="true"
          @input="handleInput"
          @keydown="handleKeyDown"
          style="height: 10rem; border: 1px solid #ccc;padding: 10px;"
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

.input-container {
    padding: 10px;
}

.autocomplete-container textarea {
    width: 100%;
    height: 300px;
    resize: none;
    border: none;
    outline: none;
}

.autocomplete-container textarea:focus,
.autocomplete-container:focus-within textarea {
    outline: none; /* Elimina el contorno en estados de enfoque */
    box-shadow: none; /* Elimina cualquier sombra al enfocar el textarea */
}

.autocomplete-results {
    position: absolute;
    top: 10%;
    left: 0;
    z-index: 1;
    list-style-type: none;
    background-color: #fff;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
    border-radius: 4px;
    padding: 0;
    margin: 0;
    max-height: 200px;
    overflow-y: auto;
    font-family: sans-serif;
    font-size: 12px;
    width: 25%;
}

.autocomplete-results li {
    padding: 8px 16px;
    cursor: pointer;
    display: flex;
    align-items: center;
}

.autocomplete-results li:hover {
    background-color: #f1f1f1;
}

.autocomplete-results li:last-child {
    border-bottom-left-radius: 4px;
    border-bottom-right-radius: 4px;
}

.autocomplete-results li .email {
    margin-left: 8px;
    font-size: 10px;
    font-style: italic;
    color: #999;
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
        const inputText = this.$refs.input.innerText.trim();
        const words = inputText.split(' ');
        words[words.length - 1] = `<a href="mailto:${email}" style="background-color: #bebfc5;padding:5px;border-radius:8px; text-decoration: none; color: #fff;">${name}</a>`;
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




<!--   <template>
    <div class="autocomplete-container">
      <div class="input-container">
        <div
          ref="input"
          class="input"
          contenteditable="true"
          @input="handleInput"
          @keydown="handleKeyDown"
          @paste="handlePaste"
          @mouseup="handleMouseUp"
          style="height: 10rem; border: 1px solid #ccc;"
        ></div>
      </div>
      <ul v-if="showSuggestions" class="autocomplete-results">
        <li v-for="contact in suggestions" :key="contact.id">
          <a
            @click.prevent="replaceWith(contact.name, contact.email)"
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
  /* Estilos CSS aquí */
  </style>
  
  <script>
  export default {
    data() {
      return {
        inputValue: '',
        suggestions: [],
        showSuggestions: false,
        links: [],
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
      handlePaste(event) {
        event.preventDefault();
        const text = event.clipboardData.getData('text/plain');
        document.execCommand('insertText', false, text);
      },
      handleMouseUp(event) {
        const selectedText = window.getSelection().toString();
        if (selectedText && this.links.includes(selectedText)) {
          event.preventDefault();
          event.stopPropagation();
        }
      },
      replaceWith(name, email) {
        const range = window.getSelection().getRangeAt(0);
        const link = document.createElement('a');
        link.href = `mailto:${email}`;
        link.textContent = name;
        range.deleteContents();
        range.insertNode(link);
  
        this.links.push(name);
  
        this.$refs.input.focus();
        this.$refs.input.dispatchEvent(new Event('input'));
  
        this.suggestions = [];
        this.showSuggestions = false;
      },
    },
  };
  </script> -->
  



  
  
  
  
  