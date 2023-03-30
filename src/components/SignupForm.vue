<template>
    <form @submit.prevent="handleSubmit" v-if="showForm">
        <h1>Register</h1>
        <label>Email:</label>
        <input type="email" required v-model="email">

        <label>Password:</label>
        <input type="password" required v-model="password">
        <div class="error" v-if="passwordError">{{ passwordError }}</div>

        <label>Role:</label>
        <select v-model="role">
            <option value="developer">Develop</option>
            <option value="designer">Design</option>
            <option value="tester">QA</option>
        </select>

        <label>Skills:</label>
        <input type="text" v-model="tempSkill" @keyup="addSkill">
        <div v-for="skill in skills" :key="skill" class="pill">
            <span @click="deleteSkill(skill)">{{ skill }}</span>
        </div>

        <div class="terms">
            <input type="checkbox" v-model="terms" id="terms">
            <label for="terms">Accept terms and Conditions</label>
            <div class="error" v-if="termsError">{{ termsError }}</div>
        </div>

        <div class="submit">
            <button type="submit">Create an account</button>
        </div>

    </form>
    <div class="formdone" v-if="!showForm">
        <div><h1>Form Submitted!</h1></div>
        <div class="data">Email: {{ email }}</div>
        <div class="data">Password: {{ password }}</div>
        <div class="data">Role: {{ role }}</div>
        <div class="data">Skills:
            <div v-for="skill in skills" :key="skill" class="pill done">
                <span>{{ skill }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            email: '',
            password: '',
            role: 'tester',
            terms: false,
            tempSkill: '',
            skills: [],
            passwordError: '',
            termsError: '',
            showForm: true
        }
    },
    methods: {
        addSkill(e) {
            if (e.key === ',' && this.tempSkill != ',' && this.tempSkill) {
                this.tempSkill = this.tempSkill.slice(0, -1)
                if (!this.skills.includes(this.tempSkill)) {
                    this.skills.push(this.tempSkill)
                }
                this.tempSkill = ''
            }
        },
        deleteSkill(skill) {
            this.skills = this.skills.filter((item) => {
                return skill !== item
            })
        },
        handleSubmit() {
            //validate password
            this.passwordError = this.password.length > 11 ? '' : '*Password must be at least 12 characters long'
            this.termsError = this.terms ? '' : '*Terms and conditions must be accepted'
            if (!this.passwordError && !this.termsError) {
                this.showForm = false
            }
        }
    }
}
</script>

<style>
form,
.formdone {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}

.formdone .data {
    margin: 0 0 10px 0;
}

label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}

input,
select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
    transition: all 0.3s;
    outline: none;
}

input:focus,
select:focus {
    border-bottom: 1px solid black;
}

input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}

.pill {
    display: inline-block;
    margin: 0 5px 5px 5px;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}

.done.pill {
    cursor: default;
}

.submit button {
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}

.submit {
    text-align: center;
}

.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
</style>