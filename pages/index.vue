<template>
  <main>
    <users-table />
  </main>
</template>

<script>
import UsersTable from '../components/UsersTable.vue'
export default {
  components: { UsersTable },
  name: 'IndexPage'
}
</script>
