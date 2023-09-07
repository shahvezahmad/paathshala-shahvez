<template>
    <form>

        <label for="name">Full Name: </label>
        <input type="text" name="name" id="name" required v-model="name" ref="nameField" />

        <label for="email">Enter your email: </label>
        <input type="email" name="email" id="email" required v-model="email" ref="emailField" />

        <label for="tel">Enter your number: </label>
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

        <div class="terms">
            <label>
                <input type="checkbox" name="terms" id="terms" required v-model="terms" />
                Accept terms and conditions</label
            >
        </div>

        <div class="submit">
            <button>Submit</button>
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
            terms: false,
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
            this.tel = !cleaned[2]
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
.terms label {
    cursor: pointer;
}
.terms label:hover {
    color: #188809;
}
.terms input:hover {
    outline: none;
}
.terms input:focus {
    outline: none;
}
.pill {
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}
.pill:hover {
    background-color: #01a71db7;
    color: #000000;
}

.pill:active {
    background-color: #9b0404e1;
    text-decoration: line-through;
    color: #ffffff;
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
.submit {
    text-align: center;
}
.error {
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
.visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    border: 0;
    padding: 0;
    white-space: nowrap;
    clip-path: inset(100%);
    clip: rect(0 0 0 0);
    overflow: hidden;
}
</style>
