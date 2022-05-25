<template>
    <div>
      <slot name="tools"/>
      <hr/>
      <slot name="preview"/>
    </div>
</template>

<script>
export default {
  name: 'ImageResizer',

}
</script>