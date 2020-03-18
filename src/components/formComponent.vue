<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="6" v-for="(answer,index) in model.answers" :key="index">
        <v-card height="100%">
          <v-card-title>{{answer.label}}</v-card-title>
          <template v-if="answer.type === 'text'">
            <v-text-field v-model="model.answers[index].value" class="px-3" outlined></v-text-field>
          </template>
          <template v-if="answer.type === 'textarea'">
            <v-textarea v-model="model.answers[index].value" class="px-3" outlined></v-textarea>
          </template>
          <template v-if="answer.type === 'date'">
            <v-date-picker v-model="model.answers[index].value" class="px-3" full-width landscape></v-date-picker>
          </template>
          <template v-if="answer.type === 'date-multi'">
            <v-date-picker
              v-model="model.answers[index].value"
              multiple
              class="px-3"
              full-width
              landscape
            ></v-date-picker>
          </template>
          <template v-if="answer.type === 'check'">
            <v-select
              class="px-3"
              v-model="model.answers[index].value"
              outlined
              :items="answer.options"
              multiple
            ></v-select>
          </template>
          <template v-if="answer.type === 'select'">
            <v-select
              class="px-3"
              v-model="model.answers[index].value"
              outlined
              :items="answer.options"
              v-if="answer.onchange"
              @change="loadMore"
            ></v-select>
            <v-select
              v-model="model.answers[index].value"
              outlined
              :items="answer.options"
              class="px-3"
              v-else
            ></v-select>
          </template>
          <template v-if="answer.type === 'text-multi'">
            <v-row no-gutters>
              <v-col class="px-3" cols="10">
                <v-text-field v-model="model.answers[index].helper" outlined></v-text-field>
              </v-col>
              <v-col cols="2">
                <v-btn icon fab @click="addValue(index)">
                  <v-icon class="green--text">mdi-plus</v-icon>
                </v-btn>
              </v-col>
            </v-row>
            <v-row no-gutters>
              <template v-for="(line,index2) in model.answers[index].value">
                <v-container :key="index2" fluid fill-height>
                  <v-col class="px-3" cols="10">
                    <p>{{line}}</p>
                  </v-col>
                  <v-col cols="2">
                    <v-btn icon @click="removeValue(index,index2)">
                      <v-icon>mdi-minus</v-icon>
                    </v-btn>
                  </v-col>
                </v-container>
              </template>
            </v-row>
          </template>
        </v-card>
      </v-col>
    </v-row>
    <v-btn @click="testModel">teste</v-btn>
  </v-container>
</template>

<script>
export default {
  name: "formComponent",

  data: () => ({
    model: {
      answers: []
    },
    inputs: [
      {
        id: 1,
        type: "text",
        label: "primeiro input",
        onchange: false
      },
      {
        id: 2,
        type: "date",
        label: "segundo input",
        onchange: false
      },
      {
        id: 3,
        type: "text-multi",
        label: "terceiro input",
        onchange: false
      },
      {
        id: 4,
        type: "select",
        onchange: true,
        label: "quarto input",
        options: ["opcao 1", "opcao 2", "opcao 3"]
      }
    ],
    secondRequestMock: [
      {
        id: 5,
        type: "date-multi",
        label: "quinto input",
        onchange: false
      },
      {
        id: 6,
        type: "textarea",
        label: "sexto input",
        onchange: false
      },
      {
        id: 7,
        type: "check",
        label: "setimo input",
        options: [
          "opcao 4",
          "opcao 5",
          "opcao 6",
          "opcao 4",
          "opcao 5",
          "opcao 6"
        ],
        onchange: false
      },
      {
        id: 8,
        type: "select",
        onchange: true,
        label: "oitavo input",
        options: ["opcao 4", "opcao 5", "opcao 6"]
      }
    ]
  }),
  created() {
    for (let input of this.inputs) {
      this.model.answers.push({
        type: input.type,
        id: input.id,
        onchange: input.onchange,
        label: input.label,
        options: input.options,
        value: input.type === "date-multi" ? [] : null
      });
    }
  },
  methods: {
    testModel() {
      console.log(this.model);
    },
    loadMore() {
      console.log("entrou aqui");
      for (let input of this.secondRequestMock) {
        this.model.answers.push({
          type: input.type,
          id: input.id,
          onchange: input.onchange,
          label: input.label,
          options: input.options,
          value: input.type === "date-multi" ? [] : null
        });
      }
    },
    addValue(index) {
      if (this.model.answers[index].value === null)
        this.model.answers[index].value = [];
      this.model.answers[index].value.push(this.model.answers[index].helper);
      this.model.answers[index].helper = "";
    },
    removeValue(index1, index2) {
      this.model.answers[index1].value.splice(index2, 1);
    }
  }
};
</script>
