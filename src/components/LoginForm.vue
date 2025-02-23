<template>
    <v-container style="max-width: 1280px;">
        <v-sheet class="mx-auto" width="600">
            <div class="text-h2 mb-4">Login Form</div>
            <v-form @submit.prevent="onSubmit">
                <v-text-field v-model="account" label="Account"></v-text-field>
                <div>{{ errorMessages.account }}</div>

                <v-text-field v-model="password" label="Password"></v-text-field>
                <div>{{ errorMessages.password }}</div>

                <v-text-field v-model="confirmPassword" label="Confirm Password"></v-text-field>
                <div>{{ errorMessages.confirmPassword }}</div>

                <v-text-field v-model="email" label="Email"></v-text-field>
                <div>{{ errorMessages.email }}</div>

                <v-container fluid>
                    <v-checkbox v-model="rememberAccount" color="success" label="記住密碼"></v-checkbox>
                </v-container>

                <div class="d-flex flex-column">
                    <v-btn class="mt-4" color="success" block type="submit">
                        Validate
                    </v-btn>
                </div>
            </v-form>
        </v-sheet>
    </v-container>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useForm } from 'vee-validate';
import * as yup from 'yup';

const validationSchema = yup.object({
    account: yup.string().required('帳號必填'),
    password: yup.string().required('密碼必填'),
    confirmPassword: yup.string().required('請確認密碼'),
    email: yup.string().email('請輸入有效的 Email').required('Email 必填'),
});


const { handleSubmit, defineField, errors } = useForm({ validationSchema });

const [account] = defineField('account');
const [password] = defineField('password');
const [confirmPassword] = defineField('confirmPassword');
const [email] = defineField('email');

const errorMessages = computed(() => errors.value);

const rememberAccount = ref(false);

const onSubmit = handleSubmit((values, { resetForm }) => {
    console.log('success submit', values);

    if (values.password !== values.confirmPassword) {
        alert('密碼與確認密碼不符');
        return;
    }

    resetForm();
});
</script>
