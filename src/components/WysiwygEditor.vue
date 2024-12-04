<template>
  <div>
    <div class="toolbar">
      <button @click="undo">⮌ Назад</button>
      <button @click="redo">⮊ Вперед</button>
      <button @click="makeHeader">Заголовок</button>
      <button @click="makeParagraph">Абзац</button>
      <button @click="addImage">Добавить картинку</button>
      <button @click="copyHtml">Копировать HTML</button>
    </div>
    <div ref="editor" class="editor"></div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import Quill from 'quill';
import 'quill/dist/quill.snow.css';

export default {
  setup() {
    const editor = ref(null);
    let quillInstance;

    onMounted(() => {
      quillInstance = new Quill(editor.value, {
        theme: 'snow',
        modules: {
          toolbar: false
        }
      });
    });

    const undo = () => quillInstance.history.undo();
    const redo = () => quillInstance.history.redo();
    const makeHeader = () => quillInstance.format('header', 1);
    const makeParagraph = () => quillInstance.format('header', false);
    const addImage = () => {
      const url = prompt('Введите URL изображения:');
      if (url) {
        const range = quillInstance.getSelection();
        quillInstance.insertEmbed(range.index, 'image', url);
      }
    };
    const copyHtml = () => {
      const html = quillInstance.root.innerHTML;
      navigator.clipboard.writeText(html).then(() => {
        alert('HTML скопирован!');
      });
    };

    return {
      editor,
      undo,
      redo,
      makeHeader,
      makeParagraph,
      addImage,
      copyHtml
    };
  }
};
</script>

<style>
.toolbar {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}
.editor {
  height: 300px;
  border: 1px solid #ccc;
}
</style>