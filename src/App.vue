<template>
  <div id="app">
    <div class="centered-container">
      <h1>Anmeldung zum CoderDojo Saar</h1>
      <form @submit.prevent="submit">
        <label
          v-for="field in Object.values(fields)" :key="field.id"
          :for="field.id"
          :class="{checkbox: field.type === 'checkbox', error: !!field.error}"
        >
          {{ field.type !== "checkbox" ? field.title : "" }}
          <input
            :id="field.id"
            :type="field.type || 'text'"
            v-model="field.value"
            @blur="validateField(field)"
          >
          {{ field.type === "checkbox" ? field.title : "" }}
          <transition name="vertical" mode="out-in">
            <small v-if="!field.error" key="description">{{ field.description }}</small>
            <small v-else class="error" key="error">{{ field.error }}</small>
          </transition>
        </label>

        <p>
          Wenn du auf den Button klickst, öffnet sich dein Email-Programm.
          Du kannst den Text gerne nochmal abändern oder etwas hinzufügen, aber vergiss nicht, ihn abzusenden!
        </p>

        <button type="submit">Bestätigen</button>
      </form>
    </div>
  </div>
</template>

<script>
  import Vue from "vue";

  export default {
    name: "app",
    components: {},
    data: () => ({
      fields: [
        {
          id: "first-name",
          type: "text",
          title: "Dein Vorname",
          description: "Damit wir wissen, wie wir dich nennen sollen.",
          value: "",
          validate(value) {
            return value === "" ? "Dieses Feld darf nicht leer sein." : null;
          }
        },
        {
          id: "last-name",
          type: "text",
          title: "Dein Nachname",
          value: "",
          validate(value) {
            return value === "" ? "Dieses Feld darf nicht leer sein." : null;
          }
        },
        {
          id: "age",
          type: "number",
          title: "Dein Alter",
          value: "",
          validate(value) {
            if (value === "") {
              return "Dieses Feld darf nicht leer sein.";
            }

            const number = parseInt(value);

            if (isNaN(number)) {
              return "Das scheint keine Zahl zu sein.";
            } else if (number < 7) {
              return "Du bist leider noch zu jung.";
            } else if (number > 17) {
              return "Du bist leider schon zu alt.";
            }
          }
        },
        {
          id: "email",
          type: "email",
          title: "Deine Email-Adresse",
          description: "Falls du keine eigene Email-Adresse hast, gib einfach die deiner Eltern an.",
          value: "",
          validate(value) {
            if (value === "") {
              return "Dieses Feld darf nicht leer sein.";
            } else if (value.length < 7 || !value.includes("@") || !value.includes(".")) {
              return "Das scheint keine valide Email-Adresse zu sein.";
            }
          }
        },
        {
          id: "own-laptop",
          type: "checkbox",
          title: "Ich habe ein eigenes Laptop",
          description: "Falls nicht, leihen wir dir gerne eins aus.",
          value: false
        }
      ]
    }),
    methods: {
      validateField(field) {
        Vue.set(field, "error", (field.validate && field.validate(field.value)) || null);
      },
      submit() {
        this.fields.forEach(this.validateField);
      }
    }
  };
</script>

<style scoped lang="scss">
  .vertical-enter-active, .vertical-leave-active {
    transition: opacity 0.2s, transform 0.2s;
  }

  .vertical-enter {
    opacity: 0;
    transform: translateY(-2px);
  }

  .vertical-enter-to, .vertical-leave {
    opacity: 1;
    transform: translateY(0px);
  }

  .vertical-leave-to {
    opacity: 0;
    transform: translateY(6px);
  }

  #app {
    max-width: 100vw;
    width: 500px;
    margin: 0 auto;
    padding: 10px;
  }

  .centered-container {
    width: 100%;
    height: 100%;

    padding: 20px;

    border: 1px solid #e8e8e8;
    border-radius: 15px;

    background-color: #f8f8f8;

    font-size: 16px;
    font-family: 'Raleway', monospace;
  }

  h1:first-child {
    margin-top: 0;
  }

  label {
    display: inline-block;

    font-size: 18px;

    color: #2a2a2a;
    transition: color 0.2s linear;

    width: 100%;

    input {
      width: 100%;
      height: 30px;

      margin-top: 5px;

      font-size: 16px;
      font-family: 'Raleway', monospace;

      outline: none;
      border: 1px solid #c9c9c9;
      border-radius: 2px;
      transition: border-color 0.2s ease-in;

      padding: 4px;

      &:focus {
        border-color: #217bb7;
      }
    }

    small {
      margin-top: 2px;
      display: inline-block;
    }

    &.checkbox {
      input {
        width: unset;
        height: unset;
      }

      small {
        margin-top: 5px;
        display: block;
      }
    }

    &.error {
      color: #d4493e;

      input {
        border-color: #d4493e;

        &:focus {
          border-color: #d4493e;
        }
      }
    }

    &:focus-within:not(.error) {
      color: #1c679a;
    }

    &:not(:first-child) {
      margin-top: 20px;
    }
  }

  p {
    color: #2a2a2a;
  }

  button {
    width: 100%;
    height: 50px;

    background-color: #ffffff;
    border: 1px solid #c9c9c9;
    border-radius: 2px;
    transition: background-color 0.2s linear;

    font-family: "Raleway", monospace;
    font-size: 20px;

    &:hover, &:focus {
      background-color: #f5f5f5;
    }
  }
</style>

