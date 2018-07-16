<template>
<div class="all-container flex flex-column">
    <div class="header flex justify-center align-center">
        <p v-if="!responding">小川</p>
        <p v-else>对方正在输入......</p>
    </div>
    <div class="chat-container flex flex-column">
        <div v-for="(item, index) in chatList" :key="index">
            <div class="xiaochuan-container flex" v-if="item.role === 1">
                <div style="width:30px;height:30px;background:red;"></div>
                <div style="position: relative;">
                    <div class="xiaochuan-triangle"></div>
                    <p class="xiaochuan-content">{{item.content}}</p>
                </div>
            </div>
            <div class="xiaochuan-container flex justify-end" v-else>
                <div style="position: relative;">
                    <div class="xiaochuan-triangle-r"></div>
                    <p class="xiaochuan-content-r">{{item.content}}</p>
                </div>
                <div style="width:30px;height:30px;background:red;"></div>
            </div>
        </div>
    </div>
    <div class="footer">
        <p @click="openSheet">说点儿什么吧 ......</p>
    </div>
    <action-sheet ref="actionSheet">
        <div class="sheet-one sheet-title flex justify-between">
            <p>选点儿什么吧 ......</p>
            <p @click="closeSheet">X</p>
        </div>
        <div class="sheet-one flex justify-between" v-for="(item, index) in getOptionsById(currentId)" :key="index" v-if="item" @click="selectOption(currentId, index)">
            <p >{{item}}</p>
        </div>
        <div class="sheet-one flex justify-between" v-else @click="reload">
            <p>重来</p>
        </div>

    </action-sheet>
</div>
</template>

<script type="text/javascript">
import ActionSheet from "@/components/ActionSheet";
import "@/css/common.css";
export default {
    name: "Main",
    data() {
        return {
            chatList: [],
            currentId: 0,
            responding: false,
            chatData: {
                0: [{
                    title: "你好呀",
                    response: "hi",
                    id: 1
                }, {
                    title: "sorry，见错了",
                    response: "no no no",
                    id: 2
                }],
                1: [{
                    title: "ni hao ya",
                    response: "yeyeye",
                    id: null
                }, {
                    title: "hahaha",
                    response: "yyyyyy",
                    id: null
                }],
                2: [{
                    title: "aa bb cc",
                    response: "dd ee ff",
                    id: null
                }]
            }
        }
    },
    computed: {

    },
    methods: {
        openSheet() {
            this.$refs.actionSheet.openSheet();
        },
        closeSheet() {
            this.$refs.actionSheet.closeSheet();
        },
        getOptionsById(id) {
            if(id == null) {
                return [""]
            }
            return this.chatData[id].map((item, index) => {
                return item.title
            })
        },
        selectOption(id, index) {
            if(id !== null) {
                this.$refs.actionSheet.closeSheet();
                const chatItem = this.chatData[id][index];
                this.chatList.push({
                    role: 0,
                    content: chatItem.title
                })
                this.responding = true;
                setTimeout(() => {
                    this.chatList.push({
                        role: 1,
                        content: chatItem.response
                    });
                    this.currentId = chatItem.id;
                    this.responding = false;
                }, 2000)
            }
            else {
                alert("over")
            }
        },
        reload() {
            window.location.reload();
        }
    },
    components: {
        actionSheet: ActionSheet
    }
}
</script>

<style scoped>
.all-container {
    width: 100vw;
    height: 100vh;
    background-color: #eeeeee;
}
.header {
    height: 50px;
    background-color: black;
}
.header p {
    font-size: 17px;
    color: white;
}
.footer {
    height: 52px;
    line-height: 52px;
    background-color: white;
}
.footer p {
    font-size: 16px;
    color: #666666;
    margin-left: 10px;
}

.chat-container {
    padding: 0 10px 20px 10px;
    height: calc(100vh - 52px - 50px);
    overflow: scroll;
}

.xiaochuan-container {
    margin-top: 20px;
    flex-shrink: 0;
}

.xiaochuan-triangle {
    position: absolute;
    width: 10px;
    height: 10px;
    border-color: transparent white transparent transparent;
    border-width: 10px 20px 10px 20px;
    border-style: solid;
    left: -10px;
}

.xiaochuan-triangle-r {
    position: absolute;
    width: 10px;
    height: 10px;
    border-color: transparent transparent transparent green;
    border-width: 10px 20px 10px 20px;
    border-style: solid;
    right: -10px;
}

.xiaochuan-content, .xiaochuan-content-r {
    max-width: calc(100vw - 20px - 60px - 10px);
    margin-left: 20px;
    padding: 5px 10px;
    background-color: white;
    color: #333333;
    border-radius: 5px;
    -webkit-border-radius: 5px;
}

.xiaochuan-content-r {
    margin-right: 20px;
    margin-left: 0;
    background-color: green;
}

.sheet-one {
    line-height: 50px;
    height: 50px;
    border-bottom: 1px solid #eeeeee;
    background-color: white;
    padding: 0 10px;
    color: #333333;
    font-size: 15px;
}

.sheet-title {
    color: #666666;
    margin-left: 15px;
    font-size: 14px;
}
</style>
