<template>


  <!--  todo: move to layout or create a reusable component -->
  <div class="nc-container">
    <div class="nc-topbar shadow-2">
    </div>
    <div class="nc-sidebar shadow-2 p-4 overflow-y-auto">

    </div>
    <div class="nc-content p-4 overflow-auto">
      <div class="grid">
        <div class="col-3 p-3" v-for="project in projects" :key="project.id">

          <Card @click="navigateToDashboard(project)">
            <template #content>
              <div class="text-center">
                <h3>{{ project.title }}</h3>
              </div>
            </template>
          </Card>

        </div>
      </div>
    </div>
  </div>

</template>

<script setup lang="ts">

import {useRouter} from "#app";

const {$api} = useNuxtApp()
const {user} = useUser()



const $router = useRouter()

const projects = ref()

const loadProjects = async () => {
  const projectsResponse = await $api.project.list({}, {
    headers: {
      'xc-auth': user.token
    }
  })
  projects.value = projectsResponse.list
}

const navigateToDashboard = async (project) => {
  await $router.push('/dashboard/' + project.id)
}

onMounted(async () => {
  await loadProjects()
})
</script>

<style scoped lang="scss">
.nc-container {
  .nc-topbar {
    position: fixed;
    top: 0;
    left: 0;
    height: 50px;
    width: 100%;
    z-index: 5;
  }

  .nc-sidebar {
    position: fixed;
    top: 50px;
    left: 0;
    height: calc(100% - 50px);
    width: 250px;
  }

  .nc-content {
    position: fixed;
    top: 50px;
    left: 250px;
    height: calc(100% - 50px);
    width: calc(100% - 250px);
  }
}
</style>
