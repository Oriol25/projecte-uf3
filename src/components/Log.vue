<template>
    
</template>

<script lang="ts">
    
    import { defineComponent } from 'vue'
    import Swal from 'sweetalert2'
    type Profile = {
        name: String,
        email: String,
        tel: String,
    }
    export default defineComponent({
        data() {
            return {
                profile: {
                    name: '',
                    email: '',
                    tel: ''
                } as Profile,
                errors: [] as String[],
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
                    if (this.validateData(nameInput.value, emailInput.value, telInput.value)) {
                        Swal.showValidationMessage(this.errors.join('<br>'))
                        console.log(this.errors.join('<br>'))
                        this.errors = []
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
            validateName(name: String){  
                if (!name) {
                    this.errors.push("El camp nom es obligatori"); 
                    
                    return
                }

                if (!name.match(/^[A-Z][a-z]{1,}$/)) {
                    this.errors.push("Nom incorrecte, exemple: Samuel")
                  
                }
                
                
            },
            validateEmail(email: String) {
                if (!email) {
                    this.errors.push("El camp email es obligatori");
                    return
                }

                if(!email.match(/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/)){
                    this.errors.push("Email no es valid");
                    
                }

                

            },
            validateTel(tel: String): boolean {
                /* if (!tel || isNaN(tel) || tel.length != 9) {
                    return true
                } */
                
                return false
            },
            validateData(name: String, email: String, tel: String) : boolean{
                this.validateName(name);
                this.validateEmail(email);
                this.validateTel(tel);

                return (this.errors.length != 0);
            }
        }
    })

</script>

<style>

</style>