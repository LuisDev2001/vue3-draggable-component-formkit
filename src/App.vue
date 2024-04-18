<!-- eslint-disable @typescript-eslint/no-explicit-any -->
<script setup lang="ts">
  import { ref } from 'vue'
  import DraggableFormKit from './components/DraggableFormKit.vue'
  import DraggIcon from './components/DraggIcon.vue'

  const data = ref([
    {
      name: 'Section 1',
      order: 1,
      questions: [
        {
          name: 'Pregunta 1',
          order: 1
        },
        {
          name: 'Pregunta 2',
          order: 2
        },
        {
          name: 'Pregunta 3',
          order: 3
        },
      ]
    },
    {
      name: 'Section 2',
      order: 2,
      questions: [
        {
          name: 'Pregunta section 2 - 1',
          order: 1
        },
        {
          name: 'Pregunta section 2 - 2',
          order: 2
        },
        {
          name: 'Pregunta section 2 - 3',
          order: 3
        },
      ]
    },
  ])

  const handleAddSection = () => {
    data.value.push({
      name: `Section ${data.value.length + 1}`,
      order: data.value.length + 1,
      questions: [
        {
          name: 'Pregunta 1',
          order: 1
        },
        {
          name: 'Pregunta 2',
          order: 2
        },
        {
          name: 'Pregunta 3',
          order: 3
        },
      ]
    },)
  }

  const handleAddQuestion = (section: any) => {
    section.questions.push({
      name: `Pregunta ${section.questions.length + 1}`,
      order: section.questions.length + 1
    })
  }

  const handleUpdateListSections = (list: any) => {
    data.value = list
  }

  const handleUpdateListQuestions = (questionsUpdate: any, section: any) => {
    section.questions = questionsUpdate
  }
</script>

<template>
  <main>
    <DraggableFormKit
      class="draggable-zone"
      :list="data"
      handle=".handle-section"
      @update-list="handleUpdateListSections"
    >
      <template #item="{ item: section }">
        <div class="sections">
          <span
            style="width: 24px; display: block; cursor: move;"
            class="handle-section"
          >
            <DraggIcon />
          </span>
          {{ section.name }}
          <div>
            Preguntas:
          </div>
          <DraggableFormKit
            :list="section.questions"
            handle=".handle-quesion"
            @update-list="handleUpdateListQuestions($event, section)"
          >
            <template #item="{ item: question }">
              <section class="question">
                <span
                  style="width: 24px; display: block; cursor: move;"
                  class="handle-quesion"
                >
                  <DraggIcon />
                </span>
                {{ question.name }}
              </section>
            </template>
          </DraggableFormKit>
          <button @click="handleAddQuestion(section)">
            Agregar preguntas
          </button>
        </div>
      </template>
    </DraggableFormKit>

    <button @click="handleAddSection">
      Agregar nueva seccion
    </button>
  </main>
</template>

<style>
.sections {
  background: rgba(255, 170, 68, .4);
  border: 1px solid #ccc;
  margin: 1rem 0;
  padding: 1rem;
}

.sections div {
  padding: .5rem;
}

.question {
  border: 1px solid #ccc;
  padding: .5rem;
}
</style>