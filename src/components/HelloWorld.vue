<template>
  <v-container fluid>
    <v-row>
      <v-col cols="2" md="2" sm="6">
        <v-card class="mx-auto" tile>
          <v-list flat>
            <v-subheader>Options</v-subheader>
            <v-list-item-group v-model="item" color="primary">
              <v-list-item v-for="(item, i) in items" :key="i">
                <v-list-item-icon>
                  <v-icon v-text="item.icon"></v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title v-text="item.text"></v-list-item-title>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn icon>
                    <v-icon @click="addNewElement(item.value)" color="grey lighten-1">mdi-plus-box</v-icon>
                  </v-btn>
                </v-list-item-action>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
      <v-col cols="7" md="7" sm="6">
        <!-- <v-card class="mx-auto" tile>
          <v-subheader>Options</v-subheader>
        </v-card>-->
        <VueNestable v-model="elements" key-prop="id" class-prop="class">
          <template slot-scope="{ item,index }">
            <!-- Handler -->

            <!-- Content -->
            <v-card outlined class="mt-2">
              <v-card-text>
                <v-card-subtitle>
                  <VueNestableHandle :item="item">
                    <v-icon color="grey lighten-1">mdi-cursor-move</v-icon>
                  </VueNestableHandle>
                  {{ item.elementName }}
                  <span class="float-right">
                    <v-icon @click="removeElement(index)" color="grey lighten-1">mdi-minus-circle</v-icon>
                  </span>
                </v-card-subtitle>

                <v-row>
                  <v-col>
                    <v-text-field v-model="item.label" label="Label" outlined dense></v-text-field>
                  </v-col>
                  <v-col>
                    <v-text-field v-model="item.placeholder" label="placeholder" outlined dense></v-text-field>
                  </v-col>
                  <v-col>
                    <v-text-field v-model="item.name" label="Name" outlined dense></v-text-field>
                  </v-col>

                  <v-col>
                    <v-checkbox v-model="item.required" :label="item.required"></v-checkbox>
                  </v-col>
                </v-row>
                <v-row
                  v-if="item.type ==='select' || item.type ==='radio' || item.type ==='checkbox'"
                >
                  <v-col>
                    <v-combobox
                      v-model="item.options"
                      hide-selected
                      label="Add Options/Values"
                      multiple
                      outlined
                      dense
                      persistent-hint
                      small-chips
                    >
                      <template v-slot:no-data>
                        <v-list-item>
                          <v-list-item-content>
                            <v-list-item-title>
                              Press
                              <kbd>enter</kbd> to create a new one
                            </v-list-item-title>
                          </v-list-item-content>
                        </v-list-item>
                      </template>
                    </v-combobox>
                  </v-col>
                  <!-- <v-col>
                <v-text-field outlined dense label="value"></v-text-field>
                  </v-col>-->
                </v-row>
              </v-card-text>
            </v-card>
          </template>
        </VueNestable>
      </v-col>
      <v-col cols="3" md="3" sm="6">
        <v-card class="mx-auto" tile>
          <v-subheader>Previews</v-subheader>
          <vue-nestable v-model="elements">
            <vue-nestable-handle slot-scope="{ item }" :item="item">
              <v-card-text>
                <div v-if="item.type==='text-field'">
                  <span>{{item.label}}</span>
                  <v-text-field :placeholder="item.placeholder" outlined dense></v-text-field>
                </div>
                <div v-if="item.type==='email'">
                  <span>{{item.label}}</span>
                  <v-text-field type="email" :placeholder="item.placeholder" outlined dense></v-text-field>
                </div>
                <div v-if="item.type==='number'">
                  <span>{{item.label}}</span>
                  <v-text-field type="number" :placeholder="item.placeholder" outlined dense></v-text-field>
                </div>
                <div v-if="item.type==='password'">
                  <span>{{item.label}}</span>
                  <v-text-field type="password" :placeholder="item.placeholder" outlined dense></v-text-field>
                </div>
                <div v-if="item.type==='date'">
                  <span>{{item.label}}</span>
                  <v-text-field type="date" :placeholder="item.placeholder" outlined dense></v-text-field>
                </div>
                <div v-if="item.type==='button'">
                  <v-btn v-text="item.label" outlined dense></v-btn>
                </div>
                <div v-if="item.type==='text-area'">
                  <span>{{item.label}}</span>
                  <v-textarea :placeholder="item.placeholder" outlined dense></v-textarea>
                </div>
                <div v-if="item.type==='select'">
                  <span>{{item.label}}</span>
                  <v-select :items="item.options" :placeholder="item.placeholder" outlined dense></v-select>
                </div>
                <div v-if="item.type==='radio'">
                  <span>{{item.label}}</span>

                  <v-radio-group row>
                    <v-radio v-for="(item,i) in item.options" :key="i" :label="item"></v-radio>
                  </v-radio-group>
                </div>
                <div v-if="item.type==='checkbox'">
                  <span>{{item.label}}</span>
                  <v-row>
                    <v-col v-for="(item,i) in item.options" :key="i">
                      <v-checkbox :label="item"></v-checkbox>
                    </v-col>
                  </v-row>
                </div>
              </v-card-text>
            </vue-nestable-handle>
          </vue-nestable>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { VueNestable, VueNestableHandle } from "vue-nestable";
