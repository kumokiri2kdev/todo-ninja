<template>
  <v-dialog max-width="600px" v-model="dialog">
    <template v-slot:activator="{ on }">
      <v-btn text class="success" v-on="on">Add New Project</v-btn>
    </template>
    <v-card>
      <v-card-title>
        <h2>Add a New Project</h2>
      </v-card-title>
      <v-card-text>
        <v-form class="px-3" ref="form">
          <v-text-field label="Title" v-model="title" prepend-icon="mdi-folder" :rules="inputRules">

          </v-text-field>
          <v-textarea label="Information" v-model="content" prepend-icon="mdi-border-color" :rules="inputRules"> </v-textarea>
          <v-menu v-model="menu" :close-on-content-click="false" max-width="290">
            <template v-slot:activator="{ on }">
              <v-text-field v-on="on" clearable
                :value="due" label="Due date" prepend-icon="mdi-calendar">
              </v-text-field>
              <v-spacer></v-spacer>
            </template>
            <v-date-picker v-model="due" @change="menu = false"></v-date-picker>
          </v-menu>
          <v-btn text class="success mx-0 mt-3" @click="submit" :loading="loading">Add project</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'Popup',
  props: {
    dataFromNavbar: {
      type: String,
      default: 'No Info'
    }
  },
  mounted : function(){
    console.log(this.dataFromNavbar);
  },
  data() {
    return {
      title: '',
      content: '',
      due: null,
      menu: false,
      inputRules: [
        v => !!v || 'This field is required',
        v => v.length >= 3 || 'Minimum length is 3 characters'
      ],
      loading: false,
      dialog: false
    }
  },
  methods: {
    submit() {
      if(this.$refs.form.validate()) {
        console.log(this.title, this.content)
        this.loading = true;
        setTimeout(() => {
          this.loading = false;
          this.dialog = false;
          this.$emit('projectAdded');
        },3000);
      }
    }
  }
}
</script>
