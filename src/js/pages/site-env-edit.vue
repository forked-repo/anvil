<template>
    <div>
        <div class="titlebar">
            <div class="backbtn" @click="back"><span class="fa fa-chevron-left"></span></div>
            ENV File
            <div class="backbtn" v-if="! saving" @click="save"><span class="fa fa-check"></span></div>
            <div class="backbtn" v-else><span class="fa fa-spinner fa-pulse"></span></div>
        </div>
        <div class="codeview">
            <input type="hidden" :value="site.name">
            <textarea v-model="env"></textarea>
        </div>
    </div>
</template>

<script>
    import Forge from '../Forge.js';
    let forge = config ? new Forge(config.api_token) : null;

    export default {
        data: () => ({
            env: null,
            saving: true
        }),
        computed: {
            server(){
                let server = this.$store.getters.getServerById(this.$route.params.server_id);
                return server;
            },
            site(){
                let site = this.$store.getters.getSiteById(this.$route.params.site_id)
                if(! this.env){
                    this.getEnv(site)
                }
                return site
            }
        },
        props: [],
        methods: {
            back(){
                return this.$router.push('/servers/' + this.server.id + '/sites/' + this.site.id)
            },
            save(){
                this.saving = true;
                forge.saveEnvForSite(this.site, this.env)
                    .then(r => {
                        this.env = r;
                    });
            },
            getEnv(site){
                forge.getEnvForSite(site)
                    .then(r => {
                        console.log(r);
                        this.env = r;
                        this.saving = false;
                    });
            }
        },
        mounted(){
        }
    }
</script>
