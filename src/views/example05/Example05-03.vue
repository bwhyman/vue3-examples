<template>
  <div>
    <h1>v-for</h1>
    <ul>
      <li v-for="c of courses" :key="c.id">
        <router-link :to="`/courses/${c.id}`">{{ c.name }}</router-link>
      </li>
    </ul>

    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>name</th>
          <th>createTime</th>
          <th>operation</th>
        </tr>
      </thead>
      <tr v-for="(c, index) in courses" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ c.name }}</td>
        <td>{{ formatDate(c.createTime) }}</td>
        <td>
          <button type="button" @click="removeItem(index)">remove item</button>
        </td>
      </tr>
    </table>

    <button type="button" @click="addItem">add item</button>
  </div>
</template>
<script lang="ts">
import { computed, defineComponent, Ref, ref } from "vue";

import { listCourses } from "@/datasource/DataSource";
import { Course } from "@/datasource/Types";

/**
 * 抽取出独立的，对指定时间格式化的逻辑
 */
function useDateFormat() {
  const formatDate = computed(() => (date: string) =>
    date.replace("T", " ").substring(0, 16)
  );
  return {
    formatDate
  };
}
/**
 * 抽取出独立的，对homework的增删逻辑
 */
function useCourses(courses: Ref<Course[]>) {
  const addItem = () => {
    courses.value.push({
      id: courses.value.length + 1,
      name: "Vue 3.0",
      createTime: new Date().toISOString()
    });
  };
  const removeItem = (index: number) => {
    // splice()参数，预删除元素索引，删除之后的几个
    courses.value.splice(index, 1);
  };
  return {
    addItem,
    removeItem
  };
}

export default defineComponent({
  setup() {
    const courses: Ref<Course[]> = ref(listCourses());
    const { formatDate } = useDateFormat();
    const { addItem, removeItem } = useCourses(courses);
    return {
      courses,
      formatDate,
      addItem,
      removeItem
    };
  }
});
</script>
