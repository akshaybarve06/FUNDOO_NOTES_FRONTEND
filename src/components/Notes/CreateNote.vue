<!--
* @Description :
*
* @file: CreateNote.vue
* @overview: CreateNote.vue is component for creating new Note
* @author: Akshay Dhananjay Barve
* @version: 20.04
* @since: 07/06/2020- Sunday
*
-->
<template>
  <div class="showNotes">
    <v-main>
      <div style="margin-bottom: 15px">
        <v-card class="md-layout-item1" v-show="isdisplay" @click="openCard()">
          <v-text-field flat placeholder="Take A Note " solo></v-text-field>
        </v-card>
        <v-card class="createnotecard" v-show="!isdisplay">
          <v-textarea flat solo rows="1" style="padding-bottom:1%" v-model="input.title" auto-grow label="Title"></v-textarea>
          <v-textarea flat solo rows="1" v-model="input.description" label="Take A Note" auto-grow></v-textarea>
          <v-card-actions>
            <Icon v-bind:card="true" />
            <v-spacer></v-spacer>
            <v-btn text @click="closeCard()">close</v-btn>
          </v-card-actions>
        </v-card>
      </div>
    </v-main>
    <div>
      <AllNotes ref="updatedNote" />
    </div>      
  </div>
</template>
<script>
import Icon from "../Icons/ShowNoteIcon";
import notes from "../../services/notes.service";                                                                       
import AllNotes from "../Notes/ShowAllNotes";
export default {
  name: "CreateNote",
  props: ["card"],
  components: {
    Icon,
    AllNotes
  },
  data() {
    return {
      menuVisible: false,
      isdisplay: true,
      allNotes: [],
      input: {
        file: "",
        title: "",
        description: "",
        checklist: "",
        isPinned: "",
        isArchived: "",
        color: "",
        reminder: "",
      },
      response: ""
    };
  },
  methods: {
    openCard() {
      this.isdisplay = false;
    },
    async closeCard() {
      if (this.input.title == "" || this.input.description == "")
        this.isdisplay = true;
      else {
        try {
          const noteDetails = {
            title: this.input.title,
            description: this.input.description
          };
          notes.addNote(noteDetails);
          this.$refs.updatedNote.getAllNotes();
          alert("Note Added Successfully");
          this.isdisplay = true;
        } catch (error) {
          console.log(error);
        }
      }
    },
    getId(items) {
      return items;
    }
  },
  mounted() {
    this.getId();
  }
};
</script>
<style lang="scss" scoped>
@import "../../css/CreateNote.scss";
</style>