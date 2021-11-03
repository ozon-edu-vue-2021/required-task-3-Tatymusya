<template>
    <form 
        class="form" 
        method="POST"
    >
        <fieldset class="form-wrapper">
            <legend class="form-title">Посадить работника за свободный стол</legend>
            <input 
                class="form-field"
                name="user" 
                placeholder="name"
                v-model="user.name"
            >
            <input 
                class="form-field"
                name="user" 
                placeholder="email"
                v-model="user.email"
            >
            <input 
                class="form-field"
                name="user" 
                placeholder="age"
                v-model="user.age"
            >
            <select
                class="form-select" 
                v-model="userGroup"
            >
                <option 
                    v-for="group in groups"
                    :key="group.group_id"
                    :value="group.group_id"
                >
                    {{ group.text }}
                </option>
            </select>
            <button 
                class="form-button"
                type="button"
                @click="submit"
            >
                Добавить
            </button>
        </fieldset>
        <small>* Есть всего два свободных стола</small>
    </form>
</template>

<script>
export default {
    name: 'Form',
    props: {
        groups: {
            type: Array,
            default: () => ([])
        }
    },
    data() {
        return {
            user: {
                'name': '',
                'email': '',
                'picture': 'http://old.301-1.ru/gen-mems/img_mems/67ce7786cf69dd1fa3a9267d379b4ac4.jpg',
                'about': 'Laboris pariatur culpa id nostrud Lorem ad enim quis proident non. Nulla qui esse duis laboris incididunt aute. Enim cupidatat aliqua ipsum incididunt minim reprehenderit. Nisi quis reprehenderit fugiat irure mollit. Laborum nulla dolore cillum laborum ex excepteur consectetur anim esse commodo anim labore nostrud id.\r\n',
                'age': 0,
                'registered': '2021-06-28T07:20:14'
            },
            userGroup: '',
        }
    },
    methods: {
        submit() {
            this.$emit('submit', {user: this.user, userGroup: this.userGroup});
        }
    }
};
</script>

<style scoped>
    .form {
        max-width: 1500px;
        margin: 50px auto;
    }
    .form-wrapper {
        display: flex;
        gap: 12px;
        padding: 24px;
        border: 1px solid #ccd8e4;
        border-radius: 6px;
    }
    .form-title {
        color: #2c3e50;
    }
    .form-field,
    .form-select {
        border: 1px solid #ccd8e4;
    }
    .form-button {
        padding: 0 12px;
        font-size: 13px;
        background: rgb(138, 214, 138);
        border: none;
    }
</style>