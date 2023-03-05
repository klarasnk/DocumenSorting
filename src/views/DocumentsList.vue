<template>
  <div>
    <div class="flex justify-between mt-[20px]">
      <div class="text-[20px] font-bold">Документы</div>
      <div class="flex gap-[15px]">
        <PrimaryButton>Новый тип</PrimaryButton>
        <PrimaryButton>Новый документ</PrimaryButton>
      </div>
    </div>
    <div
      class="w-[50%] h-[40px] my-[15px] flex items-center gap-[10px] text-[#b1bbd0] italic leading-[20px] relative"
    >
      <span class="absolute material-symbols-outlined">search</span>
      <input
        v-model="search"
        type="text"
        placeholder="Поиск"
        class="px-[30px] py-[5px] w-full placeholder:italic focus:outline-none border-b border-[#b1bbd0] focus:border-blue-700"
      />
      <span
        v-if="search"
        class="material-symbols-outlined absolute right-[10px] text-[20px] text-[#ff238d] cursor-pointer"
        @click="() => (search = '')"
        >close</span
      >
    </div>

    <draggable
      v-model="documentCategories"
      tag="ul"
      item-key="id"
      @start="iconColorChange"
      @end="iconColorRemove"
    >
      <template #item="{ element: category }">
        <DocumentCategory
          @click="dropdown(category.id)"
          @deleteCategory="deleteCategory"
          @deleteDocument="deleteDocument"
          :documentCategories="documentCategories"
          :categoryId="category.id"
          :dragIcon="category.icon"
          :category="category"
          :showDropdown="category.showDropdown"
        >
          <template v-slot:documentCategory>
            <input
              class="focus:outline-none border-b border-[#b1bbd0] focus:border-blue-700"
              v-if="category.input"
              type="text"
              @click="(e) => e.stopPropagation()"
              v-model="category.documentCategory"
              @keyup.enter="() => (category.input = false)"
            />
            <div v-else>{{ category.documentCategory }}</div>
          </template>
          <template v-if="category.coloredDots" v-slot:coloredDot>
            <ColoredDot
              v-for="(color, index) in category.coloredDots"
              :key="index"
              :dotColor="color"
            />
          </template>
          <template v-slot:categoryDetails>
            <input
              class="w-[500px] focus:outline-none border-b border-[#b1bbd0] focus:border-blue-700"
              v-if="category.input"
              type="text"
              @click="(e) => e.stopPropagation()"
              v-model="category.categoryDetails"
              @keyup.enter="category.input = false"
            />
            <div v-else>{{ category.categoryDetails }}</div>
          </template>
          <template v-slot:documentType>
            <div
              :class="{
                'my-[20px]':
                  category.documentCategory === 'documentsWithoutCategories',
              }"
            >
              <draggable
                v-model="category.documentsIncluded"
                tag="ul"
                item-key="id"
              >
                <template #item="{ element: document }">
                  <DocumentType
                    v-if="
                      category.documentCategory !==
                        'documentsWithoutCategories' && category.showDropdown
                    "
                    :width="'97%'"
                    :documentId="document.id"
                  >
                    <template v-slot:documentType>
                      {{ document.documentType }}
                    </template>
                    <template v-slot:coloredDot>
                      <ColoredDot
                        v-for="(color, index) in document.coloredDots"
                        key="index"
                        :dotColor="color"
                      />
                    </template>
                    <template v-if="document.mandatory" v-slot:mandatory>
                      Обязательный
                    </template>
                    <template
                      v-if="document.documentDetails"
                      v-slot:documentDetails
                    >
                      {{ document.documentDetails }}
                    </template>
                  </DocumentType>
                  <DocumentType
                    v-else-if="
                      category.documentCategory === 'documentsWithoutCategories'
                    "
                    :width="'100%'"
                    :documentId="document.id"
                  >
                    <template v-slot:documentType>
                      {{ document.documentType }}
                    </template>
                    <template v-slot:coloredDot>
                      <ColoredDot
                        v-for="(color, index) in document.coloredDots"
                        :key="index"
                        :dotColor="color"
                      />
                    </template>
                    <template v-if="document.mandatory" v-slot:mandatory>
                      Обязательный
                    </template>
                    <template
                      v-if="document.documentDetails"
                      v-slot:documentDetails
                    >
                      {{ document.documentDetails }}
                    </template>
                  </DocumentType>
                </template>
              </draggable>
            </div>
          </template>
        </DocumentCategory>
      </template>
    </draggable>
  </div>
