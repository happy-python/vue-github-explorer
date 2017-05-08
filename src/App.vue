<template>
    <div class="container">
        <Alert v-if="alert" @alert-close="alert=false">
            <template slot="msg">
                <div class="text-center">
                    {{ errorMsg }}
                </div>
            </template>
        </Alert>
        <div class="row-fluid">
            <div class="span12">
                <div class="text-center">
                    <h3>GitHub File Explorer</h3>
                </div>
            </div>
        </div>
        <div class="text-center">
            <form class="form-inline" @submit.prevent="changeRepo">
                <div class="form-group">
                    <input type="text" v-model="fullRepoName" class="form-control" placeholder="仓库全称">
                </div>
                <input type="submit" class="btn btn-primary" value="获取">
            </form>
        </div>
        <hr>
        <GithubFileExplorer :fullRepoUrl="fullRepoName" ref="child" @show="visible=true" @invisible="invisible" v-show="visible"></GithubFileExplorer>
    </div>
</template>

<script>
import GithubFileExplorer from './components/GithubFileExplorer'
import Alert from './components/Alert'

export default {
    data() {
        return {
            fullRepoName: '',
            alert: false,
            visible: false,
            errorMsg: ''
        }
    },
    components: {
        GithubFileExplorer,
        Alert
    },
    methods: {
        changeRepo() {
            if (this.fullRepoName) {
                this.$refs.child.getFiles()
            } else {
                this.alert = true
                this.errorMsg = '仓库全称不能为空哦^_^'
            }
        },
        invisible(error){
            this.visible = false
            this.alert = true
            this.errorMsg = error.msg
        }
    }
}
</script>

<style>

</style>
