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
        <v-card outlined class="mt-2" v-for="(element,index) in elements" :key="index">
          <v-card-text>
            <v-card-subtitle>
              {{ element.elementName }}
              <span class="float-right">
                <v-icon @click="removeElement(index)" color="grey lighten-1">mdi-minus-circle</v-icon>
              </span>
            </v-card-subtitle>
            <v-row>
              <v-col>
                <v-text-field v-model="element.label" label="Label" outlined dense></v-text-field>
              </v-col>
              <v-col>
                <v-text-field v-model="element.placeholder" label="placeholder" outlined dense></v-text-field>
              </v-col>
              <v-col>
                <v-text-field v-model="element.name" label="Name" outlined dense></v-text-field>
              </v-col>

              <v-col>
                <v-checkbox v-model="element.required" :label="element.required"></v-checkbox>
              </v-col>
            </v-row>
            <v-row
              v-if="element.type ==='select' || element.type ==='radio' || element.type ==='checkbox'"
            >
              <v-col>
                <v-combobox
                  v-model="element.options"
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
      </v-col>
      <v-col cols="3" md="3" sm="6">
        <v-card class="mx-auto" tile>
          <v-subheader>Previews</v-subheader>
          <v-card-text v-for="(element,index) in elements" :key="index">
            <div v-if="element.type==='text-field'">
              <span>{{element.label}}</span>
              <v-text-field :placeholder="element.placeholder" outlined dense></v-text-field>
            </div>
            <div v-if="element.type==='email'">
              <span>{{element.label}}</span>
              <v-text-field type="email" :placeholder="element.placeholder" outlined dense></v-text-field>
            </div>
            <div v-if="element.type==='number'">
              <span>{{element.label}}</span>
              <v-text-field type="number" :placeholder="element.placeholder" outlined dense></v-text-field>
            </div>
            <div v-if="element.type==='password'">
              <span>{{element.label}}</span>
              <v-text-field type="password" :placeholder="element.placeholder" outlined dense></v-text-field>
            </div>
            <div v-if="element.type==='date'">
              <span>{{element.label}}</span>
              <v-text-field type="date" :placeholder="element.placeholder" outlined dense></v-text-field>
            </div>
            <div v-if="element.type==='button'">
              <v-btn v-text="element.label" outlined dense></v-btn>
            </div>
            <div v-if="element.type==='text-area'">
              <span>{{element.label}}</span>
              <v-textarea :placeholder="element.placeholder" outlined dense></v-textarea>
            </div>
            <div v-if="element.type==='select'">
              <span>{{element.label}}</span>
              <v-select :items="element.options" :placeholder="element.placeholder" outlined dense></v-select>
            </div>
            <div v-if="element.type==='radio'">
              <span>{{element.label}}</span>

              <v-radio-group row>
                <v-radio v-for="(item,i) in element.options" :key="i" :label="item"></v-radio>
              </v-radio-group>
            </div>
            <div v-if="element.type==='checkbox'">
              <span>{{element.label}}</span>
              <v-row>
                <v-col v-for="(item,i) in element.options" :key="i">
                  <v-checkbox :label="item"></v-checkbox>
                </v-col>
              </v-row>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    item: 1,
    items: [
      { text: "Text Field", value: "text-field", icon: "mdi-clock" },
      { text: "Text Area", value: "text-area", icon: "mdi-account" },
      { text: "Select", value: "select", icon: "mdi-flag" },
      { text: "Radio Button", value: "radio", icon: "mdi-flag" },
      { text: "Checkbox", value: "checkbox", icon: "mdi-flag" },
      { text: "Email", value: "email", icon: "mdi-flag" },
      { text: "Number", value: "number", icon: "mdi-flag" },
      { text: "Password", value: "password", icon: "mdi-flag" },
      { text: "Date", value: "date", icon: "mdi-flag" },
      { text: "Button", value: "button", icon: "mdi-flag" }
    ],
    textField: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "text-field",
      elementName: "Text Field"
    },
    textArea: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "text-area",
      elementName: "Text Area"
    },
    select: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "select",
      elementName: "Select",
      options: []
    },
    radio: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "radio",
      elementName: "radio",
      options: []
    },
    checkbox: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "checkbox",
      elementName: "checkbox",
      options: []
    },
    email: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "email",
      elementName: "email"
    },
    number: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "number",
      elementName: "number"
    },
    password: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "password",
      elementName: "password"
    },
    button: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "button",
      elementName: "button"
    },
    date: {
      label: "Label",
      placeholder: "PlaceHolder",
      name: "Name",
      required: "Required",
      type: "date",
      elementName: "date"
    },

    elements: [
      {
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
          this.elements.push(this.textField);
          break;
        case "text-area":
          this.elements.push(this.textArea);
          break;
        case "select":
          this.elements.push(this.select);
          break;
        case "radio":
          this.elements.push(this.radio);
          break;
        case "checkbox":
          this.elements.push(this.checkbox);
          break;
        case "radio":
          this.elements.push(this.radio);
          break;
        case "email":
          this.elements.push(this.email);
          break;
        case "number":
          this.elements.push(this.number);
          break;
        case "password":
          this.elements.push(this.password);
          break;
        case "date":
          this.elements.push(this.date);
          break;
        case "button":
          this.elements.push(this.button);
          break;
      }
    },
    removeElement(index) {
      this.elements.splice(index, 1);
    }
  }
};
</script>
