<template>
  11111{{msg}}1111<br/>
  {{msgComputed}}<br/>
  <input v-model="msg"/><br/>
  <child-component v-for="i in 3" :index="i">
    <span slot="mySlot">mySlot mySlot </span>
    <span slot="mySlot2">mySlot2 mySlot2</span>
  </child-component>
  <async-component></async-component>
  <br/><br/>

  <div id="modal">111</div>
  <div v-inject:modal>
    <h1>header</h1>
    <p>body</p>
    <p>footer</p>
  </div>
</template>
<script>
  import Vue from 'vue'
  var FragmentFactory = Vue.FragmentFactory
  var remove = Vue.util.remove
  var createAnchor = Vue.util.createAnchor

  Vue.component('child-component', {
    props: ['index'],
    template: '<div><h1>This is my component! {{index}}</h1>child-component</div><div><slot name="mySlot"></slot></div><div><slot name="mySlot2"></slot></div>'
  })
  export default {
    components: {
      'async-component': function (resolve, reject) {
        setTimeout(function () {
          resolve({
            template: '<div>I am async!</div>'
          })
        }, 3000)
      }
    },
    data () {
      return {
        msg: '222',
        currentView: 'child-component'
      }
    },
    computed: {
      msgComputed: {
        cache: false,
        get: function () {
          return Date.now() + '-----' + this.msg
        }
      }
    },
    ready () {
      // this.$log(this.data)
    }
  }
  Vue.directive('demo', {
    params: ['a'],
    bind () {
      console.log('demo bound')
    },
    update (value) {
      console.log(value)
      this.el.innerHTML =
        'name - ' + this.name + '<br>' +
        'expression - ' + this.expression + '<br>' +
        'argument - ' + this.arg + '<br>' +
        'modifiers - ' + JSON.stringify(this.modifiers) + '<br>' +
        'value - ' + value
    }
  })
  Vue.directive('inject', {
    terminal: true,
    bind () {
      console.log(this)
      var container = document.getElementById(this.arg)
      this.anchor = createAnchor('v-inject')
      container.appendChild(this.anchor)
      remove(this.el)

      var factory = new FragmentFactory(this.vm, this.el)
      this.frag = factory.create(this._host, this._scope, this._frag)
      this.frag.before(this.anchor)
    },
    unbind: function () {
      this.frag.remove()
      remove(this.anchor)
    }
  })
</script>
