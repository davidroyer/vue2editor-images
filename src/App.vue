<template>
  <div id="app">

    <h1>Vue2Editor - Images Upload</h1>
    <h2>Upload an image to server and use response to get url and use that for editor content instead of defualt Base64 that Quill.js provides</h2>
    <p>This example uses the imgur API, and uses my account to upload photos.</p>
    <p>I plan to merge this feature soon but am making it available for testing and feedback.</p>

    <h3>To test out this version in your project:</h3>
<pre>
  <code class="bash">yarn add vue2-editor@images</code>
</pre>
<strong>OR</strong>
<pre>
  <code class="bash">npm install vue2-editor@images</code>
</pre>
    <vue-editor id="editor"
      useCustomImageHandler
      @imageAdded="handleImageAdded" v-model="editorContent">
    </vue-editor>
  </div>
</template>

<script>
import {VueEditor} from 'vue2-editor'
import axios from 'axios'
export default {
  name: 'app',
  components: {
    VueEditor
  },
  data () {
    return {
      editorContent: 'Initial Content'
    }
  },
  methods: {
    handleImageAdded(file, Editor, cursorLocation) {
      const CLIENT_ID = '993793b1d8d3e2e'

      var formData = new FormData();
      formData.append('image', file)

      axios({
        url: 'https://api.imgur.com/3/image',
        method: 'POST',
        headers:{
          'Authorization': 'Client-ID ' + CLIENT_ID
        },
        data: formData
      })
      .then((result) => {
        console.log(result);
        let url = result.data.data.link
        Editor.insertEmbed(cursorLocation, 'image', url);
      })
      .catch((err) => {
        console.log(err);
      })
    }
  }
}
</script>

<style>
@import "https://cdnjs.cloudflare.com/ajax/libs/highlight.js/9.12.0/styles/default.min.css";

#editor {
  height: 400px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
  padding-bottom: 3em;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
