<template>
  <div class="hello">
    <input v-model="a">
    <div v-if="flag" id="demo">{{fullName}}</div>
    <div v-show="flag" id="demo">{{fullName}}</div>
    <ul id="example-2">
      <li v-for="item in items">
        {{ parentMessage }} - {{ $index }} - {{ item.message }}
      </li>
    </ul>
    <button v-on:click="doSome()">this is a button</button>
  </div>
</template>
<script>
export default {
  data () {
    return {
      // note: changing this line won't causes changes
      // with hot-reload because the reloaded component
      // preserves its current state and we are modifying
      // its initial state.
      msg: 'Hello World!',
      a: 1,
      firstName: 'Foo',
      lastName: 'Bar',
      flag: 1,
      parentMessage: 'Parent',
      items: [
        { message: 'Foo' },
        { message: 'Bar' }
      ]
    }
  },
  computed: {
    // 一个计算属性的 getter
    b: function () {
      // `this` 指向 vm 实例
      return this.a + 1
    },
    fullName: {
      get: function () {
        return this.firstName + this.lastName
      },
      set: function (newValue) {
        var names = newValue.split(' ')
        this.firstName = names[0]
        this.lastName = names[names.length - 1]
      }
    }
  },
  methods: {
    doSome: function () {
      this.items = this.items.filter(function (item) {
        return item.message.match(/Foo/)
      })
    }
  }
}
</script>
