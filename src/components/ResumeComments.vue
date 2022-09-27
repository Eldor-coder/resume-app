<template>
    <p>
      <button @click="this.$emit('comment-btn')" class="btn primary">Загрузить комментарии</button>
    </p>
    <div class="card">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="(author) in authors" :key="author.id">
          <div >
            <p><strong>{{author.name}}</strong></p>
            <small>{{author.company.catchPhrase}}</small>
          </div>
        </li>
      </ul>
    </div>
    
</template>

<script>
export default {
  emits : ['comment-btn'],
  props : ['authors']
}
</script>

<style>

</style>