<template>
    <div>
        <div class="titlebar">
            <div class="backbtn" @click="back"><span class="fa fa-chevron-left"></span></div>
            Deployment Log
            <div class="backbtn spacer"><span class="fa fa-chevron-left"></span></div>
        </div>
        <div class="codeview">
            <input type="hidden" :value="site.name">
            <div v-if="loading" class="text-center"><span class="fa fa-spinner fa-pulse fa-2x"></span></div>
            <pre>{{ log }}</pre>
        </div>
    </div>
</template>

<script>
    import Forge from '../Forge.js';
    let forge = config ? new Forge(config.api_token) : null;

    export default {
        data: () => ({
            log: null,
            loading: true
        }),
        computed: {
            server(){
                let server = this.$store.getters.getServerById(this.$route.params.server_id);
                return server;
            },
            site(){
                let site = this.$store.getters.getSiteById(this.$route.params.site_id)
                if(! this.deployScript){
                    this.getDeploymentLog(site)
                }
                return site
            }
        },
        props: [],
        methods: {
            back(){
                return this.$router.push('/servers/' + this.server.id + '/sites/' + this.site.id)
            },
            getDeploymentLog(site){
                forge.getDeploymentLog(site)
                    .then(r => {
                        console.log(r);
                        this.loading = false;
                        this.log = r;
                    });
            }
        },
        mounted(){
        }
    }
</script>
