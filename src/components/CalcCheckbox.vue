<template>
  <div
    id="accelerated-character-div"
    style="
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 0px 0px 5px 0px;
    "
  >
    <label for="accelerated-character">Accelerated Character</label>
    <input
      type="checkbox"
      v-model="acceleratedCharacter"
      class="checkbox"
      name="accelerated-character"
      id="accelerated-character"
      style="display: block; text-align: center"
    />
  </div>
</template>

<script>
export default {};
</script>

<style></style>
