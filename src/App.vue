<template>
  <div>
    <textarea v-model="token" class="TokenTextarea" />
    <br />
    <div class="PayloadDiv">
      <p>ペイロード</p>
      <p>{{ decodedPayload }}</p>
      <p>期限切れ</p>
      <p>{{ expired }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import { Buffer } from "buffer";

const token = ref("");
const decodedPayload = computed(() => {
  const payload = token.value.split(".")[1];
  if (payload) {
    const decoded = JSON.parse(Buffer.from(payload, "base64").toString("utf8"));
    return JSON.stringify(decoded);
  } else {
    return undefined;
  }
});
const expired = computed(() => {
  const payload = token.value.split(".")[1];
  if (payload) {
    const decoded = JSON.parse(Buffer.from(payload, "base64").toString("utf8"));
    return new Date(decoded.exp * 1000).toLocaleString("ja-JP");
  } else {
    return undefined;
  }
});
</script>

<style scoped>
.TokenTextarea {
  width: 80%;
  height: 200px;
}

.PayloadDiv {
  border: solid 1px gray;
}
</style>
