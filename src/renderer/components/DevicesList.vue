<template>
    <div v-loading="loading">
        <mu-flexbox>
                <mu-raised-button @click="refreshDevice" :label="$t('messages.refresh_button')" class="demo-raised-button" primary/>
                <mu-raised-button @click="rejectDevice" :label="$t('messages.reject_button')" class="demo-raised-button" primary/>
        </mu-flexbox>

        <mu-list>
            <mu-list-item v-for="item in devices" :key="item.device_path" :title="item.device_info._Mount_Point"
                          @click="toggle($event,item)">
                <mu-avatar slot="leftAvatar">
                    <i class="iconfont icon-cipan"></i>
                </mu-avatar>
                <mu-linear-progress class="line-progress" slot="describe"  mode="determinate"
                                    :max="parseInt(item.device_info._Volume_Total_Space)"
                                    :value="parseInt(item.device_info._Volume_Used_Space)"/>
                  <div class="authority" slot="right">
                      <span v-bind:class="{green:true}">可读（R）</span>
                      <span v-bind:class="{green:!item.read_only,red:item.read_only}">可写（W）</span>
                  </div>
            </mu-list-item>
        </mu-list>

        <mu-popover :trigger="trigger" :open="open" :anchorOrigin="anchorOrigin" :targetOrigin="targetOrigin"
                    @close="handleClose">
            <mu-menu>
                <mu-menu-item @click="_mountOpenDevices" v-if="select_item.read_only" :title="$t('messages.mount_open')"/>
                <mu-menu-item @click="_openInFinder" :title="$t('messages.open_finder')"/>
                <mu-menu-item @click="_mountDevices" v-if="select_item.read_only" :title="$t('messages.mount')"/>
                <mu-menu-item @click="showDialog" :title="$t('messages.info')"/>
            </mu-menu>
        </mu-popover>

        <mu-dialog :open="dialog" :title="$t('messages.info')" scrollable>
            <mu-table multiSelectable enableSelectAll :showCheckbox="false" ref="table">
                <mu-thead>
                    <mu-tr>
                        <mu-th>{{$t('messages.info')}}</mu-th>
                        <mu-th>{{$t('messages.tab_key_value')}}</mu-th>
                    </mu-tr>
                </mu-thead>
                <mu-tbody>
                    <mu-tr v-bind:key="key" v-for="(item,key) in select_item.device_info">
                        <mu-td>{{key.replace(/_+/ig, '')}}</mu-td>
                        <mu-td>{{item}}</mu-td>
                    </mu-tr>
                </mu-tbody>
            </mu-table>
            <mu-flat-button slot="actions" @click="closeDialog()" primary :label="$t('messages.ok')"/>
        </mu-dialog>
    </div>
</template>

<script>
import { getDevices, openInFinder, mountDevices,allRejected } from "@/utils/utils";

export default {
  name: "DevicesList",
  data() {
    return {
      loading: false,
      open: false,
      dialog: false,
      trigger: null,
      select_item: {},
      devices: [],
      anchorOrigin: { vertical: "center", horizontal: "middle" },
      targetOrigin: { vertical: "top", horizontal: "left" }
    };
  },
  mounted() {
    this.refreshDevice();
  },
  methods: {
    toggle(event, item) {
      this.select_item = item;
      this.trigger = event.currentTarget;
      this.open = !this.open;
    },
    refreshDevice() {
      this.loading = true;
      getDevices()
        .then(res => {
          console.log(res);
          this.loading = false;
          this.devices = res;
        })
        .catch(e => {
          this.devices = [];
          this.loading = false;
        });
    },
    rejectDevice(){
      allRejected().then(res=>{
        console.log("all device rejected !");
      }).catch(e=>{
        console.log("rejectd errors !");
      })
    },
    handleClose(e) {
      this.open = false;
    },
    showDialog() {
      this.open = false;
      this.dialog = true;
    },
    closeDialog() {
      this.dialog = false;
    },
    _openInFinder() {
      this.open = false;
      openInFinder(this.select_item.device_info._Mount_Point)
        .then(() => {
          this.$notify({
            title: this.$t("messages.prompt"),
            message: this.$t("messages.open_ok"),
            type: "success"
          });
        })
        .catch(e => {
          this.$notify.error({
            title: this.$t("messages.error"),
            message: e.message
          });
        });
    },
    _mountDevices() {
      mountDevices(
        this.select_item.device_path,
        this.select_item.device_info._Mount_Point
      )
        .then(() => {
          this.$notify({
            title: this.$t("messages.prompt"),
            message: this.$t("messages.mount_ok"),
            type: "success"
          });
          this.refreshDevice();
        })
        .catch(e => {
          this.$notify.error({
            title: this.$t("messages.error"),
            message: e.message
          });
        });
      this.handleClose();
    },
    _mountOpenDevices() {
      mountDevices(
        this.select_item.device_path,
        this.select_item.device_info._Mount_Point
      )
        .then(() => {
          this.$notify({
            title: this.$t("messages.prompt"),
            message: this.$t("messages.open_ok"),
            type: "success"
          });
          this.refreshDevice();
          this._openInFinder(this.select_item.device_info._Mount_Point);
        })
        .catch(e => {
          this.$notify.error({
            title: this.$t("messages.error"),
            message: e.message
          });
        });
    }
  }
};
</script>

<style scoped>
.mu-flexbox .mu-raised-button{
  margin:40px auto 20px;
}
.mu-list>div{
  /* background:#e0e0e0; */
  border-bottom:1px solid #eee;
}
.authority{
  display: flex;
  flex-direction: row;
  padding-right: 100px;
}
.authority span{
  width:80px;
}
.line-progress{
  width:60%;
}
.mu-tr .mu-td:nth-child(1){
  width:60px;
}
.green {
  color: green;
}

.red {
  color: red;
}
</style>
