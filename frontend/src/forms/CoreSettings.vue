<template>
    <div>
        <div class="card text-black-50 bg-white">
            <div class="card-header">Statping Settings</div>
            <div class="card-body">
    <form @submit.prevent="saveSettings">
        <div class="form-group">
            <label>{{ $t('settings.name') }}</label>
            <input v-model="core.name" type="text" class="form-control" placeholder="Great Uptime" id="project">
        </div>

        <div class="form-group">
            <label>{{ $t('settings.description') }}</label>
            <input v-model="core.description" type="text" class="form-control" placeholder="Great Uptime" id="description">
        </div>

        <div class="form-group row">
            <div class="col-8 col-sm-9">
                <label>{{ $t('domain') }}</label>
                <input v-model="core.domain" type="url" class="form-control" id="domain">
            </div>
            <div class="col-4 col-sm-3 mt-sm-1 mt-0">
                <label class="d-inline d-sm-none">Enable CDN</label>
                <label class="d-none d-sm-block">Enable CDN</label>
                <span @click="core.using_cdn = !!core.using_cdn" class="switch" id="using_cdn">
                    <input v-model="core.using_cdn" type="checkbox" name="using_cdn" class="switch" id="switch-normal" :checked="core.using_cdn">
                    <label for="switch-normal"></label>
                  </span>
            </div>
        </div>

        <div class="form-group">
            <label>{{ $t('settings.footer') }}</label>
            <textarea v-model="core.footer" rows="4" class="form-control" id="footer">{{core.footer}}</textarea>
            <small class="form-text text-muted">{{ $t('settings.footer_notes') }}</small>
        </div>

        <div class="form-group">
            <label>{{ $t('setup.language') }}</label>
            <select v-model="core.language" v-bind:value="core.language" class="form-control">
                <option value="en">English</option>
                <option value="es">Spanish</option>
                <option value="fr">French</option>
                <option value="ru">Russian</option>
                <option value="de">German</option>
            </select>
        </div>

        <div class="form-group row mt-3">
            <label class="col-sm-10 col-form-label">{{ $t('settings.error_reporting') }}</label>
            <div class="col-sm-2 float-right">
                <span @click="core.allow_reports = !!core.allow_reports" class="switch" id="allow_report">
                <input v-model="core.allow_reports" type="checkbox" name="allow_report" class="switch" id="switch_allow_report" :checked="core.allow_reports">
                <label for="switch_allow_report"></label>
              </span>
            </div>
            <div class="col-12">
                <small>{{ $t('settings.error_reporting_notes') }}</small>
            </div>
        </div>

        <button @click.prevent="saveSettings" id="save_core" type="submit" class="btn btn-primary btn-block mt-3" v-bind:disabled="loading">
            <font-awesome-icon v-if="loading" icon="circle-notch" class="mr-2" spin/>{{ $t('settings.save') }}
        </button>

    </form>
            </div>
        </div>

        <div class="card text-black-50 bg-white mt-3">
            <div class="card-header">API Settings</div>
            <div class="card-body">
                <div class="form-group row">
                    <label class="col-sm-3 col-form-label">API Secret</label>
                    <div class="col-sm-9">
                        <div class="input-group">
                            <input v-model="core.api_secret" @focus="$event.target.select()" type="text" class="form-control select-input" id="api_secret" readonly>
                            <div class="input-group-append copy-btn">
                                <button @click="copy(core.api_secret)" class="btn btn-outline-secondary" type="button">Copy</button>
                            </div>
                        </div>
                        <small class="form-text text-muted">API Secret is used for read, create, update and delete routes</small>
                        <small class="form-text text-muted">You can <a href="#" id="regenkeys" @click="renewApiKeys">Regenerate API Keys</a> if you need to.</small>
                    </div>
                </div>
            </div>
        </div>

        <div class="card text-black-50 bg-white mt-3">
            <div class="card-header">QR Code for Mobile App</div>
            <div class="card-body">

                <img class="rounded" width="300" height="300" :src="qrcode">

            </div>
        </div>
    </div>
</template>

<script>
  import Api from '../API'

  export default {
      name: 'CoreSettings',
    data () {
      return {
        loading: false,
        qrcode: "",
        qrurl: "",
      }
    },
      computed: {
          core() {
              return this.$store.getters.core
          }
      },
    mounted() {
        this.update()
    },
    methods: {
        async update() {
          const c = await Api.core()
          this.$store.commit('setCore', c)
          const n = await Api.notifiers()
          this.$store.commit('setNotifiers', n)

          this.qrurl = `statping://setup?domain=${c.domain}&api=${c.api_secret}`
          this.qrcode = "https://chart.googleapis.com/chart?chs=500x500&cht=qr&chl=" + encodeURI(this.qrurl)
          this.cache = await Api.cache()
        },
      async renewApiKeys() {
        let r = confirm("Are you sure you want to reset the API keys?");
        if (r === true) {
          await Api.renewApiKeys()
          const core = await Api.core()
          this.$store.commit('setCore', core)
          this.core = core
        }
      },
          async saveSettings() {
            this.loading = true
              const c = this.core
              await Api.core_save(c)
              const core = await Api.core()
              this.$store.commit('setCore', core)
            setInterval(() => { this.loading = false }, 1500)
          },
          selectAll() {
              this.$refs.input.select();
          }
      }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
