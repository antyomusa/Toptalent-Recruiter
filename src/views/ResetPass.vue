<template>
    <div class="container ">
        <!-- <div class="back">
            <router-link class="back-link">
                <i class="bi bi-arrow-left-circle"></i> Back
            </router-link>
        </div> -->

        <div class="row m-5">

            <div class="col-md-6 content1 ">
                <button class="btn back" onclick="javascript:window.history.back();">
                    <font-awesome-icon icon="fa-solid fa-chevron-left" />
                    go back
                </button>
                <div class="card card-reset">

                    <h1>Reset Password</h1>
                    <span class="label text-muted">
                        The verification email will be sent into mailbox<br>Please check it
                    </span>
                    <form action="" @submit.prevent="resetPass">
                        <div class="form-floating mb-3">
                            <input type="email" class="form-control" id="floatingInput" placeholder="name@example.com"
                                v-model="recruiterEmail"
                                v-bind:class="{ 'form-control': true, 'is-invalid': !validrecruiterEmail(recruiterEmail) && recruiterEmailBlured }"
                                v-on:blur="recruiterEmailBlured = true">
                            <div class="invalid-feedback">A valid email is required!</div>

                            <label for="floatingInput">Email Address</label>
                        </div>

                        <button v-if="searchDisabled == true" class="btn btn-primary btn-reset" type="button" disabled>
                            <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                            Loading...
                        </button>
                        <button v-else-if="!this.validrecruiterEmail(this.recruiterEmail)" type="submit"
                            class="btn btn-primary btn-reset w-100" disabled>Reset</button>
                        <button v-else type="submit" class="btn btn-primary btn-reset w-100"
                            :disabled="searchDisabled">Reset</button>

                    </form>
                </div>
            </div>
            <div class="col-md-6 content2">
                <img src="https://toptalentapp.com:9091/resources/kurtb4zq5unn0md.gif" alt="">
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: "ResetPass",
    data() {
        return {
            recruiterEmail: "",
            recruiterEmailBlured: false,
            searchDisabled: false
        }
    },
    methods: {
        validate() {
            this.recruiterEmailBlured = true;
            if (this.validrecruiterEmail(this.recruiterEmail)) {
                this.valid = true;
            }
        },
        validrecruiterEmail(recruiterEmail) {
            var re = /(.+)@(.+){2,}\.(.+){2,}/;
            if (re.test(recruiterEmail.toLowerCase())) {
                return true;
            }
        },
        async resetPass() {
            try {
                this.searchDisabled = true;
                await axios.post(
                    `https://toptalentapp.com:9091/api/v1/auth/reset?recruiterEmail=${this.recruiterEmail}`)
                // window.alert("E-mail has been sent");
                this.$toast.success('E-mail has been sent', {
                    position: 'top-right',
                    pauseOnHover: true
                });
                this.$router.push("VerificationPassword")
            } catch {
                this.searchDisabled = false;
                location.reload(true)
                // window.alert("failed")
                // this.$toast.error('failed', {
                //     position: 'top-right',
                //     pauseOnHover: true,
                //     duration: 10000
                // });

                this.$toast.error('Email Not Found', {
                    // optional options Object
                    position: 'top-right',
                    pauseOnHover: true
                })
            }
        }
    },
    beforeCreate() {
        document.querySelector('body').setAttribute('style', 'background:rgb(242,241,242)');
    },
    // beforeDestroy(){
    //     document.querySelector('body').setAttribute('style', '')
    // }
}
</script>

<style scoped>
* {
    font-family: 'Montserrat', sans-serif;
    font-weight: 500;
    margin: 0;
    padding: 0;
}

h1 {
    font-family: 'Poppins', sans-serif;
    font-size: 36px;
}

.card {
    padding: 20px;
    border-radius: 20px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}

.content2 {
    text-align: right;
}

.form-floating {
    margin-top: 50px;
}

img {
    width: 80%;
}

.btn-reset {
    padding: 10px;
}

.back {
    margin: 10px;
    padding: 5px;
    border-radius: 15px;
}

.back:hover {
    background: rgba(227, 44, 44, 0.692);
    border-radius: 15px;
}

/* breakpoints */
/* mobile */
@media only screen and (max-width: 576px) {
    .content2 {
        margin-top: 30px;
    }

    .content2 {
        text-align: center;
    }

    img {
        width: 70%;
    }

    .card-reset {
        width: 80vw;
    }

}
</style>