<template>
  <div class="modal-overlay" @click.self="close">
    <div class="modal-content">
      <h3>{{ day.date }}일</h3>
      <label>
        수입
        <input type="number" v-model.number="day.income" />
      </label>
      <label>
        지출
        <input type="number" v-model.number="day.expense" />
      </label>
      <button @click="close">닫기</button>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
<script>
export default {
  props: ['day'],
  methods: {
    close() {
      this.$emit('close');
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  text-align: center;
}
</style>
