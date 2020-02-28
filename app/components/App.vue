<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
        <GridLayout columns="*" rows="*">
            <Label class="message" :text="msg" col="0" row="0"/>
        </GridLayout>
    </Page>
</template>

<script >
  import { AppSync, InstallMode, SyncStatus } from 'nativescript-app-sync'
  import * as application from 'tns-core-modules/application'

  export default {
    data() {
      return {
        msg: 'Hello World!'
      }
    },
    created () {
      application.on(application.resumeEvent, () => {
        AppSync.sync({
          deploymentKey: 'INSERT KEY HERE',
          installMode: InstallMode.ON_NEXT_RESUME,
          mandatoryInstallMode: InstallMode.IMMEDIATE
        }, (syncStatus, updateLabel) => {
          console.log('**** APPSYNC STATUS:', syncStatus)
          if (syncStatus === SyncStatus.UP_TO_DATE) {
            console.log('**** APPSYNC: UP-TO-DATE')
          } else if (syncStatus === SyncStatus.UPDATE_INSTALLED) {
            console.log('**** APPSYNC: UPDATE INSTALLED:', updateLabel)
          }
        })
      })
    }
  }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
