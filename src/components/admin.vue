<template>
    <div>
        <div class="form-group">
            <h2>上传语音</h2>
            <label>
                语音中文名称
                <input type="text" v-model="newVoice['zh_CN']" class="form-control">
            </label>
            <label>
                语音日文名称
                <input type="text" v-model="newVoice['ja_JP']" class="form-control">
            </label>
            <label class="form-control-static">选择音频文件
                <input type="file" id="file" @change="getFile"></label>

            <button type="submit" @click="uploadItem" class="btn btn-primary">上传</button>
        </div>
        <h2>分类</h2>
        <table class="table">
            <thead>
            <tr>
                <th scope="col">分类序号(categoryID)</th>
                <th scope="col">分类中文名(zh-CN')</th>
                <th scope="col">分类日文名（ja-JP)</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="category in voices" :key="category.categoryName">
                <td><input v-model="category['categoryName']"></td>
                <td><input v-model="category['categoryDescription']['zh-CN']"></td>
                <td><input v-model="category['categoryDescription']['ja-JP']"></td>
            </tr>
            </tbody>
        </table>
        <div style="text-align: center">
            <button type="button" style="margin: auto" @click="addCategory" class="btn btn-primary">添加分类</button>
        </div>

        <h2>音频列表</h2>
        <table class="table" v-for="category in voices" :key="category['categoryName']" id="voiceList">
            <thead>
            <tr>
                <th scope="col">分类(categoryID)</th>
                <th scope="col">音频文件名(filename)</th>
                <th scope="col">音频中文名（zh-CN)</th>
                <th scope="col">音频日文名（ja-JP)</th>
                <th scope="col">操作</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="voice in category.voiceList" :key="voice['path']">
                <td><input v-model="category['categoryName']"></td>
                <td><input v-model="voice['path']"></td>
                <td><input v-model="voice['description']['zh-CN']"></td>
                <td><input v-model="voice['description']['ja-JP']"></td>
                <td>
                    <button class="btn btn-primary" data-toggle="modal" data-target="#moveModal"
                            @click="getInlineTableData(voice, category['categoryName'])">移动
                    </button>
                </td>
            </tr>
            </tbody>
        </table>
        <div class="modal fade" id="moveModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span
                                aria-hidden="true">&times;</span></button>
                        <h4 class="modal-title" id="myModalLabel">移动</h4>
                    </div>
                    <div class="modal-body">
                        <label>
                            <select v-model="targetCategoryID">
                                <option v-for="category in voices" :key="category['categoryName']" :value="category.categoryName">
                                    {{category['categoryDescription']['zh-CN']}}
                                </option>
                            </select>
                        </label>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-default" data-dismiss="modal">退出</button>
                        <button type="button" class="btn btn-primary" @click="moveVoice" data-dismiss="modal">保存</button>
                    </div>
                </div>
            </div>
        </div>
        <button class="btn btn-primary" style="float: right" @click="submitConfig">提交配置</button>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: "Admin",
        data() {
            return {
                voices: [],
                file: '',
                newVoice: {},
                currentVoice: undefined,
                currentCategoryID: undefined,
                targetCategoryID: undefined
            }
        },
        methods: {
            getVoice() {
                axios.get('voices.json').then(Response => (this.voices = Response['data']['voices']))
            },
            addCategory() {
                let model = {
                    "categoryName": "", "categoryDescription": {"zh-CN": "", "ja-JP": ""}, "voiceList": []
                };
                this.voices.push(model)
            },
            submitConfig() {
                axios.post('http://natsuiromatsuri.moe/api/submit', {
                    voices: this.voices
                })
            },
            uploadItem() {
                let form = new FormData();
                form.append('file', this.file);
                form.append('zh_CN', this.newVoice['zh_CN']);
                form.append('ja_JP', this.newVoice['ja_JP']);
                let config = {
                    headers: {'Content-Type': 'multipart/form-data'}
                };
                axios.post("http://natsuiromatsuri.moe/api/upload", form, config).then(
                    response => (console.log(response.data))
                );

            },
            getFile(e) {
                this.file = e.target.files[0]
            },
            getInlineTableData(voice, currentCategoryID) {
                console.log(currentCategoryID);
                this.currentVoice = voice;
                this.currentCategoryID = currentCategoryID;
            },
            moveVoice() {
                for (let c of this.voices) {
                    if (c.categoryName === this.currentCategoryID) {
                        c.voiceList.splice(c.voiceList.indexOf(this.currentVoice), 1);
                        for (let nc of this.voices) {
                            if (nc.categoryName === this.targetCategoryID) {
                                nc.voiceList.push(this.currentVoice);
                            }
                        }
                    }
                }
            },
        },
        created() {
            this.getVoice()
        }
    }
</script>

<style scoped>

</style>