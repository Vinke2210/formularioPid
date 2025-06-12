<script setup>
import { ref, onMounted } from 'vue'

const emit = defineEmits(['update:content'])

const editorRef = ref(null)
const content = ref('')
const selectedFont = ref('Helvetica')
const selectedFontSize = ref('14px')

const fonts = [
  'Helvetica',
  'Arial',
  'Times New Roman',
  'Courier New',
  'Georgia',
  'Verdana'
]

const fontSizes = [
  '10px', '12px', '14px', '16px', '18px', '20px', '24px', '28px', '32px'
]

const formatText = (command, value = null) => {
  document.execCommand(command, false, value)
  updateContent()
}

const updateContent = () => {
  if (editorRef.value) {
    content.value = editorRef.value.innerHTML
    emit('update:content', content.value)
  }
}

const changeFont = () => {
  formatText('fontName', selectedFont.value)
}

const changeFontSize = () => {
  formatText('fontSize', selectedFontSize.value)
}

const insertList = (ordered = false) => {
  formatText(ordered ? 'insertOrderedList' : 'insertUnorderedList')
}

onMounted(() => {
  if (editorRef.value) {
    editorRef.value.addEventListener('input', updateContent)
  }
})
</script>

<template>
  <div class="rich-editor-container">
    <div class="toolbar">
      <div class="toolbar-group">
        <select v-model="selectedFont" @change="changeFont" class="font-select">
          <option v-for="font in fonts" :key="font" :value="font">{{ font }}</option>
        </select>
      </div>
      
      <div class="toolbar-group">
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('bold')"
          title="Negrita"
        >
          <strong>B</strong>
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('italic')"
          title="Cursiva"
        >
          <em>I</em>
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('underline')"
          title="Subrayado"
        >
          <u>U</u>
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('strikethrough')"
          title="Tachado"
        >
          <s>S</s>
        </button>
      </div>
      
      <div class="toolbar-group">
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('superscript')"
          title="Superíndice"
        >
          x²
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('subscript')"
          title="Subíndice"
        >
          x₂
        </button>
      </div>
      
      <div class="toolbar-group">
        <button 
          type="button" 
          class="toolbar-btn"
          @click="insertList(false)"
          title="Lista con viñetas"
        >
          •
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="insertList(true)"
          title="Lista numerada"
        >
          1.
        </button>
      </div>
      
      <div class="toolbar-group">
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('justifyLeft')"
          title="Alinear a la izquierda"
        >
          ≡
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('justifyCenter')"
          title="Centrar"
        >
          ≡
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('justifyRight')"
          title="Alinear a la derecha"
        >
          ≡
        </button>
        <button 
          type="button" 
          class="toolbar-btn"
          @click="formatText('justifyFull')"
          title="Justificar"
        >
          ≡
        </button>
      </div>
      
      <div class="toolbar-group">
        <button 
          type="button" 
          class="toolbar-btn color-btn"
          @click="formatText('foreColor', '#000000')"
          title="Color de texto"
        >
          A
        </button>
        <button 
          type="button" 
          class="toolbar-btn highlight-btn"
          @click="formatText('hiliteColor', '#ffff00')"
          title="Resaltar texto"
        >
          🖍
        </button>
      </div>
    </div>
    
    <div 
      ref="editorRef"
      class="editor-content"
      contenteditable="true"
      @input="updateContent"
      :style="{ fontFamily: selectedFont, fontSize: selectedFontSize }"
    ></div>
  </div>
</template>

<style scoped>
.rich-editor-container {
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
}

.toolbar {
  background: #f8f9fa;
  border-bottom: 1px solid #ddd;
  padding: 8px;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  align-items: center;
}

.toolbar-group {
  display: flex;
  gap: 2px;
  align-items: center;
}

.toolbar-group:not(:last-child) {
  border-right: 1px solid #ddd;
  padding-right: 8px;
}

.font-select {
  padding: 4px 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
  background: white;
  font-size: 12px;
  min-width: 100px;
}

.toolbar-btn {
  background: white;
  border: 1px solid #ccc;
  border-radius: 3px;
  padding: 4px 8px;
  cursor: pointer;
  font-size: 12px;
  transition: background-color 0.2s;
  min-width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.toolbar-btn:hover {
  background: #e9ecef;
}

.toolbar-btn:active {
  background: #dee2e6;
}

.color-btn {
  font-weight: bold;
  color: #000;
}

.highlight-btn {
  background: #fff3cd;
}

.editor-content {
  min-height: 150px;
  padding: 12px;
  background: white;
  outline: none;
  line-height: 1.5;
  font-family: Helvetica, Arial, sans-serif;
  font-size: 14px;
}

.editor-content:focus {
  background: #fafafa;
}

.editor-content p {
  margin: 0 0 8px 0;
}

.editor-content ul, .editor-content ol {
  margin: 8px 0;
  padding-left: 24px;
}

.editor-content li {
  margin: 4px 0;
}

@media (max-width: 768px) {
  .toolbar {
    padding: 4px;
    gap: 4px;
  }
  
  .toolbar-group {
    gap: 1px;
  }
  
  .toolbar-btn {
    min-width: 24px;
    height: 24px;
    padding: 2px 4px;
    font-size: 11px;
  }
  
  .font-select {
    min-width: 80px;
    font-size: 11px;
  }
}
</style>