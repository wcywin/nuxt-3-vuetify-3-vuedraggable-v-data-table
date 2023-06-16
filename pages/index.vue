<template>
  <v-app>
    <v-app-bar class="bg-green-darken-4 px-8 font-weight-bold">Nuxt 3 - Vuetify 3 - VDataTable - VueDraggable-v4.1</v-app-bar>
    <v-main class="mt-4 bg-green-lighten-4">
      <v-container >
        <v-row>
          <v-col>
            <v-data-table
                v-model:items-per-page="itemsPerPage"
                :headers="headers"
                :items="desserts"
                item-value="name"
                class="elevation-1 rounded"
            >
<!--                <template #top="{ columns }">-->
<!--                  <tr>-->
<!--                    <th v-for="column in columns" :key="column.key">{{ column.title }}</th>-->
<!--                  </tr>-->
<!--                </template>-->
              <template #body>

                <draggable
                  v-model="desserts"
                  tag="tbody"
                  handle=".handle"
                  @change="handleDragChange"
                >
                  <template #item="{ element }">
                    <tr>
                      <!-- This button is for ease of dragging -->
                      <v-btn
                        icon="mdi-drag"
                        size="small"
                        class="handle"
                      />
                      <td>{{ element.name}}</td>
                      <td>{{ element.calories}}</td>
                      <td>{{ element.fat}}</td>
                      <td>{{ element.carbs}}</td>
                      <td>{{ element.protein}}</td>
                      <td>{{ element.iron}}</td>
                    </tr>
                  </template>
                </draggable>

              </template>

            </v-data-table>
          </v-col>
        </v-row>

        <v-row>
          <v-col>
            <v-card>
              <v-card-title class="font-weight-bold">
                Description:
              </v-card-title>
              <v-list>
                <v-list-item>1. The <span class="font-weight-bold">v-data-table</span> requires the content of the table to be in the <code class="bg-grey-lighten-3">#body</code> slot to show the headers</v-list-item>
                <v-list-item>2. Otherwise the content will be placed into the <code class="bg-grey-lighten-3">default</code> slot and the headers will not show</v-list-item>
                <v-list-item>3. The <span class="font-weight-bold">draggable</span> functionality expects another slot <code class="bg-grey-lighten-3">#item</code> inside the v-data-table slot</v-list-item>
                <v-list-item>4. And the two slots become two elements in the html structure breaking the classic <span class="font-weight-bold">tbody -> tr -> td</span> structure</v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>

        <v-row>
          <v-col>
            <v-card>
              <v-card-title class="font-weight-bold">
                Problem:
              </v-card-title>
              <v-list>
                <v-list-item>1. We end up having two <span class="font-weight-bold">tbody</span> because of the table slot <code class="bg-grey-lighten-3">#body</code> and draggable <code class="bg-grey-lighten-3">tag="tbody"</code></v-list-item>
                <v-list-item>2. Or we end up having two <span class="font-weight-bold">tr</span> because of the draggable <code class="bg-grey-lighten-3">tag="tr"</code> and the one child only in its template <code class="bg-grey-lighten-3">tr</code> to wrap the <code class="bg-grey-lighten-3">td</code>'s</v-list-item>
                <v-list-item>3. We cannot have multiple children in the draggable <code class="bg-grey-lighten-3">#item</code> slot if we would like to iterate only the <code class="bg-grey-lighten-3">td</code> 's of the table (assume many different td's)</v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>

        <v-row>
          <v-col>
            <v-card>
              <v-card-title class="font-weight-bold">
                Solution:
              </v-card-title>
              <v-list>
                <v-list-item>1. Either <span class="font-weight-bold">Vuetify</span> or <span class="font-weight-bold">VueDraggable</span> <span class="text-decoration-underline">removes one slot</span> to keep the styling of the table when using draggable in the v-data-table <span class="font-weight-bold">tbody -> tr -> td</span> as it was in Vue 2 Draggable</v-list-item>
                <v-list-item>2. <span class="font-weight-bold">VueDraggable</span> <span class="text-decoration-underline">enables multiple children</span> in the draggable <code class="bg-grey-lighten-3">#item</code> slot (template cannot be used)</v-list-item>
              </v-list>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import draggable from 'vuedraggable'

const itemsPerPage = ref(5)
const headers = ref([
  {
    title: 'Dessert (100g serving)',
    align: 'start',
    sortable: false,
    key: 'name',
  },
  { title: 'Calories', align: 'end', key: 'calories' },
  { title: 'Fat (g)', align: 'end', key: 'fat' },
  { title: 'Carbs (g)', align: 'end', key: 'carbs' },
  { title: 'Protein (g)', align: 'end', key: 'protein' },
  { title: 'Iron (%)', align: 'end', key: 'iron' }
])
const desserts = ref([
  {
    name: 'Frozen Yogurt',
    calories: 159,
    fat: 6.0,
    carbs: 24,
    protein: 4.0,
    iron: '1',
  },
  {
    name: 'Jelly bean',
    calories: 375,
    fat: 0.0,
    carbs: 94,
    protein: 0.0,
    iron: '0',
  },
  {
    name: 'KitKat',
    calories: 518,
    fat: 26.0,
    carbs: 65,
    protein: 7,
    iron: '6',
  },
  {
    name: 'Eclair',
    calories: 262,
    fat: 16.0,
    carbs: 23,
    protein: 6.0,
    iron: '7',
  },
  {
    name: 'Gingerbread',
    calories: 356,
    fat: 16.0,
    carbs: 49,
    protein: 3.9,
    iron: '16',
  },
  {
    name: 'Ice cream sandwich',
    calories: 237,
    fat: 9.0,
    carbs: 37,
    protein: 4.3,
    iron: '1',
  },
  {
    name: 'Lollipop',
    calories: 392,
    fat: 0.2,
    carbs: 98,
    protein: 0,
    iron: '2',
  },
  {
    name: 'Cupcake',
    calories: 305,
    fat: 3.7,
    carbs: 67,
    protein: 4.3,
    iron: '8',
  },
  {
    name: 'Honeycomb',
    calories: 408,
    fat: 3.2,
    carbs: 87,
    protein: 6.5,
    iron: '45',
  },
  {
    name: 'Donut',
    calories: 452,
    fat: 25.0,
    carbs: 51,
    protein: 4.9,
    iron: '22',
  }
])

const handleDragChange = (event) => {
  console.log(event, 'event')
}
</script>
