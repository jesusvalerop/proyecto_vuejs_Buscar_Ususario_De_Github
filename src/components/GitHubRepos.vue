<template>
    <div>
        <!-- TODO: Crear componente GitHubRepos -->
            <ul class="list-group">
                <li class="list-group-item" v-for="dato in repolist" :key="dato.id" :title="dato.description">
                    <a :href="dato.html_url" target="_blank">{{dato.full_name}}</a>
                    <span class="badge badge-primary">{{dato.forks_count}}</span>
                </li>
            </ul>
    </div>
</template>

<script>
export default {
    name: 'GitHubRepos',
    props: {
        // TODO: crear parámetro 'repolist'
        repolist: Array,
    },
}
</script>

<style scoped>
li{
    padding: 15px;
}

a{
    float: left;
    text-decoration: none;
    color: black;
}

span{
    margin-top: 5px;
    float:right;
    width: 50px;
}
</style>