export default {
  name: "vue-custom-form",
  components: {
    VueNestable,
    VueNestableHandle
  },
  data: () => ({
    item: 1,
    nestableItems: [
      {
        id: 0,
        text: "Andy"
      },
      {
        id: 1,
        text: "Harry"
      }
    ],
    items: [
      { text: "Text Field", value: "text-field", icon: "mdi-open-in-new" },
      { text: "Text Area", value: "text-area", icon: "mdi-tooltip-text" },
      { text: "Select", value: "select", icon: "mdi-select-all" },
      { text: "Radio Button", value: "radio", icon: "mdi-radiobox-marked" },
      { text: "Checkbox", value: "checkbox", icon: "mdi-checkbox-multiple-marked" },
      { text: "Email", value: "email", icon: "mdi-email-open" },
      { text: "Number", value: "number", icon: "mdi-numeric-0-box-multiple" },
      { text: "Password", value: "password", icon: "mdi-lock" },
      { text: "Date", value: "date", icon: "mdi-calendar-multiple" },
      { text: "Button", value: "button", icon: "mdi-link-variant" }
    ],
    textField: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "text-field",
      elementName: "Text Field"
    },
    textArea: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "text-area",
      elementName: "Text Area"
    },
    select: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "select",
      elementName: "Select",
      options: []
    },
    radio: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "radio",
      elementName: "radio",
      options: []
    },
    checkbox: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "checkbox",
      elementName: "checkbox",
      options: []
    },
    email: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "email",
      elementName: "email"
    },
    number: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "number",
      elementName: "number"
    },
    password: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "password",
      elementName: "password"
    },
    button: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "button",
      elementName: "button"
    },
    date: {
      id: null,
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "date",
      elementName: "date"
    },
    idCounter: 1,
    elements: [
      {
        id: 0,
        label: "Label",
        placeholder: "PlaceHolder",
        name: "Name",
        required: "Required",
        type: "text-field",
        elementName: "Text Field"
      }
    ]
  }),
  methods: {
    addNewElement(type) {
      switch (type) {
        case "text-field":
          this.textField.id = this.idCounter;
          this.elements.push(this.textField);
          break;
        case "text-area":
          this.textArea.id = this.idCounter;
          this.elements.push(this.textArea);
          break;
        case "select":
          this.select.id = this.idCounter;
          this.elements.push(this.select);
          break;
        case "radio":
          this.radio.id = this.idCounter;
          this.elements.push(this.radio);
          break;
        case "checkbox":
          this.checkbox.id = this.idCounter;
          this.elements.push(this.checkbox);
          break;
        case "radio":
          this.radio.id = this.idCounter;
          this.elements.push(this.radio);
          break;
        case "email":
          this.email.id = this.idCounter;
          this.elements.push(this.email);
          break;
        case "number":
          this.number.id = this.idCounter;
          this.elements.push(this.number);
          break;
        case "password":
          this.password.id = this.idCounter;
          this.elements.push(this.password);
          break;
        case "date":
          this.date.id = this.idCounter;
          this.elements.push(this.date);
          break;
        case "button":
          this.button.id = this.idCounter;
          this.elements.push(this.button);
          break;
      }
      this.idCounter++;
    },
    removeElement(index) {
      this.elements.splice(index, 1);
    }
    // beforeMove({ dragItem, pathFrom, pathTo }) {
    //   // Item 4 can not be nested more than one level
    //   if (dragItem.key === 4) {
    //     return pathTo.length === 1;
    //   }
    //   // All other items can be
    //   return true;
    // }
  }
};
</script>
<style>
/*
* Style for nestable
*/
.nestable {
  position: relative;
}
.nestable-rtl {
  direction: rtl;
}
.nestable .nestable-list {
  margin: 0;
  padding: 0 0 0 40px;
  list-style-type: none;
}
.nestable-rtl .nestable-list {
  padding: 0 40px 0 0;
}
.nestable > .nestable-list {
  padding: 0;
}
.nestable-item,
.nestable-item-copy {
  margin: 10px 0 0;
}
.nestable-item:first-child,
.nestable-item-copy:first-child {
  margin-top: 0;
}
.nestable-item .nestable-list,
.nestable-item-copy .nestable-list {
  margin-top: 10px;
}
.nestable-item {
  position: relative;
}
.nestable-item.is-dragging .nestable-list {
  pointer-events: none;
}
.nestable-item.is-dragging * {
  opacity: 0;
  filter: alpha(opacity=0);
}
.nestable-item.is-dragging:before {
  content: " ";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(106, 127, 233, 0.274);
  border: 1px dashed rgb(73, 100, 241);
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
.nestable-drag-layer {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
  pointer-events: none;
}
.nestable-rtl .nestable-drag-layer {
  left: auto;
  right: 0;
}
.nestable-drag-layer > .nestable-list {
  position: absolute;
  top: 0;
  left: 0;
  padding: 0;
  background-color: rgba(106, 127, 233, 0.274);
}
.nestable-rtl .nestable-drag-layer > .nestable-list {
  padding: 0;
}
.nestable [draggable="true"] {
  cursor: move;
}
.nestable-handle {
  display: inline;
}
</style>
