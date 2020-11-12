<template>
    <div class="container">
        <form @submit.prevent="register">
            <div v-show="step === 1" class="step custom-container col-sm-5 mx-auto">
                <div class="form-group">
                    <label for="name">Your name</label>
                    <input @blur="$v.formReg.name.$touch()" :class="{'is-invalid': $v.formReg.name.$error}" v-model="formReg.name" type="text" class="form-control" id="name">
                    <div v-if="!$v.formReg.name.required" class="invalid-feedback">
                        Please enter your name
                    </div>
                    <div v-if="!$v.formReg.name.alpha" class="invalid-feedback">
                        This field should only contain letters
                    </div>
                </div>
                <div class="form-group">
                    <label for="nickname">Your nickname</label>
                    <input @blur="$v.formReg.nickname.$touch()" :class="{'is-invalid': $v.formReg.nickname.$error}" v-model="formReg.nickname" type="text" class="form-control" id="nickname">
                    <div v-if="!$v.formReg.nickname.required" class="invalid-feedback">
                        Please enter your nickname
                    </div>
                </div>
                <div class="form-group">
                    <label for="Email">Email</label>
                    <input @blur="$v.formReg.Email.$touch()" :class="{'is-invalid': $v.formReg.Email.$error}" v-model="formReg.Email" type="text" class="form-control" id="Email">
                    <div v-if="!$v.formReg.Email.required" class="invalid-feedback">
                        Please enter your Email
                    </div>
                    <div v-if="!$v.formReg.Email.email" class="invalid-feedback">
                        This field should only contain an email adress
                    </div>
                </div>
                <div class="form-group">
                    <label for="date">Your date of birth</label>
                    <input @blur="$v.formReg.date.$touch()" :class="{'is-invalid': $v.formReg.date.$error}" v-model="formReg.date" type="date" class="form-control" id="date">
                    <div v-if="!$v.formReg.date.required" class="invalid-feedback">
                        Please enter your date of birth
                    </div>
                </div>
                <div class="form-group">
                    <label for="gender">Your gender</label>
                    <input v-model="formReg.gender" type="text" class="form-control" id="gender">
                </div>
                <div class="form-group">
                    <label for="lang">Favorite programming language</label>
                    <select v-model="formReg.lang" class="form-control custom-select" id="lang">
                        <option value="C++">C</option>
                        <option value="C++">C++</option>
                        <option value="C#">C#</option>
                        <option value="Java">Java</option>
                        <option value="JavaScript">JavaScript</option>
                        <option value="Python">Python</option>
                        <option value="PHP">PHP</option>
                        <option value="HTML">HTML</option>
                    </select>
                </div>
                <button @click="nextStep" :disabled="disabledBtn1" type="button" class="btn btn-light">Continue</button>
            </div>
            <transition name="slide-fade">
                <div v-show="step === 2" class="step custom-container col-sm-5 mx-auto">
                    <div class="form-group">
                        <label for="Pass">Password</label>
                        <input @blur="$v.formReg.Pass.$touch()" :class="{'is-invalid': $v.formReg.Pass.$error}" v-model="formReg.Pass" type="password" class="form-control" id="Pass">
                        <div v-if="!$v.formReg.Pass.required" class="invalid-feedback">
                            Please enter password
                        </div>
                        <div v-if="!$v.formReg.Pass.minLength" class="invalid-feedback">
                            Password must be at least 6 characters long
                        </div>
                    </div>
                    <div class="form-group">
                        <label for="PassConf">Confirm password</label>
                        <input @blur="$v.formReg.PassConf.$touch()" :class="{'is-invalid': $v.formReg.PassConf.$error}" v-model="formReg.PassConf" type="password" class="form-control" id="PassConf">
                        <div v-if="!$v.formReg.PassConf.required" class="invalid-feedback">
                            You should confirm your password
                        </div>
                        <div v-if="!$v.formReg.PassConf.sameAs" class="invalid-feedback">
                            Passwords sould match
                        </div>
                    </div>
                    <button @click="backStep" type="button" class="btn btn-light mr-2">Back</button>
                    <button @click="nextStep" :disabled="disabledBtn2" type="button" class="btn btn-light">Continue</button>
                </div>
            </transition>
            <transition name="slide-fade">
                <div v-show="step === 3" class="step custom-container col-sm-5 mx-auto">
                    <div class="form-group">
                        <label for="descr">Decribe yourself</label>
                        <textarea @blur="$v.formReg.descr.$touch()" :class="{'is-invalid': $v.formReg.descr.$error}" v-model="formReg.descr" class="form-control" id="descr" rows="5"></textarea>
                        <div v-if="!$v.formReg.descr.required" class="invalid-feedback">
                            We really need your description
                        </div>
                    </div>
                    <button @click="backStep" type="button" class="btn btn-light mr-2">Back</button>
                    <button @click="nextStep" :disabled="disabledBtn3" type="submit" class="btn btn-light">Submit</button>
                </div>
            </transition>
            <transition name="slide-fade">
                <div v-show="step === 4" class="step custom-container col-sm-6 mx-auto">
                    <h1>REGISTRATION COMPLETED</h1>
                </div>
            </transition>
        </form>
    </div>
</template>

<script>
    import { required, email, helpers, minLength, sameAs } from 'vuelidate/lib/validators'
    const alpha = helpers.regex('alpha', /^[a-zA-Z]*$/)
    export default {
        data() {
            return {
                step: 1,
                formReg: {
                    name: '',
                    nickname: '',
                    date: '',
                    Email: '',
                    gender: '',
                    lang: '',
                    Pass: '',
                    PassConf: '',
                    descr: '',
                }
            }
        },
        computed: {
            disabledBtn1() {
                return this.$v.formReg.name.$invalid || this.$v.formReg.nickname.$invalid || this.$v.formReg.Email.$invalid || this.$v.formReg.date.$invalid
            },
            disabledBtn2() {
                return this.$v.formReg.Pass.$invalid || this.$v.formReg.PassConf.$invalid
            },
            disabledBtn3() {
                return this.$v.formReg.descr.$invalid
            }
        },
        methods: {
            nextStep() {
                if (this.step < 4) {
                    this.step++
                }
            },
            backStep() {
                if (this.step > 1) {
                    this.step--
                }
            },
            register() {
                console.log('Success')
                console.log(this.formReg.name)
                console.log(this.formReg.nickname)
                console.log(this.formReg.date)
                console.log(this.formReg.Email)
                console.log(this.formReg.gender)
                console.log(this.formReg.lang)
                console.log(this.formReg.Pass)
                console.log(this.formReg.PassConf)
                console.log(this.formReg.Descr)
            }
        },
        validations: {
            formReg: {
                name: {
                    required,
                    alpha
                },
                nickname: {
                    required
                },
                date: {
                    required
                },
                Email: {
                    required,
                    email
                },
                gender: {},
                lang: {},
                Pass: {
                    required,
                    minLength: minLength(6)
               
                },
                PassConf: {
                    required,
                    sameAs: sameAs('Pass')
                },
                descr: {
                    required
                },
            }
        }
    }
</script>