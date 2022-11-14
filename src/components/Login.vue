<template>
    
</template>

<script lang="ts">
    
    import { defineComponent } from 'vue'
    import Swal from 'sweetalert2'

    export default defineComponent({
        data() {
            return {
                profile: {
                    name: '',
                    email: '',
                    tel: ''
                }
            }
        },
        created() {
            Swal.fire({
                title: 'Registrarse',
                showClass: {
                    popup: 'animate__animated animate__fadeInDown'
                },
                hideClass: {
                    popup: 'animate__animated animate__fadeOutUp'
                },
                html: `
                    <input type="text" id="name" class="swal2-input" placeholder="Nom">
                    <input type="text" id="email" class="swal2-input" placeholder="Correu electronic">
                    <input type="text" id="tel" class="swal2-input" placeholder="Telefon">
                `,
                confirmButtonText: 'Envia',
                focusConfirm: false,
                preConfirm: () => {
                    const nameInput = Swal.getPopup()?.querySelector('#name') as HTMLInputElement
                    const emailInput = Swal.getPopup()?.querySelector('#email') as HTMLInputElement
                    const telInput = Swal.getPopup()?.querySelector('#tel') as HTMLInputElement
                    if (this.validateName(nameInput.value) || this.validateEmail(emailInput.value) || this.validateTel(telInput.value)) {
                        Swal.showValidationMessage(`Dades incorrectes`)
                    }
                    return { name: nameInput.value, email: emailInput.value, tel: telInput.value }
                },
            }).then((result) => {
                this.profile.name = result.value?.name ?? ""
                this.profile.email = result.value?.email ?? ""
                this.profile.tel = result.value?.tel ?? ""
            })

        },
        mounted() {
            this.$emit('profile', this.profile)
        },
        methods: {
            validateName(name: String): boolean {  
                if (!name) {
                    return true
                }
                
                return false
            },
            validateEmail(email: String): boolean {
                /* if (!email && !email.match(/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/)) {
                    return true
                } */

                return false

            },
            validateTel(tel: String): boolean {
                /* if (!tel || isNaN(tel) || tel.length != 9) {
                    return true
                } */
                
                return false
            }
        }
    })

</script>

<style>

</style>