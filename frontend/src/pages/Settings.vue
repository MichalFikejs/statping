<template>
    <div class="col-12">
        <div class="row">
            <div class="col-md-3 col-sm-12 mb-4 mb-md-0">
                <div class="nav flex-column nav-pills" id="v-pills-tab" role="tablist" aria-orientation="vertical">
                    <h6 class="text-muted">Main Settings</h6>

                    <a @click.prevent="changeTab" class="nav-link" v-bind:class="{active: liClass('v-pills-home-tab')}" id="v-pills-home-tab" data-toggle="pill" href="#v-pills-home" role="tab" aria-controls="v-pills-home" aria-selected="true">
                        <font-awesome-icon icon="cog" class="mr-2"/> Settings
                    </a>
                    <a @click.prevent="changeTab" class="nav-link" v-bind:class="{active: liClass('v-pills-style-tab')}" id="v-pills-style-tab" data-toggle="pill" href="#v-pills-style" role="tab" aria-controls="v-pills-style" aria-selected="false">
                        <font-awesome-icon icon="image" class="mr-2"/> Theme Editor
                    </a>
                    <a @click.prevent="changeTab" class="nav-link" v-bind:class="{active: liClass('v-pills-cache-tab')}" id="v-pills-cache-tab" data-toggle="pill" href="#v-pills-cache" role="tab" aria-controls="v-pills-cache" aria-selected="false">
                        <font-awesome-icon icon="paperclip" class="mr-2"/> Cache
                    </a>
                    <a @click.prevent="changeTab" class="nav-link" v-bind:class="{active: liClass('v-pills-oauth-tab')}" id="v-pills-oauth-tab" data-toggle="pill" href="#v-pills-oauth" role="tab" aria-controls="v-pills-oauth" aria-selected="false">
                        <font-awesome-icon icon="key" class="mr-2"/> OAuth <span class="mt-1 float-right badge badge-light text-dark font-1">BETA</span>
                    </a>

                    <h6 class="mt-4 text-muted">Notifiers</h6>

                    <div id="notifiers_tabs">
                        <a v-for="(notifier, index) in notifiers" v-bind:key="`${notifier.method}`" @click.prevent="changeTab" class="nav-link text-capitalize" v-bind:class="{active: liClass(`v-pills-${notifier.method.toLowerCase()}-tab`)}" v-bind:id="`v-pills-${notifier.method.toLowerCase()}-tab`" data-toggle="pill" v-bind:href="`#v-pills-${notifier.method.toLowerCase()}`" role="tab" v-bind:aria-controls="`v-pills-${notifier.method.toLowerCase()}`" aria-selected="false">
                            <font-awesome-icon :icon="iconName(notifier.icon)" class="mr-2"/> {{notifier.title}}
                            <span v-if="notifier.enabled" class="badge badge-pill float-right mt-1" :class="{'badge-success': !liClass(`v-pills-${notifier.method.toLowerCase()}-tab`), 'badge-light': liClass(`v-pills-${notifier.method.toLowerCase()}-tab`), 'text-dark': liClass(`v-pills-${notifier.method.toLowerCase()}-tab`)}">ON</span>
                        </a>
                        <a @click.prevent="changeTab" class="nav-link text-capitalize" v-bind:class="{active: liClass(`v-pills-notifier-docs-tab`)}" v-bind:id="`v-pills-notifier-docs-tab`" data-toggle="pill" v-bind:href="`#v-pills-notifier-docs`" role="tab" v-bind:aria-controls="`v-pills-notifier-docs`" aria-selected="false">
                            <font-awesome-icon icon="question" class="mr-2"/> Variables
                        </a>
                    </div>

                    <h6 class="mt-4 mb-3 text-muted">Statping Links</h6>

                    <a href="https://github.com/statping/statping/wiki" class="mb-2 font-2 text-decoration-none text-muted">
                        <font-awesome-icon icon="question" class="mr-3"/> Documentation
                    </a>

                    <a href="https://github.com/statping/statping/wiki/API" class="mb-2 font-2 text-decoration-none text-muted">
                        <font-awesome-icon icon="laptop" class="mr-2"/> API Documentation
                    </a>

                    <a href="https://raw.githubusercontent.com/statping/statping/master/CHANGELOG.md" class="mb-2 font-2 text-decoration-none text-muted">
                        <font-awesome-icon icon="book" class="mr-3"/> Changelog
                    </a>

                    <a href="https://github.com/statping/statping" class="mb-2 font-2 text-decoration-none text-muted">
                        <font-awesome-icon icon="code-branch" class="mr-3"/> Statping Github Repo
                    </a>

                    <div class="row justify-content-center mt-2">
                        <github-button href="https://github.com/statping/statping" data-icon="octicon-star" data-show-count="true" aria-label="Star Statping on GitHub">Star</github-button>
                    </div>

                </div>

            </div>
            <div class="col-md-9 col-sm-12">

                <div class="tab-content" id="v-pills-tabContent">
                    <div class="tab-pane fade" v-bind:class="{active: liClass('v-pills-home-tab'), show: liClass('v-pills-home-tab')}" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">
                        <CoreSettings/>
                    </div>

                    <div class="tab-pane fade" v-bind:class="{active: liClass('v-pills-style-tab'), show: liClass('v-pills-style-tab')}" id="v-pills-style" role="tabpanel" aria-labelledby="v-pills-style-tab">
                        <ThemeEditor/>
                    </div>

                    <div class="tab-pane fade" v-bind:class="{active: liClass('v-pills-cache-tab'), show: liClass('v-pills-cache-tab')}" id="v-pills-cache" role="tabpanel" aria-labelledby="v-pills-cache-tab">
                        <Cache/>
                    </div>

                    <div class="tab-pane fade" v-bind:class="{active: liClass('v-pills-oauth-tab'), show: liClass('v-pills-oauth-tab')}" id="v-pills-oauth" role="tabpanel" aria-labelledby="v-pills-oauth-tab">
                        <OAuth/>
                    </div>

                    <div class="tab-pane fade" v-bind:class="{active: liClass(`v-pills-notifier-docs-tab`), show: liClass(`v-pills-notifier-docs-tab`)}" v-bind:id="`v-pills-notifier-docs-tab`" role="tabpanel" v-bind:aria-labelledby="`v-pills-notifier-docs-tab`">
                        <Variables/>
                    </div>

                    <div v-for="(notifier, index) in notifiers" v-bind:key="`${notifier.method}_${index}`" class="tab-pane fade" v-bind:class="{active: liClass(`v-pills-${notifier.method.toLowerCase()}-tab`), show: liClass(`v-pills-${notifier.method.toLowerCase()}-tab`)}" v-bind:id="`v-pills-${notifier.method.toLowerCase()}-tab`" role="tabpanel" v-bind:aria-labelledby="`v-pills-${notifier.method.toLowerCase()}-tab`">
                        <Notifier :notifier="notifier"/>
                    </div>

                </div>
            </div>

        </div>
    </div>
</template>

<script>
  import Api from '../API';
  import GithubButton from 'vue-github-button'
  import Variables from "@/components/Dashboard/Variables";

  const CoreSettings = () => import('@/forms/CoreSettings')
  const FormIntegration = () => import('@/forms/Integration')
  const Notifier = () => import('@/forms/Notifier')
  const OAuth = () => import('@/forms/OAuth')
  const ThemeEditor = () => import('@/components/Dashboard/ThemeEditor')
  const Cache = () => import('@/components/Dashboard/Cache')

  export default {
      name: 'Settings',
      components: {
        Variables,
        GithubButton,
        OAuth,
          Cache,
          ThemeEditor,
          FormIntegration,
          Notifier,
          CoreSettings
      },
      data() {
          return {
              tab: "v-pills-home-tab",
          }
      },
      computed: {
          core() {
              return this.$store.getters.core
          },
          notifiers() {
           return this.$store.getters.notifiers.sort((a, b) => {return a.title-b.title});
          }
      },
    mounted() {
        this.update()
      },
    created() {
          this.update()
      },
      methods: {
        async update() {

        },
          changeTab(e) {
              this.tab = e.target.id
          },
          liClass(id) {
              return this.tab === id
          },
      }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
