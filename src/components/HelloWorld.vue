<script setup>
import ObjectItem from "./ObjectItem.vue";
// import { ref } from "vue";

// 可以继续遍历的类型
const objectToInit = ["Object", "Array", "Set", "Map", "Arguments"];

// 判断是否是引用类型
function isObject(o) {
  return o !== null && (typeof o === "object" || typeof o === "function");
}
// 判断具体的数据类型
function getType(o) {
  return Object.prototype.toString.call(o).slice(8, -1);
}
// 初始化函数
function initCloneTarget(target) {
  return new target.constructor();
}
// 拷贝 Symbol
function cloneSymbol(target) {
  return Object(target.valueOf());
  // 或者
  // return Object(Symbol.prototype.valueOf.call(target));
  // 或者
  // return Object(Symbol(target.description));
}
// 拷贝正则对象
function cloneReg(target) {
  const reFlags = /\w*$/;
  const result = new RegExp(target.source, reFlags.exec(target));
  result.lastIndex = target.lastIndex;
  return result;
}
// 拷贝函数
function cloneFunction(target) {
  return new Function(`return (${target})()`);
}

// 处理不能继续遍历的类型
function directCloneTarget(target, type) {
  let _constructor = target.constructor;
  switch (type) {
    case "String":
    case "Boolean":
    case "Number":
    case "Error":
    case "Date":
      return new _constructor(target.valueOf());
      // 或者
      return new Object(_constructor.prototype.valueOf.call(target));
    case "RegExp":
      return cloneReg(target);
    case "Symbol":
      return cloneSymbol(target);
    case "Function":
      return cloneFunction(target);
    default:
      return null;
  }
}

// 深拷贝的核心代码
function deepClone(target, map = new WeakMap()) {
  if (!isObject(target)) return target;
  // 初始化
  let type = getType(target);
  let cloneTarget;
  if (objectToInit.includes(type)) {
    cloneTarget = initCloneTarget(target);
  } else {
    return directCloneTarget(target, type);
  }
  // 解决循环引用
  if (map.has(target)) return map.get(target);
  map.set(target, cloneTarget);
  // 拷贝 Set
  if (type === "Set") {
    target.forEach((value) => {
      cloneTarget.add(deepClone(value, map));
    });
  }
  // 拷贝 Map
  else if (type === "Map") {
    target.forEach((value, key) => {
      cloneTarget.set(key, deepClone(value, map));
    });
  }
  // 拷贝对象字面量、数组、类数组对象
  else if (type === "Object" || type === "Array" || type === "Arguments") {
    Reflect.ownKeys(target).forEach((key) => {
      cloneTarget[key] = deepClone(target[key], map);
    });
  }
  return cloneTarget;
}

const propertyList = [
  "姓名",
  "年龄",
  "性别",
  "undefined",
  "Symbol value",
  "Symbol key",
  "Infinity",
  " -Infinity",
  "生日",
  "技能",
  "同学",
  "map",
  "set",
  "function",
  "正则",
  "error",
];

const symbolKey = Symbol("名人");

let obj = {
  name: "李白",
  age: 61,
  male: true,
  birthday: new Date("701-01-02"),
  skills: ["写诗", "喝酒", "舞剑"],
  classmate: [
    {
      name: "杜甫",
      age: 56,
    },
    {
      name: "白居易",
      age: 63,
    },
  ],
  map: new Map([["age", 61]]),
  set: new Set(["libai"]),
  reg: /\d+/gi,
  error: new Error("写诗"),
  a: undefined,
  b: Symbol(),
  [symbolKey]: "李白",
  d: Infinity,
  e: -Infinity,
  getName: function () {
    console.log(this.name);
  },
};

console.log("obj", obj[symbolKey]);

// console.log(`${symbolKey}`);

// obj.f = obj;
let cloneObj = { ...obj };

// obj.name = "小王";
// obj.age = 100;
// obj.male = false;
// obj.classmate[1].age = 100;
// obj.set.add("1123");
// obj.skills.push("计算机");
// obj.name = "小梅";
// obj.map.set("name", "小明");
// obj.c = 0;
// obj.d = -Infinity;
// obj.e = Infinity;

console.log("obj", obj);
console.log("cloneObj", cloneObj);
// obj.getName();
// cloneObj.getName();

const a = () => {};
</script>

<template>
  <div class="wrap">
    <div style="font-size: 2.5rem; margin-bottom: 10px">深拷贝和浅拷贝</div>

    <div style="display: flex">
      <div>
        <div style="font-size: 2rem; margin-bottom: 10px; color: white">
          类型
        </div>
        <div
          v-for="item in propertyList"
          :key="item"
          class="text"
          style="min-height: 35px; border-bottom: 1px solid #ebeef5"
        >
          {{ item }}
        </div>
      </div>
      <ObjectItem
        :beforeObj="obj"
        :cloneObj="cloneObj"
        :isClone="false"
        :symbolKey="symbolKey"
      />
      <ObjectItem :beforeObj="obj" :cloneObj="cloneObj" :isClone="true" />
    </div>
  </div>
</template>

<style scoped>
.wrap {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 40px;
}
.success {
  color: #4fc08d;
}
.title {
  font-size: 2rem;
}

.text {
  font-size: 1.5rem;
}
</style>
