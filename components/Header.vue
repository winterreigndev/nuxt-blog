<template>
  <div>
    <div class="color-box">
      <ColorModePicker />
    </div>
    <div class="navigation">
      <div class="left site-logo">
        My First Nuxt Blog
        <small> a simple nuxt blog </small>
      </div>
      <div class="middle"></div>
      <div class="right desktop">
        <Navigation />
      </div>
      <div class="mobile">
        <label for="toggle"
          ><svg viewBox="0 0 100 80" width="40" height="40">
            <rect width="100" height="20"></rect>
            <rect y="30" width="100" height="20"></rect>
            <rect y="60" width="100" height="20"></rect></svg
        ></label>
        <input type="checkbox" id="toggle" />
        <div class="menu">
          <Navigation />
        </div>
      </div>
    </div>
  </div>
</template>

<script></script>
