<template>
  <div>
    <div>
      <div style="font-size: 2rem; margin-bottom: 10px">
        {{ isClone ? "新对象" : "源对象" }}
      </div>
      <div class="container">
        <div class="container-text">{{ obj.name }}</div>
        <div :class="isSamge('name') ? 'success' : 'error'">
          {{ typeof obj.name }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.age }}</div>
        <div :class="isSamge('age') ? 'success' : 'error'">
          {{ typeof obj.age }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.male }}</div>
        <div :class="isSamge('male') ? 'success' : 'error'">
          {{ typeof obj.male }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ `${obj.a}` }}</div>
        <div :class="isSamge('a') ? 'success' : 'error'">
          {{ typeof obj.a }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.b }}</div>
        <div :class="isSamge('b') ? 'success' : 'error'">
          {{ typeof obj.b }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.Symbo }}</div>
        <div :class="isSamge('Symbol') ? 'success' : 'error'">
          {{ typeof obj.Symbol }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.d }}</div>
        <div :class="isSamge('d') ? 'success' : 'error'">
          {{ typeof obj.d }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.e }}</div>
        <div :class="isSamge('e') ? 'success' : 'error'">
          {{ typeof obj.e }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.birthday }}</div>
        <div :class="isSamge('birthday') ? 'success' : 'error'">
          {{ typeof obj.birthday }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.skills }}</div>
        <div :class="isSamge('skills') ? 'success' : 'error'">
          {{ typeof obj.skills }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.classmate }}</div>
        <div :class="isSamge('classmate') ? 'success' : 'error'">
          {{ typeof obj.classmate }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.map }}</div>
        <div :class="isSamge('map') ? 'success' : 'error'">
          {{ typeof obj.map }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.set }}</div>
        <div :class="isSamge('set') ? 'success' : 'error'">
          {{ typeof obj.set }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.getName }}</div>
        <div :class="isSamge('getName') ? 'success' : 'error'">
          {{ typeof obj.getName }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.reg }}</div>
        <div :class="isSamge('reg') ? 'success' : 'error'">
          {{ typeof obj.reg }}
        </div>
      </div>
      <div class="container">
        <div class="container-text">{{ obj.error }}</div>
        <div :class="isSamge('error') ? 'success' : 'error'">
          {{ typeof obj.error }}
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    beforeObj: Object,
    cloneObj: Object,
    isClone: Boolean,
    symbolKey: Promise,
  },
  data() {
    return {
      obj: {},
    };
  },
  mounted() {
    this.obj = this.isClone ? this.cloneObj : this.beforeObj;
    console.log("123", this.symbolKey);
  },
  methods: {
    isSamge(key) {
      if (this.isClone) {
        return this.beforeObj[key] === this.cloneObj[key];
      }
      return true;
      ƒ;
    },
  },
};
</script>
<style scoped>
.container {
  display: flex;
  height: 35px;
  font-size: 1.5rem;
  justify-content: center;
  align-items: center;
  border-bottom: 1px solid #ebeef5;
}
.container-text {
  margin-right: 20px;
  white-space: nowrap;
  max-width: 30vw;
  overflow: hidden;
  text-overflow: ellipsis;
}
.success {
  color: #4fc08d;
  margin-right: 30px;
}

.error {
  color: red;
  margin-right: 30px;
}
</style>
