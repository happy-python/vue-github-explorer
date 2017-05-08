<template>
    <div class="row">
        <div class="col-md-12">
            <table class="table">
                <caption>当前路径：{{ path }}</caption>
                <thead>
                    <tr>
                        <th>目录结构</th>
                        <th class="text-right">
                            <button class="btn btn-default" @click="goBack" v-if="path !== '/'">返回</button>
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="file in sortedFiles">
                        <td>
                            <div class="file" v-if="file.type === 'file'">
                                <i class="fa fa-file-o"></i> {{ file.name }}
                            </div>
                            <div class="directory" v-if="file.type === 'dir'">
                                <i class="fa fa-folder-o"></i>
                                <a @click="changePath(file.path)">{{ file.name }}</a>
                            </div>
                        </td>
                        <td class="text-right">
                            <a :href="file.download_url" download v-if="file.type === 'file'">
                                <i class="fa fa-cloud-download"></i>
                            </a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            path: '/',
            files: []
        }
    },
    props: {
        fullRepoUrl: {
            type: String,
            required: true
        }
    },
    computed: {
        url() {
            return 'https://api.github.com/repos/' + this.fullRepoUrl + '/contents' + this.path
        },
        sortedFiles: function () {
            return this.files.slice(0).sort(function (a, b) {
                if (a.type !== b.type) {
                    if (a.type === 'dir') {
                        return -1
                    } else {
                        return 1
                    }
                } else {
                    if (a.name < b.name) {
                        return -1
                    } else {
                        return 1
                    }
                }
            })
        }
    },
    methods: {
        getFiles() {
            axios.get(this.url).then(response => {
                this.files = response.data
                this.$emit('show')
            }).catch(error => {
                this.$emit('invisible', {msg: '请求失败：404 Not Found'})
            })
        },
        changePath(path) {
            this.path = '/' + path
            this.getFiles()
        },
        goBack() {
            this.path = this.path.split('/').slice(0, -1).join('/')
            if (this.path === '') this.path = '/'
            this.getFiles()
        }
    }
}
</script>