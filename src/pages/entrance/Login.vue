<style lang="less" module>
    @import '../../assets/less/const.less';
    @contentW: 350px;
    @item-height: 38px;
    @iconW: 40px;
   .item {
        position: relative;
        height: @item-height;
        margin-bottom: 10px;
   }
   .iconWrap {
       position: absolute;
       left: 0;
       top: 0;
       width: @iconW;
       height: @item-height;
       text-align: center;
       line-height:  @item-height;
   }
   .input {
        padding: 0 10px 0 @iconW;
        margin: 0;
        width: 100%;
        height: @item-height;
        color: @font-color-7;
        background-color: @bg-color-4;
        transition: border .3s, background-color .3s;
        border-radius: 4px;
        border: 1px solid @line-color-3;
        &:focus{
            background-color: #fff;
            border-color: @theme-color;
        }
        &.error{
            border-color: @hint-color; 
        }
   }
   .btnSubmit{
       padding-top: 10px;
       padding-bottom: 10px;
   }
   .itemHint{
       margin-top: -8px;
       margin-bottom: 10px;
       height: 20px;
       line-height: 20px;
       color:  @hint-color;
   }
 
</style>
<template>  
    <form :class="$style.content" @submit.prevent="login">
        <div :class="$style.item">
            <span :class="$style.iconWrap">
            </span>
            <input :class="$style.input" type="text" maxlength="30" v-model="user.account" placeholder="邮箱" @focus="resetHint()">
        </div>
        <div :class="$style.itemHint" v-if="!accountlHint">
            {{emailAccountText}}
        </div>
        <div :class="$style.item">
            <span :class="$style.iconWrap">
            </span>
            <input :class="$style.input" type="password" maxlength="30" v-model="user.password" placeholder="密码" @focus="resetHint()">
        </div>
         <div :class="$style.itemHint" v-if="!pwdHint">
             {{pwdErrorText}}
        </div>
        <div>
            <y-button :class="$style.btnSubmit" type="submit" :block="true" :disabled="false">{{ isLoading ?  '正在登录...' : '登录'}}</y-button>
        </div>
    </form>
</template>
<script>
    import { mapGetters } from 'vuex';
    // import $api from 'api';
    import Util from '../../util';
    export default {
        data() {
            return {
                pwdHint: true,
                accountlHint: true,
                emailAccountText: '账号不符合规范',
                user:{
                    account: '15901126559',
                    password: 'tangliang'
                }
            }
        },
        computed: {
            ...mapGetters({
                isLoading:'loginLoading'
            })
        },
        methods: {
             resetHint() {
                this.pwdHint = true;
                this.accountlHint = true;
            },
            checkaAccount(){
                let str = this.user.account.trim();
                this.accountlHint = (str.length >= 5 && str.length <= 20);
                return this.accountlHint;
            },
            checkPwd(){
                if(this.user.password.length >= 6 && this.user.password.length <= 20){
                    this.pwdHint = true;
                }else {
                    this.pwdHint = false;
                    this.pwdErrorText = '密码长度在6~20位之间';
                }
                return this.pwdHint;
            },
            validate(){
                return (this.checkaAccount() && this.checkPwd());
            },
            login(){
                if(!this.validate())return false;
                // $api.post('/user/login', this.user)
                //     .then(data => {
                //         this.$store.dispatch('account/LOGIN_SUCCESS',data);
                //         this.$store.dispatch('user/SET_USERINFO',data);
                //         this.$router.replace(this.$route.query.redirect || '/');
                //     }, data => {
                //         this.$store.dispatch('account/LOGIN_FAILE',data);
                //         this.$store.dispatch('user/SET_USERINFO',{});
                //         this.pwdHint = false;
                //         this.pwdErrorText = res.msg;
                //     })
            }
        }
    }
</script>