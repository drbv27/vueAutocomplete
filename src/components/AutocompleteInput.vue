<template>
    <div class="email-compose">
        <div class="email-recipients">
            <div v-for="(recipient, index) in recipients" :key="index" class="recipient">
                <div class="recipient-avatar">
                    <img :src="recipient.avatar" alt="Recipient Avatar" />
                </div>
                <div class="recipient-details">
                    <div class="recipient-name">{{ recipient.name }}</div>
                    <div class="recipient-email">{{ recipient.email }}</div>
                </div>
                <div class="recipient-remove" @click="removeRecipient(index)">
                    x
                </div>
            </div>
            <input v-model="inputValue" @input="handleInput" @keydown="handleKeyDown" ref="recipientInput"
                placeholder="Add recipients..." class="recipient-input" />
            <ul v-if="showSuggestions" class="autocomplete-results">
                <li v-for="contact in suggestions" :key="contact.id">
                    <div class="suggestion-item" @click="replaceWith(contact)">
                        <div class="suggestion-avatar">
                            <img :src="getAvatarUrl(contact)" alt="Suggestion Avatar" />
                        </div>
                        <div class="suggestion-details">
                            <div class="suggestion-name">{{ contact.name }}</div>
                            <div class="suggestion-email">{{ contact.email }}</div>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
        <div class="email-body">
            <AutocompleteContainer></AutocompleteContainer>
        </div>
    </div>
</template>

<style>
.email-compose {
    display: flex;
    flex-direction: column;
    width: 80%;
    margin: auto;
}

