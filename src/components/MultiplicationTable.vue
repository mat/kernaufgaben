<template>
    <div>
      <div v-for="(group, index) in multiplicationGroups" :key="index">
        <h3>{{ group.groupName }}</h3>
        <ul>
          <li v-for="(item, index) in group.exercises" :key="index">{{ item.num1 }} · {{ item.num2 }} = {{ item.result }}</li>
        </ul>
      </div>
    </div>
  </template>

  <script>
  import { MULTIPLICATION_GROUPS_TEXT } from './constants.js';

  export default {
    name: 'MultiplicationTable',
    data() {
      return {
        multiplicationGroups: this.parseExerciseGroups(MULTIPLICATION_GROUPS_TEXT)
      }
    },
    methods: {
    parseExerciseGroups(text) {
      const groups = text.trim().split('\n\n');
      return groups.map(group => {
        const lines = group.trim().split('\n');
        const groupName = lines[0].trim();
        const exercises = lines.slice(1).map(line => this.parseMultiplicationGroup(line.trim()));
        return { groupName, exercises };
      });
    },
    parseMultiplicationGroup(line) {
      const parts = line.split(' = ');
      const result = parseInt(parts[1], 10);

      const operands = parts[0].split(' x ');
      const num1 = parseInt(operands[0], 10);
      const num2 = parseInt(operands[1], 10);

      return { num1, num2, result };
    }
  }
  }
  </script>