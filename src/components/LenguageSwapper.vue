<template>
  <select
    v-model="selectedLanguage"
    @change="changeLanguage"
    class="bg-white border border-gray-300 text-gray-700 py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
  >
    <option v-for="language in languages" :key="language.code" :value="language.code">
      {{ language.name[selectedLanguage] }}
    </option>
  </select>
</template>

<script lang="ts" setup>
import { onMounted, ref } from "vue";
import { navigate } from "astro:transitions/client";

const defaultLanguage = "es";
const selectedLanguage = ref(defaultLanguage);

const getLanguageFromPath = () => {
  const pathParts = window.location.pathname.split("/").filter(Boolean);
  if (pathParts.length && languages.some((lang) => lang.code === pathParts[0])) {
    return pathParts[0];
  }
  return defaultLanguage;
};

onMounted(() => {
  selectedLanguage.value = getLanguageFromPath();
});

interface Language {
  code: string;
  name: {
    en: string;
    es: string;
    ru: string;
    ro: string;
    ca: string;
  };
}

const languages: Language[] = [
  {
    code: "es",
    name: {
      en: "Spanish",
      es: "Español",
      ru: "Испанский",
      ro: "Spaniolă",
      ca: "Espanyol",
    },
  },
  {
    code: "en",
    name: {
      en: "English",
      es: "Inglés",
      ru: "Английский",
      ro: "Engleză",
      ca: "Anglès",
    },
  },
  {
    code: "ru",
    name: {
      en: "Russian",
      es: "Ruso",
      ru: "Русский",
      ro: "Rusă",
      ca: "Rus",
    },
  },
  {
    code: "ro",
    name: {
      en: "Romanian",
      es: "Rumano",
      ru: "Румынский",
      ro: "Română",
      ca: "Romanès",
    },
  },
  {
    code: "ca",
    name: {
      en: "Catalan",
      es: "Catalán",
      ru: "Каталанский",
      ro: "Catalană",
      ca: "Català",
    },
  },
];

const changeLanguage = () => {
  let newPath = window.location.pathname;
  const pathParts = newPath.split("/").filter(Boolean);
  if (pathParts.length && languages.some((lang) => lang.code === pathParts[0])) {
    if (selectedLanguage.value === defaultLanguage) {
      pathParts.shift();
    } else {
      pathParts[0] = selectedLanguage.value;
    }
    newPath = "/" + pathParts.join("/");
  } else if (selectedLanguage.value !== defaultLanguage) {
    newPath = `/${selectedLanguage.value}${newPath}`;
  }
  navigate(newPath);
};
</script>
