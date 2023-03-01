<template>

</template>

<script lang="ts">

import { Vue } from 'vue-class-component'; // CONVIERTE LOS COMPONENTES EN CLASES
import { Prop } from 'vue-property-decorator';
import Swal from 'sweetalert2'

export default class ShowSwal extends Vue {

    @Prop() readonly title!: string
    @Prop() readonly desc!: string
    @Prop() readonly type!: any // TODO: CAMBIAR TYPE

    async created() {
        const confirm = await this.showModal();
        if (confirm) this.$emit("closeModal");
    }

    showModal() {
        return new Promise((resolve) => {
            Swal.fire(
                {
                    title: this.title,
                    text: this.desc,
                    icon: this.type,
                    confirmButtonText: "Confirmar",
                }).then(() => {
                    resolve(true)
                }
            )
        });
    }
}


</script>