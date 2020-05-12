<template>
    <g-link :to="link" class="tag">
      {{title}}
    </g-link>
</template>
<script>
  export default {
    props: {
      link: {
        type: String,
        required: true
      },
      title: {
        type: String,
        required: true
      }
    }
  }
</script>
<style lang="scss" scoped>
.tag {
    background: #cff9e4;
    border-radius: 3px 0 0 3px;
    color: #212b33;
    display: inline-block;
    height: 26px;
    line-height: 26px;
    padding: 0 20px 0 23px;
    position: relative;
    margin: 0 10px 10px 10px;
    text-decoration: none;
    -webkit-transition: color 0.2s;

    .dark & {
      background: #eee;
    }

    &::before {
      background: $backgroundBright;
      border-radius: 10px;
      box-shadow: inset 0 1px rgba(0, 0, 0, 0.5);
      content: '';
      height: 6px;
      left: 10px;
      position: absolute;
      width: 6px;
      top: 10px;
    }

    &::after {
      background: $sidebarBright;
      border-bottom: 13px solid transparent;
      border-left: 10px solid #cff9e4;
      border-top: 13px solid transparent;
      content: '';
      position: absolute;
      right: 0;
      top: 0;

      .dark & {
        background: $sidebarDark;
        border-left: 10px solid #eee;
      }
    }

    &:hover {
      background-color: $brandPrimary;
      color: white;
      &::after {
        border-left-color: $brandPrimary;
      }
    }

}
</style>