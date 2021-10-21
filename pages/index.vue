<template>
  <v-row justify="center" align="center">
    <v-col>
      <v-card>
        <v-card-title>Schema</v-card-title>
        <prism-editor v-model="schemaYaml" class="my-editor" :highlight="highlighter" line-numbers />
      </v-card>
    </v-col>
    <v-col>
      <v-card>
        <v-card-title class="headline">
          Vuetify Schema Demo
        </v-card-title>
        <v-form>
          <v-jsf v-model="model" :schema="schema" />
        </v-form>
        <v-card-actions>
          <v-spacer />
          <v-btn
            color="primary"
            nuxt
            to="/inspire"
          >
            Continue
          </v-btn>
        </v-card-actions>
      </v-card>
      <v-card>

      </v-card>
    </v-col>
  </v-row>
</template>
<script>
import { PrismEditor } from 'vue-prism-editor';
import 'vue-prism-editor/dist/prismeditor.min.css'; // import the styles somewhere

// import highlighting library (you can use any library you want just return html string)
import { highlight, languages } from 'prismjs/components/prism-core';
import 'prismjs/components/prism-clike';
import 'prismjs/components/prism-yaml';
import 'prismjs/themes/prism-tomorrow.css'; // import syntax highlighting styles
import YAML from 'yaml';

export default {
  components: {
    PrismEditor
  },
  filters: {
    pretty(value) {
      return JSON.stringify(value, null, 2);
    }
  },
  data: () => {
    const schema = {
      type: 'object',
      properties: {
        stringProp: { type: 'string' },
        colorProp: { type: 'string', 'x-display': 'color-picker' }
      }
    };
    return {
      valid: false,
      model: {},
      schemaYaml: YAML.stringify(schema),
      schema
    };
  },
  watch: {
    schemaYaml(val) {
      console.log(val);
      try {
        this.schema = YAML.parse(this.schemaYaml);
      } catch (e) {
      }
    }
  },
  methods: {
    highlighter(code) {
      return highlight(code, languages.yaml);
    }
  }
}
</script>
<style>/* required class */
.my-editor {
  /* we dont use `language-` classes anymore so thats why we need to add background and text color manually */
  background: #2d2d2d;
  color: #ccc;

  /* you must provide font-family font-size line-height. Example: */
  font-family: Fira code, Fira Mono, Consolas, Menlo, Courier, monospace;
  font-size: 14px;
  line-height: 1.5;
  padding: 5px;
}

/* optional class for removing the outline */
.prism-editor__textarea:focus {
  outline: none;
}
</style>
