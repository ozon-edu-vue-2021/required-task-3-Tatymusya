<template>
    <div class="person">
        <div class="person__photo">
            <img
                :src="person.picture"
                alt="photo"
            />
        </div>
        <div class="person__info">
            <div class="person__info-name">
                <strong>{{ person.name }} ({{ person.age }})</strong>
            </div>

            <div class="person__info-email">
                <strong>Почта</strong>: 
                {{ person.email }}
            </div>
            <div class="person__info-email">
                <strong>Дата регистрации</strong>: 
                {{ formatedDate }}
            </div>
            <div class="person__info-about">
                <strong>О себе</strong>: 
                {{ person.about }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        person: {
            type: Object,
            default: null,
        },
    },
    computed: {
        formatedDate() {
            return this.person.registered;
        },
    },
};
</script>

<style scoped>
.person {
    display: grid;
    grid-template-columns: 50px 1fr;
    grid-gap: 8px;
}

.person__photo img {
    height: 50px;
    width: 50px;
    border-radius: 50%;
}

.person__info {
    display: grid;
    grid-gap: 8px;
}

.person__info-name {
    margin-bottom: 10px;
}
</style>
