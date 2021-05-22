<template>
  <div class="about">
    <h1>This is Callback page</h1>
  </div>
</template>

<script>
export default {
    name: "Callback",
    created () {
        var result = this.$route.hash.split('&').reduce(function (res, item) {
            var parts = item.split('=');
            res[parts[0]] = parts[1];
            return res;
        }, {});
        localStorage.ACCESS_TOKEN = result['#access_token'];
        this.$router.replace({path: '/playlist'});
    }
}
</script>