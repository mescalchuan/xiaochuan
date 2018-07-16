<template>
<div class="all-container">
    <div class="header flex justify-center align-center">
        <p v-if="!responding">小川</p>
        <p v-else>对方正在输入......</p>
    </div>
    <div class="chat-container flex flex-column">
        <div v-for="(item, index) in chatList" :key="index">
            <div class="xiaochuan-container t-o-l flex" v-if="item.role === 1">
                <img src="@/assets/xiaochuan.jpg" alt="">
                <div style="position: relative;">
                    <div class="xiaochuan-triangle"></div>
                    <p class="xiaochuan-content">{{item.content}}</p>
                </div>
            </div>
            <div class="xiaochuan-container t-o-r flex justify-end" v-else>
                <div style="position: relative;">
                    <div class="xiaochuan-triangle-r"></div>
                    <p class="xiaochuan-content-r">{{item.content}}</p>
                </div>
                <img src="@/assets/customer.jpg" alt="">
            </div>
        </div>
    </div>
    <div class="footer">
        <p @click="openSheet">说点儿什么吧 ......</p>
    </div>
    <action-sheet ref="actionSheet">
        <div class="sheet-one sheet-title flex justify-between align-center">
            <p>选点儿什么吧 ......</p>
            <img src="@/assets/close.png" alt="" @click="closeSheet">
        </div>
        <div class="sheet-one flex align-center" v-for="(item, index) in getOptionsById(currentId)" :key="index" v-if="item" @click="selectOption(currentId, index)">
            <p >{{item}}</p>
        </div>
        <div class="sheet-one flex align-center" v-else @click="reload">
            <p>重来</p>
        </div>

    </action-sheet>
</div>
</template>

<script type="text/javascript">
import ActionSheet from "@/components/ActionSheet";
import chatData from "@/model/chatData";
import "@/css/common.css";

const ROLES = {
    CUSTOMER: 0,
    XIAOCHUAN: 1
}

export default {
    name: "Main",
    data() {
        return {
            chatList: [{
                role: 1,
                content: "你好~"
            }],
            currentId: 0,
            responding: false,
            chatData
        }
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
            this.$refs.actionSheet.closeSheet();
            const chatItem = this.chatData[id][index];
            this.chatList.push({
                role: ROLES.CUSTOMER,
                content: chatItem.title
            });
            this.$nextTick(() => {
                this.scrollToBottom();
            })
            setTimeout(() => {
                this.responding = true;
            }, 800);
            setTimeout(() => {
                this.chatList.push({
                    role: ROLES.XIAOCHUAN,
                    content: chatItem.response
                });
                this.$nextTick(() => {
                    this.scrollToBottom();
                })
                this.currentId = chatItem.id;
                this.responding = false;
            }, 2000);
        },
        reload() {
            window.location.reload();
        },
        scrollToBottom() {
            const container = document.querySelector(".chat-container");
            container.scrollTop = container.scrollHeight;
        }
    },
    components: {
        actionSheet: ActionSheet
    }
}
</script>

<style scoped>
img {
    width: 30px;
    height: 30px;
}
.all-container {
    width: 100vw;
    height: 100vh;
    background-color: #eeeeee;
}
.header {
    height: 50px;
    background-color: #393B36;
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
    -webkit-overflow-scrolling: touch;
}

.chat-container::-webkit-scrollbar {
    display: none;
}

.xiaochuan-container {
    margin-top: 20px;
    flex-shrink: 0;
    animation: contentEnter .4s;
}

.t-o-l {
    transform-origin: left;
}

.t-o-r {
    transform-origin: right;
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
    border-color: transparent transparent transparent #A0E75A;
    border-width: 10px 20px 10px 20px;
    border-style: solid;
    right: -10px;
}

.xiaochuan-content, .xiaochuan-content-r {
    max-width: calc(100vw - 20px - 60px - 10px);
    margin-left: 20px;
    padding: 6px 10px;
    background-color: white;
    color: #333333;
    font-size: 15px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
}

.xiaochuan-content-r {
    margin-right: 20px;
    margin-left: 0;
    background-color: #A0E75A;
}

.sheet-one {
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

.sheet-title img {
    width: 18px;
    height: 18px;
}

@keyframes contentEnter{
    from {
        transform: scale(0);
    }
    40% {
        transform: scale(1.03);
    }
    75% {
        transform: scale(0.98);
    }
    to {
        transform:scale(1);
    }
}
</style>
