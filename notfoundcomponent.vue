<template>
  <div class="row main_container">
    <div class="large-6 columns">
      <h1>{{text}}</h1>
    </div>
  </div>
</template>

<script>
    define(["Vue"], function(Vue) {
        return Vue.component("not-found-component", {
            template: template, // the variable template will be injected
            data: function() {
                return {
                    "text": "Sorry the page you were looking for could not be found!"
                };
            }
        });
    });
</script>