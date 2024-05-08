<template>
  <q-page class="q-pa-md">
    <q-tabs v-model="activeTab">
      <q-tab name="selecties" label="Selecties"></q-tab>
      <q-tab name="weergaves" label="Weergaves"></q-tab>
    </q-tabs>

    <q-tab-panels v-model="activeTab">
      <q-tab-panel name="selecties">
        <div class="q-gutter-md row">
          <div class="col q-col-border-right">
            <q-page class="q-pa-md">
              <h6>Opgeslagen Selecties</h6>
              <!-- Toon opgeslagen selecties -->
              <!-- Zoekbalk -->
              <q-input
                v-model="searchTerm"
                filled
                clearable
                placeholder="Zoeken..."
                @input="searchSelections"
              />

              <q-list>
                <div
                  v-for="(selection, index) in filteredSelections"
                  :key="index"
                >
                  <q-item>
                    <q-item-section>
                      <div class="text-body1 font-weight-medium">
                        {{ selection.name }}
                      </div>
                      <div class="q-mt-sm">
                        <q-chip
                          v-for="(tag, tagIndex) in selection.tags"
                          :key="tagIndex"
                          class="q-mr-sm"
                          dense
                          color="primary"
                          >{{ tag }}</q-chip
                        >
                      </div>
                    </q-item-section>

                    <q-item-section side>
                      <q-btn @click="addSelection(selection)" color="positive"
                        >+</q-btn
                      >
                    </q-item-section>
                  </q-item>
                  <q-separator :key="'separator-' + index" />
                </div>
              </q-list>
            </q-page>
          </div>

          <div class="col">
            <q-page class="q-pa-md">
              <h6>Geselecteerde Selecties</h6>
              <!-- Toon geselecteerde criteria om te combineren -->
              <q-list>
                <q-item
                  v-for="(selection, index) in selectedSelections"
                  :key="index"
                >
                  <q-item-section>
                    <div class="text-body1 font-weight-medium">
                      {{ selection.name }}
                    </div>
                    <div class="q-mt-sm">
                      <q-chip
                        v-for="(tag, tagIndex) in selection.tags"
                        :key="tagIndex"
                        class="q-mr-sm"
                        dense
                        color="primary"
                        >{{ tag }}</q-chip
                      >
                    </div>
                  </q-item-section>
                  <q-item-section side>
                    <q-btn @click="removeSelection(index)" color="negative"
                      >-</q-btn
                    >
                  </q-item-section>
                </q-item>
              </q-list>

              <q-btn label="Nieuwe Selectie" @click="createNewSelection" />
            </q-page>
          </div>
        </div>
      </q-tab-panel>

      <q-tab-panel name="weergaves">
        <div class="q-gutter-md row">
          <div class="col q-col-border-right">
            <q-page class="q-pa-md">
              <h6>Opgeslagen Weergaves</h6>
              <!-- Toon opgeslagen weergaves -->
              <q-list>
                <q-item v-for="(view, index) in savedViews" :key="index">
                  <q-item-section>{{ view.name }}</q-item-section>
                  <q-item-section side>
                    <q-btn @click="addView(view)" color="positive">+</q-btn>
                  </q-item-section>
                </q-item>
              </q-list>
            </q-page>
          </div>

          <div class="col">
            <q-page class="q-pa-md">
              <h6>Geselecteerde Weergaves</h6>
              <!-- Toon geselecteerde weergaves -->
              <q-list>
                <q-item v-for="(view, index) in selectedViews" :key="index">
                  <q-item-section>{{ view.name }}</q-item-section>
                  <q-item-section side>
                    <q-btn @click="removeView(index)" color="negative">-</q-btn>
                  </q-item-section>
                </q-item>
              </q-list>

              <q-btn label="Nieuwe Weergave" @click="createNewView" />
            </q-page>
          </div>
        </div>
      </q-tab-panel>
    </q-tab-panels>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'selecties',
      savedSelections: [
        { name: 'Selectie 1', tags: ['bomen', 'wegen'] },
        {
          name: 'Selectie 2',
          tags: ['parkeerplaatsen', 'inspectie vanaf: 01-01-2024'],
        },
        { name: 'Selectie 3' },
      ],
      selectedSelections: [],
      savedViews: [
        { name: 'Weergave 1' },
        { name: 'Weergave 2' },
        { name: 'Weergave 3' },
      ],
      selectedViews: [],
      searchTerm: '', // Zoekterm
    };
  },
  computed: {
    filteredSelections() {
      // Filter de selecties op basis van de zoekterm
      const term = this.searchTerm.toLowerCase();
      return this.savedSelections.filter((selection) => {
        // Controleer of de zoekterm voorkomt in de omschrijving of tags
        const nameMatch = selection.name.toLowerCase().includes(term);
        const tagMatch = selection.tags.some((tag) =>
          tag.toLowerCase().includes(term)
        );
        return nameMatch || tagMatch;
      });
    },
  },
  methods: {
    createNewSelection() {
      // Functie om een nieuwe selectie aan te maken
      console.log('Nieuwe selectie aanmaken');
    },
    createNewView() {
      // Functie om een nieuwe weergave aan te maken
      console.log('Nieuwe weergave aanmaken');
    },
    addSelection(selection) {
      // Controleer of de selectie al is toegevoegd
      if (!this.selectedSelections.includes(selection)) {
        // Voeg de selectie alleen toe als deze nog niet is toegevoegd
        this.selectedSelections.push(selection);
      }
    },
    addView(view) {
      // Controleer of de weergave al is toegevoegd
      if (!this.selectedViews.includes(view)) {
        // Voeg de weergave alleen toe als deze nog niet is toegevoegd
        this.selectedViews.push(view);
      }
    },
    removeSelection(index) {
      // Functie om een selectie uit de geselecteerde selecties te verwijderen
      this.selectedSelections.splice(index, 1);
    },
    removeView(index) {
      // Functie om een weergave uit de geselecteerde weergaves te verwijderen
      this.selectedViews.splice(index, 1);
    },
    isSelected(selection) {
      return this.selectedSelections.some((sel) => sel.name === selection.name);
    },
    searchSelections() {
      // Wordt uitgevoerd wanneer de gebruiker iets in de zoekbalk invoert
      // Het filteren wordt automatisch afgehandeld door de computed property
    },
  },
};
</script>

<style scoped>
.q-col-border-right {
  border-right: 1px solid #ccc; /* Verticale scheidingslijn */
}
</style>