.email-recipients {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.recipient {
    display: flex;
    align-items: center;
    background-color: #f1f1f1;
    border-radius: 4px;
    padding: 4px;
    margin-right: 4px;
    margin-bottom: 4px;
}

.recipient-avatar,
.suggestion-avatar {
    width: 24px;
    height: 24px;
    margin-right: 4px;
}

.recipient-details,
.suggestion-details {
    display: flex;
    flex-direction: column;
}

.recipient-name,
.suggestion-name {
    font-weight: bold;
}

.recipient-remove {
    margin-left: 4px;
    cursor: pointer;
}

.recipient-input {
    flex-grow: 1;
    border: none;
    outline: none;
}

.autocomplete-results {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  max-height: 200px;
  overflow-y: auto;
  background-color: #fff;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  padding: 0;
  margin: 0;
  font-family: sans-serif;
  font-size: 12px;
  z-index: 999;
}

.autocomplete-results li {
  padding: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.autocomplete-results li:hover {
  background-color: #f1f1f1;
}

.suggestion-item {
    padding: 8px;
    cursor: pointer;
    display: flex;
    align-items: center;
    width: 100%;
    box-sizing: border-box;
}

.suggestion-avatar {
    width: 24px;
    height: 24px;
    margin-right: 4px;
}

.suggestion-details {
  display: flex;
  flex-direction: column;
}

.suggestion-name {
  font-weight: bold;
}

.suggestion-email {
  color: #777;
}

.editable {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 8px;
    margin-top: 8px;
    min-height: 200px;
}

.editable[contenteditable="true"] {
    background-color: #fff;
}

.email-body {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 8px;
    margin-top: 8px;
    min-height: 200px;
}

.email-body a {
    text-decoration: none;
    color: #0645ad;
}
.email-body-input {
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 8px;
  margin-top: 8px;
  min-height: 200px;
  resize: vertical;
  width: 100%;
}
</style>

<script>
import AutocompleteContainer from './AutocompleteContainer.vue';
export default {
    data() {
        return {
            inputValue: "",
            suggestions: [],
            showSuggestions: false,
            recipients: [],
            emailBody: "",
            isEditing: false,
            contacts: [],
        };
    },
    mounted() {
        document.addEventListener("input", this.handleInput);
        document.addEventListener("keydown", this.handleKeyDown);
    },
    beforeDestroy() {
        document.removeEventListener("input", this.handleInput);
        document.removeEventListener("keydown", this.handleKeyDown);
    },
    created() {
        this.handleInput();
    },
    methods: {
        async handleInput() {
            try {
                const response = await fetch("https://jsonplaceholder.typicode.com/users");
                const contacts = await response.json();
                this.contacts = contacts;
                const inputText = this.inputValue.trim();
                const words = inputText.split(" ");
                const lastWord = words[words.length - 1];
                if (inputText.includes("@") || lastWord.length >= 3) {
                    let filteredContacts = [];
                    if (lastWord.startsWith("@")) {
                        const username = lastWord.slice(1).toLowerCase();
                        filteredContacts = contacts.filter((contact) => contact.username.toLowerCase().startsWith(username));
                    }
                    else if (lastWord.length >= 3) {
                        const searchChars = lastWord.slice(0, 3).toLowerCase();
                        filteredContacts = contacts.filter((contact) => contact.name.toLowerCase().startsWith(searchChars) ||
                            contact.email.toLowerCase().startsWith(searchChars));
                    }
                    this.suggestions = filteredContacts;
                    this.showSuggestions = true;
                }
                else {
                    this.suggestions = [];
                    this.showSuggestions = false;
                }
                if (lastWord.length === 4 && this.suggestions.length === 0) {
                    this.showSuggestions = false;
                }
            }
            catch (error) {
                console.error("Error fetching contacts:", error);
            }
        },
        handleKeyDown(event) {
            const recipientInput = this.$refs.recipientInput;
            const emailBodyInput = this.$refs.emailBodyInput;
            if (event.target === recipientInput) {
                if (event.key === "Backspace" && this.inputValue === "") {
                    this.removeRecipient(this.recipients.length - 1);
                }
                else if (event.key === "Enter") {
                    if (this.showSuggestions && this.suggestions.length > 0) {
                        this.replaceWith(this.suggestions[0]);
                        recipientInput.focus();
                    }
                }
            }
            else if (event.target === emailBodyInput) {
                if (event.key === "Backspace" && event.target.selectionStart === 0) {
                    event.preventDefault();
                }
            }
            if (event.key === "Escape") {
                this.suggestions = [];
                this.showSuggestions = false;
            }
        },
        replaceWith(contact) {
            const recipient = {
                name: contact.name,
                email: contact.email,
                avatar: `https://i.pravatar.cc/24?u=${contact.id}`,
            };
            this.recipients.push(recipient);
            this.inputValue = "";
            this.suggestions = [];
            this.showSuggestions = false;
        },
        removeRecipient(index) {
            this.recipients.splice(index, 1);
        },
        getAvatarUrl(contact) {
            return `https://i.pravatar.cc/24?u=${contact.id}`;
        },
        handleBodyKeyDown(event) {
            const emailBodyInput = this.$refs.emailBodyInput;
            if (event.target === emailBodyInput) {
                if (event.key === "@") {
                    event.preventDefault();
                    this.showSuggestions = true;
                }
            }
        },
        handleBodyInput(event) {
            const emailBodyInput = this.$refs.emailBodyInput;
            const cursorPosition = emailBodyInput.selectionStart;
            const textBeforeCursor = this.emailBody.substring(0, cursorPosition);
            const lastWordStart = textBeforeCursor.lastIndexOf("@");
            const lastWord = textBeforeCursor.substring(lastWordStart + 1);
            if (lastWord.length >= 3) {
                const searchChars = lastWord.slice(0, 3).toLowerCase();
                const filteredContacts = this.contacts.filter((contact) => contact.name.toLowerCase().startsWith(searchChars) ||
                    contact.email.toLowerCase().startsWith(searchChars));
                this.suggestions = filteredContacts;
                this.showSuggestions = true;
            }
            else {
                this.suggestions = [];
                this.showSuggestions = false;
            }
            this.emailBody = event.target.value;
        },
        replaceWithContactInBody(contact) {
            const emailLink = `<a href="mailto:${contact.email}">${contact.name}</a>`;
            const emailBodyInput = this.$refs.emailBodyInput;
            const cursorPosition = emailBodyInput.selectionStart;
            const textBeforeCursor = this.emailBody.substring(0, cursorPosition);
            const textAfterCursor = this.emailBody.substring(cursorPosition);
            this.emailBody = `${textBeforeCursor}${emailLink}${textAfterCursor}`;
            this.showSuggestions = false;
            emailBodyInput.focus();
        },
    },
    components: { AutocompleteContainer }
};
</script>


