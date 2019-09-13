<template>
  <article>
    <codemirror class="input" ref="inputEditor" :options="optionsRequest" @input="onCmCodeChange"></codemirror>
    <codemirror class="output" ref="outputEditor" :value="responseOut" :options="optionsOutput"></codemirror>
  </article>
</template>

<script>
import { codemirror } from "vue-codemirror-lite/index";
import { setTimeout } from "timers";
import { Handler, connectionClient } from "rpc-client/client";
import { resolve } from "path";
require("codemirror/mode/javascript/javascript");
export default {
  name: "Article",
  components: { codemirror },
  props: {
    triggerReq: {
      type: Boolean | Object,
      default: false
    }
  },
  data: () => ({
    model: "",
    request: "",
    responseOut: "",
    CodeMirror: ""
  }),
  computed: {
    optionsRequest: () => ({
      mode: "javascript",
      tabSize: 2,
      styleActiveLine: false,
      lineNumbers: true,
      lineWrapping: true,
      styleActiveLine: true,
      line: true,
      styleSelectedText: true,
      matchBrackets: true,
      showCursorWhenSelecting: true,
      theme: "monokai"
    }),
    optionsOutput: () => ({
      mode: "javascript",
      json: true,
      tabSize: 2,
      lineNumbers: true,
      lineWrapping: true,
      viewportMargin: Infinity,
      readOnly: true,
      smartIndent: true,
      autocorrect: true,
      styleActiveLine: true,
      line: true,
      styleSelectedText: true,
      matchBrackets: true,
      showCursorWhenSelecting: true,
      theme: "monokai"
    }),
    editorOut() {
      // get current editor object
      return this.$refs.outputEditor.editor;
    },
    editorIn() {
      // get current editor object
      return this.$refs.inputEditor.editor;
    }
  },
  methods: {
    open(link) {
      this.$electron.shell.openExternal(link);
    },
    onCmCodeChange(newCode) {
      this.code = newCode;
      this.$emit("funcReq", this.code);
    }
  },
  mounted() {
    this.editorIn.focus();
  },
  watch: {
    triggerReq: function(val) {
      if (val.host && val.queue) {
        new Promise(async resolve => {
          const connection = await connectionClient(
            `amqp://${val.host}`,
            val.queue
          );
          Handler.foo = () => {};
          let [, target, params] = this.code.match(/([^\(]+)(.+)/);
          console.log(target, params);
          params = params.match(/[^'", \(\)]+/g);
          console.log(params);
          const resp = await Handler[target](params, {
            return: "String"
          }).catch(err => {
            console.log(err);
            return err;
          });

          connection.close();
          resolve(resp);
        }).then(resp => {
          this.responseOut = resp.toString();
        });
      }
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Source Sans Pro", sans-serif;
  caret-color: green;
  caret-color: #666666;
}

.input,
.output {
  width: 50%;
  background-color: #0f0f0f;
  float: left;
  border: 1px solid #6b6b6b;
}

.CodeMirror {
  font-family: monospace;
  height: 86vh;
  color: black;
  direction: ltr;
}

.cm-s-monokai.CodeMirror {
  background: #272822;
  color: #f8f8f2;
}
.cm-s-monokai div.CodeMirror-selected {
  background: #ccc;
  color: #0f0f0f;
}
.cm-s-monokai .CodeMirror-line::selection,
.cm-s-monokai .CodeMirror-line > span::selection,
.cm-s-monokai .CodeMirror-line > span > span::selection {
  background: rgba(73, 72, 62, 0.99);
}
.cm-s-monokai .CodeMirror-line::-moz-selection,
.cm-s-monokai .CodeMirror-line > span::-moz-selection,
.cm-s-monokai .CodeMirror-line > span > span::-moz-selection {
  background: rgba(73, 72, 62, 0.99);
}
s .cm-s-monokai .CodeMirror-gutters {
  background: #272822;
}
.cm-s-monokai .CodeMirror-guttermarker {
  color: white;
}
.cm-s-monokai .CodeMirror-guttermarker-subtle {
  color: #d0d0d0;
}
.cm-s-monokai .CodeMirror-linenumber {
  color: #d0d0d0;
}
.cm-s-monokai .CodeMirror-cursor {
  border-left: 1px solid #f8f8f0;
}

.cm-s-monokai span.cm-comment {
  color: #75715e;
}
.cm-s-monokai span.cm-atom {
  color: #ae81ff;
}
.cm-s-monokai span.cm-number {
  color: #ae81ff;
}

.cm-s-monokai span.cm-comment.cm-attribute {
  color: #97b757;
}
.cm-s-monokai span.cm-comment.cm-def {
  color: #bc9262;
}
.cm-s-monokai span.cm-comment.cm-tag {
  color: #bc6283;
}
.cm-s-monokai span.cm-comment.cm-type {
  color: #5998a6;
}

.cm-s-monokai span.cm-property,
.cm-s-monokai span.cm-attribute {
  color: #a6e22e;
}
.cm-s-monokai span.cm-keyword {
  color: #f92672;
}
.cm-s-monokai span.cm-builtin {
  color: #66d9ef;
}
.cm-s-monokai span.cm-string {
  color: #e6db74;
}

.cm-s-monokai span.cm-variable {
  color: #f8f8f2;
}
.cm-s-monokai span.cm-variable-2 {
  color: #9effff;
}
.cm-s-monokai span.cm-variable-3,
.cm-s-monokai span.cm-type {
  color: #66d9ef;
}
.cm-s-monokai span.cm-def {
  color: #fd971f;
}
.cm-s-monokai span.cm-bracket {
  color: #f8f8f2;
}
.cm-s-monokai span.cm-tag {
  color: #f92672;
}
.cm-s-monokai span.cm-header {
  color: #ae81ff;
}
.cm-s-monokai span.cm-link {
  color: #ae81ff;
}
.cm-s-monokai span.cm-error {
  background: #f92672;
  color: #f8f8f0;
}

.cm-s-monokai .CodeMirror-activeline-background {
  background: #373831;
}
.cm-s-monokai .CodeMirror-matchingbracket {
  text-decoration: underline;
  color: white !important;
}
</style>
