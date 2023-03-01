<template>
    
</template>

<script lang="ts">
    
    import { Vue } from 'vue-class-component';
    import { Watch } from 'vue-property-decorator';
    import Swal from 'sweetalert2'

    /************* TYPES **************/
    import { Person } from '../types/types'
    /************* FINAL **************/

    export default class Login extends Vue {
        profile: undefined | Person =  {
            name: '',
            email: '',
            tel: ''
        }

        errors: String[] = []

        @Watch('profile.name')
        onDataChanged(value: string, oldValue: string) {
            if (this.profile?.name && this.profile?.email && this.profile?.tel) {
                // EMIT
                this.$emit('profile', this.profile)
            }
        }

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
                        Swal.showValidationMessage(this.errors.join("<br>"))
                    }
                    return { name: nameInput.value, email: emailInput.value, tel: telInput.value }
                },
            }).then((result) => {

                this.profile!.name = result.value?.name ?? ""
                this.profile!.email = result.value?.email ?? ""
                this.profile!.tel = result.value?.tel ?? ""
            })
        }



        validateName(name: String) {  
            if (!name) {
                this.errors.push("El campo nombre és obligatorio"); 
                return;
            }

            if (!name.match(/^[A-Z][a-z]{1,}$/)) {
                this.errors.push("Nombre incorrecto, ejemplo: Samuel")
                return;
            }  
        }

        validateEmail(email: String) {
            if (!email) {
                this.errors.push("El campo email es obligatorio");
                return;
            }

            if(!email.match(/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/)){
                this.errors.push("Email no és válido");
                return;
            }

            

        }

        validateTel(tel: String): boolean {
            if (!tel) {
                this.errors.push("El campo teléfono es obligatorio");
                return true;
            }

            if (!parseInt(tel as string)) {
                this.errors.push("El campo teléfono debe contener números");
                return true;
            }

            if (tel.length != 9) {
                this.errors.push("El campo teléfono debe contener 9 números");
                return true;
            }
            
            return false
        }

        validateData(name: String, email: String, tel: String) : boolean{
            this.errors = [];
            this.validateName(name);
            this.validateEmail(email);
            this.validateTel(tel);

            return (this.errors.length != 0);
        }

    }

</script>

<style>

</style>