<template>
    <form>

        <label for="name">Full Name: </label>
        <input type="text" name="name" id="name" required v-model="name" ref="nameField" />

        <label for="email">Enter your email: </label>
        <input type="email" name="email" id="email" required v-model="email" ref="emailField" />

        <label for="number">Enter your number: </label>
        <input
            type="text"
            name="number"
            id="number"
            required
            v-model="number"
            placeholder=""
            @input="acceptNumber"
        />

        <label for="role">Role: </label>
        <select name="role" id="role" v-model="role">
            <option value="frontend">Frontend Developer</option>
            <option value="backend">Backend Developer</option>
            <option value="fullstack">Full stack Developer</option>
        </select>

        <label for="skills">Skills: </label>
        <input type="skills" name="" id="skills" required v-model="skills" ref="skillsField" />

        <div class="apply">
            <button>Apply</button>
        </div>

    </form>
</template>

<script>
export default {
    data() {
        return {
            name: '',
            email: '',
            number: '',
            role: 'frontend',
            skills: '',
        };
    },
    mounted() {
        this.focusInput();
    },
    methods: {
        focusInput() {
            this.$refs.nameField.focus();
        },
       
        acceptNumber() {
            let cleaned = this.number.replace(/\D/g, '').match(/(\d{0,3})(\d{0,3})(\d{0,4})/);
            this.number = !cleaned[2]
                ? cleaned[1]
                : '(' + cleaned[1] + ') ' + cleaned[2] + (cleaned[3] ? '-' + cleaned[3] : '');
        },
    },
};
</script>

<style>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
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
    cursor: pointer;
}
input:hover {
    outline: 1px solid #1bad0869;
    border-radius: 4px;
}
input:focus {
    outline: 1px solid #1bad08;
    border-radius: 4px;
}
input[type='checkbox'] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}

button {
    background: #1bad08;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
    cursor: pointer;
    transition: 0.5s;
}
button:hover {
    transform: scale(1.1);
}
button:active {
    transform: scale(0.9);
}
.apply {
    text-align: center;
}

</style>
