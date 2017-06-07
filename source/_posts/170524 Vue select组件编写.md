---
title: Vue select组件编写
date: 2017-05-24
tags: Vue
---

# Vue select组件编写
1. props 父组件向子组件传递参数
```
// 静态数据
<child name="pname"></name>
// 动态数据
<child :name="fname"></name>
```
2. 子组件更新父组件数据
```JavaScript
// 方法1 元素声明时定义 元素中调用
<ne-select @on-change="onSel"></ne-select>
methods: {
  setSel(value) {
    this.$emit('on-change', value);
  }
}
// 方法2 使用v-model
<ne-select v-model="sel">
data: {
  model: this.value
},
props: {
  value: {
    type: [String],
    default: ''
  },
},
methods: {
  setSel(val) {
    this.model = val;
  }
},
watch: {
  model(val) {
    this.$emit('input', val);
  }
}
```
3. 动态Class
```JavaScript
<div :class="classes"></div>
computed: {
  classes() {
    return [
      `` `${prefixCls}` ``, {
        `` [`${prefixCls}-visible`]: this.visible, ``
      }
    ];
  },
},
```
4. slot父子组件通信（子组件被包含在slot中）
```JavaScript
// 父组件中声明
beforeCreate() {
  // 子节点 选中事件
  this.$on('on-select-selected', (value) => {
    this.setSel(value);
  });
  // 子节点 添加选项
  this.$on('on-add-option', (opt) => {
    this.options.push(opt);
  });
},
// 子组件中使用
methods: {
  select() {
    this.dispatch('ne-select', 'on-select-selected', this.value);
    this.broadcast('ne-select', 'on-select-selected', this.value);
  },
  // 触发父节点on事件
  dispatch(componentName, eventName, params) {
    let parent = this.$parent || this.$root;
    let name = parent.$options.name;
    while (parent && (!name || name !== componentName)) {
      parent = parent.$parent;
      if (parent) {
        name = parent.$options.name;
      }
    }
    if (parent) {
      this.$emit.apply(parent, [eventName].concat(params));
    }
  },
  // 触发子节点on事件
  broadcast(componentName, eventName, params) {
    this.$children.forEach((child) => {
      const name = child.$options.name;
      if (name === componentName) {
        this.$emit.apply(child, [eventName].concat(params));
      } else {
        this.apply(child, [componentName, eventName].concat([params]));
      }
    });
  }
},
mounted() {
  this.dispatch('ne-select', 'on-add-option', {
    value: this.value,
    innerHTML: this.$el.innerHTML
  });
},
```

5. 主动调用组件事件
```JavaScript
<ne-select v-model="sel" @on-change="onSel" ref="testSel"></ne-select>
this.$refs.testSel.setSel('opt1');
// 组件定义
methods: {
  setSel(value) {
    this.$emit('on-change', value);
  }
},
```

6. 数组变更刷新
```JavaScript
order = [{name: 'n1'}, {name: 'n2'}, {name: 'n3'}];
// 方法1
this.$set(this.order, 2, {
  name: 'new n3',
});
// 方法2
this.order.splice(indexOfItem, 1, newValue)
```
