<template class="hero">
  <div v-if="editor" class="hero">
    <div class="buttons">
      <undo-button @click="editor.chain().focus().undo().run()" :disabled="!editor.can().chain().focus().undo().run()"></undo-button>
      <redo-button @click="editor.chain().focus().redo().run()" :disabled="!editor.can().chain().focus().redo().run()"></redo-button>
      <heading-button @click="editor.chain().focus().toggleHeading({ level: 1 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"></heading-button>
      <paragraph-button @click="editor.chain().focus().setParagraph().run()" :class="{ 'is-active': editor.isActive('paragraph') }"></paragraph-button>
      <image-button @click="addImage"></image-button>
      <clip-board-button @click.prevent="copyToClipboard"></clip-board-button>
    </div>
    <editor-content class="editor" :editor="editor" />
  </div>
</template>
<script>

import Image from '@tiptap/extension-image'
import Paragraph from '@tiptap/extension-paragraph'
import StarterKit from '@tiptap/starter-kit'
import Heading from '@tiptap/extension-heading'
import { Editor, EditorContent } from '@tiptap/vue-3'
import HeadingButton from "@/components/UI/HeadingButton.vue";
import UndoButton from "@/components/UI/UndoButton.vue";
import RedoButton from "@/components/UI/RedoButton.vue";
import ParagraphButton from "@/components/UI/ParagraphButton.vue";
import ImageButton from "@/components/UI/ImageButton.vue";
import ClipBoardButton from "@/components/UI/ClipBoardButton.vue";

export default {

  components: {
    ClipBoardButton,
    ImageButton,
    ParagraphButton,
    RedoButton,
    UndoButton,
    HeadingButton,
    EditorContent
  },

  data() {
    return {
      editor: null,
    }
  },
  methods: {
    // myFunction() {
    //   let copyText = document.querySelector(".editor");
    //   console.log("copytext" + copyText);
    //   // copyText.select();
    //   navigator.clipboard.write(copyText.value);
    //   // alert("Copied the text: " + copyText.value);
    // },

    addImage() {
      const url = window.prompt('URL')
      if (url) {
        this.editor.chain().focus().setImage({ src: url }).run()
      }
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.editor.getHTML())
    },
  },
  mounted() {
    this.editor = new Editor({
      extensions: [
        StarterKit, Document, Paragraph, Text, Image, Heading.configure({
          level: [1],
        }),
      ],
      content: '',
    })
  },
  beforeUnmount() {
    this.editor.destroy()
  },
}




</script>

<style lang="scss">
button{
  border: 0;
  cursor: pointer;
  background: 0;
}
*{
  color: white;
  background: #1E1E1E;
}
.hero{
  margin-top: 77px;
  margin-left: 107px;
}
.buttons{
  margin-bottom: 31px;
}
img{
  max-width: 739px;
  max-height: 1000px;
}

</style>

