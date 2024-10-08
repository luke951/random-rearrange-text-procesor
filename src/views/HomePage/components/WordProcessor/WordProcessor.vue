<template>
  <div class="container mt-5">
    <h4 class="text-center font-weight-bold mb-4"><strong>File text procesor</strong></h4>
    <form @submit.prevent="processFile" class="mb-3">
      <div class="mb-3">
        <label for="file-upload" class="btn btn-lg btn-block btn-warning">Choose a .txt file</label>
        <input id="file-upload" class="mt-3" type="file" @change="uploadFile" accept=".txt" />
      </div>
      <div v-if="error">
        <p class="error mb-2">{{ error }}</p>
      </div>
      <div class="mb-3">
        <button v-if="file" type="submit" :disabled="!file" class="btn btn-primary btn-lg btn-block mt-4">Convert text</button>
        <button v-if="file" @click.prevent="resetForm" type="button" class="btn btn-danger btn-lg btn-block mt-4 ms-2">Reset form</button>
      </div>
    </form>

    <div class="processed-box" v-if="convertText">
      <h2 class="mb-3">Converted text from the file:</h2>
      <pre>{{ convertText }}</pre>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const file = ref(null);
    const convertText = ref(null);
    const error = ref(null);

    const uploadFile = (event) => {
      const selectedFile = event.target.files[0];
      if (selectedFile && selectedFile.type === "text/plain") {
        file.value = selectedFile;
        error.value = null;
      } else {
        error.value = "Only text files (.txt) are accepted";
        file.value = null;
      }
    };

    const processFile = async () => {
      if (!file.value) return;

      const fileContent = await readFile(file.value);
      convertText.value = shuffleText(fileContent);
    };

    const readFile = (file) => {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = (event) => resolve(event.target.result);
        reader.onerror = (error) => reject(error);
        reader.readAsText(file, "UTF-8");
      });
    };

    const shuffleText = (text) => {
      const lines = text.split(/\r?\n/);
      const shuffleLines = lines.map((line) =>
        line.split(' ').map((word) => shuffleWord(word)).join(' '));
      return shuffleLines.join('\n');
    };

    const shuffleWord = (word) => {
      const match = word.match(/^([a-zA-ZąćęłńóśźżĄĆĘŁŃÓŚŹŻ]+)([.,!?]*)$/u);
      if (!match || word.length <= 3) return word;

      const [_, core, punctuation] = match;

      const middle = core.slice(1, -1).split('');

      for (let i = middle.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [middle[i], middle[j]] = [middle[j], middle[i]];
      }

      return core[0] + middle.join('') + core[core.length - 1] + punctuation;
    };

    const resetForm = () => {
      file.value = null;
      convertText.value = null;
      document.querySelector('input[type="file"]').value = null;
      error.value = null;
    };

    return {
      file,
      convertText,
      uploadFile,
      processFile,
      resetForm,
      error,
    };
  },
};
</script>

<style lang="scss" scoped>
  @import "./WordProcessor.scss";
</style>
