<template>
    <div>
        <div v-if="$i18n.locale=='zh'">
            <mu-row gutter>  
                <mu-col class="header-title" width="70" tablet="70" desktop="70">
                    <h3>让我们一起开发出更好用的软件！</h3>
                </mu-col>
            </mu-row>
            <mu-row gutter > 
                <div class="scan-icon">
                        <mu-col width="35" tablet="35" desktop="35">
                            <p>微信</p>   
                            <img src="../assets/wx.png" width="150px" height="150px" alt="">
                        </mu-col>
                        <mu-col width="35" tablet="35" desktop="35">
                            <p>支付宝</p>
                            <img src="../assets/zfb.png" width="150px" height="150px" alt="">
                        </mu-col>   
                </div>
            </mu-row>
            <mu-row gutter>
                <div class="tips">
                    <ul>
                        <li>该软件依赖Mac系统的diskutil、mount、umount、mount_ntfs、open等命令，
        如果遇到问题，请先检查这些命令是否工作正常。</li>
                        <li>该项目已经在github上开源，欢迎开发者加入，完善功能。
        Github地址：<a href="javascript:void(0)" @click="open('https://github.com/Sailiy/ntfsx')">https://github.com/Sailiy/ntfsx</a>
                        </li>
                        <li>如果你想一起参与,请请发邮件到：<a href="mailto:chenchaohan@outlook.com">chenchaohan@outlook.com</a>
                        </li>
                        <li>感谢electron，vue，electron-vue，muse-ui，element-ui的开发者们。</li>
                    </ul>
                </div>  
            </mu-row>
        </div>

<!-- 下面是英文版 -->

        <div v-if="$i18n.locale=='en'">
            <mu-row gutter>
                <mu-col width="15" tablet="15" desktop="15"></mu-col>
                <mu-col width="70" tablet="70" desktop="70">
                    <h3>Please sponsor me so that I can develop better software</h3>
                </mu-col>
                <mu-col width="15" tablet="15" desktop="15"></mu-col>
            </mu-row>
            <mu-row gutter>
                <mu-col width="15" tablet="15" desktop="15"></mu-col>
                <mu-col width="35" tablet="35" desktop="35">
                    &nbsp;&nbsp;WeChat<br>
                    <img src="../assets/wx.png" width="150px" height="150px" alt="">
                </mu-col>
                <mu-col width="35" tablet="35" desktop="35">
                    &nbsp;&nbsp;AliPay<br>
                    <img src="../assets/zfb.png" width="150px" height="150px" alt="">

                </mu-col>
                <mu-col width="15" tablet="15" desktop="15"></mu-col>
            </mu-row>
            <mu-row gutter>
                <mu-col width="15" tablet="15" desktop="15"></mu-col>
                <mu-col width="70" tablet="70" desktop="70">
               <pre>
The project relies on the Mac system diskutil, mount, umount, mount_ntfs, open, command line tool,If you have problems, check to see if these tools are working properly.
                   <br>
The project has been open source on github and welcomes developers to join and improve functionality.
Making address: <a href="javascript:void(0)" @click="open('https://github.com/Sailiy/ntfsx')">https://github.com/Sailiy/ntfsx</a>
                   <br>
My ability is not only that, if you can give me good job opportunities.
Please email to: <a href="mailto:sailiy@126.com">sailiy@126.com</a>
                   <br>
Thanks to the developers of electron, vue, electronic-vue, Muse-UI, element-UI.
               </pre>
                </mu-col>
                <mu-col width="15" tablet="15" desktop="15"></mu-col>
            </mu-row>
        </div>

    </div>
</template>

<script>
export default {
  data() {
    return {
      dialog: false
    };
  },
  methods: {
    open(link) {
      this.$electron.shell.openExternal(link);
    }
  }
};
</script>

<style scoped>
.header-title {
  flex: 1;
}
.header-title h3 {
  padding-left: 4em;
}
.scan-icon {
  display: flex;
  flex: 1;
}
.scan-icon div {
  flex: 1;
  text-align: center;
}
.scan-icon div > p {
  text-align: center;
}

.tips {
  display: flex;
}

.tips li {
  line-height: 2em;
}
</style>
