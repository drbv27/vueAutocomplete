<template>
    <div class="autocomplete-container">
        <textarea v-model="inputValue" @input="handleInput" @keydown="handleKeyDown"
            style="width: 100%; height: 300px;" resize="none"></textarea>
        <ul v-if="showSuggestions" class="autocomplete-results">
            <li v-for="contact in suggestions" :key="contact.id">
                <span @click="replaceWith(contact.name)">{{ contact.name }}</span>
                <span class="email" @click="replaceWith(contact.email)">{{ contact.email }}</span>
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
        replaceWith(value) {
            const inputText = this.inputValue.trim();
            const words = inputText.split(' ');
            words.pop(); // Eliminar la última palabra incompleta
            words.push(value); // Agregar el valor seleccionado
            this.inputValue = words.join(' ');

            this.suggestions = [];
            this.showSuggestions = false;
        },
    },
};
</script>