</template>

<script setup>
import draggable from "vuedraggable";
import PrimaryButton from "@/components/PrimaryButton.vue";
import DocumentCategory from "@/components/DocumentCategory.vue";
import ColoredDot from "@/components/ColoredDot.vue";
import { ref } from "vue";
import DocumentType from "@/components/DocumentType.vue";
import { computed } from "@vue/reactivity";

const deleteCategory = (id) => {
  documentCategories.value = documentCategories.value.filter(
    (elm) => elm.id !== id
  );
};

const deleteDocument = (id) => {
  documentCategories.value = documentCategories.value.map((elm) =>
    elm.documentsIncluded.filter((el) => el.id !== id)
  );
};

const filteredCategory = computed(() => {
  return documentCategories.value.filter((elm) =>
    elm.documentCategory.toLowerCase().includes(search.value)
  );
});

const search = ref("");

const iconColorChange = (event) => {
  documentCategories.value.map((elm, i) => {
    if (i === event.oldIndex) {
      elm.icon = true;
    }
  });
};

const iconColorRemove = (event) => {
  documentCategories.value.map((elm) => {
    elm.icon = false;
  });
};
const dropdown = (id) => {
  documentCategories.value.map((elm) =>
    elm.id === id ? (elm.showDropdown = !elm.showDropdown) : elm
  );
};
const documentCategories = ref([
  {
    id: 1,
    documentCategory: "Обязательные для всех",
    coloredDots: ["blue", "green", "aqua"],
    categoryDetails:
      "Документы, обязательные для всех сотрудников без исключения",
    documentsIncluded: [
      {
        id: 1,
        documentType: "Пасспорт",
        coloredDots: ["green", "yellow"],
        mandatory: false,
        documentDetails: "Для всех",
      },
      {
        id: 2,
        documentType: "ИНН",
        coloredDots: [],
        mandatory: true,
        documentDetails: "",
      },
    ],
  },
  {
    id: 2,
    documentCategory: "Обязательные для трудоустройства",
    coloredDots: [],
    categoryDetails:
      "Документы, без которых невозможно трудоустройство человека на какую бы то ни было должность в компании, вне зависимости от гражданства",
    documentsIncluded: [
      {
        id: 3,
        documentType: "Пасспорт",
        coloredDots: ["yellow"],
        mandatory: true,
        documentDetails: "Для всех",
      },
    ],
  },
  {
    id: 3,
    documentCategory: "Специальные",
    coloredDots: [],
    categoryDetails: "",
    documentsIncluded: [
      {
        id: 4,
        documentType: "Пасспорт",
        coloredDots: ["blue", "gray"],
        mandatory: false,
        documentDetails: "Для всех",
      },
    ],
  },
  {
    id: 4,
    documentCategory: "documentsWithoutCategories",
    documentsIncluded: [
      {
        id: 5,
        documentType: "Пасспорт",
        coloredDots: ["green", "yellow"],
        mandatory: false,
        documentDetails: "Для всех",
      },
      {
        id: 6,
        documentType: "ИНН",
        coloredDots: [],
        mandatory: true,
        documentDetails: "",
      },
      {
        id: 7,
        documentType: "Тестовое задание кандидата",
        coloredDots: [],
        mandatory: false,
        documentDetails:
          "Россия, Украина, Белорусия, администратор филиала, повар, сушист, повар-пиццмейкер, повар горячего цеха",
      },
      {
        id: 8,
        documentType: "Трудовой договор",
        coloredDots: ["blue", "gray"],
        mandatory: false,
        documentDetails: "",
      },
      {
        id: 9,
        documentType: "Мед книжка",
        coloredDots: [],
        mandatory: false,
        documentDetails: "",
      },
    ],
  },
]);
</script>
