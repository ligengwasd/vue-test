<template>
  11111
</template>
<script>
  var func = function (x, y) {
    console.log(x, y)
  }
  var env = {
    a: 1,
    b: 2
  }
  func.call(env, env.a, env.b)
  console.log(_.isEqual(1, 2))
</script>
